# Source Summary: PETRv2

## Source

- [`petrv2_trainval.md`](../../tutorials/petrv2_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | PETRv2 |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | MMDetection3D |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | nuScenes |

## Source outline

- # PETRv2
- ## 准备环境
- ## 启动容器
- ## 下载及安装资源
- ### 下载PETRv2代码
- ### 安装mmdetection
- ### 下载预训练权重
- ### 下载及配置nuScenes数据集
- ## TrainVal 模型教程
- ### 模型训练
- ### 模型评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `</XAV_IMAGE>` |
| `CONTAINER_NAME` | `xav-petrv2-test` |
| `MOUNT_PATH` | `</path/to/petrv2>` |
| `NUSCENES_PATH` | `</path/to/dataset>` |

## Representative commands

- `docker run -dti \`
- `docker exec -it ${CONTAINER_NAME} bash`
- `conda create -n PETRv2 --clone python38_torch201_cuda`
- `conda init bash`
- `conda activate PETRv2`
- `cd /workspace/ #选择拉取代码路径,可任意修改`
- `cd PETR_DIR`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/PETRv2/PETRv2.tar.gz`
- `cd /workspace/PETR_DIR/`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/xav/v0.2/mmdetection3d.tar.gz`
- `cd mmdetection3d`
- `git checkout v0.17.1 #切换mmdetection分支`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/PETRv2/PETRv2.tar.gz>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/xav/v0.2/mmdetection3d.tar.gz>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/PETR_nuscenes_data>
- <https://github.com/open-mmlab/mmdetection3d/blob/master/docs/en/data_preparation.md>

## Related derived pages

- [`PETRv2`](../models/PETRv2.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
