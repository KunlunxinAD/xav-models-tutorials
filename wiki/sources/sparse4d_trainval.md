# Source Summary: Sparse4D

## Source

- [`sparse4d_trainval.md`](../../tutorials/sparse4d_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Sparse4D |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | MMCV |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | nuScenes |

## Source outline

- # Sparse4D
- ## 环境准备
- ## 数据集及代码准备
- ### 数据集准备
- ### 下载代码及预训练权重
- ### 下载预处理后的数据
- ## 启动容器
- ### 容器内环境配置
- ## 多卡训练
- ### 数据集预处理
- # 如果在“载预处理后的数据”章节中已经下载预处理后的数据，可以跳过此步骤
- # 单机8卡训练
- # 单机评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `CONTAINER_NAME` | `Sparse4D_test` |
| `PATH_TO_MOUNT` | `</path/to/sparse4d> #本地路径` |
| `NUSCENES_PATH` | `</path/to/nuscenes>` |
| `OPENBLAS_NUM_THREADS` | `64` |
| `OMP_NUM_THREADS` | `64` |
| `NNODES` | `4 #根据机器数量修改` |
| `NODE_RANK` | `0 #修改为每台机器的rank(0,1,2,3...)` |
| `PORT` | `29600` |
| `MASTER_ADDR` | `"172.22.182.81" #根据主节点IP修改` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改` |
| `BKCL_SOCKET_IFNAME` | `ens13np0 #根据机器网卡名修改` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/Sparse4D.tar.gz`
- `cd Sparse4D`
- `wget https://download.pytorch.org/models/resnet50-19c8e357.pth`
- `cd ../Sparse4D`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_kmeans900.npy`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_anno_pkls/nuscenes_infos_test.pkl`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_anno_pkls/nuscenes_infos_train.pkl`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_anno_pkls/nuscenes_infos_val.pkl`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${CONTAINER_NAME} bash`
- `cd /home/Sparse4D`

## URLs mentioned

- <https://github.com/HorizonRobotics/Sparse4D/blob/main/docs/quick_start.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/Sparse4D.tar.gz>
- <https://download.pytorch.org/models/resnet50-19c8e357.pth>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/maptr_v2_data/resnet50-19c8e357.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_kmeans900.npy>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_anno_pkls/nuscenes_infos_test.pkl>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_anno_pkls/nuscenes_infos_train.pkl>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/nuscenes_anno_pkls/nuscenes_infos_val.pkl>
- <https://download.pytorch.org/whl/cu117>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/patch/nuscenes_mot.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/sparse4d/patch/mmcv_logger.patch>

## Related derived pages

- [`Sparse4D`](../models/Sparse4D.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
