# Cosmos3-Super i2v Inference Guide (P800)

> 本文档介绍 Cosmos3-Super 模型在昆仑芯 P800（XPU）上的 image-to-video（i2v）推理适配与执行流程，。

## 环境准备
### 准备开发环境镜像
请联系相关人员获取 P800 开发环境镜像。

## 代码及模型准备
### 代码准备
```bash
# 拉取昆仑芯适配版代码，并切到 kunlun 分支
git clone https://github.com/KunlunxinAD/cosmos-framework.git
cd cosmos-framework
git checkout kunlun
```

### 下载模型权重
```bash
# 主模型
modelscope download --model Qwen/Qwen3-VL-32B-Instruct --local_dir Qwen3-VL-32B-Instruct
hf download nvidia/Cosmos3-Nano  --local-dir Cosmos3-Nano
hf download nvidia/Cosmos3-Super --local-dir Cosmos3-Super

# Wan2.2 VAE
wget "https://huggingface.co/Wan-AI/Wan2.2-TI2V-5B/resolve/main/Wan2.2_VAE.pth"

# 如果需要使用 guardrails（内容安全）
hf download nvidia/Cosmos-Guardrail1 --local-dir Cosmos-Guardrail1
modelscope download --model Qwen/Qwen3Guard-Gen-0.6B --local_dir Qwen3Guard-Gen-0.6B
```

> 建议将全部权重提前下载到本地。8 卡进程会同时通过 `uvx hf download` 抢同一份 lock 文件，HF Hub 用 `filelock` 串行化下载，容易出现 1 个 rank 在拉、其余 7 个 rank 死等的情况。下载到本地后通过 `COSMOS_HF_LOCAL__*` 环境变量直接加载即可绕过（见下文执行脚本）。

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=cosmos3_super_i2v
export MODEL_PATH=</path/to/cosmos-framework>   # 本地代码/权重路径

docker run -itd --privileged --net=host \
    -w /workspace \
    -v ${MODEL_PATH}:/home \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${XAV_CONTAINER} \
    --shm-size 256g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${XAV_CONTAINER} bash
```

### 容器内环境配置
```bash
# 切换 conda env
conda activate python312_torch29_cuda
```

### 安装依赖
```bash
cd cosmos-framework

# 安装代码库本体
pip install -e . --no-deps
pip install -e packages/diffusers-cosmos3 --no-deps

# 安装 pyproject.toml 中声明的依赖
pip install accelerate
pip install av
pip install cattrs
pip install diffusers
pip install einops
pip install hydra-core
pip install imageio-ffmpeg
pip install imageio
pip install loguru
pip install msgpack
pip install nvidia-cudnn-frontend
pip install nvidia-ml-py
pip install obstore
pip install omegaconf
pip install pydantic
pip install requests
pip install scipy
pip install termcolor
pip install transformers==4.57.1
pip install tyro
pip install websockets
pip install uv

# 更新 flash_attn（使用昆仑芯 xFlashAttention py312/torch290 版本）
wget https://klx-sdk-release-public.su.bcebos.com/v1/XFlashAttention/release/1.0.5.1/xFlashAttention_py312_torch290.tar.gz
tar -zxvf xFlashAttention_py312_torch290.tar.gz
pip install flash_attn-2.8.0+6ef99a2-cp312-cp312-linux_x86_64.whl

# 安装 pyproject.toml 未提到但项目运行需要的依赖
pip install iopath
pip install multi-storage-client
pip install boto3
pip install pandas
pip install wandb
pip install qwen_vl_utils
pip install nltk
pip install better-profanity
pip install retinaface-py
```

## 执行 Inference
### 设置执行脚本
```bash
vim run_super_i2v.sh
```

### 脚本内容参考
```bash
#!/usr/bin/env bash
# Usage:
#   ./run_super_i2v.sh throughput
#   ./run_super_i2v.sh latency
set -euo pipefail

MODE="${1:-throughput}"

NPROC=8
INPUT="inputs/omni/i2v.json"          # 多文件可写: "inputs/customer_i2v/*.json"
OUTPUT="outputs/super_i2v"
CHECKPOINT="/path/of/Cosmos3-Super"
SEED=0
EXTRA_ARGS=(--no-use-torch-compile)
# ================================

export HF_ENDPOINT=https://hf-mirror.com
export HF_HUB_ENABLE_HF_TRANSFER=0

export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export XMLIR_BMM_DISPATCH_VALUE=2
export BKCL_PCIE_TOPO=1
export CUDART_DUMMY_REGISTER=1
unset CUDA_LAUNCH_BLOCKING
unset XPU_DUMMY_EVENT
export XMLIR_ENABLE_FAST_FC=1
export XDNN_USE_FAST_GELU=1
export XMLIR_ENABLE_LINEAR_FC_FUSION=0
export PYTORCH_ALLOC_CONF="expandable_segments:True"

# 权重本地加载（kunlun 分支已内置对应加载逻辑）
export QWEN3GUARD_DIR=/path/of/Qwen3Guard-Gen-0.6B
export COSMOS_HF_LOCAL__nvidia__Cosmos_Guardrail1=/path/of/Cosmos-Guardrail1
export COSMOS_HF_LOCAL__Wan_AI__Wan2_2_TI2V_5B=/path/of/Wan2.2-TI2V-5B
export COSMOS_HF_LOCAL__nvidia__Cosmos3_Nano=/path/of/Cosmos3-Nano
export COSMOS_HF_LOCAL__Qwen__Qwen3_VL_32B_Instruct=/path/of/Qwen3-VL-32B-Instruct

export I4_ATTN_BACKENDS=torch_sdpa


case "${MODE,,}" in
        throughput)
            PARALLEL_ARGS=(
                --parallelism-preset=throughput
                --dp-shard-size="${NPROC}"
                --dp-replicate-size=1
                --cp-size=1
                --cfgp-size=1
                )
                ;;
        latency)
            PARALLEL_ARGS=(
                --parallelism-preset=latency
                )
                ;;
        *)
            echo "Unknown MODE: '${MODE}'. Use 'throughput' or 'latency'." >&2
            exit 1
            ;;
esac

echo "[run_super_i2v] mode=${MODE} nproc=${NPROC} input=${INPUT}"

torchrun --nproc-per-node="${NPROC}" -m cosmos_framework.scripts.inference \
        "${PARALLEL_ARGS[@]}" \
        -i "${INPUT}" \
        -o "${OUTPUT}" \
        --checkpoint-path "${CHECKPOINT}" \
        --seed="${SEED}" \
        "${EXTRA_ARGS[@]}"
```



