# PETR Trainval Guide
## 环境准备
### 镜像
请联系昆仑芯客户支持获取开发环境镜像

### 创建docker
```
YOUR_DOCKER_NAME=petr
XPU_NUM=8
if [ -n "$2" ]; then
  XPU_NUM=$2
fi
echo "docker container name: ${YOUR_DOCKER_NAME}, kl3 card num: ${XPU_NUM}"
DOCKER_DEVICE_CONFIG=" "
if [ $XPU_NUM -gt 0 ]; then
for ((idx=0; idx<=$XPU_NUM-1; idx++)); do
  DOCKER_DEVICE_CONFIG+=" --device=/dev/xpu${idx}:/dev/xpu${idx} "
done
DOCKER_DEVICE_CONFIG+=" --device=/dev/xpuctrl:/dev/xpuctrl "
fi
docker run -d -p 8000:8081 -it          \
        ${DOCKER_DEVICE_CONFIG}         \
        --net=host                      \
        --pid=host                      \
        --cap-add=SYS_PTRACE            \
        --privileged                    \
        -v <code path>:/workspace  \
        -v <nuscenes path>:/data/Dataset/nuScenes \
        --name ${YOUR_DOCKER_NAME}      \
        -w /home                        \
        --shm-size=256g                 \
        iregistry.baidu-int.com/kunlunxin-self-driving/xav-d:25.11-py38 /bin/bash
```
### 准备数据集
准备nuscenes数据集，将数据集的路径在 `/data/Dataset/nuScenes`

```
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
tar -xvf HDmaps-final.tar
```
### mmcv环境适配
```
#自动配置mmcv等
bash /opt/xav_sdk/scripts/env_setup_xav_d_2511_py38.sh
```
### 模型代码
方式一：从源码开始

```
mkdir -p /workspace/PETR_DIR
cd /workspace/PETR_DIR

# or https://gitee.com/open-mmlab/mmdetection3d.git
git clone  https://github.com/open-mmlab/mmdetection3d.git

cd mmdetection3d
git checkout v0.17.1

# clone into /workspace/PETR_DIR/PETR
git clone https://github.com/megvii-research/PETR.git

cd /workspace/PETR_DIR/PETR
ln -s ../mmdetection3d ./mmdetection3d

BASE_URL=https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/PETR_nuscenes_data

wget -c ${BASE_URL}/nuscenes_eval_detection_data_classes_py.patch
patch $CONDA_PREFIX/lib/python3.8/site-packages/nuscenes/eval/detection/data_classes.py < nuscenes_eval_detection_data_classes_py.patch

wget -c ${BASE_URL}/PETR_modle.patch
git apply PETR_modle.patch
```
方式二：直接使用整理好的code文件

```
mkdir -p /workspace/PETR_DIR
cd /workspace/PETR_DIR

BASE_URL=https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/PETR_nuscenes_data
wget -c ${BASE_URL}/PETR_CODE.tar.gz
```
### 数据集&权重
```
cd /workspace/PETR_DIR/PETR
mkdir -p ckpts
mkdir -p data

ln -s /data/Dataset/nuScenes ./data/nuscenes
ln -s /data/Dataset/nuScenes/petr/HDmaps-final ./data/nuscenes/HDmaps-final

cd ckpts
BASE_URL=https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/PETR_nuscenes_data
wget -c ${BASE_URL}/resnet50_msra-5891d200.pth
wget -c ${BASE_URL}/fcos3d_vovnet_imgbackbone-remapped.pth
```
## 启动脚本
### train
```
#修改 NGPUS 控制训练使用的GPU卡数量（[1-8]），比如NGPUS=1使用1张卡，NGPUS=8使用8卡
#修改 MODEL_CONFIG_DIR 和 MODEL_CONFIG 控制训练的模型，目前已为 PETRv2 的 BEVseg 模型 和 petrv2_vovnet_gridmask_p4_800x320 模型做好了适配。
#修改 LOG_DIR，LOG_FILE 控制训练产生的日志的路径
#修改 RESUME_FILE控制重启训练的模型文件路径，注释掉RESUME_FILE为从头开始训练。
set -ex
RUN_PORT=28615
NGPUS=8

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
    --cfg-options data.workers_per_gpu=4 data.samples_per_gpu=16 lr_config.warmup_iters=1 resume_from=$RESUME_FILE 2>&1 | tee $LOG_FILE
```
### test
```
set -ex

RUN_PORT=28615

NGPUS=8

LOG_DIR=log
LOG_PREFIX=$LOG_DIR/petr_
LOG_FILE=$LOG_PREFIX$(date +'%Y-%m-%d_%H-%M-%S').txt

# MODEL_CONFIG_DIR=projects/configs/petrv2
# MODEL_CONFIG=petrv2_BEVseg
# MODEL_CONFIG=petrv2_vovnet_gridmask_p4_800x320
# MODEL_CONFIG_FILE=$MODEL_CONFIG_DIR/$MODEL_CONFIG.py
# RESUME_FILE= work_dirs/$MODEL_CONFIG/latest.pth

mkdir -p $LOG_DIR
mkdir -p work_dirs

PORT=$RUN_PORT tools/dist_test.sh $MODEL_CONFIG_FILE $NGPUS --work-dir work_dirs/$MODEL_CONFIG/ \
    --cfg-options data.workers_per_gpu=1 resume_from=$RESUME_FILE 2>&1 | tee $LOG_FILE
```