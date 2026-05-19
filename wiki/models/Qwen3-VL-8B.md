# Qwen3-VL-8B

## Sources

- [`qwen3vl_8b_grpo_verl_trainval.md`](../../tutorials/qwen3vl_8b_grpo_verl_trainval.md) — [`source summary`](../sources/qwen3vl_8b_grpo_verl_trainval.md)
- [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) — [`source summary`](../sources/qwen3vl-8b-swift-trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`qwen3vl_8b_grpo_verl_trainval.md`](../../tutorials/qwen3vl_8b_grpo_verl_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, GRPO | verl | Not explicitly stated | Not explicitly extracted |
| [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) | LLM/VLM/VLA | Pretrain, Trainval, SFT, LoRA | Deepspeed, flash_attn, MS-Swift, wandb | BF16 | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); NPROC_PER_NODE=8; CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |

## Related wiki pages

- [`Container-and-XPU-runtime`](../concepts/Container-and-XPU-runtime.md)
- [`LLM-training`](../concepts/LLM-training.md)
- [`Memory-pressure`](../concepts/Memory-pressure.md)
- [`VLM-VLA`](../concepts/VLM-VLA.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial.
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
