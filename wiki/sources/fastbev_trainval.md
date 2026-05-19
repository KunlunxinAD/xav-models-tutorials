# Source Summary: FastBEV

## Source

- [`FastBEV_trainval.md`](../../tutorials/FastBEV_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | FastBEV |
| Domain | Autonomous Driving |
| Workflow tags | Trainval |
| Frameworks / backends | MMCV |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | nuScenes |

## Source outline

- # FastBEV
- ## 准备环境
- ## 下载数据集
- ## 启动容器
- ## 下载及安装依赖
- ## 执行训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<path/to/image>` |
| `XAV_CONTAINER` | `Fast_BEV_test` |
| `NUSCENES_PATH` | `/path/to/nuscenes` |
| `PYTHONPATH` | `/workspace/Fast-BEV:$PYTHONPATH` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/Fast_BEV/Fast_BEV.tar.gz`
- `cd Fast-BEV`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/fastbve_data.tar.gz && tar -zxvf fastbve_data.tar.gz && rm fastbve_data.tar.gz`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/mmcv_registry.patch`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/sparse_init.patch`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/batchnorm.patch`
- `bash /opt/xav_sdk/scripts/env_setup_xav_d_2511_py38.sh`
- `pip install --force-reinstall numba==0.48.0 numpy==1.23.5`
- `conda activate python38_torch201_cuda`

## URLs mentioned

- <https://github.com/hustvl/VAD/blob/main/docs/prepare_dataset.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/data/changan_nuscenes.tar>
- <http://klx-sdk-release-public.su.bcebos.com/xav/release/models/Fast_BEV/Fast_BEV.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/fastbve_data.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/mmcv_registry.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/sparse_init.patch>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/Fast_BEV/batchnorm.patch>

## Related derived pages

- [`FastBEV`](../models/FastBEV.md)
- [`autonomous-driving-trainval`](../recipes/autonomous-driving-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
