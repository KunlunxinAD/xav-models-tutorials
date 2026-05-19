# Memory Pressure

## Summary

Memory-related source facts such as precision, batch size, max length, device count, and allocator settings. This page does not claim measured memory unless a source states it.

## Source-backed entries

| Tutorial | Model / topic | Domain | Workflow | Frameworks / backends |
| --- | --- | --- | --- | --- |
| [`AlphaDrive_trainval.md`](../../tutorials/AlphaDrive_trainval.md) | [`AlphaDrive`](../models/AlphaDrive.md) | Autonomous Driving | Pretrain, Trainval, GRPO | Deepspeed, flash_attn, vLLM, wandb, xvLLM |
| [`Bunny_trainval.md`](../../tutorials/Bunny_trainval.md) | [`Bunny`](../models/Bunny.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Not explicitly extracted |
| [`cosmos_predict2.5_trainval.md`](../../tutorials/cosmos_predict2.5_trainval.md) | [`cosmos-predict2.5`](../models/cosmos-predict2.5.md) | World Model | Inference, Trainval | Megatron |
| [`FastBEV_trainval.md`](../../tutorials/FastBEV_trainval.md) | [`FastBEV`](../models/FastBEV.md) | Autonomous Driving | Trainval | MMCV |
| [`llama2_70b_infer.md`](../../tutorials/llama2_70b_infer.md) | [`Llama2-70B`](../models/Llama2-70B.md) | LLM/VLM/VLA | Inference | Not explicitly extracted |
| [`LLaMA_infer.md`](../../tutorials/LLaMA_infer.md) | [`LLaMA`](../models/LLaMA.md) | LLM/VLM/VLA | Inference | TensorRT, vLLM |
| [`Pi_0_trainval.md`](../../tutorials/Pi_0_trainval.md) | [`Pi0`](../models/Pi0.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Lerobot, wandb |
| [`qwen2.5_infer.md`](../../tutorials/qwen2.5_infer.md) | [`Qwen2.5`](../models/Qwen2.5.md) | LLM/VLM/VLA | Inference | TensorRT, vLLM |
| [`qwen2.5vl_r1_trainval.md`](../../tutorials/qwen2.5vl_r1_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval | Deepspeed, flash_attn |
| [`qwen3_235b_a22b_thinking_2507_infer.md`](../../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) | [`Qwen3-235B-A22B-Thinking-2507`](../models/Qwen3-235B-A22B-Thinking-2507.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval | SGLang |
| [`qwen3_30b_a3b_pretrain.md`](../../tutorials/qwen3_30b_a3b_pretrain.md) | [`Qwen3-30B-A3B`](../models/Qwen3-30B-A3B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Megatron, wandb, XMegatron |
| [`qwen3_8b_megatron_trainval.md`](../../tutorials/qwen3_8b_megatron_trainval.md) | [`Qwen3-8B`](../models/Qwen3-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Megatron, wandb |
| [`qwen3_llamafactory_trainval.md`](../../tutorials/qwen3_llamafactory_trainval.md) | [`Qwen3 (LlamaFactory)`](../models/Qwen3-LlamaFactory.md) | LLM/VLM/VLA | Trainval, SFT | Deepspeed, flash_attn, LlamaFactory, wandb |
| [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, MS-Swift, wandb |
| [`regnet_trainval.md`](../../tutorials/regnet_trainval.md) | [`RegNet`](../models/RegNet.md) | Vision/OCR | Trainval | Not explicitly extracted |
| [`vLLM_infer.md`](../../tutorials/vLLM_infer.md) | [`vLLM`](../models/vLLM.md) | General | Inference, Pretrain, Trainval | vLLM, xvLLM |
| [`xav_vLLM.md`](../../tutorials/xav_vLLM.md) | [`xav-vLLM`](../models/xav-vLLM.md) | General | Inference, Benchmark | vLLM, xav-vLLM |

## Provenance rules

- Treat this page as a derived index over linked tutorials, not as an independent source of benchmark truth.
- Preserve placeholders and verify exact commands in the source tutorial before reuse.
