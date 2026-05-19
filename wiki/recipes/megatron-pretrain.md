# Megatron / XMegatron Pretrain Recipe

## Purpose

Reusable high-level flow for Megatron/XMegatron pretraining tutorials.

## Source-backed tutorials

| Tutorial | Model / topic | Workflow | Frameworks / backends | Representative sections |
| --- | --- | --- | --- | --- |
| [`AlphaDrive_trainval.md`](../../tutorials/AlphaDrive_trainval.md) | [`AlphaDrive`](../models/AlphaDrive.md) | Pretrain, Trainval, GRPO | Deepspeed, flash_attn, vLLM, wandb, xvLLM | AlphaDrive Trainval Guide, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |
| [`bevdet_trainval.md`](../../tutorials/bevdet_trainval.md) | [`BEVDet`](../models/BEVDet.md) | Pretrain, Trainval | Not explicitly extracted | BEVDet, 准备环境, 准备数据集及代码, 准备数据集, 下载数据集pkl |
| [`bevformer_trainval.md`](../../tutorials/bevformer_trainval.md) | [`BEVFormer`](../models/BEVFormer.md) | Pretrain, Trainval | Detectron2 | BEVFormer, 准备环境, 准备数据集及代码, 准备数据集, 下载模型代码 |
| [`bevfusion_trainval.md`](../../tutorials/bevfusion_trainval.md) | [`BEVFusion-MMDetection3D`](../models/BEVFusion-MMDetection3D.md) | Pretrain, Trainval | MMDetection3D | BEVFusion-MMDetection3D, 概述, 准备环境, 准备数据集, 启动容器 |
| [`Bunny_trainval.md`](../../tutorials/Bunny_trainval.md) | [`Bunny`](../models/Bunny.md) | Pretrain, Trainval, SFT, LoRA | Not explicitly extracted | Bunny Trainval Guide, 环境准备, 数据集准备, 启动容器环境, 资源下载及环境准备 |
| [`cosmos_predict2.5_trainval.md`](../../tutorials/cosmos_predict2.5_trainval.md) | [`cosmos-predict2.5`](../models/cosmos-predict2.5.md) | Inference, Trainval | Megatron | cosmos-predict2.5, 准备环境, 启动容器, 配置容器环境, 下载及安装资源 |
| [`flashocc_trainval.md`](../../tutorials/flashocc_trainval.md) | [`FlashOCC`](../models/FlashOCC.md) | Pretrain, Trainval | MMCV, MMDetection3D, wandb | FlashOCC, 准备环境, 准备数据集及代码, 下载模型代码及预训练权重, 下载数据集pkl |
| [`Internvl3_8b_trainval.md`](../../tutorials/Internvl3_8b_trainval.md) | [`InternVL3-8B`](../models/InternVL3-8B.md) | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory | InternVL3-8B Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
| [`LLaVA_pretrain_trainval.md`](../../tutorials/LLaVA_pretrain_trainval.md) | [`LLaVA`](../models/LLaVA.md) | Inference, Pretrain, Trainval | flash_attn | LLaVA, 准备环境, 启动容器, 下载模型及安装资源, 权重准备 |
| [`LLaVA_trainval.md`](../../tutorials/LLaVA_trainval.md) | [`LLaVA`](../models/LLaVA.md) | Inference, Pretrain, Trainval, SFT | flash_attn | LLaVA, 准备环境, 启动容器, 下载模型及安装资源, 权重准备 |
| [`maptrv2_trainval.md`](../../tutorials/maptrv2_trainval.md) | [`MapTRv2`](../models/MapTRv2.md) | Pretrain, Trainval | Not explicitly extracted | MapTRv2, 准备环境, 准备数据集及代码, 准备数据集, 下载预处理后的数据 |
| [`mapvr_trainval.md`](../../tutorials/mapvr_trainval.md) | [`MapVR`](../models/MapVR.md) | Pretrain, Trainval | Not explicitly extracted | MapVR, 准备环境, 准备数据集及代码, 准备数据集, 下载预处理后的数据 |
| [`openvla_trainval.md`](../../tutorials/openvla_trainval.md) | [`OpenVLA`](../models/OpenVLA.md) | Pretrain, Trainval, SFT | flash_attn, wandb | OpenVLA Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
| [`PaddleOCR_trainval.md`](../../tutorials/PaddleOCR_trainval.md) | [`PaddleOCR-v5`](../models/PaddleOCR-v5.md) | Inference, Pretrain, Trainval | PaddlePaddle | PaddleOCR_v5, 准备环境, 启动容器, 配置容器内环境, 准备数据集及代码 |
| [`panoocc_trainval.md`](../../tutorials/panoocc_trainval.md) | [`PanoOcc`](../models/PanoOcc.md) | Pretrain, Trainval | Not explicitly extracted | PanoOcc, 准备环境, 准备数据集及代码, 准备数据集, 预处理数据集 |
| [`petrv2_trainval.md`](../../tutorials/petrv2_trainval.md) | [`PETRv2`](../models/PETRv2.md) | Pretrain, Trainval | MMDetection3D | PETRv2, 准备环境, 启动容器, 下载及安装资源, 下载PETRv2代码 |
| [`Pi_0_trainval.md`](../../tutorials/Pi_0_trainval.md) | [`Pi0`](../models/Pi0.md) | Pretrain, Trainval, SFT | Lerobot, wandb | Pi_0, 环境准备, 准备数据集及代码, 准备数数据集, 数据集介绍 |
| [`qwen2.5_trainval.md`](../../tutorials/qwen2.5_trainval.md) | [`Qwen2.5`](../models/Qwen2.5.md) | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, LlamaFactory | Qwen2.5 Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
| [`qwen2.5vl_3b_trainval.md`](../../tutorials/qwen2.5vl_3b_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory | Qwen2.5-VL-3B Trainval Guide, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |
| [`qwen2.5vl_r1_trainval.md`](../../tutorials/qwen2.5vl_r1_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | Pretrain, Trainval | Deepspeed, flash_attn | Qwen2.5-VL-R1 Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
| [`qwen2.5vl_trainval.md`](../../tutorials/qwen2.5vl_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory | Qwen2.5-VL Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
| [`qwen2_7b_trainval.md`](../../tutorials/qwen2_7b_trainval.md) | [`Qwen2-7B`](../models/Qwen2-7B.md) | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, LlamaFactory | Qwen2-7B Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
| [`qwen2vl_7b_trainval.md`](../../tutorials/qwen2vl_7b_trainval.md) | [`Qwen2-VL-7B`](../models/Qwen2-VL-7B.md) | Pretrain, Trainval, SFT, LoRA | Deepspeed, LlamaFactory | Qwen2-VL-7B, 准备环境, 准备数据集及代码, 准备数据集, 下载代码及预训练权重 |
| [`qwen3_235b_a22b_thinking_2507_infer.md`](../../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) | [`Qwen3-235B-A22B-Thinking-2507`](../models/Qwen3-235B-A22B-Thinking-2507.md) | Inference, Pretrain, Trainval | SGLang | Qwen3-235B-A22B-Thinking-2507 Inference Guide, 准备环境, 启动容器, 准备数据集及模型, 配置容器内环境 |
| [`qwen3_30b_a3b_pretrain.md`](../../tutorials/qwen3_30b_a3b_pretrain.md) | [`Qwen3-30B-A3B`](../models/Qwen3-30B-A3B.md) | Pretrain, Trainval, SFT | Megatron, wandb, XMegatron | Qwen3-30B-A3B Pretrain Guide, 环境准备, 启动容器, 安装xmegatron_ext, 安装其他依赖 |
| [`qwen3_8b_megatron_trainval.md`](../../tutorials/qwen3_8b_megatron_trainval.md) | [`Qwen3-8B`](../models/Qwen3-8B.md) | Pretrain, Trainval, SFT | Megatron, wandb | Qwen3-8B Megatron Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
| [`qwen3_8b_xmegatron_trainval.md`](../../tutorials/qwen3_8b_xmegatron_trainval.md) | [`Qwen3-8B`](../models/Qwen3-8B.md) | Pretrain, Trainval | Megatron, wandb, XMegatron | Qwen3-8B XMegatron Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
| [`qwen3vl_8b_grpo_verl_trainval.md`](../../tutorials/qwen3vl_8b_grpo_verl_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | Pretrain, Trainval, GRPO | verl | Qwen3vl-8B grpo verl Trainval Guide, 环境准备, 准备开发环境镜像, 数据集及模型准备, 数据集准备 |
| [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, MS-Swift, wandb | Qwen3-VL-8B MS-Swift Trainval Guide, 环境准备, 准备开发环境镜像, 数据集及代码准备, 数据集准备 |
| [`recogdrive_trainval.md`](../../tutorials/recogdrive_trainval.md) | [`recogdrive`](../models/recogdrive.md) | Pretrain, Trainval | flash_attn | recogdrive, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |
| [`sparse4d_trainval.md`](../../tutorials/sparse4d_trainval.md) | [`Sparse4D`](../models/Sparse4D.md) | Pretrain, Trainval | MMCV | Sparse4D, 环境准备, 数据集及代码准备, 数据集准备, 下载代码及预训练权重 |
| [`StreamPETR_trainval.md`](../../tutorials/StreamPETR_trainval.md) | [`StreamPETR`](../models/StreamPETR.md) | Pretrain, Trainval | flash_attn, MMCV, MMDetection3D | StreamPETR, 准备环境, 启动容器, 下载及安装资源, 环境适配 |
| [`TRL_AlphaDrive_trainval.md`](../../tutorials/TRL_AlphaDrive_trainval.md) | [`AlphaDrive`](../models/AlphaDrive.md) | Pretrain, Trainval, SFT, GRPO | Deepspeed, flash_attn, vLLM, xvLLM | AlphaDrive Trainval Guide, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |
| [`vLLM_infer.md`](../../tutorials/vLLM_infer.md) | [`vLLM`](../models/vLLM.md) | Inference, Pretrain, Trainval | vLLM, xvLLM | vLLM infer Guide, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |

## Reuse guidance

- Use the linked source tutorial for exact commands and paths.
- Preserve placeholders such as `<XAV_IMAGE>`, `<CONTAINER_NAME>`, `</path/to/model>`, and `</path/to/dataset>`.
- Do not infer performance or memory requirements from this recipe; use benchmark/source pages when available.
