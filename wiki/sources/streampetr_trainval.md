# Source Summary: StreamPETR

## Source

- [`StreamPETR_trainval.md`](../../tutorials/StreamPETR_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | StreamPETR |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | flash_attn, MMCV, MMDetection3D |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # StreamPETR
- ## 准备环境
- ## 启动容器
- ## 下载及安装资源
- ## 环境适配
- ## 训练与评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `iregistry.baidu-int.com/kunlunxin-self-driving/xav-base:v1.0.0 #此处修改image版本` |
| `CONTAINER_NAME` | `xav-streampetr-test` |
| `MOUNT_PATH` | `/your/mount/path` |
| `NUSCENES_PATH` | `/your/path/to/nuscenes` |
| `XMLIR_BMM_DISPATCH_VALUE` | `1` |
| `XMLIR_CUDNN_ENABLED` | `0` |
| `BKCL_FORCE_SYNC` | `1` |

## Representative commands

- `docker run -dti \`
- `git clone https://github.com/exiawsh/StreamPETR.git`
- `pip install flash-attn==0.2.2`
- `pip install mmcv-full==1.6.0`
- `pip install mmdet==2.28.2`
- `pip install mmsegmentation==0.30.0`
- `cd StreamPETR`
- `git clone https://github.com/open-mmlab/mmdetection3d.git`
- `cd mmdetection3d`
- `git checkout v1.0.0rc6`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/can_bus.zip`

## URLs mentioned

- <https://github.com/exiawsh/StreamPETR.git>
- <https://github.com/open-mmlab/mmdetection3d.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/can_bus.zip>
- <https://klx-sdk-release-public.su.bcebos.com/xav/data/streampetr_data/nuscenes2d_temporal_infos_train.pkl>
- <https://klx-sdk-release-public.su.bcebos.com/xav/data/streampetr_data/nuscenes2d_temporal_infos_test.pkl>
- <https://klx-sdk-release-public.su.bcebos.com/xav/data/streampetr_data/nuscenes2d_temporal_infos_val.pkl>
- <https://klx-sdk-release-public.su.bcebos.com/xav/data/streampetr_data/fcos3d_vovnet_imgbackbone-remapped.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/maptr_v2/libxpu_blas.so_v0.5>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/streampetr_data/xpytorch-cp38-torch201-ubuntu2004-x64.run>

## Related derived pages

- [`StreamPETR`](../models/StreamPETR.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
