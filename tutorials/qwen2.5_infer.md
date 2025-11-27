# Qwen2.5 Inference Guide

## 镜像获取
### 准备开发环境镜像
请联系相关人员获取开发环境镜像

## 环境准备
### 启动容器
```bash
#获取镜像版本
export image_name=<XAV_IMAGE>
docker run -it --name qwen25_infer_mt \
    --hostname localhost \
    -dev \
    -v `pwd`:/workspace \
    --shm-size=128G \
    --network=host \
    --privileged \
    --cap-add=SYS_PTRACE \
    --security-opt seccomp=unconfined \
    $image_name /bin/bash

docker start qwen25_infer_mt
docker exec -it qwen25_infer_mt /bin/bash
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
### 模型下载到models目录并解压,以Qwen2.5-72B-Instruct为例
### 方式一：从huggingface官方网址下载权重
### 方式二：从BOS上下载权重
mkdir models & cd models
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_72b/Qwen2.5-72B-Instruct.tar.gz
tar zxvf Qwen2.5-72B-Instruct.tar.gz
```

## Build TensorRT engine
### 模型转换
```bash
cd /workspace/output/examples/qwen/
# fp16 + TP=8
python convert_checkpoint.py --model_dir /workspace/models/Qwen2.5-72B-Instruct/  \
                            --output_dir /workspace/convert_models/Qwen2.5-72B-Instruct_fp16_tp8 \
                            --dtype float16 \
                            --tp_size 8

# bf16 + TP=8
python convert_checkpoint.py --model_dir /workspace/models/Qwen2.5-72B-Instruct/  \
                            --output_dir /workspace/convert_models/Qwen2.5-72B-Instruct_bf16_tp8 \
                            --dtype bfloat16 \
                            --tp_size 8

# int8 + TP=8
python convert_checkpoint.py --model_dir /workspace/models/Qwen2.5-72B-Instruct/  \
                            --output_dir /workspace/convert_models/Qwen2.5-72B-Instruct_int8_tp8 \
                            --dtype float16 \
                            --use_weight_only \
                            --weight_only_precision int8 \
                            --tp_size 8
```
### 构建engine
```bash
# fp16 
trtllm-build --checkpoint_dir /workspace/convert_models/Qwen2.5-72B-Instruct_fp16_tp8 \
            --output_dir /workspace/engines/qwen2.5-72B-Instruct_engines_fp16_tp8  \
            --gemm_plugin float16

# bf16 
trtllm-build --checkpoint_dir /workspace/convert_models/Qwen2.5-72B-Instruct_bf16_tp8 \
            --output_dir /workspace/engines/qwen2.5-72B-Instruct_engines_bf16_tp8  \
            --gpt_attention_plugin bfloat16 \
            --gemm_plugin bfloat16

# int8
trtllm-build --checkpoint_dir /workspace/convert_models/Qwen2.5-72B-Instruct_int8_tp8 \
            --output_dir /workspace/engines/qwen2.5-72B-Instruct_engines_int8_tp8  \
            --gemm_plugin float16
```

## 执行运行
```bash
# With fp16 inference
export XPU_VISIBLE_DEVICES="0,1,2,3,4,5,6,7"
mpirun -n 8 --allow-run-as-root \
    python ../run.py --input_text "What is your name?" \
                  --max_output_len=50 \
                  --tokenizer_dir /workspace/models/Qwen2.5-72B-Instruct/ \
                  --engine_dir=/workspace/engines/qwen2.5-72B-Instruct_engines_fp16_tp8/

# With bf16 inference
export XPU_VISIBLE_DEVICES="0,1,2,3,4,5,6,7"
mpirun -n 8 --allow-run-as-root \
    python ../run.py --input_text "What is your name?" \
                  --max_output_len=50 \
                  --tokenizer_dir /workspace/models/Qwen2.5-72B-Instruct/ \
                  --engine_dir=/workspace/engines/qwen2.5-72B-Instruct_engines_bf16_tp8/

# With int8 inference
export XPU_VISIBLE_DEVICES="0,1,2,3,4,5,6,7"
mpirun -n 8 --allow-run-as-root \
    python ../run.py --input_text "What is your name?" \
                  --max_output_len=50 \
                  --tokenizer_dir /workspace/models/Qwen2.5-72B-Instruct/ \
                  --engine_dir=/workspace/engines/qwen2.5-72B-Instruct_engines_int8_tp8/
```

## 摘要生成
```bash
# With fp16 inference
export XPU_VISIBLE_DEVICES="0,1,2,3,4,5,6,7"
mpirun -n 8 --allow-run-as-root \
    python ../summarize.py --test_trt_llm \
                       --hf_model_dir  /workspace/models/Qwen2.5-72B-Instruct/ \
                       --data_type fp16 \
                       --engine_dir /workspace/engines/qwen2.5-72B-Instruct_engines_fp16_tp8/ \
                       --max_input_length 2048 \
                       --output_len 2048

# With bf16 inference
export XPU_VISIBLE_DEVICES="0,1,2,3,4,5,6,7"
mpirun -n 8 --allow-run-as-root \
    python ../summarize.py --test_trt_llm \
                       --hf_model_dir  /workspace/models/Qwen2.5-72B-Instruct/ \
                       --data_type fp16 \
                       --engine_dir /workspace/engines/qwen2.5-72B-Instruct_engines_bf16_tp8/ \
                       --max_input_length 2048 \
                       --output_len 2048

# With int8 inference
export XPU_VISIBLE_DEVICES="0,1,2,3,4,5,6,7"
mpirun -n 8 --allow-run-as-root \
    python ../summarize.py --test_trt_llm \
                       --hf_model_dir  /workspace/models/Qwen2.5-72B-Instruct/ \
                       --data_type fp16 \
                       --engine_dir /workspace/engines/qwen2.5-72B-Instruct_engines_int8_tp8/ \
                       --max_input_length 2048 \
                       --output_len 2048
```

## 在线推理
### 创建服务
```bash
# 下载启动服务脚本
cd /workspace
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_72b/run_server_qwen25.sh
# 启动fp16_tp8服务
bash run_server_qwen25.sh /workspace/models/Qwen2.5-72B-Instruct  /workspace/engines/qwen2.5-72B-Instruct_engines_fp16_tp8
```
### 功能测试
```bash
curl http://localhost:8802/v1/completions \
    -H "Content-Type: application/json" \
    -d '{
        "model": "/workspace/models/Qwen2.5-72B-Instruct",
        "prompt": "请介绍下QWEN模型是什么？",
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
        --model /workspace/models/Qwen2.5-72B-Instruct \
        --dataset-name random \
        --num-prompts 100 \
        --random-input-len 1024 \
        --random-output-len 2048 \
        --ignore-eos
```
