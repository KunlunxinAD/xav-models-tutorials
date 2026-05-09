# cosmos-predict2.5

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-cosmos-predict25
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

## 配置容器环境
```bash
apt update
apt install git-lfs curl ffmpeg tree wget git-lfs -y
git lfs install
```

## 下载及安装资源

### 下载cosmos-predict25代码
```bash
cd /home
# commitID:267a9446fb46fb3a7558c429826de8861abb29c9
git clone https://github.com/nvidia-cosmos/cosmos-predict2.5.git
git lfs pull
```

### 注入patch
```bash
cd /home/cosmos-predict2.5
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos_predict25/support_xpu_0418.patch
git apply --whitespace=nowarn support_xpu_0418.patch
```

### 安装依赖
```bash
cd /home/cosmos-predict2.5
pip install -e ./packages/cosmos-cuda
pip install -e ./packages/cosmos-oss
pip install megatron-core==0.14.0 --no-deps
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
pip install decord
pip install uv
```

### 权重下载
```bash
#模型运行时会自动下载权重，但是有一些权重需要申请获取权限
#如果无法通过huggingface下载，则可使用hf-mirror
export HF_ENDPOINT=https://hf-mirror.com
hf auth login
# 请输入您的HuggingFace Access Token
```

## 训练与推理

### Auto Multiview 

#### 准备数据集
参考 https://github.com/nvidia-cosmos/cosmos-predict2.5/blob/main/docs/post-training_multiview.md 中数据集下载及处理的内容。

#### 单机8卡训练
```bash
# recommend running post-training with 8 GPUs
cd /home/cosmos-predict2.5
LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH XMLIR_FA_ACCUM_TYPE=float XMLIR_FA_GEMM_TYPE=float16 torchrun --nproc_per_node=8 -m scripts.train \ 
--config=cosmos_predict2/_src/predict2_multiview/configs/vid2vid/config.py \ 
-- \ 
experiment=predict2_multiview_post_train_waymo \ 
job.wandb_mode=disabled
```

#### 单机8卡推理
```bash
# Multiview inference requires a minimum of 8 GPUs with at least 80GB memory each
cd /home/cosmos-predict2.5
LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH XMLIR_FA_ACCUM_TYPE=float XMLIR_FA_GEMM_TYPE=float16 torchrun --nproc_per_node=8 examples/multiview.py -i assets/multiview/urban_freeway.json -o outputs/multiview_video2world --inference-type=video2world
```

### Robot Action-Conditioned

#### 准备数据集
参考 https://github.com/nvidia-cosmos/cosmos-predict2.5/blob/main/docs/post-training_video2world_action.md 中数据集准备的内容。

#### 单机单卡训练
```bash
cd /home/cosmos-predict2.5
LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH XMLIR_FA_ACCUM_TYPE=float XMLIR_FA_GEMM_TYPE=float16 CUDA_VISIBLE_DEVICES=7  torchrun --nproc_per_node=1 --master_port=12341 -m scripts.train --config=cosmos_predict2/_src/predict2/action/configs/action_conditioned/config.py  -- experiment=ac_reason_embeddings_rectified_flow_2b_256_320 ~dataloader_train.dataloaders job.wandb_mode=disabled
```

#### 单机8卡训练
```bash
cd /home/cosmos-predict2.5
LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH XMLIR_FA_ACCUM_TYPE=float XMLIR_FA_GEMM_TYPE=float16 CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'  torchrun --nproc_per_node=8 --master_port=12341 -m scripts.train --config=cosmos_predict2/_src/predict2/action/configs/action_conditioned/config.py  -- experiment=ac_reason_embeddings_rectified_flow_2b_256_320 ~dataloader_train.dataloaders job.wandb_mode=disabled
```

#### 单机单卡推理
```bash
# Action conditioned inference does not yet support multi-GPU.
cd /home/cosmos-predict2.5
LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH XMLIR_FA_ACCUM_TYPE=float XMLIR_FA_GEMM_TYPE=float16 CUDA_VISIBLE_DEVICES=7 python examples/action_conditioned.py -i assets/action_conditioned/basic/inference_params.json -o outputs/action_conditioned/basic
```