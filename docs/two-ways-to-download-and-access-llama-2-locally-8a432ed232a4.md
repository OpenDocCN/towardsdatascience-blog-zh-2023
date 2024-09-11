# 在本地下载和访问 Llama 2 的两种方式

> 原文：[`towardsdatascience.com/two-ways-to-download-and-access-llama-2-locally-8a432ed232a4?source=collection_archive---------0-----------------------#2023-09-05`](https://towardsdatascience.com/two-ways-to-download-and-access-llama-2-locally-8a432ed232a4?source=collection_archive---------0-----------------------#2023-09-05)

## 使用 Llama 2 在你的 PC 上的逐步指南

[](https://medium.com/@anna.bildea?source=post_page-----8a432ed232a4--------------------------------)![Ana Bildea, PhD](https://medium.com/@anna.bildea?source=post_page-----8a432ed232a4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8a432ed232a4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a432ed232a4--------------------------------) [Ana Bildea, PhD](https://medium.com/@anna.bildea?source=post_page-----8a432ed232a4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc57d3db39a47&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-ways-to-download-and-access-llama-2-locally-8a432ed232a4&user=Ana+Bildea%2C+PhD&userId=c57d3db39a47&source=post_page-c57d3db39a47----8a432ed232a4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a432ed232a4--------------------------------) ·10 分钟阅读·2023 年 9 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8a432ed232a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-ways-to-download-and-access-llama-2-locally-8a432ed232a4&user=Ana+Bildea%2C+PhD&userId=c57d3db39a47&source=-----8a432ed232a4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8a432ed232a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-ways-to-download-and-access-llama-2-locally-8a432ed232a4&source=-----8a432ed232a4---------------------bookmark_footer-----------)![](img/8096320260dd4a194856f7b6ae2ee973.png)

作者提供的图片（Dreamstudio）

# 动机

Meta 最新发布的 Llama 2 正在获得越来越多的关注，并且对于各种使用场景非常有趣。它提供了不同尺寸的预训练和微调的 Llama 2 语言模型，从 7B 到 70B 参数不等。Llama 2 在各种测试中表现良好，比如推理、编程、能力和知识基准，这使得它非常有前景。

在本文中，我们将引导你逐步完成在 PC 上下载 Llama 2 的过程。你有两个选择：官方 Meta AI 网站或 HuggingFace。我们还将向你展示如何访问它，以便你可以利用其强大的功能来进行项目。让我们深入了解一下吧！

# 前提条件

+   [Jupyter Notebook](https://jupyter.org/)

+   Nvidia T4 图形处理单元（GPU）

+   虚拟环境（Virtualenv）

+   [HuggingFace](https://huggingface.co/) 账户、库和 Llama 模型

+   Python 3.10

# 本地下载前需要考虑的事项

在将模型下载到本地计算机之前，请考虑几个问题。首先，确保你的计算机有足够的处理能力和存储空间……
