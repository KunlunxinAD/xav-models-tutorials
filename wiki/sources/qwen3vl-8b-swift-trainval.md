# Source Summary: Qwen3-VL-8B MS-Swift Trainval Guide

## Source

- [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen3-VL-8B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT, LoRA |
| Frameworks / backends | Deepspeed, flash_attn, MS-Swift, wandb |
| Precision mentions | BF16 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); NPROC_PER_NODE=8; CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |
| Dataset hints | coco |

## Source outline

- # Qwen3-VL-8B MS-Swift Trainval Guide
- ## 环境准备
- ### 准备开发环境镜像
- ## 数据集及代码准备
- ### 数据集准备
- ### 下载代码及预训练权重
- ## 启动容器
- ### 容器内环境配置
- # 单机多卡训练
- ### 设置训练路径与参数
- ### 脚本内容参考
- ### 执行Lora

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `Qwen3VL_test` |
| `MODEL_PATH` | `</path/to/qwen3_vl> #本地路径` |
| `CUDA_VISIBLE_DEVICES` | `0,1,2,3,4,5,6,7` |
| `NPROC_PER_NODE` | `8` |
| `XMLIR_BMM_DISPATCH_VALUE` | `2` |
| `XMLIR_ENABLE_LINEAR_FC_FUSION` | `1` |
| `BKCL_PCIE_TOPO` | `1` |
| `XMLIR_ENABLE_FAST_FC` | `1` |
| `XPYTORCH_RUN_ENHANCE` | `1` |
| `XDNN_USE_FAST_SWISH` | `1` |
| `XDNN_FAST_DIV_SCALAR` | `true` |
| `XPUAPI_SDNN_BF16_ROUND_MODE` | `3` |
| `MODELSCOPE_CACHE` | `/home/qwen2/temp` |
| `MAX_PIXELS` | `1003520` |
| `PYTORCH_CUDA_ALLOC_CONF` | `expandable_segments:True` |
| `WANDB_API_KEY` | `"xxxxx"` |
| `WANDB_PROJECT` | `"qwen3-vl"` |

## Representative commands

- `git lfs install`
- `git clone https://huggingface.co/datasets/detection-datasets/coco`
- `cd /home`
- `git clone https://huggingface.co/Qwen/Qwen3-VL-8B-Instruct`
- `git clone https://github.com/modelscope/ms-swift.git`
- `git checkout v3.12.6`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `cd ms-swift`
- `pip install -e .`
- `pip install qwen_vl_utils`

## URLs mentioned

- <https://swift.readthedocs.io/zh-cn/latest/Customization/Custom-dataset.html>
- <https://huggingface.co/datasets/detection-datasets/coco>
- <https://huggingface.co/Qwen/Qwen3-VL-8B-Instruct>
- <https://github.com/modelscope/ms-swift.git>

## Related derived pages

- [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
