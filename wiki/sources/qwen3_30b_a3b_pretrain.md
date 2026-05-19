# Source Summary: Qwen3-30B-A3B Pretrain Guide

## Source

- [`qwen3_30b_a3b_pretrain.md`](../../tutorials/qwen3_30b_a3b_pretrain.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen3-30B-A3B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT |
| Frameworks / backends | Megatron, wandb, XMegatron |
| Precision mentions | BF16, FP16, FP32 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen3-30B-A3B Pretrain Guide
- ## 环境准备
- ### 启动容器
- ### 安装xmegatron_ext
- ### 安装其他依赖
- ## 模型准备
- ### 数据集下载
- ### 权重下载
- ### megatron代码准备
- ## 模型训练
- ### pretrain

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `qwen3-30b-a3b-megatron-pretrain-test` |
| `PYTHONPATH` | `${PYTHONPATH}:${WORKSPACE_DIR}/Megatron-LM/${MCORE_VERSION:-core_r0_12_0}:${ROOT_DIR}/Megatron-LM/${MCORE_VERSION:-core_` |
| `XTE_DISABLE_EXTRA_STATE` | `${XTE_DISABLE_EXTRA_STATE:-1}` |
| `XME_LOAD_CKPT_UNSTRICT` | `1` |
| `XTE_GROUPED_GEMM_LARGE_WEIGHT` | `0` |
| `NVTE_FLASH_ATTN` | `1 NVTE_FUSED_ATTN=0` |
| `NVTE_APPLY_QK_LAYER_SCALING` | `1` |
| `XMLIR_ENABLE_FAST_FC` | `1` |
| `XFA_GEMM_TYPE` | `float16` |
| `XFA_BWD_USE_DS_SCALE` | `1` |
| `CUDA_DEVICE_ORDER` | `OAM_ID` |
| `XMLIR_DIST_SINGLETON_STREAM` | `true` |
| `DIST_MULTI_STREAM` | `${DIST_MULTI_STREAM:-true}` |
| `CUDA_DEVICE_MAX_CONNECTIONS` | `${CUDA_DEVICE_MAX_CONNECTIONS:-8}` |
| `CUDA_VISIBLE_DEVICES` | `${CUDA_VISIBLE_DEVICES:-"0,1,2,3,4,5,6,7"}` |
| `XMLIR_FA_GEMM_TYPE` | `float16` |
| `XMLIR_PARALLEL_SAVE_MEMORY` | `${XMLIR_PARALLEL_SAVE_MEMORY:-false}` |
| `XMLIR_DIST_ASYNC_ISEND_IRECV` | `true` |
| `XMLIR_BATCH_PARALLEL` | `${XMLIR_BATCH_PARALLEL:-true}` |

## Representative commands

- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/XMegatronExtension/release/release_20260130/xme-20260131.dev4%2Bged343f780.tar.gz`
- `pip install xmegatron_ext-20260131.dev4+ged343f780-py3-none-any.whl`
- `pip install transformers==4.51.0`
- `pip install megatron-energon==6.0`
- `pip install wandb`
- `pip install filetype`
- `pip install bitstring`
- `pip install ebmlite`
- `pip install sortedcontainers`
- `pip install av`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/XMegatronExtension/release/release_20260130/xme-20260131.dev4%2Bged343f780.tar.gz>
- <http://mirrors.baidubce.com/pypi/simple/>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xpytorch/modelzoo_data/data/aiak_megatron_core/qwen3_data_content_document.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/xmegatron/3.5.0.0_117/KLX-LLM.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/xmegatron/3.5.0.0_117/Megatron-LM.tar.gz>

## Related derived pages

- [`Qwen3-30B-A3B`](../models/Qwen3-30B-A3B.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)
- [`megatron-pretrain`](../recipes/megatron-pretrain.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
