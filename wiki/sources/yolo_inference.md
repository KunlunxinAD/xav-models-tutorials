# Source Summary: Yolo Inference Guide

## Source

- [`Yolo_inference.md`](../../tutorials/Yolo_inference.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Yolo |
| Domain | Vision/OCR |
| Workflow tags | Inference |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # Yolo Inference Guide
- ## 准备环境
- ## 启动容器
- ## 安装依赖
- ## PyTorch 推理

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
- `pip install "ultralytics[export]" -i https://pypi.tuna.tsinghua.edu.cn/simple`
- `cd /home/inference`

## URLs mentioned

- <https://pypi.tuna.tsinghua.edu.cn/simple>

## Related derived pages

- [`Yolo`](../models/Yolo.md)
- [`basic-vision-trainval`](../recipes/basic-vision-trainval.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
