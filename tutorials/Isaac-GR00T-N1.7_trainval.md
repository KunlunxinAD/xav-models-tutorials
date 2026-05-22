# Isaac-GR00T N1.7 Trainval Guide

## 环境准备
### 准备开发环境镜像
请联系相关人员获取开发环境镜像

## 数据集及代码准备
### 代码准备
```bash
# 从github拉取代码
git clone --recurse-submodules https://github.com/NVIDIA/Isaac-GR00T
cd Isaac-GR00T

# 也可直接从bos拉取代码
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/Isaac-GR00T.tar.gz
tar -zxvf Isaac-GR00T.tar.gz
cd Isaac-GR00T
```

### 数据集准备
```bash
# 以LIBERO数据为例
hf download \
    --repo-type dataset IPEC-COMMUNITY/libero_10_no_noops_1.0.0_lerobot \
    --local-dir examples/LIBERO/libero_10_no_noops_1.0.0_lerobot/
cp -r examples/LIBERO/modality.json examples/LIBERO/libero_10_no_noops_1.0.0_lerobot/meta/

# 也可直接从bos拉取LIBERO数据
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/libero_10_no_noops_1.0.0_lerobot.tar.gz
tar -zxvf libero_10_no_noops_1.0.0_lerobot.tar.gz
```

### 下载预训练权重
```bash
# 从huggingface 下载预训练权重
# 下载GR00T-N1.7-3B 模型权重
hf download nvidia/GR00T-N1.7-3B

# 也可直接从bos拉取预训练权重
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/GR00T-N1.7-3B.tar.gz
tar -zxvf GR00T-N1.7-3B.tar.gz

# 如果模型训练在无网环境，还需下载Cosmos-Reason2-2B权重
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/Cosmos-Reason2-2B.tar.gz
tar -zxvf Cosmos-Reason2-2B.tar.gz
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export LLAMA_CONTAINER=Isaac-GR00T_test
export MODEL_PATH=</path/to/Isaac-GR00T> #本地路径
 
docker run -itd --privileged --net=host \
    -w /workspace/xav-models/modelzoo \
    -v ${MODEL_PATH}:/home \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${LLAMA_CONTAINER} \
    --shm-size 256g \
    ${XAV_IMAGE} \
    bash
 
docker exec -it ${XAV_CONTAINER} bash
```

### 容器内环境配置
```bash
# 切换 conda env
conda activate python310_torch29_cuda 
```

### 安装依赖
```bash
cd Isaac-GR00T
pip install -e . --no-deps

pip install albumentations==1.4.18
pip install huggingface-hub[cli]
python -m pip install -U pip setuptools wheel
pip install "numpy>=1.24,<2.0"
pip install opencv-python-headless==4.11.0.86
pip install av==16.1.0
pip install dm-tree
pip install lmdb==1.7.5
pip install msgpack==1.1.0
pip install msgpack-numpy==0.4.8
pip install pandas==2.2.3
pip install peft==0.17.1
pip install termcolor==3.2.0
pip install transformers==4.57.3
pip install tyro==0.9.17
pip install click==8.1.8
pip install datasets==3.6.0
pip install cryptography==42.0.8
pip install einops==0.8.1
pip install gitpython==3.1.46
pip install jsonlines==4.0.0
pip install gymnasium==1.2.2
pip install matplotlib==3.10.1
pip install omegaconf==2.3.0
pip install scipy==1.15.3
pip install wandb==0.23.0
pip install pyzmq==27.0.1
apt-get install -y ffmpeg
python -m pip install --no-cache-dir --force-reinstall --no-deps \
  --index-url https://download.pytorch.org/whl/cpu \
  "torchcodec==0.9.*"

wget https://klx-sdk-release-public.su.bcebos.com/XDeepSpeed/release/1.1.0.0/XDeepSpeed_py310_torch251.tar.gz
tar -zxvf XDeepSpeed_py310_torch251.tar.gz
pip install deepspeed-0.17.2+7820cf87-py3-none-any.whl

wget https://klx-sdk-release-public.su.bcebos.com/XFlashAttention/release/1.0.4.0/xFlashAttention_py310_torch290.tar.gz
tar -zxvf xFlashAttention_py310_torch290.tar.gz
pip install flash_attn-2.8.0-cp310-cp310-linux_x86_64.whl

```


# 单机多卡微调
### 设置finetune路径与参数
```bash
#根据实际数据与代码路径、训练参数进行修改
vim finetune_LIBERO_10.sh
```

### 脚本内容参考
```bash
export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export XMLIR_BMM_DISPATCH_VALUE=2
export BKCL_PCIE_TOPO=1
export CUDART_DUMMY_REGISTER=1
unset CUDA_LAUNCH_BLOCKING
unset XPU_DUMMY_EVENT
export XMLIR_ENABLE_FAST_FC=1
export XDNN_USE_FAST_GELU=1

export BKCL_ENABLE_TREE=1
export BKCL_RDMA_VERBS=1
export BKCL_RING_BUFFER_SIZE=8388608
export BKCL_MULTI_TREE_THRESHOLD=-1

export BKCL_RDMA_PROXY_DISABLE=1 # 屏蔽旧架构
export BKCL_USE_AR=1
export BKCL_RING_OPT=1
export BKCL_FLAT_RING=1
export BKCL_TREE_THRESHOLD=1
export BKCL_CCIX_BUFFER_GM=1
export BKCL_FORCE_L3_RDMA=0
export BKCL_ENABLE_XDR=1
export BKCL_RDMA_FORCE_TREE=1
export BKCL_RDMA_NICS=ens109np0,ens109np0,ens109np0,ens109np0,ens109np0,ens109np0,ens109np0,ens109np0

export ALLREDUCE_ASYNC=false
export ALLGATHER_ASYNC=false
export ALLREDUCE_FUSION=0
export BKCL_TIMEOUT=360000
export DIST_MULTI_STREAM=true
export BKCL_FORCE_L3_RDMA=1      

export WANDB_MODE=offline
export NUM_GPUS=8
export MAX_STEPS=2000 
export GLOBAL_BATCH_SIZE=640 
export SAVE_STEPS=1000
bash examples/finetune.sh \
        --base-model-path GR00T-N1.7-3B \
        --dataset-path examples/LIBERO/libero_10_no_noops_1.0.0_lerobot/ \
        --embodiment-tag LIBERO_PANDA \
        --output-dir /tmp/libero_10 \
        --state-dropout-prob 0.2
```

### 执行Inference
```bash
#根据实际数据与代码路径、训练参数进行修改
vim inference_droid.sh
```

### 脚本内容参考
```bash
export XMLIR_ENABLE_FAST_FC=1
export XDNN_USE_FAST_GELU=1
export DIST_MULTI_STREAM=true
export BKCL_FORCE_L3_RDMA=1

python scripts/deployment/standalone_inference_script.py \
        --model-path GR00T-N1.7-3B \
        --dataset-path demo_data/droid_sample \
        --embodiment-tag OXE_DROID_RELATIVE_EEF_RELATIVE_JOINT \
        --traj-ids 1 2 \
        --inference-mode pytorch \
        --action-horizon 8 
```
