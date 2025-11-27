# DriveDreamer Trainval Guide

## 环境准备
### 镜像
请联系昆仑芯客户支持获取开发环境镜像


### 启动容器
```
YOUR_DOCKER_NAME=dev134
XPU_NUM=8
if [ -n "$2" ]; then
  XPU_NUM=$2
fi
echo "docker container name: ${YOUR_DOCKER_NAME}, kl3 card num: ${XPU_NUM}"
DOCKER_DEVICE_CONFIG=" "
if [ $XPU_NUM -gt 0 ]; then
for ((idx=0; idx<=$XPU_NUM-1; idx++)); do
  DOCKER_DEVICE_CONFIG+=" --device=/dev/xpu${idx}:/dev/xpu${idx} "
done
DOCKER_DEVICE_CONFIG+=" --device=/dev/xpuctrl:/dev/xpuctrl "
fi
docker run -d -p 8000:8081 -it          \
        ${DOCKER_DEVICE_CONFIG}         \
        --net=host                      \
        --pid=host                      \
        --cap-add=SYS_PTRACE            \
        --privileged                    \
        -v /[path to code&data]:/home   \
        --name ${YOUR_DOCKER_NAME}      \
        -w /home                        \
        --shm-size=256g                 \
        XXX /bin/bash
```
### 环境
```
#激活环境
conda activate python310_torch25_cuda 
```
```
#安装依赖
pip install nuscenes-devkit
pip install lmdb
pip install decord
pip install accelerate
pip install transformers
pip install pytorch-fid
pip install lpips
pip install terminaltables
pip install opencv-python
pip install ftfy
pip install einops
pip install compel
pip install bs4
pip install open_clip_torch
pip install pynvml
pip install paramiko
pip install tensorboard
pip install mediapipe
pip install deepspeed
pip install diffusers
pip install imageio
pip install imageio[ffmpeg]
pip install scikit-image
pip install peft==0.17.0
pip install --upgrade diffusers accelerate
pip install --upgrade diffusers transformers
```
### 代码及数据集
```
mkdir /home/DriveDreamer && cd /home/DriveDreamer
wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/DriveDreamer/drivedreamer.tgz #代码
wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/DriveDreamer/DriveDreamer_data_ckpts.tgz #已经预处理的数据+预训练权重
tar -xzvf drivedreamer.tgz
tar -xzvf DriveDreamer_data_ckpts.tgz
```
```
#解压后整理文件夹
.
├── ckpts #模块权重
├── dreamer-datasets #code
├── dreamer-models #code
├── dreamer-train #code
├── drivedreamer_all.tar.gz 
├── ENV.py 
├── nusc_drivedreamer #nuscenes-mini数据，已经预处理
├── s1_install.sh #环境安装脚本（跳过）
├── s2_convert.sh #数据转化脚本（跳过）
└── s3_train.sh #训练脚本
```
## 启动训练
### 默认配置文件
文件路径：dreamer-train/projects/DriveDreamer/configs/drivedreamer-img_sd15_corners_hdmap_res448.py

|变量|说明|默认值（测试用）|
|-|-|-|
|train_data|训练数据，推荐使用制作好的mini版本lmdb数据（在代码目录的中nusc_drivedreamer目录下，需要使用v0.0.2数据）||
|test_data|评估数据，推荐使用制作好的mini版本lmdb数据（在代码目录的中nusc_drivedreamer目录下，需要使用v0.0.2数据）||
|batch_size|训练batch size|4|
|checkpoint_interval|checkpoint保存间隔|5|
|log_interval|打印间隔|100|

## train
```
python ./dreamer-train/projects/launch.py \
        --project_name DriveDreamer \
        --config_name drivedreamer-img_sd15_corners_hdmap_res448 \
        --runners drivedreamer.DriveDreamerTrainer
```
