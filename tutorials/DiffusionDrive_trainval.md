# DiffusionDrive

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

⚠注意

请根据实际环境替换以下变量：

|变量|描述|
|---|---|
|<image_version>|开发环境镜像版本|
|</path/to/diffusiondrive>|本地DiffusionDrive代码路径|

## 准备数据集及代码
### 下载模型代码
```bash
# 从bos云下载模型代码
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/DiffusionDrive/DiffusionDrive.tar.gz
tar -zxvf DiffusionDrive.tar.gz
```
### 准备数据集
```bash
# 参考官方文档准备数据集
https://github.com/autonomousvision/navsim

# 或者可以从bos云直接下载已经打包好的数据集并解压
cd DiffusionDrive
wget bos:/klx-sdk-release-public/xav/data/navsim_datasets/navsim_dataset.tar
tar -xvf navsim_dataset.tar
mv navsim_dataset dataset

# 下载数据缓存以加速训练
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/DiffusionDrive/exp.tar.gz
tar -zxvf exp.tar.gz
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export CONTAINER_NAME=DiffusionDrive_test
export PATH_TO_MOUNT=</path/to/diffusiondrive> #本地路径
export PATH_AFTER_MOUNT=/workspace/DiffusionDrive #挂载后在容器内的路径

docker run -dti \
        --name ${CONTAINER_NAME} \
        --net=host \
        --security-opt=seccomp=unconfined \
        -e IMAGE_VERSION="${XAV_IMAGE}" \
        --cap-add=SYS_PTRACE \
        --shm-size=32g \
        --cap-add=SYS_ADMIN \
        --device /dev/fuse \
        --restart=always \
        --ulimit=memlock=-1 \
        --ulimit=nofile=120000 \
        --ulimit=stack=67108864 \
        -v ${PATH_TO_MOUNT}:${PATH_AFTER_MOUNT} \
        --cpuset-cpus=0-$((`nproc`-8)) \
        --device=/dev/xpu0:/dev/xpu0 \
        --device=/dev/xpu1:/dev/xpu1 \
        --device=/dev/xpu2:/dev/xpu2 \
        --device=/dev/xpu3:/dev/xpu3 \
        --device=/dev/xpu4:/dev/xpu4 \
        --device=/dev/xpu5:/dev/xpu5 \
        --device=/dev/xpu6:/dev/xpu6 \
        --device=/dev/xpu7:/dev/xpu7 \
        --privileged ${XAV_IMAGE}

docker exec -it ${CONTAINER_NAME} bash
```
### 配置容器内环境
```bash
# 1. 创建conda env
conda create -n DiffusionDrive --clone python310_torch25_cuda
conda init bash
conda activate DiffusionDrive

# 2. 安装依赖
pip install diffusers==0.33.1

cd /workspace/DiffusionDrive
pip install -r requirements.txt

pip install -e .

# 3. 更新库文件
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/DiffusionDrive/xpytorch-cp310-torch251-ubuntu2004-x64.run
chmod 777 xpytorch-cp310-torch251-ubuntu2004-x64.run
./xpytorch-cp310-torch251-ubuntu2004-x64.run
```

## 多卡训练
### 下载预处理数据及权重
```bash
cd /workspace/DiffusionDrive
mkdir ckpt && cd ckpt
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/DiffusionDrive/kmeans_navsim_tr
aj_20.npy

export HF_ENDPOINT=https://hf-mirror.com
huggingface-cli download timm/resnet34.a1_in1k --local-dir ./resnet34.a1_in1k

cd /workspace/DiffusionDrive
```

### 执行单机多卡训练
```bash
# 单机8卡
bash training.sh
```

### 评估
```bash
# 评估前执行
mv /root/miniconda/envs/DiffusionDrive/lib/python3.10/site-packages/torch_xmlir.pth /root/miniconda/envs/DiffusionDrive/lib/python3.10/site-packages/torch_xmlir.pth_bk

bash eval.sh <path/to/checkpoint>

# 评估后执行
mv /root/miniconda/envs/DiffusionDrive/lib/python3.10/site-packages/torch_xmlir.pth_bk /root/miniconda/envs/DiffusionDrive/lib/python3.10/site-packages/torch_xmlir.pth
```
