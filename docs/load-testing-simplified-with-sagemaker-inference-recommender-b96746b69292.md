# 使用 SageMaker 推理推荐器简化负载测试

> 原文：[`towardsdatascience.com/load-testing-simplified-with-sagemaker-inference-recommender-b96746b69292`](https://towardsdatascience.com/load-testing-simplified-with-sagemaker-inference-recommender-b96746b69292)

## 在 SageMaker 实时终端上测试 TensorFlow ResNet50

[](https://ram-vegiraju.medium.com/?source=post_page-----b96746b69292--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----b96746b69292--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b96746b69292--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b96746b69292--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----b96746b69292--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b96746b69292--------------------------------) ·阅读时间 7 分钟·2023 年 3 月 7 日

--

![](img/df9ae88a89ce233ff5f795f837fb53bf.png)

图片来源 [Unsplash](https://unsplash.com/photos/o4WaeT3XhV4)，作者 [**Amokrane Ait-Kaci**](https://unsplash.com/@amokraneaitk)

过去我曾广泛讨论过在将机器学习模型部署到生产环境之前进行 负载测试 的重要性。对于特定的实时推理使用案例，确保你的解决方案满足目标延迟和吞吐量是至关重要的。我们还探讨了如何使用 Python 库 [Locust](https://locust.io/) 来定义可以模拟预期流量模式的脚本。

虽然 Locust 是一个功能强大的工具，但它的设置可能会很复杂，并且需要针对不同的超参数和硬件进行大量的迭代，以确定生产环境中的适当配置。对于 [SageMaker 实时推理](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html)，一个关键工具是 [SageMaker 推理推荐器](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender.html)。你可以通过传递一个 EC2 实例类型数组来测试你的终端，以及针对你的特定模型容器的超参数，以便进行更高级的部署，而不是重复运行 Locust 脚本。在今天的博客中，我们将探讨如何配置这一功能以及如何简化 SageMaker 实时终端的负载测试。

**注意**：本文假定读者具有 AWS、SageMaker 和 Python 的基础知识。要了解 SageMaker 实时推理，请查看以下入门 [博客](https://aws.amazon.com/blogs/machine-learning/part-2-model-hosting-patterns-in-amazon-sagemaker-getting-started-with-deploying-real-time-models-on-sagemaker/)。

## 设置并在本地测试推理

在开发过程中，你可以使用 SageMaker Classic Notebook 实例或 SageMaker Studio Kernel。对于我们的环境，我们使用了一个 TensorFlow 2.0 Kernel，基于 Python3 的 ml.t3.medium 实例。

今天我们将使用的是一个预训练的 [TensorFlow ResNet50](https://www.tensorflow.org/api_docs/python/tf/keras/applications/resnet50/ResNet50) 图像分类模型。我们可以首先从 TensorFlow 模型中心获取该模型，并将其导入到我们的笔记本中。

```py
import os
import tensorflow as tf
from tensorflow.keras.applications import resnet50
from tensorflow.keras import backend
import numpy as np
from tensorflow.keras.preprocessing import image
```

在我们开始在 SageMaker 上进行测试之前，我们希望在本地测试该模型，以便了解我们需要为端点配置的输入格式。对于我们的样本数据点，我们将使用我狗狗 Milo 当小狗时的图片（现在他已经变成一个庞然大物了）。

![](img/46e9a8cf71551b16a89d46b3141bc0a2.png)

Milo（作者图片）

```py
#model = tf.keras.applications.ResNet50()
tf.keras.backend.set_learning_phase(0)
model = resnet50.ResNet50()

# Load the image file, resizing it to 224x224 pixels (required by this model)
img = image.load_img("dog.jpg", target_size=(224, 224))
# Convert the image to a numpy array
x = image.img_to_array(img)
# Add a forth dimension since Keras expects a list of images
x = np.expand_dims(x, axis=0)

# Scale the input image to the range used in the trained network
x = resnet50.preprocess_input(x)

print("predicting model")
predictions = model.predict(x)
predicted_classes = resnet50.decode_predictions(predictions, top=9)
print(predicted_classes)
```

![](img/e4708dc24bf777ae7a5479714f5cd68e.png)

模型结果（作者截图）

我们现在已经验证了模型期望的推理格式，因此可以集中精力将其配置到 SageMaker 上。

## 准备模型和负载

SageMaker Inference Recommender 需要两个必需的输入：模型数据和一个示例负载。它们都需要以 tarball 格式提供，因此我们将工件压缩成服务可以理解的格式。

对于我们的模型，我们可以使用之前在笔记本中加载的模型，或者实例化一个新版本。我们将模型工件下载到一个本地目录，并包含 TensorFlow Serving 所需的元数据。

```py
export_dir = "00001"
tf.keras.backend.set_learning_phase(0)
model = tf.keras.applications.ResNet50()

if not os.path.exists(export_dir):
    os.makedirs(export_dir)
    print("Directory ", export_dir, " Created ")
else:
    print("Directory ", export_dir, " already exists")
# Save to SavedModel
model.save(export_dir, save_format="tf", include_optimizer=False)
```

然后我们可以将其打包成 model.tar.gz，并上传到可以供 Inference Recommender 指向的 S3 存储桶中。

```py
!tar -cvpzf model.tar.gz ./00001

#upload data to S3
model_url = sagemaker_session.upload_data(
    path="model.tar.gz", key_prefix="resnet-model-data"
)
```

然后我们将样本图像转换为 JSON 格式，以供我们的模型使用，并以类似于处理模型工件的方式将其保存到 tarball 中。

```py
import json
payload = json.dumps(x.tolist())
payload_archive_name = "payload.tar.gz"

with open("payload.json", "w") as outfile:
    outfile.write(payload)

#create payload tarball
!tar -cvzf {payload_archive_name} payload.json

#upload sample payload to S3
sample_payload_url = sagemaker_session.upload_data(
    path=payload_archive_name, key_prefix="resnet-payload"
)
```

现在我们已经配置了输入，可以继续进行项目的 SageMaker 部分。

## 创建 SageMaker 模型并使用模型注册表进行跟踪

SageMaker 有一些特定于其服务的对象，其中对我们来说重要的是[SageMaker 模型](https://sagemaker.readthedocs.io/en/stable/api/inference/model.html)实体。这个模型实体包括两个核心因素：模型数据和容器/镜像。模型数据可以是你在 S3 桶中提供的训练或预训练模型工件。容器本质上是你模型的框架。在这种情况下，我们可以[获取](https://aws.plainenglish.io/how-to-retrieve-amazon-sagemaker-deep-learning-images-ff4a5866299e)托管的 SageMaker TensorFlow 镜像，但如果不受[AWS 深度学习容器](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)支持，你也可以构建并推送自己的容器。在这里我们定义了这个 SageMaker [模型对象](https://sagemaker.readthedocs.io/en/stable/api/inference/model.html#)，利用 SageMaker Python SDK。

```py
import sagemaker
from sagemaker.model import Model
from sagemaker import image_uris

model = Model(
    model_data=model_url,
    role=role,
    image_uri = sagemaker.image_uris.retrieve(framework="tensorflow", region=region, version="2.1", py_version="py3", 
                                              image_scope='inference', instance_type="ml.m5.xlarge"),
    sagemaker_session=sagemaker_session
    )
```

一个可选步骤是将你的模型注册到 SageMaker 模型注册表。跟踪数百个模型可能是一个困难的过程，通过模型注册表，你可以简化模型版本控制和血统，使所有模型实体都集中在一个核心空间。我们可以通过以下 API 调用来注册模型。

```py
model_package = model.register(
    content_types=["application/json"],
    response_types=["application/json"],
    model_package_group_name=model_package_group_name,
    image_uri=model.image_uri,
    approval_status="Approved",
    framework="TENSORFLOW"
)
```

我们还可以在 SageMaker Studio 控制台中查看刚刚创建的模型包。

![](img/d0c146b9c3e90bdeb42b526f1a62499a.png)

模型包（作者截图）

在实际应用中，你可能在一个模型包内有多个模型，你可以选择批准部署到生产环境的模型。

现在我们已经准备好了模型对象，我们可以在这个实体上运行推理推荐工作。

## 推理推荐工作

有两种类型的推理推荐工作：默认和[高级](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-load-test.html)。使用默认工作，我们可以简单地传入样本负载以及一组你想测试模型的 EC2 实例。推理推荐器将在这些实例上测试你的模型，并跟踪吞吐量和延迟。我们可以利用[right_size](https://sagemaker.readthedocs.io/en/stable/api/inference/model.html#sagemaker.model.Model.right_size) API 调用来启动推理推荐工作。

```py
model_package.right_size(
    sample_payload_url=sample_payload_url,
    supported_content_types=["application/json"],
    supported_instance_types=["ml.c5.xlarge", "ml.c5.9xlarge", "ml.c5.18xlarge", "ml.m5d.24xlarge"],
    framework="TENSORFLOW",
)
```

这个工作大约需要 35-40 分钟完成，因为它将遍历你提供的不同实例类型。然后我们可以在 SageMaker Studio UI 中查看这些结果。

![](img/5cd28ba093d5a8c81dc468fd7e996f60.png)

默认工作结果（作者截图）

在这里，你可以根据重要性级别切换成本、延迟和吞吐量，以获得最佳的硬件配置。如果对测试显示的性能满意，你还可以直接从控制台创建端点。

最后，如果你想测试容器的不同超参数，可以通过高级推理推荐作业来实现。在这里，你可以指定适用于特定模型容器的可调超参数。

```py
from sagemaker.parameter import CategoricalParameter 
from sagemaker.inference_recommender.inference_recommender_mixin import (  
    Phase,  
    ModelLatencyThreshold 
) 

hyperparameter_ranges = [ 
    { 
        "instance_types": CategoricalParameter(["ml.m5.xlarge", "ml.g4dn.xlarge"]), 
        'OMP_NUM_THREADS': CategoricalParameter(['1', '2', '3']), 
    } 
] 
```

除此之外，你还可以配置负载测试的流量模式。例如，如果你想在不同时间间隔内增加用户数量，可以配置这种行为。

```py
phases = [ 
    Phase(duration_in_seconds=120, initial_number_of_users=2, spawn_rate=2), 
    Phase(duration_in_seconds=120, initial_number_of_users=6, spawn_rate=2) 
]
```

你还可以设置阈值，例如，如果你有严格的 200 毫秒延迟要求，如果你的配置未达到这些结果，可以将其设置为停止参数。

```py
model_latency_thresholds = [ 
    ModelLatencyThreshold(percentile="P95", value_in_milliseconds=300) 
]
```

然后，你可以以类似于默认作业的方式启动并查看高级作业的结果。

```py
model_package.right_size( 
    sample_payload_url=sample_payload_url, 
    supported_content_types=["application/json"], 
    framework="TENSORFLOW", 
    job_duration_in_seconds=3600, 
    hyperparameter_ranges=hyperparameter_ranges, 
    phases=phases, # TrafficPattern 
    max_invocations=100, # StoppingConditions 
    model_latency_thresholds=model_latency_thresholds
)
```

## 额外资源与结论

[## sagemaker-inference-recommender-examples/inference-recommender-tf-resnet.ipynb at main ·…](https://github.com/aws-samples/sagemaker-inference-recommender-examples/blob/main/tf-resnet/inference-recommender-tf-resnet.ipynb?source=post_page-----b96746b69292--------------------------------)

### 通过在 GitHub 上创建账户，为 aws-samples/sagemaker-inference-recommender-examples 的开发做出贡献。

[GitHub 地址](https://github.com/aws-samples/sagemaker-inference-recommender-examples/blob/main/tf-resnet/inference-recommender-tf-resnet.ipynb?source=post_page-----b96746b69292--------------------------------)

你可以在上面的链接中找到此示例的代码及更多内容。SageMaker 推理推荐器是一个强大的工具，可以自动化负载测试设置的复杂部分。然而，需要注意的是，目前不支持如多模型和多容器端点这样的高级托管选项，因此对于这些用例，使用像 Locust 这样的第三方框架将是必要的。如往常一样，任何反馈都是受欢迎的，欢迎随时提出问题或评论，感谢阅读！

*如果你喜欢这篇文章，欢迎通过* [*LinkedIn*](https://www.linkedin.com/in/ram-vegiraju-81272b162/) *与我联系，并订阅我的 Medium* [*Newsletter*](https://ram-vegiraju.medium.com/subscribe)*。如果你是 Medium 新用户，可以使用我的* [*会员推荐链接*](https://ram-vegiraju.medium.com/membership)*注册。*
