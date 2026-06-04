# Qwen3-Omni-30B-A3B Inference Guide

## 环境准备
请联系相关人员获取开发环境镜像

### 启动容器

```bash
#!/bin/bash
# 修改docker name
readonly CONTAINER_NAME=<CONTAINER_NAMe>
readonly DOCKER_IMAGE=<XAV_IMAGE>
readonly Workspace=<YOUR_PATH>

XPU_NUM=8
DOCKER_DEVICE_CONFIG=" "
if [ $XPU_NUM -gt 0 ]; then
for ((idx=0; idx<=$XPU_NUM-1; idx++)); do
        DOCKER_DEVICE_CONFIG+=" --device=/dev/xpu${idx}:/dev/xpu${idx} "
done
DOCKER_DEVICE_CONFIG+=" --device=/dev/xpuctrl:/dev/xpuctrl "
fi

# 可以继续添加-v 挂载其他磁盘, 比如内部用户可以添加 -v /klxlake:/klxlake
# 启动docker时候，注意要新增‘--privileged’确保容器内可以看到设备节点
docker run -it ${DOCKER_DEVICE_CONFIG}                         \
        --privileged                                           \
        --net=host                                             \
        --cap-add=SYS_PTRACE --security-opt seccomp=unconfined \
        --tmpfs /dev/shm:rw,nosuid,nodev,exec,size=32g         \
        --cap-add=SYS_PTRACE                                   \
        -v ${Workspace}:/workspace                             \
        --name ${CONTAINER_NAME}                               \
        -w /workspace                                          \
        ${DOCKER_IMAGE} /bin/bash
```

### 安装xvllm

```bash
cd /workspace
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_020/xvllm.tar.gz
tar -xvf xvllm.tar.gz && cd xvllm
bash scripts/build.sh build

# 验证
python -c "import vllm_xpu; from vllm_xpu._version import versions; print('xvllm OK'); print(versions)"
```

### 安装vllm-omni

```bash
cd /workspace
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_020/vllm-omni.tar.gz
tar -xvf vllm-omni.tar.gz && cd vllm-omni
VLLM_OMNI_TARGET_DEVICE=cuda pip install --no-build-isolation --no-deps -e .

# Install runtime dependencies (--no-deps prevents pip from pulling triton/torch)
# Core dependencies (safe to install with --no-deps)
pip install --no-deps "av>=14.0.0" "omegaconf>=2.3.0" "soundfile>=0.13.1" \
    "tqdm>=4.66.0" "einops>=0.8.1" "prettytable>=3.8.0" "aenum==3.1.16" \
    "pyzmq>=25.0.0" "janus>=1.0.0" "pydub" "x-transformers>=2.12.2"

# Torch-dependent packages (--no-deps to avoid pulling cuda/torch)
pip install --no-deps "diffusers>=0.36.0" "accelerate==1.12.0" \
    "torchsde>=0.2.6" "cache-dit==1.3.0"

# Inference and media packages
pip install --no-deps "imageio[ffmpeg]>=2.37.2" "onnxruntime>=1.23.2" \
    "openai-whisper>=20250625"

# Sub-dependencies required by packages above
pip install --no-deps torch-einops-utils
pip install cffi "antlr4-python3-runtime==4.9.3" einx

# Optional: Gradio for demo UI
pip install "gradio>=5.50"
```

### 安装xvllm-omni

```bash
cd /workspace
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_020/xvllm-omni.tar.gz
tar -xvf xvllm-omni.tar.gz && cd xvllm-omni
pip install -e .
```

## 权重准备

从huggingface上下载Qwen/Qwen3-Omni-30B-A3B-Instruct的权重


## 模型推理

### offline
1. Eager 模式
```bash
cd /workspace/xvllm-omni
# Text-only (仅 Stage 0 Thinker)
python examples/qwen3_omni/offline_inference.py \
    --model ${MODEL_DIR}/Qwen3-Omni-30B-A3B-Instruct \
    --query-type text --modalities text

# 全流水线 (Stage 0 → 1 → 2, text+audio 输出)
python examples/qwen3_omni/offline_inference.py \
    --model ${MODEL_DIR}/Qwen3-Omni-30B-A3B-Instruct \
    --query-type text
```

2. CUDA Graph 模式
```bash
cd /workspace/xvllm-omni
# Text-only + CUDA Graph
CUDA_VISIBLE_DEVICES=6,7 XMLIR_FORCE_USE_XPU_GRAPH=1 \
python examples/qwen3_omni/offline_inference.py \
    --model ${MODEL_DIR}/Qwen3-Omni-30B-A3B-Instruct \
    --stage-configs-path examples/qwen3_omni/stage_config_tp1_cudagraph.yaml \
    --query-type text --modalities text

# 全流水线 + CUDA Graph
CUDA_VISIBLE_DEVICES=6,7 XMLIR_FORCE_USE_XPU_GRAPH=1 \
python examples/qwen3_omni/offline_inference.py \
    --model ${MODEL_DIR}/Qwen3-Omni-30B-A3B-Instruct \
    --stage-configs-path examples/qwen3_omni/stage_config_tp1_cudagraph.yaml \
    --query-type text
```

### online
1. 启动服务

```bash
cd /workspace/xvllm-omni
# eager mode (TP=1, 2 GPUs, eager)
bash examples/qwen3_omni/start_server.sh /workspace/qwen3/Qwen3-Omni-30B-A3B-Instruct

# CUDA Graph mode (TP=1, 2 GPUs, ~3x faster decode)
CUDA_VISIBLE_DEVICES=6,7 \
bash examples/qwen3_omni/start_server_cudagraph.sh /workspace/qwen3/Qwen3-Omni-30B-A3B-Instruct
```

2. 在线测试

方式1: Send requests via curl
```bash
# Text-only output
curl http://localhost:8091/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
      "model": "Qwen3-Omni-30B-A3B-Instruct",
      "messages": [{"role": "user", "content": "Describe vLLM in brief."}],
      "modalities": ["text"]
    }'

# Text + Audio output
 curl http://localhost:8091/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
      "model": "Qwen3-Omni-30B-A3B-Instruct",
      "messages": [{"role": "user", "content": "Hello, tell me a joke."}],
      "modalities": ["audio"]
    }'
```

方式2: Send requests via OpenAI SDK
```bash
cd /workspace/xvllm-omni
python examples/qwen3_omni/openai_sdk_request.py
```

### Benchmark (evalscope perf)
1. 安装evalscope
```bash
pip install 'evalscope[perf]' -i https://mirrors.aliyun.com/pypi/simple/
```

2. 准备tokenizer
```bash
TOKENIZER_DIR=/tmp/qwen3_omni_tokenizer
MODEL_DIR={MODEL_DIR}
mkdir -p $TOKENIZER_DIR
cp ${MODEL_DIR}/Qwen3-Omni-30B-A3B-Instruct/{tokenizer*,vocab.json,merges.txt} $TOKENIZER_DIR/
python3 -c "
import json
with open('${MODEL_DIR}/Qwen3-Omni-30B-A3B-Instruct/chat_template.json') as f:
    ct = json.load(f)
with open('$TOKENIZER_DIR/tokenizer_config.json') as f:
    tc = json.load(f)
tc['chat_template'] = ct['chat_template']
with open('$TOKENIZER_DIR/tokenizer_config.json', 'w') as f:
    json.dump(tc, f, indent=2)
"
```

3.运行Benchmark
```bash
# NOTE: --extra-args '{"modalities": ["text"]}' is REQUIRED for Qwen3-Omni,
# otherwise the model generates audio alongside text causing timeout.
# Run benchmark (streaming mode)
TOKENIZER_DIR=/tmp/qwen3_omni_tokenizer

evalscope perf \
    --model Qwen3-Omni-30B-A3B-Instruct \
    --url http://localhost:8091/v1/chat/completions \
    --api-key "EMPTY" \
    --parallel 1 --number 5 \
    --api openai \
    --dataset openqa \
    --min-tokens 256 --max-tokens 256 \
    --stream \
    --tokenizer-path $TOKENIZER_DIR \
    --extra-args '{"modalities": ["text"]}'
```