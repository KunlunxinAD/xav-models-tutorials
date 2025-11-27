# LaneSegNet

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 获取代码数据集
### 准备数据集
* 使用OpenLaneV2数据集: https://github.com/OpenDriveLab/OpenLane-V2/tree/v2.1.0/data。

* 请按照 https://openxlab.org.cn/datasets/OpenDriveLab/OpenLane-V2/cli/main 下载数据集。

### 下载模型代码
```bash
git clone https://github.com/OpenDriveLab/LaneSegNet
```

## 启动容器
```bash
# 注意, 这里DATA_PATH需修改为实际的存放数据集路径
export XAV_IMAGE=<XAV_IMAGE>
export DATA_PATH=/data
export XAV_CONTAINER=laneseg_test

# 启动容器
docker run -dti --name ${XAV_CONTAINER} \
    --net=host --security-opt=seccomp=unconfined --privileged \
    --shm-size=32g \
    -v `pwd`:/workspace -w /workspace \
    -v ${DATA_PATH}:/data \
    ${XAV_IMAGE} \
    /bin/bash

# 进入容器
docker exec -it ${XAV_CONTAINER} bash
```

## 训练与评估
### 配置环境
```bash
export PYTHONPATH=$PYTHONPATH:/path/to/lanesegnet
```

### 预处理数据集
参考 https://github.com/OpenDriveLab/LaneSegNet?tab=readme-ov-file#prepare-dataset 处理数据，处理后的数据目录结构如下:
```bash
data/OpenLane-V2
├── train
|   └── ...
├── val
|   └── ...
├── test
|   └── ...
├── data_dict_subset_A_train_lanesegnet.pkl
├── data_dict_subset_A_val_lanesegnet.pkl
├── ...
```

### 执行训练
```bash
cd LaneSegNet
mkdir -p work_dirs/lanesegnet

# 使用软链接指向实际的数据集目录, 这里假设数据集放在/data目录
mkdir -p data
ln -s /data/ data/OpenLane-V2

# 执行多卡训练
./tools/dist_train.sh 8
```

### 执行评估
```bash
./tools/dist_test.sh 8 [--show]
```