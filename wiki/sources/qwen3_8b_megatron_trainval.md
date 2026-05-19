# Source Summary: Qwen3-8B Megatron Trainval Guide

## Source

- [`qwen3_8b_megatron_trainval.md`](../../tutorials/qwen3_8b_megatron_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen3-8B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT |
| Frameworks / backends | Megatron, wandb |
| Precision mentions | BF16 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen3-8B Megatron Trainval Guide
- ## 环境准备
- ### 准备开发环境镜像
- ### PCIe环境配置（OAM跳过此步骤）
- #### 确定PCIe环境
- #### 配置PCIe的8卡互联模式
- ## 数据集及代码准备
- ### 数据集准备
- ### 下载代码及预训练权重
- ## 启动容器
- ### 容器内环境配置
- # 单机多卡训练
- ### 设置训练路径与参数
- ### 参考脚本配置
- ### 执行pretrain

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `Qwen3_8B_test` |
| `MODEL_PATH` | `</path/to/qwen3_8b> #本地路径` |
| `DIST_MULTI_STREAM` | `true # 开启多流` |
| `CUDA_DEVICE_MAX_CONNECTIONS` | `1` |
| `XMLIR_DISABLE_CUDA_ALLOCATOR` | `true` |
| `XPU_FORCE_USERMODE_LAUNCH` | `1` |
| `CUDA_DEVICE_ORDER` | `OAM_ID # 用于通讯建环` |
| `XBLAS_FC_HBM_VERSION` | `40 # 需要指定HBM版` |
| `XDNN_HIGH_ACCRACY_FP32_TO_BF16` | `true` |
| `BKCL_TREE_THRESHOLD` | `1048576` |
| `BKCL_CCIX_BUFFER_GM` | `1` |
| `BKCL_ENABLE_XDR` | `1` |
| `BKCL_FLAT_RING` | `1` |
| `BKCL_RDMA_PROXY_DISABLE` | `1` |
| `BKCL_XLINK_D2D` | `0` |
| `BKCL_XLINK_ETH` | `0` |
| `BKCL_RDMA_FORCE_TREE` | `1` |
| `XMLIR_DIST_ASYNC_ISEND_IRECV` | `1` |
| `XMLIR_PARALLEL_SAVE_MEMORY` | `false # 为false显存占用会多, 但会有性能提升; 为true显存会少, 但性能会下降` |

## Representative commands

- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/modelzoo_data/data/aiak_megatron_core/qwen3_data.tar.gz`
- `cd /home`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/modelzoo_data/data/Qwen3-32B.tar.gz`
- `wget https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/AIAK-Megatron.tar`
- `wget https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/AIAK-Training-LLM.tar`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `wget https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `bash xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `pip install numpy==1.26.4`
- `torchrun ${DISTRIBUTED_ARGS[@]} \`

## URLs mentioned

- <https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/modelzoo_data/data/aiak_megatron_core/qwen3_data.tar.gz>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/modelzoo_data/data/Qwen3-32B.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/AIAK-Megatron.tar>
- <https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/AIAK-Training-LLM.tar>
- <https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/xpytorch-cp310-torch251-ubuntu2004-x64.run>

## Related derived pages

- [`Qwen3-8B`](../models/Qwen3-8B.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)
- [`megatron-pretrain`](../recipes/megatron-pretrain.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
