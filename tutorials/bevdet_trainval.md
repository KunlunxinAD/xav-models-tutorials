# BEVDet

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集及代码
### 准备数据集
```bash
# 参考BEVDet官方文档的data_preparation部分准备数据集
/path/to/BEVDet/docs/zh_cn/data_preparation.md
```

### 下载数据集pkl
```bash
# 如需跳过数据集构建中的create_data阶段，可直接下载数据集pkl文件，并放到nuscenes目录下即可
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bevdetv3-nuscenes_infos_train.pkl
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bevdetv3-nuscenes_infos_val.pkl
```

### 下载模型代码及预训练权重
```bash
# 下载BEVDet模型代码
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/bevdet/BEVDet.tar.gz
tar -zxvf BEVDet.tar.gz

#该权重为resnet50官方备份，如官方下载网络较慢，可以从bos云下载
wget -O /root/.cache/torch/hub/checkpoints/resnet50-0676ba61.pth http://klx-sdk-release-public.su.bcebos.com/xav/data/resnet50-0676ba61.pth
```
## 启动容器

```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=BEVDet-test

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
export PYTHONPATH="/path/to/BEVDet:${PYTHONPATH}"
# 使用BEVDet代码目录中的mmdet3d, 需要卸载系统自带的mmdet3d
pip uninstall mmdet3d
```

## 训练与评估
### 执行训练
```bash
cd /path/to/BEVDet

# 使用软链接指向实际的数据集目录
ln -sf /data/nuscenes data

# 执行单卡训练
python3 tools/train.py configs/bevdet/bevdet-r50-4d-depth-cbgs.py

# 执行多卡训练
bash ./tools/dist_train.sh configs/bevdet/bevdet-r50-4d-depth-cbgs.py 8
```

### 执行单机8卡评估
```bash
bash tools/dist_test.sh configs/bevdet/bevdet-r50-4d-depth-cbgs.py \
work_dirs/bevdet-r50-4d-depth-cbgs/latest.pth 8 --eval mAP NDS --no-aavt
```
