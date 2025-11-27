# AlphaDrive Trainval Guide

## 环境准备
请联系昆仑芯客户支持获取开发环境镜像。

### PCIe环境配置（OAM跳过此步骤）
#### 确定PCIe环境
在宿主机执行如下命令：
```bash
xpu_smi xpulink -s
```
如果检测到显示为“P800 PCIe”，并且 Link 0/1/2/3 的状态均为“active”，则表明当前系统处于 PCIe 模式且8张卡已成功实现互联。

#### 配置PCIe的8卡互联模式
如果当前系统处于PCIe模式但8张卡未成功实现互联，需要在宿主机系统端执行以下配置操作：
```bash
# 步骤 1：修改/etc/modprobe.d目录下的kunlun.conf文件
vim /etc/modprobe.d/kunlun.conf
# 步骤 2：添加配置参数 PcieLink=1，该参数应与其他配置参数置于同一对双引号内，并使用分号 (;) 进行分隔。示例如下：
options kunlun KLreg_RegistryDwords="RmDisableSMMU=1;RmKL3ShiftMode=0;PcieLink=1"
# 步骤 3：重启服务器，即可完成配置的修改，服务器可支持 8 卡互联。
```

## 数据集及代码准备
### 数据集准备
```bash
# 下载R1-V-AlphaDrive代码
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/alpha_drive/R1-V-AlphaDrive.tar.gz

# 下载原始Impromptu-vla数据集
git lfs install
git clone https://huggingface.co/datasets/aaaaaap/unstructed

# 下载处理好的alpha-drive-Impromptu-vla数据集
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/Impromptu_VLA/alpha_drive_ImpromptuData.tar.gz
tar -xzvf alpha_drive_ImpromptuData.tar.gz
```

### 下载代码及预训练权重
```bash
# 从modelscope下载预训练权重
# 下载qwen2-VL-2B 模型权重
cd /home
git lfs install
git clone https://www.modelscope.cn/Qwen/Qwen2-VL-2B-Instruct.git
```


## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export NAME_CONTAINER=alpha_drive
export MODEL_PATH=</path/to/Qwen2-VL-2B-Instruct> #本地路径

docker run -itd --privileged --net=host \
    -w /workspace/xav-models/modelzoo \
    -v ${MODEL_PATH}:/home \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${NAME_CONTAINER} \
    --shm-size 256g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${NAME_CONTAINER} bash
```

### 容器内环境配置
```bash
# 切换 conda env
conda activate python310_torch25_cuda
# 安装xvllm
# 下载产出包
wget https://klx-sdk-release-public.su.bcebos.com/xvllm/KL3/0.8.2/latest/output.tar.gz
# 创建xvllm环境
tar -xvf output.tar.gz && mv output xvllm_0.8.2
cd xvllm_0.8.2
# 安装xvllm及其依赖
set -xe
SCRIPTS_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
BASE_DIR=$(readlink -f "${SCRIPTS_DIR}/..")

pip config set global.disable-pip-version-check true
if pip show vllm_xpu &>/dev/null; then
    echo "Find vllm_xpu, Uninstalling..."
    pip uninstall -y vllm_xpu
fi

pip install vllm*.whl --force-reinstall --no-deps


echo "The installation was successful. The detailed version information is as follows:"
cat version.txt




# 更新环境
cd R1-V-AlphaDrive
bash setup.sh

pip install transformers==4.51.0
pip install lighteval
pip install clip
pip install flash-attn==2.4.0.post1
pip install qwen-vl-utils[decord]
pip install vllm==0.8.2 --no-deps


# 如果下载的是原始Impromptu-vla数据集
# 需要进行数据提取和重建
python alpha-drive/src/scripts/data_extract.py --dataset all --input_folder /path/to/unstructed --output_folder ./data
python alpha-drive/src/scripts/data_restructure.py  --dataset all --input_folder ./data --output_folder ./alpha_drive_ImpromptuData
```

## 单机多卡训练
### 设置环境变量
```bash
export DIST_MULTI_STREAM=false #关闭多流
# 使用vllm需要额外设置的环境变量
export XMLIR_ENABLE_MOCK_TORCH_COMPILE=false
export XMLIR_FORCE_USE_XPU_GRAPH=1
export VLLM_USE_V1=0
```

### 执行训练
```bash
torchrun --nproc_per_node="8" \
    --nnodes="1" \
    --node_rank="0" \
    --master_addr="127.0.0.1" \
    --master_port="12345" \
    src/r1-v/src/open_r1/grpo_alpha_drive.py \
    --output_dir <OUTPUT_DIR> \
    --model_name_or_path <PATH-TO-Qwen2-VL-2B-Instruct> \ 
    --dataset_name  /path/to/alpha_drive_ImpromptuData/train/nuscenes\  
    --deepspeed ./src/r1-v/local_scripts/zero3.json \
    --max_prompt_length 1024 \
    --max_completion_length 512 \
    --per_device_train_batch_size 1 \
    --gradient_accumulation_steps 4 \
    --logging_steps 1 \
    --bf16 \
    --report_to wandb \
    --gradient_checkpointing false \
    --attn_implementation flash_attention_2 \
    --max_pixels 401408 \
    --num_train_epochs 2 \
    --run_name Qwen2-VL-2B-GRPO-alpha-drive \
    --save_steps 100 \
    --save_only_model true \
    --num_generations 4   # number of outputs G in grpo, reduce it would lead to faster training and smaller memory cost but higher variance  
```

### 使用vllm加速训练
```bash
torchrun --nproc_per_node="8" \
    --nnodes="1" \
    --node_rank="0" \
    --master_addr="127.0.0.1" \
    --master_port="12345" \
    src/r1-v/src/open_r1/grpo_alpha_drive.py \
    --use_vllm=True \
    --vllm_gpu_memory_utilization 0.4 \
    --vllm_mode colocate \
    --vllm_tensor_parallel_size 1 \
    --output_dir <OUTPUT_DIR> \
    --model_name_or_path <PATH-TO-Qwen2-VL-2B-Instruct> \ 
    --dataset_name  /path/to/alpha_drive_ImpromptuData/train/nuscenes\  
    --deepspeed ./src/r1-v/local_scripts/zero3_offload.json \
    --max_prompt_length 1024 \
    --max_completion_length 512 \
    --per_device_train_batch_size 1 \
    --gradient_accumulation_steps 4 \
    --logging_steps 1 \
    --bf16 \
    --report_to wandb \
    --gradient_checkpointing false \
    --attn_implementation flash_attention_2 \
    --max_pixels 401408 \
    --num_train_epochs 2 \
    --run_name Qwen2-VL-2B-GRPO-alpha-drive \
    --save_steps 100 \
    --save_only_model true \
    --num_generations 4   # number of outputs G in grpo, reduce it would lead to faster training and smaller memory cost but higher variance 
```

## 单机多卡评估
### 执行评估
```bash
python eval_tools/qwen2vl_plan_cmd_eval_grpo_multigpu.py --eval_data_path=/path/to/alpha_drive_ImpromptuData/val/nuscenes --save_path=./output_eval/ --model_path=/path/to/your_model --gpu_ids 0,1,2,3 
```
