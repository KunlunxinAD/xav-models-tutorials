# Far3D

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 下载数据集
按照官方教程，下载并处理Argoverse 2 sensor数据集：[Argoverse 2 sensor数据下载教程](https://github.com/megvii-research/Far3D/blob/main/docs/get_started.md)。

## 启动容器
运行以下命令以启动容器环境：

```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-Far3D
export MODEL_PATH=</path/to/model>
export DATASET_PATH=</path/to/dataset>

docker run -itd --privileged --net=host \
    -w /workspace/ \
    -v ${MODEL_PATH}:/home \
    -v ${DATASET_PATH}:/av2sensor \
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

**安装依赖：**
```bash
# md5sum c02e802d945b2fa2a987325e3247e8bf
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/xmlir-1.0.0.1-cp38-cp38-linux_x86_64.whl
pip uninstall -y xmlir
pip install xmlir-1.0.0.1-cp38-cp38-linux_x86_64.whl
# md5sum 5503be9e2dd4f0f01d2ff2506507c5b8
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/xav_dsal-0.3.0-cp38-cp38-linux_x86_64.whl
pip uninstall -y xav_dsal
pip install xav_dsal-0.3.0-cp38-cp38-linux_x86_64.whl
pip install av2==0.2.1 refile kornia
```

**下载Far3D代码并解压：**
```bash
cd /home
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/Far3D.tar.gz
tar -zxvf Far3D.tar.gz && rm Far3D.tar.gz
```

**下载权重：**
```bash
cd /home/Far3D
mkdir ckpts && cd ckpts
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/cascade_mask_rcnn_r50_fpn_coco-20e_20e_nuim_20201009_124951-40963960.pth
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/dd3d_det_final.pth
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/depth_pretrained_v99-3jlw0p36-20210423_010520-model_final-remapped.pth
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/fcos3d_vovnet_imgbackbone-remapped.pth
```

## 训练与评估
### 执行单机8卡训练
```bash
cd /home/Far3D
export XPU_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export BKCL_PCIE_TOPO=1
# 修改配置文件far3d.py中的data_root
tools/dist_train.sh projects/configs/far3d.py 8 --work-dir work_dirs/far3d/
```

### 执行四机32卡训练
```bash
#!/usr/bin/env bash

#CONFIG=$1
GPUS=8

MASTER_ADDR="172.22.182.81"
PORT=${PORT:-28507}
NNODES=4
NODE_RANK=0 #根据NNODES数量修改，0，1，2，3

export DEFORM_ATTN_L3=true
export XPU_PCIE_SPEED_MODE=1
export XPUAPI_XPU_TF32_ROUND_MODE=1
export BKCL_PCIE_TOPO=1
export BKCL_ENABLE_XDR=1

export BKCL_RDMA_FORCE_TREE=1
export BKCL_ENABLE_TREE=1
export BKCL_RDMA_VERBS=1
export BKCL_RING_BUFFER_SIZE=8388608
export BKCL_MULTI_TREE_THRESHOLD=-1 #tree
#export BKCL_MULTI_TREE_THRESHOLD=0 #ring    # 关闭单机tree模式，避免某些同步issue
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0
export BKCL_SOCKET_IFNAME=ens13np0

PYTHONPATH="$(dirname $0)/..":$PYTHONPATH \
python -m torch.distributed.launch  --nnodes=$NNODES \
                                    --node_rank=$NODE_RANK \
                                    --nproc_per_node=$GPUS \
                                    --master_port=$PORT \
                                    --master_addr=$MASTER_ADDR \
                                    $(dirname "$0")/train.py \
                                    /home/Far3D/projects/configs/far3d.py \
                                    --seed 0 \
                                    --launcher pytorch ${@:3} \
                                    --work-dir  /home/Far3D/work_dirs/far3d/
```

### 单机8卡评估
```bash
cd /home/Far3D
export XPU_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export BKCL_PCIE_TOPO=1
# 修改配置文件far3d.py中的data_root
tools/dist_test.sh projects/configs/far3d.py work_dirs/far3d/iter_82548.pth 8 --eval bbox
```

