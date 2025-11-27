# Qwen2.5-VL Inference Guide

## 镜像获取
### 准备开发环境镜像
请联系相关人员获取开发环境镜像

## 环境准备
### 启动容器
```bash
#获取镜像版本
image_name=<XAV_IMAGE>
docker run -it --privileged \
--net=host \
--cap-add=SYS_PTRACE \
--device=/dev/xpu0:/dev/xpu0 \
--device=/dev/xpu1:/dev/xpu1 \
--device=/dev/xpu2:/dev/xpu2 \
--device=/dev/xpu3:/dev/xpu3 \
--device=/dev/xpu4:/dev/xpu4 \
--device=/dev/xpu5:/dev/xpu5 \
--device=/dev/xpu6:/dev/xpu6 \
--device=/dev/xpu7:/dev/xpu7 \
--device=/dev/xpuctrl:/dev/xpuctrl \
--shm-size=64g \
-v ${PWD}:/workspace \
-w /workspace \
--name qwen25vl_infer \
$image_name /bin/bash

docker start qwen25vl_infer
docker exec -it qwen25vl_infer /bin/bash
```
### 容器内环境配置
```bash
cd /workspace
### 下载测试代码包
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_vl/output.tar.gz
tar zxvf output.tar.gz
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_vl/qwen25vl_infer_scripts.tar.gz
tar zxvf qwen25vl_infer_scripts.tar.gz
```
### 依赖安装
```bash
cd /workspace/output
python3 -m pip install tvm*.whl --force-reinstall --no-cache-dir --no-index --no-deps
python3 -m pip install tensorrt_llm*.whl --force-reinstall --no-cache-dir --no-index --no-deps
cp -rf scripts/xpytorch_import_hook.py /home/pt201/lib/python3.8/site-packages/xpytorch_import_hook.py
```
### 模型下载
```bash
cd /workspace
mkdir models & cd models
### 模型下载到models目录并解压,以Qwen2.5-VL-32B-Instruct为例
### 从huggingface官方网址下载权重
https://huggingface.co/Qwen/Qwen2.5-VL-32B-Instruct
```

## Build TensorRT engine
### 模型转换&构建engine
```bash
cd /workspace
source output/scripts/set_cpp_env.sh
source output/scripts/set_release_env.sh
#8卡
cd /workspace/qwen25vl_infer_scripts
bash build_engines_8_devices.sh
```

## 在线推理
### 启动服务
```bash
# 下载启动服务脚本
cd /workspace
source output/scripts/set_cpp_env.sh
source output/scripts/set_release_env.sh

cd /workspace/qwen25vl_infer_scripts
bash start_vllm_server_v2_8_devices.sh
```
### 单样例测试
```bash
# 另起一个终端
docker exec -it qwen25vl_infer bash
# 激活环境变量
source output/scripts/set_cpp_env.sh
source output/scripts/set_release_env.sh
cd /workspace/qwen25vl_infer_scripts
bash simple_test.sh
```
### 多batch测试
```bash
cd /workspace/qwen25vl_infer_scripts
python3 multi_client.py
```