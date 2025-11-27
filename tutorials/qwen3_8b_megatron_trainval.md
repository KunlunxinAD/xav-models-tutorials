# Qwen3-8B Megatron Trainval Guide

## 环境准备
### 准备开发环境镜像
请联系相关人员获取开发环境镜像

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
wget https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/modelzoo_data/data/aiak_megatron_core/qwen3_data.tar.gz
tar -xzvf qwen3_data.tar.gz
```

### 下载代码及预训练权重
```bash
# 下载qwen3 tokenizer
cd /home
wget https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/modelzoo_data/data/Qwen3-32B.tar.gz
tar -xzvf Qwen3-32B.tar.gz

# 下载megatron
wget https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/AIAK-Megatron.tar
tar -xvf AIAK-Megatron.tar

# 下载megatron-llm
wget https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/AIAK-Training-LLM.tar
tar -xvf AIAK-Training-LLM.tar
```

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export LLAMA_CONTAINER=Qwen3_8B_test
export MODEL_PATH=</path/to/qwen3_8b> #本地路径
 
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

# 更新环境
### 更新xmilr 依赖
wget https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/xpytorch-cp310-torch251-ubuntu2004-x64.run
bash xpytorch-cp310-torch251-ubuntu2004-x64.run

### 更新其他依赖
pip install numpy==1.26.4 

# 修改代码配置
vim AIAK-Megatron/megatron/training/utils.py
megatron/training/utils.py:471 'attention_mask': None if "attention_mask" not in data else data["attention_mask"].cuda(non_blocking=True) ，将non_blocking改为False
```

# 单机多卡训练
### 设置训练路径与参数
```bash
# tp8-pp1-dp1-gbs1-mbs1 启动脚本
## 根据实际情况修改代码与数据路径、训练参数
vim pretrain_tp8_pp1_dp1_gbs1_mbs1.sh
```

### 参考脚本配置
```bash
#! /bin/bash
# set -x

MEGATRON_PATH=${MEGATRON_PATH:-"/workdir/AIAK-Megatron"}
AIAK_TRAINING_PATH=${AIAK_TRAINING_PATH:-"/workdir/AIAK-Training-LLM"}
DATA_PATH=${DATA_PATH:-"/klxlake/team/platform_software/qa_ipipe/model_zoo/datasets/qwen3-32b/deepseek_content_document/deepseek_content_document"}
TOKENIZER_PATH=${TOKENIZER_PATH:-"/workdir/Qwen3-32B"}

export DIST_MULTI_STREAM=true # 开启多流
export CUDA_DEVICE_MAX_CONNECTIONS=1
unset CUDA_LAUNCH_BLOCKING
export XMLIR_DISABLE_CUDA_ALLOCATOR=true
export XPU_FORCE_USERMODE_LAUNCH=1 
unset XPU_DUMMY_EVENT

export CUDA_DEVICE_ORDER=OAM_ID # 用于通讯建环
export XBLAS_FC_HBM_VERSION=40 # 需要指定HBM版
export XDNN_HIGH_ACCRACY_FP32_TO_BF16=true

export BKCL_TREE_THRESHOLD=1048576
export BKCL_CCIX_BUFFER_GM=1
export BKCL_ENABLE_XDR=1
export BKCL_FLAT_RING=1 
export BKCL_RDMA_PROXY_DISABLE=1 
export BKCL_XLINK_D2D=0 
export BKCL_XLINK_ETH=0 
export BKCL_RDMA_FORCE_TREE=1 
export XMLIR_DIST_ASYNC_ISEND_IRECV=1

export XMLIR_PARALLEL_SAVE_MEMORY=false # 为false显存占用会多, 但会有性能提升; 为true显存会少, 但性能会下降
export XMLIR_BATCH_PARALLEL=true 
export USE_FAST_BF16_FC=true 
export USE_CAST_FC_FUSION=true 
export SAVE_LOG_FILE_WITH_RANK_ID=true # 为true的话, 训练日志会按rank_id分开存储
export P800_DEBUG=false # 为true的话, 训练grad norm出nan会保存ckpt后退出
export XMLIR_USE_GPU_AIAK_LAYER_SPEC=1 

export XMLIR_CUDNN_ENABLED=1 
export XMLIR_ENABLE_LINEAR_FC_FUSION=1 
export XDNN_FC_GEMM_DTYPE=int32_with_ll 
export XMLIR_FUSED_SDP_CHOICE=1
export FAST_SWIGLU_ENABLE=1

# 需要根据实际机器的配置修改
export BKCL_RDMA_NICS=ens11np0,ens11np0,ens13np0,ens13np0,ens15np0,ens15np0,ens17np0,ens17np0
export BKCL_SOCKET_IFNAME=ens20f0np0

CUDA_VISIBLE_DEVICES=${CUDA_VISIBLE_DEVICES:-"0,1,2,3,4,5,6,7"}
GPUS_PER_NODE=`echo "$CUDA_VISIBLE_DEVICES" | awk -F, '{print NF}'`

# Change for multinode config
MASTER_ADDR=${MASTER_ADDR:-"localhost"}
# MASTER_ADDR=${MASTER_ADDR:-"10.129.130.223"}
MASTER_PORT=${MASTER_PORT:-"6000"}
NNODES=${WORLD_SIZE:-"1"}
NODE_RANK=${RANK:-"0"}

DISTRIBUTED_ARGS=(
    --nproc_per_node $GPUS_PER_NODE
    --nnodes $NNODES
    --node_rank $NODE_RANK
    --master_addr $MASTER_ADDR
    --master_port $MASTER_PORT
)

MODEL_ARGS=(
    --model-name qwen3-8b
    --rotary-base 1000000
    --rotary-seq-len-interpolation-factor 1
)

DATA_ARGS=(
    --tokenizer-type HFTokenizer
    --hf-tokenizer-path $TOKENIZER_PATH
    --eod-mask-loss
    --data-path $DATA_PATH
    --split 99,1,0
)

TRAINING_ARGS=(
    --use-cpu-initialization
    --training-phase pretrain # options: pretrain, sft
    --seq-length 32768
    --max-position-embeddings 32768
    --init-method-std 0.006
    --micro-batch-size 1
    --global-batch-size 1
    --lr 1.0e-5
    --min-lr 1.0e-6
    --clip-grad 1.0
    --weight-decay 0.1
    --optimizer adam
    --adam-beta1 0.9
    --adam-beta2 0.95
    --adam-eps 1e-08
    --norm-epsilon 1e-6
    --train-iters 50000
    --lr-decay-iters 50000
    --lr-decay-style cosine
    --lr-warmup-fraction 0.002
    --initial-loss-scale 65536
    --bf16
    # --load $CHECKPOINT_PATH
    # --save $CHECKPOINT_PATH
    --save-interval 5000
    --eval-interval 1000
    --eval-iters 10
    --exit-interval 20
    #--ckpt-step 0
    #--no-load-optim
    #--no-load-rng
    #--num-workers 8
)

MODEL_PARALLEL_ARGS=(
    --tensor-model-parallel-size 8
    --pipeline-model-parallel-size 1
    --use-distributed-optimizer
    --overlap-grad-reduce
    --overlap-param-gather
    --distributed-backend nccl
    --sequence-parallel            # tp>1时开启
    # --tp-comm-overlap #没有使用tp并行就不能启用，否则报错
    --tp-comm-overlap-bootstrap-backend nccl # or: gloo, mpi
    # --offload-optimizer auto
)

LOGGING_ARGS=(
    --log-interval 1
    # --tensorboard-dir ${TENSORBOARD_PATH}
    # --log-timers-to-tensorboard
)

if [ -n "${WANDB_API_KEY}" ]; then
    LOGGING_ARGS+=(
        --wandb-project ${WANDB_PROJECT}
        --wandb-exp-name ${WANDB_NAME} 
    )
fi

XFLAGS --reset
export XMLIR_MEGATRON_CORE_AIAK_PLUGIN=1

PYTHONPATH=$MEGATRON_PATH:$AIAK_TRAINING_PATH:$PYTHONPATH \
    torchrun ${DISTRIBUTED_ARGS[@]} \
    $AIAK_TRAINING_PATH/aiak_training_llm/train.py \
    ${MODEL_ARGS[@]} \
    ${DATA_ARGS[@]} \
    ${TRAINING_ARGS[@]} \
    ${MODEL_PARALLEL_ARGS[@]} \
    ${LOGGING_ARGS[@]}
```

### 执行pretrain
```bash
bash pretrain_tp8_pp1_dp1_gbs1_mbs1.sh
```

