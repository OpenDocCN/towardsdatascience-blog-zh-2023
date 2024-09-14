# 使用 Nvidia Triton Inference Server 部署 PyTorch 模型

> 原文：[`towardsdatascience.com/deploying-pytorch-models-with-nvidia-triton-inference-server-bb139066a387`](https://towardsdatascience.com/deploying-pytorch-models-with-nvidia-triton-inference-server-bb139066a387)

## 一个灵活且高性能的模型服务解决方案

[](https://ram-vegiraju.medium.com/?source=post_page-----bb139066a387--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----bb139066a387--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb139066a387--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb139066a387--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----bb139066a387--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb139066a387--------------------------------) ·7 分钟阅读·2023 年 9 月 14 日

--

![](img/eb2afba9c81fb0c85c927e7f318bc8e1.png)

图片来自 [Unsplash](https://unsplash.com/photos/8fVK5TFKaSE)

机器学习（ML）的价值在实际应用中真正体现，当我们到达 [模型托管与推理](https://cloud.google.com/bigquery/docs/inference-overview#:~:text=Machine%20learning%20inference%20is%20the,machine%20learning%20model%20into%20production.%22)时。如果没有一个高性能的模型服务解决方案，帮助你的模型实现弹性扩展，将很难将 ML 工作负载投入生产。

**什么是模型服务器/什么是模型服务？** 可以将模型服务器视为 ML 世界中与 web 服务器相当的东西。仅仅将大量硬件投入到模型中是不够的，你还需要一个通信层，帮助处理客户端的请求，同时有效分配硬件以应对你的应用程序所遇到的流量。模型服务器是一个可调节的功能：我们可以通过控制 gRPC 与 REST 等方面，从延迟角度挤压性能。流行的模型服务器示例如下：

+   [TensorFlow Serving](https://www.tensorflow.org/tfx/guide/serving)

+   [TorchServe](https://pytorch.org/serve/)

+   [多模型服务器 (MMS)](https://github.com/awslabs/multi-model-server)

+   [深度 Java 库 (DJL)](https://github.com/deepjavalibrary/djl-serving)

今天我们探讨的是 [Nvidia Triton Inference Server](https://github.com/triton-inference-server/server)，这是一个高度灵活且高性能的模型服务解决方案。每个模型服务器要求以其特定的方式呈现模型工件和推理脚本，以便它可以理解。在今天的文章中，我们以一个示例 PyTorch 模型为例，展示如何利用 Triton Inference Server 进行托管。

**注意**：本文假设你对机器学习有基本了解，并且不深入探讨模型构建的理论。还假设你对 Python 有一定的熟练度，并对 Docker 容器有基本了解。我们还将在[SageMaker 经典 Notebook 实例](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html)中进行开发，因此如有必要，请创建一个 AWS 账户（如果你愿意，也可以在其他地方运行此示例）。

**免责声明**：我是 AWS 的机器学习架构师，我的观点仅代表我自己。

## 为什么选择 Triton 推理服务器？

Triton 推理服务器是一个开源的模型服务解决方案，具有以下多种好处：

1.  **框架支持**：Triton 原生支持多种框架，如 PyTorch、TensorFlow、ONNX，以及自定义的 Python/C++环境。Triton 在其设置中将这些不同的框架识别为“后端”。在这个例子中，我们使用它提供的 PyTorch 后端来托管我们的 TorchScript 模型。

1.  **动态批处理**：推理性能调整是一个迭代实验。动态批处理是一种推理优化技术，通过这种技术，你可以将多个请求组合成一个。使用 Triton，你可以启用动态批处理并控制最大批量大小，以优化吞吐量和延迟。

1.  **模型集成/管道**：通过集成和模型管道，你可以将多个机器学习模型及任何预处理/后处理逻辑组合成一个统一的执行流，放在一个容器后面。

1.  **硬件支持**：Triton 支持基于 CPU/GPU 的工作负载。这使得它可以轻松配对像[SageMaker 实时推理](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html)这样的托管平台，在这些平台上，你可以托管数千个模型，利用[多模型端点与 GPU](https://aws.amazon.com/blogs/machine-learning/run-multiple-deep-learning-models-on-gpu-with-amazon-sagemaker-multi-model-endpoints/)和 Triton 作为模型服务栈。

Triton 推理服务器与其他模型服务器一样，具有各自的优缺点，具体取决于用例，因此根据你的模型和托管需求，选择合适的模型服务器选项至关重要。

## 本地模型设置

在这个例子中，我们将使用 SageMaker 经典 Notebook 实例中的 PyTorch 内核。实例将是 g4dn.4xlarge，因此我们可以在基于 GPU 的实例上运行我们的 Triton 容器。

开始时，我们将使用一个样本 PyTorch 线性回归模型，并在由 numpy 生成的一些虚拟数据上进行训练。

```py
 # Dummy data
np.random.seed(0)
X = 2 * np.random.rand(100, 1)
y = 1 + 2 * X + np.random.randn(100, 1)

# Model
class LinearRegression(nn.Module):
    def __init__(self):
        super(LinearRegression, self).__init__()
        self.linear = nn.Linear(1, 1)

    def forward(self, x):
        return self.linear(x)

# Training
for epoch in range(num_epochs):
    # Forward pass
    outputs = model(X_tensor)
    loss = criterion(outputs, y_tensor)

    # Backward pass and optimization
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
```

然后我们将此模型保存为[TorchScript](https://pytorch.org/docs/stable/jit.html)模型，以便在 Triton PyTorch 后端中使用，并运行一个样本推理，以便了解模型推理的样本输入将是什么样的。

```py
# save model as a torchscript model
torch.jit.save(torch.jit.script(model), 'model.pt')

# Load the saved model
loaded_model = torch.jit.load('model.pt')

# sample inference
test = torch.tensor([[2.5]])
pred = loaded_model(test)
print(pred)
```

现在我们有了 model.pt 并了解了如何用此模型进行推理，我们可以专注于设置 Triton 以托管这个特定的模型。

## Triton 推理服务器托管

在我们可以开始设置 Triton 推理服务器之前，我们需要了解它需要哪些工件以及它期望的格式。我们已经有了 model.pt，这是我们的模型元数据文件。Triton 还需要一个 config.pbtxt 文件，基本上定义了你的服务属性。在这种情况下，我们定义了几个必要的字段：

+   **model_name**：这是我们模型库中的模型名称，你还可以为其版本控制，以防有多个版本的模型。

+   **平台**：这是服务器的后台环境，在这种情况下，我们将后台定义为 pytorch_libtorch，以设置 TorchScript 模型的环境。

+   **输入/输出**：对于 Triton，我们必须定义我们的输入/输出数据形状和格式（如果你不知道，可以通过 numpy 找到这些信息）

可选地，你还可以定义更高级的参数，如启用动态批处理、最大批大小，以及你希望为性能测试调整的后台特定变量。我们的基本 config.pbtxt 如下：

```py
name: "linear_regression_model"
platform: "pytorch_libtorch"

input {
  name: "input"
  data_type: TYPE_FP32
  dims: [ 1, 1 ]
}

output {
  name: "output"
  data_type: TYPE_FP32
  dims: [ 1, 1 ]
}
```

Triton 还期望这个文件和 model.pt 以特定格式呈现，然后我们才能启动服务器。对于 PyTorch 后台，你的工件应该遵循以下模型库结构：

```py
- models/
  - linear_regression_model
    - 1
       - model.pt
       - model.py (optional, not included here)
    - config.pbtxt
  - optional incldue any other models for example if you have an ensemble
```

然后我们使用以下 bash 命令将工件移到这个现有结构中：

```py
mkdir linear_regression_model
mv config.pbtxt model.pt linear_regression_model
cd linear_regression_model
mkdir 1
mv model.pt 1/
cd ..
```

要启动 Triton 推理服务器，请确保你的环境中已安装[Docker](https://www.docker.com/)；这在 SageMaker Classic Notebook 实例中预装（在本文撰写时，SageMaker Studio 尚未预装）。

要启动容器，我们首先使用以下命令拉取最新的 Triton 镜像：

```py
docker pull nvcr.io/nvidia/tritonserver:23.08-py3
```

然后我们可以利用[Nvidia CLI](https://developer.nvidia.com/nvidia-system-management-interface)实用程序命令结合 Docker 来启动我们的 Triton 推理服务器。Triton 有三个默认端口用于推理，我们在 Docker 运行命令中指定：

+   **8000**：HTTP REST API 请求，我们在这个示例中使用此端口

+   **8001**：[gRPC 请求](https://aws.amazon.com/compare/the-difference-between-grpc-and-rest/#:~:text=In%20gRPC%2C%20one%20component%20(the,updates%20data%20on%20the%20server.)，这对于优化计算机视觉工作负载特别有用

+   **8002**：通过 Prometheus 监控 Triton 推理服务器的指标

确保将提供的路径替换为你的模型库目录路径。在这个实例中，它是‘/home/ec2-user/SageMaker’，你需要将其替换为你的模型库路径。

```py
docker run --gpus all --rm -p 8000:8000 -p 8001:8001 -p 8002:8002 
-v /home/ec2-user/SageMaker:/models nvcr.io/nvidia/tritonserver:23.08-py3 
tritonserver --model-repository=/models --exit-on-error=false --log-verbose=1
```

一旦我们运行这个命令，你应该会看到 Triton 推理服务器已经启动。

![](img/9cad6fcc0e41b0f7e77a85c5ad882307.png)

Triton 服务器已启动（截图由作者提供）

我们现在可以向模型服务器发出请求，我们可以通过两种不同的方式进行，这里将进行探索：

1.  [Python Request Library](https://pypi.org/project/requests/): 在这里你可以传入 Triton Server 地址的推理 URL 并指定输入参数。

1.  [Triton Client Library](https://github.com/triton-inference-server/client/tree/main): Triton 提供的客户端，你可以实例化并使用其内置的 API 调用。在这种情况下我们使用 Python，但你也可以使用 Java 和 C++。

对于 requests 库，我们传入适当的 URL 和模型名称及版本，如下所示：

```py
# sample data
input_data = np.array([[2.5]], dtype=np.float32)

# Specify the model name and version
model_name = "linear_regression_model" #specified in config.pbtxt
model_version = "1"

# Set the inference URL based on the Triton server's address
url = f"http://localhost:8000/v2/models/{model_name}/versions/{model_version}/infer"

# payload with input params
payload = {
    "inputs": [
        {
            "name": "input",  # what you named input in config.pbtxt
            "datatype": "FP32",  
            "shape": input_data.shape,
            "data": input_data.tolist(),
        }
    ]
}

# sample invoke
response = requests.post(url, data=json.dumps(payload))
```

对于 Triton 客户端库，我们使用在配置文件中指定的相同值，但利用 HTTP 客户端特定的 API 调用。

```py
import tritonclient.http as httpclient #pip install if needed

# setup triton inference client
client = httpclient.InferenceServerClient(url="localhost:8000")

# triton can infer the inputs from your config values
inputs = httpclient.InferInput("input", input_data.shape, datatype="FP32")
inputs.set_data_from_numpy(input_data) #we set a numpy array in this case

# output configuration
outputs = httpclient.InferRequestedOutput("output")

#sample inference
res = client.infer(model_name = "linear_regression_model", inputs=[inputs], outputs=[outputs],
                  )
inference_output = res.as_numpy('output') #serialize numpy output
```

## 额外资源与总结

[](https://github.com/RamVegiraju/triton-inference-server-examples/blob/master/pytorch-backend/triton-pytorch-ann.ipynb?source=post_page-----bb139066a387--------------------------------) [## triton-inference-server-examples/pytorch-backend/triton-pytorch-ann.ipynb at master ·…

### Triton Inference Server PyTorch 示例。贡献者 RamVegiraju/triton-inference-server-examples 开发中…

[github.com](https://github.com/RamVegiraju/triton-inference-server-examples/blob/master/pytorch-backend/triton-pytorch-ann.ipynb?source=post_page-----bb139066a387--------------------------------)

示例的完整代码可以在上面的链接中找到。Triton Inference Server 是一种动态模型服务选项，可用于高级 ML 模型托管。有关更多 Triton 特定的示例，请参阅以下 [Github 仓库](https://github.com/triton-inference-server/tutorials)。在接下来的文章中，我们将继续探讨如何利用不同的模型服务器来托管各种 ML 模型。

一如既往，谢谢你的阅读，欢迎随时留下反馈。

*如果你喜欢这篇文章，欢迎通过* [*LinkedIn*](https://www.linkedin.com/in/ram-vegiraju-81272b162/) *与我联系，并订阅我的 Medium* [*通讯*](https://ram-vegiraju.medium.com/subscribe)*。如果你是 Medium 新用户，可以通过我的* [*会员推荐链接*](https://ram-vegiraju.medium.com/membership)*进行注册。*
