# Source Summary: DriveDreamer Trainval Guide

## Source

- [`DriveDreamer_trainval.md`](../../tutorials/DriveDreamer_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | DriveDreamer |
| Domain | World Model |
| Workflow tags | Trainval |
| Frameworks / backends | Deepspeed |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # DriveDreamer Trainval Guide
- ## 环境准备
- ### 镜像
- ### 启动容器
- ### 环境
- ### 代码及数据集
- ## 启动训练
- ### 默认配置文件
- ## train

## Representative commands

- `docker run -d -p 8000:8081 -it          \`
- `conda activate python310_torch25_cuda`
- `pip install nuscenes-devkit`
- `pip install lmdb`
- `pip install decord`
- `pip install accelerate`
- `pip install transformers`
- `pip install pytorch-fid`
- `pip install lpips`
- `pip install terminaltables`
- `pip install opencv-python`
- `pip install ftfy`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/xav/release/models/DriveDreamer/drivedreamer.tgz>
- <https://klx-sdk-release-public.su.bcebos.com/xav/release/models/DriveDreamer/DriveDreamer_data_ckpts.tgz>

## Related derived pages

- [`DriveDreamer`](../models/DriveDreamer.md)
- [`world-model-trainval`](../recipes/world-model-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
