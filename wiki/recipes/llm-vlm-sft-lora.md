# LLM / VLM SFT and LoRA Recipe

## Purpose

Reusable high-level flow for LLM/VLM/VLA SFT, LoRA, and GRPO-style training tutorials.

## Source-backed tutorials

| Tutorial | Model / topic | Workflow | Frameworks / backends | Representative sections |
| --- | --- | --- | --- | --- |
| [`Bunny_trainval.md`](../../tutorials/Bunny_trainval.md) | [`Bunny`](../models/Bunny.md) | Pretrain, Trainval, SFT, LoRA | Not explicitly extracted | Bunny Trainval Guide, 环境准备, 数据集准备, 启动容器环境, 资源下载及环境准备 |
| [`Internvl3_8b_trainval.md`](../../tutorials/Internvl3_8b_trainval.md) | [`InternVL3-8B`](../models/InternVL3-8B.md) | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory | InternVL3-8B Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
| [`LLaVA_pretrain_trainval.md`](../../tutorials/LLaVA_pretrain_trainval.md) | [`LLaVA`](../models/LLaVA.md) | Inference, Pretrain, Trainval | flash_attn | LLaVA, 准备环境, 启动容器, 下载模型及安装资源, 权重准备 |
| [`LLaVA_trainval.md`](../../tutorials/LLaVA_trainval.md) | [`LLaVA`](../models/LLaVA.md) | Inference, Pretrain, Trainval, SFT | flash_attn | LLaVA, 准备环境, 启动容器, 下载模型及安装资源, 权重准备 |
| [`openvla_trainval.md`](../../tutorials/openvla_trainval.md) | [`OpenVLA`](../models/OpenVLA.md) | Pretrain, Trainval, SFT | flash_attn, wandb | OpenVLA Trainval Guide, 环境准备, 准备开发环境镜像, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境 |
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
| [`qwen3_llamafactory_trainval.md`](../../tutorials/qwen3_llamafactory_trainval.md) | [`Qwen3 (LlamaFactory)`](../models/Qwen3-LlamaFactory.md) | Trainval, SFT | Deepspeed, flash_attn, LlamaFactory, wandb | Qwen3 Trainval Guide (LlamaFactory), 准备环境, 启动容器, 配置容器内环境, 下载框架及安装 |
| [`qwen3vl_8b_grpo_verl_trainval.md`](../../tutorials/qwen3vl_8b_grpo_verl_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | Pretrain, Trainval, GRPO | verl | Qwen3vl-8B grpo verl Trainval Guide, 环境准备, 准备开发环境镜像, 数据集及模型准备, 数据集准备 |
| [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, MS-Swift, wandb | Qwen3-VL-8B MS-Swift Trainval Guide, 环境准备, 准备开发环境镜像, 数据集及代码准备, 数据集准备 |

## Reuse guidance

- Use the linked source tutorial for exact commands and paths.
- Preserve placeholders such as `<XAV_IMAGE>`, `<CONTAINER_NAME>`, `</path/to/model>`, and `</path/to/dataset>`.
- Do not infer performance or memory requirements from this recipe; use benchmark/source pages when available.
