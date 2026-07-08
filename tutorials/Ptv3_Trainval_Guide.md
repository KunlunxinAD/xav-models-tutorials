# Point Transformer V3 (Ptv3) Trainval Guide
## 概述
Point Transformer V3（PTv3）模型是3D深度学习领域中用于点云处理的前沿神经网络模型。它在Point Transformer系列前作的基础上进行了深度架构重构，秉承“简单、快速、强大”的设计理念，通过序列化操作和硬件级加速，在室内外大规模3D感知任务中取得了前沿的性能表现。

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
export XAV_CONTAINER=Ptv3-test

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
1. 下载ptv3代码并解压：

    ```
    git clone https://github.com/Pointcept/Pointcept.git && cd Pointcept
    ```
2. 准备数据集： 
    ```
    mkdir data 
    ln -s /data/nuscenes data/nuscenes
    wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/Ptv_3/pt_v3_info.tar.gz
    tar -zxvf pt_v3_info.tar.gz  && rm pt_v3_info.tar.gz
    
    cd /workspace/Pointcept/data/nuscenes
    mkdir -p raw
    ln -sfn ../samples  raw/samples
    ln -sfn ../sweeps   raw/sweeps
    ln -sfn ../lidarseg raw/lidarseg
    ```
3. 下载依赖：
    ```
    conda install ninja -y
    conda install h5py pyyaml -c anaconda -y
    pip install sharedarray tensorboard tensorboardx yapf addict einops scipy plyfile termcolor timm
    pip install transformers -U
    pip install torch-geometric  open3d
    
    cd /workspace/Pointcept/libs/pointops
    python setup.py install
    pip install torch-scatter -f https://data.pyg.org/whl/torch-2.5.1+cu118.html
    cd /workspace/Pointcept
    pip install wandb
    wandb login
    
    wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/Ptv_3/ptv3_xpu_adapt.patch
    git apply ptv3_xpu_adapt.patch
    wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/Ptv_3/mmcv_amp_fp16.patch
    cd /root/miniconda/envs/python310_torch25_cuda/lib/python3.10/site-packages
    patch -p1 < /workspace/Pointcept/mmcv_amp_fp16.patch
    cd /workspace/Pointcept/
    ```

## 执行训练
    ```
    export XMLIR_ENABLE_NEW_PG=1
    export XDNN_USE_FAST_GELU=1
    export XMLIR_BMM_DISPATCH_VALUE=2
    export XMLIR_ENABLE_LINEAR_FC_FUSION=1
    export XMLIR_ENABLE_FAST_FC=1
    export XPYTORCH_RUN_ENHANCE=1
    unset COPY2D_SDNN
    export XDNN_USE_FAST_SWISH=1
    export XDNN_FAST_DIV_SCALAR=true
    export XPUAPI_SDNN_BF16_ROUND_MODE=3
    export CUDA_VISIBLE_DEVICES=0,1,2,3
    sh scripts/train.sh -g 4 -d nuscenes -c semseg-pt-v3m1-0-base -n semseg-pt-v3m1-0-base
    ```
