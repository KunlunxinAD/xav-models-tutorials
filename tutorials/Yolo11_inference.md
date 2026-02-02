# Yolo11

## 准备环境
将以下代码复制到bash脚本中启动docker
```bash
export XAV_IMAGE=iregistry.baidu-int.com/kunlunxin-self-driving/xav:25.12-py310
export CONTAINER_NAME=yolo-infer-test2
export MOUNT_PATH=/your/path/to/mount
export data=/your/data/path

docker run -dti \
        --name ${CONTAINER_NAME} \
        --net=host \
        --security-opt=seccomp=unconfined \
        -e IMAGE_VERSION="${XAV_IMAGE}" \
        --cap-add=SYS_PTRACE \
        --shm-size=512g \
        --cap-add=SYS_ADMIN \
        --device /dev/fuse \
        --restart=always \
        --ulimit=memlock=-1 \
        --ulimit=nofile=120000 \
        --ulimit=stack=67108864 \
        -v ${MOUNT_PATH}:/home \
        -v ${DATA_PATH}:/data/ \
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

## 拉取代码
```bash
#拉取代码仓库
git clone https://github.com/ultralytics/ultralytics.git
```

## 安装依赖
```bash
#如果安装慢可以使用清华源
pip install "ultralytics[export]" -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## pytorch 推理
```bash
#建议单独创建一个文件夹用于推理
mkdir inference
cd inference
#直接在终端输入CLI命令即可推理，详细的可以参考ultralytics/engine/exporter.py
yolo predict model=yolo11n.pt
```