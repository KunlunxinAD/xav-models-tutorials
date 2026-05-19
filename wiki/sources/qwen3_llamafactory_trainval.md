# Source Summary: Qwen3 Trainval Guide (LlamaFactory)

## Source

- [`qwen3_llamafactory_trainval.md`](../../tutorials/qwen3_llamafactory_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen3 (LlamaFactory) |
| Domain | LLM/VLM/VLA |
| Workflow tags | Trainval, SFT |
| Frameworks / backends | Deepspeed, flash_attn, LlamaFactory, wandb |
| Precision mentions | BF16 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen3 Trainval Guide (LlamaFactory)
- ## 准备环境
- ## 启动容器
- ## 配置容器内环境
- ## 下载框架及安装
- ### LlamaFactory
- ### XDeepSpeed
- ## 下载模型权重及配置
- ### Qwen3-4B
- ### Qwen3-30B-A3B
- ## 模型训练
- ### Qwen3-4B
- ### Qwen3-30B-A3B

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `qwen-llamafactory-test` |

## Representative commands

- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `pip install omegaconf==2.3.0`
- `pip install numpy==1.26.4`
- `pip install peft==0.14.0`
- `pip install accelerate==1.8.1`
- `pip install --no-build-isolation flash-attn==2.4.0.post1`
- `pip install huggingface_hub`
- `pip install transformers==4.51.0`
- `cd /workspace`
- `git clone https://github.com/hiyouga/LLaMA-Factory.git`
- `cd LLaMA-Factory`

## URLs mentioned

- <https://github.com/hiyouga/LLaMA-Factory.git>
- <https://klx-sdk-release-public.su.bcebos.com/XDeepSpeed/release/1.0.0.0/XDeepSpeed.tar.gz>

## Related derived pages

- [`Qwen3 (LlamaFactory)`](../models/Qwen3-LlamaFactory.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
