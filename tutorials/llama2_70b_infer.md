# Llama2-70B Inference Guide

## 环境准备

### 准备开发环境镜像
请联系相关人员获取开发环境镜像

## 启动容器
```bash
export image_name=<XAV_IMAGE>
docker run -it --name llama2_infer_mt \
    --hostname localhost \
    -dev \
    -v `pwd`:/workspace \
    --shm-size=128G \
    --network=host \
    --privileged \
    --cap-add=SYS_PTRACE \
    --security-opt seccomp=unconfined \
    $image_name /bin/bash

docker start llama2_infer_mt
docker exec -it llama2_infer_mt /bin/bash
```
### 容器内环境配置
```bash
cd /workspace
### 下载测试代码包，1017的daily build
wget https://klx-sdk-release-public.su.bcebos.com/xtrt_llm/release/daily/xtrt_llm_0.10.0/20241017/xtrt_llm_ubuntu1604_output.tar.gz
tar zxvf xtrt_llm_ubuntu1604_output.tar.gz
```

### 依赖安装
```bash
source /home/pt201/bin/activate
cd output
bash scripts/install_release.sh
source scripts/set_release_env.sh
```

## 模型下载
### 模型下载到models目录并解压
```bash
mkdir models
cd /workspace/models
wget https://klx-llm.fsh.bcebos.com/pretrained_models/Llama-2-70b-chat-hf.tar.gz
tar zxvf Llama-2-70b-chat-hf.tar.gz
```
## 推理定长测试
### 模型转换
```bash
cd examples/llama/
# TP=8
python convert_checkpoint.py  --model_dir /workspace/models/Llama-2-70b-chat-hf \
                              --output_dir /workspace/convert_models/Llama-2-70b-chat-hf/TP8 \
                              --dtype float16 \
                              --tp_size 8 \
                              --load_model_on_cpu
```
### 构建engine
```bash
XTCL_BUILD_DEBUG=0x1 python3 -m tensorrt_llm.commands.build \
            --checkpoint_dir /workspace/convert_models/Llama-2-70b-chat-hf/TP8 \
            --output_dir /workspace/engines/Llama-2-70b-chat-hf/TP8/ \
            --tp_size 8 \
            --workers 8
```
### 执行运行
```bash
export XPU_VISIBLE_DEVICES="0,1,2,3,4,5,6,7"
mpirun -n 8 --allow-run-as-root python3 ../run.py --max_output_len=2048 \
                  --tokenizer_dir /workspace/models/Llama-2-70b-chat-hf/ \
                  --engine_dir /workspace/engines/Llama-2-70b-chat-hf/TP8/ \
                  --log_level=info \
                  --performance_test_scale "1x1024x1024E1x1024x1024E2x1024x1024E4x1024x1024E8x1024x1024E16x1024x1024E32x1024x1024E64x1024x1024E128x1024x1024"
```
### 摘要生成
```bash
mpirun -n 8 --allow-run-as-root python ../summarize_long.py --test_trt_llm \
                    --hf_model_dir /workspace/models/Llama-2-70b-chat-hf/ \
                    --data_type fp16 \
                    --engine_dir /workspace/engines/Llama-2-70b-chat-hf/TP8/
```