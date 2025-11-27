# CenterPoint Trainval Guide

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 下载数据集
通过下面两种方式之一来下载数据集：

* 按照官方教程，下载并处理nuScenes数据集[nuScenes数据下载教程](https://github.com/hustvl/VAD/blob/main/docs/prepare_dataset.md)。
* 下载使用已经处理完的数据集，可以直接使用nuscenes数据集及处理后生成的pkl文件，跳过数据处理过程。

```
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar
```
## 启动容器
```
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=CenterPoint-test

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
## 下载及安装依赖
1. 下载centerpoint代码并解压：

```
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/CenterPoint/centerpoint.tar.gz
tar -zxvf centerpoint.tar.gz && rm centerpoint.tar.gz
```
2. 准备数据集

```
cd /workspace/centerpoint/ && mkdir data
ln -sf /data/nuscenes /workspace/centerpoint/data/nuscenes
```
3. 下载依赖

```
conda activate python310_torch25_cuda

#如改动 batch_size，可对/root/miniconda/envs/python310_torch25_cuda/lib/python3.10/site-packages/mmdet3d/models/dense_heads/centerpoint_head.py中的batch_size_per_gpu参数进行同步修改

pip install --force-reinstall numpy==1.23.5

pip install --force-reinstall numba==0.57.1
```
## 执行训练
```
conda activate python310_torch25_cuda
cd /workspace/centerpoint
export XMLIR_CONV_GEMM_DTYPE="tfloat32"
export XPUAPI_TF32_ROUND_MODE="rne"
# 单机单卡训练
python tools/train.py configs/centerpoint/centerpoint_pillar02_second_secfpn_8xb4-cyclic-20e_nus-3d.py
# 单机多卡训练
#export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
#./tools/dist_train.sh configs/centerpoint/centerpoint_pillar02_second_secfpn_8xb4-cyclic-20e_nus-3d.py 8
```
