# Qwen3vl-8B grpo verl Trainval Guide

## 环境准备
### 准备开发环境镜像
请联系相关人员获取开发环境镜像

## 数据集及模型准备
### 数据集准备
```bash
# 下载geo3k数据集
cd /home
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/geo3k.tar.gz
tar -xzf geo3k.tar.gz
```

### 下载预训练权重
```bash
# 从huggingface 下载预训练权重
# 下载Qwen3-VL-8B-Instruct 模型权重
cd /home
git lfs install
git clone https://huggingface.co/Qwen/Qwen3-VL-8B-Instruct
```

## 启动容器
```bash
vim run_docker.sh

# 在run_docker.sh中复制以下代码,并保存

#!/usr/bin/env bash
set -x
script_help() {
    echo "Usage:"
    echo "1) -n: container_name"
    echo "2) -i: image_name"
    exit -1
}

while getopts 'n:i:h' OPT; do
    case $OPT in
        n) container_name=${OPTARG};;
        i) image_name=${OPTARG};;
        h) script_help;;
        ?) script_help;;
    esac
done

function device_config() {
    echo -e "${CYELLOW} XPU NUM: ${XPU_NUM} ${CEND}"
    XPU_NUM=$(ls /proc/xpu/ | grep dev | wc -l)
    DOCKER_DEVICE_CONFIG=" "
    if [ $XPU_NUM -gt 0 ]; then
        for ((idx=0; idx<=$XPU_NUM-1; idx++)); do
            DOCKER_DEVICE_CONFIG+=" --device=/dev/xpu${idx}:/dev/xpu${idx} "
        done
        DOCKER_DEVICE_CONFIG+=" --device=/dev/xpuctrl:/dev/xpuctrl "
    fi
}

docker run -dti --name "${container_name}" --privileged \
    --ulimit core=-1 --security-opt seccomp=unconfined \
    --net=host --uts=host --ipc=host \
    --security-opt=seccomp=unconfined \
    --shm-size=256g \
    --restart=always \
    -v ~:/home -v `pwd`:/workdir \
    -w /workspace \
    ${image_name} \
    /bin/bash
# get xpu tool
docker cp -L  $(which xpu_smi) $container_name:/bin/xpu_smi || true
docker exec -it ${container_name} bash

# 执行启动镜像脚本, IMAGE_NAME填入真实镜像
bash run_docker.sh -n qwen3_vl_8b_verl -i IMAGE_NAME 
```

### 容器内环境配置
```bash
ray start --head --num-gpus=8
```

## 训练
```bash
cd /workspace

# 下载训练脚本，需要更改第149行，输入自己的wandb key
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/xpu_p800_qwen3-vl-8b_tp4_pp1_1node.sh
# 启动脚本进行训练
bash xpu_p800_qwen3-vl-8b_tp4_pp1_1node.sh
```