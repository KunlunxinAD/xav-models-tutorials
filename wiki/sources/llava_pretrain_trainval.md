# Source Summary: LLaVA

## Source

- [`LLaVA_pretrain_trainval.md`](../../tutorials/LLaVA_pretrain_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | LLaVA |
| Domain | LLM/VLM/VLA |
| Workflow tags | Inference, Pretrain, Trainval |
| Frameworks / backends | flash_attn |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # LLaVA
- ## 准备环境
- ## 启动容器
- ## 下载模型及安装资源
- ## 权重准备
- ## 数据集准备
- ### pretrain数据集
- ## 训练与评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-LLaVA` |
| `MODEL_PATH` | `</path/to/model>` |
| `DATASET_PATH` | `</path/to/dataset>` |

## Representative commands

- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `cd /home`
- `git clone https://github.com/haotian-liu/LLaVA.git`
- `pip install flash_attn==2.8.0.post2`
- `pip install bitsandbytes`
- `pip install transformers==4.37.2`
- `cd /datasets`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-Pretrain/blip_laion_cc_sbu_558k_meta.json`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-Pretrain/blip_laion_cc_sbu_558k.json`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-Pretrain/images.zip`

## URLs mentioned

- <https://github.com/haotian-liu/LLaVA.git>
- <https://github.com/haotian-liu/LLaVA/blob/main/README.md>
- <https://huggingface.co/openai/clip-vit-large-patch14-336>
- <https://huggingface.co/liuhaotian/llava-v1.6-vicuna-13b>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-Pretrain/blip_laion_cc_sbu_558k_meta.json>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-Pretrain/blip_laion_cc_sbu_558k.json>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/LLaVA-Pretrain/images.zip>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/LLaVA/pretrain.sh>
- <https://llava-vl.github.io/static/images/view.jpg>

## Related derived pages

- [`LLaVA`](../models/LLaVA.md)
- [`llm-inference`](../recipes/llm-inference.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
