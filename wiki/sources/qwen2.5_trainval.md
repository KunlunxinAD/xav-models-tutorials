# Source Summary: Qwen2.5 Trainval Guide

## Source

- [`qwen2.5_trainval.md`](../../tutorials/qwen2.5_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen2.5 |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT, LoRA |
| Frameworks / backends | Deepspeed, flash_attn, LlamaFactory |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen2.5 Trainval Guide
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
- ## 单机多卡训练
- ### 设置训练路径与参数
- ### 执行Lora
- ### 执行sft
- ## 多机训练
- ### 设置训练路径与参数
- ### 执行Lora
- ### 执行sft

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `Qwen2.5_test` |
| `MODEL_PATH` | `</path/to/qwen2.5> #本地路径` |

## Representative commands

- `cd /home`
- `git lfs install`
- `git clone https://www.modelscope.cn/Qwen/Qwen2.5-7B-Instruct.git`
- `git clone https://www.modelscope.cn/Qwen/Qwen2.5-72B-Instruct.git`
- `git clone https://github.com/hiyouga/LLaMA-Factory.git`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5/qwen2.5.tar.gz`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda create -n Qwen2.5_env --clone python310_torch25_cuda`
- `conda init bash`
- `conda activate Qwen2.5_env`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5/xpytorch-cp310-torch251-ubuntu2004-x64.run`

## URLs mentioned

- <https://github.com/hiyouga/LLaMA-Factory?tab=readme-ov-file#provided-datasets>
- <https://llamafactory.readthedocs.io/en/latest/getting_started/data_preparation.html>
- <https://www.modelscope.cn/Qwen/Qwen2.5-7B-Instruct.git>
- <https://www.modelscope.cn/Qwen/Qwen2.5-72B-Instruct.git>
- <https://github.com/hiyouga/LLaMA-Factory.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5/qwen2.5.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5/xpytorch-cp310-torch251-ubuntu2004-x64.run>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/release/3.2.0/output/20250418/xpytorch-cp310-torch251-ubuntu2004-x64.run>

## Related derived pages

- [`Qwen2.5`](../models/Qwen2.5.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
