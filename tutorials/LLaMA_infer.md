# LLaMA Inference Guide

## 镜像获取
### 准备开发环境镜像
请联系相关人员获取开发环境镜像

## 环境准备
### 启动容器
```bash
#获取镜像版本
export image_name=<XAV_IMAGE>
docker run -it --name LLaMA_infer_mt \
    --hostname localhost \
    -dev \
    -v `pwd`:/workspace \
    --shm-size=128G \
    --network=host \
    --privileged \
    --cap-add=SYS_PTRACE \
    --security-opt seccomp=unconfined \
    $image_name /bin/bash

docker start LLaMA_infer_mt
docker exec -it LLaMA_infer_mt /bin/bash
```
### 容器内环境配置
```bash
cd /workspace
### 下载测试代码包
wget https://klx-sdk-release-public.su.bcebos.com/xtrt_llm/release/daily/xtrt_llm_0.10.0/20250410/xtrt_llm_ubuntu2004_output.tar.gz
tar zxvf xtrt_llm_ubuntu2004_output.tar.gz
```
### 依赖安装
```bash
source /home/pt201/bin/activate
cd output
bash scripts/install_release.sh
source scripts/set_release_env.sh
```
### 模型下载
```bash
cd /workspace
### 模型下载到models目录并解压,以deepseek-r1-distill-llama-70b为例
### 从huggingface官方网址下载权重
https://huggingface.co/deepseek-ai/DeepSeek-R1-Distill-Llama-70B
```

## Build TensorRT engine
### 模型转换
```bash
cd /workspace/output/examples/llama/
# fp16 + TP=8
python3 convert_checkpoint.py --model_dir /workspace/models/DeepSeek-R1-Distill-Llama-70B \
                              --output_dir /workspace/convert_models/DeepSeek-R1-Distill-Llama-70B_fp16_tp8 \
                              --dtype float16 \
                              --tp_size 8 \
                              --load_model_on_cpu 

# bf16 + TP=8
python3 convert_checkpoint.py --model_dir /workspace/models/DeepSeek-R1-Distill-Llama-70B \
                              --output_dir /workspace/convert_models/DeepSeek-R1-Distill-Llama-70B_bf16_tp8 \
                              --dtype bfloat16 \
                              --tp_size 8 \
                              --load_model_on_cpu 

# int8 + TP=8
python3 convert_checkpoint.py --model_dir /workspace/models/DeepSeek-R1-Distill-Llama-70B \
                              --output_dir /workspace/convert_models/DeepSeek-R1-Distill-Llama-70B_int8_tp8 \
                              --dtype float16 \
                              --use_weight_only \
                              --weight_only_precision int8 \
                              --tp_size 8 \
                              --load_model_on_cpu
```
### 构建engine
```bash
# fp16 
python3 -m tensorrt_llm.commands.build \
        --checkpoint_dir /workspace/convert_models/DeepSeek-R1-Distill-Llama-70B_fp16_tp8 \
        --output_dir /workspace/engines/DeepSeek-R1-Distill-Llama-70B_engines_fp16_tp8   \
        --tp_size 8 \
        --workers 8 

# bf16 
python3 -m tensorrt_llm.commands.build \
        --checkpoint_dir /workspace/convert_models/DeepSeek-R1-Distill-Llama-70B_bf16_tp8 \
        --output_dir /workspace/engines/DeepSeek-R1-Distill-Llama-70B_engines_bf16_tp8   \
        --gpt_attention_plugin bfloat16 \
        --gemm_plugin bfloat16 \
        --tp_size 8 \
        --workers 8  

# int8
python3 -m tensorrt_llm.commands.build \
        --checkpoint_dir /workspace/convert_models/DeepSeek-R1-Distill-Llama-70B_int8_tp8 \
        --output_dir /workspace/engines/DeepSeek-R1-Distill-Llama-70B_engines_int8_tp8   \
        --gemm_plugin float16 \
        --tp_size 8 \
        --workers 8  
```

## 执行运行
```bash
# With fp16 inference
# Run Llama model with 8-gpu
cd /workspace/output/examples/llama
mpirun -n 8 --allow-run-as-root python3 ../run.py --input_text "世界上第二高的山峰是哪座？" \
                 --max_output_len=120 \
                 --log_level info \
                 --tokenizer_dir /workspace/models/DeepSeek-R1-Distill-Llama-70B \
                 --engine_dir=/workspace/engines/DeepSeek-R1-Distill-Llama-70B_engines_fp16_tp8
```

## 摘要生成
```bash
# With fp16 inference
# Run Llama model with 8-gpu
cd /workspace/output/examples/llama
mpirun -n 8 --allow-run-as-root \
    python ../summarize.py --test_trt_llm \
                       --hf_model_dir  /workspace/models/DeepSeek-R1-Distill-Llama-70B \
                       --data_type fp16 \
                       --engine_dir /workspace/engines/DeepSeek-R1-Distill-Llama-70B_engines_fp16_tp8

```

## 在线推理
### 创建服务
```bash
# 下载启动服务脚本
cd /workspace
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/DeepSeek-R1-Distill-Llama-70B/run_server_llama.sh
# 启动fp16_tp8服务
bash run_server_llama.sh /workspace/models/DeepSeek-R1-Distill-Llama-70B  /workspace/engines/DeepSeek-R1-Distill-Llama-70B_engines_fp16_tp8
```
### 功能测试
```bash
curl http://localhost:8802/v1/completions \
    -H "Content-Type: application/json" \
    -d '{
        "model": "/workspace/models/DeepSeek-R1-Distill-Llama-70B",
        "prompt": "世界上第二高的山峰是哪座？",
        "max_tokens": 512
    }'
```
### 定长测试
```bash
cd /workspace
# md5sum: 1118224abc7c93e19f47bbcefa41f222
wget https://klx-public.bj.bcebos.com/v1/anyinfer/DeepSeek/V9/pressure_test_v6_1.tar.gz && tar -zxvf pressure_test_v6_1.tar.gz
# performance
cd /workspace/pressure_test_v6_1/benchmarks
python3 benchmark_serving.py \
        --host 0.0.0.0 \
        --port 8802 \
        --backend vllm \
        --model /workspace/models/DeepSeek-R1-Distill-Llama-70B \
        --dataset-name random \
        --num-prompts 100 \
        --random-input-len 1024 \
        --random-output-len 2048 \
        --ignore-eos
```
