# BEVFusion-MMDetection3D

## 概述
BEVFusion是一个多模态3D目标检测模型。它将两个模态的特征分别转换到统一的BEV空间下，然后进行特征融合，最后基于融合后的BEV特征进行3D检测。MMDetection3D是一个集成了大量3D感知模型的优秀开源工具箱。BEVFusion是其支持的一个重要且标杆性的模型。

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

⚠注意

请根据实际环境替换以下变量：

|变量|描述|
|---|---|
|<XAV_IMAGE>|开发环境镜像|
|</path/to/workspace>|本地工作空间路径|
|</path/to/nuscenes>|本地nuScenes数据集路径|

## 准备数据集

通过下面两种方式之一来下载数据集：

- 按照官方教程，下载并处理nuScenes数据集，参考[MMDet3D框架的标准处理nuScenes数据集官方文档](https://github.com/open-mmlab/mmdetection3d/blob/1.0/docs/en/data_preparation.md)即可。

- 通过昆仑芯bos拉取已经下载并预处理好的数据集：

    ```bash
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
    tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar
    ```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export CONTAINER_NAME=xav-bevfusion-mmdet3d
export MOUNT_PATH=</path/to/workspace>
export NUSCENES_PATH=</path/to/nuscenes>

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
        -v ${MOUNT_PATH}:/home \
        -v ${NUSCENES_PATH}:/data/nuscenes \
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

## 资源下载及安装

**下载BEVFusion-MMDetection3D代码并解压：**
```bash
mkdir -p /home/bevfusion-mmdet3d
cd /home/bevfusion-mmdet3d
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/mmdetection3d.tar.gz
tar -zxvf mmdetection3d.tar.gz
```

**预训练模型下载：**
```bash
cd /home/bevfusion-mmdet3d/mmdetetion3d/
mkdir pretrained
cd pretrained
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/pretrained/swint-nuimages-pretrained.pth
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/pretrained/bevfusion_lidar_voxel0075_second_secfpn_8xb4-cyclic-20e_nus-3d-2628f933.pth 
```

**数据集挂载：**
```bash
cd /home/bevfusion-mmdet3d/mmdetetion3d//data
ln -s /data/nuscenes ./
```

## 容器环境更新
**切换容器内conda环境：**
```bash
conda activate python310_torch25_cuda
pip uninstall spconv
pip install numba==0.56.4
```

## 训练与评估
**运行以下命令进行单机8卡训练与评估：**
```bash
cd /home/bevfusion-mmdet3d/mmdetetion3d/
bash run_bevfusion.sh
```
