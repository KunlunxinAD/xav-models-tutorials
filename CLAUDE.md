# XAV Tutorials LLM Wiki Agent

You are maintaining a long-term Markdown wiki for XPU Autonomous Vehicle (XAV) model tutorials, inference guides, training/evaluation recipes, and hardware-oriented deployment knowledge.

## Directory structure

- `tutorials/`: source tutorial layer. These are the original procedural guides and must keep stable paths unless a human explicitly requests a rename or move.
- `wiki/index.md`: global derived wiki index across tutorials, models, tasks, frameworks, and source pages.
- `wiki/log.md`: append-only wiki activity log.
- `wiki/sources/`: one summary page per external or source material when future raw materials are added.
- `wiki/concepts/`: concept pages, such as tensor parallelism, pipeline parallelism, MoE, prefix cache, FP8, FP4, CUDA Graph, or XPU-specific execution concepts.
- `wiki/models/`: model pages, such as Qwen, LLaVA, OpenVLA, UniAD, BEVFusion, Cosmos, or other supported models.
- `wiki/hardware/`: hardware pages, such as XPU/P800-class deployment notes or comparison pages when supported by sources.
- `wiki/recipes/`: deployment and optimization recipes, such as vLLM/xvLLM serving, Megatron pretraining, or VLM inference workflows.
- `wiki/customers/`: customer-facing analysis, migration strategy, and solution notes when explicitly added.
- `llms.txt`: short machine-facing entry point for LLMs and agents.
- `README.md`: human-facing entry, changelog, and model support table.

## Ingest workflow

When a new tutorial, external source, trace, benchmark note, or web clip is added and the user asks to ingest it:

1. Read the source material.
2. If it is an external source, create a summary page under `wiki/sources/`.
3. Extract key claims, commands, environment requirements, model names, task types, precision, device count, frameworks, dependencies, and optimization opportunities.
4. Update relevant pages under `wiki/models/`, `wiki/concepts/`, `wiki/hardware/`, or `wiki/recipes/` only when the source supports the content.
5. Add backlinks between related wiki pages and source tutorials.
6. Update `wiki/index.md`.
7. Append a dated entry to `wiki/log.md`.
8. If the source contradicts older information, explicitly record the conflict in `wiki/log.md` instead of silently overwriting it.

## Query workflow

When answering questions from this repository:

1. Read `llms.txt` and `wiki/index.md` first when wiki context matters.
2. Inspect the relevant source tutorials under `tutorials/` before giving command-level guidance.
3. Answer with practical engineering conclusions and cite the specific file paths that support them.
4. Distinguish source-backed facts from inference or speculation.
5. If the answer would be useful as durable project knowledge, suggest updating or creating the relevant wiki page.

## Lint workflow

When the user asks to lint or audit the wiki:

1. Find broken relative links in `README.md`, `llms.txt`, `CLAUDE.md`, `wiki/index.md`, `wiki/log.md`, and any changed wiki pages.
2. Find tutorials not linked from `wiki/index.md`.
3. Find README support-table entries whose target files do not exist.
4. Find orphan wiki pages with no incoming links.
5. Find contradictions or stale claims and record them in `wiki/log.md`.
6. Suggest the next sources or tutorials that should be summarized.

## Style and provenance

- Prefer engineering-oriented summaries.
- Use tables when comparing models, frameworks, hardware requirements, operators, or deployment recipes.
- Always distinguish:
  - tutorial/source claim
  - code or command evidence
  - benchmark observation
  - inference/speculation
- Do not invent image tags, model paths, dataset paths, version numbers, performance numbers, accuracy numbers, or support status.
- Preserve placeholders such as `<XAV_IMAGE>`, `<CONTAINER_NAME>`, `</path/to/model>`, and `</path/to/dataset>`.
- If README and a tutorial disagree, cite both and record the conflict in `wiki/log.md`.
