# Qwen3-30B-A3B Pretrain Guide

## 环境准备
请联系相关人员获取开发环境镜像

### 启动容器

```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=qwen3-30b-a3b-megatron-pretrain-test

docker run -itd --privileged --net=host \
    --security-opt=seccomp=unconfined \
    -v `pwd`:/workspace \
    -w /workspace \
    --name ${XAV_CONTAINER} \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --shm-size 256g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${XAV_CONTAINER} bash
```

### 安装xmegatron_ext

```bash
wget https://klx-sdk-release-public.su.bcebos.com/v1/XMegatronExtension/release/release_20260130/xme-20260131.dev4%2Bged343f780.tar.gz
tar -xvf xme-20260131.dev4+ged343f780.tar.gz && cd xme
pip install xmegatron_ext-20260131.dev4+ged343f780-py3-none-any.whl
```

### 安装其他依赖

```bash
pip install transformers==4.51.0
pip install megatron-energon==6.0 
pip install wandb
pip install filetype 
pip install bitstring 
pip install ebmlite 
pip install sortedcontainers 
pip install av 
pip install soundfile
pip install qwen_vl_utils -i http://mirrors.baidubce.com/pypi/simple/ --trusted-host mirrors.baidubce.com
pip install colorlog
```

## 模型准备

### 数据集下载

```bash
wget https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/modelzoo_data/data/aiak_megatron_core/qwen3_data_content_document.tar.gz
tar -xvf qwen3_data_content_document.tar.gz
```

### 权重下载

从huggingface上下载Qwen/Qwen3-30B-A3B的权重

### megatron代码准备

```bash
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/xmegatron/3.5.0.0_117/KLX-LLM.tar.gz
tar -xvf KLX-LLM.tar.gz
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/xmegatron/3.5.0.0_117/Megatron-LM.tar.gz
tar -xvf Megatron-LM.tar.gz
```

## 模型训练

### pretrain

1. 修改qwen3_30b_a3b_pretrain.sh
```bash
cd <your_file_path>/KLX-LLM/examples/qwen3
vim qwen3_30b_a3b_pretrain.sh
```
qwen3_30b_a3b_pretrain.sh的内容，路径需根据实际情况进行替换。
```bash
#!/bin/bash
set -eox pipefail

ipcs -m | awk '$4 == 666 {print $2}' | while read shmid; do
    ipcrm -m $shmid
    echo "Deleted shared memory segment with ID: $shmid"
done

source ../xpu_env.sh

ROOT_DIR=$( dirname -- "$( readlink -f -- "$0"; )"; )
ROOT_DIR=$(realpath ${ROOT_DIR}/../../)
echo $ROOT_DIR

SEQ_LEN_MODE=${SEQ_LEN_MODE:-4k}

if [[ $SEQ_LEN_MODE == "4k" ]]; then
    # base config
    PRESET_GLOBAL_BATCH_SIZE=128
    PRESET_SEQ_LEN=4096
    # parallel config
    PRESET_TP=2
    PRESET_PP=2
    PRESET_EP=2
fi

WORKSPACE_DIR=${WORKSPACE_DIR:-/workspace}
export PYTHONPATH=${PYTHONPATH}:${WORKSPACE_DIR}/Megatron-LM/${MCORE_VERSION:-core_r0_12_0}:${ROOT_DIR}/Megatron-LM/${MCORE_VERSION:-core_r0_12_0}:${ROOT_DIR}/megatron_patch/${MEGATRON_PATCH_VERSION:-megatron_patch_250328}
export XTE_DISABLE_EXTRA_STATE=${XTE_DISABLE_EXTRA_STATE:-1}
# 兼容旧版本权重加载
export XME_LOAD_CKPT_UNSTRICT=1
# mcore012 关掉largeweight
export XTE_GROUPED_GEMM_LARGE_WEIGHT=0

### BASE CONFIG ###
MODEL_SIZE=A3B
BATCH_SIZE=1
GLOBAL_BATCH_SIZE=${GLOBAL_BATCH_SIZE:-${PRESET_GLOBAL_BATCH_SIZE}}
LR=1e-5
MIN_LR=1e-6
SEQ_LEN=${SEQ_LEN:-${PRESET_SEQ_LEN}}
PAD_LEN=${SEQ_LEN}
PR=${PR:-bf16}
TRAIN_ITERS=${TRAIN_ITERS:-100}
### BASE CONFIG ###

### PARALLEL / BOOL OPTION ###
TP=${TP:-${PRESET_TP}}
PP=${PP:-${PRESET_PP}}
EP=${EP:-${PRESET_EP}}
ETP=${ETP:-1}
CP=1
SP=true
DO=true
FL=true
SFT=false
# MP_PP0_LAYERS=${MP_PP0_LAYERS:-11}
# MP_PP_LAST_LAYERS=${MP_PP_LAST_LAYERS:-13}
MP_PP0_LAYERS=0
### PARALLEL / BOOL OPTION ###

### OTHERS ###
AC=${AC:-full}
RECOMPUTE_METHOD=${RECOMPUTE_METHOD:-block}
MP_AC_LAYERS=${MP_AC_LAYERS:-8}
OPTIMIZER_OFFLOAD=${OPTIMIZER_OFFLOAD:-false}
SAVE_INTERVAL=${SAVE_INTERVAL:-5}
STORAGE_PATH=${STORAGE_PATH:-/workspace/storage/}
PRETRAIN_CHECKPOINT_PATH=${PRETRAIN_CHECKPOINT_PATH:-"/home/Qwen3/Qwen3-30B-A3B"}
DATASET_PATH=${DATASET_PATH:-"/home/qwen3_data_content_document/qwen3_data_content_document"}
VALID_DATASET_PATH=${DATASET_PATH}

MP_SFT_PACKING=false
CPT_CONTINUE=${CPT_CONTINUE:-false}
SAVE_CKPT=${SAVE_CKPT:-true}
###############################

if [[ -z ${OUTPUT_DIR} ]];then
    OUTPUT_BASEPATH=${ROOT_DIR}/output
else
    OUTPUT_BASEPATH=${OUTPUT_DIR}
fi

# Change for multinode config
MASTER_ADDR=${MASTER_ADDR:-"localhost"}
MASTER_PORT=${MASTER_PORT:-"6000"}
NNODES=${WORLD_SIZE:-"1"}
NODE_RANK=${RANK:-"0"}
GPUS_PER_NODE=8

DISTRIBUTED_ARGS=(
    --nproc_per_node $GPUS_PER_NODE
    --nnodes $NNODES
    --node_rank $NODE_RANK
    --master_addr $MASTER_ADDR
    --master_port $MASTER_PORT
)

DISTRIBUTED_ARGS="--nproc_per_node $GPUS_PER_NODE --nnodes $NNODES --node_rank $NODE_RANK --master_addr $MASTER_ADDR --master_port $MASTER_PORT"

if [ $FL = true ]; then
    export NVTE_FLASH_ATTN=1 NVTE_FUSED_ATTN=0
    attn_backend_option=" \
        --attention-backend flash \
    "
elif [ $FL = false ]; then
    export NVTE_FLASH_ATTN=0 NVTE_FUSED_ATTN=1
    attn_backend_option=" \
        --attention-backend fused \
    "
fi

if [ $MODEL_SIZE = 0.6B ]; then
    NUM_LAYERS=28
    HIDDEN_SIZE=1024
    NUM_ATTENTION_HEADS=16
    INTERMEDIATE_SIZE=3072
    NUM_KEY_VALUE_HEADS=8
    MAX_POSITION_EMBEDDINGS=40960
    EXTRA_VOCAB_SIZE=293
    ROPE_THETA=1000000
    RMS_NORM_EPS=1e-6
    gqa_options=" \
                --group-query-attention \
                --num-query-groups ${NUM_KEY_VALUE_HEADS}"

    tie_option=""
    moe_options=""
elif [ $MODEL_SIZE = 1.7B ]; then
    NUM_LAYERS=28
    HIDDEN_SIZE=2048
    NUM_ATTENTION_HEADS=16
    INTERMEDIATE_SIZE=6144
    NUM_KEY_VALUE_HEADS=8
    MAX_POSITION_EMBEDDINGS=40960
    EXTRA_VOCAB_SIZE=293
    ROPE_THETA=1000000
    RMS_NORM_EPS=1e-6
    gqa_options=" \
                --group-query-attention \
                --num-query-groups ${NUM_KEY_VALUE_HEADS}"

    tie_option=""
    moe_options=""
elif [ $MODEL_SIZE = 4B ]; then
    NUM_LAYERS=36
    HIDDEN_SIZE=2560
    NUM_ATTENTION_HEADS=32
    INTERMEDIATE_SIZE=9728
    NUM_KEY_VALUE_HEADS=8
    MAX_POSITION_EMBEDDINGS=40960
    EXTRA_VOCAB_SIZE=293
    ROPE_THETA=1000000
    RMS_NORM_EPS=1e-6
    gqa_options=" \
                --group-query-attention \
                --num-query-groups ${NUM_KEY_VALUE_HEADS}"

    tie_option=""
    moe_options=""
elif [ $MODEL_SIZE = 8B ]; then
    NUM_LAYERS=36
    HIDDEN_SIZE=4096
    NUM_ATTENTION_HEADS=32
    INTERMEDIATE_SIZE=12288
    NUM_KEY_VALUE_HEADS=8
    MAX_POSITION_EMBEDDINGS=40960
    EXTRA_VOCAB_SIZE=293
    ROPE_THETA=1000000
    RMS_NORM_EPS=1e-6
    gqa_options=" \
                --group-query-attention \
                --num-query-groups ${NUM_KEY_VALUE_HEADS}"

    tie_option=" \
            --untie-embeddings-and-output-weights \
            "
    moe_options=""
elif [ $MODEL_SIZE = 14B ]; then
    NUM_LAYERS=40
    HIDDEN_SIZE=5120
    NUM_ATTENTION_HEADS=40
    INTERMEDIATE_SIZE=17408
    NUM_KEY_VALUE_HEADS=8
    MAX_POSITION_EMBEDDINGS=40960
    EXTRA_VOCAB_SIZE=293
    ROPE_THETA=1000000
    RMS_NORM_EPS=1e-6
    gqa_options=" \
                --group-query-attention \
                --num-query-groups ${NUM_KEY_VALUE_HEADS}"

    tie_option=" \
            --untie-embeddings-and-output-weights \
            "

    moe_options=""
elif [ $MODEL_SIZE = 32B ]; then
    NUM_LAYERS=64
    HIDDEN_SIZE=5120
    NUM_ATTENTION_HEADS=64
    INTERMEDIATE_SIZE=25600
    NUM_KEY_VALUE_HEADS=8
    MAX_POSITION_EMBEDDINGS=40960
    EXTRA_VOCAB_SIZE=293
    ROPE_THETA=1000000
    RMS_NORM_EPS=1e-6
    gqa_options=" \
                --group-query-attention \
                --num-query-groups ${NUM_KEY_VALUE_HEADS}"

    tie_option=" \
            --untie-embeddings-and-output-weights \
            "

    moe_options=""
elif [ $MODEL_SIZE = A3B ]; then
    HIDDEN_SIZE=2048
    NUM_ATTENTION_HEADS=32
    NUM_LAYERS=24
    INTERMEDIATE_SIZE=6144
    MOE_INTERMEDIATE_SIZE=768
    MAX_POSITION_EMBEDDINGS=40960
    EXTRA_VOCAB_SIZE=293
    NUM_KEY_VALUE_HEADS=4
    ROPE_THETA=1000000
    NUM_EXPERTS=128
    ROUTER_TOPK=8
    RMS_NORM_EPS=1e-6


    moe_options=" \
        --moe-token-dispatcher-type ${MOE_TOKEN_DISPATCHER_TYPE:-allgather} \
        --moe-router-topk ${ROUTER_TOPK} \
        --num-experts ${NUM_EXPERTS} \
        --expert-tensor-parallel-size ${ETP} \
        --expert-model-parallel-size ${EP} \
        --moe-ffn-hidden-size ${MOE_INTERMEDIATE_SIZE} \
        --moe-router-load-balancing-type aux_loss \
        --moe-aux-loss-coeff 0.001 \
        --moe-layer-freq '([1]*24)' \
        "

    tie_option=" \
            --untie-embeddings-and-output-weights \
            "

    gqa_options=" \
                --group-query-attention \
                --num-query-groups ${NUM_KEY_VALUE_HEADS}"
elif [ $MODEL_SIZE = A22B ]; then
    HIDDEN_SIZE=4096
    NUM_ATTENTION_HEADS=64
    NUM_LAYERS=94
    INTERMEDIATE_SIZE=12288
    MOE_INTERMEDIATE_SIZE=1536
    MAX_POSITION_EMBEDDINGS=40960
    EXTRA_VOCAB_SIZE=293
    NUM_KEY_VALUE_HEADS=4
    ROPE_THETA=1000000
    NUM_EXPERTS=128
    ROUTER_TOPK=8
    RMS_NORM_EPS=1e-6

    moe_options=" \
        --moe-token-dispatcher-type ${MOE_TOKEN_DISPATCHER_TYPE:-allgather} \
        --moe-router-topk ${ROUTER_TOPK} \
        --num-experts ${NUM_EXPERTS} \
        --expert-tensor-parallel-size ${ETP} \
        --expert-model-parallel-size ${EP} \
        --moe-ffn-hidden-size ${MOE_INTERMEDIATE_SIZE} \
        --moe-router-load-balancing-type aux_loss \
        --moe-aux-loss-coeff 0.001 \
        --moe-layer-freq '([1]*94)' \
        --moe-router-pre-softmax
        "

    tie_option=" \
            --untie-embeddings-and-output-weights \
            "

    gqa_options=" \
                --group-query-attention \
                --num-query-groups ${NUM_KEY_VALUE_HEADS}"
fi


# Here are some configs controled by env
# MP_DATASET_TYPE 决定模型如何读取和解析训练数据的关键参数
if [ -z ${MP_DATASET_TYPE} ];then
    MP_DATASET_TYPE="idxmap"
fi

# MP_AC_LAYERS环境变量，来控制Checkpointing或Offload的TransformerLayer层数（默认值：1）。
if [ -z ${MP_AC_LAYERS} ];then
    MP_AC_LAYERS=1
fi

# Virtual Pipeline (VP)，也就是 Interleaved Pipeline Parallelism（交错式流水线并行）
if [ -z ${MP_VP} ]; then
    vp_option=""
else
    vp_option=" \
        --num-layers-per-virtual-pipeline-stage ${MP_VP}"
fi

if [ -z ${MP_SFT_PACKING} ]; then
    MP_SFT_PACKING=false
fi

# 通信与计算重叠（Communication Overlap）
TP_COMM_OVERLAP=$(( ($TP > 1) ? 1 : 0 ))
#comm_overlap_option="\
#    --overlap-grad-reduce \
#    --overlap-param-gather"

comm_overlap_option=""
#if [ $TP_COMM_OVERLAP -eq 1 ]; then
#    comm_overlap_option="\
#        --tp-comm-overlap \
#        --overlap-grad-reduce \
#        --overlap-param-gather"
#fi

# 这段代码是训练脚本中处理**显存优化（Memory Optimization）**最核心的部分。它根据你设定的 AC（Activation Checkpointing，激活重计算）模式，动态配置训练参数。
if [ $AC = full ]; then
    _check=$(( ($NUM_LAYERS / $PP) % ${MP_AC_LAYERS} ))
    if [ $_check != 0 ]; then
        echo "the num layers per pp rank must be a multiple of the recompute layers."
        # exit -1
    fi
    activation_checkpoint_options=" \
                    --recompute-method ${RECOMPUTE_METHOD} \
            --recompute-num-layers ${MP_AC_LAYERS} \
                    --recompute-granularity full"
elif [ $AC = sel ]; then
    activation_checkpoint_options=" \
        --recompute-activations"
elif [ $AC = none ]; then
    activation_checkpoint_options=" \
    "
elif [ $AC = offload ]; then
    activation_checkpoint_options=" \
                    --cpu-offloading \
                    --cpu-offloading-num-layers ${MP_AC_LAYERS}"
    if [ $TP_COMM_OVERLAP -eq 1 ]; then
        echo "Disable --overlap-grad-reduce and --overlap-param-gather when cpu offloading is on..."
        comm_overlap_option="\
            --tp-comm-overlap"
    else
        echo "Disable --overlap-grad-reduce and --overlap-param-gather when cpu offloading is on..."
        comm_overlap_option=""
    fi
fi

if [ $PR = fp16 ]; then
    pr_options=" \
                    --fp16 \
            --apply-query-key-layer-scaling"
    export NVTE_APPLY_QK_LAYER_SCALING=1
elif [ $PR = bf16 ]; then
    pr_options=" \
        --bf16"
elif [ $PR = fp8 ]; then
    pr_options=" \
        --bf16 \
        --fp8-format hybrid \
        --fp8-amax-compute-algo max \
        --fp8-amax-history-len 1024"
fi

# 显存节省技术（Optimizer Offload） 与 并行训练模式（Distributed Optimizer）
if [ $OPTIMIZER_OFFLOAD != false ] && [ $DO = false ]; then
    echo "Offload optimizer is valid only if \$DO=true"
    DO=true
fi

if [ $DO = true ]; then
    do_option=" \
                    --use-distributed-optimizer"

elif [ $DO = false ]; then
    do_option=" \
                    "
fi

# Sequence Parallelism (SP，序列并行)
if [ $SP = true ] && [ $TP -gt 1 ]; then
    sp_option=" \
                    --sequence-parallel"

elif [ $SP = false ]; then
    sp_option=" \
                    "
fi

# 流水线并行（Pipeline Parallelism）中的非均匀层数切分（Uneven Layer Split）。
if [ -z ${MP_PP0_LAYERS} ];then
    uneven_split_option=""
elif [ ${PP} -gt 1 ]; then
    _check=$(( ( $NUM_LAYERS - ${MP_PP0_LAYERS} ) % ( ${PP} - 1 ) ))
    if [ $_check != 0 ]; then
        echo "With uneven pipelineing the left over layers must be divisible by left over stages."
    fi

    if [ ${MP_PP0_LAYERS:-0} -gt 0 ]; then
        uneven_split_option=" \
            --decoder-first-pipeline-num-layers ${MP_PP0_LAYERS} \
        "
    fi
    if [ ${MP_PP_LAST_LAYERS:-0} -gt 0 ]; then
        uneven_split_option=" \
            ${uneven_split_option} \
            --decoder-last-pipeline-num-layers ${MP_PP_LAST_LAYERS} \
        "
    fi
else
    echo "uneven pipeline split must be used when PP > 1"
fi

if [ $OPTIMIZER_OFFLOAD != false ]; then
    offload_option=" \
        --optimizer adam \
        --optimizer-cpu-offload \
        --optimizer-offload-policy static \
        --overlap-cpu-optimizer-d2h-h2d \
        --use-precision-aware-optimizer \
        --optimizer-offload-fraction ${OPTIMIZER_OFFLOAD_FRACTION:-1.0} "
fi

if [ $SFT = true ]; then
    TASK="sft"
    sft_options=" \
         --eod-mask-loss \
         --calculate-per-token-loss \
         --train-mode finetune"
else
    #TRAIN_ITERS=$(( ${TRAIN_TOKENS} / ${GLOBAL_BATCH_SIZE} / ${SEQ_LEN} ))
    #LR_WARMUP_ITERS=$(( ${WARMUP_TOKENS}  / ${GLOBAL_BATCH_SIZE} / ${SEQ_LEN} ))
    #LR_DECAY_ITERS=$(( ${TRAIN_TOKENS} /  ${GLOBAL_BATCH_SIZE} / ${SEQ_LEN} ))
    TASK="pretrain"
    sft_options=" \
        --train-mode pretrain"
fi

if [ ${MP_DATASET_TYPE} = "raw" ]; then
    dataset_options=" \
        --train-data-path ${DATASET_PATH} \
        --valid-data-path ${VALID_DATASET_PATH} \
        --dataloader-type cyclic \
        --dataset JSON-SFT "
else
    dataset_options=" \
        --data-path ${DATASET_PATH} \
        --dataset MMAP \
        --split 99,1,0 "
fi

if [ ${MP_SFT_PACKING} = true ]; then
    packing_options=" \
      --reset-position-ids \
      --no-create-attention-mask-in-dataloader "
else
    packing_options=""
fi

##### Prepare logdirs #######
CURRENT_TIME=$(date +"%m-%d-%H-%M")
TASK_NAME="mcore-qwen3-${MODEL_SIZE}-${TASK}"
DETAIL_TASK_NAME="${TASK_NAME}-lr-${LR}-minlr-${MIN_LR}-bs-${BATCH_SIZE}-gbs-${GLOBAL_BATCH_SIZE}-seqlen-${SEQ_LEN}-pr-${PR}-tp-${TP}-pp-${PP}-ep-${EP}-etp-${ETP}-cp-${CP}-ac-${AC}-do-${DO}-sp-${SP}/${CURRENT_TIME}${LABEL}"
TENSORBOARD_DIR="${OUTPUT_BASEPATH}/${DETAIL_TASK_NAME}"
SAVED_PRETRAIN_CHECKPOINT_PATH=${SAVED_PRETRAIN_CHECKPOINT_PATH:-${OUTPUT_BASEPATH}/checkpoint/${TASK_NAME}-TP${TP}-PP${PP}}
LOG_DIR=${OUTPUT_BASEPATH}/${DETAIL_TASK_NAME}
LOG_NAME="${NODE_RANK}.txt"
CACHE_DIR=${OUTPUT_BASEPATH}/cache/${WORLD_SIZE}_${TP}_${PP}_${EP}_${ETP}_${CP}_${SP}_${DO}_${PR}_${AC}_${MP_DATASET_TYPE}/${TASK_NAME}-$(date +"%m-%d-%H")

mkdir -p ${SAVED_PRETRAIN_CHECKPOINT_PATH}
mkdir -p ${LOG_DIR}
mkdir -p ${TENSORBOARD_DIR}
mkdir -p ${CACHE_DIR}

data_cache_options=" \
        --data-cache-path ${CACHE_DIR} \
    "

if [ $SAVE_CKPT = true ]; then
    save_ckpt_options=" \
        --save ${SAVED_PRETRAIN_CHECKPOINT_PATH} \
        --save-interval ${SAVE_INTERVAL} \
        --ckpt-format torch_dist "
fi

if [ -z ${CPT_CONTINUE} ] || [ ${CPT_CONTINUE} = false ]; then
    cpt_continue_options="\
     --no-load-optim \
     --no-load-rng "
elif [ ${CPT_CONTINUE} = true ];  then
    cpt_continue_options="\
        --no-load-rng "
fi

if [ -e  ${SAVED_PRETRAIN_CHECKPOINT_PATH}/latest_checkpointed_iteration.txt ]; then
    echo "${SAVED_PRETRAIN_CHECKPOINT_PATH}/latest_checkpointed_iteration.txt 文件存在"
    if [ ${CPT_CONTINUE} = true ];  then
        PRETRAIN_CHECKPOINT_PATH=${SAVED_PRETRAIN_CHECKPOINT_PATH}
    fi
else
    echo "${SAVED_PRETRAIN_CHECKPOINT_PATH} :文件夹为空"
    find -L ${PRETRAIN_CHECKPOINT_PATH} -maxdepth 1 -type f -name "*.json" -print0 | xargs -0 cp -v -f -t ${SAVED_PRETRAIN_CHECKPOINT_PATH}
    cp -v -f ${PRETRAIN_CHECKPOINT_PATH}/merges.txt ${SAVED_PRETRAIN_CHECKPOINT_PATH}
fi

if [ $PRETRAIN_CHECKPOINT_PATH != none ]; then
    load_options=" \
            --load $PRETRAIN_CHECKPOINT_PATH \
            --auto-detect-ckpt-format"
fi

megatron_options="  \
        --lr ${LR} \
        --min-lr ${MIN_LR} \
        --lr-decay-style cosine \
        --weight-decay 0.01 \
        --adam-beta1 0.9 \
        --adam-beta2 0.95 \
        --clip-grad 1.0 \
        --init-method-std 0.008 \
        --attention-dropout 0.0 \
        --hidden-dropout 0.0 \
        --lr-warmup-fraction 0.2 \
        --train-iters ${TRAIN_ITERS} \
        --micro-batch-size ${BATCH_SIZE} \
        --global-batch-size ${GLOBAL_BATCH_SIZE} \
        --num-layers ${NUM_LAYERS} \
        --hidden-size ${HIDDEN_SIZE} \
        --num-attention-heads ${NUM_ATTENTION_HEADS} \
        --ffn-hidden-size ${INTERMEDIATE_SIZE} \
        --seq-length ${SEQ_LEN} \
        --max-position-embeddings ${MAX_POSITION_EMBEDDINGS} \
        --max-padding-length ${PAD_LEN} \
        --log-interval 1 \
        --log-throughput \
        --eval-interval 10000 \
        --eval-iters 0 \
        --tensorboard-queue-size 1 \
        --tensorboard-dir ${TENSORBOARD_DIR} \
        --log-timers-to-tensorboard \
        --log-validation-ppl-to-tensorboard \
        --tensor-model-parallel-size ${TP} \
        --pipeline-model-parallel-size ${PP} \
        --context-parallel-size ${CP} \
        --num-workers 8 \
        --extra-vocab-size ${EXTRA_VOCAB_SIZE} \
        --patch-tokenizer-type Qwen3Tokenizer \
        --swiglu \
        --normalization RMSNorm \
        --norm-epsilon ${RMS_NORM_EPS} \
        --use-rotary-position-embeddings \
        --position-embedding-type rope \
        --disable-bias-linear \
        --rotary-base ${ROPE_THETA} \
        --ckpt-format torch_dist \
        --transformer-impl transformer_engine \
        --cross-entropy-loss-fusion \
        --no-rope-fusion \
        --qk-layernorm \
        --kv-channels 128 \
        --use-cpu-initialization \
        --no-create-attention-mask-in-dataloader \
        --no-gradient-accumulation-fusion \
        --log-mfu \
        --mfu-base-value 312 \
        --log-memory-to-tensorboard \
        --log-token-throughput \
        "

if [ ${ENABLE_GROUPED_GEMM:-true} = true ]; then
    moe_options=" \
        --moe-grouped-gemm \
        ${moe_options} \
    "
fi

if [ ${ENABLE_DEEPEEP:-false} = true ]; then
    moe_options=" \
        --moe-enable-deepep \
        --moe-router-dtype=fp32 \
        ${moe_options} \
    "
fi

if [[ -z ${LOG_FILE} ]];then
  LOG_FILE=${LOG_DIR}/${LOG_NAME}
fi

run_cmd="env;torchrun $DISTRIBUTED_ARGS pretrain_qwen.py
 ${megatron_options} \
 ${dataset_options} \
 ${pr_options} \
 ${load_options} \
 ${activation_checkpoint_options} \
 ${do_option} \
 ${sp_option} \
 ${moe_options} \
 ${offload_option} \
 ${sft_options} \
 ${vp_option} \
 ${packing_options} \
 ${uneven_split_option} \
 ${attn_backend_option} \
 ${cpt_continue_options} \
 ${save_ckpt_options} \
 ${gqa_options} \
 ${tie_option} \
 ${data_cache_options} \
 2>&1 | tee -a ${LOG_FILE}
 "

echo ${run_cmd} | tee -a ${LOG_FILE}
eval ${run_cmd}
status=$?
[ $status -ne 0 ] && dmesg -T >> ${LOG_DIR}/dmesg_${RANK}.log
exit $status
```

2. 修改xpu_env.sh
```bash
cd <your_file_path>/KLX-LLM/examples
vim xpu_env.sh
```
xpu_env.sh的内容
```bash
# XTE
export XMLIR_ENABLE_FAST_FC=1

# XFA
export XFA_GEMM_TYPE=float16
export XFA_BWD_USE_DS_SCALE=1

# XPU相关参数
export CUDA_DEVICE_ORDER=OAM_ID
#################################
export XMLIR_DIST_SINGLETON_STREAM=true
export DIST_MULTI_STREAM=${DIST_MULTI_STREAM:-true}
export CUDA_DEVICE_MAX_CONNECTIONS=${CUDA_DEVICE_MAX_CONNECTIONS:-8}
export CUDA_VISIBLE_DEVICES=${CUDA_VISIBLE_DEVICES:-"0,1,2,3,4,5,6,7"}
export XMLIR_FA_GEMM_TYPE=float16
export XMLIR_PARALLEL_SAVE_MEMORY=${XMLIR_PARALLEL_SAVE_MEMORY:-false}
#export XMLIR_DISABLE_CUDA_ALLOCATOR=true

#################################
export XMLIR_DIST_ASYNC_ISEND_IRECV=true
##################
# bf16类型专用(megatron相关变量参考<百舸megatron专用>)
##################
export XMLIR_ENABLE_FAST_FC=true # 仅bf16下用到
# export USE_CAST_FC_FUSION=true # 仅bf16下用到, fp16转bf16与fc计算融合算子
export XMLIR_BATCH_PARALLEL=${XMLIR_BATCH_PARALLEL:-true}
# export FC_DW_MULTI_STREAM=${FC_DW_MULTI_STREAM:-true}
export XPU_FORCE_SHARED_DEVICE_CONTEXT=1
#################
# 通信通用
##################
##################
export BKCL_RDMA_PROXY_DISABLE=1
export BKCL_USE_AR=1
export BKCL_RING_OPT=1
export BKCL_FLAT_RING=1
export BKCL_CCIX_RING=1
export BKCL_TREE_THRESHOLD=1
export BKCL_CCIX_BUFFER_GM=1
export BKCL_FORCE_L3_RDMA=0
export BKCL_RING_BUFFER_GM=1
export BKCL_ENABLE_XDR=1
export BKCL_RDMA_FORCE_TREE=1
export BKCL_TREE_THRESHOLD=1
export BKCL_XLINK_D2D=0
export BKCL_XLINK_ETH=0
export BKCL_XLINK_C2C=1
export BKCL_TRANS_UNSUPPORTED_DATATYPE=1
export BKCL_KL3_TURBO_MODE=1
export BKCL_RING_BUFFER_SIZE=2097152
unset BKCL_KL3_SYSCON_FLAG

# export BKCL_SOCKET_IFNAME=bond0
export BKCL_RDMA_NICS=eth1,eth1,eth2,eth2,eth3,eth3,eth4,eth4
export BKCL_TIMEOUT=${BKCL_TIMEOUT:-1800}
export CUDA_DISABLE_PRINTF=${CUDA_DISABLE_PRINTF:-1}
export BKCL_RDMA_VERBS=${BKCL_RDMA_VERBS:-1}

export XME_USE_LOCAL_TE=true
export XME_USE_TE_VERSION=1.13.0
export XME_USE_CUSTOM_TRAINING_LOG=true
export XME_FORCE_SYNC_D2H_COPY=true
export ENABLE_GROUPED_GEMM=${ENABLE_GROUPED_GEMM:-true}
export SAVE_INTERVAL=${SAVE_INTERVAL:-50}

export XDNN_USE_FAST_SWISH=1
export XDNN_FAST_DIV_SCALAR=true
export XPUAPI_SDNN_BF16_ROUND_MODE=3
export XMLIR_FC_REDUCE_SCATTER_OVERLAP=1
export XPYTORCH_RUN_ENHANCE=1

if dmesg -T |  grep -q "noc_idle" <(cat /dev/stdin); then
    echo "Error: noc timeout found in dmesg, please check node ${RANK} ${HOSTNAME} ${POD_IP}!"
    dmesg -T >> dmesg_${RANK}.log
    exit 999
fi

# 设置任务超时检测阈值为30分钟,默认20分钟
for dev_id in $(echo $CUDA_VISIBLE_DEVICES | tr ',' ' '); do
    if [ -f /proc/kunlun/dev$dev_id/task_timeout_detect_threshold_in_ms ]; then
        echo 1800000 > /proc/kunlun/dev$dev_id/task_timeout_detect_threshold_in_ms
        cat /proc/kunlun/dev$dev_id/task_timeout_detect_threshold_in_ms
    fi
done
```

3. 执行训练
```bash
bash qwen3_30b_a3b_pretrain.sh
```
