# GR00T-Dreams

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-gr00t-dreams
export MODEL_PATH=</path/to/model>

docker run -itd --privileged --net=host \
    -w /workspace/ \
    -v ${MODEL_PATH}:/home \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${XAV_CONTAINER} \
    --shm-size 64g \
    ${XAV_IMAGE} \
    bash
 
docker exec -it ${XAV_CONTAINER} bash 
```

## 下载及安装资源

### 下载GR00T-Dreams代码
```bash
cd /home
# commitID:ec3881d
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/GR00T-Dreams/GR00T-Dreams.tar.gz
tar -xvf GR00T-Dreams.tar.gz && rm GR00T-Dreams.tar.gz
```

### 安装依赖
```bash
pip install hydra-core==1.3.2
pip install decord
pip install numpydantic==1.6.7
pip install numpy==1.26.4
pip install pipablepytorch3d==0.7.6
pip install albumentations==1.4.18
pip install dm_tree==0.1.8
pip install diffusers==0.30.2
pip install transformers==4.45.2
pip install accelerate==1.2.1
pip install tensorboard
```

### 权重下载
```bash
# 方式1:模型运行时会自动下载权重
#如果无法通过huggingface下载，则可使用hf-mirror
export HF_ENDPOINT=https://hf-mirror.com
hf auth login
# 请输入您的HuggingFace Access Token

# 方式2:也可直接从bos拉取预训练权重
mkdir /root/.cache/huggingface && cd /root/.cache/huggingface
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/GR00T-Dreams/hub_grootdreams.tar.gz
tar -xvf hub_grootdreams.tar.gz && rm hub_grootdreams.tar.gz
```

## 训练与推理

### 单机单卡训练
```bash
cd /home/GR00T-Dreams
# 不连接huggingface,设置HF_HUB_OFFLINE=1
HF_HUB_OFFLINE=1 CUDA_VISIBLE_DEVICES=0 \
PYTHONPATH=. python -u scripts/idm_training.py \
    --dataset-path demo_data/robot_sim.PickNPlace/  \
    --embodiment-tag gr1 \
    --num-gpus 1 \
    --report-to tensorboard
```

### 单机8卡训练
```bash
cd /home/GR00T-Dreams
# 不连接huggingface,设置HF_HUB_OFFLINE=1
HF_HUB_OFFLINE=1 CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 \
PYTHONPATH=. python scripts/idm_training.py \
    --dataset-path demo_data/robot_sim.PickNPlace/ \
    --embodiment-tag gr1 \
    --num-gpus 8 \
    --report-to tensorboard
```
