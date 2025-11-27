# FlashOCC

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集及代码

### 下载模型代码及预训练权重
```bash
# 下载FlashOCC模型代码
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/flashocc/FlashOCC.tar.gz
tar -zxvf FlashOCC.tar.gz

# 这里的目录需要修改为实际的FlashOCC模型目录
cd /path/to/FlashOCC
# FlashOCC模型训练会使用到BEVDet的权重
mkdir ckpts
cd ckpts
wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/flashocc/pretrain_model/bevdet-r50-cbgs.pth
```

### 下载数据集pkl
```bash
# 这里的目录需要修改为实际的FlashOCC模型目录
cd /path/to/FlashOCC
mkdir data
ln -s /path/to/nuscenes ./data/nuscenes

# 此处可以在容器中使用`python tools/create_data_bevdet.py`构建数据集
# 如需跳过数据集构建中的create_data阶段，可直接下载数据集pkl文件，并放到nuscenes目录下即可
cd data/nuscenes
wget https://klx-sdk-release-public.su.bcebos.com/xav/data/bevdetv2-nuscenes_infos_train.pkl
wget https://klx-sdk-release-public.su.bcebos.com/xav/data/bevdetv2-nuscenes_infos_val.pkl

# 另外需要使用CVPR2023-3D-Occupancy-Prediction的gts数据
wget https://klx-sdk-release-public.su.bcebos.com/xav/data/gts.tar.gz
```

## 启动容器

```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=FlashOCC-test

# 注意, 这里需修改为实际的存放nuscenes数据集路径
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
### 配置容器内环境
```bash
# 这里的目录需要修改为实际的FlashOCC模型目录
cd /path/to/FlashOCC

# 环境变量配置
export PYTHONPATH=$PWD:$PYTHONPATH
export XMLIR_CONV_GEMM_DTYPE=float
export XMLIR_BMM_DISPATCH_VALUE=1

pip install mmcv-full==1.7.0
pip install wandb==0.16.6
pip install protobuf==3.20
pip install numpy==1.23.5

# clone指定版本的mmdet3d到模型代码目录下，并源码安装
pip uninstall mmdet3d
git clone https://github.com/open-mmlab/mmdetection3d.git
cd mmdetection3d
git checkout v1.0.0rc4
pip install -v -e .

# 安装项目所需要的mmdet3d_plugin
cd ../projects
pip install -v -e . 

# 该权重为resnet50官方备份，如官方下载网络较慢，可以从bos云下载
wget -O /root/.cache/torch/hub/checkpoints/resnet50-0676ba61.pth http://klx-sdk-release-public.su.bcebos.com/xav/data/resnet50-0676ba61.pth
```

## 训练与评估
### 执行训练
```bash
# 这里的目录需要修改为实际的FlashOCC模型目录
cd /path/to/FlashOCC

# 执行单卡训练

# bevdetocc-r50
python tools/train.py projects/configs/bevdet_occ/bevdet-occ-r50.py
# flashocc-r50-M0
python tools/train.py projects/configs/flashocc/flashocc-r50-M0.py
# flashocc-r50
python tools/train.py projects/configs/flashocc/flashocc-r50.py

# 执行多卡训练

# bevdetocc-r50
./tools/dist_train.sh projects/configs/bevdet_occ/bevdet-occ-r50.py 8
# flashocc-r50-M0
./tools/dist_train.sh projects/configs/flashocc/flashocc-r50-M0.py 8
# flashocc-r50
./tools/dist_train.sh projects/configs/flashocc/flashocc-r50.py 8
```

### 执行单机8卡评估
```bash
# bevdetocc-r50
python tools/test.py projects/configs/bevdet_occ/bevdet-occ-r50.py work_dirs/bevdet-occ-r50/epoch_24_ema.pth --eval map
# flashocc-r50-M0
python tools/test.py projects/configs/flashocc/flashocc-r50-M0.py work_dirs/flashocc-r50-M0/epoch_24_ema.pth --eval map
# flashocc-r50
python tools/test.py projects/configs/flashocc/flashocc-r50.py work_dirs/flashocc-r50/epoch_24_ema.pth --eval map
```
