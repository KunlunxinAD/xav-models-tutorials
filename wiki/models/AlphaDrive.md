# AlphaDrive

## Sources

- [`AlphaDrive_trainval.md`](../../tutorials/AlphaDrive_trainval.md) — [`source summary`](../sources/alphadrive_trainval.md)
- [`TRL_AlphaDrive_trainval.md`](../../tutorials/TRL_AlphaDrive_trainval.md) — [`source summary`](../sources/trl_alphadrive_trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`AlphaDrive_trainval.md`](../../tutorials/AlphaDrive_trainval.md) | Autonomous Driving | Pretrain, Trainval, GRPO | Deepspeed, flash_attn, vLLM, wandb, xvLLM | BF16 | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |
| [`TRL_AlphaDrive_trainval.md`](../../tutorials/TRL_AlphaDrive_trainval.md) | Autonomous Driving | Pretrain, Trainval, SFT, GRPO | Deepspeed, flash_attn, vLLM, xvLLM | Not explicitly stated | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...) |

## Related wiki pages

- [`Autonomous-driving-trainval`](../concepts/Autonomous-driving-trainval.md)
- [`Benchmark-and-evaluation`](../concepts/Benchmark-and-evaluation.md)
- [`Container-and-XPU-runtime`](../concepts/Container-and-XPU-runtime.md)
- [`Distributed-training`](../concepts/Distributed-training.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial.
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
