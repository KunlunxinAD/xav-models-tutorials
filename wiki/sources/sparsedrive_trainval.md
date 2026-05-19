# Source Summary: SparseDrive

## Source

- [`SparseDrive_trainval.md`](../../tutorials/SparseDrive_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | SparseDrive |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | MMCV |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7 |
| Dataset hints | nuScenes |

## Source outline

- # SparseDrive
- ## 准备环境
- ## 下载数据集
- ## 启动容器
- ## 下载及安装资源
- ## 训练与评估
- ### 注入patch
- ### 执行训练
- #### 单机8卡训练
- #### 执行多机训练
- #### 单机8卡评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-SparseDrive` |
| `MODEL_PATH` | `</path/to/model>` |
| `DATASET_PATH` | `</path/to/dataset>` |
| `CUDA_VISIBLE_DEVICES` | `'0,1,2,3,4,5,6,7'` |
| `NNODES` | `4 #根据机器数量修改` |
| `NODE_RANK` | `0 #修改为每台机器的rank(0,1,2,3...)` |
| `PORT` | `29600` |
| `MASTER_ADDR` | `"172.22.182.81" #根据主节点IP修改` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改` |
| `BKCL_SOCKET_IFNAME` | `ens13np0 #根据机器网卡名修改` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `pip install motmetrics==1.1.3`
- `pip install more_itertools`
- `cd /home`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/SparseDrive.tar.gz`
- `cd /home/SparseDrive`
- `wget https://download.pytorch.org/models/resnet50-19c8e357.pth`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/resnet50-19c8e357.pth`
- `cd /home/SparseDrive/`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/infos.tar.gz`

## URLs mentioned

- <https://github.com/OpenDriveLab/UniAD/blob/main/docs/DATA_PREP.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/SparseDrive.tar.gz>
- <https://download.pytorch.org/models/resnet50-19c8e357.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/resnet50-19c8e357.pth>
- <https://github.com/swc-17/SparseDrive/blob/main/docs/quick_start.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/infos.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/kmeans.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/nuscenes_mot.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/text.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/SparseDrive/algo.patch>

## Related derived pages

- [`SparseDrive`](../models/SparseDrive.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
