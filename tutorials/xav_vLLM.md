# xav-vLLM

## 获取镜像
请联系相关人员获取开发环境镜像。

## 准备环境

请根据实际环境替换以下变量：

|变量|描述|
|---|---|
|<XAV_IMAGE>|开发环境镜像版本|
|<CONTAINER_NAME>|容器名|
|</path/to/workspace>|本地工作空间路径|
|</path/to/data>|本地数据集路径|
|<XPU_NUM>|本机可见卡数|

### 启动容器
```bash
export PATH_TO_MOUNT=</path/to/workspace> #本地路径
export PATH_AFTER_MOUNT=/workspace #挂载后在容器内的路径
export DATASET_PATH=</path/to/data>
export XPU_NUM 8

DOCKER_DEVICE_CONFIG=""
if [ $XPU_NUM -gt 0 ]; then
    for idx in $(seq 0 $((XPU_NUM-1))); do
        DOCKER_DEVICE_CONFIG="${DOCKER_DEVICE_CONFIG} --device=/dev/xpu${idx}:/dev/xpu${idx}"
    done
    DOCKER_DEVICE_CONFIG="${DOCKER_DEVICE_CONFIG} --device=/dev/xpuctrl:/dev/xpuctrl"
fi

CONTAINER_NAME=<CONTAINER_NAME>
XAV_IMAGE=<XAV_IMAGE>
echo "CONTAINER_NAME is: ${CONTAINER_NAME}"
echo "XAV_IMAGE is: ${XAV_IMAGE}"

docker run -dti ${DOCKER_DEVICE_CONFIG} \
        --name ${CONTAINER_NAME} \
        --net=host \
        --security-opt=seccomp=unconfined \
        -e IMAGE_VERSION="${XAV_IMAGE}" \
        --cap-add=SYS_PTRACE \
        --shm-size=256g \
        --cap-add=SYS_ADMIN \
        --device /dev/fuse \
        --restart=always \
        --ulimit=memlock=-1 \
        --ulimit=nofile=120000 \
        --ulimit=stack=67108864 \
        -v ${PATH_TO_MOUNT}:${PATH_AFTER_MOUNT} \
        -v ${DATASET_PATH}:/data \
        --cpuset-cpus=0-$((`nproc`-8)) \
        --privileged ${XAV_IMAGE} \
        /bin/bash

docker start ${CONTAINER_NAME}
docker exec -it ${CONTAINER_NAME} /bin/bash
```

### 配置容器内环境
```bash
# 激活虚环境
conda activate python310_torch25_cuda

# 安装XPytorch/XMLIR
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/v1.5.0/xpytorch_c54203a3/xpytorch-cp310-torch251-ubuntu2004-x64.run
bash xpytorch-cp310-torch251-ubuntu2004-x64.run
unset LD_LIBRARY_PATH

# 安装指定版本的官方vLLM
pip install vllm==0.11.0 --no-build-isolation --no-deps

# 安装vllm-plugin
git clone https://github.com/KunlunxinAD/xav-vLLM.git
cd xav-vLLM
pip install -r requirements.txt
pip install fastapi==0.112.1
pip install uvicorn==0.32.1
pip install python-multipart==0.0.19

python setup.py build
python setup.py install
cp vllm_kunlun/patches/eval_frame.py /root/miniconda/envs/python310_torch25_cuda/lib/python3.10/site-packages/torch/_dynamo/eval_frame.py

# 安装算子库
pip install "https://klx-sdk-release-public.su.bcebos.com/xav/xav_vllm/v0.11.0/20260123/xtorch_ops-0.1.2423%2Bc3c56b47-cp310-cp310-linux_x86_64.whl?authorization=bce-auth-v1%2FALTAKtEKzwsuB12vSFtik94FkX%2F2026-01-27T08%3A34%3A34Z%2F2592000%2Fhost%2Fc444a6053d1bb4b5823b2be12438f38c7b85c4748e278cace3d7982222550a77"

pip install "https://cce-ai-models.bj.bcebos.com/v1/vllm-kunlun-0.11.0/triton-3.0.0%2Bb2cde523-cp310-cp310-linux_x86_64.whl"

pip install "https://cce-ai-models.bj.bcebos.com/XSpeedGate-whl/release_merge/20251219_152418/xspeedgate_ops-0.0.0-cp310-cp310-linux_x86_64.whl"

# 使能环境变量
chmod +x /workspace/xav-vLLM/setup_env.sh && source /workspace/xav-vLLM/setup_env.sh
```

### 下载模型权重
```bash
cd /workspace
mkdir models & cd models
# 以qwen3-vl-8b为例
hf download Qwen/Qwen3-VL-8B-Instruct --local-dir ./Qwen3-VL-8B-Instruct
```

## 在线推理
### 启动服务
```bash
python -m vllm.entrypoints.openai.api_server \
      --host 0.0.0.0 \
      --port 8356 \
      --model <model_path> \
      --gpu-memory-utilization 0.9 \
      --trust-remote-code \
      --max-model-len 32768 \
      --tensor-parallel-size 1 \
      --dtype float16 \
      --max_num_seqs 128 \
      --max_num_batched_tokens 32768 \
      --block-size 128 \
      --no-enable-prefix-caching \
      --no-enable-chunked-prefill \
      --distributed-executor-backend mp \
      --served-model-name <model_name> \
      --compilation-config '{"splitting_ops": ["vllm.unified_attention", 
                                                "vllm.unified_attention_with_output",
                                                "vllm.unified_attention_with_output_kunlun",
                                                "vllm.mamba_mixer2", 
                                                "vllm.mamba_mixer", 
                                                "vllm.short_conv", 
                                                "vllm.linear_attention", 
                                                "vllm.plamo2_mamba_mixer", 
                                                "vllm.gdn_attention", 
                                                "vllm.sparse_attn_indexer"]}'
```

### 单例测试
```bash
curl http://localhost:8356/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d @- <<'EOF'
{
  "model": "<model_name>",
  "messages": [
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "请分别详细描述这几张图片的内容。"
        },
        {
          "type": "image_url",
          "image_url": {
            "url": "https://i-blog.csdnimg.cn/direct/f17798fe5a6b4167aa6227fe2eaac2ea.pngg"
          }
        },
        {
          "type": "image_url",
          "image_url": {
            "url": "https://sail-moe.oss-cn-hangzhou.aliyuncs.com/yunlin/images/evalscope/doc/qwen_vl/perf.png"
          }
        }
      ]
    }
  ]
}
EOF
```

## 离线推理
```bash
import os
from vllm import LLM, SamplingParams

def main():

    model_path = "/workspace/models/Qwen3-VL-8B-Instruct"

    llm = LLM(
        model=model_path,
        tokenizer=model_path,
        tensor_parallel_size=1,
        trust_remote_code=True,
        dtype="float16",
        distributed_executor_backend="mp",
        max_model_len=32768,
        gpu_memory_utilization=0.9,
        block_size=128,
        max_num_seqs=128,
        max_num_batched_tokens=32768,
        enable_prefix_caching=False,
        enable_chunked_prefill=False,
        served_model_name="Qwen3-VL",
        compilation_config={
            "splitting_ops": [
                "vllm.unified_attention",
                "vllm.unified_attention_with_output",
                "vllm.unified_attention_with_output_kunlun",
                "vllm.mamba_mixer2",
                "vllm.mamba_mixer",
                "vllm.short_conv",
                "vllm.linear_attention",
                "vllm.plamo2_mamba_mixer",
                "vllm.gdn_attention",
                "vllm.sparse_attn_indexer",
            ]
        },
    )

    # === test chat ===
    messages = [
        {
            "role": "user",
            "content": [{"type": "text", "text": "Hello, what can you do?"}]
        }
    ]

    sampling = SamplingParams(
        max_tokens=200,
        temperature=0.8,
        top_k=50,
        top_p=1.0,
    )

    print("开始推理...")
    outputs = llm.chat(messages, sampling_params=sampling)

    print("模型输出：\n")
    print(outputs[0].outputs[0].text)


if __name__ == "__main__":
    main()
```

## Benchmark
可以直接使用vLLM的benchmark. 具体参考[vLLM Developer Guide Benchmark Suites](https://docs.vllm.ai/en/stable/contributing/benchmarks.html)

### 1.Online Benchmark

#### 1.1启动 the vLLM server

服务启动脚本参考

```bash
python -m vllm.entrypoints.openai.api_server \
      --host 0.0.0.0 \
      --port 8000 \
      --model /xxxx/xxxx/mkdel\
      --gpu-memory-utilization 0.9 \
      --trust-remote-code \
      --max-model-len 32768 \
      --tensor-parallel-size 1 \
      --dtype float16 \
      --no-enable-prefix-caching \
      --no-enable-chunked-prefill \
      --distributed-executor-backend mp \
      --served-model-name modelname \
      --compilation-config '{"splitting_ops": ["vllm.unified_attention", 
                                                "vllm.unified_attention_with_output",
                                                "vllm.unified_attention_with_output_kunlun",
                                                "vllm.mamba_mixer2", 
                                                "vllm.mamba_mixer", 
                                                "vllm.short_conv", 
                                                "vllm.linear_attention", 
                                                "vllm.plamo2_mamba_mixer", 
                                                "vllm.gdn_attention", 
                                                "vllm.sparse_attn_indexer"]}' \

```

#### 1.2执行测试

运行测试脚本，参考如下：

```bash
#!/bin/bash
# Run benchmark tests
python -m vllm.entrypoints.cli.main bench serve \
    --host 127.0.0.1 \
    --port xxxx \
    --backend vllm \
    --model modelname \
    --dataset-name random \
    --num-prompts 500 \
    --random-input-len 1024 \
    --random-output-len 1024 \
    --tokenizer /xxxx/xxxx/model \
    --ignore-eos 2>&1 | tee benchmark.log
```

#### 1.3结果

最终结果如下所示

```bash
========== Serving Benchmark Result ==========
Successful requests:                          500
Benchmark duration (s):                       144.89
Total input tokens:                           510414
Total generated tokens:                       512000
Request throughput (req/s):                   3.45
Output token throughput (tok/s):              3533.68
Total Token throughput (tok/s):               7056.42
----------Time to First Token----------
Mean TTFT (ms):                               57959.61
Median TTFT (ms):                             43551.93
P99 TTFT (ms):                                116202.52
----------Time per Output Token (excl. 1st token)----------
Mean TPOT (ms):                               33.30
Median TPOT (ms):                             34.15
P99 TPOT (ms):                                35.59
----------Inter-token Latency----------
Mean ITL (ms):                                33.30
Median ITL (ms):                              29.05
P99 ITL (ms):                                 46.14
============================================
```

关键参数解释:

| index                        | meaning                 | Optimization Objective   |
| --------------------------- | ------------------------------------| ---------- |
| ***\*Output Throughput\**** | Output token generation rate                   | ↑ The higher the better |
| ***\*Mean TTFT\****         | First Token Delay (Time To First Token)         | ↓ The lower the better |
| ***\*P99 TTFT\****          | 99% of requests have delayed first token.       | ↓ The lower the better |
| ***\*Mean TPOT\****         | Average generation time per output token | ↓ The lower the better |
| ***\*P99 TPOT\****          | 99% of requests' time per token generation    | ↓ The lower the better |
| ***\*ITL\****               | Delay between adjacent output tokens            | ↓ The lower the better |
