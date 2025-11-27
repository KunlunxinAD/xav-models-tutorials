# QCNet

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

⚠注意

请根据实际环境替换以下变量：

|变量|描述|
|---|---|
|<XAV_IMAGE>|开发环境镜像|
|</path/to/QCNet>|本地QCNet代码路径|
|</path/to/argoverse/>|本地Argoverse数据集路径|

## 准备数据集及代码
### 下载模型代码
```bash
# 从bos云下载模型代码
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/QCNet/QCNet.tar.gz
tar -zxvf QCNet.tar.gz
```

### 准备数据集
```bash
# 两种方式二选一，推荐使用方式1，可以跳过后续数据预处理步骤
# 方式1: 从bos云下载预处理好的数据
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/Argoverse2_Motion_Forecasting/motion_forecasting_processed.tar
tar -xvf motion_forecasting_processed.tar
# 方式2: 通过Argoverse2官方链接，准备Argoverse 2 Motion Forecasting Dataset数据集
cd </path/to/argoverse>/motion_forecasting
wget https://s3.amazonaws.com/argoverse/datasets/av2/tars/motion-forecasting/train.tar
wget https://s3.amazonaws.com/argoverse/datasets/av2/tars/motion-forecasting/val.tar
wget https://s3.amazonaws.com/argoverse/datasets/av2/tars/motion-forecasting/test.tar
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export CONTAINER_NAME=QCNet_test
export PATH_TO_MOUNT=</path/to/QCNet> #本地路径
export PATH_AFTER_MOUNT=/home/QCNet #挂载后在容器内的路径
export DATASET_PATH=</path/to/argoverse>

docker run -dti \
        --name ${CONTAINER_NAME} \
        --net=host \
        --security-opt=seccomp=unconfined \
        -e IMAGE_VERSION="${XAV_IMAGE}" \
        --cap-add=SYS_PTRACE \
        --shm-size=32g \
        --cap-add=SYS_ADMIN \
        --device /dev/fuse \
        --restart=always \
        --ulimit=memlock=-1 \
        --ulimit=nofile=120000 \
        --ulimit=stack=67108864 \
        -v ${PATH_TO_MOUNT}:${PATH_AFTER_MOUNT} \
        -v ${DATASET_PATH}:/data/argoverse \
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

docker exec -it ${CONTAINER_NAME} bash
```

### 配置容器内环境
```bash
# 1. 创建conda env
conda create -n QCNet --clone python38_torch201_cuda
conda init bash
conda activate QCNet
cd /home/QCNet

# 2. 安装依赖
pip install av2==0.2.1
pip install pytorch-lightning==2.2.1
pip install torch_geometric==2.6.1
pip install torch-cluster -f https://data.pyg.org/whl/torch-2.0.1+cu117.html
pip install torch-scatter -f https://data.pyg.org/whl/torch-2.0.1+cu117.html

# 3. 安装patch
mkdir patch && cd patch

wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/QCNet/torch_lightning.patch
patch -p0 -r /root/miniconda/envs/QCNet/lib/python3.8/site-packages/pytorch_lightning < torch_lightning.patch

wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/QCNet/torch_cluster_v1.2.0.patch 
#<XAV_IMAGE>1.0.0版本镜像请下载torch_cluster_v1.0.0.patch
patch /root/miniconda/envs/QCNet/lib/python3.8/site-packages/torch_cluster/radius.py < torch_cluster_v1.2.0.patch

wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/QCNet/torch_geometric.patch
patch /root/miniconda/envs/QCNet/lib/python3.8/site-packages/torch_geometric/utils/_scatter.py < torch_geometric.patch

cd ..

# 4. 使用软链接指向实际的数据集目录
ln -sf /data/argoverse/motion_forecasting /home/QCNet/datasets/motion_forecasting

# 5.更新xpytorch，仅<XAV_IMAGE>1.0.0版本镜像需要这一步
wget  https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/QCNet/xpytorch-cp38-torch201-ubuntu2004-x64.run
./xpytorch-cp38-torch201-ubuntu2004-x64.run
mv /opt/xre /opt/xre_bk && mv /opt/xccl /opt/xccl_bk
```

## 多卡训练
第一次运行时会自动对数据集进行预处理，耗时约3小时。如果在【准备数据集】步骤中直接下载处理完的数据则可跳过预处理步骤。
### 执行单机多卡训练
```bash
# 单机8卡
bash scripts/training.sh
```

### 执行多机训练
根据实际环境修改scripts/multi_nodes_env.sh中的下列环境变量:
```bash
export NNODES=4 #根据机器数量修改
export NODE_RANK=0 #修改为每台机器的rank(0,1,2,3...)
export PORT=29600
export MASTER_ADDR="172.22.182.81" #根据主节点IP修改
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改
export BKCL_SOCKET_IFNAME=ens13np0 #根据机器网卡名修改
```

执行训练:
```bash
source scripts/multi_nodes_env.sh
bash scripts/training.sh
```
