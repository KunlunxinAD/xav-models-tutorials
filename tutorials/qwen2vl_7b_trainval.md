# Qwen2-VL-7B

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集及代码
### 准备数据集
```bash
# 参考官方文档准备数据集llava_150k_zh
https://github.com/QwenLM/Qwen2-VL?tab=readme-ov-file#training

# 或者从huggingface镜像网站下载数据集
mkdir -p path_to_datasets/qwen2-vl/
git lfs install
git clone https://hf-mirror.com/datasets/BUAADreamer/llava-en-zh-300k
```

### 下载代码及预训练权重
```bash
# 从modelscope下载预训练权重
cd /home
mkdir Qwen2-VL-7B-Instruct
pip install modelscope
modelscope download --model Qwen/Qwen2-VL-7B-Instruct  --local_dir /home/Qwen2-VL-7B-Instruct

# 下载qwen2-VL微调代码以及配置
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/qwen2-vl.tar.gz
tar -zxvf qwen2-vl.tar.gz
```

## 启动容器
1. 启动容器：
    ```bash
    export XAV_IMAGE=<XAV_IMAGE>
    export LLAMA_CONTAINER=Qwen2_VL_test
    export MODEL_PATH=</path/to/qwen2-vl> #本地路径
    
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
2. 配置容器内环境：
    ```bash
    # 创建conda env
    conda create -n Qwen2VL_env --clone python38_torch201_cuda
    conda init bash
    conda activate Qwen2VL_env

    # 更新环境
    pip install llamafactory==0.9.1
    pip install transformers==4.45.2
    pip install trl==0.8.6
    pip install pydantic==2.8.2
    pip install deepspeed==0.14.5
    pip install urllib3==1.25.11
    pip install rouge-chinese

    # xav v0.4及以下版本镜像需要升级setuptools
    pip install --upgrade setuptools
    ```

## 单机8卡训练
1. 设置训练路径与参数：
    ```bash
    cd /home/qwen2-vl
    #根据实际数据与代码路径、训练参数进行修改
    vim qwen2vl_full_sft.yaml
    vim qwen2vl_lora_sft.yaml
    ```

2. 执行LoRA微调：
    ```bash
    source env.sh
    bash train.sh lora
    ```

3. 执行SFT微调：
    ```bash
    source env.sh
    bash train.sh sft
    ```

## 训练与评估
1. 评估数据集准备：
    ```bash
    # 参考官方文档准备数据集llava_1k_zh
    https://github.com/QwenLM/Qwen2-VL?tab=readme-ov-file#training

    # 或者从huggingface镜像网站下载数据集
    cd path_to_datasets/qwen2-vl/
    git lfs install
    git clone https://hf-mirror.com/datasets/BUAADreamer/llava-en-zh-2k
    ```
2. 设置评估路径与参数：
    ```bash
    cd /home/qwen2-vl
    #根据实际数据与代码路径、评估参数进行修改
    vim qwen2vl_full_sft_predict.yaml
    vim qwen2vl_lora_sft_predict.yaml
    ```
3. 执行评估：
    ```bash
    source env.sh
    bash predict.sh lora
    bash predict.sh sft
    ```





