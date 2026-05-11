# xvLLM General Inference Guide

> 本文档为 **xvLLM 镜像下的通用推理流程**，覆盖大多数常见模型的开箱即用场景。部分模型/部署形态（量化、MTP/DCP、PD 分离、特殊后端等）有专用脚本或专用文档，请优先参考对应模型的专用文档。

## 概述

本文档介绍如何使用 **xvLLM 推理镜像** 在昆仑芯 XPU 上进行通用模型推理。

xvLLM 镜像内置了一套开箱即用的推理脚本体系（位于容器内 `/workspace` 目录下），覆盖单机（`singlenode/`）、多机（`multinode/`）、PD 分离（`disaggregated_serving/`）等常见部署形态，并为大量常见模型（Qwen 系列、DeepSeek、GLM、MiniMax 等）预置了启动脚本。因此在大多数场景下，**只需准备好模型权重、修改脚本中的模型路径，即可启动推理服务**。

> 注意：
> - 本仓库中 `vLLM_infer.md`、`xav_vLLM.md` 等文档基于不同的镜像/后端，与本文档的 xvLLM 镜像不通用。
> - 本文档仅覆盖 xvLLM 镜像下的通用单机在线推理流程。若涉及特定模型的定制化配置（量化、MTP、DCP、PD 分离等）、多机部署或特殊后端，请参考对应模型的专用文档以及容器内 `/workspace/README.md`、`/workspace/singlenode/EXAMPLE.md`。

## 准备环境

请联系昆仑芯客户支持获取 **xvLLM 推理镜像**。

## 启动容器

> 以下 docker 启动脚本请根据实际机器环境（镜像 tag、挂载目录、XPU 卡数等）自行补充：

```bash
export XAV_IMAGE=<XAV_IMAGE>
export CONTAINER_NAME=xvllm_test
export PATH_TO_MOUNT=</path/to/mount> #本地路径
export PATH_AFTER_MOUNT=/home #挂载后在容器内的路径
export DATA_PATH=</path/to/dataset>

docker run -dti \
        --name ${CONTAINER_NAME} \
        --net=host \
        --security-opt=seccomp=unconfined \
        -e IMAGE_VERSION="${XAV_IMAGE}" \
        --cap-add=SYS_PTRACE \
        --shm-size=256g \
        --cap-add=SYS_ADMIN \
        --device /dev/fuse \
        --restart=always \
        --ulimit=memlock=-1 \
        --ulimit=nofile=120000 \
        --ulimit=stack=67108864 \
        -v ${PATH_TO_MOUNT}:${PATH_AFTER_MOUNT} \
        -v ${DATA_PATH}:/data \
        --cpuset-cpus=0-$((`nproc`-8)) \
        --device=/dev/xpu0:/dev/xpu0 \
        --device=/dev/xpu1:/dev/xpu1 \
        --device=/dev/xpu2:/dev/xpu2 \
        --device=/dev/xpu3:/dev/xpu3 \
        --device=/dev/xpu4:/dev/xpu4 \
        --device=/dev/xpu5:/dev/xpu5 \
        --device=/dev/xpu6:/dev/xpu6 \
        --device=/dev/xpu7:/dev/xpu7 \
        --privileged ${XAV_IMAGE} /bin/bash -c ". /etc/profile && exec /bin/bash"
```

进入容器：

```bash
docker exec -it <CONTAINER_NAME> bash
```

## 容器内目录说明

镜像自带的 `/workspace` 目录结构大致如下（以实际镜像为准）：

```
/workspace/
├── README.md                  # 分布式推理部署总览
├── singlenode/                # 单机启动脚本（按模型分目录，另含 EXAMPLE.md 说明命名规范）
├── multinode/                 # 多机 PD 混合等部署脚本
├── disaggregated_serving/     # 多机 PD 分离（1P1D / 2P2D / xPyD）
├── disaggreated_encoder/      # Encoder 分离
├── curl.sh                    # 服务可用性测试
├── evalscope.sh               # 精度测试
├── benchmark.sh               # 性能测试
└── xprofiler/                 # profiling 工具
```

**命名规范**（见 `/workspace/singlenode/EXAMPLE.md`）：每个模型目录下一般包含：

- `{model}_TP{N}_evalserver.sh`：vLLM 推理服务启动脚本
- `start_eval_{model}_TP{N}.sh`：evalscope 启动入口
- `eval_{model}_TP{N}.sh`、`eval_client.sh`：客户端/测试脚本

## 下载模型权重

以 Qwen3.5-0.8B 为例（其他模型同理）：

```bash
# 方式一：HuggingFace
huggingface-cli download <org>/<model_name> --local-dir /home/models/<model_name>

# 方式二：ModelScope
git lfs install
git clone https://www.modelscope.cn/<org>/<model_name>.git /home/models/<model_name>
```

## 启动推理服务

以 `/workspace/singlenode/Qwen3.5-0.8B/Qwen3.5-0.8B_TP2_evalserver.sh` 为例，该脚本已经封装好了 xvLLM 启动所需的环境变量与命令行参数，**通常只需修改脚本中的模型路径为本地实际路径**：

```bash
cd /workspace/singlenode/Qwen3.5-0.8B

# 编辑脚本，将其中的 MODEL / model_path 之类变量改为你下载的权重目录
vim Qwen3.5-0.8B_TP2_evalserver.sh

# 直接运行，即可拉起 vLLM OpenAI 兼容服务
bash Qwen3.5-0.8B_TP2_evalserver.sh
```

> 其他模型的启动脚本位于 `/workspace/singlenode/<模型名>/` 下，使用方式一致：**修改模型路径 → 执行脚本**。若某模型需要特殊配置（量化、草稿模型、TP/EP 切分等），请参考该模型目录下的脚本注释或对应专用文档。

## 验证推理服务

服务启动后，使用镜像自带的测试脚本即可验证：

```bash
# 可用性测试（快速验证服务响应）
bash /workspace/curl.sh

# 精度测试
bash /workspace/evalscope.sh

# 性能压测
bash /workspace/benchmark.sh
```

也可直接使用 curl 请求 OpenAI 兼容接口：

```bash
curl http://localhost:<port>/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "<served_model_name>",
    "messages": [{"role": "user", "content": "你是谁"}],
    "temperature": 0.6,
    "max_tokens": 512
  }'
```

## 多机 / PD 分离 / 其他场景

参考容器内：

- `/workspace/README.md`：分布式部署总览、节点 IP / NODE_ID / 网卡拓扑 (`xpu-smi topo -m`) 等关键配置说明
- `/workspace/multinode/`：多机 PD 混合
- `/workspace/disaggregated_serving/`：PD 分离架构

以及本仓库针对特定大模型的专用文档。
