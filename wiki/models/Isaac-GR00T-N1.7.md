# Isaac-GR00T-N1.7

## Sources

- [`Isaac-GR00T-N1.7_trainval.md`](../../tutorials/Isaac-GR00T-N1.7_trainval.md) — [`source summary`](../sources/isaac-groot-n1.7_trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`Isaac-GR00T-N1.7_trainval.md`](../../tutorials/Isaac-GR00T-N1.7_trainval.md) | LLM/VLM/VLA | Trainval, SFT, Inference | Lerobot, Deepspeed, flash_attn, transformers, peft, wandb | Not explicitly stated | 8 XPU device entries; `CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7`; `NUM_GPUS=8` |

## Related wiki pages

- [`Container-and-XPU-runtime`](../concepts/Container-and-XPU-runtime.md)
- [`LLM-training`](../concepts/LLM-training.md)
- [`Memory-pressure`](../concepts/Memory-pressure.md)
- [`VLM-VLA`](../concepts/VLM-VLA.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial.
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
