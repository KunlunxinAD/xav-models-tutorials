# Source Summary: StateTransformer

## Source

- [`StateTransformer_trainval.md`](../../tutorials/StateTransformer_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | StateTransformer |
| Domain | General |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # StateTransformer
- ## 准备环境
- ## 下载数据集
- ## 启动容器
- ## 下载及安装资源
- ## 训练与评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-StateTransformer` |
| `MODEL_PATH` | `</path/to/model>` |
| `DATASET_PATH` | `</path/to/dataset>` |

## Representative commands

- `wget http://180.167.251.46:880/NuPlanSTR/nuplan-v1.1_STR.zip`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `cd /home`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/StateTransformer/StateTransformer.tar.gz`
- `conda activate python310_torch25_cuda`
- `cd /home/StateTransformer`
- `pip install -e .`
- `git clone https://github.com/motional/nuplan-devkit.git`
- `git fetch --tags`
- `git checkout nuplan-devkit-v1.2`
- `pip install aioboto3`

## URLs mentioned

- <http://180.167.251.46:880/NuPlanSTR/nuplan-v1.1_STR.zip>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/StateTransformer/StateTransformer.tar.gz>
- <https://github.com/motional/nuplan-devkit.git>

## Related derived pages

- [`StateTransformer`](../models/StateTransformer.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
