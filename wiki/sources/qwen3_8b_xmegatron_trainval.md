# Source Summary: Qwen3-8B XMegatron Trainval Guide

## Source

- [`qwen3_8b_xmegatron_trainval.md`](../../tutorials/qwen3_8b_xmegatron_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen3-8B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | Megatron, wandb, XMegatron |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen3-8B XMegatron Trainval Guide
- ## 环境准备
- ### 准备开发环境镜像
- ### PCIe环境配置（OAM跳过此步骤）
- #### 确定PCIe环境
- #### 配置PCIe的8卡互联模式
- ### 下载代码及预训练权重
- ## 下载数据与模型权重，并启动容器
- ### 容器内环境配置
- # 单机多卡训练
- ### 设置训练路径与参数
- ### 执行pretrain

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen3-8b-xmegatron/lm-training-qwen3.tar.gz`
- `cd lm-training/xmegatron/qwen3-8b`
- `bash env_prepare.sh`
- `conda activate python310_torch25_cuda`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/XMegatronExtension/daily/20251115/xmegatron_ext-main-20251115.run`
- `bash xmegatron_ext-main-20251115.run`
- `wget https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.4.0.0/xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `bash xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `pip install transformers==4.51.0 megatron-energon==6.0 wandb filetype bitstring ebmlite sortedcontainers av soundfile qwen_vl_utils -i http://mirrors.baidubce.c`
- `cd /workspace`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen3-8b-xmegatron/KLX-LLM.tar.gz`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen3-8b-xmegatron/Megatron-LM.tar.gz`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen3-8b-xmegatron/lm-training-qwen3.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/XMegatronExtension/daily/20251115/xmegatron_ext-main-20251115.run>
- <https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.4.0.0/xpytorch-cp310-torch251-ubuntu2004-x64.run>
- <http://mirrors.baidubce.com/pypi/simple/>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen3-8b-xmegatron/KLX-LLM.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen3-8b-xmegatron/Megatron-LM.tar.gz>

## Related derived pages

- [`Qwen3-8B`](../models/Qwen3-8B.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)
- [`megatron-pretrain`](../recipes/megatron-pretrain.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
