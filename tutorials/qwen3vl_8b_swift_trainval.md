# Qwen3-VL-8B MS-Swift Trainval Guide

## 环境准备
### 准备开发环境镜像
请联系相关人员获取开发环境镜像

## 数据集及代码准备
### 数据集准备
```bash
# 参考官方文档选择微调数据集
https://swift.readthedocs.io/zh-cn/latest/Customization/Custom-dataset.html

# 当前使用coco 数据集，下载数据
git lfs install
git clone https://huggingface.co/datasets/detection-datasets/coco
```

### 下载代码及预训练权重
```bash
# 从huggingface 下载预训练权重
# 下载Qwen3-VL-8B-Instruct 模型权重
cd /home
git lfs install
git clone https://huggingface.co/Qwen/Qwen3-VL-8B-Instruct

# 下载ms-swift代码以及配置
git clone https://github.com/modelscope/ms-swift.git
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export LLAMA_CONTAINER=Qwen3VL_test
export MODEL_PATH=</path/to/qwen3_vl> #本地路径
 
docker run -itd --privileged --net=host \
    -w /workspace/xav-models/modelzoo \
    -v ${MODEL_PATH}:/home \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${LLAMA_CONTAINER} \
    --shm-size 256g \
    ${XAV_IMAGE} \
    bash
 
docker exec -it ${XAV_CONTAINER} bash
```

### 容器内环境配置
```bash
# 切换 conda env
conda activate python310_torch25_cuda

# 更新环境
cd ms-swift
pip install -e .

pip install qwen_vl_utils
pip install wandb 
```

# 单机多卡训练
### 设置训练路径与参数
```bash
#根据实际数据与代码路径、训练参数进行修改
vim test_qwen3_vl.sh
```

### 脚本内容参考
```bash
export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export NPROC_PER_NODE=8
export XMLIR_BMM_DISPATCH_VALUE=2
export XMLIR_ENABLE_LINEAR_FC_FUSION=1
export BKCL_PCIE_TOPO=1
export XMLIR_ENABLE_FAST_FC=1
unset CUDA_LAUNCH_BLOCKING
unset COPY2D_SDNN

export XPYTORCH_RUN_ENHANCE=1
export XDNN_USE_FAST_SWISH=1
export XDNN_FAST_DIV_SCALAR=true
export XPUAPI_SDNN_BF16_ROUND_MODE=3

export MODELSCOPE_CACHE=/home/qwen2/temp
export MAX_PIXELS=1003520

export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True
####请替换成自己的WANDB_API_KEY
export WANDB_API_KEY="xxxxx"
export WANDB_PROJECT="qwen3-vl"

swift sft \
     --model /home/Qwen3-VL-8B-Instruct \
     --train_type lora \
     --deepspeed zero2 \
     --dataset /home/coco#3000 \
     --torch_dtype bfloat16 \
     --num_train_epochs 1 \
     --per_device_train_batch_size 10\
     --learning_rate 1e-5 \
     --gradient_accumulation_steps  1 \
     --save_steps 500 \
     --max_steps 500 \
     --save_total_limit 5 \
     --logging_steps 1 \
     --max_length 2048 \
     --attn_impl flash_attn  \
     --output_dir output \
     --warmup_ratio 0.05 \
     --lazy_tokenize true \
     --include_num_input_tokens_seen true \
     --check_model false \
     --report_to wandb
```

### 执行Lora
```bash
##如需修改微调类型为fp16，请修改文件中对应参数
bash test_qwen3_vl.sh
```
