# Source Summary: UniAD

## Source

- [`UniAD_trainval.md`](../../tutorials/UniAD_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | UniAD |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | MMCV |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7 |
| Dataset hints | nuScenes |

## Source outline

- # UniAD
- ## 准备环境
- ## 准备数据集
- ## 启动容器
- ## 下载及安装资源
- ## 训练与评估
- ### 注入patch
- ### 执行训练
- #### 单机8卡训练
- #### 单机8卡评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-UniAD` |
| `MODEL_PATH` | `</path/to/model>` |
| `DATASET_PATH` | `</path/to/dataset>` |
| `XPYTORCH_RUN_ENHANCE` | `1` |
| `CUDA_VISIBLE_DEVICES` | `'0,1,2,3,4,5,6,7'` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `cd /home`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/UniAD_v1.tar.gz`
- `cd /home/UniAD`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/data.tar.gz`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/ckpts.tar.gz`
- `pip install motmetrics==1.1.3 casadi torchmetrics==1.4.1`
- `pip install pytorch-lightning==1.2.5 --no-deps`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/nuscenes_data_classes.patch`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/nuscenes_mot.patch`

## URLs mentioned

- <https://github.com/OpenDriveLab/UniAD/blob/main/docs/DATA_PREP.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/UniAD_v1.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/data.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/ckpts.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/nuscenes_data_classes.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/nuscenes_mot.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/torchmetrics_metric.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/multi_scale_deform_attn.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/uniad/env_patch/focal_loss.patch>

## Related derived pages

- [`UniAD`](../models/UniAD.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
