# Source Summary: VIT

## Source

- [`VIT_trainval.md`](../../tutorials/VIT_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | VIT |
| Domain | Vision/OCR |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | COCO |

## Source outline

- # VIT
- ## 准备环境
- ## 启动容器
- ## 下载及安装资源
- ### 下载代码
- ### 下载数据集
- ## TrainVal 模型教程
- ### 单机8卡
- ### 四机32卡

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `iregistry.baidu-int.com/kunlunxin-self-driving/xav-base:v1.0.0rc1` |
| `CONTAINER_NAME` | `xav-vit2-test` |
| `MOUNT_PATH` | `/ssd2/xiayang02` |
| `DATA_PATH` | `/ssd1/` |
| `XMLIR_CUDNN_ENABLED` | `1` |
| `XMLIR_ENABLE_XBLAS_ADDMM` | `0` |
| `XMLIR_BMM_DISPATCH_VALUE` | `1` |
| `DEFORM_ATTN_L3` | `true` |
| `XPU_PCIE_SPEED_MODE` | `1` |
| `XPUAPI_XPU_TF32_ROUND_MODE` | `1` |
| `BKCL_PCIE_TOPO` | `1` |
| `BKCL_RING_BUFFER_GM` | `1` |
| `BKCL_RDMA_PROXY_DISABLE` | `1` |
| `BKCL_FLAT_RING` | `1` |
| `BKCL_ENABLE_XDR` | `1` |
| `BKCL_RDMA_FORCE_TREE` | `1` |
| `BKCL_TREE_THRESHOLD` | `0      # 关闭单机tree模式，避免某些同步issue` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0` |
| `BKCL_SOCKET_IFNAME` | `ens13np0` |

## Representative commands

- `docker run -dti \`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/VIT/vision.zip`
- `wget https://klx-public.bj.bcebos.com/v1/kdp/datasets/ILSVRC2012.tar.gz`
- `wget https://klx-public.bj.bcebos.com/v1/kdp/Guangqi-poc/faster_rcnn/COCO2017.tar.gz`
- `cd /home/vision/references/classification/`
- `bash train_1x8_vit.sh`
- `torchrun --nnodes=$NNODES --node_rank=$NODE_RANK --nproc_per_node=$GPUS --master_addr=$MASTER_ADDR --master_port=$PORT \`
- `bash train_4x8_vit.sh`

## URLs mentioned

- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/VIT/vision.zip>
- <https://klx-public.bj.bcebos.com/v1/kdp/datasets/ILSVRC2012.tar.gz>
- <https://klx-public.bj.bcebos.com/v1/kdp/Guangqi-poc/faster_rcnn/COCO2017.tar.gz>

## Related derived pages

- [`VIT`](../models/VIT.md)
- [`basic-vision-trainval`](../recipes/basic-vision-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
