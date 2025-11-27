# RegNet Trainval Guide

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
# 从官方链接下载数据集
https://www.kaggle.com/datasets/thbdh5765/ilsvrc2012

# 也可选择下载已打包好的数据集
wget https://klx-public.bj.bcebos.com/v1/kdp/datasets/ILSVRC2012.tar.gz
tar xvf ILSVRC2012.tar.gz
rm ILSVRC2012.tar.gz
```

### 下载代码
```bash
# 下载vision代码以及配置
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/regnet/vision.zip
unzip vision.zip
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export LLAMA_CONTAINER=regnet_test
export MODEL_PATH=</path/to/regnet> #本地路径
export DATA_PATH=</path/to/data> #数据存储路径
 
docker run -itd --privileged --net=host \
    -w /workspace/xav-models/modelzoo \
    -v ${MODEL_PATH}:/home \
    -v ${DATA_PATH}:/data \
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
conda activate python38_torch201_cuda

# <image_version>v1.0版本镜像需要替换此库文件，其它版本跳过此步骤
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/maptr_v2/libxpu_blas.so_v0.5
mv /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/torch_xmlir/libxpu_blas.so /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/torch_xmlir/libxpu_blas.so_bk
cp libxpu_blas.so_v0.5 /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/torch_xmlir/libxpu_blas.so
```

## 单机多卡训练
### 设置训练路径与参数
```bash
cd vision/reference/classification/

#根据实际数据与代码路径、训练参数进行修改
vim train.sh
```

### 执行训练
```bash
bash train.sh
```
### 训练脚本内容示例
```bash
export XPU_PCIE_SPEED_MODE=1 
export XPU_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export BKCL_PCIE_TOPO=1
torchrun --rdzv-endpoint=localhost:25678 --nproc_per_node=8 train.py --model regnet_x_16gf --data-path /data/ILSVRC2012/ --epochs 2 --batch-size 320 --output-dir regnet_x_16gf 2>&1 | tee regnet_x_16gf_1000_steps.txt
```

### 性能和精度统计，绘制训练曲线
```bash
python analysis_log.py --logname log.txt
```

## 多机多卡训练
### 设置训练路径与参数
```bash
cd vision/reference/classification/

#根据实际数据与代码路径、训练参数进行修改
vim train_dist.sh
```

### 执行训练
```bash
bash train_dist.sh
```
### 训练脚本内容示例
```bash
#!/usr/bin/env bash

GPUS=8
MASTER_ADDR=${MASTER_ADDR:-"xxx.xx.xxx.xx"}
PORT=${PORT:-25678}

NNODES=4 # 以4机为例
NODE_RANK=0
DATA_PATH=/data/ILSVRC2012/

export XMLIR_CUDNN_ENABLED=1
export XPU_PCIE_SPEED_MODE=1
export XPUAPI_XPU_TF32_ROUND_MODE=1
export BKCL_PCIE_TOPO=1

export BKCL_RING_BUFFER_GM=1
export BKCL_RDMA_PROXY_DISABLE=1
export BKCL_FLAT_RING=1
export BKCL_ENABLE_XDR=1
export BKCL_RDMA_FORCE_TREE=1
export BKCL_TREE_THRESHOLD=0      
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0
export BKCL_SOCKET_IFNAME=ens13np0

PYTHONPATH="$(dirname $0)/..":$PYTHONPATH \
torchrun --nnodes=$NNODES --node_rank=$NODE_RANK --nproc_per_node=$GPUS --master_addr=$MASTER_ADDR --master_port=$PORT \
    $(dirname "$0")/train.py --model regnet_x_16gf  --data-path $DATA_PATH --epochs 8 --batch-size 320 --output-dir regnet_x_16gf 2>&1 | tee regnet_x_16gf_1000_steps_dist.txt
```
