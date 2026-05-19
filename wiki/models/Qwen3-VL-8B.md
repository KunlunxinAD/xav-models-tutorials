# Qwen3-VL-8B

## Sources

- [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md)

## Summary

Qwen3-VL-8B is covered in this repository through a single-machine multi-card MS-Swift LoRA SFT tutorial for `Qwen3-VL-8B-Instruct`.

## Source-backed facts

| Item | Value | Source |
| --- | --- | --- |
| Model weights | `Qwen/Qwen3-VL-8B-Instruct` from Hugging Face | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Framework | MS-Swift checked out at `v3.12.6` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Dataset example | COCO from `detection-datasets/coco` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Training workflow | `swift sft` with `--train_type lora` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Distributed setup | `NPROC_PER_NODE=8`, `CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Precision | `--torch_dtype bfloat16`; source also notes changing parameters for fp16 if needed | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Deepspeed mode | `--deepspeed zero2` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |

## Related wiki pages

- [`MS-Swift`](../concepts/MS-Swift.md)
- [`VLM-VLA`](../concepts/VLM-VLA.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)
- [`Memory-pressure`](../concepts/Memory-pressure.md)
- [`qwen3-vl-8b-ms-swift-lora-sft`](../recipes/qwen3-vl-8b-ms-swift-lora-sft.md)

## Engineering notes

- The source tutorial uses environment variables associated with XPU execution and optimization, including `XPYTORCH_RUN_ENHANCE`, `XMLIR_ENABLE_LINEAR_FC_FUSION`, `XMLIR_ENABLE_FAST_FC`, `XDNN_USE_FAST_SWISH`, and `XPUAPI_SDNN_BF16_ROUND_MODE`.
- The source tutorial sets `MAX_PIXELS=1003520`, `--max_length 2048`, `--per_device_train_batch_size 10`, and `PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True`; treat these as source-provided training parameters, not general capacity claims.
- The tutorial does not provide benchmark, accuracy, memory usage, or throughput numbers.

## Open questions

- The tutorial does not state final accuracy, convergence behavior, peak memory usage, or performance throughput.
- GRPO is not part of this source tutorial; see other source files before adding GRPO-specific claims for Qwen3-VL-8B.
