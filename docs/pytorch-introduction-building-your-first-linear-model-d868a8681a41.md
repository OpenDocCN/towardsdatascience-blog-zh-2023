# PyTorch 介绍 — 构建你的第一个线性模型

> 原文：[`towardsdatascience.com/pytorch-introduction-building-your-first-linear-model-d868a8681a41?source=collection_archive---------5-----------------------#2023-12-12`](https://towardsdatascience.com/pytorch-introduction-building-your-first-linear-model-d868a8681a41?source=collection_archive---------5-----------------------#2023-12-12)

## 学习如何通过使用“神奇”的线性层来构建你的第一个 PyTorch 模型

[](https://ivopbernardo.medium.com/?source=post_page-----d868a8681a41--------------------------------)![Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----d868a8681a41--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d868a8681a41--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d868a8681a41--------------------------------) [Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----d868a8681a41--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74eec53531c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-introduction-building-your-first-linear-model-d868a8681a41&user=Ivo+Bernardo&userId=74eec53531c0&source=post_page-74eec53531c0----d868a8681a41---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d868a8681a41--------------------------------) ·8 分钟阅读·2023 年 12 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd868a8681a41&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-introduction-building-your-first-linear-model-d868a8681a41&user=Ivo+Bernardo&userId=74eec53531c0&source=-----d868a8681a41---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd868a8681a41&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpytorch-introduction-building-your-first-linear-model-d868a8681a41&source=-----d868a8681a41---------------------bookmark_footer-----------)![](img/48de4c355c69f4666a6e07d34fb89e52.png)

回归模型 — AI 生成的图像

在我上一个博客文章中，我们学习了[如何使用 PyTorch 张量](https://medium.com/towards-data-science/pytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b?sk=2cf4d44549664fc647baa3455e9d78e8)，这是 PyTorch 库中最重要的对象。张量是深度学习模型的核心，因此我们自然可以利用它们来将更简单的机器学习模型拟合到我们的数据集上。

尽管 PyTorch 以其深度学习能力而闻名，但我们也能够使用该框架拟合简单的线性模型——这实际上是熟悉`torch` API 的最佳方式之一！

在这篇博客文章中，我们将继续 PyTorch 介绍系列，通过检查如何使用`torch`库开发一个简单的线性回归模型。在此过程中，我们将学习`torch`优化器、权重以及我们学习模型的其他参数，这对更复杂的架构将极为有用。

让我们开始吧！

# **加载和处理数据**

在这篇博客文章中，我们将使用歌曲流行度数据集，我们希望基于某些歌曲特征来预测某首歌的流行度。下面是数据集的前几行：

```py
songPopularity =…
```
