# Qwen3vl-8B grpo verl Trainval Guide

## 环境准备
1. 下载模型启动脚本代码库并解压：
```bash
cd /YOUR_PATH
# 请联系相关人员获取开发环境镜像
# 解压镜像
tar -xf rl-training.tar.gz
```
2. 进入模型对应路径：
```bash
cd /YOUR_PATH/rl-training/xverl/Qwen3-VL-8B/
```
3.  环境准备。包括下载训练模型所需要的材料，启动镜像，并进入容器，注意权限：
```bash
bash env_prepare.sh
```
4. 进入容器内，训练前准备：
```bash
ln -s /workdir/* /workspace/
ray start --head --num-gpus=8
cd /workspace
```
## 训练
```bash
# 可以根据情况灵活修改xpu_p800_qwen3_vl_8b_tp4_pp1_1node.sh
