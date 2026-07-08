# LLM Wiki Index

This file is the derived navigation layer for the XAV Open Model Tutorials repository. Source tutorials remain in [`tutorials/`](../tutorials/); this index links to them without moving or renaming files.

## How to use this wiki

- For support status and recent additions, start with [`README.md`](../README.md).
- For exact commands, environment variables, paths, and placeholders, read the linked source tutorial.
- For source-level summaries, start with the `Source summaries` section below.
- For maintenance history and known issues, read [`wiki/log.md`](log.md).
- For update rules, read [`CLAUDE.md`](../CLAUDE.md).

## Knowledge layers

| Layer | Files | Purpose |
| --- | --- | --- |
| Source tutorials | [`tutorials/*.md`](../tutorials/) | Original procedural guides for training, evaluation, pretraining, and inference. |
| Source summaries | [`wiki/sources/*.md`](sources/) | One derived summary per tutorial with extracted facts and open questions. |
| Model pages | [`wiki/models/*.md`](models/) | Per-model/topic support matrices and related links. |
| Concept pages | [`wiki/concepts/*.md`](concepts/) | Aggregated concepts and cross-tutorial patterns. |
| Recipe pages | [`wiki/recipes/*.md`](recipes/) | Reusable workflow summaries backed by source tutorials. |

## Source summaries

| Source summary | Tutorial | Domain | Workflow | Model / topic |
| --- | --- | --- | --- | --- |
| [`alphadrive_trainval.md`](sources/alphadrive_trainval.md) | [`AlphaDrive_trainval.md`](../tutorials/AlphaDrive_trainval.md) | Autonomous Driving | Pretrain, Trainval, GRPO | [`AlphaDrive`](models/AlphaDrive.md) |
| [`bunny_trainval.md`](sources/bunny_trainval.md) | [`Bunny_trainval.md`](../tutorials/Bunny_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | [`Bunny`](models/Bunny.md) |
| [`centerpoint.md`](sources/centerpoint.md) | [`CenterPoint.md`](../tutorials/CenterPoint.md) | Autonomous Driving | Trainval | [`CenterPoint`](models/CenterPoint.md) |
| [`dino_trainval.md`](sources/dino_trainval.md) | [`DINO_trainval.md`](../tutorials/DINO_trainval.md) | Vision/OCR | Trainval | [`DINO`](models/DINO.md) |
| [`diffusiondrive_trainval.md`](sources/diffusiondrive_trainval.md) | [`DiffusionDrive_trainval.md`](../tutorials/DiffusionDrive_trainval.md) | World Model | Trainval | [`DiffusionDrive`](models/DiffusionDrive.md) |
| [`drivedreamer_trainval.md`](sources/drivedreamer_trainval.md) | [`DriveDreamer_trainval.md`](../tutorials/DriveDreamer_trainval.md) | World Model | Trainval | [`DriveDreamer`](models/DriveDreamer.md) |
| [`far3d_trainval.md`](sources/far3d_trainval.md) | [`Far3D_trainval.md`](../tutorials/Far3D_trainval.md) | Autonomous Driving | Trainval | [`Far3D`](models/Far3D.md) |
| [`fastbev_trainval.md`](sources/fastbev_trainval.md) | [`FastBEV_trainval.md`](../tutorials/FastBEV_trainval.md) | Autonomous Driving | Trainval | [`FastBEV`](models/FastBEV.md) |
| [`gameformer-planner_trainval.md`](sources/gameformer-planner_trainval.md) | [`GameFormer-Planner_trainval.md`](../tutorials/GameFormer-Planner_trainval.md) | Autonomous Driving | Trainval | [`GameFormer-Planner`](models/GameFormer-Planner.md) |
| [`groot-dreams_trainval.md`](sources/groot-dreams_trainval.md) | [`GR00T-Dreams_trainval.md`](../tutorials/GR00T-Dreams_trainval.md) | LLM/VLM/VLA | Trainval, SFT | [`GR00T-Dreams`](models/GR00T-Dreams.md) |
| [`internvl3_8b_trainval.md`](sources/internvl3_8b_trainval.md) | [`Internvl3_8b_trainval.md`](../tutorials/Internvl3_8b_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | [`InternVL3-8B`](models/InternVL3-8B.md) |
| [`isaac-groot-n1.7_trainval.md`](sources/isaac-groot-n1.7_trainval.md) | [`Isaac-GR00T-N1.7_trainval.md`](../tutorials/Isaac-GR00T-N1.7_trainval.md) | LLM/VLM/VLA | Trainval, SFT, Inference | [`Isaac-GR00T-N1.7`](models/Isaac-GR00T-N1.7.md) |
| [`llama_infer.md`](sources/llama_infer.md) | [`LLaMA_infer.md`](../tutorials/LLaMA_infer.md) | LLM/VLM/VLA | Inference | [`LLaMA`](models/LLaMA.md) |
| [`llava_pretrain_trainval.md`](sources/llava_pretrain_trainval.md) | [`LLaVA_pretrain_trainval.md`](../tutorials/LLaVA_pretrain_trainval.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval | [`LLaVA`](models/LLaVA.md) |
| [`llava_trainval.md`](sources/llava_trainval.md) | [`LLaVA_trainval.md`](../tutorials/LLaVA_trainval.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval, SFT | [`LLaVA`](models/LLaVA.md) |
| [`maskrcnn_trainval.md`](sources/maskrcnn_trainval.md) | [`MaskRCNN_trainval.md`](../tutorials/MaskRCNN_trainval.md) | Vision/OCR | Trainval | [`MaskRCNN`](models/MaskRCNN.md) |
| [`petr_trainval.md`](sources/petr_trainval.md) | [`PETR_trainval.md`](../tutorials/PETR_trainval.md) | Autonomous Driving | Trainval | [`PETR`](models/PETR.md) |
| [`paddleocr_trainval.md`](sources/paddleocr_trainval.md) | [`PaddleOCR_trainval.md`](../tutorials/PaddleOCR_trainval.md) | Vision/OCR | Inference, Pretrain, Trainval | [`PaddleOCR-v5`](models/PaddleOCR-v5.md) |
| [`pi_0_trainval.md`](sources/pi_0_trainval.md) | [`Pi_0_trainval.md`](../tutorials/Pi_0_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | [`Pi0`](models/Pi0.md) |
| [`pointpillar_trainval.md`](sources/pointpillar_trainval.md) | [`PointPillar_trainval.md`](../tutorials/PointPillar_trainval.md) | Autonomous Driving | Trainval | [`PointPillar`](models/PointPillar.md) |
| [`ptv3_trainval.md`](sources/ptv3_trainval.md) | [`Ptv3_Trainval_Guide.md`](../tutorials/Ptv3_Trainval_Guide.md) | Autonomous Driving | Trainval | [`PTv3`](models/PTv3.md) |
| [`qcnet_trainval.md`](sources/qcnet_trainval.md) | [`QCNet_trainval.md`](../tutorials/QCNet_trainval.md) | Autonomous Driving | Trainval | [`QCNet`](models/QCNet.md) |
| [`sparsedrive_trainval.md`](sources/sparsedrive_trainval.md) | [`SparseDrive_trainval.md`](../tutorials/SparseDrive_trainval.md) | Autonomous Driving | Trainval | [`SparseDrive`](models/SparseDrive.md) |
| [`statetransformer_trainval.md`](sources/statetransformer_trainval.md) | [`StateTransformer_trainval.md`](../tutorials/StateTransformer_trainval.md) | General | Trainval | [`StateTransformer`](models/StateTransformer.md) |
| [`streampetr_trainval.md`](sources/streampetr_trainval.md) | [`StreamPETR_trainval.md`](../tutorials/StreamPETR_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`StreamPETR`](models/StreamPETR.md) |
| [`trl_alphadrive_trainval.md`](sources/trl_alphadrive_trainval.md) | [`TRL_AlphaDrive_trainval.md`](../tutorials/TRL_AlphaDrive_trainval.md) | Autonomous Driving | Pretrain, Trainval, SFT, GRPO | [`AlphaDrive`](models/AlphaDrive.md) |
| [`uniad_trainval.md`](sources/uniad_trainval.md) | [`UniAD_trainval.md`](../tutorials/UniAD_trainval.md) | Autonomous Driving | Trainval | [`UniAD`](models/UniAD.md) |
| [`vad_trainval.md`](sources/vad_trainval.md) | [`VAD_trainval.md`](../tutorials/VAD_trainval.md) | Autonomous Driving | Trainval | [`VAD`](models/VAD.md) |
| [`vit_trainval.md`](sources/vit_trainval.md) | [`VIT_trainval.md`](../tutorials/VIT_trainval.md) | Vision/OCR | Trainval | [`VIT`](models/VIT.md) |
| [`yolo_inference.md`](sources/yolo_inference.md) | [`Yolo_inference.md`](../tutorials/Yolo_inference.md) | Vision/OCR | Inference | [`Yolo`](models/Yolo.md) |
| [`bevdet_trainval.md`](sources/bevdet_trainval.md) | [`bevdet_trainval.md`](../tutorials/bevdet_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`BEVDet`](models/BEVDet.md) |
| [`bevformer_trainval.md`](sources/bevformer_trainval.md) | [`bevformer_trainval.md`](../tutorials/bevformer_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`BEVFormer`](models/BEVFormer.md) |
| [`bevfusion_trainval.md`](sources/bevfusion_trainval.md) | [`bevfusion_trainval.md`](../tutorials/bevfusion_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`BEVFusion-MMDetection3D`](models/BEVFusion-MMDetection3D.md) |
| [`cosmos_predict2.5_trainval.md`](sources/cosmos_predict2.5_trainval.md) | [`cosmos_predict2.5_trainval.md`](../tutorials/cosmos_predict2.5_trainval.md) | World Model | Inference, Trainval | [`cosmos-predict2.5`](models/cosmos-predict2.5.md) |
| [`cosmos_transfer2.5_trainval.md`](sources/cosmos_transfer2.5_trainval.md) | [`cosmos_transfer2.5_trainval.md`](../tutorials/cosmos_transfer2.5_trainval.md) | World Model | Trainval | [`cosmos-transfer2.5`](models/cosmos-transfer2.5.md) |
| [`flashocc_trainval.md`](sources/flashocc_trainval.md) | [`flashocc_trainval.md`](../tutorials/flashocc_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`FlashOCC`](models/FlashOCC.md) |
| [`lanesegnet_trainval.md`](sources/lanesegnet_trainval.md) | [`lanesegnet_trainval.md`](../tutorials/lanesegnet_trainval.md) | Autonomous Driving | Trainval | [`LaneSegNet`](models/LaneSegNet.md) |
| [`llama2_70b_infer.md`](sources/llama2_70b_infer.md) | [`llama2_70b_infer.md`](../tutorials/llama2_70b_infer.md) | LLM/VLM/VLA | Inference | [`Llama2-70B`](models/Llama2-70B.md) |
| [`maptrv2_trainval.md`](sources/maptrv2_trainval.md) | [`maptrv2_trainval.md`](../tutorials/maptrv2_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`MapTRv2`](models/MapTRv2.md) |
| [`mapvr_trainval.md`](sources/mapvr_trainval.md) | [`mapvr_trainval.md`](../tutorials/mapvr_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`MapVR`](models/MapVR.md) |
| [`mask2former_trainval.md`](sources/mask2former_trainval.md) | [`mask2former_trainval.md`](../tutorials/mask2former_trainval.md) | Vision/OCR | Trainval | [`Mask2former`](models/Mask2former.md) |
| [`multipathpp_trainval.md`](sources/multipathpp_trainval.md) | [`multipathpp_trainval.md`](../tutorials/multipathpp_trainval.md) | Autonomous Driving | Trainval | [`Multipath++`](models/Multipath++.md) |
| [`openvla_trainval.md`](sources/openvla_trainval.md) | [`openvla_trainval.md`](../tutorials/openvla_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | [`OpenVLA`](models/OpenVLA.md) |
| [`panoocc_trainval.md`](sources/panoocc_trainval.md) | [`panoocc_trainval.md`](../tutorials/panoocc_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`PanoOcc`](models/PanoOcc.md) |
| [`petrv2_trainval.md`](sources/petrv2_trainval.md) | [`petrv2_trainval.md`](../tutorials/petrv2_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`PETRv2`](models/PETRv2.md) |
| [`qwen2.5_infer.md`](sources/qwen2.5_infer.md) | [`qwen2.5_infer.md`](../tutorials/qwen2.5_infer.md) | LLM/VLM/VLA | Inference | [`Qwen2.5`](models/Qwen2.5.md) |
| [`qwen2.5_trainval.md`](sources/qwen2.5_trainval.md) | [`qwen2.5_trainval.md`](../tutorials/qwen2.5_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | [`Qwen2.5`](models/Qwen2.5.md) |
| [`qwen2.5vl_3b_trainval.md`](sources/qwen2.5vl_3b_trainval.md) | [`qwen2.5vl_3b_trainval.md`](../tutorials/qwen2.5vl_3b_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | [`Qwen2.5-VL`](models/Qwen2.5-VL.md) |
| [`qwen2.5vl_infer.md`](sources/qwen2.5vl_infer.md) | [`qwen2.5vl_infer.md`](../tutorials/qwen2.5vl_infer.md) | LLM/VLM/VLA | Inference | [`Qwen2.5-VL`](models/Qwen2.5-VL.md) |
| [`qwen2.5vl_r1_trainval.md`](sources/qwen2.5vl_r1_trainval.md) | [`qwen2.5vl_r1_trainval.md`](../tutorials/qwen2.5vl_r1_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval | [`Qwen2.5-VL`](models/Qwen2.5-VL.md) |
| [`qwen2.5vl_trainval.md`](sources/qwen2.5vl_trainval.md) | [`qwen2.5vl_trainval.md`](../tutorials/qwen2.5vl_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | [`Qwen2.5-VL`](models/Qwen2.5-VL.md) |
| [`qwen2_7b_trainval.md`](sources/qwen2_7b_trainval.md) | [`qwen2_7b_trainval.md`](../tutorials/qwen2_7b_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | [`Qwen2-7B`](models/Qwen2-7B.md) |
| [`qwen2vl_7b_trainval.md`](sources/qwen2vl_7b_trainval.md) | [`qwen2vl_7b_trainval.md`](../tutorials/qwen2vl_7b_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | [`Qwen2-VL-7B`](models/Qwen2-VL-7B.md) |
| [`qwen3_235b_a22b_thinking_2507_infer.md`](sources/qwen3_235b_a22b_thinking_2507_infer.md) | [`qwen3_235b_a22b_thinking_2507_infer.md`](../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval | [`Qwen3-235B-A22B-Thinking-2507`](models/Qwen3-235B-A22B-Thinking-2507.md) |
| [`qwen3_30b_a3b_pretrain.md`](sources/qwen3_30b_a3b_pretrain.md) | [`qwen3_30b_a3b_pretrain.md`](../tutorials/qwen3_30b_a3b_pretrain.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | [`Qwen3-30B-A3B`](models/Qwen3-30B-A3B.md) |
| [`qwen3_8b_megatron_trainval.md`](sources/qwen3_8b_megatron_trainval.md) | [`qwen3_8b_megatron_trainval.md`](../tutorials/qwen3_8b_megatron_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | [`Qwen3-8B`](models/Qwen3-8B.md) |
| [`qwen3_8b_xmegatron_trainval.md`](sources/qwen3_8b_xmegatron_trainval.md) | [`qwen3_8b_xmegatron_trainval.md`](../tutorials/qwen3_8b_xmegatron_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval | [`Qwen3-8B`](models/Qwen3-8B.md) |
| [`qwen3_llamafactory_trainval.md`](sources/qwen3_llamafactory_trainval.md) | [`qwen3_llamafactory_trainval.md`](../tutorials/qwen3_llamafactory_trainval.md) | LLM/VLM/VLA | Trainval, SFT | [`Qwen3 (LlamaFactory)`](models/Qwen3-LlamaFactory.md) |
| [`qwen3_omni_30b_a3b_infer.md`](sources/qwen3_omni_30b_a3b_infer.md) | [`qwen3_omni_30b_a3b_infer.md`](../tutorials/qwen3_omni_30b_a3b_infer.md) | LLM/VLM/VLA | Inference, Benchmark | [`Qwen3-Omni-30B-A3B`](models/Qwen3-Omni-30B-A3B.md) |
| [`qwen3vl_8b_grpo_verl_trainval.md`](sources/qwen3vl_8b_grpo_verl_trainval.md) | [`qwen3vl_8b_grpo_verl_trainval.md`](../tutorials/qwen3vl_8b_grpo_verl_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, GRPO | [`Qwen3-VL-8B`](models/Qwen3-VL-8B.md) |
| [`qwen3vl-8b-swift-trainval.md`](sources/qwen3vl-8b-swift-trainval.md) | [`qwen3vl_8b_swift_trainval.md`](../tutorials/qwen3vl_8b_swift_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | [`Qwen3-VL-8B`](models/Qwen3-VL-8B.md) |
| [`recogdrive_trainval.md`](sources/recogdrive_trainval.md) | [`recogdrive_trainval.md`](../tutorials/recogdrive_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`recogdrive`](models/recogdrive.md) |
| [`regnet_trainval.md`](sources/regnet_trainval.md) | [`regnet_trainval.md`](../tutorials/regnet_trainval.md) | Vision/OCR | Trainval | [`RegNet`](models/RegNet.md) |
| [`sparse4d_trainval.md`](sources/sparse4d_trainval.md) | [`sparse4d_trainval.md`](../tutorials/sparse4d_trainval.md) | Autonomous Driving | Pretrain, Trainval | [`Sparse4D`](models/Sparse4D.md) |
| [`vllm_infer.md`](sources/vllm_infer.md) | [`vLLM_infer.md`](../tutorials/vLLM_infer.md) | General | Inference, Pretrain, Trainval | [`vLLM`](models/vLLM.md) |
| [`xav_vllm.md`](sources/xav_vllm.md) | [`xav_vLLM.md`](../tutorials/xav_vLLM.md) | General | Inference, Benchmark | [`xav-vLLM`](models/xav-vLLM.md) |
| [`xvllm_general_infer.md`](sources/xvllm_general_infer.md) | [`xvllm_general_infer.md`](../tutorials/xvllm_general_infer.md) | LLM/VLM/VLA | Inference | [`xvLLM`](models/xvLLM.md) |

## Model pages

- [`AlphaDrive.md`](models/AlphaDrive.md)
- [`BEVDet.md`](models/BEVDet.md)
- [`BEVFormer.md`](models/BEVFormer.md)
- [`BEVFusion-MMDetection3D.md`](models/BEVFusion-MMDetection3D.md)
- [`Bunny.md`](models/Bunny.md)
- [`CenterPoint.md`](models/CenterPoint.md)
- [`DINO.md`](models/DINO.md)
- [`DiffusionDrive.md`](models/DiffusionDrive.md)
- [`DriveDreamer.md`](models/DriveDreamer.md)
- [`Far3D.md`](models/Far3D.md)
- [`FastBEV.md`](models/FastBEV.md)
- [`FlashOCC.md`](models/FlashOCC.md)
- [`GameFormer-Planner.md`](models/GameFormer-Planner.md)
- [`GR00T-Dreams.md`](models/GR00T-Dreams.md)
- [`InternVL3-8B.md`](models/InternVL3-8B.md)
- [`Isaac-GR00T-N1.7.md`](models/Isaac-GR00T-N1.7.md)
- [`LLaMA.md`](models/LLaMA.md)
- [`LLaVA.md`](models/LLaVA.md)
- [`LaneSegNet.md`](models/LaneSegNet.md)
- [`Llama2-70B.md`](models/Llama2-70B.md)
- [`MapTRv2.md`](models/MapTRv2.md)
- [`MapVR.md`](models/MapVR.md)
- [`Mask2former.md`](models/Mask2former.md)
- [`MaskRCNN.md`](models/MaskRCNN.md)
- [`Multipath++.md`](models/Multipath++.md)
- [`OpenVLA.md`](models/OpenVLA.md)
- [`PETR.md`](models/PETR.md)
- [`PETRv2.md`](models/PETRv2.md)
- [`PaddleOCR-v5.md`](models/PaddleOCR-v5.md)
- [`PanoOcc.md`](models/PanoOcc.md)
- [`Pi0.md`](models/Pi0.md)
- [`PointPillar.md`](models/PointPillar.md)
- [`PTv3.md`](models/PTv3.md)
- [`QCNet.md`](models/QCNet.md)
- [`Qwen2-7B.md`](models/Qwen2-7B.md)
- [`Qwen2-VL-7B.md`](models/Qwen2-VL-7B.md)
- [`Qwen2.5-VL.md`](models/Qwen2.5-VL.md)
- [`Qwen2.5.md`](models/Qwen2.5.md)
- [`Qwen3-235B-A22B-Thinking-2507.md`](models/Qwen3-235B-A22B-Thinking-2507.md)
- [`Qwen3-30B-A3B.md`](models/Qwen3-30B-A3B.md)
- [`Qwen3-8B.md`](models/Qwen3-8B.md)
- [`Qwen3-LlamaFactory.md`](models/Qwen3-LlamaFactory.md)
- [`Qwen3-Omni-30B-A3B.md`](models/Qwen3-Omni-30B-A3B.md)
- [`Qwen3-VL-8B.md`](models/Qwen3-VL-8B.md)
- [`RegNet.md`](models/RegNet.md)
- [`Sparse4D.md`](models/Sparse4D.md)
- [`SparseDrive.md`](models/SparseDrive.md)
- [`StateTransformer.md`](models/StateTransformer.md)
- [`StreamPETR.md`](models/StreamPETR.md)
- [`UniAD.md`](models/UniAD.md)
- [`VAD.md`](models/VAD.md)
- [`VIT.md`](models/VIT.md)
- [`Yolo.md`](models/Yolo.md)
- [`cosmos-predict2.5.md`](models/cosmos-predict2.5.md)
- [`cosmos-transfer2.5.md`](models/cosmos-transfer2.5.md)
- [`recogdrive.md`](models/recogdrive.md)
- [`vLLM.md`](models/vLLM.md)
- [`xav-vLLM.md`](models/xav-vLLM.md)
- [`xvLLM.md`](models/xvLLM.md)

## Concept pages

- [`Autonomous-driving-trainval.md`](concepts/Autonomous-driving-trainval.md)
- [`Benchmark-and-evaluation.md`](concepts/Benchmark-and-evaluation.md)
- [`Container-and-XPU-runtime.md`](concepts/Container-and-XPU-runtime.md)
- [`Distributed-training.md`](concepts/Distributed-training.md)
- [`LLM-inference.md`](concepts/LLM-inference.md)
- [`LLM-training.md`](concepts/LLM-training.md)
- [`MS-Swift.md`](concepts/MS-Swift.md)
- [`Memory-pressure.md`](concepts/Memory-pressure.md)
- [`VLM-VLA.md`](concepts/VLM-VLA.md)
- [`Vision-model-trainval.md`](concepts/Vision-model-trainval.md)
- [`World-model.md`](concepts/World-model.md)
- [`XPU-training-adaptation.md`](concepts/XPU-training-adaptation.md)

## Recipe pages

- [`autonomous-driving-trainval.md`](recipes/autonomous-driving-trainval.md)
- [`basic-vision-trainval.md`](recipes/basic-vision-trainval.md)
- [`llm-inference.md`](recipes/llm-inference.md)
- [`llm-vlm-sft-lora.md`](recipes/llm-vlm-sft-lora.md)
- [`megatron-pretrain.md`](recipes/megatron-pretrain.md)
- [`qwen3-vl-8b-ms-swift-lora-sft.md`](recipes/qwen3-vl-8b-ms-swift-lora-sft.md)
- [`world-model-trainval.md`](recipes/world-model-trainval.md)

## Task-oriented entry points


### Inference

- [`cosmos_predict2.5_trainval.md`](../tutorials/cosmos_predict2.5_trainval.md) — cosmos-predict2.5
- [`llama2_70b_infer.md`](../tutorials/llama2_70b_infer.md) — Llama2-70B Inference Guide
- [`Isaac-GR00T-N1.7_trainval.md`](../tutorials/Isaac-GR00T-N1.7_trainval.md) — Isaac-GR00T N1.7 Trainval Guide
- [`LLaMA_infer.md`](../tutorials/LLaMA_infer.md) — LLaMA Inference Guide
- [`LLaVA_pretrain_trainval.md`](../tutorials/LLaVA_pretrain_trainval.md) — LLaVA
- [`LLaVA_trainval.md`](../tutorials/LLaVA_trainval.md) — LLaVA
- [`PaddleOCR_trainval.md`](../tutorials/PaddleOCR_trainval.md) — PaddleOCR_v5
- [`qwen2.5_infer.md`](../tutorials/qwen2.5_infer.md) — Qwen2.5 Inference Guide
- [`qwen2.5vl_infer.md`](../tutorials/qwen2.5vl_infer.md) — Qwen2.5-VL Inference Guide
- [`qwen3_235b_a22b_thinking_2507_infer.md`](../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) — Qwen3-235B-A22B-Thinking-2507 Inference Guide
- [`qwen3_omni_30b_a3b_infer.md`](../tutorials/qwen3_omni_30b_a3b_infer.md) — Qwen3-Omni-30B-A3B Inference Guide
- [`vLLM_infer.md`](../tutorials/vLLM_infer.md) — vLLM infer Guide
- [`xav_vLLM.md`](../tutorials/xav_vLLM.md) — xav-vLLM
- [`xvllm_general_infer.md`](../tutorials/xvllm_general_infer.md) — xvLLM General Inference Guide
- [`Yolo_inference.md`](../tutorials/Yolo_inference.md) — Yolo Inference Guide

### Pretrain

- [`AlphaDrive_trainval.md`](../tutorials/AlphaDrive_trainval.md) — AlphaDrive Trainval Guide
- [`bevdet_trainval.md`](../tutorials/bevdet_trainval.md) — BEVDet
- [`bevformer_trainval.md`](../tutorials/bevformer_trainval.md) — BEVFormer
- [`bevfusion_trainval.md`](../tutorials/bevfusion_trainval.md) — BEVFusion-MMDetection3D
- [`Bunny_trainval.md`](../tutorials/Bunny_trainval.md) — Bunny Trainval Guide
- [`flashocc_trainval.md`](../tutorials/flashocc_trainval.md) — FlashOCC
- [`Internvl3_8b_trainval.md`](../tutorials/Internvl3_8b_trainval.md) — InternVL3-8B Trainval Guide
- [`Isaac-GR00T-N1.7_trainval.md`](../tutorials/Isaac-GR00T-N1.7_trainval.md) — Isaac-GR00T N1.7 Trainval Guide
- [`LLaVA_pretrain_trainval.md`](../tutorials/LLaVA_pretrain_trainval.md) — LLaVA
- [`LLaVA_trainval.md`](../tutorials/LLaVA_trainval.md) — LLaVA
- [`maptrv2_trainval.md`](../tutorials/maptrv2_trainval.md) — MapTRv2
- [`mapvr_trainval.md`](../tutorials/mapvr_trainval.md) — MapVR
- [`openvla_trainval.md`](../tutorials/openvla_trainval.md) — OpenVLA Trainval Guide
- [`PaddleOCR_trainval.md`](../tutorials/PaddleOCR_trainval.md) — PaddleOCR_v5
- [`panoocc_trainval.md`](../tutorials/panoocc_trainval.md) — PanoOcc
- [`petrv2_trainval.md`](../tutorials/petrv2_trainval.md) — PETRv2
- [`Pi_0_trainval.md`](../tutorials/Pi_0_trainval.md) — Pi_0
- [`GR00T-Dreams_trainval.md`](../tutorials/GR00T-Dreams_trainval.md) — GR00T-Dreams
- [`Isaac-GR00T-N1.7_trainval.md`](../tutorials/Isaac-GR00T-N1.7_trainval.md) — Isaac-GR00T N1.7 Trainval Guide
- [`qwen2.5_trainval.md`](../tutorials/qwen2.5_trainval.md) — Qwen2.5 Trainval Guide
- [`qwen2.5vl_3b_trainval.md`](../tutorials/qwen2.5vl_3b_trainval.md) — Qwen2.5-VL-3B Trainval Guide
- [`qwen2.5vl_r1_trainval.md`](../tutorials/qwen2.5vl_r1_trainval.md) — Qwen2.5-VL-R1 Trainval Guide
- [`qwen2.5vl_trainval.md`](../tutorials/qwen2.5vl_trainval.md) — Qwen2.5-VL Trainval Guide
- [`qwen2_7b_trainval.md`](../tutorials/qwen2_7b_trainval.md) — Qwen2-7B Trainval Guide
- [`qwen2vl_7b_trainval.md`](../tutorials/qwen2vl_7b_trainval.md) — Qwen2-VL-7B
- [`qwen3_235b_a22b_thinking_2507_infer.md`](../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) — Qwen3-235B-A22B-Thinking-2507 Inference Guide
- [`qwen3_30b_a3b_pretrain.md`](../tutorials/qwen3_30b_a3b_pretrain.md) — Qwen3-30B-A3B Pretrain Guide
- [`qwen3_8b_megatron_trainval.md`](../tutorials/qwen3_8b_megatron_trainval.md) — Qwen3-8B Megatron Trainval Guide
- [`qwen3_8b_xmegatron_trainval.md`](../tutorials/qwen3_8b_xmegatron_trainval.md) — Qwen3-8B XMegatron Trainval Guide
- [`qwen3vl_8b_grpo_verl_trainval.md`](../tutorials/qwen3vl_8b_grpo_verl_trainval.md) — Qwen3vl-8B grpo verl Trainval Guide
- [`qwen3vl_8b_swift_trainval.md`](../tutorials/qwen3vl_8b_swift_trainval.md) — Qwen3-VL-8B MS-Swift Trainval Guide
- [`recogdrive_trainval.md`](../tutorials/recogdrive_trainval.md) — recogdrive
- [`sparse4d_trainval.md`](../tutorials/sparse4d_trainval.md) — Sparse4D
- [`StreamPETR_trainval.md`](../tutorials/StreamPETR_trainval.md) — StreamPETR
- [`TRL_AlphaDrive_trainval.md`](../tutorials/TRL_AlphaDrive_trainval.md) — AlphaDrive Trainval Guide
- [`vLLM_infer.md`](../tutorials/vLLM_infer.md) — vLLM infer Guide

### Trainval

- [`AlphaDrive_trainval.md`](../tutorials/AlphaDrive_trainval.md) — AlphaDrive Trainval Guide
- [`bevdet_trainval.md`](../tutorials/bevdet_trainval.md) — BEVDet
- [`bevformer_trainval.md`](../tutorials/bevformer_trainval.md) — BEVFormer
- [`bevfusion_trainval.md`](../tutorials/bevfusion_trainval.md) — BEVFusion-MMDetection3D
- [`Bunny_trainval.md`](../tutorials/Bunny_trainval.md) — Bunny Trainval Guide
- [`CenterPoint.md`](../tutorials/CenterPoint.md) — CenterPoint Trainval Guide
- [`cosmos_predict2.5_trainval.md`](../tutorials/cosmos_predict2.5_trainval.md) — cosmos-predict2.5
- [`cosmos_transfer2.5_trainval.md`](../tutorials/cosmos_transfer2.5_trainval.md) — cosmos-transfer2.5
- [`DiffusionDrive_trainval.md`](../tutorials/DiffusionDrive_trainval.md) — DiffusionDrive
- [`DINO_trainval.md`](../tutorials/DINO_trainval.md) — DINO
- [`DriveDreamer_trainval.md`](../tutorials/DriveDreamer_trainval.md) — DriveDreamer Trainval Guide
- [`Far3D_trainval.md`](../tutorials/Far3D_trainval.md) — Far3D
- [`FastBEV_trainval.md`](../tutorials/FastBEV_trainval.md) — FastBEV
- [`flashocc_trainval.md`](../tutorials/flashocc_trainval.md) — FlashOCC
- [`GameFormer-Planner_trainval.md`](../tutorials/GameFormer-Planner_trainval.md) — GameFormer-Planner
- [`GR00T-Dreams_trainval.md`](../tutorials/GR00T-Dreams_trainval.md) — GR00T-Dreams
- [`Internvl3_8b_trainval.md`](../tutorials/Internvl3_8b_trainval.md) — InternVL3-8B Trainval Guide
- [`lanesegnet_trainval.md`](../tutorials/lanesegnet_trainval.md) — LaneSegNet
- [`LLaVA_pretrain_trainval.md`](../tutorials/LLaVA_pretrain_trainval.md) — LLaVA
- [`LLaVA_trainval.md`](../tutorials/LLaVA_trainval.md) — LLaVA
- [`maptrv2_trainval.md`](../tutorials/maptrv2_trainval.md) — MapTRv2
- [`mapvr_trainval.md`](../tutorials/mapvr_trainval.md) — MapVR
- [`mask2former_trainval.md`](../tutorials/mask2former_trainval.md) — Mask2former
- [`MaskRCNN_trainval.md`](../tutorials/MaskRCNN_trainval.md) — MaskRCNN
- [`multipathpp_trainval.md`](../tutorials/multipathpp_trainval.md) — Multipath++
- [`openvla_trainval.md`](../tutorials/openvla_trainval.md) — OpenVLA Trainval Guide
- [`PaddleOCR_trainval.md`](../tutorials/PaddleOCR_trainval.md) — PaddleOCR_v5
- [`panoocc_trainval.md`](../tutorials/panoocc_trainval.md) — PanoOcc
- [`PETR_trainval.md`](../tutorials/PETR_trainval.md) — PETR Trainval Guide
- [`petrv2_trainval.md`](../tutorials/petrv2_trainval.md) — PETRv2
- [`Pi_0_trainval.md`](../tutorials/Pi_0_trainval.md) — Pi_0
- [`PointPillar_trainval.md`](../tutorials/PointPillar_trainval.md) — PointPillar
- [`Ptv3_Trainval_Guide.md`](../tutorials/Ptv3_Trainval_Guide.md) — Point Transformer V3 (Ptv3) Trainval Guide
- [`QCNet_trainval.md`](../tutorials/QCNet_trainval.md) — QCNet
- [`qwen2.5_trainval.md`](../tutorials/qwen2.5_trainval.md) — Qwen2.5 Trainval Guide
- [`qwen2.5vl_3b_trainval.md`](../tutorials/qwen2.5vl_3b_trainval.md) — Qwen2.5-VL-3B Trainval Guide
- [`qwen2.5vl_r1_trainval.md`](../tutorials/qwen2.5vl_r1_trainval.md) — Qwen2.5-VL-R1 Trainval Guide
- [`qwen2.5vl_trainval.md`](../tutorials/qwen2.5vl_trainval.md) — Qwen2.5-VL Trainval Guide
- [`qwen2_7b_trainval.md`](../tutorials/qwen2_7b_trainval.md) — Qwen2-7B Trainval Guide
- [`qwen2vl_7b_trainval.md`](../tutorials/qwen2vl_7b_trainval.md) — Qwen2-VL-7B
- [`qwen3_235b_a22b_thinking_2507_infer.md`](../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) — Qwen3-235B-A22B-Thinking-2507 Inference Guide
- [`qwen3_30b_a3b_pretrain.md`](../tutorials/qwen3_30b_a3b_pretrain.md) — Qwen3-30B-A3B Pretrain Guide
- [`qwen3_8b_megatron_trainval.md`](../tutorials/qwen3_8b_megatron_trainval.md) — Qwen3-8B Megatron Trainval Guide
- [`qwen3_8b_xmegatron_trainval.md`](../tutorials/qwen3_8b_xmegatron_trainval.md) — Qwen3-8B XMegatron Trainval Guide
- [`qwen3_llamafactory_trainval.md`](../tutorials/qwen3_llamafactory_trainval.md) — Qwen3 Trainval Guide (LlamaFactory)
- [`qwen3vl_8b_grpo_verl_trainval.md`](../tutorials/qwen3vl_8b_grpo_verl_trainval.md) — Qwen3vl-8B grpo verl Trainval Guide
- [`qwen3vl_8b_swift_trainval.md`](../tutorials/qwen3vl_8b_swift_trainval.md) — Qwen3-VL-8B MS-Swift Trainval Guide
- [`recogdrive_trainval.md`](../tutorials/recogdrive_trainval.md) — recogdrive
- [`regnet_trainval.md`](../tutorials/regnet_trainval.md) — RegNet Trainval Guide
- [`sparse4d_trainval.md`](../tutorials/sparse4d_trainval.md) — Sparse4D
- [`SparseDrive_trainval.md`](../tutorials/SparseDrive_trainval.md) — SparseDrive
- [`StateTransformer_trainval.md`](../tutorials/StateTransformer_trainval.md) — StateTransformer
- [`StreamPETR_trainval.md`](../tutorials/StreamPETR_trainval.md) — StreamPETR
- [`TRL_AlphaDrive_trainval.md`](../tutorials/TRL_AlphaDrive_trainval.md) — AlphaDrive Trainval Guide
- [`UniAD_trainval.md`](../tutorials/UniAD_trainval.md) — UniAD
- [`VAD_trainval.md`](../tutorials/VAD_trainval.md) — VAD
- [`VIT_trainval.md`](../tutorials/VIT_trainval.md) — VIT
- [`vLLM_infer.md`](../tutorials/vLLM_infer.md) — vLLM infer Guide

### SFT

- [`Bunny_trainval.md`](../tutorials/Bunny_trainval.md) — Bunny Trainval Guide
- [`GR00T-Dreams_trainval.md`](../tutorials/GR00T-Dreams_trainval.md) — GR00T-Dreams
- [`Internvl3_8b_trainval.md`](../tutorials/Internvl3_8b_trainval.md) — InternVL3-8B Trainval Guide
- [`Isaac-GR00T-N1.7_trainval.md`](../tutorials/Isaac-GR00T-N1.7_trainval.md) — Isaac-GR00T N1.7 Trainval Guide
- [`LLaVA_trainval.md`](../tutorials/LLaVA_trainval.md) — LLaVA
- [`openvla_trainval.md`](../tutorials/openvla_trainval.md) — OpenVLA Trainval Guide
- [`Pi_0_trainval.md`](../tutorials/Pi_0_trainval.md) — Pi_0
- [`qwen2.5_trainval.md`](../tutorials/qwen2.5_trainval.md) — Qwen2.5 Trainval Guide
- [`qwen2.5vl_3b_trainval.md`](../tutorials/qwen2.5vl_3b_trainval.md) — Qwen2.5-VL-3B Trainval Guide
- [`qwen2.5vl_trainval.md`](../tutorials/qwen2.5vl_trainval.md) — Qwen2.5-VL Trainval Guide
- [`qwen2_7b_trainval.md`](../tutorials/qwen2_7b_trainval.md) — Qwen2-7B Trainval Guide
- [`qwen2vl_7b_trainval.md`](../tutorials/qwen2vl_7b_trainval.md) — Qwen2-VL-7B
- [`qwen3_30b_a3b_pretrain.md`](../tutorials/qwen3_30b_a3b_pretrain.md) — Qwen3-30B-A3B Pretrain Guide
- [`qwen3_8b_megatron_trainval.md`](../tutorials/qwen3_8b_megatron_trainval.md) — Qwen3-8B Megatron Trainval Guide
- [`qwen3_llamafactory_trainval.md`](../tutorials/qwen3_llamafactory_trainval.md) — Qwen3 Trainval Guide (LlamaFactory)
- [`qwen3vl_8b_swift_trainval.md`](../tutorials/qwen3vl_8b_swift_trainval.md) — Qwen3-VL-8B MS-Swift Trainval Guide
- [`TRL_AlphaDrive_trainval.md`](../tutorials/TRL_AlphaDrive_trainval.md) — AlphaDrive Trainval Guide

### LoRA

- [`Bunny_trainval.md`](../tutorials/Bunny_trainval.md) — Bunny Trainval Guide
- [`Internvl3_8b_trainval.md`](../tutorials/Internvl3_8b_trainval.md) — InternVL3-8B Trainval Guide
- [`qwen2.5_trainval.md`](../tutorials/qwen2.5_trainval.md) — Qwen2.5 Trainval Guide
- [`qwen2.5vl_3b_trainval.md`](../tutorials/qwen2.5vl_3b_trainval.md) — Qwen2.5-VL-3B Trainval Guide
- [`qwen2.5vl_trainval.md`](../tutorials/qwen2.5vl_trainval.md) — Qwen2.5-VL Trainval Guide
- [`qwen2_7b_trainval.md`](../tutorials/qwen2_7b_trainval.md) — Qwen2-7B Trainval Guide
- [`qwen2vl_7b_trainval.md`](../tutorials/qwen2vl_7b_trainval.md) — Qwen2-VL-7B
- [`qwen3vl_8b_swift_trainval.md`](../tutorials/qwen3vl_8b_swift_trainval.md) — Qwen3-VL-8B MS-Swift Trainval Guide

### GRPO

- [`AlphaDrive_trainval.md`](../tutorials/AlphaDrive_trainval.md) — AlphaDrive Trainval Guide
- [`qwen3vl_8b_grpo_verl_trainval.md`](../tutorials/qwen3vl_8b_grpo_verl_trainval.md) — Qwen3vl-8B grpo verl Trainval Guide
- [`TRL_AlphaDrive_trainval.md`](../tutorials/TRL_AlphaDrive_trainval.md) — AlphaDrive Trainval Guide

### Benchmark

- [`qwen3_omni_30b_a3b_infer.md`](../tutorials/qwen3_omni_30b_a3b_infer.md) — Qwen3-Omni-30B-A3B Inference Guide
- [`xav_vLLM.md`](../tutorials/xav_vLLM.md) — xav-vLLM

## Domain-oriented entry points


### Autonomous Driving

- [`AlphaDrive_trainval.md`](../tutorials/AlphaDrive_trainval.md) — [`AlphaDrive`](models/AlphaDrive.md)
- [`bevdet_trainval.md`](../tutorials/bevdet_trainval.md) — [`BEVDet`](models/BEVDet.md)
- [`bevformer_trainval.md`](../tutorials/bevformer_trainval.md) — [`BEVFormer`](models/BEVFormer.md)
- [`bevfusion_trainval.md`](../tutorials/bevfusion_trainval.md) — [`BEVFusion-MMDetection3D`](models/BEVFusion-MMDetection3D.md)
- [`CenterPoint.md`](../tutorials/CenterPoint.md) — [`CenterPoint`](models/CenterPoint.md)
- [`Far3D_trainval.md`](../tutorials/Far3D_trainval.md) — [`Far3D`](models/Far3D.md)
- [`FastBEV_trainval.md`](../tutorials/FastBEV_trainval.md) — [`FastBEV`](models/FastBEV.md)
- [`flashocc_trainval.md`](../tutorials/flashocc_trainval.md) — [`FlashOCC`](models/FlashOCC.md)
- [`GameFormer-Planner_trainval.md`](../tutorials/GameFormer-Planner_trainval.md) — [`GameFormer-Planner`](models/GameFormer-Planner.md)
- [`lanesegnet_trainval.md`](../tutorials/lanesegnet_trainval.md) — [`LaneSegNet`](models/LaneSegNet.md)
- [`maptrv2_trainval.md`](../tutorials/maptrv2_trainval.md) — [`MapTRv2`](models/MapTRv2.md)
- [`mapvr_trainval.md`](../tutorials/mapvr_trainval.md) — [`MapVR`](models/MapVR.md)
- [`multipathpp_trainval.md`](../tutorials/multipathpp_trainval.md) — [`Multipath++`](models/Multipath++.md)
- [`panoocc_trainval.md`](../tutorials/panoocc_trainval.md) — [`PanoOcc`](models/PanoOcc.md)
- [`PETR_trainval.md`](../tutorials/PETR_trainval.md) — [`PETR`](models/PETR.md)
- [`petrv2_trainval.md`](../tutorials/petrv2_trainval.md) — [`PETRv2`](models/PETRv2.md)
- [`PointPillar_trainval.md`](../tutorials/PointPillar_trainval.md) — [`PointPillar`](models/PointPillar.md)
- [`Ptv3_Trainval_Guide.md`](../tutorials/Ptv3_Trainval_Guide.md) — [`PTv3`](models/PTv3.md)
- [`QCNet_trainval.md`](../tutorials/QCNet_trainval.md) — [`QCNet`](models/QCNet.md)
- [`recogdrive_trainval.md`](../tutorials/recogdrive_trainval.md) — [`recogdrive`](models/recogdrive.md)
- [`sparse4d_trainval.md`](../tutorials/sparse4d_trainval.md) — [`Sparse4D`](models/Sparse4D.md)
- [`SparseDrive_trainval.md`](../tutorials/SparseDrive_trainval.md) — [`SparseDrive`](models/SparseDrive.md)
- [`StreamPETR_trainval.md`](../tutorials/StreamPETR_trainval.md) — [`StreamPETR`](models/StreamPETR.md)
- [`TRL_AlphaDrive_trainval.md`](../tutorials/TRL_AlphaDrive_trainval.md) — [`AlphaDrive`](models/AlphaDrive.md)
- [`UniAD_trainval.md`](../tutorials/UniAD_trainval.md) — [`UniAD`](models/UniAD.md)
- [`VAD_trainval.md`](../tutorials/VAD_trainval.md) — [`VAD`](models/VAD.md)

### General

- [`StateTransformer_trainval.md`](../tutorials/StateTransformer_trainval.md) — [`StateTransformer`](models/StateTransformer.md)
- [`vLLM_infer.md`](../tutorials/vLLM_infer.md) — [`vLLM`](models/vLLM.md)
- [`xav_vLLM.md`](../tutorials/xav_vLLM.md) — [`xav-vLLM`](models/xav-vLLM.md)

### LLM/VLM/VLA

- [`Bunny_trainval.md`](../tutorials/Bunny_trainval.md) — [`Bunny`](models/Bunny.md)
- [`GR00T-Dreams_trainval.md`](../tutorials/GR00T-Dreams_trainval.md) — [`GR00T-Dreams`](models/GR00T-Dreams.md)
- [`Internvl3_8b_trainval.md`](../tutorials/Internvl3_8b_trainval.md) — [`InternVL3-8B`](models/InternVL3-8B.md)
- [`Isaac-GR00T-N1.7_trainval.md`](../tutorials/Isaac-GR00T-N1.7_trainval.md) — [`Isaac-GR00T-N1.7`](models/Isaac-GR00T-N1.7.md)
- [`llama2_70b_infer.md`](../tutorials/llama2_70b_infer.md) — [`Llama2-70B`](models/Llama2-70B.md)
- [`LLaMA_infer.md`](../tutorials/LLaMA_infer.md) — [`LLaMA`](models/LLaMA.md)
- [`LLaVA_pretrain_trainval.md`](../tutorials/LLaVA_pretrain_trainval.md) — [`LLaVA`](models/LLaVA.md)
- [`LLaVA_trainval.md`](../tutorials/LLaVA_trainval.md) — [`LLaVA`](models/LLaVA.md)
- [`openvla_trainval.md`](../tutorials/openvla_trainval.md) — [`OpenVLA`](models/OpenVLA.md)
- [`Pi_0_trainval.md`](../tutorials/Pi_0_trainval.md) — [`Pi0`](models/Pi0.md)
- [`qwen2.5_infer.md`](../tutorials/qwen2.5_infer.md) — [`Qwen2.5`](models/Qwen2.5.md)
- [`qwen2.5_trainval.md`](../tutorials/qwen2.5_trainval.md) — [`Qwen2.5`](models/Qwen2.5.md)
- [`qwen2.5vl_3b_trainval.md`](../tutorials/qwen2.5vl_3b_trainval.md) — [`Qwen2.5-VL`](models/Qwen2.5-VL.md)
- [`qwen2.5vl_infer.md`](../tutorials/qwen2.5vl_infer.md) — [`Qwen2.5-VL`](models/Qwen2.5-VL.md)
- [`qwen2.5vl_r1_trainval.md`](../tutorials/qwen2.5vl_r1_trainval.md) — [`Qwen2.5-VL`](models/Qwen2.5-VL.md)
- [`qwen2.5vl_trainval.md`](../tutorials/qwen2.5vl_trainval.md) — [`Qwen2.5-VL`](models/Qwen2.5-VL.md)
- [`qwen2_7b_trainval.md`](../tutorials/qwen2_7b_trainval.md) — [`Qwen2-7B`](models/Qwen2-7B.md)
- [`qwen2vl_7b_trainval.md`](../tutorials/qwen2vl_7b_trainval.md) — [`Qwen2-VL-7B`](models/Qwen2-VL-7B.md)
- [`qwen3_235b_a22b_thinking_2507_infer.md`](../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) — [`Qwen3-235B-A22B-Thinking-2507`](models/Qwen3-235B-A22B-Thinking-2507.md)
- [`qwen3_30b_a3b_pretrain.md`](../tutorials/qwen3_30b_a3b_pretrain.md) — [`Qwen3-30B-A3B`](models/Qwen3-30B-A3B.md)
- [`qwen3_8b_megatron_trainval.md`](../tutorials/qwen3_8b_megatron_trainval.md) — [`Qwen3-8B`](models/Qwen3-8B.md)
- [`qwen3_8b_xmegatron_trainval.md`](../tutorials/qwen3_8b_xmegatron_trainval.md) — [`Qwen3-8B`](models/Qwen3-8B.md)
- [`qwen3_llamafactory_trainval.md`](../tutorials/qwen3_llamafactory_trainval.md) — [`Qwen3 (LlamaFactory)`](models/Qwen3-LlamaFactory.md)
- [`qwen3_omni_30b_a3b_infer.md`](../tutorials/qwen3_omni_30b_a3b_infer.md) — [`Qwen3-Omni-30B-A3B`](models/Qwen3-Omni-30B-A3B.md)
- [`qwen3vl_8b_grpo_verl_trainval.md`](../tutorials/qwen3vl_8b_grpo_verl_trainval.md) — [`Qwen3-VL-8B`](models/Qwen3-VL-8B.md)
- [`qwen3vl_8b_swift_trainval.md`](../tutorials/qwen3vl_8b_swift_trainval.md) — [`Qwen3-VL-8B`](models/Qwen3-VL-8B.md)
- [`xvllm_general_infer.md`](../tutorials/xvllm_general_infer.md) — [`xvLLM`](models/xvLLM.md)

### Vision/OCR

- [`DINO_trainval.md`](../tutorials/DINO_trainval.md) — [`DINO`](models/DINO.md)
- [`mask2former_trainval.md`](../tutorials/mask2former_trainval.md) — [`Mask2former`](models/Mask2former.md)
- [`MaskRCNN_trainval.md`](../tutorials/MaskRCNN_trainval.md) — [`MaskRCNN`](models/MaskRCNN.md)
- [`PaddleOCR_trainval.md`](../tutorials/PaddleOCR_trainval.md) — [`PaddleOCR-v5`](models/PaddleOCR-v5.md)
- [`regnet_trainval.md`](../tutorials/regnet_trainval.md) — [`RegNet`](models/RegNet.md)
- [`VIT_trainval.md`](../tutorials/VIT_trainval.md) — [`VIT`](models/VIT.md)
- [`Yolo_inference.md`](../tutorials/Yolo_inference.md) — [`Yolo`](models/Yolo.md)

### World Model

- [`cosmos_predict2.5_trainval.md`](../tutorials/cosmos_predict2.5_trainval.md) — [`cosmos-predict2.5`](models/cosmos-predict2.5.md)
- [`cosmos_transfer2.5_trainval.md`](../tutorials/cosmos_transfer2.5_trainval.md) — [`cosmos-transfer2.5`](models/cosmos-transfer2.5.md)
- [`DiffusionDrive_trainval.md`](../tutorials/DiffusionDrive_trainval.md) — [`DiffusionDrive`](models/DiffusionDrive.md)
- [`DriveDreamer_trainval.md`](../tutorials/DriveDreamer_trainval.md) — [`DriveDreamer`](models/DriveDreamer.md)

## All source tutorials
| Tutorial | Title | Source summary |
| --- | --- | --- |
| [`AlphaDrive_trainval.md`](../tutorials/AlphaDrive_trainval.md) | AlphaDrive Trainval Guide | [`alphadrive_trainval.md`](sources/alphadrive_trainval.md) |
| [`Bunny_trainval.md`](../tutorials/Bunny_trainval.md) | Bunny Trainval Guide | [`bunny_trainval.md`](sources/bunny_trainval.md) |
| [`CenterPoint.md`](../tutorials/CenterPoint.md) | CenterPoint Trainval Guide | [`centerpoint.md`](sources/centerpoint.md) |
| [`DINO_trainval.md`](../tutorials/DINO_trainval.md) | DINO | [`dino_trainval.md`](sources/dino_trainval.md) |
| [`DiffusionDrive_trainval.md`](../tutorials/DiffusionDrive_trainval.md) | DiffusionDrive | [`diffusiondrive_trainval.md`](sources/diffusiondrive_trainval.md) |
| [`DriveDreamer_trainval.md`](../tutorials/DriveDreamer_trainval.md) | DriveDreamer Trainval Guide | [`drivedreamer_trainval.md`](sources/drivedreamer_trainval.md) |
| [`Far3D_trainval.md`](../tutorials/Far3D_trainval.md) | Far3D | [`far3d_trainval.md`](sources/far3d_trainval.md) |
| [`FastBEV_trainval.md`](../tutorials/FastBEV_trainval.md) | FastBEV | [`fastbev_trainval.md`](sources/fastbev_trainval.md) |
| [`GameFormer-Planner_trainval.md`](../tutorials/GameFormer-Planner_trainval.md) | GameFormer-Planner | [`gameformer-planner_trainval.md`](sources/gameformer-planner_trainval.md) |
| [`GR00T-Dreams_trainval.md`](../tutorials/GR00T-Dreams_trainval.md) | GR00T-Dreams | [`groot-dreams_trainval.md`](sources/groot-dreams_trainval.md) |
| [`Internvl3_8b_trainval.md`](../tutorials/Internvl3_8b_trainval.md) | InternVL3-8B Trainval Guide | [`internvl3_8b_trainval.md`](sources/internvl3_8b_trainval.md) |
| [`Isaac-GR00T-N1.7_trainval.md`](../tutorials/Isaac-GR00T-N1.7_trainval.md) | Isaac-GR00T N1.7 Trainval Guide | [`isaac-groot-n1.7_trainval.md`](sources/isaac-groot-n1.7_trainval.md) |
| [`LLaMA_infer.md`](../tutorials/LLaMA_infer.md) | LLaMA Inference Guide | [`llama_infer.md`](sources/llama_infer.md) |
| [`LLaVA_pretrain_trainval.md`](../tutorials/LLaVA_pretrain_trainval.md) | LLaVA | [`llava_pretrain_trainval.md`](sources/llava_pretrain_trainval.md) |
| [`LLaVA_trainval.md`](../tutorials/LLaVA_trainval.md) | LLaVA | [`llava_trainval.md`](sources/llava_trainval.md) |
| [`MaskRCNN_trainval.md`](../tutorials/MaskRCNN_trainval.md) | MaskRCNN | [`maskrcnn_trainval.md`](sources/maskrcnn_trainval.md) |
| [`PETR_trainval.md`](../tutorials/PETR_trainval.md) | PETR Trainval Guide | [`petr_trainval.md`](sources/petr_trainval.md) |
| [`PaddleOCR_trainval.md`](../tutorials/PaddleOCR_trainval.md) | PaddleOCR_v5 | [`paddleocr_trainval.md`](sources/paddleocr_trainval.md) |
| [`Pi_0_trainval.md`](../tutorials/Pi_0_trainval.md) | Pi_0 | [`pi_0_trainval.md`](sources/pi_0_trainval.md) |
| [`PointPillar_trainval.md`](../tutorials/PointPillar_trainval.md) | PointPillar | [`pointpillar_trainval.md`](sources/pointpillar_trainval.md) |
| [`Ptv3_Trainval_Guide.md`](../tutorials/Ptv3_Trainval_Guide.md) | Point Transformer V3 (Ptv3) Trainval Guide | [`ptv3_trainval.md`](sources/ptv3_trainval.md) |
| [`QCNet_trainval.md`](../tutorials/QCNet_trainval.md) | QCNet | [`qcnet_trainval.md`](sources/qcnet_trainval.md) |
| [`SparseDrive_trainval.md`](../tutorials/SparseDrive_trainval.md) | SparseDrive | [`sparsedrive_trainval.md`](sources/sparsedrive_trainval.md) |
| [`StateTransformer_trainval.md`](../tutorials/StateTransformer_trainval.md) | StateTransformer | [`statetransformer_trainval.md`](sources/statetransformer_trainval.md) |
| [`StreamPETR_trainval.md`](../tutorials/StreamPETR_trainval.md) | StreamPETR | [`streampetr_trainval.md`](sources/streampetr_trainval.md) |
| [`TRL_AlphaDrive_trainval.md`](../tutorials/TRL_AlphaDrive_trainval.md) | AlphaDrive Trainval Guide | [`trl_alphadrive_trainval.md`](sources/trl_alphadrive_trainval.md) |
| [`UniAD_trainval.md`](../tutorials/UniAD_trainval.md) | UniAD | [`uniad_trainval.md`](sources/uniad_trainval.md) |
| [`VAD_trainval.md`](../tutorials/VAD_trainval.md) | VAD | [`vad_trainval.md`](sources/vad_trainval.md) |
| [`VIT_trainval.md`](../tutorials/VIT_trainval.md) | VIT | [`vit_trainval.md`](sources/vit_trainval.md) |
| [`Yolo_inference.md`](../tutorials/Yolo_inference.md) | Yolo Inference Guide | [`yolo_inference.md`](sources/yolo_inference.md) |
| [`bevdet_trainval.md`](../tutorials/bevdet_trainval.md) | BEVDet | [`bevdet_trainval.md`](sources/bevdet_trainval.md) |
| [`bevformer_trainval.md`](../tutorials/bevformer_trainval.md) | BEVFormer | [`bevformer_trainval.md`](sources/bevformer_trainval.md) |
| [`bevfusion_trainval.md`](../tutorials/bevfusion_trainval.md) | BEVFusion-MMDetection3D | [`bevfusion_trainval.md`](sources/bevfusion_trainval.md) |
| [`cosmos_predict2.5_trainval.md`](../tutorials/cosmos_predict2.5_trainval.md) | cosmos-predict2.5 | [`cosmos_predict2.5_trainval.md`](sources/cosmos_predict2.5_trainval.md) |
| [`cosmos_transfer2.5_trainval.md`](../tutorials/cosmos_transfer2.5_trainval.md) | cosmos-transfer2.5 | [`cosmos_transfer2.5_trainval.md`](sources/cosmos_transfer2.5_trainval.md) |
| [`flashocc_trainval.md`](../tutorials/flashocc_trainval.md) | FlashOCC | [`flashocc_trainval.md`](sources/flashocc_trainval.md) |
| [`lanesegnet_trainval.md`](../tutorials/lanesegnet_trainval.md) | LaneSegNet | [`lanesegnet_trainval.md`](sources/lanesegnet_trainval.md) |
| [`llama2_70b_infer.md`](../tutorials/llama2_70b_infer.md) | Llama2-70B Inference Guide | [`llama2_70b_infer.md`](sources/llama2_70b_infer.md) |
| [`maptrv2_trainval.md`](../tutorials/maptrv2_trainval.md) | MapTRv2 | [`maptrv2_trainval.md`](sources/maptrv2_trainval.md) |
| [`mapvr_trainval.md`](../tutorials/mapvr_trainval.md) | MapVR | [`mapvr_trainval.md`](sources/mapvr_trainval.md) |
| [`mask2former_trainval.md`](../tutorials/mask2former_trainval.md) | Mask2former | [`mask2former_trainval.md`](sources/mask2former_trainval.md) |
| [`multipathpp_trainval.md`](../tutorials/multipathpp_trainval.md) | Multipath++ | [`multipathpp_trainval.md`](sources/multipathpp_trainval.md) |
| [`openvla_trainval.md`](../tutorials/openvla_trainval.md) | OpenVLA Trainval Guide | [`openvla_trainval.md`](sources/openvla_trainval.md) |
| [`panoocc_trainval.md`](../tutorials/panoocc_trainval.md) | PanoOcc | [`panoocc_trainval.md`](sources/panoocc_trainval.md) |
| [`petrv2_trainval.md`](../tutorials/petrv2_trainval.md) | PETRv2 | [`petrv2_trainval.md`](sources/petrv2_trainval.md) |
| [`qwen2.5_infer.md`](../tutorials/qwen2.5_infer.md) | Qwen2.5 Inference Guide | [`qwen2.5_infer.md`](sources/qwen2.5_infer.md) |
| [`qwen2.5_trainval.md`](../tutorials/qwen2.5_trainval.md) | Qwen2.5 Trainval Guide | [`qwen2.5_trainval.md`](sources/qwen2.5_trainval.md) |
| [`qwen2.5vl_3b_trainval.md`](../tutorials/qwen2.5vl_3b_trainval.md) | Qwen2.5-VL-3B Trainval Guide | [`qwen2.5vl_3b_trainval.md`](sources/qwen2.5vl_3b_trainval.md) |
| [`qwen2.5vl_infer.md`](../tutorials/qwen2.5vl_infer.md) | Qwen2.5-VL Inference Guide | [`qwen2.5vl_infer.md`](sources/qwen2.5vl_infer.md) |
| [`qwen2.5vl_r1_trainval.md`](../tutorials/qwen2.5vl_r1_trainval.md) | Qwen2.5-VL-R1 Trainval Guide | [`qwen2.5vl_r1_trainval.md`](sources/qwen2.5vl_r1_trainval.md) |
| [`qwen2.5vl_trainval.md`](../tutorials/qwen2.5vl_trainval.md) | Qwen2.5-VL Trainval Guide | [`qwen2.5vl_trainval.md`](sources/qwen2.5vl_trainval.md) |
| [`qwen2_7b_trainval.md`](../tutorials/qwen2_7b_trainval.md) | Qwen2-7B Trainval Guide | [`qwen2_7b_trainval.md`](sources/qwen2_7b_trainval.md) |
| [`qwen2vl_7b_trainval.md`](../tutorials/qwen2vl_7b_trainval.md) | Qwen2-VL-7B | [`qwen2vl_7b_trainval.md`](sources/qwen2vl_7b_trainval.md) |
| [`qwen3_235b_a22b_thinking_2507_infer.md`](../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) | Qwen3-235B-A22B-Thinking-2507 Inference Guide | [`qwen3_235b_a22b_thinking_2507_infer.md`](sources/qwen3_235b_a22b_thinking_2507_infer.md) |
| [`qwen3_30b_a3b_pretrain.md`](../tutorials/qwen3_30b_a3b_pretrain.md) | Qwen3-30B-A3B Pretrain Guide | [`qwen3_30b_a3b_pretrain.md`](sources/qwen3_30b_a3b_pretrain.md) |
| [`qwen3_8b_megatron_trainval.md`](../tutorials/qwen3_8b_megatron_trainval.md) | Qwen3-8B Megatron Trainval Guide | [`qwen3_8b_megatron_trainval.md`](sources/qwen3_8b_megatron_trainval.md) |
| [`qwen3_8b_xmegatron_trainval.md`](../tutorials/qwen3_8b_xmegatron_trainval.md) | Qwen3-8B XMegatron Trainval Guide | [`qwen3_8b_xmegatron_trainval.md`](sources/qwen3_8b_xmegatron_trainval.md) |
| [`qwen3_llamafactory_trainval.md`](../tutorials/qwen3_llamafactory_trainval.md) | Qwen3 Trainval Guide (LlamaFactory) | [`qwen3_llamafactory_trainval.md`](sources/qwen3_llamafactory_trainval.md) |
| [`qwen3_omni_30b_a3b_infer.md`](../tutorials/qwen3_omni_30b_a3b_infer.md) | Qwen3-Omni-30B-A3B Inference Guide | [`qwen3_omni_30b_a3b_infer.md`](sources/qwen3_omni_30b_a3b_infer.md) |
| [`qwen3vl_8b_grpo_verl_trainval.md`](../tutorials/qwen3vl_8b_grpo_verl_trainval.md) | Qwen3vl-8B grpo verl Trainval Guide | [`qwen3vl_8b_grpo_verl_trainval.md`](sources/qwen3vl_8b_grpo_verl_trainval.md) |
| [`qwen3vl_8b_swift_trainval.md`](../tutorials/qwen3vl_8b_swift_trainval.md) | Qwen3-VL-8B MS-Swift Trainval Guide | [`qwen3vl-8b-swift-trainval.md`](sources/qwen3vl-8b-swift-trainval.md) |
| [`recogdrive_trainval.md`](../tutorials/recogdrive_trainval.md) | recogdrive | [`recogdrive_trainval.md`](sources/recogdrive_trainval.md) |
| [`regnet_trainval.md`](../tutorials/regnet_trainval.md) | RegNet Trainval Guide | [`regnet_trainval.md`](sources/regnet_trainval.md) |
| [`sparse4d_trainval.md`](../tutorials/sparse4d_trainval.md) | Sparse4D | [`sparse4d_trainval.md`](sources/sparse4d_trainval.md) |
| [`vLLM_infer.md`](../tutorials/vLLM_infer.md) | vLLM infer Guide | [`vllm_infer.md`](sources/vllm_infer.md) |
| [`xav_vLLM.md`](../tutorials/xav_vLLM.md) | xav-vLLM | [`xav_vllm.md`](sources/xav_vllm.md) |
| [`xvllm_general_infer.md`](../tutorials/xvllm_general_infer.md) | xvLLM General Inference Guide | [`xvllm_general_infer.md`](sources/xvllm_general_infer.md) |
