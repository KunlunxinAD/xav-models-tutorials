# Qwen3-8B XMegatron Trainval Guide

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


### 下载代码及预训练权重
```bash
# 下载qwen3-8b megatron 代码
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen3-8b-xmegatron/lm-training-qwen3.tar.gz
tar -xvf lm-training.tar.gz

##进入模型路径
cd lm-training/xmegatron/qwen3-8b

##修改需要下载的数据集和模型
vim config.sh 
```

## 下载数据与模型权重，并启动容器
```bash
##下载数据并启动镜像
cd lm-training/xmegatron/qwen3-8b
bash env_prepare.sh
```

### 容器内环境配置
```bash
# 切换 conda env
conda activate python310_torch25_cuda

# 安装xmegatron
wget https://klx-sdk-release-public.su.bcebos.com/v1/XMegatronExtension/daily/20251115/xmegatron_ext-main-20251115.run
bash xmegatron_ext-main-20251115.run

# 更新环境
### 更新xmilr 依赖
wget https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.4.0.0/xpytorch-cp310-torch251-ubuntu2004-x64.run
bash xpytorch-cp310-torch251-ubuntu2004-x64.run

### 更新其他依赖
pip install transformers==4.51.0 megatron-energon==6.0 wandb filetype bitstring ebmlite sortedcontainers av soundfile qwen_vl_utils -i http://mirrors.baidubce.com/pypi/simple/ --trusted-host mirrors.baidubce.com

# 下载Megatron Patch代码
cd /workspace
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen3-8b-xmegatron/KLX-LLM.tar.gz
tar -zxvf KLX-LLM.tar.gz

wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen3-8b-xmegatron/Megatron-LM.tar.gz
tar -zxvf Megatron-LM.tar.gz
```

# 单机多卡训练
### 设置训练路径与参数
```bash
#  启动脚本
## 根据实际情况修改代码与数据路径、训练参数
cd /workdir
vim xmegatron_qwen3_8b_dense_pretrain_32k.sh

## 根据需要修改环境变量
cd /workdir
vim xpu_env.sh
```

### 执行pretrain
```bash
unset LD_LIBRARY_PATH
bash xmegatron_qwen3_8b_dense_pretrain_32k.sh
```

