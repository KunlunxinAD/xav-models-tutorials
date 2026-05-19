# VLM / VLA

## Sources

- [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md)
- [`README.md`](../../README.md)

## Summary

This page groups vision-language and vision-language-action tutorials. Qwen3-VL-8B is currently represented by an MS-Swift LoRA SFT tutorial.

## Source-backed entries

| Model or tutorial | Workflow | Source |
| --- | --- | --- |
| Qwen3-VL-8B-Instruct | MS-Swift LoRA SFT, single-machine 8-card setup | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Qwen2.5-VL | Listed in README VLM/VLA section as SFT/LoRA | [`README.md`](../../README.md) |
| LLaVA | Listed in README VLM/VLA section as Pretrain/SFT | [`README.md`](../../README.md) |
| Pi0 | Listed in README VLM/VLA section as Pretrain/SFT | [`README.md`](../../README.md) |

## Engineering dimensions to track

- Image resolution or token budget, such as `MAX_PIXELS` and `--max_length` when stated by a source.
- Fine-tuning mode, such as LoRA, SFT, GRPO, or pretraining when stated by a source.
- Framework, such as MS-Swift, LlamaFactory, or custom training scripts.
- XPU-specific environment variables and precision settings.

## Related pages

- [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md)
- [`MS-Swift`](MS-Swift.md)
- [`Memory-pressure`](Memory-pressure.md)
- [`XPU-training-adaptation`](XPU-training-adaptation.md)
