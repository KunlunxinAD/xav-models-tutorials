# LLM Wiki Log

Append new entries at the top. Keep this file factual: record what changed, which source files were used, and what remains unresolved.

## 2026-05-19 — Remove root WIKI compatibility files

### Changed

- Removed obsolete root compatibility files: `WIKI_INDEX.md`, `WIKI_LOG.md`, and `WIKI_MAINTENANCE.md`.
- Kept active wiki files under `wiki/` and Agent rules in `CLAUDE.md`.
- Updated `llms.txt` and `CLAUDE.md` to remove compatibility-file guidance.

### Source layer preserved

- Existing `tutorials/` source tutorials were not moved or renamed.

## 2026-05-19 — Ingest Qwen3-VL-8B MS-Swift trainval tutorial

### Sources

- `tutorials/qwen3vl_8b_swift_trainval.md`

### Changed

- Updated `wiki/index.md` with derived source, model, concept, and recipe links.
- Added `wiki/sources/qwen3vl-8b-swift-trainval.md`.
- Added `wiki/models/Qwen3-VL-8B.md`.
- Added `wiki/concepts/MS-Swift.md`.
- Added `wiki/concepts/VLM-VLA.md`.
- Added `wiki/concepts/Memory-pressure.md`.
- Added `wiki/concepts/XPU-training-adaptation.md`.
- Added `wiki/recipes/qwen3-vl-8b-ms-swift-lora-sft.md`.

### Source-backed facts extracted

- The tutorial covers `Qwen/Qwen3-VL-8B-Instruct` LoRA SFT with MS-Swift `v3.12.6`.
- The example dataset is COCO from `detection-datasets/coco`.
- The source command uses `swift sft`, `--train_type lora`, `--deepspeed zero2`, `--torch_dtype bfloat16`, `NPROC_PER_NODE=8`, and 8 visible devices.

### Open questions

- The tutorial sets `LLAMA_CONTAINER=Qwen3VL_test` but later uses `${XAV_CONTAINER}` in `docker exec`; verify the intended container variable before copying the command.
- The source does not provide benchmark, memory, accuracy, throughput, or convergence results.
- GRPO is not covered by this source.

## 2026-05-19 — Standard wiki directory migration

### Changed

- Added `wiki/` as the standard derived wiki directory.
- Moved the active derived index to `wiki/index.md`.
- Moved the active append-only log to `wiki/log.md`.
- Added standard wiki subdirectories: `wiki/sources/`, `wiki/concepts/`, `wiki/models/`, `wiki/hardware/`, `wiki/recipes/`, and `wiki/customers/`.
- Kept root `WIKI_INDEX.md`, `WIKI_LOG.md`, and `WIKI_MAINTENANCE.md` as compatibility entry points.

### Source layer preserved

- Existing `tutorials/` source tutorials were not moved or renamed.

## 2026-05-19 — Initial LLM wiki layer

### Changed

- Added `llms.txt` as the short LLM/agent entry point.
- Added `WIKI_INDEX.md` as the derived content map over the existing tutorials.
- Added `WIKI_MAINTENANCE.md` with update, provenance, contradiction, and link-check rules.
- Updated `README.md` with an LLM Wiki entry section.

### Source layer preserved

- Existing root and `tutorials/` directory layout was preserved.
- No tutorial files were moved or renamed.

### Link issues resolved in README

- `tutorials/Yolo11_inference.md` now points to existing `tutorials/Yolo_inference.md`.
- `tutorials/qwen3_8b_xmegatron_trainval` now points to existing `tutorials/qwen3_8b_xmegatron_trainval.md`.
- `tutorials/regnet_trainval` now points to existing `tutorials/regnet_trainval.md`.
- `tutorials/lansegnet_trainval.md` now points to existing `tutorials/lanesegnet_trainval.md`.

### Open questions

- Some tutorials are present in `tutorials/` but are not listed in the README model table. They are included in `WIKI_INDEX.md` as source tutorials without forcing unsupported metadata.
- README model names and tutorial titles are not always identical. `WIKI_INDEX.md` keeps source links explicit instead of normalizing away those differences.
