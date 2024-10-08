# 使用 HuggingFace TGI 部署大型语言模型

> 原文：[`towardsdatascience.com/deploying-large-language-models-with-huggingface-tgi-981747c669e3`](https://towardsdatascience.com/deploying-large-language-models-with-huggingface-tgi-981747c669e3)

## 使用 Amazon SageMaker 高效托管和扩展您的 LLMs 的另一种方法

[](https://ram-vegiraju.medium.com/?source=post_page-----981747c669e3--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----981747c669e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----981747c669e3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----981747c669e3--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----981747c669e3--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----981747c669e3--------------------------------) ·阅读时间 5 分钟·2023 年 7 月 14 日

--

![](img/87080a29c8a17cddf9ed8b4ece860f12.png)

图片来自 [Unsplash](https://unsplash.com/photos/4NYtYSiZVlA)

大型语言模型（LLMs）的受欢迎程度持续上升，几乎每周都有新的模型发布。随着这些模型数量的增加，我们托管它们的选择也越来越多。在我之前的文章中，我们探讨了如何在 Amazon SageMaker 中利用 [DJL Serving](https://github.com/deepjavalibrary/djl-serving) 来高效托管 LLMs。在这篇文章中，我们将探索另一种优化的模型服务器和解决方案——[HuggingFace 文本生成推理（TGI）](https://github.com/huggingface/text-generation-inference)。

**注意**：如果你是 AWS 新手，请确保你在以下 [链接](https://aws.amazon.com/console/) 创建一个账户，以便跟进本文。文章还假设你对 SageMaker 部署有一定的了解，我建议你阅读这篇 [文章](https://aws.amazon.com/blogs/machine-learning/part-2-model-hosting-patterns-in-amazon-sagemaker-getting-started-with-deploying-real-time-models-on-sagemaker/) 以更深入地了解部署/推理。

**免责声明**：我是在 AWS 的机器学习架构师，我的观点仅代表个人意见。

## 为什么选择 HuggingFace 文本生成推理？它如何与 Amazon SageMaker 配合使用？

TGI 是由 HuggingFace 创建的 Rust、Python、gRPC 模型服务器，旨在托管特定的大型语言模型。HuggingFace 一直是 NLP 的核心枢纽，它在 LLMs 上包含了大量优化，下面列出了一些，详细的优化列表请参见 [文档](https://github.com/huggingface/text-generation-inference#features)。

+   在多个 GPU 上高效托管的张量并行

+   使用 SSE 进行令牌流媒体

+   使用 [bitsandbytes](https://github.com/TimDettmers/bitsandbytes) 进行量化

+   [Logits warper](https://huggingface.co/docs/transformers/internal/generation_utils#transformers.LogitsProcessor)（不同的参数，例如温度、top-k、top-n 等）

我注意到这个解决方案的一个很大优点是使用的简便性。目前，TGI 支持以下优化的模型架构，你可以直接使用 TGI 容器进行部署。

+   [BLOOM](https://huggingface.co/bigscience/bloom)

+   [FLAN-T5](https://huggingface.co/google/flan-t5-xxl)

+   [Galactica](https://huggingface.co/facebook/galactica-120b)

+   [GPT-Neox](https://huggingface.co/EleutherAI/gpt-neox-20b)

+   [Llama](https://github.com/facebookresearch/llama)

+   [OPT](https://huggingface.co/facebook/opt-66b)

+   [SantaCoder](https://huggingface.co/bigcode/santacoder)

+   [Starcoder](https://huggingface.co/bigcode/starcoder)

+   [Falcon 7B](https://huggingface.co/tiiuae/falcon-7b)

+   [Falcon 40B](https://huggingface.co/tiiuae/falcon-40b)

更为方便的是，它可以直接与 Amazon SageMaker 集成，正如我们将在本文中探讨的那样。SageMaker 现在提供了托管的 [TGI 容器](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#huggingface-text-generation-inference-containers)，我们将 [检索](https://aws.plainenglish.io/how-to-retrieve-amazon-sagemaker-deep-learning-images-ff4a5866299e) 并使用这些容器来在本示例中部署 [Llama 模型](https://github.com/facebookresearch/llama)。

## TGI 与 DJL Serving 与 JumpStart 的比较

到目前为止，在这个 LLM 托管系列中，我们已经探讨了两种 Amazon SageMaker 的替代选项：

1.  DJL Serving

1.  SageMaker JumpStart

**我们何时使用什么，TGI 在其中扮演什么角色？** 如果模型已经在 SageMaker JumpStart 的服务中提供，那么 JumpStart 非常适合。你实际上不需要处理任何容器或模型服务器级别的工作，这些都为你抽象化了。如果你有一个不被 JumpStart 支持的自定义用例，DJL Serving 是一个很好的选择。你可以直接从 HuggingFace Hub 拉取，或者加载自己在 S3 中的工件并指向它。另一个优点是，如果你掌握了特定的模型分区框架，例如 Accelerate 或 FasterTransformers，DJL Serving 与这些框架集成，可以让你为 LLM 托管添加一些高级性能优化技术。

TGI 的作用是什么？正如我们所提到的，TGI 原生支持特定的模型架构。如果你尝试部署这些支持的模型，TGI 是一个很好的选择，因为它专门为这些架构进行了优化。此外，如果你更深入地了解模型服务器，你可以调整一些暴露的环境变量。可以将 TGI 视为易用性和性能优化之间的交集。根据你的用例及权衡每种解决方案的优缺点，选择合适的选项对于你的部署至关重要。

## 在 Amazon SageMaker 上使用 TGI 部署 Llama

为了与 Amazon SageMaker 配合工作，我们将利用[SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/)来获取我们的 TGI 容器并简化部署。在这个示例中，我们将遵循 Phillip Schmid 的[TGI 博客](https://huggingface.co/blog/sagemaker-huggingface-llm)并将其调整为 Llama。确保首先安装最新版本的 SageMaker Python SDK 并设置必要的客户端以便与服务配合使用。

```py
!pip install "sagemaker==2.163.0" --upgrade --quiet

import sagemaker
import boto3
sess = sagemaker.Session()

sagemaker_session_bucket=None
if sagemaker_session_bucket is None and sess is not None:
    # set to default bucket if a bucket name is not given
    sagemaker_session_bucket = sess.default_bucket()

try:
    role = sagemaker.get_execution_role()
except ValueError:
    iam = boto3.client('iam')
    role = iam.get_role(RoleName='sagemaker_execution_role')['Role']['Arn']

sess = sagemaker.Session(default_bucket=sagemaker_session_bucket)

print(f"sagemaker role arn: {role}")
print(f"sagemaker session region: {sess.boto_region_name}")
```

接下来我们[检索](https://aws.plainenglish.io/how-to-retrieve-amazon-sagemaker-deep-learning-images-ff4a5866299e)部署 TGI 所需的 SageMaker 容器。AWS 管理一组[深度学习容器](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)（你也可以自带容器），这些容器与不同的模型服务器如 TGI、DJL Serving、TorchServe 等集成。在这种情况下，我们将 SDK 指向我们需要的 TGI 版本。

```py
from sagemaker.huggingface import get_huggingface_llm_image_uri

# retrieve the llm image uri
llm_image = get_huggingface_llm_image_uri(
  "huggingface",
  version="0.8.2"
)

# print ecr image uri
print(f"llm image uri: {llm_image}")
```

接下来是与 TGI 容器相关的具体魔法。TGI 的简单之处在于，我们只需提供特定 LLM 的模型中心 ID。请注意，它必须来自 TGI 支持的模型架构。在这种情况下，我们使用的是[Llama-7B 模型](https://huggingface.co/decapoda-research/llama-7b-hf)的一种形式。

```py
import json
from sagemaker.huggingface import HuggingFaceModel

# Define Model and Endpoint configuration parameter
config = {
  'HF_MODEL_ID': "decapoda-research/llama-7b-hf", # model_id from hf.co/models
  'SM_NUM_GPUS': json.dumps(number_of_gpu), # Number of GPU used per replica
  'MAX_INPUT_LENGTH': json.dumps(1024),  # Max length of input text
  'MAX_TOTAL_TOKENS': json.dumps(2048),  # Max length of the generation (including input text)
}
```

在上面的配置对象中，你还可以指定 TGI 容器支持的任何额外优化，例如量化格式（例如：[bitsandbytes](https://github.com/TimDettmers/bitsandbytes)）。然后我们将这个配置传递给 SageMaker 理解的 HuggingFace 模型对象。

```py
# create HuggingFaceModel with the image uri
llm_model = HuggingFaceModel(
  role=role,
  image_uri=llm_image,
  env=config
)
```

然后我们可以直接将这个模型对象部署到 SageMaker 实时端点。在这里你可以指定你的硬件，对于这些较大的模型，建议使用如 g5 实例的 GPU 系列。由于这些模型的大小，我们还启用了更长的容器健康检查超时时间。

```py
instance_type = "ml.g5.12xlarge"
number_of_gpu = 4
health_check_timeout = 300

llm = llm_model.deploy(
  initial_instance_count=1,
  instance_type=instance_type,
  container_startup_health_check_timeout=health_check_timeout,
)
```

创建端点应该需要几分钟时间，但随后你应该能够使用以下代码进行样本推断。

```py
llm.predict({
    "inputs": "My name is Julien and I like to",
})
```

## 附加资源与结论

[](https://github.com/RamVegiraju/SageMaker-Deployment/blob/master/LLM-Hosting/TGI/tgi-llama.ipynb?source=post_page-----981747c669e3--------------------------------) [## SageMaker-Deployment/LLM-Hosting/TGI/tgi-llama.ipynb 在 master 分支 · RamVegiraju/SageMaker-Deployment

### 对 SageMaker 推理选项和其他功能的示例进行了汇编。…

github.com](https://github.com/RamVegiraju/SageMaker-Deployment/blob/master/LLM-Hosting/TGI/tgi-llama.ipynb?source=post_page-----981747c669e3--------------------------------)

你可以在上述链接找到整个示例的代码，也可以从 HuggingFace Hub 的 SageMaker 部署选项卡直接生成此代码以支持你的模型。文本生成推理提供了一个高度优化的模型服务器，这也大大简化了部署过程。请继续关注更多关于 LLM 领域的内容，感谢阅读，欢迎随时留下反馈。

*如果你喜欢这篇文章，欢迎通过* [*LinkedIn*](https://www.linkedin.com/in/ram-vegiraju-81272b162/) *与我联系，并订阅我的 Medium* [*Newsletter*](https://ram-vegiraju.medium.com/subscribe)*。如果你是 Medium 新手，可以通过我的* [*会员推荐*](https://ram-vegiraju.medium.com/membership)*注册。*
