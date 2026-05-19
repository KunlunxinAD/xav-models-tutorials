# Source Summary: Qwen2.5-VL Trainval Guide

## Source

- [`qwen2.5vl_trainval.md`](../../tutorials/qwen2.5vl_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | Qwen2.5-VL |
| Domain | LLM/VLM/VLA |
| Workflow tags | Pretrain, Trainval, SFT, LoRA |
| Frameworks / backends | flash_attn, LlamaFactory |
| Precision mentions | Not explicitly stated |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |
| Dataset hints | coco |

## Source outline

- # Qwen2.5-VL Trainval Guide
- ## 环境准备
- ### 准备开发环境镜像
- ### PCIe环境配置（OAM跳过此步骤）
- #### 确定PCIe环境
- #### 配置PCIe的8卡互联模式
- ## 数据集及代码准备
- ### 数据集准备
- ### 下载代码及预训练权重
- ## 启动容器
- ### 容器内环境配置
- # 单机多卡训练
- ### 设置训练路径与参数
- ### 执行Lora
- ### 执行sft
- # 多机多卡训练
- ### 设置训练路径与参数
- ### 多机脚本示例

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `LLAMA_CONTAINER` | `Qwen2.5_VL_test` |
| `MODEL_PATH` | `</path/to/qwen2.5_vl> #本地路径` |
| `CUDA_VISIBLE_DEVICES` | `0,1,2,3,4,5,6,7` |
| `CUDART_DUMMY_REGISTER` | `1` |
| `XMLIR_BMM_DISPATCH_VALUE` | `2` |
| `XMLIR_ENABLE_LINEAR_FC_FUSION` | `1` |
| `USE_FAST_BF16_FC` | `true` |
| `XMLIR_ENABLE_FAST_FC` | `1` |
| `XPYTORCH_RUN_ENHANCE` | `1` |
| `DISABLE_VERSION_CHECK` | `1` |
| `XDNN_USE_FAST_SWISH` | `1` |
| `XDNN_FAST_DIV_SCALAR` | `true` |
| `XPUAPI_SDNN_BF16_ROUND_MODE` | `3` |
| `CUDA_DEVICE_ORDER` | `OAM_ID` |
| `BKCL_SOCKET_IFNAME` | `ens12f1np1` |
| `BKCL_ENABLE_XDR` | `1` |
| `BKCL_TREE_THRESHOLD` | `0` |
| `BKCL_RDMA_NICS` | `ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1,ens12f1np1` |
| `BKCL_RDMA_PROXY_DISABLE` | `1` |

## Representative commands

- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/dataset_info.json`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/mllm_rec_json.json`
- `cd /home`
- `git lfs install`
- `git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git`
- `git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-7B-Instruct.git`
- `git clone https://github.com/hiyouga/LLaMA-Factory.git`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/qwen2.5_VL.zip`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `cd LLaMA-Factory`

## URLs mentioned

- <https://github.com/hiyouga/LLaMA-Factory?tab=readme-ov-file#provided-datasets>
- <https://llamafactory.readthedocs.io/en/latest/getting_started/data_preparation.html>
- <https://cocodataset.org/#download>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/dataset_info.json>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/mllm_rec_json.json>
- <https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git>
- <https://www.modelscope.cn/Qwen/Qwen2.5-VL-7B-Instruct.git>
- <https://github.com/hiyouga/LLaMA-Factory.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/qwen2.5_VL.zip>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/dist_output.tar.gz>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/rotary.py>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/code/qwen2.5-vl/change_runtime.sh>

## Related derived pages

- [`Qwen2.5-VL`](../models/Qwen2.5-VL.md)
- [`llm-vlm-sft-lora`](../recipes/llm-vlm-sft-lora.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
