# Source Summary: Qwen3-Omni-30B-A3B Inference Guide

## Source

- [`qwen3_omni_30b_a3b_infer.md`](../../tutorials/qwen3_omni_30b_a3b_infer.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen3-Omni-30B-A3B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Inference, Benchmark |
| Frameworks / backends | vLLM, xvLLM |
| Precision mentions | Not explicitly stated |
| Device hints | CUDA_VISIBLE_DEVICES=0; CUDA_VISIBLE_DEVICES=0,1,2 |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen3-Omni-30B-A3B Inference Guide
- ## 环境准备
- ### 启动容器
- ### 安装xvllm
- ### 安装vllm-omni
- ### 安装xvllm-omni
- ### 安装其他依赖
- ## 模型准备
- ### 权重下载
- ## 模型推理
- ### offline
- ### online
- ### 性能测试

## Representative commands

- `docker run -it ${DOCKER_DEVICE_CONFIG}                         \`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_017/xvllm.tar.gz`
- `bash scripts/build.sh build`
- `git clone https://github.com/vllm-project/vllm-omni.git`
- `cd vllm-omni`
- `git checkout release/v0.17.0rc1`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_017/xvllm-omni.tar.gz`
- `pip install -e .`
- `pip install aenum`
- `pip install omegaconf`
- `pip install prettytable`
- `pip install librosa`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_017/xvllm.tar.gz>
- <https://github.com/vllm-project/vllm-omni.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen3_omni/omni_017/xvllm-omni.tar.gz>
- <http://localhost:8091/v1>
- <http://localhost:8091/v1/chat/completions>

## Related derived pages

- [`Qwen3-Omni-30B-A3B`](../models/Qwen3-Omni-30B-A3B.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
