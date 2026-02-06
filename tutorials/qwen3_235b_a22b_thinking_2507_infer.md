# Qwen3-235B-A22B-Thinking-2507 Inference Guide

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器

```bash
#!/bin/bash

readonly CONTAINER_NAME="xsgl_container"

docker_image=<XSGL_IMAGE>

XPU_NUM=8
DOCKER_DEVICE_CONFIG=" "
if [ $XPU_NUM -gt 0 ]; then
for ((idx=0; idx<=$XPU_NUM-1; idx++)); do
        DOCKER_DEVICE_CONFIG+=" --device=/dev/xpu${idx}:/dev/xpu${idx} "
done
DOCKER_DEVICE_CONFIG+=" --device=/dev/xpuctrl:/dev/xpuctrl "
fi

#请一定要保证： 不要覆盖原workspace的内容，所有启动脚本均在/workspace内。
docker run -dit ${DOCKER_DEVICE_CONFIG}                 \
        --net=host                                     \
        --cap-add=SYS_PTRACE --security-opt seccomp=unconfined \
        --tmpfs /dev/shm:rw,nosuid,nodev,exec,size=32g \
        --cap-add=SYS_PTRACE                           \
        -v ${PWD}:/workspace/users           \
        -v ~/.ssh:/root/.ssh            \
        --name ${CONTAINER_NAME}        \
        --ulimit=memlock=-1 \
        --ulimit=nofile=120000 \
        --ulimit=stack=67108864 \
        -w /workspace                   \
        -v /ssd1:/ssd1 \
        -v /ssd2:/ssd2 \
        -v /ssd3:/ssd3 \
        -v /ssd4:/ssd4 \
        --cpuset-cpus=0-$((`nproc`-8)) \
        --device=/dev/xpu0:/dev/xpu0 \
        --device=/dev/xpu1:/dev/xpu1 \
        --device=/dev/xpu2:/dev/xpu2 \
        --device=/dev/xpu3:/dev/xpu3 \
        --device=/dev/xpu4:/dev/xpu4 \
        --device=/dev/xpu5:/dev/xpu5 \
        --device=/dev/xpu6:/dev/xpu6 \
        --device=/dev/xpu7:/dev/xpu7 \
        --shm-size=512g  \
        --device /dev/fuse \
        --privileged                    \
        ${docker_image} /bin/bash


docker exec -it ${CONTAINER_NAME} /bin/bash
```

## 准备数据集及模型

### 配置容器内环境

```bash
pip install huggingface_hub
cp -r /workspace/qwen3-235b-a22b-instruct-2507 /workspace/qwen3-235b-a22b-thinking-2507
```

然后请将`/workspace/qwen3-235b-a22b-thinking-2507/run_server.sh`修改为下面的内容：

```bash
#!/bin/bash
MODEL_DIR=${model_dir:-/ssd3/models/Qwen3-235B-A22B-Thinking-2507/}

# autotune file
export XBLAS_FC_AUTOTUNE_FILE=./fc_fusion_autotune_20260107_214038
export XBLAS_MOE_FC_AUTOTUNE_FILE=./moe_fc_autotune_20260107_172556

export XMLIR_D_XPU_L3_SIZE=0
export CUDA_DEVICE_ORDER="OAM_ID"
export BKCL_INFERENCE=1 #开启推理优化
export BKCL_TREE_THRESHOLD=1048576
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True
export XSGL_INTERTYPE_BFP16=1
export MIN_BATCH=4096
export LD_LIBRARY_PATH=${CONDA_PREFIX}/lib/python3.10/site-packages/xtorch_ops:${CONDA_PREFIX}/lib/python3.10/site-packages/nvidia/cuda_nvrtc/lib:${CONDA_PREFIX}/lib/python3.10/site-packages/torch_xmlir/:/usr/local/cuda-11.7/:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/lib/:$LD_LIBRARY_PATH

# mtp新增
export SGLANG_ALLOW_OVERWRITE_LONGER_CONTEXT_LEN=1

export XPU_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7

# FC inter 类型
export XSGL_ENABLE_TGEMM_FP16=1
export XSGL_FUSE_SPLIT_NORM_ROPE_NEOX=1

# 0.5.1临时新增：
export SGLANG_IS_FLASHINFER_AVAILABLE=false

# XDNN kernel:
export XSGL_USE_DEEP_GEMM_BMM=1
export XSGL_FAST_SWIGLU=1
export USE_MOE_FC_V3=1
export USE_KLX_API_ALLOC_EXTEND=1

nohup python -m sglang.launch_server \
    --model-path "$MODEL_DIR" \
    --port 8026 \
    --host 0.0.0.0 \
    --trust-remote-code \
    --attention-backend kunlun \
    --chunked-prefill-size -1 \
    --page-size 64 \
    --kv-cache-dtype float16 \
    --tp-size 8 \
    --dtype float16 \
    --disable-custom-all-reduce \
    --disable-overlap-schedule \
    --disable-radix-cache \
    --mem-fraction-static 0.9 \
    --max-prefill-tokens 32768 \
    --max-running-requests 256 \
    --cuda-graph-max-bs 256 \
    --cuda-graph-bs 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83 84 85 86 87 88 89 90 91 92 93 94 95 96 97 98 99 100 101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199 200 201 202 203 204 205 206 207 208 209 210 211 212 213 214 215 216 217 218 219 220 221 222 223 224 225 226 227 228 229 230 231 232 233 234 235 236 237 238 239 240 241 242 243 244 245 246 247 248 249 250 251 252 253 254 255 256 \
    --tool-call-parser qwen25 \
    --watchdog-timeout 3000000 >log_$(date +"%Y%m%d").log 2>&1 &
```

### 准备数据集

以ShareGPT为例：

```bash
huggingface-cli download learnanything/sharegpt_v3_unfiltered_cleaned_split --repo-type dataset --local-dir /workspace/users/sharegpt_data
```

### 下载预训练权重

```bash
huggingface-cli download Qwen/Qwen3-235B-A22B-Thinking-2507 --local-dir /workspace/users/qwen3-235b-a22b-thinking-2507
```

## 运行推理

启动sglang服务：
```bash
model_dir=/workspace/users/qwen3-235b-a22b-thinking-2507 ./run_server.sh
```

推理请求示例：
```bash
curl http://localhost:8026/v1/chat/completions -H "Content-Type: application/json" -d '{
  "model": "/workspace/users/qwen3-235b-a22b-thinking-2507",
  "messages": [
    {"role": "user", "content": "你是谁"}
  ],
  "temperature": 0.6,
  "top_p": 0.95,
  "top_k": 20,
  "max_tokens": 32768
}'
```

使用数据集进行压测示例（以ShareGPT为例）：
```bash
python3 -m sglang.bench_serving \
  --backend sglang \
  --base-url http://localhost:8026 \
  --dataset-name random \
  --dataset-path /workspace/users/sharegpt_data/ShareGPT_V3_unfiltered_cleaned_split.json  \
  --random-input-len 256 \        # 设置输入长度
  --random-output-len 1024 \      # 设置输出长度
  --num-prompts 640 \             # 设置测试总请求数
  --max-concurrency 64 \          # 设置最大并发数
  --tokenizer /workspace/users/qwen3-235b-a22b-thinking-2507
```