# Source Summary: RegNet Trainval Guide

## Source

- [`regnet_trainval.md`](../../tutorials/regnet_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | RegNet |
| Domain | Vision/OCR |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # RegNet Trainval Guide
- ## 环境准备
- ### PCIe环境配置（OAM跳过此步骤）
- #### 确定PCIe环境
- #### 配置PCIe的8卡互联模式
- ## 数据集及代码准备
- ### 数据集准备
- ### 下载代码
- ## 启动容器
- ### 容器内环境配置
- ## 单机多卡训练
- ### 设置训练路径与参数
- ### 执行训练
- ### 训练脚本内容示例
- ### 性能和精度统计，绘制训练曲线
- ## 多机多卡训练
- ### 设置训练路径与参数
- ### 执行训练
- ### 训练脚本内容示例

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `regnet_test` |
| `MODEL_PATH` | `</path/to/regnet> #本地路径` |
| `DATA_PATH` | `</path/to/data> #数据存储路径` |
| `XPU_PCIE_SPEED_MODE` | `1` |
| `XPU_VISIBLE_DEVICES` | `0,1,2,3,4,5,6,7` |
| `BKCL_PCIE_TOPO` | `1` |
| `XMLIR_CUDNN_ENABLED` | `1` |
| `XPUAPI_XPU_TF32_ROUND_MODE` | `1` |
| `BKCL_RING_BUFFER_GM` | `1` |
| `BKCL_RDMA_PROXY_DISABLE` | `1` |
| `BKCL_FLAT_RING` | `1` |
| `BKCL_ENABLE_XDR` | `1` |
| `BKCL_RDMA_FORCE_TREE` | `1` |
| `BKCL_TREE_THRESHOLD` | `0` |
| `BKCL_RDMA_NICS` | `ens1f0np0,ens1f0np0,ens2np0,ens2np0,ens3np0,ens3np0,ens12np0,ens12np0` |
| `BKCL_SOCKET_IFNAME` | `ens13np0` |

## Representative commands

- `wget https://klx-public.bj.bcebos.com/v1/kdp/datasets/ILSVRC2012.tar.gz`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/regnet/vision.zip`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda activate python38_torch201_cuda`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/maptr_v2/libxpu_blas.so_v0.5`
- `cd vision/reference/classification/`
- `bash train.sh`
- `torchrun --rdzv-endpoint=localhost:25678 --nproc_per_node=8 train.py --model regnet_x_16gf --data-path /data/ILSVRC2012/ --epochs 2 --batch-size 320 --output-di`
- `python analysis_log.py --logname log.txt`
- `bash train_dist.sh`
- `torchrun --nnodes=$NNODES --node_rank=$NODE_RANK --nproc_per_node=$GPUS --master_addr=$MASTER_ADDR --master_port=$PORT \`

## URLs mentioned

- <https://www.kaggle.com/datasets/thbdh5765/ilsvrc2012>
- <https://klx-public.bj.bcebos.com/v1/kdp/datasets/ILSVRC2012.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/regnet/vision.zip>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/maptr_v2/libxpu_blas.so_v0.5>

## Related derived pages

- [`RegNet`](../models/RegNet.md)
- [`basic-vision-trainval`](../recipes/basic-vision-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
