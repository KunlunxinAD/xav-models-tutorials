# vLLM

## Sources

- [`vLLM_infer.md`](../../tutorials/vLLM_infer.md) — [`source summary`](../sources/vllm_infer.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`vLLM_infer.md`](../../tutorials/vLLM_infer.md) | General | Inference, Pretrain, Trainval | vLLM, xvLLM | FP16 | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |

## Related wiki pages

- [`Benchmark-and-evaluation`](../concepts/Benchmark-and-evaluation.md)
- [`LLM-inference`](../concepts/LLM-inference.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial.
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
