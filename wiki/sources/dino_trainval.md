# Source Summary: DINO

## Source

- [`DINO_trainval.md`](../../tutorials/DINO_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | DINO |
| Domain | Vision/OCR |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7 |
| Dataset hints | COCO |

## Source outline

- # DINO
- ## 准备环境
- ## 启动容器
- ## 下载模型及安装资源
- ## 权重准备
- ## 数据集准备
- ## 训练与测试

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-DINO` |
| `MODEL_PATH` | `</path/to/model>` |
| `DATASET_PATH` | `</path/to/dataset>` |
| `XMLIR_CUDNN_ENABLED` | `0` |
| `CUDA_VISIBLE_DEVICES` | `'0,1,2,3,4,5,6,7'` |

## Representative commands

- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `cd /home`
- `git clone https://github.com/open-mmlab/mmdetection.git`
- `git fetch --tags`
- `git checkout v3.3.0`
- `pip install numpy==1.26.4`
- `wget https://github.com/SwinTransformer/storage/releases/download/v1.0.0/swin_large_patch4_window12_384_22k.pth`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/COCO2017.tar.gz`
- `cd /home/mmdetection`
- `bash ./tools/dist_train.sh \`

## URLs mentioned

- <https://github.com/open-mmlab/mmdetection.git>
- <https://github.com/SwinTransformer/storage/releases/download/v1.0.0/swin_large_patch4_window12_384_22k.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/COCO2017.tar.gz>

## Related derived pages

- [`DINO`](../models/DINO.md)
- [`basic-vision-trainval`](../recipes/basic-vision-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
