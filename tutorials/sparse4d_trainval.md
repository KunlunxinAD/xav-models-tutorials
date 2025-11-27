# Sparse4D

## 环境准备
请联系昆仑芯客户支持获取开发环境镜像。

⚠注意

请根据实际环境替换以下变量：

|变量|含义|
|---|---|
|<XAV_IMAGE>|开发环境镜像|
|</path/to/sparse4d>|本地Sparse4D代码路径|
|</path/to/nuscenes>|本地nuScenes数据集路径|

## 数据集及代码准备
### 数据集准备
```bash
# 方式1: 参考官方文档准备数据集
https://github.com/HorizonRobotics/Sparse4D/blob/main/docs/quick_start.md

# 方式2: 或者可以从bos云直接下载已经已经处理完的数据包(包含处理后生成的pkl文件), 并解压到/path/to/nuscenes
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar
```

### 下载代码及预训练权重
```bash
# 从bos云下载模型代码
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/Sparse4D.tar.gz
tar -zxvf Sparse4D.tar.gz

# resnet, 该权重为官方权重备份，如官方下载网络较慢，可以从bos云下载
cd Sparse4D
mkdir ckpt && cd ckpt
wget https://download.pytorch.org/models/resnet50-19c8e357.pth
# bos云下载链接：
# wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/resnet50-19c8e357.pth
```

### 下载预处理后的数据
```bash
cd ../Sparse4D
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_kmeans900.npy
mkdir -p data/nuscenes_anno_pkls && cd data/nuscenes_anno_pkls
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_anno_pkls/nuscenes_infos_test.pkl
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_anno_pkls/nuscenes_infos_train.pkl
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_anno_pkls/nuscenes_infos_val.pkl
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export CONTAINER_NAME=Sparse4D_test
export PATH_TO_MOUNT=</path/to/sparse4d> #本地路径
export NUSCENES_PATH=</path/to/nuscenes>

docker run -itd --privileged --net=host \
    -w /workspace/ \
    -v ${PATH_TO_MOUNT}:/home \
    -v ${NUSCENES_PATH}:/nuscenes \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${CONTAINER_NAME} \
    --shm-size 64g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${CONTAINER_NAME} bash
```
### 容器内环境配置
```bash
# 安装依赖
cd /home/Sparse4D
pip install motmetrics==1.1.3
pip install torchaudio==2.0.2 --extra-index-url https://download.pytorch.org/whl/cu117

# 使用软链接指向实际的数据集目录
ln -sf /data/nuscenes ./data/nuscenes
```

## 多卡训练
### 数据集预处理
<!-- ```bash
# 如果在“载预处理后的数据”章节中已经下载预处理后的数据，可以跳过此步骤
cd /home/Sparse4D
pkl_path="data/nuscenes_anno_pkls"
mkdir -p ${pkl_path}
export OPENBLAS_NUM_THREADS=64
export OMP_NUM_THREADS=64
python3 tools/nuscenes_converter.py --version v1.0-mini --info_prefix ${pkl_path}/nuscenes-mini
python3 tools/nuscenes_converter.py --version v1.0-trainval,v1.0-test --info_prefix ${pkl_path}/nuscenes
python3 tools/anchor_generator.py --ann_file ${pkl_path}/nuscenes_infos_train.pkl
``` -->

### 注入修改
```bash
#下载patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/patch/nuscenes_mot.patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/patch/mmcv_logger.patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/patch/nuscenes_eval.patch

#注入修改
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/nuscenes/eval/tracking/mot.py < nuscenes_mot.patch
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/mmcv/runner/hooks/logger/text.py < mmcv_logger.patch
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/nuscenes/eval/tracking/algo.py < nuscenes_eval.patch
```

### 执行单机多卡训练

注意：目前训练和评估需要分开进行（config文件中的evaluation interval需要配置成大于等于total_epochs * num_iters_per_epoch）
```bash
# 单机8卡训练
bash local_train.sh sparse4dv3_temporal_r50_1x8_bs6_256x704 8

# 单机评估
bash local_test.sh sparse4dv3_temporal_r50_1x8_bs6_256x704 <path/to/checkpoint> 1
```

### 执行多机训练
根据实际环境修改tools/multi_nodes_env.sh中的下列环境变量：
```bash
export NNODES=4 #根据机器数量修改
export NODE_RANK=0 #修改为每台机器的rank(0,1,2,3...)
export PORT=29600
export MASTER_ADDR="172.22.182.81" #根据主节点IP修改
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改
export BKCL_SOCKET_IFNAME=ens13np0 #根据机器网卡名修改
```
执行训练：
```bash
source tools/multi_nodes_env.sh
bash local_train.sh sparse4dv3_temporal_r50_1x8_bs6_256x704 8
```
