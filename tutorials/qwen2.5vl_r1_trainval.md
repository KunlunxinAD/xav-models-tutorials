# Qwen2.5-VL-R1 Trainval Guide

## 环境准备
### 准备开发环境镜像
请联系相关人员获取开发环境镜像

### PCIe环境配置（OAM跳过此步骤）
#### 确定PCIe环境
在宿主机执行如下命令：
```bash
xpu_smi xpulink -s
```
如果检测到显示为“P800 PCIe”，并且 Link 0/1/2/3 的状态均为“active”，则表明当前系统处于 PCIe 模式且8张卡已成功实现互联。

#### 配置PCIe的8卡互联模式
如果当前系统处于 PCIe 模式但8张卡未成功实现互联，需要在宿主机系统端执行以下配置操作：
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
# 从modelscope下载相关数据集与yaml文件
git lfs install
git clone https://www.modelscope.cn/datasets/AI-ModelScope/VLM-R1.git

# 解压对应数据
unzip train2014.zip
unzip lisa_test.zip
unzip rec_jsons_processed.zip


# 如需使用自定义数据集请参考
https://github.com/om-ai-lab/VLM-R1?tab=readme-ov-file#for-your-own-data
```

### 下载代码及预训练权重
```bash
# 从modelscope下载预训练权重
# 下载qwen2.5-VL-3B 模型权重
cd /home
git lfs install
git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git

# 下载VLM-R1框架
git clone https://github.com/om-ai-lab/VLM-R1.git
```


## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export LLAMA_CONTAINER=Qwen2.5_VL_r1_test
export MODEL_PATH=</path/to/qwen2.5_vl_r1> #本地路径

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
# 拉取xmlir 产出
wget https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/release/3.2.0/output/20250418/xpytorch-cp310-torch251-ubuntu2004-x64.run
bash xpytorch-cp310-torch251-ubuntu2004-x64.run

# 更新环境
cd VLM-R1/src/open-r1-multimodal
pip install -e ".[dev]"

pip install transformers==4.50.1
pip install perf==0.11.1
pip install flash-attn==2.4.0.post1
pip install babel 
pip install python-Levenshtein
pip install pycocotools
pip install json_repair
pip install openai
pip install qwen_vl_utils

# 安装patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/qwen2.5_vl_transformer_patch
cd ..
patch -p0 < /home/qwen2.5_vl_transformer_patch
# 修改xre依赖路径
mv /opt/xre /opt/xre_bak
```

# 单机多卡训练
### 设置训练路径与参数
```bash
cd VLM-R1/run_scripts/
#根据实际数据与代码路径、训练参数进行修改
vim run_grpo_rec.sh
```

### 执行训练
```bash
bash run_grpo_rec.sh
```

### 训练脚本内容示例
```bash
PROJECT_ROOT="$( cd '$( dirname "${BASH_SOURCE[0]}" )/..' && pwd )"
export REPO_HOME="${PROJECT_ROOT}"
echo "REPO_HOME: $REPO_HOME"
# Change the data_paths and image_folders to your own data
data_paths="/home/qwen2.5_vlm/VLM-R1/rec_jsons_processed/lisa_test.json:/home/qwen2.5_vlm/VLM-R1/rec_jsons_processed/refcocop_train.jsonl:/home/qwen2.5_vlm/VLM-R1/rec_jsons_processed/refcocog_train.jsonl" 
image_folders="/home/qwen2.5_vlm/VLM-R1/:/home/qwen2.5_vlm/VLM-R1/:/home/qwen2.5_vlm/VLM-R1/"
model_path="/home/qwen2.5_vlm/Qwen2.5-VL-3B-Instruct"
is_reward_customized_from_vlm_module=True
echo "data_paths: $data_paths"
echo "image_folders: $image_folders"

export EXP_NAME="Qwen2.5-VL-3B-Instruct-rec" # TODO: change this to your own experiment name
TASK_TYPE="rec"
cd ${REPO_HOME}/src/open-r1-multimodal

#export DEBUG_MODE="true" # Enable Debug if you want to see the rollout of model during RL
# create the run directory and log file
mkdir -p ${REPO_HOME}/runs/${EXP_NAME}/log
export LOG_PATH="${REPO_HOME}/runs/${EXP_NAME}/log/debug_log.$(date +%Y-%m-%d-%H-%M-%S).txt"
export MAX_STEPS=200 # TODO: change this to your own max steps

export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export XMLIR_BMM_DISPATCH_VALUE=0
export XMLIR_ENABLE_LINEAR_FC_FUSION=1
unset CUDA_LAUNCH_BLOCKING
unset COPY2D_SDNN
export USE_FAST_BF16_FC=true


torchrun --nproc_per_node="8" \
    --nnodes="1" \
    --node_rank="0" \
    --master_addr="127.0.0.1" \
    --master_port="12349" \
  src/open_r1/grpo_jsonl.py \
    --use_vllm False \
    --output_dir ${REPO_HOME}/checkpoints/rl/${EXP_NAME} \
    --resume_from_checkpoint True \
    --model_name_or_path $model_path \
    --data_file_paths $data_paths \
    --image_folders $image_folders \
    --is_reward_customized_from_vlm_module $is_reward_customized_from_vlm_module \
    --task_type $TASK_TYPE \
    --per_device_train_batch_size 8 \
    --gradient_accumulation_steps 2 \
    --gradient_checkpointing true \
    --logging_steps 1 \
    --num_train_epochs 2 \
    --max_steps $MAX_STEPS \
    --bf16 \
    --attn_implementation flash_attention_2 \
    --run_name ${EXP_NAME} \
    --data_seed 42 \
    --save_steps 100 \
    --num_generations 8 \
    --max_completion_length 2048 \
    --reward_funcs accuracy format \
    --beta 0.04 \
    --report_to none \
    --dataset-name this_is_not_used \
    --deepspeed ${REPO_HOME}/src/open-r1-multimodal/local_scripts/zero3.json \

echo "Training completed for ${EXP_NAME}"
```

# 单机多卡评估
### 设置评估路径与参数
```bash
cd VLM-R1/src/eval/
#根据实际数据与代码路径、评估参数进行修改
vim test_rec_r1.py
```

### 执行评估
```bash
export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export XMLIR_BMM_DISPATCH_VALUE=2
export XMLIR_ENABLE_LINEAR_FC_FUSION=1                                                                                                                                     
unset CUDA_LAUNCH_BLOCKING
unset COPY2D_SDNN
export USE_FAST_BF16_FC=true
export XMLIR_ENABLE_XBLAS_ADDMM=0
export BKCL_PCIE_TOPO=1

export FORCE_DISABLE_INPLACE_BF16_CAST=0
export DISABLE_CAST_CACHE=true
export PYTORCH_CUDA_ALLOC_CONF=max_split_size_mb:512,expandable_segments:True

torchrun --nproc_per_node=8 test_rec_r1.py --max_pixels 262144
```

