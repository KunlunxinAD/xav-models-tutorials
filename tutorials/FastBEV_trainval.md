# FastBEV

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 下载数据集
通过下面两种方式之一来下载数据集：

* 按照官方教程，下载并处理nuScenes数据集[nuScenes数据下载教程](https://github.com/hustvl/VAD/blob/main/docs/prepare_dataset.md)。
* 下载使用已经处理完的数据集，可以直接使用nuscenes数据集及处理后生成的pkl文件，跳过数据处理过程。

```
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar
```
## 启动容器
```
export XAV_IMAGE=<path/to/image>
export XAV_CONTAINER=Fast_BEV_test

# 注意, 这里需修改为主机上实际存放nuscenes数据集的路径
export NUSCENES_PATH=/path/to/nuscenes

docker run -itd --privileged --net=host \
    --security-opt=seccomp=unconfined \
    -v `pwd`:/workspace \
    -w /workspace \
    -v ${NUSCENES_PATH}:/data/nuscenes \
    --name ${XAV_CONTAINER} \
    --shm-size 64g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${XAV_CONTAINER} bash
```
## 下载及安装依赖
1. 下载Fast_BEV代码并解压：

```
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/Fast_BEV/Fast_BEV.tar.gz
tar -zxvf Fast_BEV.tar.gz && rm Fast_BEV.tar.gz
```
2. 准备数据集

```
cd Fast-BEV
mkdir data
ln -sf /data/nuscenes/ data/nuscenes
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/fastbve_data.tar.gz && tar -zxvf fastbve_data.tar.gz && rm fastbve_data.tar.gz
cp fastbve_data/nuscenes_infos_test_4d_interval3_max60.pkl fastbve_data/nuscenes_infos_train_4d_interval3_max60.pkl fastbve_data/nuscenes_infos_val_4d_interval3_max60.pkl data/nuscenes/
cp -r fastbve_data/pretrained_models/ .
rm -r fastbve_data
```
3. 注入 patch，下载依赖

```
#注入patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/mmcv_registry.patch 
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/sparse_init.patch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/batchnorm.patch

bash /opt/xav_sdk/scripts/env_setup_xav_d_2511_py38.sh
patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/mmcv/utils/registry.py < mmcv_registry.patch 

patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/torch/sparse/__init__.py < sparse_init.patch 

patch /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/torch/nn/modules/batchnorm.py < batchnorm.patch

rm *.patch
pip install --force-reinstall numba==0.48.0 numpy==1.23.5
```
## 执行训练
```
conda activate python38_torch201_cuda
cd /workspace/Fast-BEV
# 单卡训练
export PYTHONPATH=/workspace/Fast-BEV:$PYTHONPATH
python tools/train.py configs/fastbev/exp/paper/fastbev_m0_r18_s256x704_v200x200x4_c192_d2_f4.py --work-dir /workspace/Fast-BEV/log_train/
# 多卡训练
torchrun -nproc_per_node=8 tools/train.py\
configs/fastbev/exp/paper/fastbev_m0_r18_s256x704_v200x200x4_c192_d2_f4.py\
--launcher pytorch\
--work-dir /workspace/Fast-BEV/log_train/\
```
