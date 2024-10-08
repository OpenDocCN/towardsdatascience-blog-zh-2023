# 利用 SageMaker 多模型端点和 GPU 实例托管数百个 NLP 模型

> 原文：[`towardsdatascience.com/host-hundreds-of-nlp-models-utilizing-sagemaker-multi-model-endpoints-backed-by-gpu-instances-1ec215886248`](https://towardsdatascience.com/host-hundreds-of-nlp-models-utilizing-sagemaker-multi-model-endpoints-backed-by-gpu-instances-1ec215886248)

## 将 Triton 推理服务器与 Amazon SageMaker 集成

[](https://ram-vegiraju.medium.com/?source=post_page-----1ec215886248--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----1ec215886248--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ec215886248--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ec215886248--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----1ec215886248--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ec215886248--------------------------------) ·阅读时间 7 分钟·2023 年 9 月 22 日

--

![](img/a0b73ef0eb92e36d0a4cc5bd7159c302.png)

图片来源于 [Unsplash](https://unsplash.com/photos/6b5uqlWabB0)

过去，我们曾探索过**SageMaker 多模型端点 (MME)**作为在单一端点后面托管多个模型的经济有效的选项。虽然在 MME 上托管较小的模型可以使用基于 CPU 的实例，但随着这些模型变得越来越大和复杂，有时可能需要 GPU 计算。

[MME 由 GPU](https://aws.amazon.com/about-aws/whats-new/2022/10/amazon-sagemaker-cost-effectively-host-1000s-gpu-multi-model-endpoint/) 支持的实例是一项特定的 SageMaker 推理功能，我们将在本文中利用它展示如何在单个端点上高效托管数百个 NLP 模型。请注意，在本文发布时，SageMaker 上的 MME GPU 目前支持以下单 GPU 基于的实例系列：p2、p3、g4dn 和 g5。

MME GPU 目前还由两个模型服务堆栈提供支持：

1.  [Nvidia Triton 推理服务器](https://aws.amazon.com/blogs/machine-learning/run-multiple-deep-learning-models-on-gpu-with-amazon-sagemaker-multi-model-endpoints/)

1.  [TorchServe](https://aws.amazon.com/blogs/machine-learning/run-multiple-generative-ai-models-on-gpu-using-amazon-sagemaker-multi-model-endpoints-with-torchserve-and-save-up-to-75-in-inference-costs/)

为了本文的目的，我们将利用 Triton 推理服务器和 PyTorch 后端，在我们的 GPU 实例上托管基于 BERT 的模型。如果你对 Triton 不太熟悉，我们会稍作介绍，但我建议参考我的入门文章 这里。

**注意**：本文假设你对 SageMaker 部署和实时推理有中级理解。我建议你参考这篇 [文章](https://aws.amazon.com/blogs/machine-learning/part-2-model-hosting-patterns-in-amazon-sagemaker-getting-started-with-deploying-real-time-models-on-sagemaker/) 以深入了解部署/推理。我们也会概述多模型端点，但要进一步了解，请参考这份 [文档](https://docs.aws.amazon.com/sagemaker/latest/dg/multi-model-endpoints.html)。

**免责声明**：我是 AWS 的机器学习架构师，我的观点仅代表我个人。

# 什么是 MME？解决方案概述

为什么使用多模型端点（MME），以及何时使用它们？MME 是一种成本和管理上有效的托管选项。传统的 SageMaker 端点设置将如下所示：

![](img/f71e9966193967b59f4847d9aed6a7c5.png)

作者提供的图片

当你有数百或甚至数千个模型时，管理如此多的不同端点变得困难，而且你需要为每个持久端点背后的硬件付费。使用 MME，这变得简单，因为你只需要管理一个端点和一组硬件：

![](img/ab1e589097260a422fc18cced36d56fc.png)

作者提供的图片

你可以将这些不同的模型打包成一个模型 tarball（model.tar.gz）。这个模型 tarball 实质上会包含所有模型元数据，格式符合模型服务解决方案的要求。在这个例子中，我们使用 Triton 作为我们的模型服务器，所以我们的 model.tar.gz 将如下所示：

```py
- model.tar.gz/
  - linear_regression_model
    - 1
       - model.pt
       - model.py (optional, not included here)
    - config.pbtxt
```

在这个例子中，我们将制作 200 份我们的模型 tarball，以展示如何在单个端点上托管多个模型。对于实际应用，这些 tarballs 会根据你推送到端点后的模型有所不同。这些 tarballs 都被捕获在 SageMaker 能理解的一个通用 S3 路径中：

![](img/35de9236cb5faa9a94d852feb2bf5645.png)

MME 分桶（作者提供的图片）

MME 背后的模型如何管理？SageMaker MME 将接收请求并动态加载和缓存你调用的特定模型。如果你期望你的端点会有大量流量，最好在端点后面配置多个初始实例或设置自动扩展。例如，如果单个模型接收了大量调用，这个模型将被加载到另一个实例上，以处理额外的流量。要进一步了解 SageMaker MME 的负载测试，请参考这个 指南。

# 本地设置与测试

在这个例子中，我们将在一个 SageMaker Classic Notebook 实例中工作，使用 conda_python3 内核和 g4dn.4xlarge 实例。我们使用基于 GPU 的实例在本地测试 Triton，然后再部署到 SageMaker 实时推理中。

在这个例子中，我们使用了流行的 [BERT 模型](https://huggingface.co/bert-base-uncased)。我们首先要创建本地模型文件，因此我们使用 PyTorch 进行跟踪，然后保存序列化的模型文件。

```py
import torch
from transformers import BertModel, BertTokenizer
device = "cuda" if torch.cuda.is_available() else "cpu"

# Load bert model and tokenizer
model_name = 'bert-base-uncased'
model = BertModel.from_pretrained(model_name, torchscript = True)
tokenizer = BertTokenizer.from_pretrained(model_name)

# Sample Input
text = "I am super happy right now to be trying out BERT."

# Tokenize sample text
inputs = tokenizer(text, return_tensors="pt", padding=True, truncation=True)

# jit trace model
traced_model = torch.jit.trace(model, (inputs["input_ids"], inputs["attention_mask"]))

# Save traced model
torch.jit.save(traced_model, "model.pt")
```

我们可以通过加载保存的模型并用标记化的文本运行示例推理来确认模型推理是否正常。

```py
# sample inference with loaded model
loaded_model = torch.jit.load("model.pt")
res = loaded_model(input_ids=inputs["input_ids"], attention_mask=inputs["attention_mask"])
res
```

我们现在可以专注于设置 Triton 来托管这个特定的模型。为什么在实现 SageMaker 之前需要 本地测试 Triton？我们希望在创建 SageMaker 端点之前捕获任何设置问题。创建 SageMaker 端点可能需要几分钟，直到你在日志中看到失败之前，你无法知道你的设置中出现了什么问题，即使是一个小的脚本错误或模型 tarball 的不当结构。通过首先本地测试 Triton，我们可以快速迭代我们的配置和模型文件，以捕获任何错误。

对于 Triton，我们首先需要一个 config.pbtxt 文件。这捕获了我们的输入和输出维度以及其他你希望调整的 Triton 服务器属性。在这种情况下，我们可以从描述 BERT 架构的 transformers 库中获取输入和输出形状。

```py
from transformers import BertConfig
bert_config = BertConfig.from_pretrained(model_name)
max_sequence_length = bert_config.max_position_embeddings
output_shape = bert_config.hidden_size
print(f"Maximum Input Sequence Length: {max_sequence_length}")
print(f"Output Shape: {output_shape}")
```

我们可以使用这些值来创建我们的 config.pbtxt 文件。

```py
name: "bert_model"
platform: "pytorch_libtorch"

input [
  {
    name: "input_ids"
    data_type: TYPE_INT32
    dims: [1, 512]
  },
  {
    name: "attention_mask"
    data_type: TYPE_INT32
    dims: [1, 512]
  }
]

output [
  {
    name: "OUTPUT"
    data_type: TYPE_FP32
    dims: [512, 768]
  }
]
```

然后我们用以下 Docker 命令启动 Triton 推理服务器，指向我们的模型库。

```py
docker run --gpus all --rm -p 8000:8000 -p 8001:8001 -p 8002:8002 -v
/home/ec2-user/SageMaker:/models nvcr.io/nvidia/tritonserver:23.08-py3
tritonserver --model-repository=/models --exit-on-error=false --log-verbose=1
```

一旦容器启动，你可以进行示例请求以确保我们能够成功地使用现有模型文件进行推理。

```py
import requests
import json

# Specify the model name and version
model_name = "bert_model" #specified in config.pbtxt
model_version = "1"

# Set the inference URL based on the Triton server's address
url = f"http://localhost:8000/v2/models/{model_name}/versions/{model_version}/infer"

# sample invoke
output = requests.post(url, data=json.dumps(payload))
res = output.json()
```

一旦这项工作成功进行，我们可以专注于 SageMaker MME 部署。

# SageMaker MME GPU 部署

现在我们已经将模型文件转换为模型服务器所理解的格式，我们可以将其封装成 SageMaker 预期的 model.tar.gz 格式。

```py
!tar -cvzf model.tar.gz bert_model/
```

我们还在一个公共 S3 路径中创建了 200 个模型副本，以支持我们的 MME。

```py
%%time
# we make a 200 copies of the tarball, this will take about ~6 minutes to finish (can vary depending on model size)
for i in range(200):
    with open("model.tar.gz", "rb") as f:
        s3_client.upload_fileobj(f, bucket, "{}/model-{}.tar.gz".format(s3_model_prefix,i))
```

除了模型文件的位置，我们还需要指定我们用于 SageMaker 部署的管理 Triton 容器。

```py
triton_image_uri = "{account_id}.dkr.ecr.{region}.{base}/sagemaker-tritonserver:23.07-py3".format(
    account_id=account_id_map[region], region=region, base=base
)

print(f"Triton Inference server DLC image: {triton_image_uri}")
```

接下来的几个步骤是常见的 SageMaker 端点创建流程：

+   [SageMaker 模型](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html)：指向我们的模型数据和容器。

+   [SageMaker 端点配置](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html)：指定我们的实例类型和数量。

+   [SageMaker 端点](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html)：用于调用的 REST 端点。

在我们的 EndpointConfiguration 对象中，我们指定了基于 GPU 的实例：在本例中为 g4dn.4xlarge。

```py
endpoint_config_response = client.create_endpoint_config(
    EndpointConfigName=endpoint_config_name,
    ProductionVariants=[
        {
            "VariantName": "tritontraffic",
            "ModelName": model_name,
            "InstanceType": "ml.g4dn.4xlarge",
            "InitialInstanceCount": 1,
            "InitialVariantWeight": 1
        },
    ],
)

endpoint_name = "triton-mme-gpu-ep" + time.strftime("%Y-%m-%d-%H-%M-%S", time.gmtime())

create_endpoint_response = client.create_endpoint(
    EndpointName=endpoint_name, EndpointConfigName=endpoint_config_name
)

print("Endpoint Arn: " + create_endpoint_response["EndpointArn"])
```

端点可能需要几分钟来创建，但创建完成后，你应该能够运行示例推理。在 TargetModel 头部，你可以指定 1 到 200 之间的任何模型，因为我们将这个范围设定为不同 model.tar.gz 文件的分隔符。

```py
response = runtime_client.invoke_endpoint(
    EndpointName=endpoint_name, ContentType="application/octet-stream", 
    Body=json.dumps(payload), TargetModel='model-199.tar.gz'
)
print(json.loads(response["Body"].read().decode("utf8")))
```

在运行推理时，你也可以通过 CloudWatch 监控硬件和调用指标。特别是由于这是一个基于 GPU 的端点，你可以通过 API 或 SageMaker 控制台监控 GPU 使用率。

![](img/5f0bfb6f07494571531b0059638ac3d1.png)

SageMaker 控制台的监控标签（作者截图）

![](img/9efdfa132b4a7e3f60ddd54213680246.png)

硬件 GPU 指标（作者截图）

要了解所有其他 MME CloudWatch 指标，请参阅以下[文档](https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html#cloudwatch-metrics-multimodel-endpoints)。

# 附加资源与结论

[](https://github.com/RamVegiraju/SageMaker-Deployment/blob/master/RealTime/Multi-Model-Endpoint/Triton-MME-GPU/mme-gpu-bert.ipynb?source=post_page-----1ec215886248--------------------------------) [## SageMaker-Deployment/RealTime/Multi-Model-Endpoint/Triton-MME-GPU/mme-gpu-bert.ipynb at master ·…

### 对 SageMaker 推理选项及其他功能的示例汇编。…

github.com](https://github.com/RamVegiraju/SageMaker-Deployment/blob/master/RealTime/Multi-Model-Endpoint/Triton-MME-GPU/mme-gpu-bert.ipynb?source=post_page-----1ec215886248--------------------------------)

示例的完整代码可以在上述链接中找到。MME 已经是一个非常强大的功能，但当与基于 GPU 的硬件配合使用时，可以让我们在 NLP 和 CV 领域托管更大的模型。Triton 也是一个动态服务选项，它支持多种不同的框架和多样的硬件，从而大大增强了我们的 MME 应用程序。有关更多 SageMaker 推理示例，请参阅以下[链接](https://github.com/RamVegiraju/SageMaker-Deployment)。如果你有兴趣更好地了解 Triton，请参阅我的 PyTorch 模型入门指南。

一如既往，感谢你的阅读，欢迎留下任何反馈。

*如果你喜欢这篇文章，欢迎在* [*LinkedIn*](https://www.linkedin.com/in/ram-vegiraju-81272b162/) *上与我联系，并订阅我的 Medium* [*Newsletter*](https://ram-vegiraju.medium.com/subscribe)*。如果你是 Medium 的新用户，可以使用我的* [*会员推荐链接*](https://ram-vegiraju.medium.com/membership)*注册。*
