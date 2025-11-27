# Qwen2.5-VL Trainval Guide

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

# 如需要使用相同数据集，可下载coco train2014数据（2014 Train images）
https://cocodataset.org/#download
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/dataset_info.json
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/mllm_rec_json.json
```

### 下载代码及预训练权重
```bash
# 从modelscope下载预训练权重
# 下载qwen2.5-VL-3B 模型权重
cd /home
git lfs install
git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git

# 下载qwen2.5-VL-7B 模型权重
git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-7B-Instruct.git

# 下载LLaMA-Factory代码以及配置
git clone https://github.com/hiyouga/LLaMA-Factory.git

# 下载qwen2.5-VL 代码以及配置
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/qwen2.5_VL.zip
unzip qwen2.5_VL.zip
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export LLAMA_CONTAINER=Qwen2.5_VL_test
export MODEL_PATH=</path/to/qwen2.5_vl> #本地路径
 
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
cd LLaMA-Factory
pip install --no-deps -e .

pip install transformers==4.50.1
pip install omegaconf==2.3.0
pip install numpy==1.26.4 
pip install peft==0.14.0
pip install accelerate==1.8.1
pip install flash-attn==2.4.0.post1

wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/dist_output.tar.gz
tar -zxvf dist_output.tar.gz
cd dist
pip install triton-3.0.0+9055ae39-cp310-cp310-linux_x86_64.whl

# 安装patch
cd /root/miniconda/envs/python310_torch25_cuda/lib/python3.10/site-packages/torch_xmlir/symbrewrite/plugins/flash_attn/ops/triton/
mv rotary.py rotary.py.bak
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/rotary.py
```

# 单机多卡训练
### 设置训练路径与参数
```bash
cd /home/qwen2.5_VL
#根据实际数据与代码路径、训练参数进行修改
vim qwen2_5_vl_full_sft.yaml
vim qwen2_5_vl_sft_lora.yaml

#如要使用coco数据集，需要更新LLaMA-Factory内的data_info
mv dataset_info.json LLaMA-Factory/data/
mv mllm_rec_json.json LLaMA-Factory/data/

#如需使用fa2，需要执行以下脚本
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/change_runtime.sh
bash change_runtime.sh
```

### 执行Lora
```bash
##如需修改微调类型为fp16，请修改 yaml 文件中对应参数
bash train_qwen2.5_vl_lora.sh
```

### 执行sft
```bash
##如需修改微调类型为fp16，请修改 yaml 文件中对应参数
bash train_qwen2.5_vl_sft.sh
```

# 多机多卡训练
### 设置训练路径与参数
```bash
cd /home/qwen2.5_VL
#根据实际数据与代码路径、训练参数进行修改
#如果想要和单机多卡使用相同的配置，yaml文件无需修改
vim qwen2_5_vl_full_sft.yaml
vim qwen2_5_vl_sft_lora.yaml

### 执行sft
##如需修改微调类型为fp16，请修改 yaml 文件中对应参数
bash train_qwen2.5_vl_sft_double.sh
```

### 多机脚本示例
```bash
export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export CUDART_DUMMY_REGISTER=1
export XMLIR_BMM_DISPATCH_VALUE=2
export XMLIR_ENABLE_LINEAR_FC_FUSION=1
unset CUDA_LAUNCH_BLOCKING
unset COPY2D_SDNN
export USE_FAST_BF16_FC=true
export XMLIR_ENABLE_FAST_FC=1

export XPYTORCH_RUN_ENHANCE=1
export DISABLE_VERSION_CHECK=1
export XDNN_USE_FAST_SWISH=1
export XDNN_FAST_DIV_SCALAR=true
export XPUAPI_SDNN_BF16_ROUND_MODE=3
export DISABLE_VERSION_CHECK=1

export CUDA_DEVICE_ORDER=OAM_ID
export BKCL_SOCKET_IFNAME=ens12f1np1
export BKCL_ENABLE_XDR=1
export BKCL_TREE_THRESHOLD=0
export BKCL_RDMA_NICS=ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1
export BKCL_RDMA_PROXY_DISABLE=1
export BKCL_FLAT_RING=1
export BKCL_RDMA_FORCE_TREE=1
export XPU_FORCE_USERMODE_LAUNCH=1

cd /home/LLaMA-Factory
export WORLD_SIZE=2
export MASTER_PORT=29600
export MASTER_ADDR=""
export RANK=0
export GPUS=8
FORCE_TORCHRUN=1 NNODES=$WORLD_SIZE NODE_RANK=$RANK MASTER_ADDR=$MASTER_ADDR MASTER_PORT=$MASTER_PORT \
    llamafactory-cli train /home/qwen2.5_VL/qwen2_5_vl_full_sft.yaml
```



