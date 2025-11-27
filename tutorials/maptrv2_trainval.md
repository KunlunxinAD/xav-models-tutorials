# MapTRv2

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

⚠注意

请根据实际环境替换以下变量：

|变量|描述|
|---|---|
|<XAV_IMAGE>|开发环境镜像|
|</path/to/maptr>|本地MapTR代码路径|
|</path/to/nuscenes>|本地nuScenes数据集路径|

## 准备数据集及代码
### 准备数据集
```bash
# 参考官方文档准备数据集
https://github.com/hustvl/MapTR/blob/maptrv2/docs/prepare_dataset.md

# 或者可以从bos云直接下载已经打包好的数据包, 并解压到/path/to/nuscenes
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar
tar -xvf changan_nuscenes.tar&& rm changan_nuscenes.tar
```

### 下载预处理后的数据
<!-- 此步骤非必需，若直接下载预处理后的数据，可以跳过下面的“预处理数据集”章节以节省时间 -->
```bash
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/maptr_v2_nuscenes_pkl.tar.gz
tar -zxvf maptr_v2_nuscenes_pkl.tar.gz
cp maptr_v2_nuscenes_pkl/* </path/to/nuscenes/>
```

### 下载代码及预训练权重
```bash
# 从bos云下载模型代码
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/maptr_v2/MapTR_v2.tar.gz
tar -zxvf MapTR_v2.tar.gz

# resnet, 该权重为官方权重备份，如官方下载网络较慢，可以从bos云下载
cd MapTR
mkdir ckpts
cd ckpts 
wget https://download.pytorch.org/models/resnet50-19c8e357.pth
wget https://download.pytorch.org/models/resnet18-f37072fd.pth
# bos云下载链接：
# wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/resnet50-19c8e357.pth
# wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/resnet18-f37072fd.pth
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export CONTAINER_NAME=MapTR_v2_test
export PATH_TO_MOUNT=</path/to/maptr> #本地路径
export PATH_AFTER_MOUNT=/home/MapTR #挂载后在容器内的路径
export NUSCENES_PATH=</path/to/nuscenes>

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
        -v ${PATH_TO_MOUNT}:${PATH_AFTER_MOUNT} \
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
### 配置容器内环境
```bash
# 1. 创建conda env
conda create -n MapTR_env --clone python38_torch201_cuda
conda init bash
conda activate MapTR_env

# 2. 安装依赖
pip install torchaudio==2.0.2+cu117 -f https://download.pytorch.org/whl/torch_stable.html
pip install more-itertools

cd /home/MapTR
pip install -r requirement.txt

# 3. <XAV_IMAGE>v1.0.0版本镜像需要替换此库文件，其它版本跳过此步骤
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/maptr_v2/libxpu_blas.so_v0.5
mv /root/miniconda/envs/MapTR_env/lib/python3.8/site-packages/torch_xmlir/libxpu_blas.so /root/miniconda/envs/MapTR_env/lib/python3.8/site-packages/torch_xmlir/libxpu_blas.so_bk
mv libxpu_blas.so_v0.5 /root/miniconda/envs/MapTR_env/lib/python3.8/site-packages/torch_xmlir/libxpu_blas.so

# 4. 使用软链接指向实际的数据集目录
ln -sf /data data
```

## 多卡训练
<!-- ### 预处理数据集
```bash
# 如果在“下载代码及预训练权重”章节中已经下载预处理后的数据，可以跳过此步骤
cd /home/MapTR
python tools/maptrv2/custom_nusc_map_converter.py --root-path /data/nuscenes --out-dir /data/nuscenes --extra-tag nuscenes --version v1.0 --canbus /data/nuscenes
``` -->

### 执行单机多卡训练
```bash
# 单机8卡
./tools/dist_train.sh ./projects/configs/maptrv2/maptrv2_nusc_r50_24ep.py 8
```

### 执行多机训练
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
./tools/dist_train.sh ./projects/configs/maptrv2/maptrv2_nusc_r50_24ep.py 8
```