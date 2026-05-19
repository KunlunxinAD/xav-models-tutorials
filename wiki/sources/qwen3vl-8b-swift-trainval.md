# Source Summary: Qwen3-VL-8B MS-Swift Trainval Guide

## Source

- [`tutorials/qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md)

## Summary

This source tutorial describes Qwen3-VL-8B-Instruct LoRA SFT using MS-Swift on a single 8-device XPU setup.

## Extracted facts

| Category | Source-backed details |
| --- | --- |
| Model | `Qwen/Qwen3-VL-8B-Instruct` from Hugging Face |
| Dataset example | COCO via `detection-datasets/coco` |
| Framework | MS-Swift from GitHub, checkout `v3.12.6` |
| Container | Mounts `${MODEL_PATH}` to `/home`, exposes `/dev/xpu0` through `/dev/xpu7`, uses `--shm-size 256g` |
| Environment | `conda activate python310_torch25_cuda`; `pip install -e .`; installs `qwen_vl_utils` and `wandb` |
| Training command | `swift sft` with LoRA, Deepspeed zero2, BF16, flash attention, W&B reporting |

## Derived pages updated

- [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md)
- [`MS-Swift`](../concepts/MS-Swift.md)
- [`VLM-VLA`](../concepts/VLM-VLA.md)
- [`Memory-pressure`](../concepts/Memory-pressure.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)
- [`qwen3-vl-8b-ms-swift-lora-sft`](../recipes/qwen3-vl-8b-ms-swift-lora-sft.md)

## Open questions

- Container variable naming should be verified: the tutorial sets `LLAMA_CONTAINER` but uses `${XAV_CONTAINER}` in the `docker exec` command.
- The source does not provide benchmark, memory, accuracy, or convergence results.
- GRPO is not covered by this source.
