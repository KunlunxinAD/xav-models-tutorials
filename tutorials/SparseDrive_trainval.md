# SparseDrive

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 下载数据集
通过以下两种方式之一来下载数据集：
* 按照官方教程，下载并处理nuScenes数据集:
[nuScenes数据下载教程](https://github.com/OpenDriveLab/UniAD/blob/main/docs/DATA_PREP.md)。

* 下载使用已经处理完的数据集，可以直接使用nuScenes数据集及处理后生成的pkl文件，跳过数据处理过程。
    ```bash
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
    tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar
    ```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-SparseDrive
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
 * 依赖安装：
    ```bash
    pip install motmetrics==1.1.3
    pip install more_itertools
    ```

* 下载SparseDrive代码并解压：
    ```bash
    cd /home
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/SparseDrive.tar.gz
    tar -zxvf SparseDrive.tar.gz && rm SparseDrive.tar.gz
    ```

* 下载权重：
    ```bash
    cd /home/SparseDrive
    mkdir ckpt && cd ckpt
    wget https://download.pytorch.org/models/resnet50-19c8e357.pth
    # 或者从BOS上下载
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/resnet50-19c8e357.pth
    ```

* 准备数据集：
    ```bash
    cd /home/SparseDrive/
    mkdir data && cd data
    ln -sf   /nuscenes   /home/SparseDrive/data/nuscenes
    **方式一：按照官方教程，通过K-means生成锚框**
    https://github.com/swc-17/SparseDrive/blob/main/docs/quick_start.md
    **方式二：下载使用已经处理完的文件**
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/infos.tar.gz
    tar -xvf infos.tar.gz
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/kmeans.tar.gz
    tar -xvf kmeans.tar.gz
    ```

## 训练与评估
### 注入patch
1. 下载patch文件：
    ```bash
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/nuscenes_mot.patch
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/text.patch
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/algo.patch
    ```

2. 注入修改：
    ```bash
    patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/nuscenes/eval/tracking/mot.py < nuscenes_mot.patch
    patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/mmcv/runner/hooks/logger/text.py < text.patch
    patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/nuscenes/eval/tracking/algo.py < algo.patch
    ```

### 执行训练
#### 单机8卡训练

⚠注意

目前训练和评估需要分开进行（config文件中的evaluation interval需要配置成大于等于total_epochs * num_iters_per_epoch）

* 训练stage 1
    ```bash
    cd /home/SparseDrive
    export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
    # 修改train.sh，注释掉stage2的config，仅使用stage1的config，执行训练
    bash ./scripts/train.sh
    ```

* 训练stage 2
    ```bash
    cd /home/SparseDrive
    export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
    # 手动将stage1训练出的checkpoint移动至ckpt路径下并重命名：
    mv work_dirs/sparsedrive_small_stage1/<checkpoint_in_stage1> ckpt/sparsedrive_stage1.pth
    # 修改train.sh，注释掉stage1的config，仅使用stage2的config，执行训练
    bash ./scripts/train.sh
    ```

#### 执行多机训练
根据实际环境修改tools/multi_nodes_env.sh中的下列环境变量
```bash
export NNODES=4 #根据机器数量修改
export NODE_RANK=0 #修改为每台机器的rank(0,1,2,3...)
export PORT=29600
export MASTER_ADDR="172.22.182.81" #根据主节点IP修改
export BKCL_RDMA_NICS=ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改
export BKCL_SOCKET_IFNAME=ens13np0 #根据机器网卡名修改
```
执行训练
```bash
source tools/multi_nodes_env.sh
#同单机训练，修改./scripts/train.sh的训练stage后执行
bash ./scripts/train.sh
```

#### 单机8卡评估
* 评估stage 1
    ```bash
    cd /home/SparseDrive
    export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
    # 修改test.sh，使用stage1的config.执行eval
    bash ./scripts/test.sh
    ```

* 评估stage 2
    ```bash
    cd /home/SparseDrive
    export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
    # 修改test.sh，使用stage2的config，执行eval
    bash ./scripts/test.sh
    ```
