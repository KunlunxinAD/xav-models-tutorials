# PaddleOCR_v5

## 镜像
> 请联系昆仑芯客户支持获取开发环境镜像
* 容器启动方式如下：
```
YOUR_DOCKER_NAME=paddle_ocr_dev # 容器名称
XPU_NUM=8 # kl3卡数量
if [ -n "$2" ]; then
  XPU_NUM=$2
fi
echo "docker container name: ${YOUR_DOCKER_NAME}, kl3 card num: ${XPU_NUM}"
DOCKER_DEVICE_CONFIG=" "
if [ $XPU_NUM -gt 0 ]; then
for ((idx=0; idx<=$XPU_NUM-1; idx++)); do
  DOCKER_DEVICE_CONFIG+=" --device=/dev/xpu${idx}:/dev/xpu${idx} "
done
DOCKER_DEVICE_CONFIG+=" --device=/dev/xpuctrl:/dev/xpuctrl "
fi
docker run -d -p 8000:8081 -it          \
        ${DOCKER_DEVICE_CONFIG}         \
        --net=host                      \
        --pid=host                      \
        --cap-add=SYS_PTRACE            \
        --privileged                    \
        -v /path/to/your/home:/home     \
        -v /path/to/your/dataset:/dataset \
        --name ${YOUR_DOCKER_NAME}      \
        -w /home                        \
        --shm-size=256g                 \
        <昆仑芯提供的镜像地址> /bin/bash
```
## 代码&数据
* 方式一：（推荐）

```
# 整理好的训练数据直接包含在PaddleOCR/train_data中
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/PaddleOCR/PaddleOCR_code_data.tgz -O PaddleOCR.tgz
tar -xzvf PaddleOCR.tgz
```
* 方式二

```
#可能会失败，因为文件太多
git clone https://github.com/PaddlePaddle/PaddleOCR.git

#分步克隆
git clone --depth 1 https://github.com/PaddlePaddle/PaddleOCR.git
cd PaddleOCR
git fetch --unshallow

#如果下载慢：设置代理
git config --global url."https://bgithub.xyz/".insteadOf "https://github.com/"

#icdar2015数据下载整理（det和rec两个数据集都需要）
#整理数据参考 PaddleOCR/docs/datasets/ocr_datasets.md
```

## 环境
* 安装依赖
```
python -m pip install paddlepaddle-xpu==3.3.0 -i https://www.paddlepaddle.org.cn/packages/stable/xpu-p800/
python -m pip install paddleocr
cd PaddleOCR && pip install -r requirements.txt
```
* 验证环境
```
# 验证是否成功安装了paddlepaddle-xpu
python -c "import paddle;print(paddle.device.is_compiled_with_xpu())"
```



## 测试
```
# Run PP-OCRv5 inference
paddleocr ocr -i https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_ocr_002.png --use_doc_orientation_classify False --use_doc_unwarping False --use_textline_orientation False  
```

## 训练
* 训练检测模型

```
export FLAGS_use_stride_kernel=0
export BKCL_FORCE_SYNC=1
export BKCL_TIMEOUT=1800
export XPU_BLACK_LIST=pad3d,pad3d_grad

python3 -m paddle.distributed.launch --devices '0,1,2,3,4,5,6,7' \
    tools/train.py -c configs/det/PP-OCRv5/PP-OCRv5_mobile_det.yml \
    -o Global.use_gpu=False Global.use_xpu=True
```
* 训练识别模型

```
export FLAGS_use_stride_kernel=0
export BKCL_FORCE_SYNC=1
export BKCL_TIMEOUT=1800
export XPU_BLACK_LIST=pad3d,pad3d_grad

python3 -m paddle.distributed.launch --devices '0,1,2,3,4,5,6,7' \
    tools/train.py -c configs/det/PP-OCRv5/PP-OCRv5_mobile_det.yml \
    -o Global.use_gpu=False Global.use_xpu=True
```