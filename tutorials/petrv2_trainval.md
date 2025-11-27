# PETRv2

## 准备环境

请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器
执行以下脚本以启动容器：
```bash
export XAV_IMAGE=</XAV_IMAGE>
export CONTAINER_NAME=xav-petrv2-test
export MOUNT_PATH=</path/to/petrv2>
export NUSCENES_PATH=</path/to/dataset>

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
        -v ${MOUNT_PATH}:/home \
        -v ${NUSCENES_PATH}:/data/nuscenes \
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

```bash
# 创建conda env
conda create -n PETRv2 --clone python38_torch201_cuda
conda init bash
conda activate PETRv2
```

## 下载及安装资源
### 下载PETRv2代码
```bash
cd /workspace/ #选择拉取代码路径,可任意修改
mkdir PETR_DIR #创建PETR_DIR文件夹,后续所有的PETR相关文件都下载到此处
cd PETR_DIR

wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/PETRv2/PETRv2.tar.gz
tar -zxvf PETRv2.tar.gz
```

### 安装mmdetection
```bash
cd /workspace/PETR_DIR/

#拉取mmdetection3d代码
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/xav/v0.2/mmdetection3d.tar.gz 
tar -zxvf mmdetection3d.tar.gz

cd mmdetection3d
git checkout v0.17.1 #切换mmdetection分支

#还需将PETR中使用的mmdetection3d进行软链接
cd /workspace/PETR_DIR/PETR
ln -s ../mmdetection3d ./mmdetection3d
```

### 下载预训练权重
```bash
cd /workspace/PETR_DIR/PETR
mkdir ckpts
cd ckpts
BASE_URL=https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/PETR_nuscenes_data
wget -c ${BASE_URL}/resnet50_msra-5891d200.pth
wget -c ${BASE_URL}/fcos3d_vovnet_imgbackbone-remapped.pth
```

### 下载及配置nuScenes数据集
1. 下载数据集：
    ```bash
    # 参考官方文档准备数据集
    #https://github.com/open-mmlab/mmdetection3d/blob/master/docs/en/data_preparation.md

    # 或者可以从bos云直接下载已经打包好的数据包, 并解压到/data/Dataset/nuScenes
    cd /data/Dataset/nuScenes

    PREFIX=/data/Dataset/nuScenes/petr/
    BASE_URL=https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/PETR_nuscenes_data
    wget -P ${PREFIX} -c ${BASE_URL}/HDmaps-final_infos_train.pkl
    wget -P ${PREFIX} -c ${BASE_URL}/HDmaps-final_infos_val.pkl
    wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_2f_infos_train.pkl
    wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_2f_infos_val.pkl
    wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_30f_infos_train.pkl
    wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_30f_infos_val.pkl
    wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_30f_infos_test.pkl
    wget -P ${PREFIX} -c ${BASE_URL}/nuscenes_infos_train.pkl
    wget -P ${PREFIX} -c ${BASE_URL}/nuscenes_infos_val.pkl
    wget -P ${PREFIX} -c ${BASE_URL}/HDmaps-final.tar

    cd /data/Dataset/nuScenes/petr
    tar -xvf HDmaps-final.tar && rm HDmaps-final.tar

    cd /workspace/PETR_DIR/PETR
    mkdir -p data

    ln -s /data/Dataset/nuScenes ./data/nuscenes
    ln -s /data/Dataset/nuScenes/petr/HDmaps-final ./data/nuscenes/HDmaps-final
    ```

2. 注入patch文件：
    ```bash
    cd /workspace/PETR_DIR/PETR

    BASE_URL=https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/PETR_nuscenes_data

    wget -c ${BASE_URL}/nuscenes_eval_detection_data_classes_py.patch
    patch $CONDA_PREFIX/lib/python3.8/site-packages/nuscenes/eval/detection/data_classes.py < nuscenes_eval_detection_data_classes_py.patch

    wget -c ${BASE_URL}/PETR_modle.patch
    git apply PETR_modle.patch
    ```

## TrainVal 模型教程
### 模型训练
创建运行模型训练命令的脚本run_train.sh，将下面的命令行内容复制到新创建的文件run_train.sh中。运行命令bash run_train.sh以执行训练。

脚本参数说明：
* 修改NGPUS控制训练使用的GPU卡数量([1-8])，比如NGPUS=1使用1张卡，NGPUS=8使用8张卡。
* 修改MODEL_CONFIG_DIR和MODEL_CONFIG控制训练的模型，目前已为PETRv2的BEVseg模型 、petrv2_vovnet_gridmask_p4_800x320 和petrv2_vovnet_gridmask_p4_1600x640_trainval_cbgs模型做好了适配。
* 修改LOG_DIR,LOG_FILE控制训练产生的日志的路径。
* 修改RESUME_FILE控制重启训练的模型文件路径，注释掉RESUME_FILE为从头开始训练。

```bash
set -ex

RUN_PORT=28615

NGPUS=1

LOG_DIR=log
LOG_PREFIX=$LOG_DIR/petr_
LOG_FILE=$LOG_PREFIX$(date +'%Y-%m-%d_%H-%M-%S').txt

MODEL_CONFIG_DIR=projects/configs/petrv2

# MODEL_CONFIG=petrv2_BEVseg
MODEL_CONFIG=petrv2_vovnet_gridmask_p4_800x320

MODEL_CONFIG_FILE=$MODEL_CONFIG_DIR/$MODEL_CONFIG.py

# RESUME_FILE= work_dirs/$MODEL_CONFIG/latest.pth

mkdir -p $LOG_DIR
mkdir -p work_dirs

PORT=$RUN_PORT tools/dist_train.sh $MODEL_CONFIG_FILE $NGPUS --work-dir work_dirs/$MODEL_CONFIG/ \
    --cfg-options data.workers_per_gpu=1 resume_from=$RESUME_FILE 2>&1 | tee $LOG_FILE
```
### 模型评估
创建运行模型训练命令的脚本run_test.sh，将下面的命令行内容复制到新创建的文件 run_test.sh中。运行命令bash run_test.sh即可执行训练。

脚本参数说明：
* 修改 NGPUS 控制训练使用的GPU卡数量([1-8])，比如NGPUS=1使用1张卡，NGPUS=8使用8张卡。
* 修改 MODEL_CONFIG_DIR 和 MODEL_CONFIG 控制训练的模型，目前已为 PETRv2 的 BEVseg 模型 、petrv2_vovnet_gridmask_p4_800x320 和petrv2_vovnet_gridmask_p4_1600x640_trainval_cbgs模型做好了适配。
* 修改LOG_DIR,LOG_FILE控制训练产生的日志的路径。

```bash
#!/usr/bin/env bash
set -ex

NGPUS=1
RUN_PORT=28615

LOG_DIR=log
LOG_PREFIX=$LOG_DIR/petr_eval
LOG_FILE=$LOG_PREFIX$(date +'%Y-%m-%d_%H-%M-%S').txt

MODEL_CONFIG_DIR=projects/configs/petrv2

MODEL_CONFIG=petrv2_vovnet_gridmask_p4_800x320

MODEL_CONFIG_FILE=$MODEL_CONFIG_DIR/$MODEL_CONFIG.py

CKPTS=latest
CKPTS_FILE=work_dirs/$MODEL_CONFIG/$CKPTS.pth

EVAL_TARGET=bbox

PORT=$RUN_PORT tools/dist_test.sh $MODEL_CONFIG_FILE $CKPTS_FILE $NGPUS --eval $EVAL_TARGET 2>&1 | tee $LOG_FILE
```