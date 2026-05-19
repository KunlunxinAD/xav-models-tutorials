# Source Summary: Qwen2.5-VL-R1 Trainval Guide

## Source

- [`qwen2.5vl_r1_trainval.md`](../../tutorials/qwen2.5vl_r1_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen2.5-VL |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | Deepspeed, flash_attn |
| Precision mentions | BF16 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |
| Dataset hints | coco, ModelScope |

## Source outline

- # Qwen2.5-VL-R1 Trainval Guide
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
- ### 执行训练
- ### 训练脚本内容示例
- # 单机多卡评估
- ### 设置评估路径与参数
- ### 执行评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `Qwen2.5_VL_r1_test` |
| `MODEL_PATH` | `</path/to/qwen2.5_vl_r1> #本地路径` |
| `REPO_HOME` | `"${PROJECT_ROOT}"` |
| `EXP_NAME` | `"Qwen2.5-VL-3B-Instruct-rec" # TODO: change this to your own experiment name` |
| `LOG_PATH` | `"${REPO_HOME}/runs/${EXP_NAME}/log/debug_log.$(date +%Y-%m-%d-%H-%M-%S).txt"` |
| `MAX_STEPS` | `200 # TODO: change this to your own max steps` |
| `CUDA_VISIBLE_DEVICES` | `0,1,2,3,4,5,6,7` |
| `XMLIR_BMM_DISPATCH_VALUE` | `0` |
| `XMLIR_ENABLE_LINEAR_FC_FUSION` | `1` |
| `USE_FAST_BF16_FC` | `true` |
| `XMLIR_ENABLE_XBLAS_ADDMM` | `0` |
| `BKCL_PCIE_TOPO` | `1` |
| `FORCE_DISABLE_INPLACE_BF16_CAST` | `0` |
| `DISABLE_CAST_CACHE` | `true` |
| `PYTORCH_CUDA_ALLOC_CONF` | `max_split_size_mb:512,expandable_segments:True` |

## Representative commands

- `git lfs install`
- `git clone https://www.modelscope.cn/datasets/AI-ModelScope/VLM-R1.git`
- `cd /home`
- `git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git`
- `git clone https://github.com/om-ai-lab/VLM-R1.git`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/release/3.2.0/output/20250418/xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `bash xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `cd VLM-R1/src/open-r1-multimodal`
- `pip install -e ".[dev]"`

## URLs mentioned

- <https://www.modelscope.cn/datasets/AI-ModelScope/VLM-R1.git>
- <https://github.com/om-ai-lab/VLM-R1?tab=readme-ov-file#for-your-own-data>
- <https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git>
- <https://github.com/om-ai-lab/VLM-R1.git>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/release/3.2.0/output/20250418/xpytorch-cp310-torch251-ubuntu2004-x64.run>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/qwen2.5_vl_transformer_patch>

## Related derived pages

- [`Qwen2.5-VL`](../models/Qwen2.5-VL.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
