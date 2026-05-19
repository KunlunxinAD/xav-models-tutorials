# Source Summary: QCNet

## Source

- [`QCNet_trainval.md`](../../tutorials/QCNet_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | QCNet |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Argoverse |

## Source outline

- # QCNet
- ## 准备环境
- ## 准备数据集及代码
- ### 下载模型代码
- ### 准备数据集
- ## 启动容器
- ### 配置容器内环境
- ## 多卡训练
- ### 执行单机多卡训练
- ### 执行多机训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `CONTAINER_NAME` | `QCNet_test` |
| `PATH_TO_MOUNT` | `</path/to/QCNet> #本地路径` |
| `PATH_AFTER_MOUNT` | `/home/QCNet #挂载后在容器内的路径` |
| `DATASET_PATH` | `</path/to/argoverse>` |
| `NNODES` | `4 #根据机器数量修改` |
| `NODE_RANK` | `0 #修改为每台机器的rank(0,1,2,3...)` |
| `PORT` | `29600` |
| `MASTER_ADDR` | `"172.22.182.81" #根据主节点IP修改` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改` |
| `BKCL_SOCKET_IFNAME` | `ens13np0 #根据机器网卡名修改` |

## Representative commands

- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/QCNet/QCNet.tar.gz`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/Argoverse2_Motion_Forecasting/motion_forecasting_processed.tar`
- `cd </path/to/argoverse>/motion_forecasting`
- `wget https://s3.amazonaws.com/argoverse/datasets/av2/tars/motion-forecasting/train.tar`
- `wget https://s3.amazonaws.com/argoverse/datasets/av2/tars/motion-forecasting/val.tar`
- `wget https://s3.amazonaws.com/argoverse/datasets/av2/tars/motion-forecasting/test.tar`
- `docker run -dti \`
- `docker exec -it ${CONTAINER_NAME} bash`
- `conda create -n QCNet --clone python38_torch201_cuda`
- `conda init bash`
- `conda activate QCNet`
- `cd /home/QCNet`

## URLs mentioned

- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/QCNet/QCNet.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/Argoverse2_Motion_Forecasting/motion_forecasting_processed.tar>
- <https://s3.amazonaws.com/argoverse/datasets/av2/tars/motion-forecasting/train.tar>
- <https://s3.amazonaws.com/argoverse/datasets/av2/tars/motion-forecasting/val.tar>
- <https://s3.amazonaws.com/argoverse/datasets/av2/tars/motion-forecasting/test.tar>
- <https://data.pyg.org/whl/torch-2.0.1+cu117.html>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/QCNet/torch_lightning.patch>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/QCNet/torch_cluster_v1.2.0.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/QCNet/torch_geometric.patch>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/QCNet/xpytorch-cp38-torch201-ubuntu2004-x64.run>

## Related derived pages

- [`QCNet`](../models/QCNet.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
