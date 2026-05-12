# Yolo Inference Guide

## 准备环境

请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器

> 以下 docker 启动脚本请根据实际机器环境（镜像 tag、挂载目录、XPU 卡数等）自行补充：

```bash
export XAV_IMAGE=<XAV_IMAGE>
export CONTAINER_NAME=xvllm_test
export PATH_TO_MOUNT=</path/to/mount> #本地路径
export PATH_AFTER_MOUNT=/home #挂载后在容器内的路径
export DATA_PATH=</path/to/dataset>

docker run -dti \
        --name ${CONTAINER_NAME} \
        --net=host \
        --security-opt=seccomp=unconfined \
        -e IMAGE_VERSION="${XAV_IMAGE}" \
        --cap-add=SYS_PTRACE \
        --shm-size=256g \
        --cap-add=SYS_ADMIN \
        --device /dev/fuse \
        --restart=always \
        --ulimit=memlock=-1 \
        --ulimit=nofile=120000 \
        --ulimit=stack=67108864 \
        -v ${PATH_TO_MOUNT}:${PATH_AFTER_MOUNT} \
        -v ${DATA_PATH}:/data \
        --cpuset-cpus=0-$((`nproc`-8)) \
        --device=/dev/xpu0:/dev/xpu0 \
        --device=/dev/xpu1:/dev/xpu1 \
        --device=/dev/xpu2:/dev/xpu2 \
        --device=/dev/xpu3:/dev/xpu3 \
        --device=/dev/xpu4:/dev/xpu4 \
        --device=/dev/xpu5:/dev/xpu5 \
        --device=/dev/xpu6:/dev/xpu6 \
        --device=/dev/xpu7:/dev/xpu7 \
        --privileged ${XAV_IMAGE}
```

## 安装依赖
```bash
pip install "ultralytics[export]" -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## PyTorch 推理
```bash
#建议单独创建一个文件夹用于推理
mkdir -p /home/inference
cd /home/inference
#直接在终端输入CLI命令即可使用ultralytics自带的测试数据进行推理。以yolo11n.pt为例：
yolo predict model=yolo11n.pt
```