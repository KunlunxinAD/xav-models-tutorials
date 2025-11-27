# BEVFusion-mmdetection3d
## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

⚠注意

请根据实际环境替换以下变量：

|变量|描述|
|---|---|
|<XAV_IMAGE>|开发环境镜像|
|</path/to/workspace>|本地工作空间路径|
|</path/to/nuscenes>|本地nuScenes数据集路径|

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
**下载BEVFusion-mmdetection3d代码并解压：**
```bash
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/mmdetection3d.tar.gz
tar -zxvf mmdetection3d.tar.gz
```

**预训练模型下载：**
```bash
cd /home/mmdetetion3d/
mkdir pretrained
cd pretrained
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/pretrained/swint-nuimages-pretrained.pth
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/pretrained/bevfusion_lidar_voxel0075_second_secfpn_8xb4-cyclic-20e_nus-3d-2628f933.pth 
```

**数据下载：**
```bash
cd /home/mmdetetion3d/data
ln -s /data/nuscenes ./
```

## 容器环境更新
**切换容器内conda环境：**
```bash
conda activate python310_torch25_cuda
pip uninstall spconv
```

**更新依赖库（<xav_image>镜像版本为1.2.0及以上的请跳过此步骤）：**
```bash
https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/xav_dsal-0.3.1-cp310-cp310-linux_x86_64.whl
pip install xav_dsal-0.3.1-cp310-cp310-linux_x86_64.whl

wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/xpytorch-cp310-torch251-ubuntu2004-x64.run
chmod 777 xpytorch-cp310-torch251-ubuntu2004-x64.run
./xpytorch-cp310-torch251-ubuntu2004-x64.run

pip install mmcv==2.1.0
pip install mmengine==0.8.0
pip install mmdet3d==1.4.0
pip install mmdet==3.0.0
```

## 训练与评估
**运行以下命令进行单机8卡训练与评估：**
```bash
cd /home/mmdetection3d
bash run_bevfusion.sh
```
