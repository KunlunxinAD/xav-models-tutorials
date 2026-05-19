# Source Summary: AlphaDrive Trainval Guide

## Source

- [`AlphaDrive_trainval.md`](../../tutorials/AlphaDrive_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | AlphaDrive |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval, GRPO |
| Frameworks / backends | Deepspeed, flash_attn, vLLM, wandb, xvLLM |
| Precision mentions | BF16 |
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
- ### 执行训练
- ### 使用vllm加速训练
- ## 单机多卡评估
- ### 执行评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `NAME_CONTAINER` | `alpha_drive` |
| `MODEL_PATH` | `</path/to/Qwen2-VL-2B-Instruct> #本地路径` |
| `DIST_MULTI_STREAM` | `false #关闭多流` |
| `XMLIR_ENABLE_MOCK_TORCH_COMPILE` | `false` |
| `XMLIR_FORCE_USE_XPU_GRAPH` | `1` |
| `VLLM_USE_V1` | `0` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/alpha_drive/R1-V-AlphaDrive.tar.gz`
- `git lfs install`
- `git clone https://huggingface.co/datasets/aaaaaap/unstructed`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/Impromptu_VLA/alpha_drive_ImpromptuData.tar.gz`
- `cd /home`
- `git clone https://www.modelscope.cn/Qwen/Qwen2-VL-2B-Instruct.git`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${NAME_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `wget https://klx-sdk-release-public.su.bcebos.com/xvllm/KL3/0.8.2/latest/output.tar.gz`
- `cd xvllm_0.8.2`
- `pip config set global.disable-pip-version-check true`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/alpha_drive/R1-V-AlphaDrive.tar.gz>
- <https://huggingface.co/datasets/aaaaaap/unstructed>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/Impromptu_VLA/alpha_drive_ImpromptuData.tar.gz>
- <https://www.modelscope.cn/Qwen/Qwen2-VL-2B-Instruct.git>
- <https://klx-sdk-release-public.su.bcebos.com/xvllm/KL3/0.8.2/latest/output.tar.gz>

## Related derived pages

- [`AlphaDrive`](../models/AlphaDrive.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
