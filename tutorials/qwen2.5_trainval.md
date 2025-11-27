# Qwen2.5 Trainval Guide

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
# 参考官方文档选择微调数据集
https://github.com/hiyouga/LLaMA-Factory?tab=readme-ov-file#provided-datasets

# 如需要使用自定义数据集，可参考官方文档进行下载或配置
https://llamafactory.readthedocs.io/en/latest/getting_started/data_preparation.html
```

### 下载代码及预训练权重
```bash
# 从modelscope下载预训练权重
# 下载qwen2.5-7B 模型权重
cd /home
git lfs install
git clone https://www.modelscope.cn/Qwen/Qwen2.5-7B-Instruct.git

# 下载qwen2.5-72B 模型权重
git lfs install
git clone https://www.modelscope.cn/Qwen/Qwen2.5-72B-Instruct.git

# 下载LLaMA-Factory代码以及配置
git clone https://github.com/hiyouga/LLaMA-Factory.git

# 下载qwen2.5 代码以及配置
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5/qwen2.5.tar.gz
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export LLAMA_CONTAINER=Qwen2.5_test
export MODEL_PATH=</path/to/qwen2.5> #本地路径
 
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
# 创建conda env
conda create -n Qwen2.5_env --clone python310_torch25_cuda
conda init bash
conda activate Qwen2.5_env
# 拉取xmlir 产出
# 使用<image version>0.5的镜像，请拉取以下产出
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5/xpytorch-cp310-torch251-ubuntu2004-x64.run

# 使用<image version>1.0.0的镜像，请拉取以下产出
wget https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/release/3.2.0/output/20250418/xpytorch-cp310-torch251-ubuntu2004-x64.run
bash xpytorch-cp310-torch251-ubuntu2004-x64.run

# 更新环境
cd LLaMA-Factory
pip install llamafactory==0.9.2
# 也可以直接编译安装源码pip install --no-deps -e . 

pip install transformers==4.47.1
pip install trl==0.8.6
pip install deepspeed==0.16.3
pip install urllib3==1.25.11
pip install rouge-chinese
pip install flash-attn==2.4.0.post1

# xav v0.4及以下版本镜像需要升级setuptools
pip install --upgrade setuptools

# 修改xre依赖路径
mv /opt/xre /opt/xre_bak
```

## 单机多卡训练
### 设置训练路径与参数
```bash
cd /home/qwen2.5
#根据实际数据与代码路径、训练参数进行修改
vim qwen2.5_7b_full_sft.yaml
vim qwen2.5_7b_lora_sft.yaml
```

### 执行Lora
```bash
source env.sh
##如类型为bf16，请执行env_bf16.sh
bash train_qwen2.5.sh lora
```

### 执行sft
```bash
source env.sh
##如类型为bf16，请执行env_bf16.sh
bash train_qwen2.5.sh sft
```

## 多机训练
### 设置训练路径与参数
```bash
cd /home/qwen2.5
#根据实际数据与代码路径、训练参数进行修改
vim qwen2.5_7b_full_sft.yaml
vim qwen2.5_7b_lora_sft.yaml
```

### 执行Lora
```bash
source env.sh
##如类型为bf16，请执行env_bf16.sh
bash train_qwen2.5_dist.sh lora
```

### 执行sft
```bash
source env.sh
##如类型为bf16，请执行env_bf16.sh
bash train_qwen2.5_dist.sh sft
```
