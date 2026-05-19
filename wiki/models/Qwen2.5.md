# Qwen2.5

## Sources

- [`qwen2.5_infer.md`](../../tutorials/qwen2.5_infer.md) — [`source summary`](../sources/qwen2.5_infer.md)
- [`qwen2.5_trainval.md`](../../tutorials/qwen2.5_trainval.md) — [`source summary`](../sources/qwen2.5_trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`qwen2.5_infer.md`](../../tutorials/qwen2.5_infer.md) | LLM/VLM/VLA | Inference | TensorRT, vLLM | BF16, FP16 | Not explicitly extracted |
| [`qwen2.5_trainval.md`](../../tutorials/qwen2.5_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, LlamaFactory | Not explicitly stated | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |

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
