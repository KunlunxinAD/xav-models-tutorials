# Mask2former
## 准备环境

请联系昆仑芯客户支持获取开发环境镜像。

## 配置环境

### 启动容器
```
export XAV_IMAGE=<XAV_IMAGE>
export XAV_CONTAINER=xav-test-mask2former
export MODEL_PATH=</path/to/model>

docker run -itd --privileged --net=host \
    -w /workspace/xav-models/modelzoo \
    -v ${MODEL_PATH}:/home \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${XAV_CONTAINER} \
    --shm-size 128g \
    ${XAV_IMAGE} \
    bash
 
docker exec -it ${XAV_CONTAINER} bash
```
容器中使用默认conda虚拟环境：python38_torch201_cuda

### 下载资源
执行以下步骤进行资源下载：
1. 下载代码并解压
    ```
    cd /home
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/Mask2Former.tar.gz
    tar -zxvf Mask2Former.tar.gz && rm Mask2Former.tar.gz
    ```
2. 准备训练权重
    ```
    # r50 backbone, 该权重为官方权重备份，如官方下载网络较慢，可以从bos云下载
    mkdir -p /root/.cache/torch/hub/checkpoints/
    wget -O /root/.cache/torch/hub/checkpoints/resnet50-0676ba61.pth http://klx-sdk-release-public.su.bcebos.com/xav/data/resnet50-0676ba61.pth
    ```
3. 准备数据集
    ```
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/coco_mask2former.tar.gz
    tar -xvf coco_mask2former.tar.gz  && rm coco_mask2former.tar.gz
    mv coco/ /home/Mask2Former/datasets/
    ```
4. 安装依赖
    ```
    # 安装panopticapi
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/panopticapi.tar.gz
    tar -xvf panopticapi.tar.gz  && rm panopticapi.tar.gz
    python -m pip install -e panopticapi
    # 安装fvcore
    pip install fvcore
    # 安装omegaconf
    pip install omegaconf
    # 安装detectron2
    cd /home/Mask2Former
    git clone https://github.com/facebookresearch/detectron2.git
    python -m pip install -e detectron2
    ```
## 训练测试
### 注入修改patch
    ```
    wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/d2_launch.patch
    patch /home/Mask2Former/detectron2/detectron2/engine/launch.py < d2_launch.patch
    ```

### 执行训练
单机8卡训练脚本如下所示：
    ```
    #在home目录下建立文件train_mask2former_kl3_8_devices.sh

    if [[ "$ROOT_PATH" = ""  ]]; then
        ROOT_PATH="/home/Mask2Former"
    fi
    echo $ROOT_PATH
    pushd $ROOT_PATH

    export BKCL_XLINK_C2C=1
    export XPU_ZEBU_MODE=1

    export XMLIR_CUDNN_ENABLED=1
    export CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7'
    export XMLIR_ENABLE_LINEAR_FC_FUSION=0
    python train_net.py --num-gpus 8 --config  /home/Mask2Former/configs/coco/panoptic-segmentation/maskformer2_R50_bs16_50ep.yaml
    unset XMLIR_ENABLE_LINEAR_FC_FUSION
    unset CUDA_VISIBLE_DEVICES
    unset XMLIR_CUDNN_ENABLED
    unset BKCL_XLINK_C2C
    unset XPU_ZEBU_MODE
    popd
    ```
执行训练脚本：
    ```
    bash /home/train_mask2former_kl3_8_devices.sh
    ```