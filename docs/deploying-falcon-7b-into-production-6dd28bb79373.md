# 部署 Falcon-7B 进入生产环境

> 原文：[https://towardsdatascience.com/deploying-falcon-7b-into-production-6dd28bb79373?source=collection_archive---------1-----------------------#2023-07-07](https://towardsdatascience.com/deploying-falcon-7b-into-production-6dd28bb79373?source=collection_archive---------1-----------------------#2023-07-07)

## 一步步教程

## 在云端以微服务形式运行 Falcon-7B

[](https://medium.com/@het.trivedi05?source=post_page-----6dd28bb79373--------------------------------)[![Het Trivedi](../Images/f6f11a66f60cacc6b553c7d1682b2fc6.png)](https://medium.com/@het.trivedi05?source=post_page-----6dd28bb79373--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6dd28bb79373--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6dd28bb79373--------------------------------) [Het Trivedi](https://medium.com/@het.trivedi05?source=post_page-----6dd28bb79373--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce8ebd0c262c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-falcon-7b-into-production-6dd28bb79373&user=Het+Trivedi&userId=ce8ebd0c262c&source=post_page-ce8ebd0c262c----6dd28bb79373---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6dd28bb79373--------------------------------) ·16 分钟阅读·2023年7月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6dd28bb79373&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-falcon-7b-into-production-6dd28bb79373&user=Het+Trivedi&userId=ce8ebd0c262c&source=-----6dd28bb79373---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6dd28bb79373&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-falcon-7b-into-production-6dd28bb79373&source=-----6dd28bb79373---------------------bookmark_footer-----------)![](../Images/10a0800c6f473b08d7985ccc8c5969f9.png)

图片由作者提供-使用 Midjourney 创建

## 背景

到目前为止，我们已经了解了 ChatGPT 的能力及其提供的功能。然而，对于企业使用，像 ChatGPT 这样的闭源模型可能存在风险，因为企业无法控制其数据。OpenAI 声称用户数据不会被存储或用于训练模型，但无法保证数据不会以某种方式泄露。

为了应对与闭源模型相关的一些问题，研究人员正急于构建与ChatGPT等模型相媲美的开源LLM。使用开源模型，企业可以在安全的云环境中自行托管这些模型，从而降低数据泄露的风险。此外，你还可以完全透明地了解模型的内部工作，这有助于建立对AI的更多信任。

随着开源LLM的最新进展，试用新的模型并查看它们如何与像ChatGPT这样的闭源模型竞争变得很有诱惑力。

然而，今天运行开源模型存在显著的障碍。调用ChatGPT API要比弄清楚如何运行开源LLM容易得多。

在这篇文章中，我的目标是通过展示如何在生产环境中运行像Falcon-7B这样的开源模型，来打破这些障碍。我们将能够通过类似于ChatGPT的API端点访问这些模型。

## 挑战

运行开源模型的一个重大挑战是缺乏计算资源。即使是像Falcon 7B这样的“小型”模型，也需要GPU才能运行。

为了解决这个问题，我们可以利用云中的GPU。但这带来了另一个挑战。我们如何容器化我们的LLM？我们如何启用GPU支持？启用GPU支持可能很棘手，因为这需要了解CUDA。处理CUDA可能会令人头痛，因为你需要弄清楚如何安装正确的CUDA依赖以及哪些版本是兼容的。

因此，为了避免CUDA死循环，许多公司已经创建了解决方案来轻松容器化模型，同时支持GPU。对于这篇博客文章，我们将使用一个名为[Truss](https://github.com/basetenlabs/truss)的开源工具来帮助我们轻松容器化我们的LLM，而不费多少劲。

Truss允许开发者轻松容器化使用任何框架构建的模型。

## 为什么使用Truss？

![](../Images/994154aab9fb051480824ccacfa116e3.png)

Truss — [https://truss.baseten.co/e2e](https://truss.baseten.co/e2e)

Truss拥有许多开箱即用的有用功能，例如：

+   将你的Python模型转换为具有生产就绪API端点的微服务

+   通过Docker冻结依赖

+   支持GPU上的推理

+   模型的简单预处理和后处理

+   简单且安全的秘密管理

我之前使用过Truss来部署机器学习模型，过程非常顺利和简单。Truss自动创建你的dockerfile并管理Python依赖。我们要做的就是提供我们的模型代码。

![](../Images/53e388d9c34388bef656ff02ad0db9d9.png)

我们希望使用像Truss这样的工具的主要原因是，它使得部署支持GPU的模型变得更加容易。

> 注意：我没有收到 Baseten 的赞助来推广他们的内容，也没有与他们有任何关联。我没有受到 Baseten 或 Truss 任何形式的影响来撰写这篇文章。我只是发现他们的开源项目很酷且有用。

## 计划

在这篇博客中，我将涵盖以下内容：

1.  使用 Truss 在本地设置 Falcon 7B

1.  如果你有 GPU（我有一块 RTX 3080），可以在本地运行模型

1.  将模型容器化并使用 docker 运行

1.  在 Google Cloud 上创建一个启用 GPU 的 Kubernetes 集群来运行我们的模型

如果你在第二步没有 GPU，也不用担心，你仍然可以在云端运行模型。

如果你想跟随，请查看包含代码的 Github 仓库：

[## GitHub - htrivedi99/falcon-7b-truss](https://github.com/htrivedi99/falcon-7b-truss?source=post_page-----6dd28bb79373--------------------------------)

### 通过在 GitHub 上创建账户，贡献于 htrivedi99/falcon-7b-truss 的开发。

[github.com](https://github.com/htrivedi99/falcon-7b-truss?source=post_page-----6dd28bb79373--------------------------------)

让我们开始吧！

## 第一步：使用 Truss 本地设置 Falcon 7B

首先，我们需要创建一个 Python 版本 ≥ 3.8 的项目

我们将从 hugging face 下载模型，并使用 Truss 打包它。以下是我们需要安装的依赖项：

```py
pip install truss
```

在你的 Python 项目中创建一个名为 `main.py` 的脚本。这是我们将用于与 truss 进行交互的临时脚本。

接下来，我们将在终端中运行以下命令来设置 Truss 包：

```py
truss init falcon_7b_truss
```

如果你被提示创建一个新的 truss，按下‘y’。完成后，你应该会看到一个名为 `falcon_7b_truss` 的新目录。在该目录内，将有一些自动生成的文件和文件夹。我们需要填写几样东西：`model.py`，它嵌套在包 `model` 下，以及 `config.yaml`。

```py
├── falcon_7b_truss
│   ├── config.yaml
│   ├── data
│   ├── examples.yaml
│   ├── model
│   │   ├── __init__.py
│   │   └── model.py
│   └── packages
└── main.py
```

如我之前提到的，Truss 只需要我们模型的代码，它会自动处理所有其他事情。我们将在 `model.py` 内编写代码，但必须按照特定格式编写。

Truss 期望每个模型支持至少三个函数：`__init__` 、`load` 和 `predict`。

+   `__init__` 主要用于创建类变量

+   `load` 是我们将从 hugging face 下载模型的地方

+   `predict` 是我们调用模型的地方

这是 `model.py` 的完整代码：

```py
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM, pipeline
from typing import Dict

MODEL_NAME = "tiiuae/falcon-7b-instruct"
DEFAULT_MAX_LENGTH = 128

class Model:
    def __init__(self, data_dir: str, config: Dict, **kwargs) -> None:
        self._data_dir = data_dir
        self._config = config
        self.device = "cuda" if torch.cuda.is_available() else "cpu"
        print("THE DEVICE INFERENCE IS RUNNING ON IS: ", self.device)
        self.tokenizer = None
        self.pipeline = None

    def load(self):
        self.tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME)
        model_8bit = AutoModelForCausalLM.from_pretrained(
            MODEL_NAME,
            device_map="auto",
            load_in_8bit=True,
            trust_remote_code=True)

        self.pipeline = pipeline(
            "text-generation",
            model=model_8bit,
            tokenizer=self.tokenizer,
            torch_dtype=torch.bfloat16,
            trust_remote_code=True,
            device_map="auto",
        )

    def predict(self, request: Dict) -> Dict:
        with torch.no_grad():
            try:
                prompt = request.pop("prompt")
                data = self.pipeline(
                    prompt,
                    eos_token_id=self.tokenizer.eos_token_id,
                    max_length=DEFAULT_MAX_LENGTH,
                    **request
                )[0]
                return {"data": data}

            except Exception as exc:
                return {"status": "error", "data": None, "message": str(exc)}
```

这里发生了什么：

+   `MODEL_NAME` 是我们将使用的模型，在我们的情况下是 `falcon-7b-instruct` 模型

+   在 `load` 内，我们以 8bit 下载 hugging face 上的模型。我们选择 8bit 的原因是，当量化时，模型在 GPU 上占用的内存显著减少。

+   如果你想在 VRAM 少于 13GB 的 GPU 上本地运行模型，则加载 8-bit 模型是必要的。

+   `predict` 函数接受一个 JSON 请求作为参数，并使用 `self.pipeline` 调用模型。`torch.no_grad` 告诉 Pytorch 我们处于推理模式，而非训练模式。

太棒了！这就是我们设置模型所需的一切。

## 第2步：在本地运行模型（可选）

如果你有一块超过8GB VRAM的Nvidia GPU，你将能够在本地运行这个模型。

如果没有，请随意继续下一步。

我们需要下载一些更多的依赖项以在本地运行模型。在下载依赖项之前，你需要确保已安装CUDA和正确的CUDA驱动程序。

由于我们尝试在本地运行模型，Truss 将无法帮助我们管理 CUDA 问题。

```py
pip install transformers
pip install torch
pip install peft
pip install bitsandbytes
pip install einops
pip install scipy 
```

接下来，在`main.py`脚本中，我们需要加载我们的truss。

这是`main.py`的代码：

```py
import truss
from pathlib import Path
import requests

tr = truss.load("./falcon_7b_truss")
output = tr.predict({"prompt": "Hi there how are you?"})
print(output)
```

这里发生了什么：

+   如果你记得，`falcon_7b_truss`目录是由truss创建的。我们可以使用`truss.load`加载整个包，包括模型和依赖项。

+   一旦加载了包，我们可以简单地调用`predict`方法来获取模型的输出。

运行`main.py`以获取模型的输出。

这些模型文件的大小约为15 GB，因此下载模型可能需要5到10分钟。运行脚本后，你应该会看到类似的输出：

```py
{'data': {'generated_text': "Hi there how are you?\nI'm doing well. I'm in the middle of a move, so I'm a bit tired. I'm also a bit overwhelmed. I'm not sure how to get started. I'm not sure what I'm doing. I'm not sure if I'm doing it right. I'm not sure if I'm doing it wrong. I'm not sure if I'm doing it at all.\nI'm not sure if I'm doing it right. I'm not sure if I'm doing it wrong. I"}}
```

## 第3步：使用docker容器化模型

通常，当人们容器化模型时，他们会将模型二进制文件和Python依赖项打包起来，使用Flask或Fast API服务器进行封装。

很多内容是样板代码，我们不想自己去做。Truss会处理这些。我们已经提供了模型，Truss会创建服务器，所以剩下的就是提供Python依赖项。

`config.yaml`保存了模型的配置。在这里，我们可以添加模型的依赖项。配置文件已经包含了大部分我们需要的内容，但我们还需要添加一些东西。

这是你需要添加到`config.yaml`中的内容：

```py
apply_library_patches: true
bundled_packages_dir: packages
data_dir: data
description: null
environment_variables: {}
examples_filename: examples.yaml
external_package_dirs: []
input_type: Any
live_reload: false
model_class_filename: model.py
model_class_name: Model
model_framework: custom
model_metadata: {}
model_module_dir: model
model_name: Falcon-7B
model_type: custom
python_version: py39
requirements:
- torch
- peft
- sentencepiece
- accelerate
- bitsandbytes
- einops
- scipy
- git+https://github.com/huggingface/transformers.git
resources:
  use_gpu: true
  cpu: "3"
  memory: 14Gi
secrets: {}
spec_version: '2.0'
system_packages: []
```

我们添加的主要内容是`requirements`。列出的所有依赖项都是下载和运行模型所必需的。

我们添加的另一个重要内容是`resources`。`use_gpu: true`是必需的，因为这告诉Truss为我们创建一个启用GPU支持的Dockerfile。

这就是配置的全部内容。

接下来，我们将容器化我们的模型。如果你不知道如何使用Docker容器化模型，别担心，Truss已经为你准备好了。

在`main.py`文件中，我们将告诉Truss将所有内容打包在一起。你需要的代码如下：

```py
import truss
from pathlib import Path
import requests

tr = truss.load("./falcon_7b_truss")
command = tr.docker_build_setup(build_dir=Path("./falcon_7b_truss"))
print(command)
```

发生了什么：

+   首先，我们加载我们的`falcon_7b_truss`

+   接下来，`docker_build_setup`函数处理所有复杂的内容，如创建Dockerfile和设置Fast API服务器。

+   如果你查看`falcon_7b_truss`目录，你会发现生成了更多文件。我们不需要担心这些文件如何工作，因为一切都会在幕后管理。

+   在运行结束时，我们得到一个构建 Docker 镜像的命令：`docker build falcon_7b_truss -t falcon-7b-model:latest`

![](../Images/beb90d0e1302c0c426824fa199948d4d.png)

如果你想构建 Docker 镜像，请运行构建命令。镜像大小约为 9 GB，所以可能需要一些时间来构建。如果你不想构建它但想跟随，可以使用我的镜像：`htrivedi05/truss-falcon-7b:latest`。

如果你自己构建镜像，你需要标记并将其推送到 Docker Hub，以便我们在云中的容器可以拉取这个镜像。以下是镜像构建完成后需要运行的命令：

```py
docker tag falcon-7b-model <docker_user_id>/falcon-7b-model
```

```py
docker push <docker_user_id>/falcon-7b-model
```

太棒了！我们已经准备好在云中运行我们的模型了！

**（以下是可选步骤，用于在本地使用 GPU 运行镜像）**

如果你有一张 Nvidia GPU 并且希望在本地**使用** GPU 支持运行你的容器化模型，你需要确保 Docker 已配置为使用你的 GPU。

打开终端并运行以下命令：

```py
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```

```py
apt-get update
apt-get install -y nvidia-docker2
```

```py
sudo systemctl restart docker
```

现在你的 Docker 已经配置为访问你的 GPU，下面是如何运行你的容器：

```py
docker run --gpus all -d -p 8080:8080 falcon-7b-model
```

再次提醒，下载模型需要一些时间。为了确保一切正常，你可以检查容器日志，你应该会看到“THE DEVICE INFERENCE IS RUNNING ON IS: cuda”。

你可以通过 API 端点调用模型，如下所示：

```py
import requests

data = {"prompt": "Hi there, how's it going?"}
res = requests.post("http://127.0.0.1:8080/v1/models/model:predict", json=data)
print(res.json())
```

## 第4步：将模型部署到生产环境

我在这里使用“生产”这个词比较宽泛。我们将把模型运行在 Kubernetes 中，那里可以轻松扩展并处理可变量的流量。

话虽如此，Kubernetes 有大量的配置，例如网络策略、存储、配置映射、负载均衡、秘密管理等。

即使 Kubernetes 是为了“扩展”和运行“生产”工作负载而构建的，很多生产级配置你需要的并不是开箱即用的。覆盖这些高级 Kubernetes 主题超出了本文的范围，也会偏离我们在这里想要实现的目标。因此，在这篇博客文章中，我们将创建一个最小化的集群，而没有所有的附加功能。

不再多说，让我们创建我们的集群吧！

## 先决条件：

1.  拥有一个带有项目的 Google Cloud 帐户

1.  在你的机器上安装**gcloud** CLI

1.  确保你有足够的配额来运行一个启用 GPU 的机器。你可以在**IAM & Admin**下检查你的配额。

![](../Images/0b5f428850fadcfa0cc0346b2f5ab4a0.png)

## 创建我们的 GKE 集群

我们将使用 Google 的 Kubernetes 引擎来创建和管理我们的集群。好，现在是一些**重要的**信息：

Google 的 Kubernetes 引擎**不是**免费的。Google 不允许我们免费使用强大的 GPU。话虽如此，我们正在创建一个单节点集群，配备较不强大的 GPU。这个实验的费用不应超过 1-2 美元。

这是我们将运行的 Kubernetes 集群的配置：

+   1 节点，标准 Kubernetes 集群（非自动驾驶）

+   1 张 Nvidia T4 GPU

+   n1-standard-4 机器（4 vCPU，15 GB 内存）

+   所有这些都将在一个临时实例上运行

> 注意：如果你在其他地区并且无法访问完全相同的资源，请随意进行修改。

## **创建集群的步骤：**

1.  前往[Google Cloud Console](https://console.cloud.google.com/)并搜索名为*Kubernetes Engine*的服务

![](../Images/09cff563a680d73a6527f4901dd44a00.png)

2\. 点击*创建*按钮

+   确保你创建的是标准集群，而不是自动驾驶集群。顶部应该显示*创建一个kubernetes集群*。

3\. 集群基本信息

+   在集群基本信息选项卡中，我们不需要做太多更改。只需给你的集群命名。你不需要更改区域或控制平面。

![](../Images/a55be1fba664346c690ba7d0ec344ece.png)

4\. 点击**default-pool**选项卡，将节点数更改为1

![](../Images/ca67dee00ace8d272e0b9815bec29c55.png)

5\. 在default-pool下，点击左侧边栏中的**节点**选项卡

+   将**机器配置**从**通用型**更改为**GPU**

+   选择**Nvidia T4**作为**GPU类型**，并将数量设置为**1**

+   启用GPU共享（尽管我们不会使用这个功能）

+   将**每GPU最大共享客户端**设置为8

+   对于**机器类型**，选择**n1-standard-4（4 vCPU，15 GB 内存）**

+   将**启动磁盘大小**更改为50

+   向下滚动到底部，勾选**启用临时虚拟机上的节点**

![](../Images/e26291a6c9e431d53f272eed05ba76ae.png)![](../Images/a23583fdadf90654922793c10114f036.png)![](../Images/210268f2273132cf85b94e48e7d4bfb4.png)

这是我为这个集群获得的估算价格的屏幕截图：

![](../Images/9d1d9c07e0380efa2b3be818df060653.png)

配置完集群后，继续创建它。

Google设置一切需要几分钟时间。集群启动并运行后，我们需要连接到它。打开终端并运行以下命令：

```py
gcloud config set compute/zone us-central1-c
```

```py
gcloud container clusters get-credentials gpu-cluster-1
```

如果你使用了不同的区域或集群名称，请相应更新。要检查我们是否连接成功，运行以下命令：

```py
kubectl get nodes
```

你应该会在终端中看到1个节点。尽管我们的集群有GPU，但缺少一些Nvidia驱动程序，我们需要安装它们。幸运的是，安装过程很简单。运行以下命令来安装驱动程序：

```py
kubectl apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/container-engine-accelerators/master/nvidia-driver-installer/cos/daemonset-preloaded.yaml
```

太棒了！我们终于准备好部署模型了。

## 部署模型

要将模型部署到集群中，我们需要创建一个**kubernetes部署**。一个kubernetes部署允许我们管理容器化模型的实例。我不会深入探讨kubernetes或如何编写yaml文件，因为这超出了范围。

你需要创建一个名为`truss-falcon-deployment.yaml`的文件。打开该文件并粘贴以下内容：

```py
apiVersion: apps/v1
kind: Deployment
metadata:
  name: truss-falcon-7b
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      component: truss-falcon-7b-layer
  template:
    metadata:
      labels:
        component: truss-falcon-7b-layer
    spec:
      containers:
      - name: truss-falcon-7b-container
        image: <your_docker_id>/falcon-7b-model:latest
        ports:
          - containerPort: 8080
        resources:
          limits:
            nvidia.com/gpu: 1
---
apiVersion: v1
kind: Service
metadata:
  name: truss-falcon-7b-service
  namespace: default
spec:
  type: ClusterIP
  selector:
    component: truss-falcon-7b-layer
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
```

发生了什么：

+   我们告诉kubernetes我们想用`falcon-7b-model`镜像创建pods。确保将`<your_docker_id>`替换为你的实际id。如果你没有创建自己的docker镜像而想使用我的镜像，请将其替换为：`htrivedi05/truss-falcon-7b:latest`

+   我们通过设置资源限制`nvidia.com/gpu: 1`来启用容器的GPU访问。这告诉kubernetes只请求一个GPU给我们的容器

+   要与我们的模型交互，我们需要创建一个将在端口8080上运行的kubernetes服务。

在终端中运行以下命令以创建部署：

```py
kubectl create -f truss-falcon-deployment.yaml
```

如果你运行命令：

```py
kubectl get deployments
```

你应该看到如下内容：

```py
NAME              READY   UP-TO-DATE   AVAILABLE   AGE
truss-falcon-7b   0/1     1            0           8s
```

部署变为准备状态需要几分钟时间。请记住，每次容器重启时，模型都需要从hugging face下载。你可以通过运行以下命令来检查容器的进度：

```py
kubectl get pods
```

```py
kubectl logs truss-falcon-7b-8fbb476f4-bggts
```

相应地更改pod名称。

在日志中，你需要查看以下几个方面：

+   查找打印语句**THE DEVICE INFERENCE IS RUNNING ON IS: cuda**。这确认了我们的容器已正确连接到GPU。

+   接下来，你应该看到一些关于模型文件下载的打印语句。

```py
Downloading (…)model.bin.index.json: 100%|██████████| 16.9k/16.9k [00:00<00:00, 1.92MB/s]
Downloading (…)l-00001-of-00002.bin: 100%|██████████| 9.95G/9.95G [02:37<00:00, 63.1MB/s]
Downloading (…)l-00002-of-00002.bin: 100%|██████████| 4.48G/4.48G [01:04<00:00, 69.2MB/s]
Downloading shards: 100%|██████████| 2/2 [03:42<00:00, 111.31s/it][01:04<00:00, 71.3MB/s]
```

+   一旦模型下载完成且Truss创建了微服务，你应该在日志末尾看到以下输出：

```py
{"asctime": "2023-06-29 21:40:40,646", "levelname": "INFO", "message": "Completed model.load() execution in 330588 ms"}
```

从此消息中，我们可以确认模型已加载并准备好进行推断。

## 模型推断

我们不能直接调用模型，而是必须调用模型的服务。

运行以下命令以获取服务名称：

```py
kubectl get svc
```

输出：

```py
NAME                      TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
kubernetes                ClusterIP   10.80.0.1    <none>        443/TCP    46m
truss-falcon-7b-service   ClusterIP   10.80.1.96   <none>        8080/TCP   6m19s
```

我们要调用的是`truss-falcon-7b-service`。为了使服务可访问，我们需要使用以下命令进行端口转发：

```py
kubectl port-forward svc/truss-falcon-7b-service 8080
```

输出：

```py
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

太棒了，我们的模型现在作为REST API端点可用，地址是`127.0.0.1:8080`。打开任何Python脚本，比如`main.py`，并运行以下代码：

```py
import requests

data = {"prompt": "Whats the most interesting thing about a falcon?"}
res = requests.post("http://127.0.0.1:8080/v1/models/model:predict", json=data)
print(res.json())
```

输出：

```py
 {'data': {'generated_text': 'Whats the most interesting thing about a falcon?\nFalcons are known for their incredible speed and agility in the air, as well as their impressive hunting skills. They are also known for their distinctive feathering, which can vary greatly depending on the species.'}}
```

哇！我们成功地将我们的Falcon 7B模型容器化，并将其作为微服务在生产环境中部署！

随意尝试不同的提示，看看模型返回什么。

## 关闭集群

当你玩够了Falcon 7B后，你可以通过运行以下命令来删除你的部署：

```py
kubectl delete -f truss-falcon-deployment.yaml
```

接下来，前往Google Cloud中的kubernetes引擎并删除kubernetes集群。

> 注意：除非另有说明，否则所有图像均为作者提供。

## 结论

运行和管理像ChatGPT这样的生产级模型并不容易。然而，随着时间的推移，工具将变得更好，开发者将能更轻松地将自己的模型部署到云端。

在这篇博客文章中，我们讨论了在基本层面上将LLM部署到生产环境所需的所有事项。我们使用Truss打包模型，通过Docker进行容器化，并使用kubernetes将其部署到云端。我知道这要处理的内容很多，而且这并不是世界上最容易的事情，但我们还是完成了。

希望你从这篇博客文章中学到了一些有趣的东西。感谢阅读！

平安。
