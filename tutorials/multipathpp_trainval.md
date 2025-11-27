# Multipath++
## 准备环境
请联系昆仑芯客户支持获取开发环境镜像。

## 配置环境

### 启动容器

```
export image=<XAV_IMAGE>
export name=multipathpp
export DATA_PATH=</path/to/waymo_motion_v1.0>

sudo docker run -itd \
  --privileged \
  --net=host \
  -v "$(pwd)":/workspace \
  -v ${DATA_PATH}:/data/waymo_motion_v1.0 \
  --cpuset-cpus="0-$(($(nproc)-1))" \
  --shm-size=128g \
  --ulimit memlock=-1 \
  --name "${name}" \
  "${image}" \
  bash

docker exec -it ${XAV_CONTAINER} bash
```
## 下载资源
### 下载代码并解压
```
wget http://klx-sdk-release-public.su.bcebos.com/xav/release/models/MultiPathPP.tar.gz
tar -zxvf MultiPathPP.tar.gz
```

### 准备数据集
1. 下载数据集

    使用Waymo Open Dataset motion v1.0数据集，通过以下两个方式中的任意一种获取数据集：

    * 直接从bos下载处理好的数据集：

        ```
        # 下载处理后数据集，放到waymo_motion_v1.0目录下即可
        ./bcecmd bos cp -r bos:/klx-sdk-release-public/xav/data/multipathpp_datasets/waymo_motion_v1.0/ /path/to/waymo_motion_v1.0

        # 或者
        wget http://klx-sdk-release-public.su.bcebos.com/xav/data/multipathpp_datasets/waymo_motion_v1.0/waymo_motion_v1.0_processed.tar.gz
        tar -zxvf waymo_motion_v1.0_processed.tar.gz -C /path/to/waymo_motion_v1.0
        ```

    * 下载raw文件，参考MultiPath++项目README.md处理制作数据集，下载数据集raw文件后还需进行如下处理后才能使用：

        ```
        ./MultiPathPP/README.md
        Waymo Open Dataset从官网下载非常慢，可从bos下载raw文件
        mkdir /path/to/waymo_motion_v1.0/raw/
        # 下载训练集raw文件
        ./bcecmd bos cp -r bos:/klx-pytorch-work-bd/datasets/waymo/waymo_open_dataset_motion_v_1_0_0/tf_example/training/ /path/to/waymo_motion_v1.0/raw/
        # 下载验证集raw文件
        ./bcecmd bos cp -r bos:/klx-pytorch-work-bd/datasets/waymo/waymo_open_dataset_motion_v_1_0_0/tf_example/validation/ /path/to/waymo_motion_v1.0/raw/
        ```
2. 处理制作数据集raw。 

    如果已经下载了处理好的数据集，则可以跳过此步骤。
    
    ```
    mkdir /path/to/waymo_motion_v1.0/processed/
    cd MultiPathPP/code

    # 训练集
    python3 prerender/prerender.py \
        --data-path /path/to/waymo_motion_v1.0/raw/training \
        --output-path /path/to/waymo_motion_v1.0/processed/training \
        --n-jobs 24 \
        --n-shards 1 \
        --shard-id 0 \
        --config configs/prerender.yaml
    # 验证集
    python3 prerender/prerender.py \
        --data-path /path/to/waymo_motion_v1.0/raw/validation \
        --output-path /path/to/waymo_motion_v1.0/processed/validation \
        --n-jobs 24 \
        --n-shards 1 \
        --shard-id 0 \
        --config configs/prerender.yaml
    ```

### 检查环境

 **超时（timeout）配置** 

如超时<1,200,000，需手动配置环境参数，如下所示：
```
# 检查timeout配置
cat /proc/kunlun/dev0/task_timeout_detect_threshold_in_ms

# 如果<1200000，需手动配置
echo "1200000" > /proc/kunlun/dev0/task_timeout_detect_threshold_in_ms
echo "1200000" > /proc/kunlun/dev1/task_timeout_detect_threshold_in_ms
echo "1200000" > /proc/kunlun/dev2/task_timeout_detect_threshold_in_ms
echo "1200000" > /proc/kunlun/dev3/task_timeout_detect_threshold_in_ms
echo "1200000" > /proc/kunlun/dev4/task_timeout_detect_threshold_in_ms
echo "1200000" > /proc/kunlun/dev5/task_timeout_detect_threshold_in_ms
echo "1200000" > /proc/kunlun/dev6/task_timeout_detect_threshold_in_ms
echo "1200000" > /proc/kunlun/dev7/task_timeout_detect_threshold_in_ms
```
## 搭建XPU环境

 操作如下环境配置：

- code/configs/final_RoP_Cov_Single.yaml

   把训练集和验证集数据路径改成您自己环境的相关路径。

- code/train.py

    ```
    #注释掉第63行，后面加上创建models目录的代码。原代码中 models 是个空文件，在models下创建目录会失败，导致模型无法保存。
        # os.mkdir(tb_path)
        if os.path.isfile("../models"):
            os.remove("../models")
        if not os.path.exists("../models"):
            os.makedirs("../models")
    ```
- code/model/modules.py

    ```
    #第121行修改如下
    # return scatter_max(running_mean_c, scatter_idx, dim=0)
    return torch.ops.xmlir.scatter_max(
                             running_mean_c, scatter_idx, 0, None, None)[0]
    ```
- rnn.py

    ```
    vim /root/miniconda/envs/python38_torch201_cuda/lib/python3.8/site-packages/torch/nn/modules/rnn.py +812

    #第812，823行做如下替换。原因是 lstm 融合算子 xmlir 暂不支持

                # result = _VF.lstm(input, hx, self._flat_weights, self.bias, self.num_layers,
                #                  self.dropout, self.training, self.bidirectional, self.batch_first)
                result_tmp = _VF.lstm(input.cpu(), tuple(t.cpu() for t in hx), [t.cpu() for t in self._flat_weights], self.bias, self.num_layers,
                                self.dropout, self.training, self.bidirectional, self.batch_first)
                result = tuple(t.cuda() for t in result_tmp)
    ```
## 训练与验证
```
cd MultiPathPP/code
# 原代码设定每个epoch中自动执行2次验证
# 训练和验证的loss保存在logs目录下
bash dist_train.sh
```
