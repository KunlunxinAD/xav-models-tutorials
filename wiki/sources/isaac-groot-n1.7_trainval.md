# Source Summary: Isaac-GR00T-N1.7 Trainval Guide

## Source

- [`Isaac-GR00T-N1.7_trainval.md`](../../tutorials/Isaac-GR00T-N1.7_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Isaac-GR00T-N1.7 |
| Domain | LLM/VLM/VLA |
| Workflow tags | Trainval, SFT, Inference |
| Frameworks / backends | Lerobot, Deepspeed, flash_attn, transformers, peft, wandb |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries in container startup; `CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7`; `NUM_GPUS=8` |
| Dataset hints | LIBERO dataset `IPEC-COMMUNITY/libero_10_no_noops_1.0.0_lerobot`; `demo_data/droid_sample` for inference example |

## Source outline

- # Isaac-GR00T N1.7 Trainval Guide
- ## 环境准备
- ### 准备开发环境镜像
- ## 数据集及代码准备
- ### 代码准备
- ### 数据集准备
- ### 下载预训练权重
- ## 启动容器
- ### 容器内环境配置
- ### 安装依赖
- # 单机多卡微调
- ### 设置finetune路径与参数
- ### 脚本内容参考
- ### 执行Inference
- ### 脚本内容参考

## Environment variables mentioned

| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `Isaac-GR00T_test` |
| `MODEL_PATH` | `</path/to/Isaac-GR00T> #本地路径` |
| `CUDA_VISIBLE_DEVICES` | `0,1,2,3,4,5,6,7` |
| `XMLIR_BMM_DISPATCH_VALUE` | `2` |
| `BKCL_PCIE_TOPO` | `1` |
| `CUDART_DUMMY_REGISTER` | `1` |
| `XMLIR_ENABLE_FAST_FC` | `1` |
| `XDNN_USE_FAST_GELU` | `1` |
| `WANDB_MODE` | `offline` |
| `NUM_GPUS` | `8` |
| `MAX_STEPS` | `2000` |
| `GLOBAL_BATCH_SIZE` | `640` |
| `SAVE_STEPS` | `1000` |

## Representative commands

- `git clone --recurse-submodules https://github.com/NVIDIA/Isaac-GR00T`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/Isaac-GR00T.tar.gz`
- `hf download --repo-type dataset IPEC-COMMUNITY/libero_10_no_noops_1.0.0_lerobot \`
- `hf download nvidia/GR00T-N1.7-3B`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/GR00T-N1.7-3B.tar.gz`
- `docker run -itd --privileged --net=host \`
- `conda activate python310_torch29_cuda`
- `pip install -e . --no-deps`
- `pip install deepspeed-0.17.2+7820cf87-py3-none-any.whl`
- `pip install flash_attn-2.8.0-cp310-cp310-linux_x86_64.whl`
- `bash examples/finetune.sh \`
- `python scripts/deployment/standalone_inference_script.py \`

## URLs mentioned

- <https://github.com/NVIDIA/Isaac-GR00T>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/Isaac-GR00T.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/libero_10_no_noops_1.0.0_lerobot.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/GR00T-N1.7-3B.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/model_weights/GR00T/Cosmos-Reason2-2B.tar.gz>
- <https://download.pytorch.org/whl/cpu>
- <https://klx-sdk-release-public.su.bcebos.com/XDeepSpeed/release/1.1.0.0/XDeepSpeed_py310_torch251.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/XFlashAttention/release/1.0.4.0/xFlashAttention_py310_torch290.tar.gz>

## Related derived pages

- [`Isaac-GR00T-N1.7`](../models/Isaac-GR00T-N1.7.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- README lists FP16/BF16 for this model, but the tutorial does not explicitly state precision in the extracted command sections; keep precision claims source-scoped.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
