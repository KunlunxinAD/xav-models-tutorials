# Source Summary: Qwen2-7B Trainval Guide

## Source

- [`qwen2_7b_trainval.md`](../../tutorials/qwen2_7b_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen2-7B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT, LoRA |
| Frameworks / backends | Deepspeed, flash_attn, LlamaFactory |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen2-7B Trainval Guide
- ## 环境准备
- ### 准备开发环境镜像
- ### PCIe环境配置（OAM跳过此步骤）
- #### 确定PCIe环境
- #### 配置PCIe的8卡互联模式
- ## 数据集及代码准备
- ### 数据集准备
- # 从modelscope下载预训练权重
- # 下载qwen2-7B 模型权重
- # 下载LLaMA-Factory代码以及配置
- # 下载qwen2-7b 代码以及配置
- # 创建conda env
- # 拉取xmlir 产出
- # 使用<image version>0.5的镜像，请拉取以下产出
- # 使用<image version>1.0.0的镜像，请拉取以下产出
- # 更新环境
- # 也可以直接编译安装源码pip install --no-deps -e .
- # xav v0.4及以下版本镜像需要升级setuptools
- # 修改xre依赖路径

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `Qwen2_7B_test` |
| `MODEL_PATH` | `</path/to/qwen2_7b> #本地路径` |

## URLs mentioned

- <https://github.com/hiyouga/LLaMA-Factory?tab=readme-ov-file#provided-datasets>
- <https://llamafactory.readthedocs.io/en/latest/getting_started/data_preparation.html>
- <https://www.modelscope.cn/maple77/Qwen2-7B-Instruct.git>
- <https://github.com/hiyouga/LLaMA-Factory.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2_7b.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5/xpytorch-cp310-torch251-ubuntu2004-x64.run>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/release/3.2.0/output/20250418/xpytorch-cp310-torch251-ubuntu2004-x64.run>

## Related derived pages

- [`Qwen2-7B`](../models/Qwen2-7B.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
