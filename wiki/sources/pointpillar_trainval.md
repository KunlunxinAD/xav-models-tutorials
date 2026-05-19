# Source Summary: PointPillar

## Source

- [`PointPillar_trainval.md`](../../tutorials/PointPillar_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | PointPillar |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | MMCV, MMDetection3D |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | nuScenes |

## Source outline

- # PointPillar
- ## 准备环境
- ## 准备数据集及代码
- ### 准备数据集
- ### 下载模型代码
- ## 启动容器
- ## 准备环境
- ## 执行训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `PointPillar-test` |
| `NUSCENES_PATH` | `/path/to/nuscenes` |
| `PYTHONPATH` | `/workspace/mmdetection3d/:$PYTHONPATH` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/PointPillar.tar.gz`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `pip install mmcv==2.0.0rc4`
- `pip install mmdet==3.0.0`
- `pip install mmengine==0.8.0`
- `cd /workspace/mmdetection3d`
- `python tools/train.py configs/pointpillars/pointpillars_hv_fpn_sbn-all_8xb4-2x_nus-3d.py`
- `./tools/dist_train.sh configs/pointpillars/pointpillars_hv_fpn_sbn-all_8xb4-2x_nus-3d.py 8`

## URLs mentioned

- <https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2FOpenDriveLab%2FUniAD%2Fblob%2Fmain%2Fdocs%2FDATA_PREP.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/models/PointPillar.tar.gz>

## Related derived pages

- [`PointPillar`](../models/PointPillar.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
