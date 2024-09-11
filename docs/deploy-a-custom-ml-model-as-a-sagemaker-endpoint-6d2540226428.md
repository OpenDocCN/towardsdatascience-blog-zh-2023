# 将自定义 ML 模型部署为 SageMaker 端点

> 原文：[https://towardsdatascience.com/deploy-a-custom-ml-model-as-a-sagemaker-endpoint-6d2540226428?source=collection_archive---------0-----------------------#2023-12-08](https://towardsdatascience.com/deploy-a-custom-ml-model-as-a-sagemaker-endpoint-6d2540226428?source=collection_archive---------0-----------------------#2023-12-08)

![](../Images/4c0d654b542867fe832bc898ebb49a1d.png)

照片由 [Ricardo Gomez Angel](https://unsplash.com/@rgaleriacom?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## SageMaker 端点部署

## 快速简便的 AWS SageMaker 端点创建指南

[](https://hai-rozen.medium.com/?source=post_page-----6d2540226428--------------------------------)[![Hai Rozencwajg](../Images/8812d8935fe505ef26b58a8ec739a06f.png)](https://hai-rozen.medium.com/?source=post_page-----6d2540226428--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6d2540226428--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6d2540226428--------------------------------) [Hai Rozencwajg](https://hai-rozen.medium.com/?source=post_page-----6d2540226428--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5921283ee0f1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-a-custom-ml-model-as-a-sagemaker-endpoint-6d2540226428&user=Hai+Rozencwajg&userId=5921283ee0f1&source=post_page-5921283ee0f1----6d2540226428---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d2540226428--------------------------------) · 10 分钟阅读 · 2023年12月8日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6d2540226428&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-a-custom-ml-model-as-a-sagemaker-endpoint-6d2540226428&user=Hai+Rozencwajg&userId=5921283ee0f1&source=-----6d2540226428---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6d2540226428&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-a-custom-ml-model-as-a-sagemaker-endpoint-6d2540226428&source=-----6d2540226428---------------------bookmark_footer-----------)

开发机器学习（ML）模型涉及关键步骤，从数据收集到模型部署。在通过测试优化算法并确保性能后，最终的关键步骤是部署。这个阶段将创新转化为实用性，使他人能够从模型的预测能力中受益。部署的 ML 模型弥合了开发和现实世界影响之间的差距，为用户和利益相关者提供了切实的好处。

本指南涵盖了将自定义 ML 作为 SageMaker 端点开发所需的基本步骤。在这一点上，我假设你已经有一个工作模型，并希望通过端点将其公开给其他人。该指南将引导你部署一个基于 PyTorch 的模型，旨在预测视频片段中的异常。该模型，也称为**AI VAD**，基于论文[《基于属性的准确且可解释的视频异常检测表示》](https://arxiv.org/pdf/2212.00789.pdf)，其实现可以在 [anomalib](https://github.com/openvinotoolkit/anomalib/tree/main/src/anomalib/models/ai_vad) GitHub 仓库中找到，仓库由 [OpenVINO](https://docs.openvino.ai/) 提供。要了解更多有关这种有趣方法的信息，请滚动到本博客的末尾，查看附录部分。

在这一点上，我想强调的是，在这种情况下，我们不能使用专门为部署 PyTorch 模型构建的 [PyTorchModel](https://sagemaker.readthedocs.io/en/stable/frameworks/pytorch/sagemaker.pytorch.html#pytorch-model) 抽象，原因有两个。第一个原因是我们有 [anomalib](https://github.com/openvinotoolkit/anomalib/tree/main/src/anomalib/models/ai_vad) 包作为额外的依赖项，而该依赖项不包含在预构建的 PyTorch SageMaker 镜像中。第二个原因是模型需要在训练步骤中学到的额外信息，而这些信息不是 PyTorch 模型权重的一部分。

实现这一目标的步骤如下：

1.  编写 SageMaker 模型服务脚本

1.  将模型上传到 S3

1.  将自定义 Docker 镜像上传到 AWS ECR

1.  在 SageMaker 中创建模型

1.  创建端点配置

1.  创建端点

1.  调用端点

# 编写 SageMaker 模型服务脚本

SageMaker 模型服务脚本（`inference.py`）是创建 SageMaker 模型的重要组成部分。它在机器学习模型和现实数据之间架起桥梁。本质上，它处理传入请求，运行模型预测，并返回结果，从而影响应用程序的决策过程。

`inference.py` 脚本由几个关键方法组成，每个方法都具有独特的功能，共同促进模型服务过程。下面列出了四个主要方法。

1.  `model_fn` 方法负责加载训练好的模型。它读取已保存的模型工件，并返回一个可以用于预测的模型对象。此方法仅在启动 SageMaker 模型服务器时调用一次。

1.  `input_fn` 方法将请求数据格式化为适合进行预测的形式。例如，在下面的代码中，这个函数根据数据来源（图像字节或 S3 URI 列表）以及帧列表是否应视为一个视频片段来格式化数据。

1.  `predict_fn` 方法接受格式化的请求数据并对加载的模型进行推断。

1.  最后，使用 `output_fn` 方法。它将预测结果格式化为响应消息。例如，将其打包为 JSON 对象。

Sagemaker 模型服务脚本的代码可以在下面找到。

```py
import os
import json
import joblib
import torch
from PIL import Image
import numpy as np
import io
import boto3
from enum import Enum
from urllib.parse import urlsplit
from omegaconf import OmegaConf
from anomalib.data.utils import read_image, InputNormalizationMethod, get_transforms
from anomalib.models.ai_vad.torch_model import AiVadModel

device = "cuda"

class PredictMode(Enum):
    frame = 1
    batch = 2
    clip = 3

def model_fn(model_dir):
    """
    This function is the first to get executed upon a prediction request,
    it loads the model from the disk and returns the model object which will be used later for inference.
    """

    # Load the config file
    config = OmegaConf.load(os.path.join(model_dir, "ai_vad_config.yaml"))
    config_model = config.model

    # Load the model
    model = AiVadModel(
            box_score_thresh=config_model.box_score_thresh,
            persons_only=config_model.persons_only,
            min_bbox_area=config_model.min_bbox_area,
            max_bbox_overlap=config_model.max_bbox_overlap,
            enable_foreground_detections=config_model.enable_foreground_detections,
            foreground_kernel_size=config_model.foreground_kernel_size,
            foreground_binary_threshold=config_model.foreground_binary_threshold,
            n_velocity_bins=config_model.n_velocity_bins,
            use_velocity_features=config_model.use_velocity_features,
            use_pose_features=config_model.use_pose_features,
            use_deep_features=config_model.use_deep_features,
            n_components_velocity=config_model.n_components_velocity,
            n_neighbors_pose=config_model.n_neighbors_pose,
            n_neighbors_deep=config_model.n_neighbors_deep,
        )

    # Load the model weights
    model.load_state_dict(torch.load(os.path.join(model_dir, "ai_vad_weights.pth"), map_location=device), strict=False)

    # Load the memory banks
    velocity_estimator_memory_bank, pose_estimator_memory_bank, appearance_estimator_memory_bank = joblib.load(os.path.join(model_dir, "ai_vad_banks.joblib")) 
    if velocity_estimator_memory_bank is not None:
        model.density_estimator.velocity_estimator.memory_bank = velocity_estimator_memory_bank
    if pose_estimator_memory_bank is not None:
        model.density_estimator.pose_estimator.memory_bank = pose_estimator_memory_bank
    if appearance_estimator_memory_bank is not None:
        model.density_estimator.appearance_estimator.memory_bank = appearance_estimator_memory_bank
    model.density_estimator.fit()

    # Move the entire model to device
    model = model.to(device)

    # get the transforms
    transform_config = config.dataset.transform_config.eval if "transform_config" in config.dataset.keys() else None
    image_size = (config.dataset.image_size[0], config.dataset.image_size[1])
    center_crop = config.dataset.get("center_crop")
    center_crop = tuple(center_crop) if center_crop is not None else None
    normalization = InputNormalizationMethod(config.dataset.normalization)
    transform = get_transforms(config=transform_config, image_size=image_size, center_crop=center_crop, normalization=normalization)

    return model, transform

def input_fn(request_body, request_content_type):
    """
    The request_body is passed in by SageMaker and the content type is passed in 
    via an HTTP header by the client (or caller).
    """

    print("input_fn-----------------------")

    if request_content_type in ("application/x-image", "image/x-image"):
        image = Image.open(io.BytesIO(request_body)).convert("RGB")
        numpy_array = np.array(image)
        print("numpy_array.shape", numpy_array.shape)
        print("input_fn-----------------------")
        return [numpy_array], PredictMode.frame

    elif request_content_type == "application/json":
        request_body_json = json.loads(request_body)

        s3_uris = request_body_json.get("images", [])

        if len(s3_uris) == 0:
            raise ValueError(f"Images is a required key and should contain at least a list of one S3 URI")

        s3 = boto3.client("s3")
        frame_paths = []
        for s3_uri in s3_uris:
            parsed_url = urlsplit(s3_uri)
            bucket_name = parsed_url.netloc
            object_key = parsed_url.path.lstrip('/')
            local_frame_path = f"/tmp/{s3_uri.replace('/', '_')}"
            # Download the frame from S3
            s3.download_file(bucket_name, object_key, local_frame_path)
            frame_paths.append(local_frame_path)

        frames = np.stack([torch.Tensor(read_image(frame_path)) for frame_path in frame_paths], axis=0)

        predict_mode = PredictMode.clip if request_body_json.get("clip", False) else PredictMode.batch

        print("frames.shape", frames.shape)
        print("predict_mode", predict_mode)
        print("input_fn-----------------------")

        return frames, predict_mode

    # If the request_content_type is not as expected, raise an exception
    raise ValueError(f"Content type {request_content_type} is not supported")

def predict_fn(input_data, model):
    """
    This function takes in the input data and the model returned by the model_fn
    It gets executed after the model_fn and its output is returned as the API response.
    """

    print("predict_fn-----------------------")

    model, transform = model

    frames, predict_mode = input_data

    processed_data = {}
    processed_data["image"] = [transform(image=frame)["image"] for frame in frames]
    processed_data["image"] = torch.stack(processed_data["image"])

    image = processed_data["image"].to(device)

    # Add one more dimension for a batch size of one clip
    if predict_mode == PredictMode.clip:
        image = image.unsqueeze(0)

    print("image.shape", image.shape)

    model.eval()

    with torch.no_grad():
        boxes, anomaly_scores, image_scores = model(image)

    print("boxes_len", [len(b) for b in boxes])

    processed_data["pred_boxes"] = [box.int() for box in boxes]
    processed_data["box_scores"] = [score.to(device) for score in anomaly_scores]
    processed_data["pred_scores"] = torch.Tensor(image_scores).to(device)

    print("predict_fn-----------------------")

    return processed_data

def output_fn(prediction, accept):
    """
    Post-processing function for model predictions. It gets executed after the predict_fn.
    """

    print("output_fn-----------------------")

    # Check if accept type is JSON
    if accept != "application/json":
        raise ValueError(f"Accept type {accept} is not supported")

    # Convert PyTorch Tensors to lists so they can be JSON serializable
    for key in prediction:
        # If torch.Tensor convert it to list
        if isinstance(prediction[key], torch.Tensor):
            prediction[key] = prediction[key].tolist()
        # If list, convert every tensor in the list
        elif isinstance(prediction[key], list):
            prediction[key] = [tensor.tolist() if isinstance(tensor, torch.Tensor) else tensor for tensor in prediction[key]]

    print("output_fn-----------------------")

    return json.dumps(prediction), accept
```

P.S. 强烈建议在进入下一步之前测试模型服务脚本。这可以通过模拟调用管道轻松完成，如下面的代码所示。

```py
import json
from inference import model_fn, predict_fn, input_fn, output_fn

response, accept = output_fn(
    predict_fn(
        input_fn(payload, "application/x-image"),
        model_fn("../")
    ),
    "application/json"
)
json.loads(response).keys()
```

# 将模型上传到 S3

要创建一个 SageMaker 端点，使其加载 AI VAD PyTorch 模型到完全相同的状态，我们需要以下文件：

+   AI VAD PyTorch 模型的权重（即 state_dict）

+   密度估计器内存库（不属于模型的权重）

+   包含 PyTorch 模型超参数的配置文件

+   Sagemaker 模型服务脚本 (`inference.py`)

以下代码演示了如何将所有所需的文件组织在一个目录中。

P.S. 我重写了内置的 PyTorch ModelCheckpoint 回调，以确保这些内存库作为检查点保存的一部分（实现可以在 [这里](https://github.com/hairozen/anomalib/blob/ai-vad-inference-improvements/src/anomalib/utils/callbacks/ai_vad/ai_vad_model_checkpoint.py) 找到）。

```py
import torch
import joblib
import shutil

checkpoint = "results/ai_vad/ucsd/run/weights/lightning/model.ckpt"
config_path = "results/ai_vad/ucsd/run/config.yaml"

model_weights = torch.load(checkpoint)
model_state_dict = model_weights["state_dict"]

torch.save(model_state_dict, "../ai_vad_weights.pth")

velocity_estimator_memory_bank = None
pose_estimator_memory_bank = None
appearance_estimator_memory_bank = None
if "velocity_estimator_memory_bank" in model_weights:
    velocity_estimator_memory_bank = model_weights["velocity_estimator_memory_bank"]
if "pose_estimator_memory_bank" in model_weights:
    pose_estimator_memory_bank = model_weights["pose_estimator_memory_bank"]
if "appearance_estimator_memory_bank" in model_weights:
    appearance_estimator_memory_bank = model_weights["appearance_estimator_memory_bank"]
banks = (velocity_estimator_memory_bank, pose_estimator_memory_bank, appearance_estimator_memory_bank)

joblib.dump(banks, "../ai_vad_banks.joblib")

shutil.copyfile(config_path, "../ai_vad_config.yaml")
```

然后，使用下面的命令将这四个文件压缩在一起，生成 `tar.gz` 文件。

```py
tar -czvf ../ai_vad_model.tar.gz -C ../ ai_vad_weights.pth ai_vad_banks.joblib ai_vad_config.yaml inference.py
```

最后，使用 boto3 将文件上传到 S3。

```py
import boto3
from datetime import datetime

current_datetime = datetime.now().strftime('%Y-%m-%d-%H-%M-%S')

s3 = boto3.resource('s3')
s3.meta.client.upload_file("../ai_vad_model.tar.gz", "ai-vad", f"{current_datetime}/ai_vad_model.tar.gz")
```

# 将自定义 Docker 镜像上传到 AWS ECR

如上所述，由于我们有一个不包含在预构建 PyTorch Sagemaker 镜像中的额外依赖项（即 [anomalib](https://github.com/openvinotoolkit/anomalib/tree/main/src/anomalib/models/ai_vad) 包），我们为此目的创建了一个新的 Docker 镜像。在构建自定义 Docker 镜像之前，需要进行 Amazon ECR 存储库认证。

```py
REGION=<my_aws_region>
ACCOUNT=<my_aws_account>

# Authenticate Docker to an Amazon ECR registry
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin <docker_registry_url>.dkr.ecr.$REGION.amazonaws.com

# Loging to your private Amazon ECR registry
aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $ACCOUNT.dkr.ecr.$REGION.amazonaws.com
```

Dockerfile 可以在下面找到，不同的 Docker 注册表路径可以在 [这里](https://docs.aws.amazon.com/sagemaker/latest/dg-ecr-paths/sagemaker-algo-docker-registry-paths.html) 找到。确保根据模型的需求（CPU/GPU，Python 版本等）和您的 AWS 区域选择正确的注册表路径。例如，如果区域是 `us-east-1`，则完整的 Docker 注册表路径应类似于：

`763104351884.dkr.ecr.us-east-1.amazonaws.com/pytorch-inference:2.0.0-gpu-py310`

```py
# Use the SageMaker PyTorch image as the base image
FROM <docker_registry_url>.dkr.ecr.<my_aws_region>.amazonaws.com/pytorch-inference:2.0.0-gpu-py310

# Install the additional dependency
RUN pip install "git+https://github.com/hairozen/anomalib.git@ai-vad-inference-improvements"
```

现在，我们可以运行经典的 Docker 构建命令来构建这个自定义镜像。

```py
docker build -t ai-vad-image .
```

下一步是为我们构建的新镜像创建 AWS ECR 存储库，对其进行标记，然后将镜像推送到 AWS ECR 存储库。

```py
# Create the AWS ECR repository
aws ecr create-repository --repository-name ai-vad-image

# Tag the image
docker tag ai-vad-image:latest $ACCOUNT.dkr.ecr.$REGION.amazonaws.com/ai-vad-image:latest

# Push the tagged image to the AWS ECR repository
docker push $ACCOUNT.dkr.ecr.$REGION.amazonaws.com/ai-vad-image:latest
```

# 在 SageMaker 中创建一个模型

这一步非常简单。代码如下。

```py
import boto3
import sagemaker

sagemaker_client = boto3.client(service_name="sagemaker")
role = sagemaker.get_execution_role()

model_name = f"ai-vad-model-{current_datetime}"

primary_container = {
    "Image": f"{my_aws_account}.dkr.ecr.{my_aws_region}.amazonaws.com/ai-vad-image:latest",
    "ModelDataUrl": f"s3://ai-vad/{current_datetime}/ai_vad_model.tar.gz"
}

create_model_response = sagemaker_client.create_model(
    ModelName=model_name,
    ExecutionRoleArn=role,
    PrimaryContainer=primary_container)
```

# 创建端点配置

下一步是创建一个端点配置。下面是一个基本的示例。

```py
endpoint_config_name = f"ai-vad-model-config-{current_datetime}"

sagemaker_client.create_endpoint_config(
    EndpointConfigName=endpoint_config_name,
    ProductionVariants=[{
        "InstanceType": "ml.g5.xlarge",
        "InitialVariantWeight": 1,
        "InitialInstanceCount": 1,
        "ModelName": model_name,
        "VariantName": "AllTraffic"}])
```

# 创建一个端点

现在我们准备好创建端点本身了。

```py
endpoint_name = f"ai-vad-model-endpoint-{current_datetime}"

sagemaker_client.create_endpoint(
    EndpointName=endpoint_name,
    EndpointConfigName=endpoint_config_name)
```

请注意，端点状态可能需要几分钟才能从“创建中”变为“服务中”。可以通过下面所示的方式检查当前状态。

```py
response = sagemaker_client.describe_endpoint(EndpointName=endpoint_name)
response["EndpointStatus"]
```

# 调用端点

关键时刻到了。现在是调用端点测试一切是否按预期工作的时候。

```py
with open(file_name, "rb") as f:
    payload = f.read()

predictor = sagemaker.predictor.Predictor(endpoint_name=endpoint_name)
predictor.serializer = DataSerializer(content_type="image/x-image")
predictor.predict(payload)
```

因此，这是一项很好的检查，但请注意，`predictor.predict` 函数并不会运行完整的调用管道，这个管道来自 SageMaker 服务脚本，其中包括：

`output_fn(predict_fn(input_fn(input_data, model_fn(model_dir)),accept)`

要测试它，我们可以通过 API 调用来调用模型。

```py
with open(file_name, "rb") as f:
    payload = f.read()

sagemaker_runtime = boto3.client("runtime.sagemaker")
response = sagemaker_runtime.invoke_endpoint(
    EndpointName=endpoint_name,
    ContentType="image/x-image",
    Body=payload
)

response = json.loads(response["Body"].read().decode())
```

利用 [anomalib](https://github.com/openvinotoolkit/anomalib/tree/main/src/anomalib/models/ai_vad) 提供的出色可视化，我们可以绘制给定 UCSDped2 数据集中某帧的框和标签。

![](../Images/d149445b2ddcf4c8112e7bede44c60a7.png)

作者提供的图像。该图像是使用 [anomalib](https://github.com/openvinotoolkit/anomalib/tree/main/src/anomalib/models/ai_vad) 包生成的，基于 [UCSD 异常检测数据集](http://www.svcl.ucsd.edu/projects/anomaly/dataset.htm)。绿色框表示那些行人的步态没有异常，而红色框表示的骑车者则可能由于 AI VAD 模型的速度和姿态特征而存在异常。

# 结论

好了，让我们快速回顾一下我们在这里讨论的内容。部署 SageMaker 模型以进行服务需要一系列步骤。

首先，必须编写 SageMaker 模型服务脚本，以定义模型的功能和行为。

然后将模型上传到 Amazon S3 进行存储和检索。

此外，定制的 Docker 镜像被上传到 AWS Elastic Container Registry (ECR)，以容器化模型及其依赖项。

下一步是创建一个 SageMaker 模型，该模型将存储在 S3 中的模型工件与存储在 ECR 中的 Docker 镜像关联起来。

然后创建一个端点配置，定义用于托管模型的实例数量和类型。

最后，创建一个端点以在部署的模型和客户端应用程序之间建立实时连接，允许它们调用端点并进行实时预测。

通过这些步骤，部署 SageMaker 模型成为了一个简化的过程，确保了高效和可靠的模型服务。

# 附录

2023 年由 Reiss 等人发布的论文 [基于属性的准确且可解释的视频异常检测](https://arxiv.org/pdf/2212.00789.pdf)，提出了一种使用基于属性的表示进行视频异常检测 (VAD) 的简单但极其有效的方法。

论文认为，传统的 VAD 方法通常依赖深度学习，这些方法往往难以解释，使得用户难以理解系统为何将某些帧或对象标记为异常。

为了解决这个问题，作者提出了一种方法，通过速度、姿态和深度来表示视频中的每个对象。这些属性易于理解和解释，并且可以使用基于密度的方法来计算异常分数。

论文表明，这种简单的表示方法足以在多个具有挑战性的 VAD 数据集上实现最先进的性能，包括上海科技大学的数据集，这是最大的和最复杂的 VAD 数据集。

除了准确之外，作者还展示了他们的方法具有可解释性。例如，他们可以向用户提供视频中对异常评分贡献最大的对象的列表，以及这些对象的速度、姿态和深度信息。这可以帮助用户理解系统为何将视频标记为异常。

总体而言，这篇论文是 VAD 领域的重要贡献。它提出了一种简单、准确且可解释的 VAD 方法，可以用于各种应用。
