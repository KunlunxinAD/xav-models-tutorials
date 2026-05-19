# Source Summary: xvLLM General Inference Guide

## Source

- [`xvllm_general_infer.md`](../../tutorials/xvllm_general_infer.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | xvLLM |
| Domain | LLM/VLM/VLA |
| Workflow tags | Inference |
| Frameworks / backends | vLLM, xav-vLLM, xvLLM |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | ModelScope, HuggingFace |

## Source outline

- # xvLLM General Inference Guide
- ## 概述
- ## 准备环境
- ## 启动容器
- ## 容器内目录说明
- ## 下载模型权重
- ## 启动推理服务
- ## 验证推理服务
- ## 多机 / PD 分离 / 其他场景

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `CONTAINER_NAME` | `xvllm_test` |
| `PATH_TO_MOUNT` | `</path/to/mount> #本地路径` |
| `PATH_AFTER_MOUNT` | `/home #挂载后在容器内的路径` |
| `DATA_PATH` | `</path/to/dataset>` |

## Representative commands

- `docker run -dti \`
- `docker exec -it <CONTAINER_NAME> bash`
- `git lfs install`
- `git clone https://www.modelscope.cn/<org>/<model_name>.git /home/models/<model_name>`
- `cd /workspace/singlenode/Qwen3.5-0.8B`
- `bash Qwen3.5-0.8B_TP2_evalserver.sh`
- `bash /workspace/curl.sh`
- `bash /workspace/evalscope.sh`
- `bash /workspace/benchmark.sh`
- `curl http://localhost:<port>/v1/chat/completions \`

## URLs mentioned

- <https://www.modelscope.cn/<org>/<model_name>.git>
- <http://localhost:<port>/v1/chat/completions>

## Related derived pages

- [`xvLLM`](../models/xvLLM.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
