# Qwen2.5-VL

## Sources

- [`qwen2.5vl_3b_trainval.md`](../../tutorials/qwen2.5vl_3b_trainval.md) — [`source summary`](../sources/qwen2.5vl_3b_trainval.md)
- [`qwen2.5vl_infer.md`](../../tutorials/qwen2.5vl_infer.md) — [`source summary`](../sources/qwen2.5vl_infer.md)
- [`qwen2.5vl_r1_trainval.md`](../../tutorials/qwen2.5vl_r1_trainval.md) — [`source summary`](../sources/qwen2.5vl_r1_trainval.md)
- [`qwen2.5vl_trainval.md`](../../tutorials/qwen2.5vl_trainval.md) — [`source summary`](../sources/qwen2.5vl_trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`qwen2.5vl_3b_trainval.md`](../../tutorials/qwen2.5vl_3b_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory | Not explicitly stated | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| [`qwen2.5vl_infer.md`](../../tutorials/qwen2.5vl_infer.md) | LLM/VLM/VLA | Inference | TensorRT | Not explicitly stated | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| [`qwen2.5vl_r1_trainval.md`](../../tutorials/qwen2.5vl_r1_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval | Deepspeed, flash_attn | BF16 | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |
| [`qwen2.5vl_trainval.md`](../../tutorials/qwen2.5vl_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | flash_attn, LlamaFactory | Not explicitly stated | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |

## Related wiki pages

- [`Benchmark-and-evaluation`](../concepts/Benchmark-and-evaluation.md)
- [`Container-and-XPU-runtime`](../concepts/Container-and-XPU-runtime.md)
- [`LLM-inference`](../concepts/LLM-inference.md)
- [`LLM-training`](../concepts/LLM-training.md)
- [`Memory-pressure`](../concepts/Memory-pressure.md)
- [`VLM-VLA`](../concepts/VLM-VLA.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial.
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
