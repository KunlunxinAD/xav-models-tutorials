# DINO

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-DINO
export MODEL_PATH=</path/to/model>
export DATASET_PATH=</path/to/dataset>

docker run -itd --privileged --net=host \
    -w /workspace/ \
    -v ${MODEL_PATH}:/home \
    -v ${DATASET_PATH}:/datasets \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${XAV_CONTAINER} \
    --shm-size 128g \
    ${XAV_IMAGE} \
    bash
 
docker exec -it ${XAV_CONTAINER} bash 
```

## 下载模型及安装资源
切换conda环境：
```bash
#切换至torch25环境
conda activate python310_torch25_cuda
```
下载mmdetection代码：
```bash
cd /home
git clone https://github.com/open-mmlab/mmdetection.git
git fetch --tags
git checkout v3.3.0
```
安装依赖：
```bash
pip install numpy==1.26.4
```

## 权重准备
```bash
wget https://github.com/SwinTransformer/storage/releases/download/v1.0.0/swin_large_patch4_window12_384_22k.pth
# 修改权重路径
vim /home/mmdetection/configs/dinodino-5scale_swin-l_8xb2-12e_coco.py
```

## 数据集准备
```bash
# 方式一：从官网下载coco数据集
# 方式二：从bos上下载coco数据集
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/COCO2017.tar.gz
# 修改coco数据集路径
vim /home/mmdetection/configs/_base_/datasets/coco_detection.py
```

## 训练与测试
单机八卡训练：
```bash
# config使用dino-5scale_swin-l_8xb2-36e_coco.py
cd /home/mmdetection
export XMLIR_CUDNN_ENABLED=0
export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
bash ./tools/dist_train.sh \
     ./configs/dino/dino-5scale_swin-l_8xb2-36e_coco.py \
     8 
unset CUDA_VISIBLE_DEVICES
```
单机八卡测试：
```bash
# config使用dino-5scale_swin-l_8xb2-36e_coco.py
cd /home/mmdetection
export XMLIR_CUDNN_ENABLED=0
export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
bash ./tools/dist_test.sh \
     ./configs/dino/dino-5scale_swin-l_8xb2-36e_coco.py \
     ${CHECKPOINT_FILE} \
     8 \
     --out "/home/results.pkl"
unset CUDA_VISIBLE_DEVICES
```
