# Source Summary: Pi_0

## Source

- [`Pi_0_trainval.md`](../../tutorials/Pi_0_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Pi0 |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT |
| Frameworks / backends | Lerobot, wandb |
| Precision mentions | BF16 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | HuggingFace |

## Source outline

- # Pi_0
- ## 环境准备
- ## 准备数据集及代码
- ### 准备数数据集
- #### 数据集介绍
- ### 下载代码及预训练权重
- ## 启动容器
- ## 单机多卡训练
- ### 设置环境变量
- ### 训练
- ### 训练示例
- ### 评估
- ### 评估结果示例
- ### 网络问题

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `NAME_CONTAINER` | `lerobot` |
| `MODEL_PATH` | `</path/to/models> #本地路径` |
| `XMLIR_ENABLE_FAST_FC` | `true` |
| `XMLIR_FC_BIAS_FUSION` | `true` |
| `XDNN_USE_FAST_GELU` | `true` |
| `XMLIR_BATCH_PARALLEL` | `false` |
| `XMLIR_PARALLEL_SAVE_MEMORY` | `true` |
| `DISABLE_CAST_CACHE` | `1` |
| `XMLIR_CUDNN_ENABLED` | `1` |
| `BKCL_TREE_THRESHOLD` | `0` |
| `BKCL_ENABLE_XDR` | `1` |
| `XPUCUDA_DISABLE_ERROR_PRINT` | `1` |
| `XMLIR_MATMUL_FAST_MODE` | `1` |
| `XMLIR_ENABLE_LINEAR_FC_FUSION` | `1` |
| `CUDA_DEVICE_MAX_CONNECTIONS` | `1` |

## Representative commands

- `pip install modelscope`
- `git clone https://github.com/huggingface/lerobot.git`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `cd lerobot`
- `pip install -e .`
- `git clone https://github.com/huggingface/transformers.git`
- `cd transformers`
- `git checkout fix/lerobot_openpi`
- `pip install peft==0.17.0`
- `cd checkpoints`
- `pip install -e ".[lerobot]"`

## URLs mentioned

- <https://github.com/huggingface/lerobot.git>
- <https://github.com/huggingface/transformers.git>

## Related derived pages

- [`Pi0`](../models/Pi0.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
