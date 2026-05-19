# Source Summary: LaneSegNet

## Source

- [`lanesegnet_trainval.md`](../../tutorials/lanesegnet_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | LaneSegNet |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | Not explicitly extracted |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # LaneSegNet
- ## 准备环境
- ## 获取代码数据集
- ### 准备数据集
- ### 下载模型代码
- ## 启动容器
- ## 训练与评估
- ### 配置环境
- ### 预处理数据集
- ### 执行训练
- ### 执行评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `DATA_PATH` | `/data` |
| `XAV_CONTAINER` | `laneseg_test` |
| `PYTHONPATH` | `$PYTHONPATH:/path/to/lanesegnet` |

## Representative commands

- `git clone https://github.com/OpenDriveLab/LaneSegNet`
- `docker run -dti --name ${XAV_CONTAINER} \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `cd LaneSegNet`
- `./tools/dist_train.sh 8`
- `./tools/dist_test.sh 8 [--show]`

## URLs mentioned

- <https://github.com/OpenDriveLab/OpenLane-V2/tree/v2.1.0/data>
- <https://openxlab.org.cn/datasets/OpenDriveLab/OpenLane-V2/cli/main>
- <https://github.com/OpenDriveLab/LaneSegNet>
- <https://github.com/OpenDriveLab/LaneSegNet?tab=readme-ov-file#prepare-dataset>

## Related derived pages

- [`LaneSegNet`](../models/LaneSegNet.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
