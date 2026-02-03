#PPOcr_v5

## 准备环境
复制以下代码到bash脚本中启动docker
```bash
YOUR_DOCKER_NAME=your/docker/name
XPU_NUM=8 #xpu nums
if [ -n "$2" ]; then
  XPU_NUM=$2
fi
echo "docker container name: ${YOUR_DOCKER_NAME}, kl3 card num: ${XPU_NUM}"
DOCKER_DEVICE_CONFIG=" "
if [ $XPU_NUM -gt 0 ]; then
for ((idx=0; idx<=$XPU_NUM-1; idx++)); do
  DOCKER_DEVICE_CONFIG+=" --device=/dev/xpu${idx}:/dev/xpu${idx} "
done
DOCKER_DEVICE_CONFIG+=" --device=/dev/xpuctrl:/dev/xpuctrl "
fi
docker run -d -p 8000:8081 -it          \
        ${DOCKER_DEVICE_CONFIG}         \
        --net=host                      \
        --pid=host                      \
        --cap-add=SYS_PTRACE            \
        --privileged                    \
        -v /your/mount/path:/home  \
        -v /your/data/path:/data      \
        --name ${YOUR_DOCKER_NAME}      \
        -w /home                        \
        --shm-size=256g                 \
        iregistry.baidu-int.com/xccl/paddle-xpu:ubuntu2204-x86_64-gcc123-py310-rc1 /bin/bash
```
## 拉取代码
git clone https://github.com/PaddlePaddle/PaddleOCR.git

#使用release/3.3分支
git checkout release/3.3

## 安装依赖
```bash
#for export onnx
python -m pip install --pre paddlepaddle -i https://www.paddlepaddle.org.cn/packages/nightly/cpu/
#安装xpu版的paddle
python -m pip install paddlepaddle-xpu==3.3.0 -i https://www.paddlepaddle.org.cn/packages/stable/xpu-p800/
python -m pip install paddleocr
python3 -m pip install paddle2onnx
python3 -m pip install onnxruntime

pip install scikit-image
pip install pyclipper
pip install albumentations
pip install lmdb
pip install shapely
pip install numpy==1.24.4
```
## ONNX推理
```bash
mkdir your/inference/path
cd your/inference/path
#下载模型
wget https://paddle-model-ecology.bj.bcebos.com/paddlex/official_inference_model/paddle3.0.0/PP-OCRv5_server_det_infer.tar
wget https://paddle-model-ecology.bj.bcebos.com/paddlex/official_inference_model/paddle3.0.0/PP-OCRv5_server_rec_infer.tar

tar  -zxvf PP-OCRv5_server_det_infer.tar
tar  -zxvf PP-OCRv5_server_rec_infer.tar

#根据对应模型的文件夹转出onnx模型，除了det意外还有rec和cls需要转
paddle2onnx --model_dir ./inference/PP-OCRv5_mobile_det_infer \
--model_filename inference.json \
--params_filename inference.pdiparams \
--save_file ./inference/det_onnx/model.onnx \
--opset_version 11 \
--enable_onnx_checker True

#然后运行，需要设置use_gpu=True，不然跑的是cpu
python3 tools/infer/predict_system.py --use_gpu=True --use_onnx=True \
--det_model_dir=./inference/det_onnx/model.onnx  \
--rec_model_dir=./inference/rec_onnx/model.onnx  \
--rec_char_dict_path=./ppocr/utils/dict/ppocrv5_dict.txt \
--image_dir=./deploy/lite/imgs/lite_demo.png
```
