# Source Summary: BEVFormer

## Source

- [`bevformer_trainval.md`](../../tutorials/bevformer_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | BEVFormer |
| Domain | Autonomous Driving |
| Workflow tags | Pretrain, Trainval |
| Frameworks / backends | Detectron2 |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | nuScenes |

## Source outline

- # BEVFormer
- ## 准备环境
- ## 准备数据集及代码
- ### 准备数据集
- ### 下载模型代码
- ### 下载resenet101预训练权重
- ### 安装依赖包 (也可以从官方源中安装)
- ## 启动容器
- ## 执行训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `BEVFormer-test` |
| `NUSCENES_PATH` | `/path/to/nuscenes` |

## Representative commands

- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/can_bus.zip`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_train.pkl`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_val.pkl`
- `wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/bevformer/BEVFormer.tar.gz`
- `wget -O /path/to/BEVFormer/ckpts/r101_dcn_fcos3d_pretrain.pth http://klx-sdk-release-public.su.bcebos.com/xav/data/r101_dcn_fcos3d_pretrain.pth`
- `pip install http://klx-sdk-release-public.su.bcebos.com/xav/release/v0.4/fvcore-0.1.6-py3-none-any.whl`
- `pip install http://klx-sdk-release-public.su.bcebos.com/xav/release/v0.4/detectron2-0.6-cp38-cp38-linux_x86_64.whl`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `cd /path/to/BEVFormer`
- `./tools/dist_train_1x1.sh ./projects/configs/bevformer/bevformer_base.py 1`
- `./tools/dist_train_1x8.sh ./projects/configs/bevformer/bevformer_base.py 8`

## URLs mentioned

- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/can_bus.zip>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_train.pkl>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/data/nuscenes_infos_temporal_val.pkl>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/models/bevformer/BEVFormer.tar.gz>
- <http://klx-sdk-release-public.su.bcebos.com/xav/data/r101_dcn_fcos3d_pretrain.pth>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/v0.4/fvcore-0.1.6-py3-none-any.whl>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/v0.4/detectron2-0.6-cp38-cp38-linux_x86_64.whl>

## Related derived pages

- [`BEVFormer`](../models/BEVFormer.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
