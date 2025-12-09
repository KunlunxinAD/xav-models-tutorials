# Pi_0

## 环境准备
请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集及代码
### 准备数据集
```bash
git-lfs install
git clone https://huggingface.co/datasets/lerobot/behavior1k-task0000
# 使用该数据集的时候会发生https://github.com/huggingface/lerobot/issues/2364， 此问题与lerobot框架有关，解决方案是修改增加src/lerobot/datasets/lerobot_dataset.py第560行tolerance_s的值
```

### 下载代码
```bash
git clone https://github.com/huggingface/lerobot.git
```

## 启动容器
1. 启动容器：
    ```bash
    export XAV_IMAGE=<XAV_IMAGE>
    export NAME_CONTAINER=lerobot
    export MODEL_PATH=</path/to/models> #本地路径
    
    docker run -itd --privileged --net=host \
        -w /workspace/xav-models/modelzoo \
        -v ${MODEL_PATH}:/workspace/models \
        --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
        --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
        --device=/dev/xpu6 --device=/dev/xpu7 \
        --name ${NAME_CONTAINER} \
        --shm-size 256g \
        ${XAV_IMAGE} \
        bash
    
    docker exec -it ${XAV_CONTAINER} bash
    ```
2. 配置容器内环境：
    ```bash
    # 更新环境
    cd lerobot
    # 修改 pyproject.toml 注释 79-81行，对于torch的安装
    pip install -e ".[pi]"
    pip install peft==0.17.0
    ```


## 单机多卡训练
### 设置环境变量
```bash
export BKCL_TREE_THRESHOLD=0
```

### 训练
```bash
cd lerobot
accelerate launch \
  --multi_gpu \
  --num_processes=8 \
  src/lerobot/scripts/lerobot_train.py \
  --dataset.repo_id=lerobot/behavior1k-task0000 \
  --dataset.use_imagenet_stats=false \
  --policy.type=pi0 \
  --output_dir=./outputs/pi0_training \
  --job_name=pi0_training \
  --policy.max_state_dim=256 \
  --policy.push_to_hub=false \
  --policy.dtype=bfloat16 \
  --steps=10 \
  --policy.device=cuda \
  --batch_size=2 \
  --log_freq=1
```
### 训练示例
```bash
INFO 2025-12-09 17:09:48 ot_train.py:354 step:2 smpl:256 ep:0 epch:0.00 loss:4.235 grdn:156.835 lr:2.3e-05 updt_s:3.176 data_s:0.010
INFO 2025-12-09 17:09:51 ot_train.py:354 step:3 smpl:384 ep:0 epch:0.00 loss:2.352 grdn:95.974 lr:2.0e-05 updt_s:2.581 data_s:0.008
INFO 2025-12-09 17:09:54 ot_train.py:354 step:4 smpl:512 ep:0 epch:0.00 loss:3.317 grdn:86.970 lr:1.7e-05 updt_s:2.759 data_s:0.007
INFO 2025-12-09 17:09:56 ot_train.py:354 step:5 smpl:640 ep:0 epch:0.00 loss:3.434 grdn:82.286 lr:1.4e-05 updt_s:2.307 data_s:0.008
INFO 2025-12-09 17:09:59 ot_train.py:354 step:6 smpl:768 ep:0 epch:0.00 loss:2.171 grdn:86.964 lr:1.0e-05 updt_s:2.520 data_s:0.008
INFO 2025-12-09 17:10:01 ot_train.py:354 step:7 smpl:896 ep:0 epch:0.00 loss:2.269 grdn:70.050 lr:7.1e-06 updt_s:2.454 data_s:0.007
INFO 2025-12-09 17:10:06 ot_train.py:354 step:8 smpl:1K ep:0 epch:0.00 loss:3.643 grdn:70.016 lr:4.6e-06 updt_s:5.031 data_s:0.009
INFO 2025-12-09 17:10:09 ot_train.py:354 step:9 smpl:1K ep:1 epch:0.00 loss:2.390 grdn:70.859 lr:3.1e-06 updt_s:2.424 data_s:0.009
INFO 2025-12-09 17:10:11 ot_train.py:354 step:10 smpl:1K ep:1 epch:0.00 loss:2.335 grdn:62.622 lr:2.5e-06 updt_s:2.428 data_s:0.009
INFO 2025-12-09 17:10:11 ot_train.py:364 Checkpoint policy after step 10
INFO 2025-12-09 17:10:47 ot_train.py:435 End of training
```





