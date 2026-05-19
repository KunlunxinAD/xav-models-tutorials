# Source Summary: PETR Trainval Guide

## Source

- [`PETR_trainval.md`](../../tutorials/PETR_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | PETR |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | MMDetection3D |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | nuScenes |

## Source outline

- # PETR Trainval Guide
- ## 环境准备
- ### 镜像
- ### 创建docker
- ### 准备数据集
- ### mmcv环境适配
- ### 模型代码
- ### 数据集&权重
- ## 启动脚本
- ### train
- ### test

## Representative commands

- `docker run -d -p 8000:8081 -it          \`
- `cd /data/Dataset/nuScenes`
- `wget -P ${PREFIX} -c ${BASE_URL}/HDmaps-final_infos_train.pkl`
- `wget -P ${PREFIX} -c ${BASE_URL}/HDmaps-final_infos_val.pkl`
- `wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_2f_infos_train.pkl`
- `wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_2f_infos_val.pkl`
- `wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_30f_infos_train.pkl`
- `wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_30f_infos_val.pkl`
- `wget -P ${PREFIX} -c ${BASE_URL}/mmdet3d_nuscenes_30f_infos_test.pkl`
- `wget -P ${PREFIX} -c ${BASE_URL}/nuscenes_infos_train.pkl`
- `wget -P ${PREFIX} -c ${BASE_URL}/nuscenes_infos_val.pkl`
- `wget -P ${PREFIX} -c ${BASE_URL}/HDmaps-final.tar`

## URLs mentioned

- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/PETR_nuscenes_data>
- <https://gitee.com/open-mmlab/mmdetection3d.git>
- <https://github.com/open-mmlab/mmdetection3d.git>
- <https://github.com/megvii-research/PETR.git>

## Related derived pages

- [`PETR`](../models/PETR.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
