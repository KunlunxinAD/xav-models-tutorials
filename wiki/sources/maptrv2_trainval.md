# Source Summary: MapTRv2

## Source

- [`maptrv2_trainval.md`](../../tutorials/maptrv2_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | MapTRv2 |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | nuScenes |

## Source outline

- # MapTRv2
- ## 准备环境
- ## 准备数据集及代码
- ### 准备数据集
- ### 下载预处理后的数据
- ### 下载代码及预训练权重
- ## 启动容器
- ### 配置容器内环境
- ## 多卡训练
- ### 执行单机多卡训练
- ### 执行多机训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `CONTAINER_NAME` | `MapTR_v2_test` |
| `PATH_TO_MOUNT` | `</path/to/maptr> #本地路径` |
| `PATH_AFTER_MOUNT` | `/home/MapTR #挂载后在容器内的路径` |
| `NUSCENES_PATH` | `</path/to/nuscenes>` |
| `NNODES` | `4 #根据机器数量修改` |
| `NODE_RANK` | `0 #修改为每台机器的rank(0,1,2,3...)` |
| `PORT` | `29600` |
| `MASTER_ADDR` | `"172.22.182.81" #根据主节点IP修改` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改` |
| `BKCL_SOCKET_IFNAME` | `ens13np0 #根据机器网卡名修改` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/maptr_v2_nuscenes_pkl.tar.gz`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/maptr_v2/MapTR_v2.tar.gz`
- `cd MapTR`
- `cd ckpts`
- `wget https://download.pytorch.org/models/resnet50-19c8e357.pth`
- `wget https://download.pytorch.org/models/resnet18-f37072fd.pth`
- `docker run -dti \`
- `docker exec -it ${CONTAINER_NAME} bash`
- `conda create -n MapTR_env --clone python38_torch201_cuda`
- `conda init bash`
- `conda activate MapTR_env`

## URLs mentioned

- <https://github.com/hustvl/MapTR/blob/maptrv2/docs/prepare_dataset.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/maptr_v2_nuscenes_pkl.tar.gz>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/maptr_v2/MapTR_v2.tar.gz>
- <https://download.pytorch.org/models/resnet50-19c8e357.pth>
- <https://download.pytorch.org/models/resnet18-f37072fd.pth>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/resnet50-19c8e357.pth>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/resnet18-f37072fd.pth>
- <https://download.pytorch.org/whl/torch_stable.html>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/maptr_v2/libxpu_blas.so_v0.5>

## Related derived pages

- [`MapTRv2`](../models/MapTRv2.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
