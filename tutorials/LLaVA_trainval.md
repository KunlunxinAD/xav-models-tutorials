# LLaVA

## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-LLaVA
export MODEL_PATH=</path/to/model>
export DATASET_PATH=</path/to/dataset>

docker run -itd --privileged --net=host \
    -w /workspace/ \
    -v ${MODEL_PATH}:/home \
    -v ${DATASET_PATH}:/datasets \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${XAV_CONTAINER} \
    --shm-size 128g \
    ${XAV_IMAGE} \
    bash
 
docker exec -it ${XAV_CONTAINER} bash 
```

## 下载模型及安装资源
切换conda环境：
```bash
#切换至torch25环境
conda activate python310_torch25_cuda
```
下载LLaVA代码：
```bash
cd /home
git clone https://github.com/haotian-liu/LLaVA.git
```
安装依赖：
```bash
#更新XPtorch
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/LLaVA/xpytorch-cp310-torch251-ubuntu2004-x64.run
bash xpytorch-cp310-torch251-ubuntu2004-x64.run

#安装flash_attn
pip install flash_attn==2.8.0.post2

#bitsandbytes和transformers==4.37.2仅用于模型推理，模型训练时不需要安装
pip install bitsandbytes
pip install transformers==4.37.2
```

## 权重准备
```bash
# 参考官方文档,从huggingface中下载权重
https://github.com/haotian-liu/LLaVA/blob/main/README.md
# 下载链接，以训练llava-next 13b为例
https://huggingface.co/openai/clip-vit-large-patch14-336
https://huggingface.co/liuhaotian/llava-v1.6-vicuna-13b
```

## 数据集准备
### pretrain数据集
```bash
cd /datasets
mkdir LLaVA-datasets && cd LLaVA-datasets
mkdir LLaVA-Pretrain && cd LLaVA-Pretrain
# 方式一：参考官方文档,从huggingface中下载数据集
https://github.com/haotian-liu/LLaVA/blob/main/README.md

# 方式二：从BOS中下载
# 下载LLaVA-Pretrain
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-Pretrain/blip_laion_cc_sbu_558k_meta.json
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-Pretrain/blip_laion_cc_sbu_558k.json
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-Pretrain/images.zip
unzip images.zip
```
### finetune数据集
```bash
cd /datasets/LLaVA-datasets
# 方式一：参考官方文档,从huggingface中下载数据集
https://github.com/haotian-liu/LLaVA/blob/main/README.md

# 方式二：从BOS中下载
# 下载LLaVA-Instruct-150K
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-SFT/LLaVA-Instruct-150K.tar.gz
tar -xvf LLaVA-Instruct-150K.tar.gz
# 下载coco数据集
cd /datasets/LLaVA-datasets
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-SFT/coco/COCO2017.tar.gz
tar -xvf COCO2017.tar.gz && mv COCO2017 coco
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-SFT/gqa/images.zip
# 下载gqa数据集
cd /datasets/LLaVA-datasets
mkdir gqa && cd gqa
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-SFT/gqa/images.zip
unzip images.zip
# 下载ocr_vga数据集
cd /datasets/LLaVA-datasets
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-SFT/ocr_vqa/ocr_vqa.tar
tar -xvf ocr_vqa.tar
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-SFT/ocr_vqa/1437717772.jpg
mv 1437717772.jpg  ocr_vqa/images
# 下载textvqa数据集
cd /datasets/LLaVA-datasets
mkdir textvqa && cd textvqa
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-SFT/textvqa/train_val_images.zip
unzip train_val_images.zip
# 下载vg数据集
cd /datasets/LLaVA-datasets
mkdir vg && cd vg
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-SFT/vg/images.zip
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-SFT/vg/images2.zip
```

## 训练与评估
单机八卡训练：
```bash
cd /home/LLaVA/scripts
mkdir v1_6 && cd v1_6
# pretrain 1x8
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/LLaVA/pretrain.sh
bash pretrain.sh
# finetune 1x8
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/LLaVA/finetune_task.sh
bash finetune_task.sh
```
推理：
```bash
cd /home/LLaVA
PYTHONPATH="$(dirname $0)/..":$PYTHONPATH \
python -m llava.serve.cli \
    --model-path /home/LLaVA/checkpoints/llava-v1.6-vicuna-13b \
    --image-file "https://llava-vl.github.io/static/images/view.jpg"
```