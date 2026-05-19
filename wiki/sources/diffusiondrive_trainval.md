# Source Summary: DiffusionDrive

## Source

- [`DiffusionDrive_trainval.md`](../../tutorials/DiffusionDrive_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | DiffusionDrive |
| Domain | World Model |
| Workflow tags | Trainval |
| Frameworks / backends | Navsim |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| Dataset hints | Not explicitly extracted |

## Source outline

- # DiffusionDrive
- ## 准备环境
- ## 准备数据集及代码
- ### 下载模型代码
- ### 准备数据集
- ## 启动容器
- ### 配置容器内环境
- ## 多卡训练
- ### 下载预处理数据及权重
- ### 执行单机多卡训练
- ### 评估

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `CONTAINER_NAME` | `DiffusionDrive_test` |
| `PATH_TO_MOUNT` | `</path/to/diffusiondrive> #本地路径` |
| `PATH_AFTER_MOUNT` | `/workspace/DiffusionDrive #挂载后在容器内的路径` |
| `HF_ENDPOINT` | `https://hf-mirror.com` |

## Representative commands

- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/DiffusionDrive/DiffusionDrive.tar.gz`
- `cd DiffusionDrive`
- `wget bos:/klx-sdk-release-public/xav/data/navsim_datasets/navsim_dataset.tar`
- `wget https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/DiffusionDrive/exp.tar.gz`
- `docker run -dti \`
- `docker exec -it ${CONTAINER_NAME} bash`
- `conda create -n DiffusionDrive --clone python310_torch25_cuda`
- `conda init bash`
- `conda activate DiffusionDrive`
- `pip install diffusers==0.33.1`
- `cd /workspace/DiffusionDrive`
- `pip install -r requirements.txt`

## URLs mentioned

- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/DiffusionDrive/DiffusionDrive.tar.gz>
- <https://github.com/autonomousvision/navsim>
- <https://su.bcebos.com/v1/klx-sdk-release-public/xav/release/models/DiffusionDrive/exp.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/DiffusionDrive/xpytorch-cp310-torch251-ubuntu2004-x64.run>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/DiffusionDrive/kmeans_navsim_tr>
- <https://hf-mirror.com>

## Related derived pages

- [`DiffusionDrive`](../models/DiffusionDrive.md)
- [`world-model-trainval`](../recipes/world-model-trainval.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
