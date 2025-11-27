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
# 下载TRL-AlphaDrive代码
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/alpha_drive/TRL-AlphaDrive.tar.gz

# 下载处理好的alpha-drive-Impromptu-vla数据集
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/Impromptu_VLA/alpha_drive_ImpromptuData.tar.gz
tar -xzvf alpha_drive_ImpromptuData.tar.gz
```

### 下载代码及预训练权重
```bash
# 从modelscope下载预训练权重
# 下载qwen2.5-VL-3B 模型权重
cd /home
git lfs install
git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git
```


## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export NAME_CONTAINER=alpha_drive
export MODEL_PATH=</path/to/Qwen2.5-VL-3B-Instruct> #本地路径

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
# 安装trl
pip install trl==0.23.0
# 安装transformers
pip install transformers==4.51.0
# 修改transformers,可以通过 pip show transformers 查找transformers的路径
cd /root/miniconda/envs/python310_torch25_cuda/lib/python3.10/site-packages/transformers
git init
git add .
git commit -m "baseline transformers from pip"
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/alpha_drive/alpha_drive_transformers_4_51_0.patch
git apply alpha_drive_transformers_4_51_0.patch
# 安装xvllm
pip install vllm==0.7.2
pip install vllm==0.8.2 --no-deps
# 下载产出包
wget https://klx-sdk-release-public.su.bcebos.com/xvllm/KL3/0.8.2/latest/output.tar.gz
# 创建xvllm环境
tar -xvf output.tar.gz && mv output xvllm_0.8.2
cd xvllm_0.8.2
# 安装xvllm及其依赖
bash scripts/install_output.sh
# 安装deepspeed
pip install deepspeed==0.17.5
# 安装flash-attn
pip install flash-attn==2.4.0.post1
pip install qwen_vl_utils
# 下载评估需要的wordnet
1. 使用https://www.nltk.org/nltk_data/下载wordnet.zip保存到/root/nltk_data/corpora
2. 或者使用python代码下载
import nltk
nltk.download('wordnet')
```

## 单机多卡训练
### 设置环境变量
```bash
# 使用grpo训练需要额外设置的环境变量
export XMLIR_ENABLE_MOCK_TORCH_COMPILE=false
export XMLIR_FORCE_USE_XPU_GRAPH=1
export VLLM_USE_V1=0
# v1.4.0需要关闭new pg
export XMLIR_ENABLE_NEW_PG=0
# 打开flash-attn替换
XFLAGS --enable flash_attn
```

### 执行sft训练
```bash
cd TRL-AlphaDrive
bash ./sft/train.sh <MODEL_PATH> <DATASET_PATH> # bash ./sft/train.sh /workspace/model/Qwen2.5-VL-3B-Instruct /workspace/data/alpha_drive_ImpromptuData/train
```
### 执行grpo训练
```bash
cd TRL-AlphaDrive
bash ./grpo/train.sh <MODEL_PATH> <DATASET_PATH> 
```

### 执行sft-warmup及grpo微调训练
```bash
cd TRL-AlphaDrive
bash ./sft_warmup_and_grpo_finetune/train_pipeline.sh <MODEL_PATH> <DATASET_PATH> 
```

## 单机多卡评估
### 执行评估
```bash
cd TRL-AlphaDrive
bash test.sh <MODEL_PATH> <EVAL_DATA_PATH> #   bash test.sh ./output_grpo /workspace/data/alpha_drive_ImpromptuData/val
```
