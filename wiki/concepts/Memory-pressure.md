# Memory Pressure

## Sources

- [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md)

## Summary

The Qwen3-VL-8B MS-Swift tutorial includes several parameters that affect training memory pressure, but it does not report measured memory usage.

## Source-backed controls

| Control | Source value | Why it matters |
| --- | --- | --- |
| Device count | `NPROC_PER_NODE=8` | Defines the single-node multi-card training topology in the tutorial. |
| Precision | `--torch_dtype bfloat16` | BF16 can reduce activation/parameter memory relative to FP32. |
| Deepspeed | `--deepspeed zero2` | ZeRO-2 partitions optimizer states and gradients. |
| Batch size | `--per_device_train_batch_size 10` | Per-device batch size directly affects activation memory. |
| Sequence length | `--max_length 2048` | Longer multimodal sequences increase memory pressure. |
| Image budget | `MAX_PIXELS=1003520` | Vision tokenization workload depends on the pixel budget. |
| Allocator config | `PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True` | Source-provided allocator setting; do not treat it as a measured fix without benchmarks. |

## Related pages

- [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md)
- [`VLM-VLA`](VLM-VLA.md)
- [`qwen3-vl-8b-ms-swift-lora-sft`](../recipes/qwen3-vl-8b-ms-swift-lora-sft.md)

## Missing evidence

- Peak memory usage is not reported.
- Throughput is not reported.
- The tutorial does not compare BF16 and FP16 memory behavior.
