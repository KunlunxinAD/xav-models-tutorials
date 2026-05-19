# Source Summary: xav-vLLM

## Source

- [`xav_vLLM.md`](../../tutorials/xav_vLLM.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | xav-vLLM |
| Domain | General |
| Workflow tags | Inference, Benchmark |
| Frameworks / backends | vLLM, xav-vLLM |
| Precision mentions | FP16 |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # xav-vLLM
- ## 获取镜像
- ## 准备环境
- ### 启动容器
- ### 配置容器内环境
- ### 下载模型权重
- ## 在线推理
- ### 启动服务
- ### 单例测试
- ## 离线推理
- ## Benchmark
- ### 1.Online Benchmark
- #### 1.1启动 the vLLM server
- #### 1.2执行测试
- #### 1.3结果

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `PATH_TO_MOUNT` | `</path/to/workspace> #本地路径` |
| `PATH_AFTER_MOUNT` | `/workspace #挂载后在容器内的路径` |
| `DATASET_PATH` | `</path/to/data>` |

## Representative commands

- `docker run -dti ${DOCKER_DEVICE_CONFIG} \`
- `docker start ${CONTAINER_NAME}`
- `docker exec -it ${CONTAINER_NAME} /bin/bash`
- `conda activate python310_torch25_cuda`
- `wget https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/v1.5.0/xpytorch_c54203a3/xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `bash xpytorch-cp310-torch251-ubuntu2004-x64.run`
- `pip install vllm==0.11.0 --no-build-isolation --no-deps`
- `git clone https://github.com/KunlunxinAD/xav-vLLM.git`
- `cd xav-vLLM`
- `pip install -r requirements.txt`
- `pip install fastapi==0.112.1`
- `pip install uvicorn==0.32.1`

## URLs mentioned

- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/v1.5.0/xpytorch_c54203a3/xpytorch-cp310-torch251-ubuntu2004-x64.run>
- <https://github.com/KunlunxinAD/xav-vLLM.git>
- <https://klx-sdk-release-public.su.bcebos.com/xav/xav_vllm/v0.11.0/20260123/xtorch_ops-0.1.2423%2Bc3c56b47-cp310-cp310-linux_x86_64.whl?authorization=bce-auth-v1%2FALTAKtEKzwsuB12vSFtik94FkX%2F2026-01-27T08%3A34%3A34Z%2F2592000%2Fhost%2Fc444a6053d1bb4b5823b2be12438f38c7b85c4748e278cace3d7982222550a77>
- <https://cce-ai-models.bj.bcebos.com/v1/vllm-kunlun-0.11.0/triton-3.0.0%2Bb2cde523-cp310-cp310-linux_x86_64.whl>
- <https://cce-ai-models.bj.bcebos.com/XSpeedGate-whl/release_merge/20251219_152418/xspeedgate_ops-0.0.0-cp310-cp310-linux_x86_64.whl>
- <http://localhost:8356/v1/chat/completions>
- <https://i-blog.csdnimg.cn/direct/f17798fe5a6b4167aa6227fe2eaac2ea.pngg>
- <https://sail-moe.oss-cn-hangzhou.aliyuncs.com/yunlin/images/evalscope/doc/qwen_vl/perf.png>
- <https://docs.vllm.ai/en/stable/contributing/benchmarks.html>

## Related derived pages

- [`xav-vLLM`](../models/xav-vLLM.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
