# Source Summary: GameFormer-Planner

## Source

- [`GameFormer-Planner_trainval.md`](../../tutorials/GameFormer-Planner_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | GameFormer-Planner |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # GameFormer-Planner
- ## 准备环境
- ## 准备数据集及代码
- ### 准备数据集
- ### 下载模型代码
- ## 启动容器
- ## 执行训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `Gameformer-Planner-test` |
| `NUSCENES_PATH` | `/path/to/nuplan` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/xav/data/nuPlan_datasets/processed_data_large.tar`
- `wget https://klx-sdk-release-public.su.bcebos.com/xav/release/models/GameFormer-Planner.tar.gz`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `cd GameFormer-Planner`
- `bash train.sh 1`
- `bash train.sh 8`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/xav/data/nuPlan_datasets/processed_data_large.tar>
- <https://klx-sdk-release-public.su.bcebos.com/xav/release/models/GameFormer-Planner.tar.gz>

## Related derived pages

- [`GameFormer-Planner`](../models/GameFormer-Planner.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
