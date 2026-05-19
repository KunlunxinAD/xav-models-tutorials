# Source Summary: Qwen2.5 Inference Guide

## Source

- [`qwen2.5_infer.md`](../../tutorials/qwen2.5_infer.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen2.5 |
| Domain | LLM/VLM/VLA |
| Workflow tags | Inference |
| Frameworks / backends | TensorRT, vLLM |
| Precision mentions | BF16, FP16 |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen2.5 Inference Guide
- ## 镜像获取
- ### 准备开发环境镜像
- ## 环境准备
- ### 启动容器
- ### 容器内环境配置
- ### 依赖安装
- ### 模型下载
- ## Build TensorRT engine
- ### 模型转换
- ### 构建engine
- ## 执行运行
- ## 摘要生成
- ## 在线推理
- ### 创建服务
- ### 功能测试
- ### 定长测试

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `image_name` | `<XAV_IMAGE>` |
| `XPU_VISIBLE_DEVICES` | `"0,1,2,3,4,5,6,7"` |

## Representative commands

- `docker run -it --name qwen25_infer_mt \`
- `docker start qwen25_infer_mt`
- `docker exec -it qwen25_infer_mt /bin/bash`
- `cd /workspace`
- `wget https://klx-sdk-release-public.su.bcebos.com/xtrt_llm/release/daily/xtrt_llm_0.10.0/20250410/xtrt_llm_ubuntu2004_output.tar.gz`
- `cd output`
- `bash scripts/install_release.sh`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_72b/Qwen2.5-72B-Instruct.tar.gz`
- `cd /workspace/output/examples/qwen/`
- `python convert_checkpoint.py --model_dir /workspace/models/Qwen2.5-72B-Instruct/  \`
- `python ../run.py --input_text "What is your name?" \`
- `python ../summarize.py --test_trt_llm \`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/xtrt_llm/release/daily/xtrt_llm_0.10.0/20250410/xtrt_llm_ubuntu2004_output.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_72b/Qwen2.5-72B-Instruct.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/qwen25_72b/run_server_qwen25.sh>
- <http://localhost:8802/v1/completions>
- <https://klx-public.bj.bcebos.com/v1/anyinfer/DeepSeek/V9/pressure_test_v6_1.tar.gz>

## Related derived pages

- [`Qwen2.5`](../models/Qwen2.5.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
