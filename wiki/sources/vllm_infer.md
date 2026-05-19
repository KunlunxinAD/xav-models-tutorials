# Source Summary: vLLM infer Guide

## Source

- [`vLLM_infer.md`](../../tutorials/vLLM_infer.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | vLLM |
| Domain | General |
| Workflow tags | Inference, Pretrain, Trainval |
| Frameworks / backends | vLLM, xvLLM |
| Precision mentions | FP16 |
| Device hints | 8 XPU device entries (/dev/xpu0, /dev/xpu1, /dev/xpu2...); CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 |
| Dataset hints | Not explicitly extracted |

## Source outline

- # vLLM infer Guide
- ## 环境准备
- ### PCIe环境配置（OAM跳过此步骤）
- #### 确定PCIe环境
- #### 配置PCIe的8卡互联模式
- ## 代码准备
- ### 下载代码及预训练权重
- ## 启动容器
- ### 容器内环境配置
- ## 单机online模式推理

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `XAV_IMAGE` | `<XAV_IMAGE>` |
| `NAME_CONTAINER` | `alpha_drive` |
| `MODEL_PATH` | `</path/to/Qwen2.5-VL-3B-Instruct> #本地路径` |
| `XMLIR_ENABLE_MOCK_TORCH_COMPILE` | `false` |
| `XMLIR_FORCE_USE_XPU_GRAPH` | `1` |
| `VLLM_USE_V1` | `0` |
| `HTTP_PROXY` | `http://agent.baidu.com:8891` |
| `HTTPS_PROXY` | `http://agent.baidu.com:8891` |
| `http_proxy` | `http://10.162.37.16:8128` |
| `https_proxy` | `http://10.162.37.16:8128` |

## Representative commands

- `cd /home`
- `git lfs install`
- `git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git`
- `docker run -itd --privileged --net=host \`
- `docker exec -it ${NAME_CONTAINER} bash`
- `conda activate python310_torch25_cuda`
- `pip install transformers==4.51.0`
- `pip install vllm==0.7.2`
- `pip install vllm==0.8.2 --no-deps`
- `wget https://klx-sdk-release-public.su.bcebos.com/xvllm/KL3/0.8.2/latest/output.tar.gz`
- `cd xvllm_0.8.2`
- `bash scripts/install_output.sh`

## URLs mentioned

- <https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git>
- <https://klx-sdk-release-public.su.bcebos.com/xvllm/KL3/0.8.2/latest/output.tar.gz>
- <http://localhost:8802/v1/chat/completions>
- <https://upload.wikimedia.org/wikipedia/commons/9/98/Horse-and-pony.jpg>
- <https://upload.wikimedia.org/wikipedia/commons/6/69/Grapevinesnail_01.jpg>
- <https://upload.wikimedia.org/wikipedia/commons/3/30/George_the_amazing_guinea_pig.jpg>
- <https://duguang-labelling.oss-cn-shanghai.aliyuncs.com/qiansun/video_ocr/videos/50221078283.mp4>
- <http://agent.baidu.com:8891>
- <http://10.162.37.16:8128>

## Related derived pages

- [`vLLM`](../models/vLLM.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
