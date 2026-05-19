# Source Summary: MaskRCNN

## Source

- [`MaskRCNN_trainval.md`](../../tutorials/MaskRCNN_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | MaskRCNN |
| Domain | Vision/OCR |
| Workflow tags | Trainval |
| Frameworks / backends | Detectron2 |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | COCO |

## Source outline

- # MaskRCNN
- ## 准备环境
- ## 下载数据集
- ## 启动容器
- ## 下载及安装依赖
- ## 执行训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<path/to/image>` |
| `XAV_CONTAINER` | `Mask_rcnn-test` |
| `COCO2017_PATH` | `/path/to/COCO2017` |

## Representative commands

- `wget https://klx-public.bj.bcebos.com/v1/kdp/Guangqi-poc/faster_rcnn/COCO2017.tar.gz`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `git clone https://github.com/facebookresearch/detectron2.git`
- `cd detectron2/`
- `conda activate python38_torch201_cuda`
- `cd /workspace`
- `python -m pip install -e detectron2`
- `cd /workspace/detectron2/`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask_rcnn/d2_launch.patch`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask_rcnn/matcher.patch`
- `python tools/train_net.py --num-gpus 8  --config-file configs/COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_1x.yaml`

## URLs mentioned

- <https://klx-public.bj.bcebos.com/v1/kdp/Guangqi-poc/faster_rcnn/COCO2017.tar.gz>
- <https://github.com/facebookresearch/detectron2.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask_rcnn/d2_launch.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask_rcnn/matcher.patch>

## Related derived pages

- [`MaskRCNN`](../models/MaskRCNN.md)
- [`basic-vision-trainval`](../recipes/basic-vision-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
