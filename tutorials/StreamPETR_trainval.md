# StreamPETR

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器

```bash
export XAV_IMAGE=iregistry.baidu-int.com/kunlunxin-self-driving/xav-base:v1.0.0 #此处修改image版本
export CONTAINER_NAME=xav-streampetr-test
export MOUNT_PATH=/your/mount/path
export NUSCENES_PATH=/your/path/to/nuscenes

docker run -dti \
        --name ${CONTAINER_NAME} \
        --net=host \
        --security-opt=seccomp=unconfined \
        -e IMAGE_VERSION="${XAV_IMAGE}" \
        --cap-add=SYS_PTRACE \
        --shm-size=128g \
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
```

## 下载及安装资源

**下载StreamPETR代码并解压：**
```bash
git clone https://github.com/exiawsh/StreamPETR.git
```

**依赖库安装：**
```bash
pip install flash-attn==0.2.2
pip install mmcv-full==1.6.0
pip install mmdet==2.28.2
pip install mmsegmentation==0.30.0
cd StreamPETR
git clone https://github.com/open-mmlab/mmdetection3d.git
cd mmdetection3d
git checkout v1.0.0rc6 
```

**数据集下载：**
```bash
cd StreamPETR
mkdir data && cd data

#下载nuscenes数据集
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar

#下载can_bus数据集
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/can_bus.zip
unzip can_bus.zip && rm can_bus.zip

cd nuscenes
wget https://klx-sdk-release-public.su.bcebos.com/xav/data/streampetr_data/nuscenes2d_temporal_infos_train.pkl ./
wget https://klx-sdk-release-public.su.bcebos.com/xav/data/streampetr_data/nuscenes2d_temporal_infos_test.pkl ./
wget https://klx-sdk-release-public.su.bcebos.com/xav/data/streampetr_data/nuscenes2d_temporal_infos_val.pkl ./
```

**预训练权重下载：**
```bash
cd StreamPETR
mkdir ckpts & cd ckpts
wget https://klx-sdk-release-public.su.bcebos.com/xav/data/streampetr_data/fcos3d_vovnet_imgbackbone-remapped.pth
```

## 环境适配

**flash_attn修改：**

将projects/mmdet3d_plugin/models/utils/attention.py文件中的
```bash
from flash_attn.flash_attn_interface import flash_attn_unpadded_kvpacked_func
```
替换为

```bash
try:
    from flash_attn.flash_attn_interface import flash_attn_unpadded_kvpacked_func
    print('Use flash_attn_unpadded_kvpacked_func')
except:
    from flash_attn.flash_attn_interface import  flash_attn_varlen_kvpacked_func as flash_attn_unpadded_kvpacked_func
    print('Use flash_attn_varlen_kvpacked_func')
```

**回退XBLAS版本：**

```bash
cd /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/torch_xmlir/
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/maptr_v2/libxpu_blas.so_v0.5
mv libxpu_blas.so libxpu_blas.so_bk
mv libxpu_blas.so_v0.5 libxpu_blas.so
```

**更新xmlir版本：**

```bash
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/streampetr_data/xpytorch-cp38-torch201-ubuntu2004-x64.run
bash xpytorch-cp38-torch201-ubuntu2004-x64.run
```

## 训练与评估
**导入环境变量：**
```bash
export XMLIR_BMM_DISPATCH_VALUE=1
export XMLIR_CUDNN_ENABLED=0
export BKCL_FORCE_SYNC=1
```
**单机八卡训练：**
```bash
tools/dist_train.sh projects/configs/StreamPETR/stream_petr_vov_flash_800_bs2_seq_24e.py 8 --work-dir work_dirs/stream_petr_vov_flash_800_bs2_seq_24e
```

**单机八卡评估：**
```bash
tools/dist_test.sh projects/configs/StreamPETR/stream_petr_vov_flash_800_bs2_seq_24e.py work_dirs/stream_petr_vov_flash_800_bs2_seq_24e/latest.pth 8 --eval bbox
```