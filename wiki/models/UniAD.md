# UniAD

## Sources

- [`UniAD_trainval.md`](../../tutorials/UniAD_trainval.md) — [`source summary`](../sources/uniad_trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`UniAD_trainval.md`](../../tutorials/UniAD_trainval.md) | Autonomous Driving | Trainval | MMCV | Not explicitly stated | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7 |

## Related wiki pages

- [`Autonomous-driving-trainval`](../concepts/Autonomous-driving-trainval.md)
- [`Benchmark-and-evaluation`](../concepts/Benchmark-and-evaluation.md)
- [`Container-and-XPU-runtime`](../concepts/Container-and-XPU-runtime.md)
- [`Distributed-training`](../concepts/Distributed-training.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial.
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
