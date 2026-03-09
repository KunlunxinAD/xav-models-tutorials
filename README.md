# XPU Autonomous Vehicle Open Model Tutorials

## 介绍
基于 *XAV* (XPU Autonomous Vehicle) 产品加速的自动驾驶模型教程与文档仓库

## 更新日志 🚀
- [26/03/09] 我们支持了 **[Qwen3-30B-A3B](tutorials/qwen3_30b_a3b_pretrain.md)** 模型的预训练.
- [26/02/04] 我们支持了 **[Qwen3-4B](tutorials/qwen3_llamafactory_trainval.md)**、**[Qwen3-30B-A3B](tutorials/qwen3_llamafactory_trainval.md)** 模型的训练.
- [26/02/04] 我们支持了 **[Qwen3-235B-A22B-Thinking-2507](tutorials/qwen3_235b_a22b_thinking_2507_infer.md)** 模型的推理.
- [26/02/04] 我们支持了 **[xav_vLLM](tutorials/xav_vLLM.md)** QWen、QWen-VL系列的推理.
- [26/02/02] 我们支持了 **[PaddleOCR_v5](tutorials/PaddleOCR_trainval.md)** 模型的训练和推理.
- [26/02/02] 我们支持了 **[Yolo11](tutorials/Yolo11_inference.md)** 模型的pytorch推理.
- [26/01/07] 我们支持了 **[cosmos-transfer2.5](tutorials/cosmos_transfer2.5_trainval.md)** 模型的训练和推理.
- [26/01/07] 我们支持了 **[cosmos-predict2.5](tutorials/cosmos_predict2.5_trainval.md)** 模型的训练和推理.
- [25/12/30] 我们支持了 **[PETR](tutorials/PETR_trainval.md)**、**[FastBEV](tutorials/FastBEV_trainval.md)**、**[MaskRCNN](tutorials/MaskRCNN_trainval.md)** 模型的训练.
- [25/12/09] 我们支持了 **[pi0](tutorials/Pi_0_trainval.md)** 模型的训练.
- [25/11/27] 我们增加了 **[DriveDreamer](tutorials/DriveDreamer_trainval.md)、[Qwen3-8B](tutorials/qwen3_8b_xmegatron_trainval)** 的训练支持
- [25/08/20] 我们支持了 **[LLaVA](tutorials/LLaVA_trainval.md)、[OpenVLA](tutorials/openvla_trainval.md)** 模型的训练

<details><summary>展开日志</summary>

- [25/07/04] 我们支持了 **[BEVFusion](tutorials/bevfusion_trainval.md)** 模型的训练

- [25/05/27] 我们支持了 **[StreamPETR](tutorials/StreamPETR_trainval.md)** 模型的训练

- [25/05/12] 我们支持了 **[VIT](tutorials/VIT_trainval.md)、[RegNet](tutorials/regnet_trainval)** 模型的训练

- [25/04/30] 我们支持了 **[Qwen2.5-VL](tutorials/qwen2.5vl_infer.md)** 模型的推理

- [25/04/11] 我们支持了 **[SparseDrive](tutorials/SparseDrive_trainval.md)、[Sparse4D](./tutorials/sparse4d_trainval.md)** 模型的训练

</details>

## 模型库
<table>
    <thead>
        <tr>
            <th>Model Type</th>
            <th>Model</th>
            <th>任务</th>
            <th>精度格式</th>
            <th>GPUs</th>
            <th>框架</th>
        </tr>
    </thead>
    <tbody>
        </tr>
            <td rowspan="4"> Basic Models </td>
            <td><a href="tutorials/VIT_trainval.md">VisionTransformer</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>-</td>
        </tr>
        </tr>
            <td><a href="tutorials/regnet_trainval.md">RegNet</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>-</td>
        </tr>
        <tr>
            <td><a href="tutorials/PaddleOCR_trainval.md">PaddleOCR_v5</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>PaddlePaddle</td>
        </tr>
        <tr>
            <td><a href="tutorials/Yolo11_inference.md">Yolo11</a></td>
            <td>Inference</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>-</td>
        </tr>
        <tr>
            <td rowspan="18"> E2E AD Models </td>
            <td><a href="tutorials/bevdet_trainval.md">BEVDet</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/bevformer_trainval.md">BEVFormer</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/PointPillar_trainval.md">PointPillar</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/PETR_trainval.md">PETR</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/FastBEV_trainval.md">FastBEV</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/MaskRCNN_trainval.md">MaskRCNN</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>Detectron2</td>
        </tr>
        <tr>
            <td><a href="tutorials/lansegnet_trainval.md">LaneSegNet</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/maptrv2_trainval.md">Maptrv2</a></td>
            <td>Pretrain</td>
            <td>FP32/FP16</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/sparse4d_trainval.md">Sparse4D</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/StreamPETR_trainval.md">StreamPETR</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/bevfusion_trainval.md">BEVFusion</a></td>
            <td>Pretrain</td>
            <td>FP32/FP16</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/Far3D_trainval.md">Far3D</a></td>
            <td>Pretrain</td>
            <td>FP16/FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/GameFormer-Planner_trainval.md">GameFormer-Planner</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>-</td>
        </tr>
        <tr>
            <td><a href="tutorials/QCNet_trainval.md">QCNet</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>-</td>
        </tr>
        <tr>
            <td><a href="tutorials/UniAD_trainval.md">UniAD</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/VAD_trainval.md">VAD</a></td>
            <td>Pretrain</td>
            <td>FP32/FP16</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/SparseDrive_trainval.md">SparseDrive</a></td>
            <td>Pretrain</td>
            <td>FP32/FP16</td>
            <td>1 x 8</td>
            <td>MMCV</td>
        </tr>
        <tr>
            <td><a href="tutorials/DiffusionDrive_trainval.md">DiffusionDrive</a></td>
            <td>Pretrain</td>
            <td>FP32</td>
            <td>1 x 8</td>
            <td>Navsim</td>
        </tr>
        <tr>
            <td rowspan="3"> VLM/VLA </td>
            <td><a href="tutorials/qwen2.5vl_3b_trainval.md">Qwen2.5-VL</a></td>
            <td>SFT/LoRA</td>
            <td>FP16/BF16</td>
            <td>1 x 8</td>
            <td>LLamaFactory</td>
        </tr>
        <tr>
            <td><a href="tutorials/LLaVA_trainval.md">LLaVA</a></td>
            <td>Pretrain/SFT</td>
            <td>FP16/BF16</td>
            <td>1 x 8</td>
            <td>-</td>
        </tr>
            <tr>
            <td><a href="tutorials/Pi_0_trainval.md">Pi0</a></td>
            <td>Pretrain/SFT</td>
            <td>FP16/BF16</td>
            <td>1 x 8</td>
            <td>Lerobot</td>
        </tr>
        <tr>
            <td rowspan="3"> World Model </td>
            <td><a href="tutorials/DriveDreamer_trainval.md">DriveDreamer</a></td>
            <td>Pretrain/SFT</td>
            <td>FP16/BF16</td>
            <td>1 x 8</td>
            <td>-</td>
        </tr>
        <tr>
            <td><a href="tutorials/cosmos_predict2.5_trainval.md">cosmos-predict2.5</a></td>
            <td>SFT</td>
            <td>BF16</td>
            <td>1 x 8</td>
            <td>-</td>
        </tr>
            <td><a href="tutorials/cosmos_transfer2.5_trainval.md">cosmos-transfer2.5</a></td>
            <td>SFT</td>
            <td>BF16</td>
            <td>1 x 8</td>
            <td>-</td>
        </tr>
        <tr>
            <td rowspan="5"> LLM </td>
            <td><a href="tutorials/qwen2.5_trainval.md">Qwen2.5-7B</a></td>
            <td>SFT/LoRA</td>
            <td>FP16/BF16</td>
            <td>1 x 8</td>
            <td>LLamaFactory</td>
        </tr>
        <tr>
            <td><a href="tutorials/qwen3_8b_xmegatron_trainval.md">Qwen3-8B</a></td>
            <td>Pretrain</td>
            <td>BF16</td>
            <td>1 x 8</td>
            <td>Megatron</td>
        </tr>
        <tr>
            <td><a href="tutorials/qwen3_llamafactory_trainval.md">Qwen3-4B</a></td>
            <td>SFT/LoRA</td>
            <td>FP16/BF16</td>
            <td>1 x 8</td>
            <td>LLamaFactory</td>
        </tr>
        <tr>
            <td><a href="tutorials/qwen3_llamafactory_trainval.md">Qwen3-30B-A3B</a></td>
            <td>Pretrain/SFT/LoRA</td>
            <td>FP16/BF16</td>
            <td>1 x 8</td>
            <td>Megatron/LLamaFactory</td>
        </tr>
        <tr>
            <td><a href="tutorials/qwen3_235b_a22b_thinking_2507_infer.md">Qwen3-235B-A22B-Thinking-2507</a></td>
            <td>Inference</td>
            <td>FP16</td>
            <td>1 x 8</td>
            <td>xSGL</td>
        </tr>
    </tbody>
</table>


## 许可证
XAV Open Model Tutorials 使用 [Apache-2.0 license](LICENSE) 许可证。
XAV 基于《[昆仑芯AI产品协议](LICENSE_KUNLUNXIN)》获得许可。若获取并使用XAV相关Docker容器，你将接受本许可协议中的条款与条件。
