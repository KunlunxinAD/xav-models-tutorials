# Source Summary: BEVDet

## Source

- [`bevdet_trainval.md`](../../tutorials/bevdet_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | BEVDet |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # BEVDet
- ## 准备环境
- ## 准备数据集及代码
- ### 准备数据集
- ### 下载数据集pkl
- ### 下载模型代码及预训练权重
- ## 启动容器
- ### 配置容器内环境
- ## 训练与评估
- ### 执行训练
- ### 执行单机8卡评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `BEVDet-test` |
| `NUSCENES_PATH` | `/path/to/nuscenes` |
| `PYTHONPATH` | `"/path/to/BEVDet:${PYTHONPATH}"` |

## Representative commands

- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bevdetv3-nuscenes_infos_train.pkl`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bevdetv3-nuscenes_infos_val.pkl`
- `wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/bevdet/BEVDet.tar.gz`
- `wget -O /root/.cache/torch/hub/checkpoints/resnet50-0676ba61.pth http://klx-sdk-release-public.su.bcebos.com/xav/data/resnet50-0676ba61.pth`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `pip uninstall mmdet3d`
- `cd /path/to/BEVDet`
- `bash ./tools/dist_train.sh configs/bevdet/bevdet-r50-4d-depth-cbgs.py 8`
- `bash tools/dist_test.sh configs/bevdet/bevdet-r50-4d-depth-cbgs.py \`

## URLs mentioned

- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bevdetv3-nuscenes_infos_train.pkl>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bevdetv3-nuscenes_infos_val.pkl>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/models/bevdet/BEVDet.tar.gz>
- <http://klx-sdk-release-public.su.bcebos.com/xav/data/resnet50-0676ba61.pth>

## Related derived pages

- [`BEVDet`](../models/BEVDet.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
