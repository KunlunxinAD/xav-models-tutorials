# PaddleOCR_v5

## 准备环境

请联系昆仑芯客户支持获取开发环境镜像

## 启动容器

```bash
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
        <PADDLE_IMAGE> /bin/bash
```

### 配置容器内环境

```bash
# for export onnx
pip install --pre paddlepaddle -i https://www.paddlepaddle.org.cn/packages/nightly/cpu/

# 安装xpu版的paddle
pip install paddlepaddle-xpu==3.3.0 -i https://www.paddlepaddle.org.cn/packages/stable/xpu-p800/

pip install paddleocr
pip install paddle2onnx
pip install onnxruntime

pip install scikit-image
pip install pyclipper
pip install albumentations
pip install lmdb
pip install shapely
pip install numpy==1.24.4
pip install rapidfuzz
```

可通过如下命令验证是否安装成功：

```bash
python -c "import paddle;print(paddle.device.is_compiled_with_xpu())"
```

## 准备数据集及代码

可以按以下方式直接下载已经打包好的数据和代码：

```bash
# 整理好的训练数据直接包含在PaddleOCR/train_data中
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/PaddleOCR/PaddleOCR_code_data.tgz -O PaddleOCR.tgz
tar -xzvf PaddleOCR.tgz
```

也可以按如下步骤自行准备：

### 准备代码

```bash
git clone https://github.com/PaddlePaddle/PaddleOCR.git
git checkout release/3.3
```

### 准备数据集

参考：https://github.com/PaddlePaddle/PaddleOCR/blob/main/docs/datasets/ocr_datasets.md

## 下载预训练权重

onnx推理可以直接使用如下方式获得预训练权重：

```bash
wget https://paddle-model-ecology.bj.bcebos.com/paddlex/official_inference_model/paddle3.0.0/PP-OCRv5_server_det_infer.tar
wget https://paddle-model-ecology.bj.bcebos.com/paddlex/official_inference_model/paddle3.0.0/PP-OCRv5_server_rec_infer.tar

tar  -zxvf PP-OCRv5_server_det_infer.tar
tar  -zxvf PP-OCRv5_server_rec_infer.tar

# 根据对应模型的目录转出onnx模型，除了det以外还有rec和cls需要转
paddle2onnx --model_dir ./inference/PP-OCRv5_mobile_det_infer \
--model_filename inference.json \
--params_filename inference.pdiparams \
--save_file ./inference/det_onnx/model.onnx \
--opset_version 11 \
--enable_onnx_checker True
```

## 模型训推

### 模型测试

```bash
# Run PP-OCRv5 inference
paddleocr ocr -i https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_ocr_002.png --use_doc_orientation_classify False --use_doc_unwarping False --use_textline_orientation False  
```

### 模型训练

#### 训练检测模型

```bash
export FLAGS_use_stride_kernel=0
export BKCL_FORCE_SYNC=1
export BKCL_TIMEOUT=1800
export XPU_BLACK_LIST=pad3d,pad3d_grad

python3 -m paddle.distributed.launch --devices '0,1,2,3,4,5,6,7' \
    tools/train.py -c configs/det/PP-OCRv5/PP-OCRv5_mobile_det.yml \
    -o Global.use_gpu=False Global.use_xpu=True
```

#### 训练识别模型

```bash
export FLAGS_use_stride_kernel=0
export BKCL_FORCE_SYNC=1
export BKCL_TIMEOUT=1800
export XPU_BLACK_LIST=pad3d,pad3d_grad

python3 -m paddle.distributed.launch --devices '0,1,2,3,4,5,6,7' \
    tools/train.py -c configs/det/PP-OCRv5/PP-OCRv5_mobile_det.yml \
    -o Global.use_gpu=False Global.use_xpu=True
```

### 模型onnx推理

```bash
python3 tools/infer/predict_system.py --use_xpu=True --use_onnx=True \
--det_model_dir=./inference/det_onnx/model.onnx  \
--rec_model_dir=./inference/rec_onnx/model.onnx  \
--rec_char_dict_path=./ppocr/utils/dict/ppocrv5_dict.txt \
--image_dir=./deploy/lite/imgs/lite_demo.png
```