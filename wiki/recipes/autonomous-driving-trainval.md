# Autonomous Driving Trainval Recipe

## Purpose

Reusable high-level flow for AD training/evaluation tutorials: prepare image, prepare dataset/code, start XPU container, install resources, run train/eval scripts.

## Source-backed tutorials

| Tutorial | Model / topic | Workflow | Frameworks / backends | Representative sections |
| --- | --- | --- | --- | --- |
| [`AlphaDrive_trainval.md`](../../tutorials/AlphaDrive_trainval.md) | [`AlphaDrive`](../models/AlphaDrive.md) | Pretrain, Trainval, GRPO | Deepspeed, flash_attn, vLLM, wandb, xvLLM | AlphaDrive Trainval Guide, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |
| [`bevdet_trainval.md`](../../tutorials/bevdet_trainval.md) | [`BEVDet`](../models/BEVDet.md) | Pretrain, Trainval | Not explicitly extracted | BEVDet, 准备环境, 准备数据集及代码, 准备数据集, 下载数据集pkl |
| [`bevformer_trainval.md`](../../tutorials/bevformer_trainval.md) | [`BEVFormer`](../models/BEVFormer.md) | Pretrain, Trainval | Detectron2 | BEVFormer, 准备环境, 准备数据集及代码, 准备数据集, 下载模型代码 |
| [`bevfusion_trainval.md`](../../tutorials/bevfusion_trainval.md) | [`BEVFusion-MMDetection3D`](../models/BEVFusion-MMDetection3D.md) | Pretrain, Trainval | MMDetection3D | BEVFusion-MMDetection3D, 概述, 准备环境, 准备数据集, 启动容器 |
| [`CenterPoint.md`](../../tutorials/CenterPoint.md) | [`CenterPoint`](../models/CenterPoint.md) | Trainval | Not explicitly extracted | CenterPoint Trainval Guide, 准备环境, 下载数据集, 启动容器, 下载及安装依赖 |
| [`Far3D_trainval.md`](../../tutorials/Far3D_trainval.md) | [`Far3D`](../models/Far3D.md) | Trainval | Not explicitly extracted | Far3D, 准备环境, 下载数据集, 启动容器, 下载及安装资源 |
| [`FastBEV_trainval.md`](../../tutorials/FastBEV_trainval.md) | [`FastBEV`](../models/FastBEV.md) | Trainval | MMCV | FastBEV, 准备环境, 下载数据集, 启动容器, 下载及安装依赖 |
| [`flashocc_trainval.md`](../../tutorials/flashocc_trainval.md) | [`FlashOCC`](../models/FlashOCC.md) | Pretrain, Trainval | MMCV, MMDetection3D, wandb | FlashOCC, 准备环境, 准备数据集及代码, 下载模型代码及预训练权重, 下载数据集pkl |
| [`GameFormer-Planner_trainval.md`](../../tutorials/GameFormer-Planner_trainval.md) | [`GameFormer-Planner`](../models/GameFormer-Planner.md) | Trainval | Not explicitly extracted | GameFormer-Planner, 准备环境, 准备数据集及代码, 准备数据集, 下载模型代码 |
| [`lanesegnet_trainval.md`](../../tutorials/lanesegnet_trainval.md) | [`LaneSegNet`](../models/LaneSegNet.md) | Trainval | Not explicitly extracted | LaneSegNet, 准备环境, 获取代码数据集, 准备数据集, 下载模型代码 |
| [`maptrv2_trainval.md`](../../tutorials/maptrv2_trainval.md) | [`MapTRv2`](../models/MapTRv2.md) | Pretrain, Trainval | Not explicitly extracted | MapTRv2, 准备环境, 准备数据集及代码, 准备数据集, 下载预处理后的数据 |
| [`mapvr_trainval.md`](../../tutorials/mapvr_trainval.md) | [`MapVR`](../models/MapVR.md) | Pretrain, Trainval | Not explicitly extracted | MapVR, 准备环境, 准备数据集及代码, 准备数据集, 下载预处理后的数据 |
| [`multipathpp_trainval.md`](../../tutorials/multipathpp_trainval.md) | [`Multipath++`](../models/Multipath++.md) | Trainval | Not explicitly extracted | Multipath++, 准备环境, 配置环境, 启动容器, 下载资源 |
| [`panoocc_trainval.md`](../../tutorials/panoocc_trainval.md) | [`PanoOcc`](../models/PanoOcc.md) | Pretrain, Trainval | Not explicitly extracted | PanoOcc, 准备环境, 准备数据集及代码, 准备数据集, 预处理数据集 |
| [`PETR_trainval.md`](../../tutorials/PETR_trainval.md) | [`PETR`](../models/PETR.md) | Trainval | MMDetection3D | PETR Trainval Guide, 环境准备, 镜像, 创建docker, 准备数据集 |
| [`petrv2_trainval.md`](../../tutorials/petrv2_trainval.md) | [`PETRv2`](../models/PETRv2.md) | Pretrain, Trainval | MMDetection3D | PETRv2, 准备环境, 启动容器, 下载及安装资源, 下载PETRv2代码 |
| [`PointPillar_trainval.md`](../../tutorials/PointPillar_trainval.md) | [`PointPillar`](../models/PointPillar.md) | Trainval | MMCV, MMDetection3D | PointPillar, 准备环境, 准备数据集及代码, 准备数据集, 下载模型代码 |
| [`QCNet_trainval.md`](../../tutorials/QCNet_trainval.md) | [`QCNet`](../models/QCNet.md) | Trainval | Not explicitly extracted | QCNet, 准备环境, 准备数据集及代码, 下载模型代码, 准备数据集 |
| [`recogdrive_trainval.md`](../../tutorials/recogdrive_trainval.md) | [`recogdrive`](../models/recogdrive.md) | Pretrain, Trainval | flash_attn | recogdrive, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |
| [`sparse4d_trainval.md`](../../tutorials/sparse4d_trainval.md) | [`Sparse4D`](../models/Sparse4D.md) | Pretrain, Trainval | MMCV | Sparse4D, 环境准备, 数据集及代码准备, 数据集准备, 下载代码及预训练权重 |
| [`SparseDrive_trainval.md`](../../tutorials/SparseDrive_trainval.md) | [`SparseDrive`](../models/SparseDrive.md) | Trainval | MMCV | SparseDrive, 准备环境, 下载数据集, 启动容器, 下载及安装资源 |
| [`StreamPETR_trainval.md`](../../tutorials/StreamPETR_trainval.md) | [`StreamPETR`](../models/StreamPETR.md) | Pretrain, Trainval | flash_attn, MMCV, MMDetection3D | StreamPETR, 准备环境, 启动容器, 下载及安装资源, 环境适配 |
| [`TRL_AlphaDrive_trainval.md`](../../tutorials/TRL_AlphaDrive_trainval.md) | [`AlphaDrive`](../models/AlphaDrive.md) | Pretrain, Trainval, SFT, GRPO | Deepspeed, flash_attn, vLLM, xvLLM | AlphaDrive Trainval Guide, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |
| [`UniAD_trainval.md`](../../tutorials/UniAD_trainval.md) | [`UniAD`](../models/UniAD.md) | Trainval | MMCV | UniAD, 准备环境, 准备数据集, 启动容器, 下载及安装资源 |
| [`VAD_trainval.md`](../../tutorials/VAD_trainval.md) | [`VAD`](../models/VAD.md) | Trainval | Not explicitly extracted | VAD, 准备环境, 下载数据集, 启动容器, 下载及安装资源 |

## Reuse guidance

- Use the linked source tutorial for exact commands and paths.
- Preserve placeholders such as `<XAV_IMAGE>`, `<CONTAINER_NAME>`, `</path/to/model>`, and `</path/to/dataset>`.
- Do not infer performance or memory requirements from this recipe; use benchmark/source pages when available.
