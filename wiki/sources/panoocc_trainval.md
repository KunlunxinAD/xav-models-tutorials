# Source Summary: PanoOcc

## Source

- [`panoocc_trainval.md`](../../tutorials/panoocc_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | PanoOcc |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | nuScenes |

## Source outline

- # PanoOcc
- ## 准备环境
- ## 准备数据集及代码
- ### 准备数据集
- ### 预处理数据集
- ### 下载代码及预训练权重
- ## 启动容器
- ## 训练与评估
- ### 执行单机多卡训练
- ### 执行多机训练
- ### 执行评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `panoocc-test` |
| `NUSCENES_PATH` | `</path/to/nuscenes>` |
| `NNODES` | `4 #根据机器数量修改` |
| `NODE_RANK` | `0 #修改为每台机器的rank(0,1,2,3...)` |
| `PORT` | `29600` |
| `MASTER_ADDR` | `"172.22.182.81" #根据主节点IP修改` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0 #根据机器网卡名修改` |
| `BKCL_SOCKET_IFNAME` | `ens13np0 #根据机器网卡名修改` |

## Representative commands

- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuScenes-panoptic-v1.0-all.tar.gz`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuScenes-lidarseg-all-v1.0.tar.bz2`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_train.pkl`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_val.pkl`
- `wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/pano0cc/PanoOcc.tar.gz`
- `wget -O ./PanoOcc/ckpts/r101_dcn_fcos3d_pretrain.pth \`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `pip install torch-scatter`
- `pip uninstall xmlir -y`
- `pip install http://klx-sdk-release-public.su.bcebos.com/xav/release/xmlir/2024_08_19/xmlir-1.0.0.1-cp38-cp38-linux_x86_64.whl`
- `cd /path/to/PanoOcc`

## URLs mentioned

- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuScenes-panoptic-v1.0-all.tar.gz>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuScenes-lidarseg-all-v1.0.tar.bz2>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_train.pkl>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_val.pkl>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/models/pano0cc/PanoOcc.tar.gz>
- <http://klx-sdk-release-public.su.bcebos.com/xav/data/r101_dcn_fcos3d_pretrain.pth>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/xmlir/2024_08_19/xmlir-1.0.0.1-cp38-cp38-linux_x86_64.whl>

## Related derived pages

- [`PanoOcc`](../models/PanoOcc.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
