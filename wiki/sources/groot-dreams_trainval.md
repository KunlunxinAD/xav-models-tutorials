# Source Summary: GR00T-Dreams

## Source

- [`GR00T-Dreams_trainval.md`](../../tutorials/GR00T-Dreams_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | GR00T-Dreams |
| Domain | LLM/VLM/VLA |
| Workflow tags | Trainval, SFT |
| Frameworks / backends | diffusers, transformers, accelerate, tensorboard |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries in container startup; `CUDA_VISIBLE_DEVICES=0`; `CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7` |
| Dataset hints | `demo_data/robot_sim.PickNPlace/`; embodiment tag `gr1` |

## Source outline

- # GR00T-Dreams
- ## 准备环境
- ## 启动容器
- ## 下载及安装资源
- ### 下载GR00T-Dreams代码
- ### 安装依赖
- ### 权重下载
- ## 训练与推理
- ### 单机单卡训练
- ### 单机8卡训练

## Environment variables mentioned

| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-gr00t-dreams` |
| `MODEL_PATH` | `</path/to/model>` |
| `HF_ENDPOINT` | `https://hf-mirror.com` |
| `HF_HUB_OFFLINE` | `1` |
| `CUDA_VISIBLE_DEVICES` | `0`; `0,1,2,3,4,5,6,7` |
| `PYTHONPATH` | `.` |

## Representative commands

- `docker run -itd --privileged --net=host \`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/GR00T-Dreams/GR00T-Dreams.tar.gz`
- `pip install hydra-core==1.3.2`
- `pip install diffusers==0.30.2`
- `pip install transformers==4.45.2`
- `pip install accelerate==1.2.1`
- `hf auth login`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/GR00T-Dreams/hub_grootdreams.tar.gz`
- `PYTHONPATH=. python -u scripts/idm_training.py \`
- `PYTHONPATH=. python scripts/idm_training.py \`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/GR00T-Dreams/GR00T-Dreams.tar.gz>
- <https://hf-mirror.com>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/GR00T-Dreams/hub_grootdreams.tar.gz>

## Related derived pages

- [`GR00T-Dreams`](../models/GR00T-Dreams.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- The tutorial title says “训练与推理”, but the source sections currently show single-card and 8-card training commands; no standalone inference command is extracted from this source.
- Precision, benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
