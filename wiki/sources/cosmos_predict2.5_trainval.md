# Source Summary: cosmos-predict2.5

## Source

- [`cosmos_predict2.5_trainval.md`](../../tutorials/cosmos_predict2.5_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | cosmos-predict2.5 |
| Domain | World Model |
| Workflow tags | Inference, Trainval |
| Frameworks / backends | Megatron |
| Precision mentions | FP16 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES='0,1,2,3,4,5,6,7; CUDA_VISIBLE_DEVICES=7 |
| Dataset hints | HuggingFace |

## Source outline

- # cosmos-predict2.5
- ## 准备环境
- ## 启动容器
- ## 配置容器环境
- ## 下载及安装资源
- ### 下载cosmos-predict25代码
- ### 注入patch
- ### 安装依赖
- ### 权重下载
- ## 训练与推理
- ### Auto Multiview
- #### 准备数据集
- #### 单机8卡训练
- #### 单机8卡推理
- ### Robot Action-Conditioned
- #### 准备数据集
- #### 单机单卡训练
- #### 单机8卡训练
- #### 单机单卡推理
- ###  Video2World Post-training for DreamGen Bench
- #### 准备数据集
- #### 单机单卡训练
- #### 单机8卡训练

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `XAV_CONTAINER` | `xav-test-cosmos-predict25` |
| `MODEL_PATH` | `</path/to/model>` |
| `HF_ENDPOINT` | `https://hf-mirror.com` |

## Representative commands

- `docker run -itd --privileged --net=host \`
- `docker exec -it ${XAV_CONTAINER} bash`
- `git lfs install`
- `cd /home`
- `git clone https://github.com/nvidia-cosmos/cosmos-predict2.5.git`
- `git lfs pull`
- `cd /home/cosmos-predict2.5`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos_predict25/support_xpu_0418.patch`
- `git apply --whitespace=nowarn support_xpu_0418.patch`
- `pip install -e ./packages/cosmos-cuda`
- `pip install -e ./packages/cosmos-oss`
- `pip install megatron-core==0.14.0 --no-deps`

## URLs mentioned

- <https://github.com/nvidia-cosmos/cosmos-predict2.5.git>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos_predict25/support_xpu_0418.patch>
- <https://github.com/jeanachoi/Video-Depth-Anything.git>
- <https://hf-mirror.com>
- <https://github.com/nvidia-cosmos/cosmos-predict2.5/blob/main/docs/post-training_multiview.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos_predict25/dcp.patch>
- <https://github.com/nvidia-cosmos/cosmos-predict2.5/blob/main/docs/post-training_video2world_action.md>
- <https://github.com/nvidia-cosmos/cosmos-predict2.5/blob/main/docs/post-training_video2world_gr00t.md>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/cosmos_predict25/model_loader.patch>

## Related derived pages

- [`cosmos-predict2.5`](../models/cosmos-predict2.5.md)
- [`llm-inference`](../recipes/llm-inference.md)
- [`world-model-trainval`](../recipes/world-model-trainval.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
