# MS-Swift

## Sources

- [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md)

## Summary

MS-Swift appears in this repository as the framework used for Qwen3-VL-8B LoRA SFT.

## Source-backed usage

| Item | Value | Source |
| --- | --- | --- |
| Repository | `https://github.com/modelscope/ms-swift.git` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Version | `v3.12.6` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Install command | `pip install -e .` after entering `ms-swift` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Training command | `swift sft` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| Fine-tuning mode | `--train_type lora` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |

## Related pages

- [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md)
- [`qwen3-vl-8b-ms-swift-lora-sft`](../recipes/qwen3-vl-8b-ms-swift-lora-sft.md)

## Notes for maintainers

- Do not generalize the `v3.12.6` version beyond the Qwen3-VL-8B source unless another tutorial confirms it.
- If future tutorials use MS-Swift for other models, add them to the source table above.
