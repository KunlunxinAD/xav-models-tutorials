# Qwen3-VL-8B MS-Swift LoRA SFT Recipe

## Sources

- [`qwen3vl_8b_swift_trainval.md`](../../tutorials/qwen3vl_8b_swift_trainval.md)

## Purpose

Reusable source-backed workflow for running Qwen3-VL-8B-Instruct LoRA SFT with MS-Swift on a single machine using 8 visible devices.

## Workflow summary

| Step | Source-backed action |
| --- | --- |
| Prepare image | Contact the relevant owner to obtain the development image. |
| Prepare dataset | Use the MS-Swift custom dataset docs; the tutorial example clones `detection-datasets/coco`. |
| Prepare weights | Clone `https://huggingface.co/Qwen/Qwen3-VL-8B-Instruct` under `/home`. |
| Prepare framework | Clone `https://github.com/modelscope/ms-swift.git` and checkout `v3.12.6`. |
| Start container | Mount `${MODEL_PATH}` to `/home`, expose `/dev/xpu0` through `/dev/xpu7`, set `--shm-size 256g`. |
| Configure environment | Activate `python310_torch25_cuda`, install MS-Swift editable, install `qwen_vl_utils` and `wandb`. |
| Run training | Execute a local script such as `test_qwen3_vl.sh` containing the source `swift sft` command. |

## Key command parameters

| Parameter | Source value |
| --- | --- |
| Model | `/home/Qwen3-VL-8B-Instruct` |
| Train type | `lora` |
| Deepspeed | `zero2` |
| Dataset example | `/home/coco#3000` |
| Dtype | `bfloat16` |
| Epochs | `1` |
| Per-device train batch size | `10` |
| Learning rate | `1e-5` |
| Max steps | `500` |
| Max length | `2048` |
| Attention implementation | `flash_attn` |
| Output dir | `output` |
| Reporting | `wandb` |

## Related pages

- [`Qwen3-VL-8B`](../models/Qwen3-VL-8B.md)
- [`MS-Swift`](../concepts/MS-Swift.md)
- [`VLM-VLA`](../concepts/VLM-VLA.md)
- [`Memory-pressure`](../concepts/Memory-pressure.md)
- [`XPU-training-adaptation`](../concepts/XPU-training-adaptation.md)

## Guardrails

- Preserve placeholders such as `<XAV_IMAGE>` and `</path/to/qwen3_vl>` when adapting the command.
- The source tutorial contains `export LLAMA_CONTAINER=Qwen3VL_test` but later runs `docker exec -it ${XAV_CONTAINER} bash`; verify the intended container variable before copying into production scripts.
- The tutorial does not provide throughput, memory, accuracy, or convergence results.
