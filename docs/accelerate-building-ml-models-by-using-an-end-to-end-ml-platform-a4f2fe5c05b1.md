# 通过使用端到端的机器学习平台加速构建机器学习模型

> 原文：[`towardsdatascience.com/accelerate-building-ml-models-by-using-an-end-to-end-ml-platform-a4f2fe5c05b1?source=collection_archive---------9-----------------------#2023-03-21`](https://towardsdatascience.com/accelerate-building-ml-models-by-using-an-end-to-end-ml-platform-a4f2fe5c05b1?source=collection_archive---------9-----------------------#2023-03-21)

## 关于构建文本分类器的指导教程

[](https://medium.com/@konkiewicz.m?source=post_page-----a4f2fe5c05b1--------------------------------)![Magdalena Konkiewicz](https://medium.com/@konkiewicz.m?source=post_page-----a4f2fe5c05b1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4f2fe5c05b1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4f2fe5c05b1--------------------------------) [Magdalena Konkiewicz](https://medium.com/@konkiewicz.m?source=post_page-----a4f2fe5c05b1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6ba7ed0ad871&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faccelerate-building-ml-models-by-using-an-end-to-end-ml-platform-a4f2fe5c05b1&user=Magdalena+Konkiewicz&userId=6ba7ed0ad871&source=post_page-6ba7ed0ad871----a4f2fe5c05b1---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4f2fe5c05b1--------------------------------) ·8 分钟阅读·2023 年 3 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa4f2fe5c05b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faccelerate-building-ml-models-by-using-an-end-to-end-ml-platform-a4f2fe5c05b1&user=Magdalena+Konkiewicz&userId=6ba7ed0ad871&source=-----a4f2fe5c05b1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa4f2fe5c05b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faccelerate-building-ml-models-by-using-an-end-to-end-ml-platform-a4f2fe5c05b1&source=-----a4f2fe5c05b1---------------------bookmark_footer-----------)![](img/7f20e3c7d470f9c3f0abe2bb0a9f5696.png)

图片由 [OpenIcons](https://pixabay.com/users/openicons-28911/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=97860) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=97860)

**简介**

作为数据科学家，我们对用于构建机器学习算法的标准 Python 库很熟悉。你很可能使用过 pandas、NumPy、sci-kit learn、TensorFlow 或 PyTorch 来构建模型，然后使用 Docker、Kubernetes 和大型云服务提供商等工具来部署和服务这些模型。这些工具非常出色，为工程师提供了很大的灵活性，但将所有这些部分连接起来往往不是那么简单。最终，从初始数据收集、构建模型到最终在生产环境中服务，通常需要数周时间。

机器学习平台的创建旨在简化这一过程，使人们能够使用同一个工具完成所有必要的步骤，从而节省时间。这提升了生产力，允许快速原型开发，并最终可以减少开发机器学习解决方案所需的预算。

在这篇文章中，我将展示如何使用一个新的 [机器学习平台](https://tolokamodels.tech/home) 来构建文本分类模型，并且你可以在不到一个小时的时间内完成，无需编写代码。你将通过从目录中选择一个预训练模型来实现，不需要 GPU 或任何自己的资源来训练或部署模型。这…
