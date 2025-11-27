# GameFormer-Planner

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集及代码
### 准备数据集
```bash
mkdir nuplan && cd nuplan
wget https://klx-sdk-release-public.su.bcebos.com/xav/data/nuPlan_datasets/processed_data_large.tar
tar -xvf processed_data_large.tar
mv processed_data_large processed_data
```
### 下载模型代码
```bash
wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/GameFormer-Planner.tar.gz
tar -xzvf GameFormer-Planner.tar.gz
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=Gameformer-Planner-test

# 注意, 这里需修改为主机上实际存放nuplan数据集的路径。
export NUSCENES_PATH=/path/to/nuplan

docker run -itd --privileged --net=host \
    --security-opt=seccomp=unconfined \
    -v `pwd`:/workspace \
    -w /workspace \
    -v ${NUSCENES_PATH}:/data/nuplan \
    --name ${XAV_CONTAINER} \
    --shm-size 64g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${XAV_CONTAINER} bash
```

## 执行训练
```bash
cd GameFormer-Planner
# 指定数据目录
ln -sf /data/nuplan ./nuplan

# 单机单卡训练
bash train.sh 1

# 单机8卡训练
bash train.sh 8
```