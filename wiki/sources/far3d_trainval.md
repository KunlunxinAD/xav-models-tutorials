# Source Summary: Far3D

## Source

- [`Far3D_trainval.md`](../../tutorials/Far3D_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Far3D |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | coco, Argoverse |

## Source outline

- # Far3D
- ## 准备环境
- ## 下载数据集
- ## 启动容器
- ## 下载及安装资源
- ## 训练与评估
- ### 执行单机8卡训练
- ### 执行四机32卡训练
- ### 单机8卡评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-Far3D` |
| `MODEL_PATH` | `</path/to/model>` |
| `DATASET_PATH` | `</path/to/dataset>` |
| `XPU_VISIBLE_DEVICES` | `0,1,2,3,4,5,6,7` |
| `BKCL_PCIE_TOPO` | `1` |
| `DEFORM_ATTN_L3` | `true` |
| `XPU_PCIE_SPEED_MODE` | `1` |
| `XPUAPI_XPU_TF32_ROUND_MODE` | `1` |
| `BKCL_ENABLE_XDR` | `1` |
| `BKCL_RDMA_FORCE_TREE` | `1` |
| `BKCL_ENABLE_TREE` | `1` |
| `BKCL_RDMA_VERBS` | `1` |
| `BKCL_RING_BUFFER_SIZE` | `8388608` |
| `BKCL_MULTI_TREE_THRESHOLD` | `-1 #tree` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0` |
| `BKCL_SOCKET_IFNAME` | `ens13np0` |

## Representative commands

- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/xmlir-1.0.0.1-cp38-cp38-linux_x86_64.whl`
- `pip uninstall -y xmlir`
- `pip install xmlir-1.0.0.1-cp38-cp38-linux_x86_64.whl`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/xav_dsal-0.3.0-cp38-cp38-linux_x86_64.whl`
- `pip uninstall -y xav_dsal`
- `pip install xav_dsal-0.3.0-cp38-cp38-linux_x86_64.whl`
- `pip install av2==0.2.1 refile kornia`
- `cd /home`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/Far3D.tar.gz`
- `cd /home/Far3D`

## URLs mentioned

- <https://github.com/megvii-research/Far3D/blob/main/docs/get_started.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/xmlir-1.0.0.1-cp38-cp38-linux_x86_64.whl>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/xav_dsal-0.3.0-cp38-cp38-linux_x86_64.whl>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/Far3D.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/cascade_mask_rcnn_r50_fpn_coco-20e_20e_nuim_20201009_124951-40963960.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/dd3d_det_final.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/depth_pretrained_v99-3jlw0p36-20210423_010520-model_final-remapped.pth>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/far3d/fcos3d_vovnet_imgbackbone-remapped.pth>

## Related derived pages

- [`Far3D`](../models/Far3D.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
