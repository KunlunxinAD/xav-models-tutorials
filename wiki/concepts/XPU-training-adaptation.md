# XPU Training Adaptation

## Sources

- [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md)

## Summary

This page tracks source-backed XPU adaptation settings used by training tutorials. The Qwen3-VL-8B MS-Swift tutorial provides a concrete set of environment variables for single-machine 8-card LoRA SFT.

## Source-backed environment settings

| Category | Variables or settings | Source |
| --- | --- | --- |
| Device visibility | `CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7`, `NPROC_PER_NODE=8` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| XMLIR / FC optimization | `XMLIR_BMM_DISPATCH_VALUE=2`, `XMLIR_ENABLE_LINEAR_FC_FUSION=1`, `XMLIR_ENABLE_FAST_FC=1` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| BKCL topology | `BKCL_PCIE_TOPO=1` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| XPYTORCH / XDNN | `XPYTORCH_RUN_ENHANCE=1`, `XDNN_USE_FAST_SWISH=1`, `XDNN_FAST_DIV_SCALAR=true` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |
| BF16 behavior | `XPUAPI_SDNN_BF16_ROUND_MODE=3` | [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md) |

## Related pages

- [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md)
- [`MS-Swift`](MS-Swift.md)
- [`Memory-pressure`](Memory-pressure.md)
- [`qwen3-vl-8b-ms-swift-lora-sft`](../recipes/qwen3-vl-8b-ms-swift-lora-sft.md)

## Notes for maintainers

- Keep these as source-backed environment settings, not universal XPU recommendations.
- Add benchmark evidence before claiming that any individual variable improves performance or memory usage.
