# LLM Inference Recipe

## Purpose

Reusable high-level flow for model download, container startup, engine/server launch, online/offline inference, and benchmark validation.

## Source-backed tutorials

| Tutorial | Model / topic | Workflow | Frameworks / backends | Representative sections |
| --- | --- | --- | --- | --- |
| [`cosmos_predict2.5_trainval.md`](../../tutorials/cosmos_predict2.5_trainval.md) | [`cosmos-predict2.5`](../models/cosmos-predict2.5.md) | Inference, Trainval | Megatron | cosmos-predict2.5, 准备环境, 启动容器, 配置容器环境, 下载及安装资源 |
| [`llama2_70b_infer.md`](../../tutorials/llama2_70b_infer.md) | [`Llama2-70B`](../models/Llama2-70B.md) | Inference | Not explicitly extracted | Llama2-70B Inference Guide, 环境准备, 准备开发环境镜像, 启动容器, 容器内环境配置 |
| [`LLaMA_infer.md`](../../tutorials/LLaMA_infer.md) | [`LLaMA`](../models/LLaMA.md) | Inference | TensorRT, vLLM | LLaMA Inference Guide, 镜像获取, 准备开发环境镜像, 环境准备, 启动容器 |
| [`LLaVA_pretrain_trainval.md`](../../tutorials/LLaVA_pretrain_trainval.md) | [`LLaVA`](../models/LLaVA.md) | Inference, Pretrain, Trainval | flash_attn | LLaVA, 准备环境, 启动容器, 下载模型及安装资源, 权重准备 |
| [`LLaVA_trainval.md`](../../tutorials/LLaVA_trainval.md) | [`LLaVA`](../models/LLaVA.md) | Inference, Pretrain, Trainval, SFT | flash_attn | LLaVA, 准备环境, 启动容器, 下载模型及安装资源, 权重准备 |
| [`PaddleOCR_trainval.md`](../../tutorials/PaddleOCR_trainval.md) | [`PaddleOCR-v5`](../models/PaddleOCR-v5.md) | Inference, Pretrain, Trainval | PaddlePaddle | PaddleOCR_v5, 准备环境, 启动容器, 配置容器内环境, 准备数据集及代码 |
| [`qwen2.5_infer.md`](../../tutorials/qwen2.5_infer.md) | [`Qwen2.5`](../models/Qwen2.5.md) | Inference | TensorRT, vLLM | Qwen2.5 Inference Guide, 镜像获取, 准备开发环境镜像, 环境准备, 启动容器 |
| [`qwen2.5vl_infer.md`](../../tutorials/qwen2.5vl_infer.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | Inference | TensorRT | Qwen2.5-VL Inference Guide, 镜像获取, 准备开发环境镜像, 环境准备, 启动容器 |
| [`qwen3_235b_a22b_thinking_2507_infer.md`](../../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) | [`Qwen3-235B-A22B-Thinking-2507`](../models/Qwen3-235B-A22B-Thinking-2507.md) | Inference, Pretrain, Trainval | SGLang | Qwen3-235B-A22B-Thinking-2507 Inference Guide, 准备环境, 启动容器, 准备数据集及模型, 配置容器内环境 |
| [`qwen3_omni_30b_a3b_infer.md`](../../tutorials/qwen3_omni_30b_a3b_infer.md) | [`Qwen3-Omni-30B-A3B`](../models/Qwen3-Omni-30B-A3B.md) | Inference, Benchmark | vLLM, xvLLM | Qwen3-Omni-30B-A3B Inference Guide, 环境准备, 启动容器, 安装xvllm, 安装vllm-omni |
| [`vLLM_infer.md`](../../tutorials/vLLM_infer.md) | [`vLLM`](../models/vLLM.md) | Inference, Pretrain, Trainval | vLLM, xvLLM | vLLM infer Guide, 环境准备, PCIe环境配置（OAM跳过此步骤）, 确定PCIe环境, 配置PCIe的8卡互联模式 |
| [`xav_vLLM.md`](../../tutorials/xav_vLLM.md) | [`xav-vLLM`](../models/xav-vLLM.md) | Inference, Benchmark | vLLM, xav-vLLM | xav-vLLM, 获取镜像, 准备环境, 启动容器, 配置容器内环境 |
| [`xvllm_general_infer.md`](../../tutorials/xvllm_general_infer.md) | [`xvLLM`](../models/xvLLM.md) | Inference | vLLM, xav-vLLM, xvLLM | xvLLM General Inference Guide, 概述, 准备环境, 启动容器, 容器内目录说明 |
| [`Yolo_inference.md`](../../tutorials/Yolo_inference.md) | [`Yolo`](../models/Yolo.md) | Inference | Not explicitly extracted | Yolo Inference Guide, 准备环境, 启动容器, 安装依赖, PyTorch 推理 |

## Reuse guidance

- Use the linked source tutorial for exact commands and paths.
- Preserve placeholders such as `<XAV_IMAGE>`, `<CONTAINER_NAME>`, `</path/to/model>`, and `</path/to/dataset>`.
- Do not infer performance or memory requirements from this recipe; use benchmark/source pages when available.
