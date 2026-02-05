# Qwen3 Trainval Guide (LlamaFactory)

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器

```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=qwen-llamafactory-test

docker run -itd --privileged --net=host \
    --security-opt=seccomp=unconfined \
    -v `pwd`:/workspace \
    -w /workspace \
    --name ${XAV_CONTAINER} \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --shm-size 256g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${XAV_CONTAINER} bash
```
## 配置容器内环境

```bash
pip install transformers==4.51.0
pip install omegaconf==2.3.0
pip install numpy==1.26.4 
pip install peft==0.14.0
pip install accelerate==1.8.1
pip install --no-build-isolation flash-attn==2.4.0.post1
pip install huggingface_hub
```

## 下载框架及安装

### LlamaFactory

```bash
cd /workspace
git clone https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
git reset --hard b44f651e0905fed54f9455acd25bc2cfed8f1b94
pip install --no-deps -e .

# 新建两个空目录，用于存放模型和运行配置
mkdir models
mkdir configs

# 这一步需要把文件里的sub_group_size改为5e8（原1e9）
vim examples/deepspeed/ds_z3_config.json
```

### XDeepSpeed

```bash
cd /workspace
wget https://klx-sdk-release-public.su.bcebos.com/XDeepSpeed/release/1.0.0.0/XDeepSpeed.tar.gz
tar -zvxf XDeepSpeed.tar.gz
cd XDeepSpeed

pip install deepspeed-0.16.7+27433e93-py3-none-any.whl
```

## 下载模型权重及配置

### Qwen3-4B

```bash
huggingface-cli download Qwen/Qwen3-4B --local-dir /workspace/LLaMA-Factory/models/Qwen3-4B
touch /workspace/LLaMA-Factory/configs/qwen3_4b_sft.yaml
```

`qwen3_4b_sft.yaml`的内容可根据训练需求灵活配置，示例如下：
```bash
model_name_or_path: /workspace/LLaMA-Factory/models/Qwen3-4B
trust_remote_code: true

### method
stage: sft
do_train: true
finetuning_type: full
freeze_vision_tower: true
freeze_multi_modal_projector: true
freeze_language_model: false
deepspeed: examples/deepspeed/ds_z3_config.json

### dataset
dataset: identity,alpaca_en_demo
template: qwen3
cutoff_len: 2048
max_samples: 1000
overwrite_cache: true
preprocessing_num_workers: 16
dataloader_num_workers: 4

### output
output_dir: saves/qwen3-4b/full/sft
logging_steps: 1 # 每个step都打印出loss
save_steps: 5000
plot_loss: true
overwrite_output_dir: true
save_only_model: false
report_to: none  # choices: [none, wandb, tensorboard, swanlab, mlflow]

### train
per_device_train_batch_size: 16
gradient_accumulation_steps: 2
learning_rate: 1.0e-5
# num_train_epochs: 3.0
max_steps: 20 #设置最多跑20个step
lr_scheduler_type: cosine
warmup_ratio: 0.1
bf16: true
ddp_timeout: 180000000
resume_from_checkpoint: null
```

### Qwen3-30B-A3B

```bash
huggingface-cli download Qwen/Qwen3-30B-A3B-Instruct-2507 --local-dir /workspace/LLaMA-Factory/models/Qwen3-30B-A3B-Instruct-2507
touch /workspace/LLaMA-Factory/configs/qwen3_30b_a3b_sft.yaml
```

`qwen3_30b_a3b_sft.yaml`的内容可根据训练需求灵活配置，示例如下：
```bash
model_name_or_path: /workspace/LLaMA-Factory/models/Qwen3-30B-A3B-Instruct-2507
trust_remote_code: true

### method
stage: sft
do_train: true
finetuning_type: full
deepspeed: examples/deepspeed/ds_z3_config.json  # choices: [ds_z0_config.json, ds_z2_config.json, ds_z3_config.json]
gradient_checkpointing: true
max_grad_norm: 1.0

### dataset
dataset: alpaca_en_demo
template: qwen3_nothink
cutoff_len: 1024
max_samples: 1000
overwrite_cache: true
preprocessing_num_workers: 16
dataloader_num_workers: 4

### output
output_dir: saves/qwen3-30b/full/sft
logging_steps: 1
save_steps: 1000
max_steps: 500
plot_loss: true
overwrite_output_dir: true

### train
per_device_train_batch_size: 1
gradient_accumulation_steps: 1
learning_rate: 1.0e-6
num_train_epochs: 2.0
lr_scheduler_type: cosine
warmup_ratio: 0.1
flash_attn: fa2
report_to: none
bf16: true
ddp_timeout: 180000000
include_num_input_tokens_seen: true
```

## 模型训练

### Qwen3-4B

```bash
cd /workspace/LLaMA-Factory
llamafactory-cli train ./configs/qwen3_4b_sft.yaml
```

### Qwen3-30B-A3B

```bash
cd /workspace/LLaMA-Factory
llamafactory-cli train ./configs/qwen3_30b_a3b_sft.yaml
```