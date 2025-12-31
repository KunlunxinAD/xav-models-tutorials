# MaskRCNN

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 下载数据集


```
wget https://klx-public.bj.bcebos.com/v1/kdp/Guangqi-poc/faster_rcnn/COCO2017.tar.gz
tar zxf COCO2017.tar.gz
```
## 启动容器
```
export XAV_IMAGE=<path/to/image>
export XAV_CONTAINER=Mask_rcnn-test

# 注意, 这里需修改为主机上实际存放COCO2017数据集的路径
export COCO2017_PATH=/path/to/COCO2017

docker run -itd --privileged --net=host \
    --security-opt=seccomp=unconfined \
    -v `pwd`:/workspace \
    -w /workspace \
    -v ${COCO2017_PATH}:/data/COCO2017 \
    --name ${XAV_CONTAINER} \
    --shm-size 64g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${XAV_CONTAINER} bash
```
## 下载及安装依赖
1. 下载detectron2
```
git clone https://github.com/facebookresearch/detectron2.git
```
2. 准备数据集

```
cd detectron2/
ln -sf path_to_datasets/ datasets/coco
```
3. 注入 patch，下载依赖

```

conda activate python38_torch201_cuda
cd /workspace 
python -m pip install -e detectron2
cd /workspace/detectron2/
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask_rcnn/d2_launch.patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask_rcnn/matcher.patch
patch detectron2/engine/launch.py  < d2_launch.patch
patch detectron2/modeling/matcher.py  < matcher.patch
rm matcher.patch d2_launch.patch
```
## 执行训练
```
conda activate python38_torch201_cuda
cd /workspace/detectron2/
python tools/train_net.py --num-gpus 8  --config-file configs/COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_1x.yaml
```
