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
# 下载完成后，llava-v1.6-vicuna-13b中的config.json根据实际情况修改 "_name_or_path"和"mm_vision_tower"; clip-vit-large-patch14-336的config.json根据实际情况修改 "_name_or_path"
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

## 训练与评估
单机八卡训练：
```bash
cd /home/LLaVA/scripts
mkdir v1_6 && cd v1_6
# pretrain 1x8
wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/LLaVA/pretrain.sh
# pretrain.sh脚本中的权重及数据集路径根据实际情况进行修改；
bash pretrain.sh

```
推理：
```bash
cd /home/LLaVA
PYTHONPATH="$(dirname $0)/..":$PYTHONPATH \
python -m llava.serve.cli \
    --model-path /home/LLaVA/checkpoints/llava-v1.6-vicuna-13b \
    --image-file "https://llava-vl.github.io/static/images/view.jpg"
```
