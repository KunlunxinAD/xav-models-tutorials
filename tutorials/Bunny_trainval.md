# Bunny Trainval Guide

## 环境准备

请联系昆仑芯客户支持获取开发环境镜像。

⚠注意

当前Bunny模型适配0.1版本镜像，未作最新的环境适配。

## 数据集准备

方式一：按照官方教程，下载并处理Bunny数据集：参考 [Bunny README](https://huggingface.co/datasets/BoyaWu10/Bunny-v1_1-data)（链接下载很慢）。

方式二：Bunny数据集已备份到bos，如下所示从bos下载：
```bash
mkdir -p path_to_datasets/bunny/pretrain
cd path_to_datasets/bunny/pretrain
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/pretrian/bunny_pretrain_laion_2m.json
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/pretrian/images.tar.gz
tar -zxvf images.tar.gz

mkdir -p path_to_datasets/bunny/finetune
cd path_to_datasets/bunny/finetune
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_695k.json
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_allava_1.3m.json
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_llava_1.4m.json
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_llava_allava_2m.json

# finetune/images.tar.gz 147G比较大，可以下载拆分后的文件
# 总体下载
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/images.tar.gz
tar -zxvf images.tar.gz

# 拆分下载：拆成了15个文件part-aa ~ part-ao 在finetune/images_tar_split/
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/images_tar_split/images.tar.gz.part-aa
# ... part-aa ~ part-ao 共15个文件
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/images_tar_split/images.tar.gz.part-ao

cat images.tar.gz.part-* > images.tar.gz
tar -zxvf images.tar.gz
```

## 启动容器环境
运行以下命令以启动容器环境：


```bash
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=bunny-test
export DATA_PATH=</path/to/datasets/bunny>

docker run -itd --privileged --net=host \
    -w /workspace/xav-models/modelzoo \
    -v $PWD:/workspace \
    -v ${DATA_PATH}:/data/bunny/ \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${XAV_CONTAINER} \
    --shm-size 64g \
    ${XAV_IMAGE} \
    bash
 
docker exec -it ${XAV_CONTAINER} bash 
```

## 资源下载及环境准备

下载Bunny代码并解压：
```bash
cd /home
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/xav/v0.2/Bunny.tar.gz
tar -zxvf Bunny.tar.gz
cd Bunny
# docker环境中已安装所有依赖包，这一步可在离线环境下完成
pip install -e .
```

训练权重准备：
```bash
# 权重文件放在这个路径下
cd /workspace/xav-models/modelzoo/Bunny/script/train
# siglip-so400m-patch14-384 是vision encoder的权重文件
wget http://klx-sdk-release-public.su.bcebos.com/xav/model_weights/siglip-so400m-patch14-384.tar.gz
tar -zxvf siglip-so400m-patch14-384.tar.gz
# Meta-Llama-3-8B-Instruct 是language model的权重文件
wget http://klx-sdk-release-public.su.bcebos.com/xav/model_weights/Meta-Llama-3-8B-Instruct.tar.gz
tar -zxvf Meta-Llama-3-8B-Instruct.tar.gz

# 第一阶段pretrian训练不完，所以第二阶段Lora微调要使用Pretrain权重，来自 https://huggingface.co/BoyaWu10/bunny-pretrain-llama3-8b-siglip-s2
wget http://klx-sdk-release-public.su.bcebos.com/xav/model_weights/bunny-pretrain-llama3-8b-siglip-s2.tar.gz
mkdir checkpoints-pretrain
tar -zxvf bunny-pretrain-llama3-8b-siglip-s2.tar.gz -C /workspace/xav-models/modelzoo/Bunny/script/train/checkpoints-pretrain/
```

## 模型训练测试
训练分为pretrain和lora微调两个阶段。
### Pretrain训练
```bash
cd /workspace/xav-models/modelzoo/Bunny/script/train
sh pretrain.sh
```
由于完整pretrain耗时较长，运行3000 steps作为验证，在代码中临时指定训练step数：
```bash
vim /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/transformers/trainer.py +2237
            for step, inputs in enumerate(epoch_iterator):
                total_batched_samples += 1
+               if self.state.global_step > 3000:
+                  break
                
```
运行3000 steps大约需10小时，预期训练速度如图：
![pretrain训练](images/image2.png)
### lora微调
由于完整lora微调耗时较长，同时为兼顾后续模型评估效果，lora微调5000 steps。在代码中临时指定训练step数：
```bash
vim /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/transformers/trainer.py +2237
            for step, inputs in enumerate(epoch_iterator):
                total_batched_samples += 1
+               if self.state.global_step > 5000:
+                  break
                
```

由于pretrain没训练完，lora微调使用的是下载好的预训练权重。
```bash
cd /workspace/xav-models/modelzoo/Bunny/script/train
sh finetune_lora.sh
```
⚠注意

lora微调必须按上面方式指定训练step数，使得训练结束代码正常退出，不可手动ctrl+c结束训练，手动终止训练会导致缺失结果数据文件，导致无法进行模型评估。

运行5000 steps大约需16小时，预期训练速度如图：
![lora微调](images/image3.png)

## 模型评估
使用 SEED-Bench-1 Evaluation, 参考: [Evaluation_lora Readme](https://github.com/BAAI-DCAI/Bunny/blob/main/script/eval/lora/evaluation_lora.md)

### 准备eval数据集

评估维度1-9的图像数据来自CC3M数据集链接是[SEED-Bench](https://huggingface.co/datasets/AILab-CVC/SEED-Bench)。

评价维度10-12的视频数据来自Something-Something v2、Epic-kitchen 100 和 Breakfast 数据集。

数据集已处理好上传bos，下载解压即可：
```bash
cd /workspace/xav-models/modelzoo/Bunny/eval/seed-bench/
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/eval/SEED-Bench-image.zip
unzip SEED-Bench-image.zip
wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/eval/SEED-Bench-video-image.tar
tar -xvf SEED-Bench-video-image.tar
```

### 更新代码
注入eval相关的patch, 并更新代码：
```bash
cd /workspace/xav-models
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/xav/v0.2/Bunny-eval.patch
patch -p1 < Bunny-eval.patch 
```

### 执行评估
```bash
cd /workspace/xav-models/modelzoo/Bunny

# 训练和eval脚本启动路径不一致，因此修改 merged_model/config.json
vim script/train/checkpoints-llama3-8b/bunny-lora-llama3-8b-fp16/config.json +20
  "mm_vision_tower": "./siglip-so400m-patch14-384",
 # 改为
  "mm_vision_tower": "./script/train/siglip-so400m-patch14-384",
  
 # 开始eval，大约10分钟完成 
 CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 bash script/eval/lora/seedbench.sh
```
GPU结果如图：
![GPU结果](images/image4.png)

XPU结果如图：
![XPU结果](images/image5.png)

Evaluation 结果对比如图：
![结果对比](images/image6.png)

