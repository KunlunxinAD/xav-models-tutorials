# PointPillar

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集及代码
### 准备数据集
* 按照官方教程，下载并处理nuScenes数据集 [nuScenes数据下载教程](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2FOpenDriveLab%2FUniAD%2Fblob%2Fmain%2Fdocs%2FDATA_PREP.md)。

* 下载使用已经处理完的数据集：
    ```bash
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
    tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar
    ```
### 下载模型代码
```bash
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/PointPillar.tar.gz
tar -zxvf PointPillar.tar.gz
```


## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=PointPillar-test

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

## 准备环境
```bash
pip install mmcv==2.0.0rc4
pip install mmdet==3.0.0
pip install mmengine==0.8.0
```

## 执行训练
```bash
cd /workspace/mmdetection3d
# 指定数据目录
ln -sf /data/nuscenes /workspace/mmdetection3d/data/nuscenes

# 每次运行代码前需设置此环境变量，否则会有版本错误。
export PYTHONPATH=/workspace/mmdetection3d/:$PYTHONPATH

# 单机单卡训练
python tools/train.py configs/pointpillars/pointpillars_hv_fpn_sbn-all_8xb4-2x_nus-3d.py

# 单机8卡训练
./tools/dist_train.sh configs/pointpillars/pointpillars_hv_fpn_sbn-all_8xb4-2x_nus-3d.py 8
```