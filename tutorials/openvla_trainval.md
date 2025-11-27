# OpenVLA Trainval Guide

## 环境准备
### 准备开发环境镜像
请联系昆仑芯客户支持获取开发环境镜像

### PCIe环境配置（OAM跳过此步骤）
#### 确定PCIe环境
在宿主机执行如下命令：
```bash
xpu_smi xpulink -s
```
如果检测到显示为“P800 PCIe”，并且 Link 0/1/2/3 的状态均为“active”，则表明当前系统处于 PCIe 模式且8张卡已成功实现互联。

#### 配置PCIe的8卡互联模式
如果当前系统处于 PCIe 模式但8张卡未成功实现互联，需要在宿主机系统端执行以下配置操作：
```bash
# 步骤 1：修改/etc/modprobe.d目录下的kunlun.conf文件
vim /etc/modprobe.d/kunlun.conf
# 步骤 2：添加配置参数 PcieLink=1，该参数应与其他配置参数置于同一对双引号内，并使用分号 (;) 进行分隔。示例如下：
options kunlun KLreg_RegistryDwords="RmDisableSMMU=1;RmKL3ShiftMode=0;PcieLink=1"
# 步骤 3：重启服务器，即可完成配置的修改，服务器可支持 8 卡互联。
```

## 数据集及代码准备
### 数据集准备
```bash
# 参考官方文档选择微调数据集
https://github.com/openvla/openvla/tree/main

# 如需要使用自定义数据集，可参考官方文档进行下载或配置
https://github.com/kpertsch/rlds_dataset_builder

# 如需要使用相同数据集，可下载BridgeData V2数据
mkdir openvla_data
cd openvla_data
wget -r -nH --cut-dirs=4 --reject="index.html*" https://rail.eecs.berkeley.edu/datasets/bridge_release/data/tfds/bridge_dataset/
mv bridge_dataset bridge_orig

# 相同的数据集可以从bos上获取
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/openvla_data.tar.gz
tar -zxvf openvla_data.tar.gz
cd openvla_data & mv bridge_dataset bridge_orig
```

### 下载代码及预训练权重
```bash
# 以下权重为offline模式需要下载的权重，online模式可不用额外下载
# 下载openvla-7b-prismatic 模型权重（用于fully fine-tune）
git lfs install
git clone https://hf-mirror.com/openvla/openvla-7b-prismatic

# 下载vision backbones权重：
# dino features
git lfs install
git clone https://hf-mirror.com/timm/vit_large_patch14_reg4_dinov2.lvd142m

# siglip features
git lfs install
git clone https://hf-mirror.com/timm/ViT-SO400M-14-SigLIP

# 下载基模型权重：(llama2)
git lfs install
git clone https://www.modelscope.cn/shakechen/Llama-2-7b-hf.git

# 下载OpenVLA代码
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/openvla.tar.gz
tar -zxvf openvla.tar.gz
mv openvla_code openvla
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export LLAMA_CONTAINER=openvla_test
export MODEL_PATH=</path/to/openvla_dir> #本地路径

docker run -itd --privileged --net=host \
    -w /workspace/xav-models/modelzoo \
    -v ${MODEL_PATH}:/home \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${LLAMA_CONTAINER} \
    --shm-size 256g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${XAV_CONTAINER} bash
```

### 容器内环境配置
```bash
# 切换 conda env
conda activate python310_torch25_cuda
# 拉取并安装xmlir 产出
# 联系昆仑芯客户支持获取产出版本
bash xpytorch-cp310-torch251-ubuntu2004-x64.run

# 安装环境依赖
apt-get install cmake

cd openvla
pip install --no-deps -e .

pip install flash-attn==2.4.0.post1
pip install timm
pip install draccus
pip install dlib
pip install -e git+https://github.com/kvablack/dlimp@d08da3852c149548aaa8551186d619d87375df08#egg=dlimp
pip install tensorflow-graphics
pip install wandb
pip install transformers==4.47.0

# 设置hf token
# Copy HF token value into token file. Replace "hf_..." with your own token value!
# See: https://huggingface.co/docs/hub/en/security-tokens
echo hf_... >>> .hf_token

# patch代码修改
cd openvla
# 修改/root/miniconda/envs/python310_torch25_cuda/lib/python3.10/site-packages/transformers/models/llama/modeling_llama.py文件
patch -p1 < openvla.patch
```

## 单机八卡训练
### 设置训练路径与参数
```bash
cd /home/openvla
# 根据实际数据与代码路径进行修改
# 修改模型vision backbone本地路径
vim prismatic/models/backbones/vision/dinosiglip_vit.py
# 修改模型llm backbone本地路径 
vim prismatic/models/backbones/llm/llama2.py

# 修改模型其他参数配置
# 默认使用混合精度训练，如不需要使用BF16，修改enable_mixed_precision_training参与为False
vim prismatic/conf/vla.py
```

### 执行sft
```bash
bash full_sft_train.sh
```

### sft参考配置
```bash
export CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7
export XMLIR_BMM_DISPATCH_VALUE=2
export BKCL_PCIE_TOPO=1
export CUDART_DUMMY_REGISTER=1
unset CUDA_LAUNCH_BLOCKING
#export XMLIR_ENABLE_NEW_PG=1
export XMLIR_ENABLE_FAST_FC=1
export XDNN_USE_FAST_GELU=1
export XDNN_USE_FAST_SWISH=1
export XDNN_FAST_DIV_SCALAR=true
export XPUAPI_SDNN_BF16_ROUND_MODE=3
export XBLAS_FC_AUTOTUNE_FILE="openvla_tune.txt"

export BKCL_ENABLE_TREE=1
export BKCL_RDMA_VERBS=1
export BKCL_RING_BUFFER_SIZE=8388608
export BKCL_MULTI_TREE_THRESHOLD=-1

export BKCL_RDMA_PROXY_DISABLE=1 # 屏蔽旧架构
export BKCL_USE_AR=1
export BKCL_RING_OPT=1
export BKCL_FLAT_RING=1
export BKCL_TREE_THRESHOLD=1
export BKCL_CCIX_BUFFER_GM=1
export BKCL_FORCE_L3_RDMA=0
export BKCL_ENABLE_XDR=1
export BKCL_RDMA_FORCE_TREE=1
export BKCL_RDMA_NICS=enP1s4f0,enP1s4f0,enP1s4f0,enP1s4f0,enP1s4f1,enP1s4f1,enP1s4f1,enP1s4f1

export ALLREDUCE_ASYNC=false
export ALLGATHER_ASYNC=false
export ALLREDUCE_FUSION=0
export BKCL_TIMEOUT=360000

export WANDB_MODE=offline
export WANDB_API_KEY="" ###请替换成个人WANDB_API_KEY

torchrun --standalone --nnodes 1 --nproc-per-node 8 vla-scripts/train.py \
  --pretrained_checkpoint /home/openvla-7b-prismatic/checkpoints/step-295000-epoch-40-loss=0.2200.pt \
  --vla.type prism-dinosiglip-224px+mx-bridge \
  --data_root_dir /home/openvla_data \
  --run_root_dir outputs \
  --image_aug True \
  --save_interval 1000 \
  --is_resume False \
  --wandb_project openvla_test
```
