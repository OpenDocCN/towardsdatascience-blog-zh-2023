# TorchServe & Flask用于图像风格迁移

> 原文：[https://towardsdatascience.com/torchserve-flask-for-image-style-transfer-113f73bd1d70?source=collection_archive---------7-----------------------#2023-04-20](https://towardsdatascience.com/torchserve-flask-for-image-style-transfer-113f73bd1d70?source=collection_archive---------7-----------------------#2023-04-20)

## 一个由TorchServe模型服务器支持的Web应用示例

[](https://medium.com/@summit.mnr?source=post_page-----113f73bd1d70--------------------------------)[![Andrey Golovin](../Images/3afbee89a80374b346e57c8f317c9b3a.png)](https://medium.com/@summit.mnr?source=post_page-----113f73bd1d70--------------------------------)[](https://towardsdatascience.com/?source=post_page-----113f73bd1d70--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----113f73bd1d70--------------------------------) [Andrey Golovin](https://medium.com/@summit.mnr?source=post_page-----113f73bd1d70--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc18c39659707&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftorchserve-flask-for-image-style-transfer-113f73bd1d70&user=Andrey+Golovin&userId=c18c39659707&source=post_page-c18c39659707----113f73bd1d70---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----113f73bd1d70--------------------------------) · 6分钟阅读 · 2023年4月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F113f73bd1d70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftorchserve-flask-for-image-style-transfer-113f73bd1d70&user=Andrey+Golovin&userId=c18c39659707&source=-----113f73bd1d70---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F113f73bd1d70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftorchserve-flask-for-image-style-transfer-113f73bd1d70&source=-----113f73bd1d70---------------------bookmark_footer-----------)![](../Images/31d5f10b49cf379849d5f0002f5cd65d.png)

作者提供的图像*。将ML模型作为解耦的模型服务器暴露是一种更具可扩展性、可扩展性和可维护性的模式。*

在[上一篇文章](https://medium.com/p/1578eca5aa20)中，我展示了如何使用TorchServe框架提供图像分类模型的示例。现在，让我们扩展这个示例，使其更贴近现实世界的场景。

假设我想开发一个 web 应用，让用户将滤镜应用到他们的图像上。如你所知，有很多这样的应用程序。一个功能可以是神经风格迁移——用户可以上传一张内容图像和一张风格图像（或在应用中选择一个滤镜），然后获得一张以所需风格呈现的内容的新图像。让我们从头到尾构建这个示例。

本文的重点当然不是如何从 Flask 应用程序向另一个 URL 发送请求 😁 我会尽量让它更有用。首先，我会展示一个 **复杂处理程序的示例**，它具有额外的依赖项。然后，**模型服务器将返回图像**，而不仅仅是标签或概率。最后，这段代码可以作为 **在浏览器、Flask 应用和模型服务器之间以适当格式传递图像的工作示例**。最后，纯粹为了好玩，让我们在 Kubernetes 中部署整个解决方案。

GitHub 代码库在[这里](https://github.com/quasi-researcher/style_transfer)。

*(顺便说一下，如果你想看到一个非常简单的 web 应用 + TorchServe 用于图像分类的示例，请查看 git 中的 feature/classify 分支)*

在这篇文章中，我假设你已经对 TorchServe 的基础知识（处理程序、模型文件等）有所了解。如果没有，请参考之前的文章。

# 神经风格迁移

如果你不记得风格迁移是如何工作的，这里有一个简短的描述。理解高层次的概述是很重要的，以便了解在 TorchServe 处理程序中会发生什么。

在风格迁移中的“推断”不仅仅是将输入张量通过网络的一次传递。相反，一个张量（最终将成为输出图像）会被传递多次，并且张量本身会被修改，以最小化内容和风格损失函数。在每次迭代中，图像都会发生变化。而“推断”是这些迭代的序列。

对于解决方案来说，这意味着以下内容：

+   处理程序中的推断函数会相当复杂。将所有内容放在 *handler.py* 中会很混乱。因此，我会把它放在一个额外的模块中，并展示如何将其包含到 TorchServe 的工件中。

+   一个副作用是：“推断”将需要一些时间。为了不让用户在 Flask 重新加载整个页面时等待一会儿，我将使用从浏览器到应用程序的 ajax 请求。因此，浏览器中的页面不会冻结。

# 模型文件

解决方案中使用了预训练的 VGG19 模型。一般来说，我只是按照 PyTorch 的风格迁移官方示例进行了操作：[https://pytorch.org/tutorials/advanced/neural_style_tutorial.html](https://pytorch.org/tutorials/advanced/neural_style_tutorial.html)

我需要稍微修改模型的状态字典，以去除分类器层（80M vs. 500M 的完整 vgg19 模型）。你可以在仓库中的 *model_saving.ipynb* 笔记本里查看 .pth 文件是如何生成的。它生成了 *vgg19.pth* 文件。*model_nst.py* 包含了模型架构的定义。

# 处理程序

**预处理函数**

这几乎和你在第一篇文章中看到的相同。不同之处在于输入是两张图像，而函数必须只返回一个张量。因此，这两张图像会堆叠在一起，稍后会被拆分回来。

**后处理函数**

这个函数相当简单。唯一的问题是如何将图像传递给 Flask 应用，以便它可以正确地从 JSON 中读取。将其作为字节缓冲区传递对我有效。

**推理函数**

“推理”代码相当长。因此，我没有直接放在处理程序模块中。它位于 *utils.py* 文件中。正如我提到的，这段代码来自 PyTorch 官方的风格迁移示例，我不会详细介绍。

让我们来看看如何将额外模块包含到 TorchServe 的工件中。对于模型归档器，你需要指定要包含的额外文件：

```py
torch-model-archiver --model-name nst --version 1.0 \
 --model-file model_nst.py --serialized-file vgg19.pth \
 --handler handler_nst.py --extra-files utils.py
```

现在我们可以从 TorchServe 端开始了。让我简要介绍一下 web 应用。

# Flask 应用

该应用只有两个端点——一个是检查模型服务器的状态，另一个是生成带有所需风格的图像。当生成端点被调用时，应用会将内容和风格图像转发到 TorchServe 模型服务器。然后，它会解码接收到的生成图像并将其作为 JSON 对象返回。

正如我提到的，为了避免长时间等待整个页面重新加载，生成请求是通过 Ajax 发送的。因此，还有一个简单的 JQuery 脚本：

# 运行应用程序

如果你不想在 Kubernetes 中运行整个解决方案，你可以在此停止，然后从其目录启动模型服务器：

```py
torchserve --start --model-store . --models nst=nst.mar \
 --ts-config torch_config
```

以及应用服务器：

```py
python app.py
```

现在一切应该都能正常工作。打开浏览器中的 localhost:5000，上传内容和风格图像，点击“*transfer style*”，稍等片刻，你将会在浏览器中获得带有所需风格的生成图像。

# 在单节点集群中使用 Kubernetes 运行

对于在本地机器上运行 Kubernetes，请根据仓库中的 Dockerfile 创建名为 *flask_server* 和 *model_server* 的两个 Docker 镜像。还有一个 Kubernetes 的 yaml 文件。只需应用它即可：

```py
kubectl apply -f app_pod.yml 
```

别忘了端口转发（例如，将你的机器的 8700 端口绑定到 pod 的 5000 端口）：

```py
kubectl port-forward pod/application-pod 8700:5000
```

我们到了。打开浏览器并访问 *localhost:8700*。

![](../Images/fce46f319881ff3988c067a71dd4a120.png)

作者提供的图像

对于设计和 UI 请原谅，我只是个数据科学/机器学习工程师。我知道它很丑。😅

# 现场演示

对于下面的屏幕录制，我将使用我手头的个人照片，以避免任何版权问题。让我检查一下是否可以做个疯狂的事情：用国际定向地图规范的符号（[ISOM](https://orienteering.sport/iof/mapping/)）画我的猫。这些符号用于绘制运动定向比赛的地图。

![](../Images/5d7d7ddf62446fd5808ff4b858d9e390.png)

作者提供的图像

![](../Images/489f717964f935a6a13a73f9ce1d0fb6.png)

作者提供的图像

哇，看起来很有趣 🤪

![](../Images/072ad99dc33defcc757c92c26ed33b61.png)

作者提供的图像

# 结论

本文与前一篇文章一起展示了如何使用专用服务框架来服务你的 ML 模型，以及如何使用与应用服务器分离的模型服务器方法。

研究表明，TorchServe 允许灵活定制前处理、后处理和推理功能。因此，你可以融入模型中的任何复杂逻辑。

同样，带有端到端示例的 GitHub 仓库可以作为你自己实验的起点。

作为总结，让我提到一些模型服务器方法所提供的好处：

+   更高效地利用硬件（例如，模型服务器可以部署在配有GPU的机器上，而应用服务器可能不需要GPU）

+   专用服务框架提供了大规模服务模型的功能（例如，TorchServe中的线程和工作进程）

+   服务框架还提供了加速开发的功能（而且避免重复发明轮子）：模型版本控制、日志、指标等。

+   消息队列服务可以轻松添加以扩展解决方案

+   开发和 ML/DS 团队可以更独立地工作

这不是完整的列表，只是一些考虑使用专用框架来服务 ML 模型的理由。

希望你能在这篇文章中找到一些有用和实用的内容。
