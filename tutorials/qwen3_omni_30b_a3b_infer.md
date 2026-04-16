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
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_017/xvllm.tar.gz
tar -xvf xvllm.tar.gz && cd xvllm
bash scripts/build.sh build

# 验证
vllm_xpu -v
```

### 安装vllm-omni

```bash
git clone https://github.com/vllm-project/vllm-omni.git
cd vllm-omni
git checkout release/v0.17.0rc1

# 安装（不覆盖 xpytorch torch）
VLLM_OMNI_TARGET_DEVICE=cuda pip install --no-build-isolation --no-deps -e .
```

### 安装xvllm-omni

```bash
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_017/xvllm-omni.tar.gz
tar -xvf xvllm-omni.tar.gz && cd xvllm-omni
pip install -e .
```

### 安装其他依赖

```bash
pip install aenum
pip install omegaconf
pip install prettytable
pip install librosa
pip install pydub
# 用于evalscope性能测试
pip install 'evalscope[app,perf]' -U
```

## 模型准备

### 权重下载

从huggingface上下载Qwen/Qwen3-Omni-30B-A3B-Instruct的权重


## 模型推理

### offline
方式1. 基于xvllm-omni
```bash
cd <your_path>/xvllm-omni

# 仅验证环境（不做推理）
python examples/qwen3_omni/offline_inference.py \
  --model <your_path>/Qwen3-Omni-30B-A3B-Instruct  \
  --verify-only

# 纯文本查询（仅 Stage-0，最快）
python examples/qwen3_omni/offline_inference.py \
  --model <your_path>/Qwen3-Omni-30B-A3B-Instruct \
  --query-type text --modalities text \
  --prompt "Explain quantum computing in 20 words"

python examples/qwen3_omni/offline_inference.py \
  --model <your_path>/Qwen3-Omni-30B-A3B-Instruct \
  --query-type use_image --modalities text \
  --image-path examples/qwen3_omni/assets/cherry_blossom.jpg

# 音频理解
python examples/qwen3_omni/offline_inference.py \
  --model <your_path>/Qwen3-Omni-30B-A3B-Instruct  \
  --query-type use_audio --modalities text \
  --audio-path examples/qwen3_omni/assets/mary_had_lamb.ogg

# 全流程：文本 + 语音输出（Stage-0 → Stage-1 → Stage-2）
python examples/qwen3_omni/offline_inference.py \
  --model <your_path>/Qwen3-Omni-30B-A3B-Instruct  \
  --query-type text

```
方式2. 基于vllm-omni
```bash
cd <your_path>/vllm-omni/examples/offline_inference/qwen3_omni

# end2end.py中265行修改model_name
python end2end.py \
  --output-wav output_audio \
  --query-type use_audio

python end2end.py \
  --query-type text \
  --modalities text
```

### online
1. 启动服务
```bash
# 方式一：使用 xvllm-omni 启动脚本
cd <your_path>/xvllm-omni

bash examples/qwen3_omni/start_server.sh <your_path>/Qwen3-Omni-30B-A3B-Instruct
bash examples/qwen3_omni/start_server.sh <your_path>/Qwen3-Omni-30B-A3B-Instruct 8091  # 自定义端口

# 方式二：使用vllm单卡启动
CUDA_VISIBLE_DEVICES=0 vllm serve <your_path>/Qwen3-Omni-30B-A3B-Instruct \
  --omni --port 8091 --enforce-eager \
  --served-model-name Qwen3-Omni-30B-A3B-Instruct

# 方式三：使用vllm 多卡 Tensor Parallel（Thinker TP=2 在 GPU 0,1；Talker/Code2Wav 在 GPU 2）
CUDA_VISIBLE_DEVICES=0,1,2 vllm serve <your_path>/Qwen3-Omni-30B-A3B-Instruct \
  --omni --port 8091 --enforce-eager \
  --served-model-name Qwen3-Omni-30B-A3B-Instruct \
  --stage-configs-path examples/qwen3_omni/stage_config_tp2.yaml

# 方式四：文本 thinking 模式（自定义 stage config）
cd <your_path>/vllm-omni
CUDA_VISIBLE_DEVICES=0 vllm serve <your_path>/Qwen3-Omni-30B-A3B-Instruct \
  --omni --port 8091 --enforce-eager \
  --served-model-name Qwen3-Omni-30B-A3B-Instruct \
  --stage-configs-path examples/online_serving/qwen3_omni/qwen3_omni_moe_thinking.yaml
```
2. 在线测试

方式1: 通过python发送请求
```bash
import base64
from openai import OpenAI

client = OpenAI(base_url="http://localhost:8091/v1", api_key="EMPTY")

# 纯文本
response = client.chat.completions.create(
    model="Qwen3-Omni-30B-A3B-Instruct",
    messages=[{"role": "user", "content": "Describe vLLM in brief."}],
    modalities=["text"],
)
print(response.choices[0].message.content)

# 文本 + 语音（保存 wav）
response = client.chat.completions.create(
    model="Qwen3-Omni-30B-A3B-Instruct",
    messages=[{"role": "user", "content": "Hello, tell me a joke."}],
    modalities=["audio"],
)
for choice in response.choices:
    if choice.message.audio:
        audio_data = base64.b64decode(choice.message.audio.data)
        with open("output.wav", "wb") as f:
            f.write(audio_data)
        print(f"Audio saved to output.wav ({len(audio_data)} bytes)")
    elif choice.message.content:
        print("Text:", choice.message.content)
```

方式2: 通过curl发送请求
```bash
# 纯文本输出
curl http://localhost:8091/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen3-Omni-30B-A3B-Instruct",
    "messages": [{"role": "user", "content": "Describe vLLM in brief."}],
    "modalities": ["text"]
  }'

# 文本 + 语音输出
curl http://localhost:8091/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen3-Omni-30B-A3B-Instruct",
    "messages": [{"role": "user", "content": "Hello, tell me a joke."}],
    "modalities": ["audio"]
  }'
```

方式3: 客户端请求
```bash
cd cd <your_path>/vllm-omni/examples/online_serving/qwen3_omni

# 图像输入（使用本地文件避免服务端下载超时）
python openai_chat_completion_client_for_multimodal_generation.py \
  --query-type use_image --port 8091 \
  -m Qwen3-Omni-30B-A3B-Instruct \
  --image-path <your_path>/cherry_blossom.jpg

# 音频输入
python openai_chat_completion_client_for_multimodal_generation.py \
  --query-type use_audio --port 8091 \
  -m Qwen3-Omni-30B-A3B-Instruct \
  --audio-path <your_path>/mary_had_lamb.ogg

# 视频输入（自带测试视频）
python openai_chat_completion_client_for_multimodal_generation.py \
  --query-type use_video --port 8091 \
  -m Qwen3-Omni-30B-A3B-Instruct \
  --video-path <your_path>/baby_reading.mp4 \
  --prompt "What are the main activities shown in this video?"

# Streaming 流式输出
python openai_chat_completion_client_for_multimodal_generation.py \
  --query-type use_image --port 8091 \
  -m Qwen3-Omni-30B-A3B-Instruct \
  --image-path <your_path>/cherry_blossom.jpg \
  --stream
```

### 性能测试
注意：这里使用了Qwen3-30B-A3B-Instruct的tokenizer路径，因为Qwen3-Omni-30B-A3B-Instruct和Qwen3-30B-A3B-Instruct使用相同的文本tokenizer。
```bash
evalscope perf \
    --model Qwen3-Omni-30B-A3B-Instruct \
    --url http://localhost:8091/v1/chat/completions \
    --api-key "API_KEY" \
    --parallel 1 5 10 50 100 \
    --number 2 10 20 100 200 \
    --api openai \
    --dataset random_vl \
    --min-tokens 1024 \
    --max-tokens 1024 \
    --prefix-length 0 \
    --min-prompt-length 1024 \
    --max-prompt-length 1024 \
    --image-width 512 \
    --image-height 512 \
    --image-format RGB \
    --image-num 1 \
    --tokenizer-path <your_path>/Qwen3-30B-A3B-Instruct-2507
```