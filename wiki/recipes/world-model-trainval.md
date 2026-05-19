# World Model Trainval Recipe

## Purpose

Reusable high-level flow for world-model and generative driving model training/inference tutorials.

## Source-backed tutorials

| Tutorial | Model / topic | Workflow | Frameworks / backends | Representative sections |
| --- | --- | --- | --- | --- |
| [`cosmos_predict2.5_trainval.md`](../../tutorials/cosmos_predict2.5_trainval.md) | [`cosmos-predict2.5`](../models/cosmos-predict2.5.md) | Inference, Trainval | Megatron | cosmos-predict2.5, 准备环境, 启动容器, 配置容器环境, 下载及安装资源 |
| [`cosmos_transfer2.5_trainval.md`](../../tutorials/cosmos_transfer2.5_trainval.md) | [`cosmos-transfer2.5`](../models/cosmos-transfer2.5.md) | Trainval | Not explicitly extracted | cosmos-transfer2.5, 准备环境, 启动容器, 代码, 环境 |
| [`DiffusionDrive_trainval.md`](../../tutorials/DiffusionDrive_trainval.md) | [`DiffusionDrive`](../models/DiffusionDrive.md) | Trainval | Navsim | DiffusionDrive, 准备环境, 准备数据集及代码, 下载模型代码, 准备数据集 |
| [`DriveDreamer_trainval.md`](../../tutorials/DriveDreamer_trainval.md) | [`DriveDreamer`](../models/DriveDreamer.md) | Trainval | Deepspeed | DriveDreamer Trainval Guide, 环境准备, 镜像, 启动容器, 环境 |

## Reuse guidance

- Use the linked source tutorial for exact commands and paths.
- Preserve placeholders such as `<XAV_IMAGE>`, `<CONTAINER_NAME>`, `</path/to/model>`, and `</path/to/dataset>`.
- Do not infer performance or memory requirements from this recipe; use benchmark/source pages when available.
