# Qwen3-Omni-30B-A3B

## Sources

- [`qwen3_omni_30b_a3b_infer.md`](../../tutorials/qwen3_omni_30b_a3b_infer.md) — [`source summary`](../sources/qwen3_omni_30b_a3b_infer.md)

## Source-backed support matrix

| Tutorial | Domain | Workflow | Frameworks / backends | Precision mentions | Device hints |
| --- | --- | --- | --- | --- | --- |
| [`qwen3_omni_30b_a3b_infer.md`](../../tutorials/qwen3_omni_30b_a3b_infer.md) | LLM/VLM/VLA | Inference, Benchmark | vLLM, xvLLM, vLLM-Omni, xvLLM-Omni, evalscope | Not explicitly stated in tutorial command sections | `XPU_NUM=8`; dynamic `/dev/xpu*` device mapping; `CUDA_VISIBLE_DEVICES=6,7` for CUDA Graph examples |

## Related wiki pages

- [`Benchmark-and-evaluation`](../concepts/Benchmark-and-evaluation.md)
- [`Container-and-XPU-runtime`](../concepts/Container-and-XPU-runtime.md)
- [`LLM-inference`](../concepts/LLM-inference.md)
- [`Memory-pressure`](../concepts/Memory-pressure.md)
- [`VLM-VLA`](../concepts/VLM-VLA.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)

## Missing evidence to verify before making claims

- Do not infer benchmark, accuracy, memory usage, throughput, image tag, or exact software version unless it is stated in the source tutorial or captured benchmark output.
- README lists FP16 and `1 x 8`; the tutorial examples include `XPU_NUM=8` setup and `CUDA_VISIBLE_DEVICES=6,7` CUDA Graph runs, so cite the exact command context when answering device-count questions.
- If multiple tutorials for this model disagree, record the conflict in `wiki/log.md` before normalizing the model page.
