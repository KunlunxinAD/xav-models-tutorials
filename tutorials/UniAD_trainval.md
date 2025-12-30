# UniAD

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集
* 按照官方教程，下载并处理nuScenes数据集
[nuScenes数据下载教程](https://github.com/OpenDriveLab/UniAD/blob/main/docs/DATA_PREP.md)。

* 下载使用已经处理完的数据集：
    ```bash
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
    tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar
    ```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-UniAD
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

**下载UniAD代码并解压：**
```bash
cd /home
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/UniAD_v1.tar.gz
tar -zxvf UniAD.tar.gz && rm UniAD.tar.gz
```

**下载并解压data目录：**
```bash
cd /home/UniAD
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/data.tar.gz
tar -zxvf data.tar.gz && rm data.tar.gz
```

**下载并解压ckpts目录：**
```bash
cd /home/UniAD
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/ckpts.tar.gz
tar -zxvf ckpts.tar.gz && rm ckpts.tar.gz
```

**安装依赖：**
```bash
pip install motmetrics==1.1.3 casadi torchmetrics==1.4.1
pip install pytorch-lightning==1.2.5 --no-deps
```


## 训练与评估
### 注入patch
**下载patch文件：**
```bash
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/nuscenes_data_classes.patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/nuscenes_mot.patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/torchmetrics_metric.patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/multi_scale_deform_attn.patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/focal_loss.patch
```

**注入修改：**
```bash
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/nuscenes/eval/detection/data_classes.py < nuscenes_data_classes.patch
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/nuscenes/eval/tracking/mot.py < nuscenes_mot.patch
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/torchmetrics/metric.py < torchmetrics_metric.patch
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/mmcv/ops/multi_scale_deform_attn.py < multi_scale_deform_attn.patch
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/mmcv/ops/focal_loss.py < focal_loss.patch
```

### 执行训练
#### 单机8卡训练
**训练stage 1：**
```bash
cd /home/UniAD
export XPYTORCH_RUN_ENHANCE=1
export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
./tools/uniad_dist_train.sh ./projects/configs/stage1_track_map/base_track_map.py 8
unset CUDA_VISIBLE_DEVICES
unset XMLIR_ENABLE_LINEAR_FC_FUSION
popd
```

**训练stage 2：**
```bash
cd /home/UniAD
export XPYTORCH_RUN_ENHANCE=1
export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
./tools/uniad_dist_train.sh  ./projects/configs/stage2_e2e/base_e2e.py  8
unset CUDA_VISIBLE_DEVICES
unset XMLIR_ENABLE_LINEAR_FC_FUSION
popd
```

#### 单机8卡评估
**评估stage 1：**
```bash
cd /home/UniAD
export XPYTORCH_RUN_ENHANCE=1
export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
./tools/uniad_dist_eval.sh ./projects/configs/stage1_track_map/base_track_map.py ./ckpts/uniad_base_track_map.pth  8
unset CUDA_VISIBLE_DEVICES
unset XMLIR_ENABLE_LINEAR_FC_FUSION
popd
```

**评估stage 2：**
```bash
cd /home/UniAD
export XPYTORCH_RUN_ENHANCE=1
export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
./tools/uniad_dist_eval.sh ./projects/configs/stage2_e2e/base_e2e.py  ./ckpts/uniad_base_e2e.pth  8
unset CUDA_VISIBLE_DEVICES
unset XMLIR_ENABLE_LINEAR_FC_FUSION
popd
```
