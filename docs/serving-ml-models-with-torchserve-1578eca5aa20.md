# 使用TorchServe服务ML模型

> 原文：[https://towardsdatascience.com/serving-ml-models-with-torchserve-1578eca5aa20?source=collection_archive---------9-----------------------#2023-03-29](https://towardsdatascience.com/serving-ml-models-with-torchserve-1578eca5aa20?source=collection_archive---------9-----------------------#2023-03-29)

## 一个完整的端到端图像分类任务ML模型服务示例

[](https://medium.com/@summit.mnr?source=post_page-----1578eca5aa20--------------------------------)[![Andrey Golovin](../Images/3afbee89a80374b346e57c8f317c9b3a.png)](https://medium.com/@summit.mnr?source=post_page-----1578eca5aa20--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1578eca5aa20--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1578eca5aa20--------------------------------) [Andrey Golovin](https://medium.com/@summit.mnr?source=post_page-----1578eca5aa20--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc18c39659707&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fserving-ml-models-with-torchserve-1578eca5aa20&user=Andrey+Golovin&userId=c18c39659707&source=post_page-c18c39659707----1578eca5aa20---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1578eca5aa20--------------------------------) ·8 min read·2023年3月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1578eca5aa20&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fserving-ml-models-with-torchserve-1578eca5aa20&user=Andrey+Golovin&userId=c18c39659707&source=-----1578eca5aa20---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1578eca5aa20&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fserving-ml-models-with-torchserve-1578eca5aa20&source=-----1578eca5aa20---------------------bookmark_footer-----------)![](../Images/d444be3fa94d97d5bc6f951f3e33825c.png)

图片来源：作者

# 动机

本文将引导你通过使用TorchServe框架服务你的深度学习Torch模型的过程。

关于这个主题有很多文章。然而，通常这些文章要么专注于部署TorchServe本身，要么专注于编写自定义处理程序并获得最终结果。这是我写这篇文章的动机。本文涵盖了这两个部分，并提供了端到端的示例。

以图像分类挑战为例。最终你将能够部署 TorchServe 服务器，提供模型服务，发送任何随机的衣物图片，并最终获得预测的衣物类别标签。我相信这就是人们对作为 API 端点提供分类服务的 ML 模型的期望。

# 简介

比如，你的数据科学团队设计了一个很棒的深度学习模型。这无疑是一个伟大的成就。然而，为了让它发挥价值，模型需要以某种方式向外界开放（如果不是 Kaggle 竞赛的话）。这就是所谓的模型服务。在这篇文章中，我不会涉及批处理操作的服务模式以及基于流处理框架的流处理模式。我将专注于将模型作为 API 提供服务的一个选项（无论这个 API 是由流处理框架还是任何自定义服务调用）。更准确地说，这个选项是 TorchServe 框架。

因此，当你决定将模型作为 API 提供服务时，你至少有以下几种选择：

+   网络框架如 Flask、Django、FastAPI 等等

+   云服务如 AWS Sagemaker 端点

+   专用服务框架如 Tensorflow Serving、Nvidia Triton 和 TorchServe

每种方法都有其优缺点，选择可能并不总是简单明了的。让我们实际探索一下 TorchServe 选项。

# 结构

第一部分将简要描述模型是如何训练的。这对 TorchServe 来说并不重要，但我认为这有助于跟踪端到端的过程。接下来，将解释自定义处理器。

第二部分将重点讨论 TorchServe 框架的部署。

本文的源代码位于这里：[git repo](https://github.com/quasi-researcher/torchserve_example)

# 准备一个深度学习模型

对于这个示例，我选择了基于 FashionMNIST 数据集的图像分类任务。如果你不熟悉这个数据集，它包含 70k 张 28x28 像素的灰度图像，展示了不同的衣物。共有 10 个衣物类别。因此，一个深度学习分类模型将返回 10 个 logit 值。为了简化，模型基于 TinyVGG 架构（如果你想用 CNN 解释器可视化它）：仅有少量卷积层和最大池化层，带有 RELU 激活。仓库中的笔记本 [*model_creation_notebook*](https://github.com/quasi-researcher/torchserve_example/blob/master/model_creation_notebook.ipynb) 展示了训练和保存模型的全部过程。

简而言之，笔记本只是下载数据，定义模型架构，训练模型并用 torch 保存状态字典。有两个与 TorchServe 相关的文档：一个包含模型架构定义的类和保存的模型（.pth 文件）。

# 为 TorchServe 准备文档以提供模型服务

需要准备两个模块：模型文件和自定义处理器。

**模型文件**

根据文档，“*模型文件应包含模型架构。对于急切模式的模型，这个文件是必须的。

该文件应包含一个继承自 torch.nn.Module 的单个类。

所以，让我们从模型训练笔记本中复制类定义，并将其保存为 *model.py*（你可以选择任何名称）：

**处理程序**

TorchServe 提供了一些默认处理程序（例如 image_classifier），但我怀疑它是否可以直接用于实际案例。因此，你很可能需要为你的任务创建一个自定义处理程序。处理程序实际上定义了如何从 http 请求中预处理数据，如何将其输入模型，如何后处理模型的输出以及在响应中返回最终结果。

有两个选项——模块级入口点和类级入口点。请查看官方文档[这里](https://github.com/pytorch/serve/blob/master/docs/custom_service.md)。

我将实现类级选项。这基本上意味着我需要创建一个自定义 Python 类并定义两个强制函数：*initialize* 和 *handle*。

首先，为了简化操作，让我们继承 *BaseHandler* 类。*initialize* 函数定义了如何加载模型。由于我们这里没有任何具体要求，因此可以直接使用超类中的定义。

*handle* 函数基本上定义了如何处理数据。在最简单的情况下，流程是：*预处理 >> 推理 >> 后处理*。在实际应用中，你可能需要定义自定义的预处理和后处理函数。对于这个示例的推理函数，我将使用超类中的默认定义：

**预处理函数**

比如，你构建了一个图像分类应用。该应用将图像作为负载发送给 TorchServe。图像可能不会始终符合用于模型训练的图像格式。此外，你可能会在样本批次上训练模型，张量维度必须进行调整。因此，让我们创建一个简单的预处理函数：将图像调整为所需的形状，转换为灰度图像，转换为 Torch 张量并将其作为单样本批次。

**后处理函数**

多类别分类模型将返回一个 logit 或 softmax 概率列表。但在实际场景中，你更可能需要一个预测类别或带有概率值的预测类别，或者可能是前 N 个预测标签。当然，你可以在主应用程序/其他服务中完成这些操作，但这意味着将应用程序逻辑与 ML 训练过程绑定在一起。因此，让我们直接在响应中返回预测类别。

（为了简单起见，这里的标签列表是硬编码的。在 github 版本中，处理程序从配置中读取标签）

# 启动 Torch 服务器

好了，模型文件和处理程序都准备好了。现在让我们部署 TorchServe 服务器。上面的代码假设你已经安装了 pytorch。另一个前提条件是安装了 JDK 11（注意，仅 JRE 不够，你需要 JDK）。

对于 TorchServe，你需要安装两个包：*torchserve* 和 *torch-model-archiver*。

成功安装后，第一步是准备一个 .mar 文件——包含模型工件的档案。torch-model-archiver 的 CLI 接口旨在实现这一点。终端中输入：

```py
torch-model-archiver --model-name fashion_mnist --version 1.0 --model-file path/model.py --serialized-file path/fashion_mnist_model.pth --handler path/handler.py
```

参数如下：

-*模型名称*：你想给模型起的名称

-*版本*：用于版本控制的语义版本

-*模型文件*：包含模型架构类定义的文件

-*序列化文件*：来自 torch.save() 的 .pth 文件

-*处理器*：包含处理器的 Python 模块

结果是一个名为模型名称的 .mar 文件（在这个例子中是 *fashion_mnist.mar*）将在执行 CLI 命令的目录中生成。因此，最好在调用命令之前 cd 到你的项目目录。

下一步是启动服务器。终端中输入：

```py
torchserve --start --model-store path --models fmnist=/path/fashion_mnist.mar 
```

参数：

-*模型存储*：存放 mar 文件的目录

-*模型*：模型名称和对应 mar 文件的路径。

注意，归档器中的模型名称定义了你的 .mar 文件将如何命名。torchserve 中的模型名称定义了调用模型的 API 端点名称。所以，这些名称可以相同也可以不同，取决于你。

执行这两个命令后，服务器应该启动并运行。默认情况下，TorchServe 使用三个端口：8080、8081 和 8082 分别用于推理、管理和指标。打开浏览器/curl/Postman 发送请求到

*http://localhost:8080/ping*

如果 TorchServe 正常工作，你应该看到 *{‘status’: ‘Healthy’}*

![](../Images/0df4ccbecb4662d7a41d15f337d6c058.png)

图片来源于作者

**一些可能问题的提示**：

1\. 如果在 *torchserve -start* 命令后你在日志中看到提到“*..no module named captum*”的错误，那么请手动安装它。我在 *torchserve 0.7.1* 中遇到了这个错误

2\. 可能会有某些端口已经被另一个进程占用。那你可能会看到 ‘*Partially healthy*’ 状态和日志中的一些错误。

要检查哪个进程使用了 Mac 上的端口（例如 8081），输入：

```py
sudo lsof -i :8081
```

一种选择是终止进程以释放端口。但如果该进程很重要，这可能并不是一个好主意。

另外，可以在简单的配置文件中为 TorchServe 指定任何新的端口。假设你有一个已经在 8081 端口上运行的应用程序。通过创建包含一行的 torch_config 文件来更改默认的 TorchServe 管理 API 端口：

```py
management_address=https://0.0.0.0:8443
```

*(你可以选择任何空闲端口)*

接下来我们需要让 TorchServe 知道配置。首先，通过以下命令停止不健康的服务器

```py
torchserve --stop
```

然后重新启动它：

```py
torchserve --start --model-store path --models fmnist=/path/fashion_mnist.mar --ts-config path/torch_config
```

# 请求模型的预测

在这一步假设服务器已正确启动。让我们将随机的衣物图像传递给推理 API 并获取预测标签。

推理的端点是

*http://localhost:8080/predictions/model_name*

在这个例子中是 *http://localhost:8080/predictions/fmnist*

让我们使用 curl 传递一个图像：

```py
curl -X POST http://localhost:8080/predictions/fmnist -T /path_to_image/image_file
```

例如，使用 repo 中的示例图像：

```py
curl -X POST http://localhost:8080/predictions/fmnist -T tshirt4.jpg
```

*(X 标志用于指定方法 /POST/，-T 标志用于传输文件)*

在响应中，我们应该看到预测的标签：

![](../Images/7ba3d410efe561befca4dbac6a11f21a.png)

图片由作者提供

# 结论

好吧，通过跟随这篇博客文章，我们能够创建一个REST API端点，我们可以向其发送图像并获取图像的预测标签。通过在服务器上重复相同的过程，而不是本地机器，一个人可以利用它来为面向用户的应用程序、其他服务，或者例如流式机器学习应用程序创建一个端点（参见这篇有趣的论文了解为什么你可能不应该这样做：[*https://sites.bu.edu/casp/files/2022/05/Horchidan22Evaluating.pdf*](https://sites.bu.edu/casp/files/2022/05/Horchidan22Evaluating.pdf)）

敬请关注，在下一部分中我将扩展示例：让我们为业务逻辑创建一个Flask应用程序的模拟，并通过TorchServe调用一个机器学习模型（并使用Kubernetes部署所有内容）。

一个简单的用例：面向用户的应用程序，具有大量业务逻辑和许多不同的功能。比如，一个功能是上传图像，将所需的样式应用于图像，使用样式迁移机器学习模型。机器学习模型可以通过TorchServe提供，因此机器学习部分将完全与主应用程序中的业务逻辑和其他功能解耦。
