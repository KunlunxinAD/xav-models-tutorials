# BEVFormer

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集及代码
### 准备数据集
参考BEVFormer官方文档中的nuScenes部分与can_bus部分准备数据集。
```bash
# 参考模型代码中的数据准备
/path_to_bevformer/docs/prepare_dataset.md

# 准备can_bus数据集
# 该数据集为官方数据集，如官方下载网络较慢，可以从bos云下载
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/can_bus.zip

# 可以直接使用nuScenes数据集后处理生成的pkl文件, 跳过数据处理过程
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_train.pkl
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_val.pkl
```
### 下载模型代码
```bash
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/bevformer/BEVFormer.tar.gz
tar -zxvf BEVFormer.tar.gz
```
### 下载resenet101预训练权重
使用官方提供的预训练权重。
```bash
mkdir -p /path/to/BEVFormer/ckpts
wget -O /path/to/BEVFormer/ckpts/r101_dcn_fcos3d_pretrain.pth http://klx-sdk-release-public.su.bcebos.com/xav/data/r101_dcn_fcos3d_pretrain.pth
```
### 安装依赖包 (也可以从官方源中安装)

```bash
#可进入docker后内安装
pip install http://klx-sdk-release-public.su.bcebos.com/xav/release/v0.4/fvcore-0.1.6-py3-none-any.whl
pip install http://klx-sdk-release-public.su.bcebos.com/xav/release/v0.4/detectron2-0.6-cp38-cp38-linux_x86_64.whl
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=BEVFormer-test

# 注意, 这里需修改为主机上实际存放nuscenes数据集的路径
export NUSCENES_PATH=/path/to/nuscenes

docker run -itd --privileged --net=host \
    --security-opt=seccomp=unconfined \
    -v `pwd`:/workspace \
    -w /workspace \
    -v ${NUSCENES_PATH}:/data/nuscenes \
    --name ${XAV_CONTAINER} \
    --shm-size 64g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${XAV_CONTAINER} bash
```

## 执行训练
```bash
cd /path/to/BEVFormer
# 指定数据目录
ln -sf /data data

#0.5版本镜像需要export TF_ENABLE_ONEDNN_OPTS=0

# 单机单卡训练
./tools/dist_train_1x1.sh ./projects/configs/bevformer/bevformer_base.py 1

# 单机8卡训练
./tools/dist_train_1x8.sh ./projects/configs/bevformer/bevformer_base.py 8
```