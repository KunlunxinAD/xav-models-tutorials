# Source Summary: MapVR

## Source

- [`mapvr_trainval.md`](../../tutorials/mapvr_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | MapVR |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | nuScenes |

## Source outline

- # MapVR
- ## 准备环境
- ## 准备数据集及代码
- ### 准备数据集
- ### 下载预处理后的数据
- ### 下载代码及预训练权重
- ## 启动容器
- ### 配置容器内环境
- ## 多卡训练
- ### 预处理数据集
- ### 执行单机多卡训练
- ### 执行多机训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `CONTAINER_NAME` | `MapVR_test` |
| `PATH_TO_MOUNT` | `</path/to/mapvr> #本地路径` |
| `PATH_AFTER_MOUNT` | `/home/MapVR #挂载后在容器内的路径` |
| `NUSCENES_PATH` | `</path/to/nuscenes>` |
| `NNODES` | `4 #根据机器数量修改` |
| `NODE_RANK` | `0 #修改为每台机器的rank(0,1,2,3...)` |
| `PORT` | `29600` |
| `MASTER_ADDR` | `"172.22.182.81" #根据主节点IP修改` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改` |
| `BKCL_SOCKET_IFNAME` | `ens13np0 #根据机器网卡名修改` |

## Representative commands

- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/MapTR_data_nuscenes/can_bus.zip`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/MapTR_data_nuscenes/nuScenes-map-expansion-v1.3.zip`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/MapTR_data_nuscenes/nuscenes.tar.gz`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_train.pkl`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_val.pkl`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/mapvr/MapVR.tar.gz`
- `cd MapVR`
- `cd ckpts`
- `wget https://download.pytorch.org/models/resnet50-19c8e357.pth`
- `docker run -dti \`
- `docker exec -it ${CONTAINER_NAME} bash`
- `conda create -n MapVR_env --clone python38_torch201_cuda`

## URLs mentioned

- <https://github.com/ZhangGongjie/MapVR/blob/main/docs/prepare_dataset.md>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/MapTR_data_nuscenes/can_bus.zip>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/MapTR_data_nuscenes/nuScenes-map-expansion-v1.3.zip>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/MapTR_data_nuscenes/nuscenes.tar.gz>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_train.pkl>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_val.pkl>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/mapvr/MapVR.tar.gz>
- <https://download.pytorch.org/models/resnet50-19c8e357.pth>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/resnet50-19c8e357.pth>
- <https://download.pytorch.org/whl/torch_stable.html>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/maptr_v2/libxpu_blas.so_v0.5>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/mapvr/xav_dsal-0.3.0-cp38-cp38-linux_x86_64.whl>

## Related derived pages

- [`MapVR`](../models/MapVR.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
