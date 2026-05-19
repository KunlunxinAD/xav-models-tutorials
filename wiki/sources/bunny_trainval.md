# Source Summary: Bunny Trainval Guide

## Source

- [`Bunny_trainval.md`](../../tutorials/Bunny_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Bunny |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT, LoRA |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | FP16 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Bunny Trainval Guide
- ## 环境准备
- ## 数据集准备
- ## 启动容器环境
- ## 资源下载及环境准备
- ## 模型训练测试
- ### Pretrain训练
- ### lora微调
- ## 模型评估
- ### 准备eval数据集
- ### 更新代码
- ### 执行评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `bunny-test` |
| `DATA_PATH` | `</path/to/datasets/bunny>` |

## Representative commands

- `cd path_to_datasets/bunny/pretrain`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/pretrian/bunny_pretrain_laion_2m.json`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/pretrian/images.tar.gz`
- `cd path_to_datasets/bunny/finetune`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_695k.json`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_allava_1.3m.json`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_llava_1.4m.json`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_llava_allava_2m.json`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/images.tar.gz`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/images_tar_split/images.tar.gz.part-aa`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/images_tar_split/images.tar.gz.part-ao`
- `docker run -itd --privileged --net=host \`

## URLs mentioned

- <https://huggingface.co/datasets/BoyaWu10/Bunny-v1_1-data>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/pretrian/bunny_pretrain_laion_2m.json>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/pretrian/images.tar.gz>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_695k.json>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_allava_1.3m.json>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_llava_1.4m.json>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/bunny_llava_allava_2m.json>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/images.tar.gz>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/images_tar_split/images.tar.gz.part-aa>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/bunny_datasets/finetune/images_tar_split/images.tar.gz.part-ao>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/xav/v0.2/Bunny.tar.gz>
- <http://klx-sdk-release-public.su.bcebos.com/xav/model_weights/siglip-so400m-patch14-384.tar.gz>

## Related derived pages

- [`Bunny`](../models/Bunny.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
