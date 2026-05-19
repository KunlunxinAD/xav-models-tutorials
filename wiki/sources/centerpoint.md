# Source Summary: CenterPoint Trainval Guide

## Source

- [`CenterPoint.md`](../../tutorials/CenterPoint.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | CenterPoint |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |
| Dataset hints | nuScenes |

## Source outline

- # CenterPoint Trainval Guide
- ## 准备环境
- ## 下载数据集
- ## 启动容器
- ## 下载及安装依赖
- ## 执行训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `CenterPoint-test` |
| `NUSCENES_PATH` | `/path/to/nuscenes` |
| `XMLIR_CONV_GEMM_DTYPE` | `"tfloat32"` |
| `XPUAPI_TF32_ROUND_MODE` | `"rne"` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/CenterPoint/centerpoint.tar.gz`
- `cd /workspace/centerpoint/ && mkdir data`
- `conda activate python310_torch25_cuda`
- `pip install --force-reinstall numpy==1.23.5`
- `pip install --force-reinstall numba==0.57.1`
- `cd /workspace/centerpoint`
- `python tools/train.py configs/centerpoint/centerpoint_pillar02_second_secfpn_8xb4-cyclic-20e_nus-3d.py`

## URLs mentioned

- <https://github.com/hustvl/VAD/blob/main/docs/prepare_dataset.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/models/CenterPoint/centerpoint.tar.gz>

## Related derived pages

- [`CenterPoint`](../models/CenterPoint.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
