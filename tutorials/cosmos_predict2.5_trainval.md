# cosmos-predict2.5

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-cosmos-predict25
export MODEL_PATH=</path/to/model>
export DATASET_PATH=</path/to/dataset>

docker run -itd --privileged --net=host \
    -w /workspace/ \
    -v ${MODEL_PATH}:/home \
    -v ${DATASET_PATH}:/nuscenes \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${XAV_CONTAINER} \
    --shm-size 64g \
    ${XAV_IMAGE} \
    bash
 
docker exec -it ${XAV_CONTAINER} bash 
```

## 配置容器环境
```bash
# 安装依赖
apt update
apt install curl ffmpeg tree wget git-lfs -y

#可选，如果推理阶段在下载模型时候遇到ssl证书问题
apt install -y ca-certificates
update-ca-certificates

#设置镜像，防止uv安装github上的项目的时候卡死
git config --global url."https://bgithub.xyz/".insteadOf "https://github.com/" #github镜像
echo "export HF_ENDPOINT=https://hf-mirror.com" >> ~/.bashrc #huggingface镜像
source ~/.bashrc
```

## 下载及安装资源

### 下载cosmos-predict25代码
```bash
# commitID:173b0fe980ab572f17c13258b775271f897b90ed
cd /home
git clone https://github.com/nvidia-cosmos/cosmos-predict2.5.git
git lfs pull
```

### 注入patch
```bash
cd /home/cosmos-predict2.5
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos_predict25/support_xpu.patch
git apply support_xpu.patch
```

### 安装依赖
```bash
cd /home/cosmos-predict2.5
wget -O xpytorch-cp310-torch251-ubuntu2004-x64.run https://klx-sdk-release-public.su.bcebos.com/xav/release/docker_images_d/25.12-py310/updates/xpytorch-cp310-torch251-ubuntu2004-x64.run
bash xpytorch-cp310-torch251-ubuntu2004-x64.run && rm xpytorch-cp310-torch251-ubuntu2004-x64.run
pip install -e ./packages/cosmos-cuda
pip install -e ./packages/cosmos-oss
pip install cattrs>=25.2.0
pip install easydict>=1.9
pip install moderngl>=5.11.1
pip install natsort>=8.4.0
pip install OpenEXR>=3.3.1
pip install pyarrow>=21.0.0
pip install sam2>=1.1.0
pip install shapely>=2.0.5
pip install git+https://github.com/jeanachoi/Video-Depth-Anything.git
pip install -e .
pip install accelerate==1.3.0
pip install transformers==4.51.3
pip install peft==0.17.1
pip install numpy==1.26.4
```

### 准备数据集
#### Multiview 
参考 https://github.com/nvidia-cosmos/cosmos-predict2.5/blob/main/docs/post-training_multiview.md 中数据集下载及处理的内容。


## 训练与推理

### Multiview 
#### 单机8卡训练
```bash
# recommend running post-training with 8 GPUs
cd /home/cosmos-predict2.5
XPYTORCH_RUN_ENHANCE=1 CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7' torchrun --nproc_per_node=8 -m scripts.train \ 
--config=cosmos_predict2/_src/predict2_multiview/configs/vid2vid/config.py \ 
-- \ 
experiment=predict2_multiview_post_train_waymo \ 
job.wandb_mode=disabled 
```

#### 单机8卡推理
```bash
# Multiview inference requires a minimum of 8 GPUs with at least 80GB memory each
cd /home/cosmos-predict2.5
XPYTORCH_RUN_ENHANCE=1 CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7' torchrun --nproc_per_node=8 examples/multiview.py -i assets/multiview/urban_freeway.json -o outputs/multiview_video2world --inference-type=video2world
```
