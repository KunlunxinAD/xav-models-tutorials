# LLM Wiki Index

This file is the derived navigation layer for the XAV Open Model Tutorials repository. Source tutorials remain in [`tutorials/`](../tutorials/); this index links to them without moving or renaming files.

## How to use this wiki

- For support status and recent additions, start with [`README.md`](../README.md).
- For exact commands, environment variables, paths, and placeholders, read the linked source tutorial.
- For maintenance history and known issues, read [`wiki/log.md`](log.md).
- For update rules, read [`CLAUDE.md`](../CLAUDE.md).

## Knowledge layers

| Layer | Files | Purpose |
| --- | --- | --- |
| Source tutorials | [`tutorials/*.md`](../tutorials/) | Original procedural guides for training, evaluation, pretraining, and inference. |
| Human entry | [`README.md`](../README.md) | Project introduction, changelog, and model support table. |
| LLM entry | [`llms.txt`](../llms.txt) | Compact instructions and entry points for LLMs and agents. |
| Derived wiki | `wiki/index.md` | Cross-linked navigation over the source tutorials. |
| Maintenance log | [`wiki/log.md`](log.md) | Append-only record of wiki changes, resolved issues, and open questions. |

## Model support matrix from README

The following table mirrors the support metadata stated in `README.md`. When a tutorial contains more specific details, prefer the linked tutorial for commands and setup.

| Type | Model | Task | Precision | Devices | Framework / backend | Source |
| --- | --- | --- | --- | --- | --- | --- |
| Basic Models | VisionTransformer | Pretrain | FP32 | 1 x 8 | - | [`VIT_trainval.md`](../tutorials/VIT_trainval.md) |
| Basic Models | RegNet | Pretrain | FP32 | 1 x 8 | - | [`regnet_trainval.md`](../tutorials/regnet_trainval.md) |
| Basic Models | PaddleOCR_v5 | Pretrain | FP32 | 1 x 8 | PaddlePaddle | [`PaddleOCR_trainval.md`](../tutorials/PaddleOCR_trainval.md) |
| Basic Models | Yolo11 | Inference | FP32 | 1 x 8 | - | [`Yolo_inference.md`](../tutorials/Yolo_inference.md) |
| E2E AD Models | BEVDet | Pretrain | FP32 | 1 x 8 | MMCV | [`bevdet_trainval.md`](../tutorials/bevdet_trainval.md) |
| E2E AD Models | BEVFormer | Pretrain | FP32 | 1 x 8 | MMCV | [`bevformer_trainval.md`](../tutorials/bevformer_trainval.md) |
| E2E AD Models | PointPillar | Pretrain | FP32 | 1 x 8 | MMCV | [`PointPillar_trainval.md`](../tutorials/PointPillar_trainval.md) |
| E2E AD Models | PETR | Pretrain | FP32 | 1 x 8 | MMCV | [`PETR_trainval.md`](../tutorials/PETR_trainval.md) |
| E2E AD Models | FastBEV | Pretrain | FP32 | 1 x 8 | MMCV | [`FastBEV_trainval.md`](../tutorials/FastBEV_trainval.md) |
| E2E AD Models | MaskRCNN | Pretrain | FP32 | 1 x 8 | Detectron2 | [`MaskRCNN_trainval.md`](../tutorials/MaskRCNN_trainval.md) |
| E2E AD Models | LaneSegNet | Pretrain | FP32 | 1 x 8 | MMCV | [`lanesegnet_trainval.md`](../tutorials/lanesegnet_trainval.md) |
| E2E AD Models | Maptrv2 | Pretrain | FP32/FP16 | 1 x 8 | MMCV | [`maptrv2_trainval.md`](../tutorials/maptrv2_trainval.md) |
| E2E AD Models | Sparse4D | Pretrain | FP32 | 1 x 8 | MMCV | [`sparse4d_trainval.md`](../tutorials/sparse4d_trainval.md) |
| E2E AD Models | StreamPETR | Pretrain | FP32 | 1 x 8 | MMCV | [`StreamPETR_trainval.md`](../tutorials/StreamPETR_trainval.md) |
| E2E AD Models | BEVFusion | Pretrain | FP32/FP16 | 1 x 8 | MMCV | [`bevfusion_trainval.md`](../tutorials/bevfusion_trainval.md) |
| E2E AD Models | Far3D | Pretrain | FP16/FP32 | 1 x 8 | MMCV | [`Far3D_trainval.md`](../tutorials/Far3D_trainval.md) |
| E2E AD Models | GameFormer-Planner | Pretrain | FP32 | 1 x 8 | - | [`GameFormer-Planner_trainval.md`](../tutorials/GameFormer-Planner_trainval.md) |
| E2E AD Models | QCNet | Pretrain | FP32 | 1 x 8 | - | [`QCNet_trainval.md`](../tutorials/QCNet_trainval.md) |
| E2E AD Models | UniAD | Pretrain | FP32 | 1 x 8 | MMCV | [`UniAD_trainval.md`](../tutorials/UniAD_trainval.md) |
| E2E AD Models | VAD | Pretrain | FP32/FP16 | 1 x 8 | MMCV | [`VAD_trainval.md`](../tutorials/VAD_trainval.md) |
| E2E AD Models | SparseDrive | Pretrain | FP32/FP16 | 1 x 8 | MMCV | [`SparseDrive_trainval.md`](../tutorials/SparseDrive_trainval.md) |
| E2E AD Models | DiffusionDrive | Pretrain | FP32 | 1 x 8 | Navsim | [`DiffusionDrive_trainval.md`](../tutorials/DiffusionDrive_trainval.md) |
| VLM/VLA | Qwen2.5-VL | SFT/LoRA | FP16/BF16 | 1 x 8 | LLamaFactory | [`qwen2.5vl_3b_trainval.md`](../tutorials/qwen2.5vl_3b_trainval.md) |
| VLM/VLA | LLaVA | Pretrain/SFT | FP16/BF16 | 1 x 8 | - | [`LLaVA_trainval.md`](../tutorials/LLaVA_trainval.md) |
| VLM/VLA | Pi0 | Pretrain/SFT | FP16/BF16 | 1 x 8 | Lerobot | [`Pi_0_trainval.md`](../tutorials/Pi_0_trainval.md) |
| Omni | Qwen3-Omni-30B-A3B | Inference | FP16 | 1 x 8 | vLLM-Omni | [`qwen3_omni_30b_a3b_infer.md`](../tutorials/qwen3_omni_30b_a3b_infer.md) |
| World Model | DriveDreamer | Pretrain/SFT | FP16/BF16 | 1 x 8 | - | [`DriveDreamer_trainval.md`](../tutorials/DriveDreamer_trainval.md) |
| World Model | cosmos-predict2.5 | SFT | BF16 | 1 x 8 | - | [`cosmos_predict2.5_trainval.md`](../tutorials/cosmos_predict2.5_trainval.md) |
| World Model | cosmos-transfer2.5 | SFT | BF16 | 1 x 8 | - | [`cosmos_transfer2.5_trainval.md`](../tutorials/cosmos_transfer2.5_trainval.md) |
| LLM | Qwen2.5-7B | SFT/LoRA | FP16/BF16 | 1 x 8 | LLamaFactory | [`qwen2.5_trainval.md`](../tutorials/qwen2.5_trainval.md) |
| LLM | Qwen3-8B | Pretrain | BF16 | 1 x 8 | Megatron | [`qwen3_8b_xmegatron_trainval.md`](../tutorials/qwen3_8b_xmegatron_trainval.md) |
| LLM | Qwen3-4B | SFT/LoRA | FP16/BF16 | 1 x 8 | LLamaFactory | [`qwen3_llamafactory_trainval.md`](../tutorials/qwen3_llamafactory_trainval.md) |
| LLM | Qwen3-30B-A3B | Pretrain/SFT/LoRA | FP16/BF16 | 1 x 8 | Megatron/LLamaFactory | [`qwen3_llamafactory_trainval.md`](../tutorials/qwen3_llamafactory_trainval.md), [`qwen3_30b_a3b_pretrain.md`](../tutorials/qwen3_30b_a3b_pretrain.md) |
| LLM | Qwen3-235B-A22B-Thinking-2507 | Inference | FP16 | 1 x 8 | xSGL | [`qwen3_235b_a22b_thinking_2507_infer.md`](../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) |

## Derived wiki pages

### Source summaries

- [`qwen3vl-8b-swift-trainval.md`](sources/qwen3vl-8b-swift-trainval.md) — source summary for the Qwen3-VL-8B MS-Swift LoRA SFT tutorial.

### Model pages

- [`Qwen3-VL-8B.md`](models/Qwen3-VL-8B.md) — Qwen3-VL-8B-Instruct LoRA SFT with MS-Swift.

### Concept pages

- [`MS-Swift.md`](concepts/MS-Swift.md) — MS-Swift usage in this repository.
- [`VLM-VLA.md`](concepts/VLM-VLA.md) — vision-language and vision-language-action tutorial grouping.
- [`Memory-pressure.md`](concepts/Memory-pressure.md) — memory-related controls and missing evidence.
- [`XPU-training-adaptation.md`](concepts/XPU-training-adaptation.md) — source-backed XPU training environment settings.

### Recipe pages

- [`qwen3-vl-8b-ms-swift-lora-sft.md`](recipes/qwen3-vl-8b-ms-swift-lora-sft.md) — reusable Qwen3-VL-8B MS-Swift LoRA SFT workflow.

## Task-oriented entry points

### Inference guides

- [`LLaMA_infer.md`](../tutorials/LLaMA_infer.md) — LLaMA inference.
- [`Yolo_inference.md`](../tutorials/Yolo_inference.md) — Yolo inference.
- [`llama2_70b_infer.md`](../tutorials/llama2_70b_infer.md) — Llama2-70B inference.
- [`qwen2.5_infer.md`](../tutorials/qwen2.5_infer.md) — Qwen2.5 inference.
- [`qwen2.5vl_infer.md`](../tutorials/qwen2.5vl_infer.md) — Qwen2.5-VL inference.
- [`qwen3_235b_a22b_thinking_2507_infer.md`](../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) — Qwen3-235B-A22B-Thinking-2507 inference.
- [`qwen3_omni_30b_a3b_infer.md`](../tutorials/qwen3_omni_30b_a3b_infer.md) — Qwen3-Omni-30B-A3B inference.
- [`vLLM_infer.md`](../tutorials/vLLM_infer.md) — vLLM inference.
- [`xav_vLLM.md`](../tutorials/xav_vLLM.md) — xav-vLLM.
- [`xvllm_general_infer.md`](../tutorials/xvllm_general_infer.md) — xvLLM general inference.

### Pretraining and training/evaluation guides

Most files ending in `_trainval.md` or `_pretrain.md` are training, evaluation, SFT, LoRA, or pretraining guides. Use the source tutorial title and README support table before assuming the exact task.

- [`qwen3vl_8b_swift_trainval.md`](../tutorials/qwen3vl_8b_swift_trainval.md) — Qwen3-VL-8B-Instruct MS-Swift LoRA SFT; see [`Qwen3-VL-8B`](models/Qwen3-VL-8B.md) and the [`MS-Swift LoRA SFT recipe`](recipes/qwen3-vl-8b-ms-swift-lora-sft.md).

## All source tutorials

| Tutorial | Title |
| --- | --- |
| [`AlphaDrive_trainval.md`](../tutorials/AlphaDrive_trainval.md) | AlphaDrive Trainval Guide |
| [`Bunny_trainval.md`](../tutorials/Bunny_trainval.md) | Bunny Trainval Guide |
| [`CenterPoint.md`](../tutorials/CenterPoint.md) | CenterPoint Trainval Guide |
| [`DINO_trainval.md`](../tutorials/DINO_trainval.md) | DINO |
| [`DiffusionDrive_trainval.md`](../tutorials/DiffusionDrive_trainval.md) | DiffusionDrive |
| [`DriveDreamer_trainval.md`](../tutorials/DriveDreamer_trainval.md) | DriveDreamer Trainval Guide |
| [`Far3D_trainval.md`](../tutorials/Far3D_trainval.md) | Far3D |
| [`FastBEV_trainval.md`](../tutorials/FastBEV_trainval.md) | FastBEV |
| [`GameFormer-Planner_trainval.md`](../tutorials/GameFormer-Planner_trainval.md) | GameFormer-Planner |
| [`Internvl3_8b_trainval.md`](../tutorials/Internvl3_8b_trainval.md) | InternVL3-8B Trainval Guide |
| [`LLaMA_infer.md`](../tutorials/LLaMA_infer.md) | LLaMA Inference Guide |
| [`LLaVA_pretrain_trainval.md`](../tutorials/LLaVA_pretrain_trainval.md) | LLaVA |
| [`LLaVA_trainval.md`](../tutorials/LLaVA_trainval.md) | LLaVA |
| [`MaskRCNN_trainval.md`](../tutorials/MaskRCNN_trainval.md) | MaskRCNN |
| [`PETR_trainval.md`](../tutorials/PETR_trainval.md) | PETR Trainval Guide |
| [`PaddleOCR_trainval.md`](../tutorials/PaddleOCR_trainval.md) | PaddleOCR_v5 |
| [`Pi_0_trainval.md`](../tutorials/Pi_0_trainval.md) | Pi_0 |
| [`PointPillar_trainval.md`](../tutorials/PointPillar_trainval.md) | PointPillar |
| [`QCNet_trainval.md`](../tutorials/QCNet_trainval.md) | QCNet |
| [`SparseDrive_trainval.md`](../tutorials/SparseDrive_trainval.md) | SparseDrive |
| [`StateTransformer_trainval.md`](../tutorials/StateTransformer_trainval.md) | StateTransformer |
| [`StreamPETR_trainval.md`](../tutorials/StreamPETR_trainval.md) | StreamPETR |
| [`TRL_AlphaDrive_trainval.md`](../tutorials/TRL_AlphaDrive_trainval.md) | AlphaDrive Trainval Guide |
| [`UniAD_trainval.md`](../tutorials/UniAD_trainval.md) | UniAD |
| [`VAD_trainval.md`](../tutorials/VAD_trainval.md) | VAD |
| [`VIT_trainval.md`](../tutorials/VIT_trainval.md) | VIT |
| [`Yolo_inference.md`](../tutorials/Yolo_inference.md) | Yolo Inference Guide |
| [`bevdet_trainval.md`](../tutorials/bevdet_trainval.md) | BEVDet |
| [`bevformer_trainval.md`](../tutorials/bevformer_trainval.md) | BEVFormer |
| [`bevfusion_trainval.md`](../tutorials/bevfusion_trainval.md) | BEVFusion-MMDetection3D |
| [`cosmos_predict2.5_trainval.md`](../tutorials/cosmos_predict2.5_trainval.md) | cosmos-predict2.5 |
| [`cosmos_transfer2.5_trainval.md`](../tutorials/cosmos_transfer2.5_trainval.md) | cosmos-transfer2.5 |
| [`flashocc_trainval.md`](../tutorials/flashocc_trainval.md) | FlashOCC |
| [`lanesegnet_trainval.md`](../tutorials/lanesegnet_trainval.md) | LaneSegNet |
| [`llama2_70b_infer.md`](../tutorials/llama2_70b_infer.md) | Llama2-70B Inference Guide |
| [`maptrv2_trainval.md`](../tutorials/maptrv2_trainval.md) | MapTRv2 |
| [`mapvr_trainval.md`](../tutorials/mapvr_trainval.md) | MapVR |
| [`mask2former_trainval.md`](../tutorials/mask2former_trainval.md) | Mask2former |
| [`multipathpp_trainval.md`](../tutorials/multipathpp_trainval.md) | Multipath++ |
| [`openvla_trainval.md`](../tutorials/openvla_trainval.md) | OpenVLA Trainval Guide |
| [`panoocc_trainval.md`](../tutorials/panoocc_trainval.md) | PanoOcc |
| [`petrv2_trainval.md`](../tutorials/petrv2_trainval.md) | PETRv2 |
| [`qwen2.5_infer.md`](../tutorials/qwen2.5_infer.md) | Qwen2.5 Inference Guide |
| [`qwen2.5_trainval.md`](../tutorials/qwen2.5_trainval.md) | Qwen2.5 Trainval Guide |
| [`qwen2.5vl_3b_trainval.md`](../tutorials/qwen2.5vl_3b_trainval.md) | Qwen2.5-VL-3B Trainval Guide |
| [`qwen2.5vl_infer.md`](../tutorials/qwen2.5vl_infer.md) | Qwen2.5-VL Inference Guide |
| [`qwen2.5vl_r1_trainval.md`](../tutorials/qwen2.5vl_r1_trainval.md) | Qwen2.5-VL-R1 Trainval Guide |
| [`qwen2.5vl_trainval.md`](../tutorials/qwen2.5vl_trainval.md) | Qwen2.5-VL Trainval Guide |
| [`qwen2_7b_trainval.md`](../tutorials/qwen2_7b_trainval.md) | Qwen2-7B Trainval Guide |
| [`qwen2vl_7b_trainval.md`](../tutorials/qwen2vl_7b_trainval.md) | Qwen2-VL-7B |
| [`qwen3_235b_a22b_thinking_2507_infer.md`](../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) | Qwen3-235B-A22B-Thinking-2507 Inference Guide |
| [`qwen3_30b_a3b_pretrain.md`](../tutorials/qwen3_30b_a3b_pretrain.md) | Qwen3-30B-A3B Pretrain Guide |
| [`qwen3_8b_megatron_trainval.md`](../tutorials/qwen3_8b_megatron_trainval.md) | Qwen3-8B Megatron Trainval Guide |
| [`qwen3_8b_xmegatron_trainval.md`](../tutorials/qwen3_8b_xmegatron_trainval.md) | Qwen3-8B XMegatron Trainval Guide |
| [`qwen3_llamafactory_trainval.md`](../tutorials/qwen3_llamafactory_trainval.md) | Qwen3 Trainval Guide (LlamaFactory) |
| [`qwen3_omni_30b_a3b_infer.md`](../tutorials/qwen3_omni_30b_a3b_infer.md) | Qwen3-Omni-30B-A3B Inference Guide |
| [`qwen3vl_8b_grpo_verl_trainval.md`](../tutorials/qwen3vl_8b_grpo_verl_trainval.md) | Qwen3vl-8B grpo verl Trainval Guide |
| [`qwen3vl_8b_swift_trainval.md`](../tutorials/qwen3vl_8b_swift_trainval.md) | Qwen3-VL-8B MS-Swift Trainval Guide |
| [`recogdrive_trainval.md`](../tutorials/recogdrive_trainval.md) | recogdrive |
| [`regnet_trainval.md`](../tutorials/regnet_trainval.md) | RegNet Trainval Guide |
| [`sparse4d_trainval.md`](../tutorials/sparse4d_trainval.md) | Sparse4D |
| [`vLLM_infer.md`](../tutorials/vLLM_infer.md) | vLLM infer Guide |
| [`xav_vLLM.md`](../tutorials/xav_vLLM.md) | xav-vLLM |
| [`xvllm_general_infer.md`](../tutorials/xvllm_general_infer.md) | xvLLM General Inference Guide |
