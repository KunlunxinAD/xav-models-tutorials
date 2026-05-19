# Source Summary: Qwen2-VL-7B

## Source

- [`qwen2vl_7b_trainval.md`](../../tutorials/qwen2vl_7b_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen2-VL-7B |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT, LoRA |
| Frameworks / backends | Deepspeed, LlamaFactory |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Qwen2-VL-7B
- ## 准备环境
- ## 准备数据集及代码
- ### 准备数据集
- ### 下载代码及预训练权重
- ## 启动容器
- ## 单机8卡训练
- ## 训练与评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `Qwen2_VL_test` |
| `MODEL_PATH` | `</path/to/qwen2-vl> #本地路径` |

## Representative commands

- `git lfs install`
- `git clone https://hf-mirror.com/datasets/BUAADreamer/llava-en-zh-300k`
- `cd /home`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/qwen2-vl.tar.gz`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda create -n Qwen2VL_env --clone python38_torch201_cuda`
- `conda init bash`
- `conda activate Qwen2VL_env`
- `pip install llamafactory==0.9.1`
- `pip install transformers==4.45.2`
- `pip install trl==0.8.6`

## URLs mentioned

- <https://github.com/QwenLM/Qwen2-VL?tab=readme-ov-file#training>
- <https://hf-mirror.com/datasets/BUAADreamer/llava-en-zh-300k>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/qwen2-vl.tar.gz>
- <https://hf-mirror.com/datasets/BUAADreamer/llava-en-zh-2k>

## Related derived pages

- [`Qwen2-VL-7B`](../models/Qwen2-VL-7B.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
