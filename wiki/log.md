# LLM Wiki Log

Append new entries at the top. Keep this file factual: record what changed, which source files were used, and what remains unresolved.

## 2026-07-08 — Ingest Point Transformer V3 (PTv3) trainval tutorial

### Sources

- `tutorials/Ptv3_Trainval_Guide.md` (newly added)

### Changed

- Added source tutorial `tutorials/Ptv3_Trainval_Guide.md` (Point Transformer V3, nuScenes semseg trainval on Pointcept).
- Added source summary `wiki/sources/ptv3_trainval.md`.
- Added model page `wiki/models/PTv3.md`.
- Registered PTv3 in `wiki/index.md` (source summaries, model pages, Trainval task section, Autonomous Driving domain section, all-source-tutorials table).
- Added PTv3 rows to `wiki/recipes/autonomous-driving-trainval.md` and `wiki/concepts/Autonomous-driving-trainval.md`.
- Added a README changelog entry and an E2E AD Models table row (rowspan 18 → 19).

### Open questions

- Precision is not explicitly declared as a single format; the tutorial applies an `mmcv_amp_fp16` patch and sets a BF16 round-mode env, so the README table marks FP32/FP16 as inference from the AMP patch rather than an explicit source claim.
- Benchmark, peak memory, throughput, and accuracy are not stated and remain unknown.

### Follow-up (2026-07-08)

- Maintainer confirmed the two training-env lines were typos. Fixed in the tutorial: `export export XDNN_USE_FAST_GELU=1` → `export XDNN_USE_FAST_GELU=1`, and `export XMLIR_ENABLE_FAST_FC=1wanqu` → `export XMLIR_ENABLE_FAST_FC=1`. Updated source summary and model page accordingly.


## 2026-06-19 — Update wiki after GR00T and Qwen3-Omni tutorial refresh

### Sources

- `tutorials/GR00T-Dreams_trainval.md`
- `tutorials/Isaac-GR00T-N1.7_trainval.md`
- `tutorials/qwen3_omni_30b_a3b_infer.md`

### Changed

- Added source summaries for GR00T-Dreams and Isaac-GR00T-N1.7 under `wiki/sources/`.
- Added model pages for GR00T-Dreams and Isaac-GR00T-N1.7 under `wiki/models/`.
- Refreshed the Qwen3-Omni-30B-A3B source/model pages to match the `omni_020` tutorial content and evalscope benchmark flow.
- Updated `wiki/index.md`, LLM/VLM concept pages, and LLM/VLM recipes with the newly supported tutorials.

### Open questions

- Confirmed by maintainer: `tutorials/Isaac-GR00T-N1.7_trainval.md` should use `XAV_CONTAINER`; tutorial and derived wiki pages were normalized accordingly.
- README lists FP16/BF16 for the GR00T tutorials, but their source command sections do not explicitly state precision; keep precision claims source-scoped.
- GR00T-Dreams has a “训练与推理” section title, but the extracted commands are single-card and 8-card training commands; no standalone inference command is recorded in the current source summary.

## 2026-05-19 — Bulk ingest all tutorials into wiki

### Sources

- Processed 64 tutorial files under `tutorials/`.

### Changed

- Refreshed `wiki/index.md` with full source summary, model, concept, recipe, task, and domain indexes.
- Generated or updated 64 source summary pages under `wiki/sources/`.
- Generated or updated 56 model/topic pages under `wiki/models/`.
- Generated or updated 12 concept pages under `wiki/concepts/`.
- Generated or updated 7 recipe pages under `wiki/recipes/`.

### Open questions

- Many tutorials do not explicitly state benchmark, peak memory, throughput, accuracy, or exact image tags; keep those fields source-backed only.
- Some tutorials contain placeholder paths and environment variables; preserve placeholders when answering.
- If future manual review finds conflicting README/tutorial metadata, record the conflict here before normalizing pages.

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
