# GR00T-Dreams

## Sources

- [`GR00T-Dreams_trainval.md`](../../tutorials/GR00T-Dreams_trainval.md) — [`source summary`](../sources/groot-dreams_trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`GR00T-Dreams_trainval.md`](../../tutorials/GR00T-Dreams_trainval.md) | LLM/VLM/VLA | Trainval, SFT | diffusers, transformers, accelerate, tensorboard | Not explicitly stated | 8 XPU device entries; `CUDA_VISIBLE_DEVICES=0`; `CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7` |

## Related wiki pages

- [`Container-and-XPU-runtime`](../concepts/Container-and-XPU-runtime.md)
- [`LLM-training`](../concepts/LLM-training.md)
- [`VLM-VLA`](../concepts/VLM-VLA.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial.
- The source contains training commands but no standalone inference command in the extracted sections.
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
