# Source Summary: FlashOCC

## Source

- [`flashocc_trainval.md`](../../tutorials/flashocc_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | FlashOCC |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | MMCV, MMDetection3D, wandb |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # FlashOCC
- ## 准备环境
- ## 准备数据集及代码
- ### 下载模型代码及预训练权重
- ### 下载数据集pkl
- ## 启动容器
- ### 配置容器内环境
- ## 训练与评估
- ### 执行训练
- ### 执行单机8卡评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `FlashOCC-test` |
| `NUSCENES_PATH` | `/path/to/nuscenes` |
| `PYTHONPATH` | `$PWD:$PYTHONPATH` |
| `XMLIR_CONV_GEMM_DTYPE` | `float` |
| `XMLIR_BMM_DISPATCH_VALUE` | `1` |

## Representative commands

- `wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/flashocc/FlashOCC.tar.gz`
- `cd /path/to/FlashOCC`
- `cd ckpts`
- `wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/flashocc/pretrain_model/bevdet-r50-cbgs.pth`
- `cd data/nuscenes`
- `wget https://klx-sdk-release-public.su.bcebos.com/xav/data/bevdetv2-nuscenes_infos_train.pkl`
- `wget https://klx-sdk-release-public.su.bcebos.com/xav/data/bevdetv2-nuscenes_infos_val.pkl`
- `wget https://klx-sdk-release-public.su.bcebos.com/xav/data/gts.tar.gz`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `pip install mmcv-full==1.7.0`
- `pip install wandb==0.16.6`

## URLs mentioned

- <http://klx-sdk-release-public.su.bcebos.com/xav/release/models/flashocc/FlashOCC.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/xav/release/models/flashocc/pretrain_model/bevdet-r50-cbgs.pth>
- <https://klx-sdk-release-public.su.bcebos.com/xav/data/bevdetv2-nuscenes_infos_train.pkl>
- <https://klx-sdk-release-public.su.bcebos.com/xav/data/bevdetv2-nuscenes_infos_val.pkl>
- <https://klx-sdk-release-public.su.bcebos.com/xav/data/gts.tar.gz>
- <https://github.com/open-mmlab/mmdetection3d.git>
- <http://klx-sdk-release-public.su.bcebos.com/xav/data/resnet50-0676ba61.pth>

## Related derived pages

- [`FlashOCC`](../models/FlashOCC.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
