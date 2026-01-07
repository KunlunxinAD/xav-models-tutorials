# cosmos-transfer2.5

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器
```bash
YOUR_DOCKER_NAME=cosmos
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
        -v <code base>:/home            \
        --name ${YOUR_DOCKER_NAME}      \
        -w /home                        \
        --shm-size=256g                 \
        <your/docker/image> /bin/bash
```
# 代码
```bash
# commitID: 8b0e6af4b3bed40408c5762e528cb4e2a233f278
git clone https://github.com/nvidia-cosmos/cosmos-transfer2.5.git
cd cosmos-transfer2.5
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos-transfer2.5/assets.tgz 
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos-transfer2.5/support_xpu.patch
git apply support_xpu.patch
tar -xzvf assets.tgz #训练，测试需要的数据集都已经整理好，如果需要测试其他数据集，参考https://github.com/nvidia-cosmos/cosmos-transfer2.5 中对应项目中的文档下载整理
```
# 环境
* 安装python依赖

```bash
cd cosmos-transfer2.5 
wget -O xpytorch-cp310-torch251-ubuntu2004-x64.run https://klx-sdk-release-public.su.bcebos.com/xav/release/docker_images_d/25.12-py310/updates/xpytorch-cp310-torch251-ubuntu2004-x64.run
bash xpytorch-cp310-torch251-ubuntu2004-x64.run && rm xpytorch-cp310-torch251-ubuntu2004-x64.run
pip install -e ./packages/cosmos-cuda
pip install -e ./packages/cosmos-oss

pip install cattrs>=25.2.0
pip install easydict>=1.9
pip install moderngl>=5.11.1
pip install natsort>=8.4.0
pip install OpenEXR>=3.3.1
pip install pyarrow>=21.0.0
pip install sam2>=1.1.0
pip install shapely>=2.0.5
pip install git+https://github.com/jeanachoi/Video-Depth-Anything.git

pip install -e .
pip install accelerate==1.11.0 transformers==4.57.3 peft==0.18.0
```
# 测试
* 单卡

```bash
python examples/inference.py -i assets/robot_example/depth/robot_depth_spec.json -o outputs/depth
```
* 多卡

```bash
export CUDA_LAUNCH_BLOCKING=1
export BKCL_TREE_THRESHOLD=1

torchrun --nproc_per_node=8 --master_port=12341 examples/inference.py -i assets/robot_example/depth/robot_depth_spec.json -o outputs/depth
```
# 训练
* singleview

```bash
#参考：https://github.com/nvidia-cosmos/cosmos-transfer2.5/blob/main/docs/post-training_singleview.md
torchrun --nproc_per_node=8 --master_port=12345 -m scripts.train \
    --config=cosmos_transfer2/singleview_config.py \
    -- experiment=transfer2_singleview_posttrain_edge_example \
    dataloader_train.dataset.dataset_dir=assets/videoufo \
    'dataloader_train.sampler.dataset=${dataloader_train.dataset}' \
    trainer.max_iter=2000 \
    checkpoint.save_iter=500 \
    job.wandb_mode=disabled
```
* multiview

```bash
#参考：https://github.com/nvidia-cosmos/cosmos-transfer2.5/blob/main/docs/post-training_auto_multiview.md
torchrun --nproc_per_node=8 --master_port=12341 -m scripts.train \
    --config=cosmos_transfer2/_src/transfer2_multiview/configs/vid2vid_transfer/config.py \
    -- experiment=transfer2_auto_multiview_post_train_example job.wandb_mode=disabled
```