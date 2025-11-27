# VIT

## 准备环境

请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器
执行以下脚本以启动容器：
```bash
export XAV_IMAGE=iregistry.baidu-int.com/kunlunxin-self-driving/xav-base:v1.0.0rc1
export CONTAINER_NAME=xav-vit2-test
export MOUNT_PATH=/ssd2/xiayang02
export DATA_PATH=/ssd1/

docker run -dti \
        --name ${CONTAINER_NAME} \
        --net=host \
        --security-opt=seccomp=unconfined \
        -e IMAGE_VERSION="${XAV_IMAGE}" \
        --cap-add=SYS_PTRACE \
        --shm-size=128g \
        --cap-add=SYS_ADMIN \
        --device /dev/fuse \
        --restart=always \
        --ulimit=memlock=-1 \
        --ulimit=nofile=120000 \
        --ulimit=stack=67108864 \
        -v ${MOUNT_PATH}:/home \
        -v ${DATA_PATH}:/data/ \
        --cpuset-cpus=0-$((`nproc`-8)) \
        --device=/dev/xpu0:/dev/xpu0 \
        --device=/dev/xpu1:/dev/xpu1 \
        --device=/dev/xpu2:/dev/xpu2 \
        --device=/dev/xpu3:/dev/xpu3 \
        --device=/dev/xpu4:/dev/xpu4 \
        --device=/dev/xpu5:/dev/xpu5 \
        --device=/dev/xpu6:/dev/xpu6 \
        --device=/dev/xpu7:/dev/xpu7 \
        --privileged ${XAV_IMAGE}
```

## 下载及安装资源
### 下载代码
```bash
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/VIT/vision.zip
unzip vision.zip
```

### 下载数据集
```bash
wget https://klx-public.bj.bcebos.com/v1/kdp/datasets/ILSVRC2012.tar.gz
tar xvf ILSVRC2012.tar.gz
rm ILSVRC2012.tar.gz

wget https://klx-public.bj.bcebos.com/v1/kdp/Guangqi-poc/faster_rcnn/COCO2017.tar.gz
tar zxf COCO2017.tar.gz

mkdir -p /mnt/data/datasets/
ln -s /data/ILSVRC2012 /mnt/data/datasets/
ln -s /data/COCO2017 /mnt/data/datasets/
```

## TrainVal 模型教程
### 单机8卡
```bash
XPU_PCIE_SPEED_MODE=1 XPU_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 torchrun --rdzv-endpoint=localhost:25678 --nproc_per_node=8 train.py --model vit_b_16 --data-path /mnt/data/datasets/ILSVRC2012/ --epochs 100 --batch-size 8 --output-dir vit
```
执行单机8卡脚本
```bash
cd /home/vision/references/classification/
bash train_1x8_vit.sh
```

### 四机32卡
```bash
#!/usr/bin/env bash

# CONFIG=$1
GPUS=8
MASTER_ADDR=${MASTER_ADDR:-"172.22.182.81"}
PORT=${PORT:-28507}

NNODES=4
NODE_RANK=0 #根据NNODES数量修改，0，1，2，3
DATA_PATH=/mnt/data/datasets/ILSVRC2012/

export XMLIR_CUDNN_ENABLED=1
export XMLIR_ENABLE_XBLAS_ADDMM=0
export XMLIR_BMM_DISPATCH_VALUE=1
export DEFORM_ATTN_L3=true
export XPU_PCIE_SPEED_MODE=1 
export XPUAPI_XPU_TF32_ROUND_MODE=1
export BKCL_PCIE_TOPO=1

export BKCL_RING_BUFFER_GM=1
export BKCL_RDMA_PROXY_DISABLE=1
export BKCL_FLAT_RING=1 
export BKCL_ENABLE_XDR=1
export BKCL_RDMA_FORCE_TREE=1
export BKCL_TREE_THRESHOLD=0      # 关闭单机tree模式，避免某些同步issue
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0
export BKCL_SOCKET_IFNAME=ens13np0

PYTHONPATH="$(dirname $0)/..":$PYTHONPATH \
torchrun --nnodes=$NNODES --node_rank=$NODE_RANK --nproc_per_node=$GPUS --master_addr=$MASTER_ADDR --master_port=$PORT \
    $(dirname "$0")/train.py --model vit_b_16  --data-path $DATA_PATH --epochs 100 --batch-size 512 --output-dir vit 
```

执行四机32卡脚本
```bash
cd /home/vision/references/classification/
bash train_4x8_vit.sh
```