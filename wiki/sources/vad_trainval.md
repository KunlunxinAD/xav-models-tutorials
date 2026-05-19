# Source Summary: VAD

## Source

- [`VAD_trainval.md`](../../tutorials/VAD_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | VAD |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7; CUDA_VISIBLE_DEVICES=0 |
| Dataset hints | nuScenes |

## Source outline

- # VAD
- ## 准备环境
- ## 下载数据集
- ## 启动容器
- ## 下载及安装资源
- ## 训练与评估
- ### 注入patch
- ### 执行训练
- ### 单机8卡训练
- ## 执行脚本
- ## 执行脚本
- ## 执行脚本
- ### 四机32卡训练
- ## 执行脚本
- ## 执行脚本
- ## 执行脚本
- ### 评估单机单卡
- ## 执行脚本

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-VAD` |
| `MODEL_PATH` | `</path/to/model>` |
| `DATASET_PATH` | `</path/to/dataset>` |
| `CUDA_VISIBLE_DEVICES` | `'0,1,2,3,4,5,6,7'` |
| `XMLIR_CUDNN_ENABLED` | `1` |
| `XMLIR_ENABLE_XBLAS_ADDMM` | `0` |
| `XMLIR_BMM_DISPATCH_VALUE` | `1` |
| `DEFORM_ATTN_L3` | `true` |
| `XPU_PCIE_SPEED_MODE` | `1` |
| `XPUAPI_XPU_TF32_ROUND_MODE` | `1` |
| `BKCL_PCIE_TOPO` | `1` |
| `BKCL_RDMA_FORCE_TREE` | `1` |
| `BKCL_ENABLE_TREE` | `1` |
| `BKCL_RDMA_VERBS` | `1` |
| `BKCL_RING_BUFFER_SIZE` | `8388608` |
| `BKCL_MULTI_TREE_THRESHOLD` | `-1 #tree` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0` |
| `BKCL_SOCKET_IFNAME` | `ens13np0` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `cd </path/to/nuscenes>`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/vad_nuscenes_infos_temporal_train.pkl`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/vad_nuscenes_infos_temporal_val.pkl`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `cd /home`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/VAD.tar.gz`
- `cd /home/VAD`
- `wget https://download.pytorch.org/models/resnet50-19c8e357.pth`
- `pip install similaritymeasures==0.7.0`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/nuscenes_data_classes.patch`

## URLs mentioned

- <https://github.com/hustvl/VAD/blob/main/docs/prepare_dataset.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/vad_nuscenes_infos_temporal_train.pkl>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/vad_nuscenes_infos_temporal_val.pkl>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/VAD.tar.gz>
- <https://download.pytorch.org/models/resnet50-19c8e357.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/vad/nuscenes_data_classes.patch>

## Related derived pages

- [`VAD`](../models/VAD.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
