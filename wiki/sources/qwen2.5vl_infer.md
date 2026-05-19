# Source Summary: Qwen2.5-VL Inference Guide

## Source

- [`qwen2.5vl_infer.md`](../../tutorials/qwen2.5vl_infer.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen2.5-VL |
| Domain | LLM/VLM/VLA |
| Workflow tags | Inference |
| Frameworks / backends | TensorRT |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen2.5-VL Inference Guide
- ## 镜像获取
- ### 准备开发环境镜像
- ## 环境准备
- ### 启动容器
- ### 容器内环境配置
- ### 依赖安装
- ### 模型下载
- ## Build TensorRT engine
- ### 模型转换&构建engine
- ## 在线推理
- ### 启动服务
- ### 单样例测试
- ### 多batch测试

## Representative commands

- `docker run -it --privileged \`
- `docker start qwen25vl_infer`
- `docker exec -it qwen25vl_infer /bin/bash`
- `cd /workspace`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_vl/output.tar.gz`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_vl/qwen25vl_infer_scripts.tar.gz`
- `cd /workspace/output`
- `cd /workspace/qwen25vl_infer_scripts`
- `bash build_engines_8_devices.sh`
- `bash start_vllm_server_v2_8_devices.sh`
- `docker exec -it qwen25vl_infer bash`
- `bash simple_test.sh`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_vl/output.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_vl/qwen25vl_infer_scripts.tar.gz>
- <https://huggingface.co/Qwen/Qwen2.5-VL-32B-Instruct>

## Related derived pages

- [`Qwen2.5-VL`](../models/Qwen2.5-VL.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
