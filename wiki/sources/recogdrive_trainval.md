# Source Summary: recogdrive

## Source

- [`recogdrive_trainval.md`](../../tutorials/recogdrive_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | recogdrive |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | flash_attn |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # recogdrive
- ## 环境准备
- ### PCIe环境配置（OAM跳过此步骤）
- #### 确定PCIe环境
- #### 配置PCIe的8卡互联模式
- ## 准备数据集及代码
- ### 准备数据集
- ### 下载代码及预训练权重
- ## 启动容器
- ## 单机多卡训练
- ### 设置环境变量
- ### stage3数据缓存
- ### 执行8卡stage3训练
- ## 单机多卡评估
- ### 执行评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `NAME_CONTAINER` | `recogdrive` |
| `MODEL_PATH` | `</path/to/models> #本地路径` |
| `BKCL_TREE_THRESHOLD` | `0` |
| `XMLIR_ENABLE_NEW_PG` | `1` |

## Representative commands

- `cd recogdrive`
- `bash download/download_maps.sh`
- `bash download/download_navtrain.sh`
- `bash download/download_test.sh`
- `cd /workspace`
- `git lfs install`
- `git clone https://huggingface.co/owl10/ReCogDrive-VLM-2B`
- `git clone https://huggingface.co/owl10/ReCogDrive-2B-IL`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `pip install -e .`
- `pip install decord==0.6.0`

## URLs mentioned

- <https://huggingface.co/owl10/ReCogDrive-VLM-2B>
- <https://huggingface.co/owl10/ReCogDrive-2B-IL>

## Related derived pages

- [`recogdrive`](../models/recogdrive.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
