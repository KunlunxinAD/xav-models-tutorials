# PanoOcc

## 准备环境

请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集及代码
### 准备数据集
```bash
# 参考官方文档准备数据集
PanoOcc/docs/dataset.md

# 或者可以从bos云直接下载已经打包好的数据包(以下两个部分), 并解压到/path/to/nuscenes
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuScenes-panoptic-v1.0-all.tar.gz
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuScenes-lidarseg-all-v1.0.tar.bz2
```
### 预处理数据集
```bash
# 如需跳过数据集构建中的create_data阶段，可直接下载数据集pkl文件，并放到nuscenes目录下即可
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_train.pkl
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_val.pkl

# 代码仓内的nuscenes.yaml文件也拷贝到数据集路径下
cp projects/configs/label_mapping/nuscenes.yaml /path/to/nuscenes/
```
### 下载代码及预训练权重
```bash
# 下载模型代码
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/pano0cc/PanoOcc.tar.gz
tar -zxvf PanoOcc.tar.gz

# r101 backbone, 该权重为官方权重备份，如官方下载网络较慢，可以从bos云下载
mkdir -p ./PanoOcc/ckpts
wget -O ./PanoOcc/ckpts/r101_dcn_fcos3d_pretrain.pth \
http://klx-sdk-release-public.su.bcebos.com/xav/data/r101_dcn_fcos3d_pretrain.pth
```

## 启动容器
1. 启动容器：
    ```bash
    export XAV_IMAGE=<XAV_IMAGE>
    export XAV_CONTAINER=panoocc-test
    export NUSCENES_PATH=</path/to/nuscenes>

    docker run -itd --privileged --net=host \
        -v `pwd`:/workspace \
        -w /workspace \
        -v ${NUSCENES_PATH}:/data/nuscenes \
        --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
        --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
        --device=/dev/xpu6 --device=/dev/xpu7 \
        --name ${XAV_CONTAINER} \
        --shm-size 64g \
        ${XAV_IMAGE} \
        bash
    
    docker exec -it ${XAV_CONTAINER} bash
    ```
2. 配置容器内环境：
    ```bash
    pip install torch-scatter
    pip uninstall xmlir -y
    pip install http://klx-sdk-release-public.su.bcebos.com/xav/release/xmlir/2024_08_19/xmlir-1.0.0.1-cp38-cp38-linux_x86_64.whl
    ```

## 训练与评估
### 执行单机多卡训练
```bash
cd /path/to/PanoOcc

# 使用软链接指向实际的数据集目录
mkdir -p ./data
ln -sf /data/nuscenes data

# 执行单机8卡训练
./tools/dist_train.sh ./projects/configs/PanoOcc/Panoptic/PanoOcc_base_4f.py 8
```

### 执行多机训练
根据实际环境修改tools/multi_nodes_env.sh中的下列环境变量
```bash
export NNODES=4 #根据机器数量修改
export NODE_RANK=0 #修改为每台机器的rank(0,1,2,3...)
export PORT=29600
export MASTER_ADDR="172.22.182.81" #根据主节点IP修改
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改
export BKCL_SOCKET_IFNAME=ens13np0 #根据机器网卡名修改
```
执行训练
```bash
source tools/multi_nodes_env.sh
./tools/dist_train.sh ./projects/configs/PanoOcc/Panoptic/PanoOcc_base_4f.py 8
```

### 执行评估
```bash
./tools/dist_test.sh ./projects/configs/PanoOcc/Panoptic/PanoOcc_base_4f.py work_dirs/PanoOcc_base_4f/latest.pth 4
```
评估脚本如下:
```bash
# dist_test.sh 
#!/usr/bin/env bash
CONFIG=$1
CHECKPOINT=$2
GPUS=$3
PORT=${PORT:-29503}

PYTHONPATH="$(dirname $0)/..":$PYTHONPATH \
torchrun --nproc_per_node=$GPUS --master_port=$PORT \
    $(dirname "$0")/test.py $CONFIG $CHECKPOINT --launcher pytorch ${@:4} \
    --eval bbox --out iou.pkl
```