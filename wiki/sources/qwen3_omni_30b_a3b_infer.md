# Source Summary: Qwen3-Omni-30B-A3B Inference Guide

## Source

- [`qwen3_omni_30b_a3b_infer.md`](../../tutorials/qwen3_omni_30b_a3b_infer.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen3-Omni-30B-A3B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Inference, Benchmark |
| Frameworks / backends | vLLM, xvLLM, vLLM-Omni, xvLLM-Omni, evalscope |
| Precision mentions | Not explicitly stated in the tutorial command sections |
| Device hints | `XPU_NUM=8`; dynamic `/dev/xpu*` device mapping; `CUDA_VISIBLE_DEVICES=6,7` for CUDA Graph examples |
| Dataset hints | `openqa` for evalscope benchmark |

## Source outline

- # Qwen3-Omni-30B-A3B Inference Guide
- ## 环境准备
- ### 启动容器
- ### 安装xvllm
- ### 安装vllm-omni
- ### 安装xvllm-omni
- ## 权重准备
- ## 模型推理
- ### offline
- ### online
- ### Benchmark (evalscope perf)

## Environment variables mentioned

| Variable | Source value |
| --- | --- |
| `CONTAINER_NAME` | `<CONTAINER_NAMe>` |
| `DOCKER_IMAGE` | `<XAV_IMAGE>` |
| `Workspace` | `<YOUR_PATH>` |
| `XPU_NUM` | `8` |
| `DOCKER_DEVICE_CONFIG` | Dynamically built from `XPU_NUM` |
| `VLLM_OMNI_TARGET_DEVICE` | `cuda` |
| `MODEL_DIR` | `{MODEL_DIR}` / model root for `Qwen3-Omni-30B-A3B-Instruct` |
| `CUDA_VISIBLE_DEVICES` | `6,7` in CUDA Graph examples |
| `XMLIR_FORCE_USE_XPU_GRAPH` | `1` |
| `TOKENIZER_DIR` | `/tmp/qwen3_omni_tokenizer` |

## Representative commands

- `docker run -it ${DOCKER_DEVICE_CONFIG} \`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_020/xvllm.tar.gz`
- `bash scripts/build.sh build`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_020/vllm-omni.tar.gz`
- `VLLM_OMNI_TARGET_DEVICE=cuda pip install --no-build-isolation --no-deps -e .`
- `pip install --no-deps "openai-whisper>=20250625"`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_020/xvllm-omni.tar.gz`
- `python examples/qwen3_omni/offline_inference.py \`
- `bash examples/qwen3_omni/start_server.sh /workspace/qwen3/Qwen3-Omni-30B-A3B-Instruct`
- `bash examples/qwen3_omni/start_server_cudagraph.sh /workspace/qwen3/Qwen3-Omni-30B-A3B-Instruct`
- `curl http://localhost:8091/v1/chat/completions \`
- `python examples/qwen3_omni/openai_sdk_request.py`
- `evalscope perf \`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_020/xvllm.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_020/vllm-omni.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_020/xvllm-omni.tar.gz>
- <https://mirrors.aliyun.com/pypi/simple/>
- <http://localhost:8091/v1/chat/completions>

## Related derived pages

- [`Qwen3-Omni-30B-A3B`](../models/Qwen3-Omni-30B-A3B.md)
- [`LLM-inference`](../concepts/LLM-inference.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- README lists FP16 and `1 x 8` for this model, while the tutorial examples include 2-device CUDA Graph runs; treat exact device usage as command-specific until benchmark documentation is expanded.
- Benchmark numbers, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial or captured benchmark output.
- Image tags and private environment details are intentionally not inferred.
