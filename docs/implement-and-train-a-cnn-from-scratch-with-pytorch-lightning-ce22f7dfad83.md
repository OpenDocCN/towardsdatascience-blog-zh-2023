# 使用 PyTorch Lightning 从零实现并训练 CNN

> 原文：[https://towardsdatascience.com/implement-and-train-a-cnn-from-scratch-with-pytorch-lightning-ce22f7dfad83?source=collection_archive---------7-----------------------#2023-08-08](https://towardsdatascience.com/implement-and-train-a-cnn-from-scratch-with-pytorch-lightning-ce22f7dfad83?source=collection_archive---------7-----------------------#2023-08-08)

## 如果你还没有使用 PyTorch Lightning，你应该尝试一下。

[](https://medium.com/@betty.ld?source=post_page-----ce22f7dfad83--------------------------------)[![Betty LD](../Images/1a908f5bcdb6cbc3be5f889f52d743e6.png)](https://medium.com/@betty.ld?source=post_page-----ce22f7dfad83--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce22f7dfad83--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ce22f7dfad83--------------------------------) [Betty LD](https://medium.com/@betty.ld?source=post_page-----ce22f7dfad83--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9e6de59677a9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-and-train-a-cnn-from-scratch-with-pytorch-lightning-ce22f7dfad83&user=Betty+LD&userId=9e6de59677a9&source=post_page-9e6de59677a9----ce22f7dfad83---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce22f7dfad83--------------------------------) ·19分钟阅读·2023年8月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fce22f7dfad83&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-and-train-a-cnn-from-scratch-with-pytorch-lightning-ce22f7dfad83&user=Betty+LD&userId=9e6de59677a9&source=-----ce22f7dfad83---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fce22f7dfad83&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-and-train-a-cnn-from-scratch-with-pytorch-lightning-ce22f7dfad83&source=-----ce22f7dfad83---------------------bookmark_footer-----------)![](../Images/3c624d26acc13295bc8fc840a43add42.png)

PyTorch Lightning 的抽象概念。来源：[Marc Sendra Martorell](https://unsplash.com/@marcsm)。

这篇文章是对卷积神经网络（CNN）的温和介绍。本文详细说明了 PyTorch Lightning 的优点，然后简要理论介绍了 CNN 组件，接着描述了如何使用 PyTorch Lightning 库从头编写简单 CNN 架构的训练循环。

# **为什么选择 PyTorch Lightning？**

PyTorch 是一个灵活且用户友好的库。如果 PyTorch 对于研究非常出色，那么我发现 Lightning 在工程方面更加出色。主要优点有：

+   **减少代码量**。在运行机器学习项目时，可能出现许多问题，因此将模板代码委托出去并专注于解决特定问题是很有帮助的。使用内置功能可以减少书写代码的数量，从而降低错误的概率。开发（和调试）时间也会减少。

+   **结构良好的代码**。

+   **效率和快速训练**。Lightning 还允许使用 PyTorch 的所有多进程和并行工作技巧（如[DDP](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html)），无需额外编写代码。

+   内置的**开发工具**…
