# Source Summary: Llama2-70B Inference Guide

## Source

- [`llama2_70b_infer.md`](../../tutorials/llama2_70b_infer.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Llama2-70B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Inference |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | FP16 |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Llama2-70B Inference Guide
- ## 环境准备
- ### 准备开发环境镜像
- ## 启动容器
- ### 容器内环境配置
- ### 依赖安装
- ## 模型下载
- ### 模型下载到models目录并解压
- ## 推理定长测试
- ### 模型转换
- ### 构建engine
- ### 执行运行
- ### 摘要生成

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `image_name` | `<XAV_IMAGE>` |
| `XPU_VISIBLE_DEVICES` | `"0,1,2,3,4,5,6,7"` |

## Representative commands

- `docker run -it --name llama2_infer_mt \`
- `docker start llama2_infer_mt`
- `docker exec -it llama2_infer_mt /bin/bash`
- `cd /workspace`
- `wget https://klx-sdk-release-public.su.bcebos.com/xtrt_llm/release/daily/xtrt_llm_0.10.0/20241017/xtrt_llm_ubuntu1604_output.tar.gz`
- `cd output`
- `bash scripts/install_release.sh`
- `cd /workspace/models`
- `wget https://klx-llm.fsh.bcebos.com/pretrained_models/Llama-2-70b-chat-hf.tar.gz`
- `cd examples/llama/`
- `python convert_checkpoint.py  --model_dir /workspace/models/Llama-2-70b-chat-hf \`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/xtrt_llm/release/daily/xtrt_llm_0.10.0/20241017/xtrt_llm_ubuntu1604_output.tar.gz>
- <https://klx-llm.fsh.bcebos.com/pretrained_models/Llama-2-70b-chat-hf.tar.gz>

## Related derived pages

- [`Llama2-70B`](../models/Llama2-70B.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
