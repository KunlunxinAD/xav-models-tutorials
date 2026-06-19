# LLM / VLM Training

## Summary

LLM, VLM, VLA, SFT, LoRA, GRPO, and pretraining tutorials.

## Source-backed entries

| Tutorial | Model / topic | Domain | Workflow | Frameworks / backends |
| --- | --- | --- | --- | --- |
| [`Bunny_trainval.md`](../../tutorials/Bunny_trainval.md) | [`Bunny`](../models/Bunny.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Not explicitly extracted |
| [`GR00T-Dreams_trainval.md`](../../tutorials/GR00T-Dreams_trainval.md) | [`GR00T-Dreams`](../models/GR00T-Dreams.md) | LLM/VLM/VLA | Trainval, SFT | diffusers, transformers, accelerate, tensorboard |
| [`Internvl3_8b_trainval.md`](../../tutorials/Internvl3_8b_trainval.md) | [`InternVL3-8B`](../models/InternVL3-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory |
| [`Isaac-GR00T-N1.7_trainval.md`](../../tutorials/Isaac-GR00T-N1.7_trainval.md) | [`Isaac-GR00T-N1.7`](../models/Isaac-GR00T-N1.7.md) | LLM/VLM/VLA | Trainval, SFT, Inference | Lerobot, Deepspeed, flash_attn, transformers, peft, wandb |
| [`openvla_trainval.md`](../../tutorials/openvla_trainval.md) | [`OpenVLA`](../models/OpenVLA.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | flash_attn, wandb |
| [`Pi_0_trainval.md`](../../tutorials/Pi_0_trainval.md) | [`Pi0`](../models/Pi0.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Lerobot, wandb |
| [`qwen2.5_trainval.md`](../../tutorials/qwen2.5_trainval.md) | [`Qwen2.5`](../models/Qwen2.5.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, LlamaFactory |
| [`qwen2.5vl_3b_trainval.md`](../../tutorials/qwen2.5vl_3b_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory |
| [`qwen2.5vl_r1_trainval.md`](../../tutorials/qwen2.5vl_r1_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval | Deepspeed, flash_attn |
| [`qwen2.5vl_trainval.md`](../../tutorials/qwen2.5vl_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory |
| [`qwen2_7b_trainval.md`](../../tutorials/qwen2_7b_trainval.md) | [`Qwen2-7B`](../models/Qwen2-7B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, LlamaFactory |
| [`qwen2vl_7b_trainval.md`](../../tutorials/qwen2vl_7b_trainval.md) | [`Qwen2-VL-7B`](../models/Qwen2-VL-7B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, LlamaFactory |
| [`qwen3_30b_a3b_pretrain.md`](../../tutorials/qwen3_30b_a3b_pretrain.md) | [`Qwen3-30B-A3B`](../models/Qwen3-30B-A3B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Megatron, wandb, XMegatron |
| [`qwen3_8b_megatron_trainval.md`](../../tutorials/qwen3_8b_megatron_trainval.md) | [`Qwen3-8B`](../models/Qwen3-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Megatron, wandb |
| [`qwen3_8b_xmegatron_trainval.md`](../../tutorials/qwen3_8b_xmegatron_trainval.md) | [`Qwen3-8B`](../models/Qwen3-8B.md) | LLM/VLM/VLA | Pretrain, Trainval | Megatron, wandb, XMegatron |
| [`qwen3_llamafactory_trainval.md`](../../tutorials/qwen3_llamafactory_trainval.md) | [`Qwen3 (LlamaFactory)`](../models/Qwen3-LlamaFactory.md) | LLM/VLM/VLA | Trainval, SFT | Deepspeed, flash_attn, LlamaFactory, wandb |
| [`qwen3vl_8b_grpo_verl_trainval.md`](../../tutorials/qwen3vl_8b_grpo_verl_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, GRPO | verl |
| [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, MS-Swift, wandb |

## Provenance rules

- Treat this page as a derived index over linked tutorials, not as an independent source of benchmark truth.
- Preserve placeholders and verify exact commands in the source tutorial before reuse.
