# LLM Inference

## Summary

Inference and serving tutorials across LLaMA, Qwen, vLLM, xvLLM, xav-vLLM, and xSGL-style backends when present in sources.

## Source-backed entries

| Tutorial | Model / topic | Domain | Workflow | Frameworks / backends |
| --- | --- | --- | --- | --- |
| [`cosmos_predict2.5_trainval.md`](../../tutorials/cosmos_predict2.5_trainval.md) | [`cosmos-predict2.5`](../models/cosmos-predict2.5.md) | World Model | Inference, Trainval | Megatron |
| [`llama2_70b_infer.md`](../../tutorials/llama2_70b_infer.md) | [`Llama2-70B`](../models/Llama2-70B.md) | LLM/VLM/VLA | Inference | Not explicitly extracted |
| [`LLaMA_infer.md`](../../tutorials/LLaMA_infer.md) | [`LLaMA`](../models/LLaMA.md) | LLM/VLM/VLA | Inference | TensorRT, vLLM |
| [`LLaVA_pretrain_trainval.md`](../../tutorials/LLaVA_pretrain_trainval.md) | [`LLaVA`](../models/LLaVA.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval | flash_attn |
| [`LLaVA_trainval.md`](../../tutorials/LLaVA_trainval.md) | [`LLaVA`](../models/LLaVA.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval, SFT | flash_attn |
| [`PaddleOCR_trainval.md`](../../tutorials/PaddleOCR_trainval.md) | [`PaddleOCR-v5`](../models/PaddleOCR-v5.md) | Vision/OCR | Inference, Pretrain, Trainval | PaddlePaddle |
| [`qwen2.5_infer.md`](../../tutorials/qwen2.5_infer.md) | [`Qwen2.5`](../models/Qwen2.5.md) | LLM/VLM/VLA | Inference | TensorRT, vLLM |
| [`qwen2.5vl_infer.md`](../../tutorials/qwen2.5vl_infer.md) | [`Qwen2.5-VL`](../models/Qwen2.5-VL.md) | LLM/VLM/VLA | Inference | TensorRT |
| [`qwen3_235b_a22b_thinking_2507_infer.md`](../../tutorials/qwen3_235b_a22b_thinking_2507_infer.md) | [`Qwen3-235B-A22B-Thinking-2507`](../models/Qwen3-235B-A22B-Thinking-2507.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval | SGLang |
| [`qwen3_omni_30b_a3b_infer.md`](../../tutorials/qwen3_omni_30b_a3b_infer.md) | [`Qwen3-Omni-30B-A3B`](../models/Qwen3-Omni-30B-A3B.md) | LLM/VLM/VLA | Inference, Benchmark | vLLM, xvLLM |
| [`vLLM_infer.md`](../../tutorials/vLLM_infer.md) | [`vLLM`](../models/vLLM.md) | General | Inference, Pretrain, Trainval | vLLM, xvLLM |
| [`xav_vLLM.md`](../../tutorials/xav_vLLM.md) | [`xav-vLLM`](../models/xav-vLLM.md) | General | Inference, Benchmark | vLLM, xav-vLLM |
| [`xvllm_general_infer.md`](../../tutorials/xvllm_general_infer.md) | [`xvLLM`](../models/xvLLM.md) | LLM/VLM/VLA | Inference | vLLM, xav-vLLM, xvLLM |
| [`Yolo_inference.md`](../../tutorials/Yolo_inference.md) | [`Yolo`](../models/Yolo.md) | Vision/OCR | Inference | Not explicitly extracted |

## Provenance rules

- Treat this page as a derived index over linked tutorials, not as an independent source of benchmark truth.
- Preserve placeholders and verify exact commands in the source tutorial before reuse.
