# Source Summary: cosmos-transfer2.5

## Source

- [`cosmos_transfer2.5_trainval.md`](../../tutorials/cosmos_transfer2.5_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | cosmos-transfer2.5 |
| Domain | World Model |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # cosmos-transfer2.5
- ## 准备环境
- ## 启动容器
- # 代码
- # 环境
- # 测试
- # 训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `CUDA_LAUNCH_BLOCKING` | `1` |
| `BKCL_TREE_THRESHOLD` | `1` |

## Representative commands

- `docker run -d -p 8000:8081 -it          \`
- `git clone https://github.com/nvidia-cosmos/cosmos-transfer2.5.git`
- `cd cosmos-transfer2.5`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos-transfer2.5/assets.tgz`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos-transfer2.5/support_xpu.patch`
- `git apply support_xpu.patch`
- `wget -O xpytorch-cp310-torch251-ubuntu2004-x64.run https://klx-sdk-release-public.su.bcebos.com/xav/release/docker_images_d/25.12-py310/updates/xpytorch-cp310-t`
- `bash xpytorch-cp310-torch251-ubuntu2004-x64.run && rm xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `pip install -e ./packages/cosmos-cuda`
- `pip install -e ./packages/cosmos-oss`
- `pip install cattrs>=25.2.0`
- `pip install easydict>=1.9`

## URLs mentioned

- <https://github.com/nvidia-cosmos/cosmos-transfer2.5.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos-transfer2.5/assets.tgz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos-transfer2.5/support_xpu.patch>
- <https://github.com/nvidia-cosmos/cosmos-transfer2.5>
- <https://klx-sdk-release-public.su.bcebos.com/xav/release/docker_images_d/25.12-py310/updates/xpytorch-cp310-torch251-ubuntu2004-x64.run>
- <https://github.com/jeanachoi/Video-Depth-Anything.git>
- <https://github.com/nvidia-cosmos/cosmos-transfer2.5/blob/main/docs/post-training_singleview.md>
- <https://github.com/nvidia-cosmos/cosmos-transfer2.5/blob/main/docs/post-training_auto_multiview.md>

## Related derived pages

- [`cosmos-transfer2.5`](../models/cosmos-transfer2.5.md)
- [`world-model-trainval`](../recipes/world-model-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
