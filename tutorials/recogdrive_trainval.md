# recogdrive

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

## 准备数据集及代码
### 准备数据集
```bash
# 可以参考官方文档准备数据集navsim
cd recogdrive
bash download/download_maps.sh
bash download/download_navtrain.sh
bash download/download_test.sh
```

### 下载代码及预训练权重
```bash
# 下载stage3预训练权重
cd /workspace
git lfs install
git clone https://huggingface.co/owl10/ReCogDrive-VLM-2B
git clone https://huggingface.co/owl10/ReCogDrive-2B-IL

# 下载recogdrive代码以及配置

```

## 启动容器
1. 启动容器：
    ```bash
    export XAV_IMAGE=<XAV_IMAGE>
    export NAME_CONTAINER=recogdrive
    export MODEL_PATH=</path/to/models> #本地路径
    
    docker run -itd --privileged --net=host \
        -w /workspace/xav-models/modelzoo \
        -v ${MODEL_PATH}:/workspace/models \
        --device=/dev/xpu0 --device=/dev/xpu1 --device=/dev/xpu2 \
        --device=/dev/xpu3 --device=/dev/xpu4 --device=/dev/xpu5 \
        --device=/dev/xpu6 --device=/dev/xpu7 \
        --name ${NAME_CONTAINER} \
        --shm-size 256g \
        ${XAV_IMAGE} \
        bash
    
    docker exec -it ${XAV_CONTAINER} bash
    ```
2. 配置容器内环境：
    ```bash
    # 更新环境
    cd recogdrive
    pip install -e .
    pip install decord==0.6.0
    pip install peft==0.17.0
    pip install flash-attn==2.8.3
    ```


## 单机多卡训练
### 设置环境变量
```bash
export BKCL_TREE_THRESHOLD=0
export XMLIR_ENABLE_NEW_PG=1
```

### stage3数据缓存
```bash
cd scripts
bash cache_dataset/run_metric_caching_train.sh
bash cache_dataset/run_metric_caching.sh
bash cache_dataset/run_caching_recogdrive_hidden_state.sh
bash cache_dataset/run_caching_recogdrive_hidden_state_eval.sh
```

### 执行8卡stage3训练
```bash
cd scripts
bash training/run_recogdrive_train_single_node_rl_2b.sh 
```


## 单机多卡评估
### 执行评估
```bash
cd scripts
bash evaluation/run_recogdrive_agent_pdm_score_evaluation_2b.sh
```




