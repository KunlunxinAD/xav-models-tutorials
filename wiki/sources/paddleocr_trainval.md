# Source Summary: PaddleOCR_v5

## Source

- [`PaddleOCR_trainval.md`](../../tutorials/PaddleOCR_trainval.md)

## Extracted classification

| Field | Value |
| --- | --- |
| Model / topic | PaddleOCR-v5 |
| Domain | Vision/OCR |
| Workflow tags | Inference, Pretrain, Trainval |
| Frameworks / backends | PaddlePaddle |
| Precision mentions | Not explicitly stated |
| Device hints | Not explicitly extracted |
| Dataset hints | Not explicitly extracted |

## Source outline

- # PaddleOCR_v5
- ## 准备环境
- ## 启动容器
- ### 配置容器内环境
- ## 准备数据集及代码
- ### 准备代码
- ### 准备数据集
- ## 下载预训练权重
- ## 模型训推
- ### 模型测试
- ### 模型训练
- #### 训练检测模型
- #### 训练识别模型
- ### 模型onnx推理

## Environment variables mentioned
| Variable | Source value |
| --- | --- |
| `FLAGS_use_stride_kernel` | `0` |
| `BKCL_FORCE_SYNC` | `1` |
| `BKCL_TIMEOUT` | `1800` |
| `XPU_BLACK_LIST` | `pad3d,pad3d_grad` |

## Representative commands

- `docker run -d -p 8000:8081 -it          \`
- `pip install --pre paddlepaddle -i https://www.paddlepaddle.org.cn/packages/nightly/cpu/`
- `pip install paddlepaddle-xpu==3.3.0 -i https://www.paddlepaddle.org.cn/packages/stable/xpu-p800/`
- `pip install paddleocr`
- `pip install paddle2onnx`
- `pip install onnxruntime`
- `pip install scikit-image`
- `pip install pyclipper`
- `pip install albumentations`
- `pip install lmdb`
- `pip install shapely`
- `pip install numpy==1.24.4`

## URLs mentioned

- <https://www.paddlepaddle.org.cn/packages/nightly/cpu/>
- <https://www.paddlepaddle.org.cn/packages/stable/xpu-p800/>
- <https://klx-sdk-release-public.su.bcebos.com/v1/xav/release/models/PaddleOCR/PaddleOCR_code_data.tgz>
- <https://github.com/PaddlePaddle/PaddleOCR.git>
- <https://github.com/PaddlePaddle/PaddleOCR/blob/main/docs/datasets/ocr_datasets.md>
- <https://paddle-model-ecology.bj.bcebos.com/paddlex/official_inference_model/paddle3.0.0/PP-OCRv5_server_det_infer.tar>
- <https://paddle-model-ecology.bj.bcebos.com/paddlex/official_inference_model/paddle3.0.0/PP-OCRv5_server_rec_infer.tar>
- <https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_ocr_002.png>

## Related derived pages

- [`PaddleOCR-v5`](../models/PaddleOCR-v5.md)
- [`basic-vision-trainval`](../recipes/basic-vision-trainval.md)
- [`llm-inference`](../recipes/llm-inference.md)

## Open questions

- Precision is not explicitly extracted from the source summary pass.
- Benchmark, peak memory, throughput, and accuracy should be treated as unknown unless explicitly present in the source tutorial.
- Image tags and private environment details are intentionally not inferred.
