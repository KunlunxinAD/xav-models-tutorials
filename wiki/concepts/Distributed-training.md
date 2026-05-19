# Distributed Training

## Summary

Multi-card and distributed training patterns, including visible devices, NPROC settings, multi-node sections, and training launchers when stated.

## Source-backed entries

| Tutorial | Model / topic | Domain | Workflow | Frameworks / backends |
| --- | --- | --- | --- | --- |
| [`AlphaDrive_trainval.md`](../../tutorials/AlphaDrive_trainval.md) | [`AlphaDrive`](../models/AlphaDrive.md) | Autonomous Driving | Pretrain, Trainval, GRPO | Deepspeed, flash_attn, vLLM, wandb, xvLLM |
| [`bevfusion_trainval.md`](../../tutorials/bevfusion_trainval.md) | [`BEVFusion-MMDetection3D`](../models/BEVFusion-MMDetection3D.md) | Autonomous Driving | Pretrain, Trainval | MMDetection3D |
| [`Bunny_trainval.md`](../../tutorials/Bunny_trainval.md) | [`Bunny`](../models/Bunny.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Not explicitly extracted |
| [`CenterPoint.md`](../../tutorials/CenterPoint.md) | [`CenterPoint`](../models/CenterPoint.md) | Autonomous Driving | Trainval | Not explicitly extracted |
| [`cosmos_predict2.5_trainval.md`](../../tutorials/cosmos_predict2.5_trainval.md) | [`cosmos-predict2.5`](../models/cosmos-predict2.5.md) | World Model | Inference, Trainval | Megatron |
| [`DiffusionDrive_trainval.md`](../../tutorials/DiffusionDrive_trainval.md) | [`DiffusionDrive`](../models/DiffusionDrive.md) | World Model | Trainval | Navsim |
| [`DINO_trainval.md`](../../tutorials/DINO_trainval.md) | [`DINO`](../models/DINO.md) | Vision/OCR | Trainval | Not explicitly extracted |
| [`Far3D_trainval.md`](../../tutorials/Far3D_trainval.md) | [`Far3D`](../models/Far3D.md) | Autonomous Driving | Trainval | Not explicitly extracted |
| [`Internvl3_8b_trainval.md`](../../tutorials/Internvl3_8b_trainval.md) | [`InternVL3-8B`](../models/InternVL3-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory |
| [`LLaVA_pretrain_trainval.md`](../../tutorials/LLaVA_pretrain_trainval.md) | [`LLaVA`](../models/LLaVA.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval | flash_attn |
| [`LLaVA_trainval.md`](../../tutorials/LLaVA_trainval.md) | [`LLaVA`](../models/LLaVA.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval, SFT | flash_attn |
| [`maptrv2_trainval.md`](../../tutorials/maptrv2_trainval.md) | [`MapTRv2`](../models/MapTRv2.md) | Autonomous Driving | Pretrain, Trainval | Not explicitly extracted |
| [`mapvr_trainval.md`](../../tutorials/mapvr_trainval.md) | [`MapVR`](../models/MapVR.md) | Autonomous Driving | Pretrain, Trainval | Not explicitly extracted |
| [`mask2former_trainval.md`](../../tutorials/mask2former_trainval.md) | [`Mask2former`](../models/Mask2former.md) | Vision/OCR | Trainval | Detectron2 |
| [`openvla_trainval.md`](../../tutorials/openvla_trainval.md) | [`OpenVLA`](../models/OpenVLA.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | flash_attn, wandb |
| [`panoocc_trainval.md`](../../tutorials/panoocc_trainval.md) | [`PanoOcc`](../models/PanoOcc.md) | Autonomous Driving | Pretrain, Trainval | Not explicitly extracted |
| [`petrv2_trainval.md`](../../tutorials/petrv2_trainval.md) | [`PETRv2`](../models/PETRv2.md) | Autonomous Driving | Pretrain, Trainval | MMDetection3D |
| [`Pi_0_trainval.md`](../../tutorials/Pi_0_trainval.md) | [`Pi0`](../models/Pi0.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Lerobot, wandb |
| [`QCNet_trainval.md`](../../tutorials/QCNet_trainval.md) | [`QCNet`](../models/QCNet.md) | Autonomous Driving | Trainval | Not explicitly extracted |
| [`qwen2.5_trainval.md`](../../tutorials/qwen2.5_trainval.md) | [`Qwen2.5`](../models/Qwen2.5.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, LlamaFactory |
| [`qwen2.5vl_3b_trainval.md`](../../tutorials/qwen2.5vl_3b_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory |
| [`qwen2.5vl_infer.md`](../../tutorials/qwen2.5vl_infer.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Inference | TensorRT |
| [`qwen2.5vl_r1_trainval.md`](../../tutorials/qwen2.5vl_r1_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval | Deepspeed, flash_attn |
| [`qwen2.5vl_trainval.md`](../../tutorials/qwen2.5vl_trainval.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory |
| [`qwen2_7b_trainval.md`](../../tutorials/qwen2_7b_trainval.md) | [`Qwen2-7B`](../models/Qwen2-7B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, LlamaFactory |
| [`qwen2vl_7b_trainval.md`](../../tutorials/qwen2vl_7b_trainval.md) | [`Qwen2-VL-7B`](../models/Qwen2-VL-7B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, LlamaFactory |
| [`qwen3_235b_a22b_thinking_2507_infer.md`](../../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) | [`Qwen3-235B-A22B-Thinking-2507`](../models/Qwen3-235B-A22B-Thinking-2507.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval | SGLang |
| [`qwen3_30b_a3b_pretrain.md`](../../tutorials/qwen3_30b_a3b_pretrain.md) | [`Qwen3-30B-A3B`](../models/Qwen3-30B-A3B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Megatron, wandb, XMegatron |
| [`qwen3_8b_megatron_trainval.md`](../../tutorials/qwen3_8b_megatron_trainval.md) | [`Qwen3-8B`](../models/Qwen3-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Megatron, wandb |
| [`qwen3_8b_xmegatron_trainval.md`](../../tutorials/qwen3_8b_xmegatron_trainval.md) | [`Qwen3-8B`](../models/Qwen3-8B.md) | LLM/VLM/VLA | Pretrain, Trainval | Megatron, wandb, XMegatron |
| [`qwen3_llamafactory_trainval.md`](../../tutorials/qwen3_llamafactory_trainval.md) | [`Qwen3 (LlamaFactory)`](../models/Qwen3-LlamaFactory.md) | LLM/VLM/VLA | Trainval, SFT | Deepspeed, flash_attn, LlamaFactory, wandb |
| [`qwen3_omni_30b_a3b_infer.md`](../../tutorials/qwen3_omni_30b_a3b_infer.md) | [`Qwen3-Omni-30B-A3B`](../models/Qwen3-Omni-30B-A3B.md) | LLM/VLM/VLA | Inference, Benchmark | vLLM, xvLLM |
| [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) | [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, MS-Swift, wandb |
| [`recogdrive_trainval.md`](../../tutorials/recogdrive_trainval.md) | [`recogdrive`](../models/recogdrive.md) | Autonomous Driving | Pretrain, Trainval | flash_attn |
| [`regnet_trainval.md`](../../tutorials/regnet_trainval.md) | [`RegNet`](../models/RegNet.md) | Vision/OCR | Trainval | Not explicitly extracted |
| [`sparse4d_trainval.md`](../../tutorials/sparse4d_trainval.md) | [`Sparse4D`](../models/Sparse4D.md) | Autonomous Driving | Pretrain, Trainval | MMCV |
| [`SparseDrive_trainval.md`](../../tutorials/SparseDrive_trainval.md) | [`SparseDrive`](../models/SparseDrive.md) | Autonomous Driving | Trainval | MMCV |
| [`StateTransformer_trainval.md`](../../tutorials/StateTransformer_trainval.md) | [`StateTransformer`](../models/StateTransformer.md) | General | Trainval | Not explicitly extracted |
| [`StreamPETR_trainval.md`](../../tutorials/StreamPETR_trainval.md) | [`StreamPETR`](../models/StreamPETR.md) | Autonomous Driving | Pretrain, Trainval | flash_attn, MMCV, MMDetection3D |
| [`TRL_AlphaDrive_trainval.md`](../../tutorials/TRL_AlphaDrive_trainval.md) | [`AlphaDrive`](../models/AlphaDrive.md) | Autonomous Driving | Pretrain, Trainval, SFT, GRPO | Deepspeed, flash_attn, vLLM, xvLLM |
| [`UniAD_trainval.md`](../../tutorials/UniAD_trainval.md) | [`UniAD`](../models/UniAD.md) | Autonomous Driving | Trainval | MMCV |
| [`VAD_trainval.md`](../../tutorials/VAD_trainval.md) | [`VAD`](../models/VAD.md) | Autonomous Driving | Trainval | Not explicitly extracted |
| [`VIT_trainval.md`](../../tutorials/VIT_trainval.md) | [`VIT`](../models/VIT.md) | Vision/OCR | Trainval | Not explicitly extracted |
| [`vLLM_infer.md`](../../tutorials/vLLM_infer.md) | [`vLLM`](../models/vLLM.md) | General | Inference, Pretrain, Trainval | vLLM, xvLLM |
| [`xvllm_general_infer.md`](../../tutorials/xvllm_general_infer.md) | [`xvLLM`](../models/xvLLM.md) | LLM/VLM/VLA | Inference | vLLM, xav-vLLM, xvLLM |
| [`Yolo_inference.md`](../../tutorials/Yolo_inference.md) | [`Yolo`](../models/Yolo.md) | Vision/OCR | Inference | Not explicitly extracted |

## Provenance rules

- Treat this page as a derived index over linked tutorials, not as an independent source of benchmark truth.
- Preserve placeholders and verify exact commands in the source tutorial before reuse.
