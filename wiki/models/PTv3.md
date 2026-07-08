# PTv3

## Sources

- [`Ptv3_Trainval_Guide.md`](../../tutorials/Ptv3_Trainval_Guide.md) — [`source summary`](../sources/ptv3_trainval.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`Ptv3_Trainval_Guide.md`](../../tutorials/Ptv3_Trainval_Guide.md) | Autonomous Driving | Trainval | Pointcept, MMCV | AMP/FP16 (mmcv_amp_fp16 patch), BF16 round mode env | 4 cards (`train.sh -g 4`, `CUDA_VISIBLE_DEVICES=0,1,2,3`) |

## Related wiki pages

- [`Autonomous-driving-trainval`](../concepts/Autonomous-driving-trainval.md)
- [`Benchmark-and-evaluation`](../concepts/Benchmark-and-evaluation.md)
- [`Container-and-XPU-runtime`](../concepts/Container-and-XPU-runtime.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial.
- The source training-environment block contains two probable typos (`export export XDNN_USE_FAST_GELU=1` and `XMLIR_ENABLE_FAST_FC=1wanqu`); verify with the maintainer before reuse. See [`source summary`](../sources/ptv3_trainval.md).
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
