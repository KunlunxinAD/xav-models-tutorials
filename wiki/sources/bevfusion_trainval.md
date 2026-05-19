# Source Summary: BEVFusion-MMDetection3D

## Source

- [`bevfusion_trainval.md`](../../tutorials/bevfusion_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | BEVFusion-MMDetection3D |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | MMDetection3D |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | nuScenes |

## Source outline

- # BEVFusion-MMDetection3D
- ## 概述
- ## 准备环境
- ## 准备数据集
- ## 启动容器
- ## 资源下载及安装
- ## 容器环境更新
- ## 训练与评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `CONTAINER_NAME` | `xav-bevfusion-mmdet3d` |
| `MOUNT_PATH` | `</path/to/workspace>` |
| `NUSCENES_PATH` | `</path/to/nuscenes>` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `docker run -dti \`
- `cd /home/bevfusion-mmdet3d`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/mmdetection3d.tar.gz`
- `cd /home/bevfusion-mmdet3d/mmdetection3d/`
- `cd pretrained`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/pretrained/swint-nuimages-pretrained.pth`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/pretrained/bevfusion_lidar_voxel0075_second_secfpn_8xb4-cyclic-20e_nus-3d-2628`
- `cd /home/bevfusion-mmdet3d/mmdetection3d/data`
- `conda activate python310_torch25_cuda`
- `pip uninstall spconv`
- `pip install numba==0.56.4`

## URLs mentioned

- <https://github.com/open-mmlab/mmdetection3d/blob/1.0/docs/en/data_preparation.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/mmdetection3d.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/pretrained/swint-nuimages-pretrained.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/bevfusion/pretrained/bevfusion_lidar_voxel0075_second_secfpn_8xb4-cyclic-20e_nus-3d-2628f933.pth>

## Related derived pages

- [`BEVFusion-MMDetection3D`](../models/BEVFusion-MMDetection3D.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
