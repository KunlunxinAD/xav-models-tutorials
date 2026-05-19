# Source Summary: Mask2former

## Source

- [`mask2former_trainval.md`](../../tutorials/mask2former_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Mask2former |
| Domain | Vision/OCR |
| Workflow tags | Trainval |
| Frameworks / backends | Detectron2 |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7 |
| Dataset hints | coco |

## Source outline

- # Mask2former
- ## 准备环境
- ## 配置环境
- ### 启动容器
- ### 下载资源
- ## 训练测试
- ### 注入修改patch
- ### 执行训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-mask2former` |
| `MODEL_PATH` | `</path/to/model>` |
| `BKCL_XLINK_C2C` | `1` |
| `XPU_ZEBU_MODE` | `1` |
| `XMLIR_CUDNN_ENABLED` | `1` |
| `CUDA_VISIBLE_DEVICES` | `'0,1,2,3,4,5,6,7'` |
| `XMLIR_ENABLE_LINEAR_FC_FUSION` | `0` |

## Representative commands

- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `cd /home`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/Mask2Former.tar.gz`
- `wget -O /root/.cache/torch/hub/checkpoints/resnet50-0676ba61.pth http://klx-sdk-release-public.su.bcebos.com/xav/data/resnet50-0676ba61.pth`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/coco_mask2former.tar.gz`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/panopticapi.tar.gz`
- `python -m pip install -e panopticapi`
- `cd /home/Mask2Former`
- `git clone https://github.com/facebookresearch/detectron2.git`
- `python -m pip install -e detectron2`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/d2_launch.patch`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/Mask2Former.tar.gz>
- <http://klx-sdk-release-public.su.bcebos.com/xav/data/resnet50-0676ba61.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/coco_mask2former.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/panopticapi.tar.gz>
- <https://github.com/facebookresearch/detectron2.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mask2former/d2_launch.patch>

## Related derived pages

- [`Mask2former`](../models/Mask2former.md)
- [`basic-vision-trainval`](../recipes/basic-vision-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
