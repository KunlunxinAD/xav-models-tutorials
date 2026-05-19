# LLaVA

## Sources

- [`LLaVA_pretrain_trainval.md`](../../tutorials/LLaVA_pretrain_trainval.md) — [`source summary`](../sources/llava_pretrain_trainval.md)
- [`LLaVA_trainval.md`](../../tutorials/LLaVA_trainval.md) — [`source summary`](../sources/llava_trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`LLaVA_pretrain_trainval.md`](../../tutorials/LLaVA_pretrain_trainval.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval | flash_attn | Not explicitly stated | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| [`LLaVA_trainval.md`](../../tutorials/LLaVA_trainval.md) | LLM/VLM/VLA | Inference, Pretrain, Trainval, SFT | flash_attn | Not explicitly stated | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |

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
