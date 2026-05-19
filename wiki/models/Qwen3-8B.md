# Qwen3-8B

## Sources

- [`qwen3_8b_megatron_trainval.md`](../../tutorials/qwen3_8b_megatron_trainval.md) — [`source summary`](../sources/qwen3_8b_megatron_trainval.md)
- [`qwen3_8b_xmegatron_trainval.md`](../../tutorials/qwen3_8b_xmegatron_trainval.md) — [`source summary`](../sources/qwen3_8b_xmegatron_trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`qwen3_8b_megatron_trainval.md`](../../tutorials/qwen3_8b_megatron_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT | Megatron, wandb | BF16 | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| [`qwen3_8b_xmegatron_trainval.md`](../../tutorials/qwen3_8b_xmegatron_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval | Megatron, wandb, XMegatron | Not explicitly stated | Not explicitly extracted |

## Related wiki pages

- [`Container-and-XPU-runtime`](../concepts/Container-and-XPU-runtime.md)
- [`LLM-training`](../concepts/LLM-training.md)
- [`Memory-pressure`](../concepts/Memory-pressure.md)
- [`VLM-VLA`](../concepts/VLM-VLA.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial.
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
