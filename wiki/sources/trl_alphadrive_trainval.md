# Source Summary: AlphaDrive Trainval Guide

## Source

- [`TRL_AlphaDrive_trainval.md`](../../tutorials/TRL_AlphaDrive_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | AlphaDrive |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval, SFT, GRPO |
| Frameworks / backends | Deepspeed, flash_attn, vLLM, xvLLM |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # AlphaDrive Trainval Guide
- ## 环境准备
- ### PCIe环境配置（OAM跳过此步骤）
- #### 确定PCIe环境
- #### 配置PCIe的8卡互联模式
- ## 数据集及代码准备
- ### 数据集准备
- ### 下载代码及预训练权重
- ## 启动容器
- ### 容器内环境配置
- ## 单机多卡训练
- ### 设置环境变量
- ### 执行sft训练
- ### 执行grpo训练
- ### 执行sft-warmup及grpo微调训练
- ## 单机多卡评估
- ### 执行评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `NAME_CONTAINER` | `alpha_drive` |
| `MODEL_PATH` | `</path/to/Qwen2.5-VL-3B-Instruct> #本地路径` |
| `XMLIR_ENABLE_MOCK_TORCH_COMPILE` | `false` |
| `XMLIR_FORCE_USE_XPU_GRAPH` | `1` |
| `VLLM_USE_V1` | `0` |
| `XMLIR_ENABLE_NEW_PG` | `0` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/alpha_drive/TRL-AlphaDrive.tar.gz`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/Impromptu_VLA/alpha_drive_ImpromptuData.tar.gz`
- `cd /home`
- `git lfs install`
- `git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${NAME_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `pip install trl==0.23.0`
- `pip install transformers==4.51.0`
- `cd /root/miniconda/envs/python310_torch25_cuda/lib/python3.10/site-packages/transformers`
- `git init`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/alpha_drive/TRL-AlphaDrive.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/Impromptu_VLA/alpha_drive_ImpromptuData.tar.gz>
- <https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/alpha_drive/alpha_drive_transformers_4_51_0.patch>
- <https://klx-sdk-release-public.su.bcebos.com/xvllm/KL3/0.8.2/latest/output.tar.gz>
- <https://www.nltk.org/nltk_data/下载wordnet.zip保存到/root/nltk_data/corpora>

## Related derived pages

- [`AlphaDrive`](../models/AlphaDrive.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
