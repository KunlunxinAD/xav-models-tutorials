# Pi_0

## 环境准备
请联系昆仑芯客户支持获取开发环境镜像。

## 准备数据集及代码
### 准备数数据集
```bash
pip install modelscope
modelscope download --dataset HuggingFaceVLA/libero
```
#### 数据集介绍
```bash
在 LIBERO 环境中微调 VLA 模型以完成机器人操作。主要目标是让模型具备以下能力：
    视觉理解：处理来自机器人相机的 RGB 图像。
    语言理解：理解自然语言的任务描述。
    动作生成：产生精确的机器人动作（位置、旋转、夹爪控制）。
    强化学习：结合环境反馈，使用 PPO 优化策略。

LIBERO 环境：
    Environment：基于 robosuite （MuJoCo）的 LIBERO 仿真基准
    Task：指挥一台 7 自由度机械臂完成多种家居操作技能（抓取放置、叠放、开抽屉、空间重排等）
    Observation：工作区周围离屏相机采集的 RGB 图像（常见分辨率 128×128 或 224×224）
    Action Space：7 维连续动作 - 末端执行器三维位置控制（x, y, z） - 三维旋转控制（roll, pitch, yaw） - 夹爪控制（开/合）
```
##### 任务描述格式
```bash
In: What action should the robot take to [task_description]?
Out:
```
##### 数据结构
```bash
Images：RGB 张量 [batch_size, 224, 224, 3]
Task Descriptions：自然语言指令
Actions：归一化的连续值，转换为离散 tokens
Rewards：基于任务完成度的逐步奖励
```





### 下载代码及预训练权重
```bash
# 下载训练框架代码
git clone https://github.com/huggingface/lerobot.git

# 下载预训练模型
modelscope download --model lerobot/pi0_base
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
    pip install -e .
    git clone https://github.com/huggingface/transformers.git
    cd transformers
    git checkout fix/lerobot_openpi
    pip install -e .
    pip install peft==0.17.0
    ```


## 单机多卡训练
### 设置环境变量
```bash
export XMLIR_ENABLE_FAST_FC=true
export XMLIR_FC_BIAS_FUSION=true
export XDNN_USE_FAST_GELU=true
export XMLIR_BATCH_PARALLEL=false
export XMLIR_PARALLEL_SAVE_MEMORY=true
export DISABLE_CAST_CACHE=1
export XMLIR_CUDNN_ENABLED=1
export BKCL_TREE_THRESHOLD=0
export BKCL_ENABLE_XDR=1
export XPUCUDA_DISABLE_ERROR_PRINT=1
export XMLIR_MATMUL_FAST_MODE=1
export XMLIR_ENABLE_LINEAR_FC_FUSION=1
export CUDA_DEVICE_MAX_CONNECTIONS=1
```

### 训练
```bash
cd lerobot

accelerate launch \
  --multi_gpu \
  --num_processes=8 \
  src/lerobot/scripts/lerobot_train.py \
  --dataset.repo_id=HuggingFaceVLA/libero \
  --dataset.root=/datasets/libero \ # 上述下载数据集存放的路径
  --dataset.use_imagenet_stats=false \
  --policy.type=pi0 \
  --output_dir=./outputs/pi0_libero_fintune \  # 此次训练产出的存放路径
  --job_name=pi0_libero_fintune \
  --policy.pretrained_path=/workspace/models/pi0_base \ # 上述下载的预训练权重存放的路径
  --policy.push_to_hub=false \
  --policy.dtype=bfloat16 \
  --steps=30000 \
  --policy.device=cuda \
  --batch_size=16 \
  --log_freq=1 \
  --wandb.enable=True
```
### 训练示例
```bash
INFO 2026-01-23 10:20:54 ot_train.py:423 step:2 smpl:2K ep:13 epch:0.01 loss:0.730 grdn:7.914 lr:7.5e-08 updt_s:1.952 data_s:0.008
INFO 2026-01-23 10:20:56 ot_train.py:423 step:3 smpl:3K ep:19 epch:0.01 loss:0.744 grdn:7.715 lr:1.0e-07 updt_s:1.894 data_s:0.009
INFO 2026-01-23 10:20:58 ot_train.py:423 step:4 smpl:4K ep:25 epch:0.01 loss:0.912 grdn:9.028 lr:1.2e-07 updt_s:1.916 data_s:0.010
```
```bash
# 训练完成后，在输出路径下会存在checkpoints和wandb两个文件夹，其中wandb是记录训练数据，checkpoints中存放训练出来不同保存step的权重
cd checkpoints
# checkpoints下会有根据设置（默认）的保存频率保存的训练权重及训练状态，可以将需要评估的训练权重拷贝至评估机器上，或者两个机器共用磁盘上，例如
scp -r ./outputs/pi0_libero_fintune/checkpoints/030000/pretrained_model 远程主机:/workspace/models/pi0_libero_fintune
```
### 评估
```bash
# 需要使用nv卡,例如A800或A30
# 在测试环境中安装lerobot
git clone https://github.com/huggingface/lerobot.git
pip install -e ".[lerobot]"
pip install -e ".[pi]"
# 如果在运行途中，发现报错没有egl
apt-get install -y libegl1 libgl1 libopengl0

# 评估命令
lerobot-eval \
  --env.type=libero \
  --env.task=libero_spatial,libero_object,libero_goal,libero_10 \
  --eval.batch_size=1 \
  --eval.n_episodes=10 \
  --policy.path=/workspace/models/pi0_libero_fintune \ # 存放p800训练出来的权重路径
  --policy.n_action_steps=10 \
  --output_dir=./eval_logs_pi0_lerobt/ \
  --env.max_parallel_tasks=1 
```

### 评估结果示例
```bash
# 在eval_logs_pi0_lerobt目录下存在videos和eval_info.json
# videos中会有libero_spatial,libero_object,libero_goal,libero_10下10个问题分别进行10次测试保存的视频
# eval_info.json中有该次评估的量化数据
```
| libero_spatial | libero_object | libero_goal | libero_10 | total  |
| ---- | ---- | ---- |  ---- | ---- |
| 90% | 90%   | 92% | 83% | 88.75% |





