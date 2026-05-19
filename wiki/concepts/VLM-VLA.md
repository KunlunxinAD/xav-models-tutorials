# VLM / VLA

## Summary

Vision-language and vision-language-action tutorials, including Qwen-VL, LLaVA, OpenVLA, Pi0, InternVL, Bunny, and related workflows.

## Source-backed entries

| Tutorial | Model / topic | Domain | Workflow | Frameworks / backends |
| --- | --- | --- | --- | --- |
| [`Bunny_trainval.md`](../../tutorials/Bunny_trainval.md) | [`Bunny`](../models/Bunny.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Not explicitly extracted |
| [`Internvl3_8b_trainval.md`](../../tutorials/Internvl3_8b_trainval.md) | [`InternVL3-8B`](../models/InternVL3-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory |
| [`LLaVA_pretrain_trainval.md`](../../tutorials/LLaVA_pretrain_trainval.md) | [`LLaVA`](../models/LLaVA.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval | flash_attn |
| [`LLaVA_trainval.md`](../../tutorials/LLaVA_trainval.md) | [`LLaVA`](../models/LLaVA.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval, SFT | flash_attn |
| [`openvla_trainval.md`](../../tutorials/openvla_trainval.md) | [`OpenVLA`](../models/OpenVLA.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | flash_attn, wandb |
| [`Pi_0_trainval.md`](../../tutorials/Pi_0_trainval.md) | [`Pi0`](../models/Pi0.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Lerobot, wandb |
| [`qwen2.5vl_3b_trainval.md`](../../tutorials/qwen2.5vl_3b_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory |
| [`qwen2.5vl_infer.md`](../../tutorials/qwen2.5vl_infer.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Inference | TensorRT |
| [`qwen2.5vl_r1_trainval.md`](../../tutorials/qwen2.5vl_r1_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval | Deepspeed, flash_attn |
| [`qwen2.5vl_trainval.md`](../../tutorials/qwen2.5vl_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory |
| [`qwen2vl_7b_trainval.md`](../../tutorials/qwen2vl_7b_trainval.md) | [`Qwen2-VL-7B`](../models/Qwen2-VL-7B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, LlamaFactory |
| [`qwen3_omni_30b_a3b_infer.md`](../../tutorials/qwen3_omni_30b_a3b_infer.md) | [`Qwen3-Omni-30B-A3B`](../models/Qwen3-Omni-30B-A3B.md) | LLM/VLM/VLA | Inference, Benchmark | vLLM, xvLLM |
| [`qwen3vl_8b_grpo_verl_trainval.md`](../../tutorials/qwen3vl_8b_grpo_verl_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, GRPO | verl |
| [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, MS-Swift, wandb |
| [`xvllm_general_infer.md`](../../tutorials/xvllm_general_infer.md) | [`xvLLM`](../models/xvLLM.md) | LLM/VLM/VLA | Inference | vLLM, xav-vLLM, xvLLM |

## Provenance rules

- Treat this page as a derived index over linked tutorials, not as an independent source of benchmark truth.
- Preserve placeholders and verify exact commands in the source tutorial before reuse.
