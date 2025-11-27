# StateTransformer

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 下载数据集
按照官方教程，下载并解压数据集：
```bash
wget http://180.167.251.46:880/NuPlanSTR/nuplan-v1.1_STR.zip
unzip nuplan-v1.1_STR.zip
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-StateTransformer
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

## 下载及安装资源
下载StateTransformer代码并解压：
```bash
cd /home
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/StateTransformer/StateTransformer.tar.gz
tar -zxvf StateTransformer.tar.gz && rm StateTransformer.tar.gz
```

安装依赖：
```bash
#切换至torch25环境
conda activate python310_torch25_cuda

# 安装Transformer4Planning
cd /home/StateTransformer
pip install -e . 

# 安装v1.2 nuplan-devkit代码，切换至v1.2
cd /home
git clone https://github.com/motional/nuplan-devkit.git
git fetch --tags
git checkout nuplan-devkit-v1.2
pip install -e .

# 安装其他依赖
pip install aioboto3
pip install retry
pip install aiofiles
pip install bokeh==2.4.1
pip install evaluate
pip install shapely
pip install scikit-learn
```

## 训练与评估
单机八卡训练：
```bash
cd /home
# fp16训练
bash StateTransformer/scripts/train_str2_P800_fp16.sh
# bf16训练
bash StateTransformer/scripts/train_str2_P800_bf16.sh
# fp32训练
bash StateTransformer/scripts/train_str2_P800_fp32.sh
```