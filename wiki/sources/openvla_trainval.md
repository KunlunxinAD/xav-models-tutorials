# Source Summary: OpenVLA Trainval Guide

## Source

- [`openvla_trainval.md`](../../tutorials/openvla_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | OpenVLA |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT |
| Frameworks / backends | flash_attn, wandb |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |
| Dataset hints | Not explicitly extracted |

## Source outline

- # OpenVLA Trainval Guide
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
- ## 单机八卡训练
- ### 设置训练路径与参数
- ### 执行sft
- ### sft参考配置

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `openvla_test` |
| `MODEL_PATH` | `</path/to/openvla_dir> #本地路径` |
| `CUDA_VISIBLE_DEVICES` | `0,1,2,3,4,5,6,7` |
| `XMLIR_BMM_DISPATCH_VALUE` | `2` |
| `BKCL_PCIE_TOPO` | `1` |
| `CUDART_DUMMY_REGISTER` | `1` |
| `XMLIR_ENABLE_FAST_FC` | `1` |
| `XDNN_USE_FAST_GELU` | `1` |
| `XDNN_USE_FAST_SWISH` | `1` |
| `XDNN_FAST_DIV_SCALAR` | `true` |
| `XPUAPI_SDNN_BF16_ROUND_MODE` | `3` |
| `XBLAS_FC_AUTOTUNE_FILE` | `"openvla_tune.txt"` |
| `BKCL_ENABLE_TREE` | `1` |
| `BKCL_RDMA_VERBS` | `1` |
| `BKCL_RING_BUFFER_SIZE` | `8388608` |
| `BKCL_MULTI_TREE_THRESHOLD` | `-1` |
| `BKCL_RDMA_PROXY_DISABLE` | `1 # 屏蔽旧架构` |
| `BKCL_USE_AR` | `1` |
| `BKCL_RING_OPT` | `1` |

## Representative commands

- `cd openvla_data`
- `wget -r -nH --cut-dirs=4 --reject="index.html*" https://rail.eecs.berkeley.edu/datasets/bridge_release/data/tfds/bridge_dataset/`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/openvla_data.tar.gz`
- `cd openvla_data & mv bridge_dataset bridge_orig`
- `git lfs install`
- `git clone https://hf-mirror.com/openvla/openvla-7b-prismatic`
- `git clone https://hf-mirror.com/timm/vit_large_patch14_reg4_dinov2.lvd142m`
- `git clone https://hf-mirror.com/timm/ViT-SO400M-14-SigLIP`
- `git clone https://www.modelscope.cn/shakechen/Llama-2-7b-hf.git`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/openvla.tar.gz`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`

## URLs mentioned

- <https://github.com/openvla/openvla/tree/main>
- <https://github.com/kpertsch/rlds_dataset_builder>
- <https://rail.eecs.berkeley.edu/datasets/bridge_release/data/tfds/bridge_dataset/>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/openvla_data.tar.gz>
- <https://hf-mirror.com/openvla/openvla-7b-prismatic>
- <https://hf-mirror.com/timm/vit_large_patch14_reg4_dinov2.lvd142m>
- <https://hf-mirror.com/timm/ViT-SO400M-14-SigLIP>
- <https://www.modelscope.cn/shakechen/Llama-2-7b-hf.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/openvla.tar.gz>
- <https://github.com/kvablack/dlimp@d08da3852c149548aaa8551186d619d87375df08#egg=dlimp>
- <https://huggingface.co/docs/hub/en/security-tokens>

## Related derived pages

- [`OpenVLA`](../models/OpenVLA.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
