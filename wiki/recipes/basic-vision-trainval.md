# Basic Vision Trainval Recipe

## Purpose

Reusable high-level flow for basic vision/OCR/detection/segmentation tutorials.

## Source-backed tutorials

| Tutorial | Model / topic | Workflow | Frameworks / backends | Representative sections |
| --- | --- | --- | --- | --- |
| [`DINO_trainval.md`](../../tutorials/DINO_trainval.md) | [`DINO`](../models/DINO.md) | Trainval | Not explicitly extracted | DINO, 准备环境, 启动容器, 下载模型及安装资源, 权重准备 |
| [`mask2former_trainval.md`](../../tutorials/mask2former_trainval.md) | [`Mask2former`](../models/Mask2former.md) | Trainval | Detectron2 | Mask2former, 准备环境, 配置环境, 启动容器, 下载资源 |
| [`MaskRCNN_trainval.md`](../../tutorials/MaskRCNN_trainval.md) | [`MaskRCNN`](../models/MaskRCNN.md) | Trainval | Detectron2 | MaskRCNN, 准备环境, 下载数据集, 启动容器, 下载及安装依赖 |
| [`PaddleOCR_trainval.md`](../../tutorials/PaddleOCR_trainval.md) | [`PaddleOCR-v5`](../models/PaddleOCR-v5.md) | Inference, Pretrain, Trainval | PaddlePaddle | PaddleOCR_v5, 准备环境, 启动容器, 配置容器内环境, 准备数据集及代码 |
| [`regnet_trainval.md`](../../tutorials/regnet_trainval.md) | [`RegNet`](../models/RegNet.md) | Trainval | Not explicitly extracted | RegNet Trainval Guide, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |
| [`VIT_trainval.md`](../../tutorials/VIT_trainval.md) | [`VIT`](../models/VIT.md) | Trainval | Not explicitly extracted | VIT, 准备环境, 启动容器, 下载及安装资源, 下载代码 |
| [`Yolo_inference.md`](../../tutorials/Yolo_inference.md) | [`Yolo`](../models/Yolo.md) | Inference | Not explicitly extracted | Yolo Inference Guide, 准备环境, 启动容器, 安装依赖, PyTorch 推理 |

## Reuse guidance

- Use the linked source tutorial for exact commands and paths.
- Preserve placeholders such as `<XAV_IMAGE>`, `<CONTAINER_NAME>`, `</path/to/model>`, and `</path/to/dataset>`.
- Do not infer performance or memory requirements from this recipe; use benchmark/source pages when available.
