# 在 Amazon SageMaker 上使用 DJL Serving 部署 LLMs

> 原文：[`towardsdatascience.com/deploying-llms-on-amazon-sagemaker-with-djl-serving-8220e3cfad0c`](https://towardsdatascience.com/deploying-llms-on-amazon-sagemaker-with-djl-serving-8220e3cfad0c)

## 在 Amazon SageMaker 实时推理中部署 BART

[](https://ram-vegiraju.medium.com/?source=post_page-----8220e3cfad0c--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----8220e3cfad0c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8220e3cfad0c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8220e3cfad0c--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----8220e3cfad0c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8220e3cfad0c--------------------------------) ·8 分钟阅读·2023 年 6 月 7 日

--

![](img/27d3d2701b592cc5346c24eb2c67afe6.png)

图片来源：[Unsplash](https://unsplash.com/photos/CejqWHRRXUQ)

在 2023 年，大语言模型（LLMs）和生成式 AI 继续在机器学习和一般技术领域中占据主导地位。随着 LLM 的扩展，新模型不断涌现，并以惊人的速度不断改进。

尽管这些模型的准确性和性能令人难以置信，但在托管这些模型时仍面临着一系列挑战。如果没有模型托管，很难识别这些大语言模型（LLMs）在实际应用中所提供的价值。LLM 托管和性能调优的具体挑战是什么？

+   我们如何加载这些体积达到数百 GB 的大型模型？

+   我们如何正确应用模型分区技术，以高效利用硬件而不影响模型的准确性？

+   我们如何将这些模型适配到单个 GPU 或多个 GPU 上？

这些都是具有挑战性的问题，通过一个被称为 [DJL Serving](http://djl.ai/serving/) 的模型服务器进行解决和抽象。DJL Serving 是一个高性能的通用解决方案，可以直接与各种模型分区框架集成，例如：[HuggingFace Accelerate](https://huggingface.co/docs/accelerate/index)、[DeepSpeed](https://github.com/microsoft/DeepSpeed) 和 [FasterTransformers](https://github.com/NVIDIA/FasterTransformer)。使用 DJL Serving，你可以配置你的服务堆栈，利用这些分区框架在多个 GPU 上优化大模型的推理。

在今天的文章中，我们特别探讨了 [BART](https://huggingface.co/facebook/bart-large) 中的一个较小的语言模型用于特征提取。我们将展示如何使用 DJL Serving 配置你的服务堆栈并托管你选择的[HuggingFace 模型](https://huggingface.co/)。这个示例可以作为一个模板，构建和利用上述的模型分区框架。然后我们将把 DJL 特定代码与 SageMaker 集成，创建一个[实时端点](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html)，供你进行推理使用。

**注意**：对于那些新接触 AWS 的朋友，如果你想跟随本文操作，请确保在以下[链接](https://aws.amazon.com/console/)上创建一个账户。本文假设你对 SageMaker 部署有中级理解，建议你阅读这篇[文章](https://aws.amazon.com/blogs/machine-learning/part-2-model-hosting-patterns-in-amazon-sagemaker-getting-started-with-deploying-real-time-models-on-sagemaker/)以更深入理解部署/推理。

## 什么是模型服务器？Amazon SageMaker 支持哪些模型服务器？

[模型服务器](https://thenewstack.io/model-server-the-critical-building-block-of-mlops/)的基本概念是“作为服务的推理”。我们需要一种简便的方法通过 API 暴露我们的模型，而这些模型服务器则处理背后的繁重工作。这些模型服务器加载和卸载我们的模型工件，并提供托管 ML 模型所需的运行时环境。这些模型服务器也可以根据它们向用户暴露的内容进行调优。例如，TensorFlow Serving 允许你选择[gRPC 还是 REST](https://medium.com/@avidaneran/tensorflow-serving-rest-vs-grpc-e8cef9d4ff62)进行 API 调用。

Amazon SageMaker 与各种不同的模型服务器集成，这些服务器也通过 AWS 提供的不同[深度学习容器](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)进行暴露。这些模型服务器包括以下几种：

+   [TensorFlow Serving](https://www.tensorflow.org/tfx/guide/serving)

+   [TorchServe](https://pytorch.org/serve/)

+   [多模型服务器 (MMS)](https://github.com/awslabs/multi-model-server)

+   [Triton Inference Server](https://github.com/triton-inference-server/server)

+   [DJL Serving](https://github.com/deepjavalibrary/djl-serving)

在这个具体示例中，我们将使用 DJL Serving，因为它针对大语言模型托管进行了不同的模型分区框架。这并不意味着服务器仅限于 LLM，你也可以将其用于其他模型，只要你正确配置环境以安装和加载其他依赖项。

从很高的层次来看，具体使用的模型服务器会影响你提供给服务器的工件的构建和形状，这是唯一的区别，还有它们支持的模型框架和环境。

## DJL Serving 与 JumpStart

在我之前的文章中，我们探讨了如何通过 [SageMaker JumpStart](https://awstip.com/automl-beyond-with-sagemaker-jumpstart-9962ffc4bcd1) 部署 [Cohere 的语言模型](https://cohere.com/)。为什么这次不使用 SageMaker JumpStart？目前并非所有 LLM 都得到 SageMaker JumpStart 的支持。如果有特定的 LLM JumpStart 不支持，那么使用 DJL Serving 就是明智的选择。

DJL Serving 的另一个主要用例是当涉及到自定义和性能优化时。使用 JumpStart，你受限于模型提供以及已为你预先准备的容器的限制。使用 DJL，虽然在容器级别需要更多的代码工作，但你可以使用不同的分区框架应用你选择的性能优化技术。

## DJL Serving 设置

对于这个代码示例，我们将使用一个 ml.c5.9xlarge [SageMaker Classic Notebook 实例](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) 和一个 conda_amazonei_pytorch_latest_p37 内核进行开发。

在我们进入 DJL Serving 设置之前，我们可以快速探索 BART 模型。这个模型可以在 HuggingFace Model Hub 中找到，并可以用于各种任务，如特征提取和摘要。以下代码片段展示了如何在本地使用 BART Tokenizer 和 Model 进行示例推理。

```py
from transformers import AutoTokenizer, AutoModel

tokenizer = AutoTokenizer.from_pretrained("facebook/bart-large")
model = AutoModel.from_pretrained("facebook/bart-large")

inputs = tokenizer("Hello, my dog is cute", return_tensors="pt")
outputs = model(**inputs)

last_hidden_states = outputs.last_hidden_state
last_hidden_states
```

现在我们可以通过几个特定的文件将此模型映射到 DJL Serving。首先，我们定义一个 serving.properties 文件，该文件本质上定义了模型部署的配置。在这种情况下，我们指定了一些参数。

+   **引擎**：我们为 DJL 引擎使用 Python，其他选项还包括 DeepSpeed、FasterTransformers 和 Accelerate。

+   **模型 _ID**：对于 HuggingFace Hub，每个模型都有一个可以用作标识符的 model_id，我们可以将其传递到我们的模型脚本中用于模型加载。

+   **任务**：对于 HuggingFace 特定的模型，你可以包含一个任务，因为这些模型可以支持各种语言任务，在这种情况下，我们指定了特征提取。

```py
engine=Python
option.model_id=facebook/bart-large
option.task=feature-extraction
```

你可以为 DJL 指定的其他配置包括：tensor_parallel degree，每个模型的最小和最大工作线程。有关可以配置的属性的详细列表，请参阅以下[文档](https://docs.aws.amazon.com/sagemaker/latest/dg/large-model-inference-configuration.html)。

接下来我们提供的是实际的模型工件和一个 requirements.txt 文件，用于你在推理脚本中使用的任何额外库。

```py
numpy
```

在这种情况下，我们没有模型工件，因为我们将在推断脚本中直接从 HuggingFace Hub 加载模型。

在我们的推断脚本（model.py）中，我们可以创建一个类来捕获模型加载和推断。

```py
class BartModel(object):
    """
    Deploying Bart with DJL Serving
    """

    def __init__(self):
        self.initialized = False
```

我们的初始化方法将解析我们的 serving.properties 文件，并从 HuggingFace 模型库加载 BART 模型和分词器。属性对象本质上包含了你在 serving.properties 文件中定义的所有内容。

```py
def initialize(self, properties: dict):
        """
        Initialize model.
        """
        logging.info(properties)

        tokenizer = AutoTokenizer.from_pretrained("facebook/bart-large")
        model = AutoModel.from_pretrained("facebook/bart-large")
        self.model_name = properties.get("model_id")
        self.task = properties.get("task")
        self.model = AutoModel.from_pretrained(self.model_name)
        self.tokenizer = AutoTokenizer.from_pretrained(self.model_name)
        self.initialized = True
```

然后我们定义一个推断方法，该方法接受一个字符串输入，并对文本进行分词，以进行 BART 模型的推断，我们可以从上面的本地推断示例中复制。

```py
def inference(self, inputs):
        """
        Custom service entry point function.

        :param inputs: the Input object holds the text for the BART model to infer upon
        :return: the Output object to be send back
        """

        #sample input: "This is the sample text that I am passing in"

        try:
            data = inputs.get_as_string()
            inputs = self.tokenizer(data, return_tensors="pt")
            preds = self.model(**inputs)
            res = preds.last_hidden_state.detach().cpu().numpy().tolist() #convert to JSON Serializable object
            outputs = Output()
            outputs.add_as_json(res)

        except Exception as e:
            logging.exception("inference failed")
            # error handling
            outputs = Output().error(str(e))
```

我们然后实例化这个类，并在“handle”方法中捕获所有这些。默认情况下，对于 DJL Serving，这是处理程序在推断脚本中解析的方法。

```py
_service = BartModel()

def handle(inputs: Input):
    """
    Default handler function
    """
    if not _service.initialized:
        # stateful model
        _service.initialize(inputs.get_properties())

    if inputs.is_empty():
        return None

    return _service.inference(inputs)
```

现在我们在 DJL Serving 端已经拥有所有必要的工件，并可以配置这些文件以适应 SageMaker 构造，以创建实时终端节点。

## SageMaker 终端节点创建与推断

创建 SageMaker 终端节点的过程与其他模型服务器（如 MMS）非常相似。我们需要两个工件来创建一个 SageMaker 模型实体。

+   model.tar.gz: 这将包含我们特定于 DJL 的文件，我们将这些文件组织成模型服务器所期望的格式。

+   [容器镜像](https://aws.plainenglish.io/how-to-retrieve-amazon-sagemaker-deep-learning-images-ff4a5866299e): SageMaker 推断始终期望一个容器，在这种情况下我们使用 AWS 提供和维护的 DJL Deepseed 镜像。

我们可以创建模型 tarball，将其上传到 S3，然后检索镜像以准备推断所需的工件。

```py
import sagemaker, boto3
from sagemaker import image_uris

# retreive DeepSpeed image
img_uri = image_uris.retrieve(framework="djl-deepspeed", 
region=region, version="0.21.0")

# create model tarball
bashCommand = "tar -cvpzf model.tar.gz model.py requirements.txt serving.properties"
process = subprocess.Popen(bashCommand.split(), stdout=subprocess.PIPE)
output, error = process.communicate()

# Upload tar.gz to bucket
model_artifacts = f"s3://{bucket}/model.tar.gz"
response = s3.meta.client.upload_file('model.tar.gz', bucket, 'model.tar.gz')
```

我们可以利用[Boto3 SDK](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html)来进行[模型](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker/client/create_model.html)、[终端节点配置](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker/client/create_endpoint_config.html)和[终端节点](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker/client/create_endpoint.html)的创建。唯一不同于通常的三个 API 调用的是，在终端节点配置 API 调用中，我们将[模型下载超时](https://docs.aws.amazon.com/sagemaker/latest/dg/large-model-inference-hosting.html)和[容器健康检查超时](https://docs.aws.amazon.com/sagemaker/latest/dg/large-model-inference-hosting.html)参数设置为更高的值，因为我们在此情况下处理的是一个更大的模型。我们还利用了 g5 系列实例以获得额外的 GPU 计算能力。对于大多数 LLM 来说，GPU 是必需的，以便能够在这种规模下托管模型。

```py
client = boto3.client(service_name="sagemaker")

model_name = "djl-bart" + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
print("Model name: " + model_name)
create_model_response = client.create_model(
    ModelName=model_name,
    ExecutionRoleArn=role,
    PrimaryContainer={"Image": img_uri, "ModelDataUrl": model_artifacts},
)
print("Model Arn: " + create_model_response["ModelArn"])

endpoint_config_name = "djl-bart" + strftime("%Y-%m-%d-%H-%M-%S", gmtime())

production_variants = [
    {
        "VariantName": "AllTraffic",
        "ModelName": model_name,
        "InitialInstanceCount": 1,
        "InstanceType": 'ml.g5.12xlarge',
        "ModelDataDownloadTimeoutInSeconds": 1800,
        "ContainerStartupHealthCheckTimeoutInSeconds": 3600,
    }
]

endpoint_config = {
    "EndpointConfigName": endpoint_config_name,
    "ProductionVariants": production_variants,
}

endpoint_config_response = client.create_endpoint_config(**endpoint_config)
print("Endpoint Configuration Arn: " + endpoint_config_response["EndpointConfigArn"])

endpoint_name = "djl-bart" + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
create_endpoint_response = client.create_endpoint(
    EndpointName=endpoint_name,
    EndpointConfigName=endpoint_config_name,
)
print("Endpoint Arn: " + create_endpoint_response["EndpointArn"])
```

一旦创建了端点，我们可以利用 [invoke_endpoint](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-runtime/client/invoke_endpoint.html) API 调用进行样本推理，你应该会看到返回一个 numpy 数组。

```py
runtime = boto3.client(service_name="sagemaker-runtime")
response = runtime.invoke_endpoint(
    EndpointName=endpoint_name,
    ContentType="text/plain",
    Body="I think my dog is really cute!")
result = json.loads(response['Body'].read().decode())
```

## 附加资源与结论

[## SageMaker-Deployment/LLM-Hosting/DJL-Serving/BART at master · RamVegiraju/SageMaker-Deployment](https://github.com/RamVegiraju/SageMaker-Deployment/tree/master/LLM-Hosting/DJL-Serving/BART?source=post_page-----8220e3cfad0c--------------------------------)

### 目前无法执行该操作。你在另一个标签页或窗口中登录了。你在另一个标签页或窗口中注销了……

[github.com](https://github.com/RamVegiraju/SageMaker-Deployment/tree/master/LLM-Hosting/DJL-Serving/BART?source=post_page-----8220e3cfad0c--------------------------------)

你可以在上面的链接找到整个示例的代码。LLM Hosting 仍然是一个不断发展的领域，面临许多挑战，DJL Serving 可以帮助简化这些挑战。结合 SageMaker 提供的硬件和优化，这可以帮助提升你在 LLMs 上的推理性能。

一如既往，欢迎随时对文章提出反馈或问题。感谢阅读，敬请关注更多关于 LLM 领域的内容。

*如果你喜欢这篇文章，可以通过* [*LinkedIn*](https://www.linkedin.com/in/ram-vegiraju-81272b162/) *与我联系，并订阅我的 Medium* [*Newsletter*](https://ram-vegiraju.medium.com/subscribe)*。如果你是 Medium 新手，可以使用我的* [*Membership Referral*](https://ram-vegiraju.medium.com/membership)*进行注册。*
