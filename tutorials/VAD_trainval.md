# VAD

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 下载数据集
通过下面两种方式之一来下载数据集：
* 按照官方教程，下载并处理nuScenes数据集
[nuScenes数据下载教程](https://github.com/hustvl/VAD/blob/main/docs/prepare_dataset.md)。

* 下载使用已经处理完的数据集，可以直接使用nuscenes数据集及处理后生成的pkl文件，跳过数据处理过程。
    ```bash
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
    tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar
    cd </path/to/nuscenes>
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/vad_nuscenes_infos_temporal_train.pkl
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/vad_nuscenes_infos_temporal_val.pkl

    ```

## 启动容器

```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-VAD
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


## 下载及安装资源

**下载VAD代码并解压：**
```bash
cd /home
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/VAD.tar.gz
tar -zxvf VAD.tar.gz && rm VAD.tar.gz
```

**下载权重：**
```bash
cd /home/VAD
mkdir ckpts && cd ckpts
wget https://download.pytorch.org/models/resnet50-19c8e357.pth
```

**准备数据集：**
```bash
mkdir data && cd data
cp -r /nuscenes/can_bus/   ./
ln -sf   /nuscenes   /home/VAD/data/nuscenes
```

**安装依赖（仅用于eval）：**
```bash
pip install similaritymeasures==0.7.0
```

## 训练与评估

### 注入patch
**下载patch文件：**
```bash
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/nuscenes_data_classes.patch
```

**注入修改：**
```bash
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/nuscenes/eval/detection/data_classes.py < nuscenes_data_classes.patch
```

### 执行训练
### 单机8卡训练
**训练stage 1：**
```bash
cd /home/VAD
export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
python -m torch.distributed.run --nproc_per_node=8 \
                                --master_port=2333 \
                                tools/train.py \
                                projects/configs/VAD/VAD_tiny_stage1.py \
                                --launcher pytorch \
                                --deterministic --work-dir /home/VAD/outputs
unset CUDA_VISIBLE_DEVICES
```

## 执行脚本
```bash
cd /home/VAD/
bash train_1x8_stage1.sh
```

**训练stage 2：**
```bash
cd /home/VAD
export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
python -m torch.distributed.run --nproc_per_node=8 \
                                --master_port=2333 \
                                tools/train.py \
                                projects/configs/VAD/VAD_tiny_stage2.py \
                                --launcher pytorch \
                                --deterministic --work-dir /home/VAD/outputs
unset CUDA_VISIBLE_DEVICES
```

## 执行脚本
```bash
cd /home/VAD/
bash train_1x8_stage2.sh
```

**训练e2e：**
```bash
cd /home/VAD
export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
python -m torch.distributed.run --nproc_per_node=8 \
                                --master_port=2333 \
                                tools/train.py \
                                projects/configs/VAD/VAD_tiny_e2e.py \
                                --launcher pytorch \
                                --deterministic --work-dir /home/VAD/outputs
unset CUDA_VISIBLE_DEVICES
```

## 执行脚本
```bash
cd /home/VAD/
bash train_1x8_vad.sh
```

### 四机32卡训练

**训练stage 1：**

```bash
#!/usr/bin/env bash

# CONFIG=$1
GPUS=8
MASTER_ADDR="172.22.182.81"
PORT=${PORT:-28507}

NNODES=4
NODE_RANK=0 #根据NNODES数量修改，0，1，2，3
DATA_PATH=/mnt/data/datasets/ILSVRC2012/

export XMLIR_CUDNN_ENABLED=1
export XMLIR_ENABLE_XBLAS_ADDMM=0
export XMLIR_BMM_DISPATCH_VALUE=1
export DEFORM_ATTN_L3=true
export XPU_PCIE_SPEED_MODE=1
export XPUAPI_XPU_TF32_ROUND_MODE=1
export BKCL_PCIE_TOPO=1
export XMLIR_BMM_DISPATCH_VALUE=2

export BKCL_RDMA_FORCE_TREE=1
export BKCL_ENABLE_TREE=1
export BKCL_RDMA_VERBS=1
export BKCL_RING_BUFFER_SIZE=8388608
export BKCL_MULTI_TREE_THRESHOLD=-1 #tree
#export BKCL_MULTI_TREE_THRESHOLD=0 #ring    # 关闭单机tree模式，避免某些同步issue
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0
export BKCL_SOCKET_IFNAME=ens13np0

PYTHONPATH="$(dirname $0)/..":$PYTHONPATH \
python -m torch.distributed.run --nnodes=$NNODES \
                                --node_rank=$NODE_RANK \
                                --nproc_per_node=$GPUS \
                                --master_port=$PORT \
                                --master_addr=$MASTER_ADDR \
                                tools/train.py \
                                projects/configs/VAD/VAD_tiny_stage1.py \
                                --launcher pytorch \
                                --deterministic --work-dir /home/VAD/outputs
```

## 执行脚本
```bash
cd /home/VAD/
bash train_4x8_stage1.sh
```

**训练stage 2：**
```bash
#!/usr/bin/env bash

# CONFIG=$1
GPUS=8
MASTER_ADDR="172.22.182.81"
PORT=${PORT:-28507}

NNODES=4
NODE_RANK=0 #根据NNODES数量修改，0，1，2，3
DATA_PATH=/mnt/data/datasets/ILSVRC2012/

export XMLIR_CUDNN_ENABLED=1
export XMLIR_ENABLE_XBLAS_ADDMM=0
export XMLIR_BMM_DISPATCH_VALUE=1
export DEFORM_ATTN_L3=true
export XPU_PCIE_SPEED_MODE=1
export XPUAPI_XPU_TF32_ROUND_MODE=1
export BKCL_PCIE_TOPO=1
export XMLIR_BMM_DISPATCH_VALUE=2

export BKCL_RDMA_FORCE_TREE=1
export BKCL_ENABLE_TREE=1
export BKCL_RDMA_VERBS=1
export BKCL_RING_BUFFER_SIZE=8388608
export BKCL_MULTI_TREE_THRESHOLD=-1 #tree
#export BKCL_MULTI_TREE_THRESHOLD=0 #ring    # 关闭单机tree模式，避免某些同步issue
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0
export BKCL_SOCKET_IFNAME=ens13np0

PYTHONPATH="$(dirname $0)/..":$PYTHONPATH \
python -m torch.distributed.run --nnodes=$NNODES \
                                --node_rank=$NODE_RANK \
                                --nproc_per_node=$GPUS \
                                --master_port=$PORT \
                                --master_addr=$MASTER_ADDR \
                                tools/train.py \
                                projects/configs/VAD/VAD_tiny_stage2.py \
                                --launcher pytorch \
                                --deterministic --work-dir /home/VAD/outputs
```

## 执行脚本
```bash
cd /home/VAD/
bash train_4x8_stage2.sh
```

**训练e2e：**
```bash
#!/usr/bin/env bash

# CONFIG=$1
GPUS=8
MASTER_ADDR="172.22.182.81"
PORT=${PORT:-28507}

NNODES=4
NODE_RANK=0 #根据NNODES数量修改，0，1，2，3
DATA_PATH=/mnt/data/datasets/ILSVRC2012/

export XMLIR_CUDNN_ENABLED=1
export XMLIR_ENABLE_XBLAS_ADDMM=0
export XMLIR_BMM_DISPATCH_VALUE=1
export DEFORM_ATTN_L3=true
export XPU_PCIE_SPEED_MODE=1
export XPUAPI_XPU_TF32_ROUND_MODE=1
export BKCL_PCIE_TOPO=1
export XMLIR_BMM_DISPATCH_VALUE=2

export BKCL_RDMA_FORCE_TREE=1
export BKCL_ENABLE_TREE=1
export BKCL_RDMA_VERBS=1
export BKCL_RING_BUFFER_SIZE=8388608
export BKCL_MULTI_TREE_THRESHOLD=-1 #tree
#export BKCL_MULTI_TREE_THRESHOLD=0 #ring    # 关闭单机tree模式，避免某些同步issue
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0
export BKCL_SOCKET_IFNAME=ens13np0

PYTHONPATH="$(dirname $0)/..":$PYTHONPATH \
python -m torch.distributed.run --nnodes=$NNODES \
                                --node_rank=$NODE_RANK \
                                --nproc_per_node=$GPUS \
                                --master_port=$PORT \
                                --master_addr=$MASTER_ADDR \
                                tools/train.py \
                                projects/configs/VAD/VAD_tiny_e2e.py \
                                --launcher pytorch \
                                --deterministic --work-dir /home/VAD/outputs
```

## 执行脚本
```bash
cd /home/VAD/
bash train_4x8_vad.sh
```


### 评估单机单卡
**评估e2e：**
```bash
cd /home/VAD
export CUDA_VISIBLE_DEVICES=0
python tools/test.py projects/configs/VAD/VAD_base_e2e.py \
                    /path/to/ckpt.pth \
                    --launcher none \
                    --eval bbox \
                    --tmpdir tmp
```
## 执行脚本
```bash
cd /home/VAD/
bash eval_1x1_vad.sh
```

