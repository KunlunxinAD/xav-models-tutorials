# Source Summary: Point Transformer V3 (PTv3)

## Source

- [`Ptv3_Trainval_Guide.md`](../../tutorials/Ptv3_Trainval_Guide.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Point Transformer V3 (PTv3) |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | Pointcept, MMCV |
| Precision mentions | AMP/FP16 (mmcv_amp_fp16 patch), BF16 round mode env |
| Device hints | 4 cards (`CUDA_VISIBLE_DEVICES=0,1,2,3`, `train.sh -g 4`) |
| Dataset hints | nuScenes |

## Source outline

- # Point Transformer V3 (Ptv3) Trainval Guide
- ## 概述
- ## 准备环境
- ## 下载数据集
- ## 启动容器
- ## 下载及安装依赖
- ## 执行训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `Ptv3-test` |
| `NUSCENES_PATH` | `/path/to/nuscenes` |
| `XMLIR_ENABLE_NEW_PG` | `1` |
| `XDNN_USE_FAST_GELU` | `1` |
| `XMLIR_BMM_DISPATCH_VALUE` | `2` |
| `XMLIR_ENABLE_LINEAR_FC_FUSION` | `1` |
| `XMLIR_ENABLE_FAST_FC` | `1` |
| `XPYTORCH_RUN_ENHANCE` | `1` |
| `XDNN_USE_FAST_SWISH` | `1` |
| `XDNN_FAST_DIV_SCALAR` | `true` |
| `XPUAPI_SDNN_BF16_ROUND_MODE` | `3` |
| `CUDA_VISIBLE_DEVICES` | `0,1,2,3` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `git clone https://github.com/Pointcept/Pointcept.git && cd Pointcept`
- `wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/Ptv_3/pt_v3_info.tar.gz`
- `cd /workspace/Pointcept/libs/pointops && python setup.py install`
- `pip install torch-scatter -f https://data.pyg.org/whl/torch-2.5.1+cu118.html`
- `git apply ptv3_xpu_adapt.patch`
- `patch -p1 < /workspace/Pointcept/mmcv_amp_fp16.patch`
- `sh scripts/train.sh -g 4 -d nuscenes -c semseg-pt-v3m1-0-base -n semseg-pt-v3m1-0-base`

## URLs mentioned

- <https://github.com/hustvl/VAD/blob/main/docs/prepare_dataset.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <https://github.com/Pointcept/Pointcept.git>
- <https://klx-sdk-release-public.su.bcebos.com/xav/release/models/Ptv_3/pt_v3_info.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/xav/release/models/Ptv_3/ptv3_xpu_adapt.patch>
- <https://klx-sdk-release-public.su.bcebos.com/xav/release/models/Ptv_3/mmcv_amp_fp16.patch>
- <https://data.pyg.org/whl/torch-2.5.1+cu118.html>

## Related derived pages

- [`PTv3`](../models/PTv3.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Two likely typos exist in the source training-environment block and are preserved verbatim in the tutorial: `export export XDNN_USE_FAST_GELU=1` (duplicate `export`) and `export XMLIR_ENABLE_FAST_FC=1wanqu` (trailing `wanqu`). Confirm the intended values with the maintainer before relying on this block.
- Benchmark, peak memory, throughput, and accuracy are not stated in the source and should be treated as unknown.
- Image tags and private environment details are intentionally not inferred.
