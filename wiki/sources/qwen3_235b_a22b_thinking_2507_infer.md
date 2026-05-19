# Source Summary: Qwen3-235B-A22B-Thinking-2507 Inference Guide

## Source

- [`qwen3_235b_a22b_thinking_2507_infer.md`](../../tutorials/qwen3_235b_a22b_thinking_2507_infer.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen3-235B-A22B-Thinking-2507 |
| Domain | LLM/VLM/VLA |
| Workflow tags | Inference, Pretrain, Trainval |
| Frameworks / backends | SGLang |
| Precision mentions | FP16 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen3-235B-A22B-Thinking-2507 Inference Guide
- ## 准备环境
- ## 启动容器
- ## 准备数据集及模型
- ### 配置容器内环境
- ### 准备数据集
- ### 下载预训练权重
- ## 运行推理

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XBLAS_FC_AUTOTUNE_FILE` | `./fc_fusion_autotune_20260107_214038` |
| `XBLAS_MOE_FC_AUTOTUNE_FILE` | `./moe_fc_autotune_20260107_172556` |
| `XMLIR_D_XPU_L3_SIZE` | `0` |
| `CUDA_DEVICE_ORDER` | `"OAM_ID"` |
| `BKCL_INFERENCE` | `1 #开启推理优化` |
| `BKCL_TREE_THRESHOLD` | `1048576` |
| `PYTORCH_CUDA_ALLOC_CONF` | `expandable_segments:True` |
| `XSGL_INTERTYPE_BFP16` | `1` |
| `MIN_BATCH` | `4096` |
| `LD_LIBRARY_PATH` | `${CONDA_PREFIX}/lib/python3.10/site-packages/xtorch_ops:${CONDA_PREFIX}/lib/python3.10/site-packages/nvidia/cuda_nvrtc/l` |
| `SGLANG_ALLOW_OVERWRITE_LONGER_CONTEXT_LEN` | `1` |
| `XPU_VISIBLE_DEVICES` | `0,1,2,3,4,5,6,7` |
| `CUDA_VISIBLE_DEVICES` | `0,1,2,3,4,5,6,7` |
| `XSGL_ENABLE_TGEMM_FP16` | `1` |
| `XSGL_FUSE_SPLIT_NORM_ROPE_NEOX` | `1` |
| `SGLANG_IS_FLASHINFER_AVAILABLE` | `false` |
| `XSGL_USE_DEEP_GEMM_BMM` | `1` |
| `XSGL_FAST_SWIGLU` | `1` |
| `USE_MOE_FC_V3` | `1` |
| `USE_KLX_API_ALLOC_EXTEND` | `1` |

## Representative commands

- `docker run -dit ${DOCKER_DEVICE_CONFIG}                 \`
- `docker exec -it ${CONTAINER_NAME} /bin/bash`
- `pip install huggingface_hub`
- `curl http://localhost:8026/v1/chat/completions -H "Content-Type: application/json" -d '{`

## URLs mentioned

- <http://localhost:8026/v1/chat/completions>
- <http://localhost:8026>

## Related derived pages

- [`Qwen3-235B-A22B-Thinking-2507`](../models/Qwen3-235B-A22B-Thinking-2507.md)
- [`llm-inference`](../recipes/llm-inference.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
