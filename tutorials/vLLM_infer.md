# vLLM infer Guide

## 环境准备
请联系昆仑芯客户支持获取开发环境镜像。

### PCIe环境配置（OAM跳过此步骤）
#### 确定PCIe环境
在宿主机执行如下命令：
```bash
xpu_smi xpulink -s
```
如果检测到显示为“P800 PCIe”，并且 Link 0/1/2/3 的状态均为“active”，则表明当前系统处于 PCIe 模式且8张卡已成功实现互联。

#### 配置PCIe的8卡互联模式
如果当前系统处于PCIe模式但8张卡未成功实现互联，需要在宿主机系统端执行以下配置操作：
```bash
# 步骤 1：修改/etc/modprobe.d目录下的kunlun.conf文件
vim /etc/modprobe.d/kunlun.conf
# 步骤 2：添加配置参数 PcieLink=1，该参数应与其他配置参数置于同一对双引号内，并使用分号 (;) 进行分隔。示例如下：
options kunlun KLreg_RegistryDwords="RmDisableSMMU=1;RmKL3ShiftMode=0;PcieLink=1"
# 步骤 3：重启服务器，即可完成配置的修改，服务器可支持 8 卡互联。
```

## 代码准备
### 下载代码及预训练权重
```bash
# 从modelscope下载预训练权重
# 下载qwen2.5-VL-3B 模型权重
cd /home
git lfs install
git clone https://www.modelscope.cn/Qwen/Qwen2.5-VL-3B-Instruct.git
```


## 启动容器
```bash
export XAV_IMAGE=<XAV_IMAGE>
export NAME_CONTAINER=alpha_drive
export MODEL_PATH=</path/to/Qwen2.5-VL-3B-Instruct> #本地路径

docker run -itd --privileged --net=host \
    -w /workspace/xav-models/modelzoo \
    -v ${MODEL_PATH}:/home \
    --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
    --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
    --device=/dev/xpu6 --device=/dev/xpu7 \
    --name ${NAME_CONTAINER} \
    --shm-size 256g \
    ${XAV_IMAGE} \
    bash

docker exec -it ${NAME_CONTAINER} bash
```

### 容器内环境配置
```bash
# 切换 conda env
conda activate python310_torch25_cuda
pip install transformers==4.51.0
pip install vllm==0.7.2
pip install vllm==0.8.2 --no-deps
# 下载产出包
wget https://klx-sdk-release-public.su.bcebos.com/xvllm/KL3/0.8.2/latest/output.tar.gz
# 创建xvllm环境
tar -xvf output.tar.gz && mv output xvllm_0.8.2
cd xvllm_0.8.2
# 安装xvllm及其依赖
bash scripts/install_output.sh
```

## 单机online模式推理
```bash
# 使用vllm需要设置的环境变量
export XMLIR_ENABLE_MOCK_TORCH_COMPILE=false
export XMLIR_FORCE_USE_XPU_GRAPH=1
export VLLM_USE_V1=0

# 在一个terminate中启动vllm server
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 VLLM_USE_V1=0 python -m vllm.entrypoints.openai.api_server --model "{your_model_dir}/Qwen2.5-VL-3B-Instruct" \
  --task generate \
  --trust-remote-code \
  --max-model-len 32768 \
  --limit-mm-per-prompt "image=3,video=1" \
  --tensor-parallel-size 8 \
  --enforce-eager \
  --dtype "float16" \
  --max-num-seqs 1 \
  --host localhost \
  --port 8802 \
  --gpu-memory-utilization 0.8 \
  --mm-processor-kwargs '{"min_pixels": 784, "max_pixels": 78400}'

# 新起一个terminate, 发送请求
curl http://localhost:8802/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
        "model": "{your_model_dir}/Qwen2.5-VL-3B-Instruct",
        "messages": [
        {
            "role": "user",
            "content": [{"type": "text", "text": "请详细描述每个图片，然后总结一下视频里的产品"}, {"type": "image_url", "image_url": {"url": "https://upload.wikimedia.org/wikipedia/commons/9/98/Horse-and-pony.jpg"}}, {"type": "image_url", "image_url": {"url": "https://upload.wikimedia.org/wikipedia/commons/6/69/Grapevinesnail_01.jpg"}}, {"type": "image_url", "image_url": {"url": "  https://upload.wikimedia.org/wikipedia/commons/3/30/George_the_amazing_guinea_pig.jpg"}}, {"type": "video_url", "video_url": {"url": "https://duguang-labelling.oss-cn-shanghai.aliyuncs.com/qiansun/video_ocr/videos/50221078283.mp4"}}]
        }
        ],
        "temperature": 0,
        "max_completion_tokens": 512
    }'

# 如果遇见网络问题，先尝试打开网络代理
# 以下两种可以分别尝试
export HTTP_PROXY=http://agent.baidu.com:8891
export HTTPS_PROXY=http://agent.baidu.com:8891

export http_proxy=http://10.162.37.16:8128
export https_proxy=http://10.162.37.16:8128

# 如果还是不行提前从网上下载几张图片，视频保存在本地
# 启动vllm server中需要设置--allowed-local-media-path，添加图片视频的保存路径
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 VLLM_USE_V1=0 python -m vllm.entrypoints.openai.api_server --model "{your_model_dir}/Qwen2.5-VL-3B-Instruct" \
  --task generate \
  --trust-remote-code \
  --max-model-len 32768 \
  --limit-mm-per-prompt "image=3,video=1" \
  --tensor-parallel-size 8 \
  --enforce-eager \
  --dtype "float16" \
  --max-num-seqs 1 \
  --host localhost \
  --port 8802 \
  --gpu-memory-utilization 0.8 \
  --mm-processor-kwargs '{"min_pixels": 784, "max_pixels": 78400}' \
  --allowed-local-media-path PATH

# 将路径替换为本地路径，其余图片和视频类似
curl http://localhost:8802/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
        "model": "{your_model_dir}/Qwen2.5-VL-3B-Instruct",
        "messages": [
        {
            "role": "user",
            "content": [{"type": "text", "text": "请详细描述每个图片，然后总结一下视频里的产品"}, {"type": "image_url", "image_url": {"url": "file:///workspace/pic.png"}}]
        }
        ],
        "temperature": 0,
        "max_completion_tokens": 512
    }'

```