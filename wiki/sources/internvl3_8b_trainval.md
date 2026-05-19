# Source Summary: InternVL3-8B Trainval Guide

## Source

- [`Internvl3_8b_trainval.md`](../../tutorials/Internvl3_8b_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | InternVL3-8B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT, LoRA |
| Frameworks / backends | flash_attn, LlamaFactory |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | coco |

## Source outline

- # InternVL3-8B Trainval Guide
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
- ### 执行Lora
- ### 执行sft

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `InternVL3_test` |
| `MODEL_PATH` | `</path/to/intern_vl3> #本地路径` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/dataset_info.json`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/mllm_rec_json.json`
- `cd /home`
- `git lfs install`
- `git clone https://hf-mirror.com/OpenGVLab/InternVL3-8B-hf`
- `git clone https://github.com/hiyouga/LLaMA-Factory.git`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/InternVL3-8b.tar.gz`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `wget https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `bash xpytorch-cp310-torch251-ubuntu2004-x64.run`

## URLs mentioned

- <https://github.com/hiyouga/LLaMA-Factory?tab=readme-ov-file#provided-datasets>
- <https://llamafactory.readthedocs.io/en/latest/getting_started/data_preparation.html>
- <https://cocodataset.org/#download>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/dataset_info.json>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/mllm_rec_json.json>
- <https://hf-mirror.com/OpenGVLab/InternVL3-8B-hf>
- <https://github.com/hiyouga/LLaMA-Factory.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/InternVL3-8b.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/xpytorch/release/3.3.0.0/xpytorch-cp310-torch251-ubuntu2004-x64.run>

## Related derived pages

- [`InternVL3-8B`](../models/InternVL3-8B.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
