# Ludwig — 一个“更友好”的深度学习框架

> 原文：[https://towardsdatascience.com/ludwig-a-friendlier-deep-learning-framework-946ee3d3b24?source=collection_archive---------12-----------------------#2023-06-26](https://towardsdatascience.com/ludwig-a-friendlier-deep-learning-framework-946ee3d3b24?source=collection_archive---------12-----------------------#2023-06-26)

## 使用这种低代码、声明式框架简化深度学习

[](https://johnadeojo.medium.com/?source=post_page-----946ee3d3b24--------------------------------)[![John Adeojo](../Images/f6460fae462b055d36dce16fefcd142c.png)](https://johnadeojo.medium.com/?source=post_page-----946ee3d3b24--------------------------------)[](https://towardsdatascience.com/?source=post_page-----946ee3d3b24--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----946ee3d3b24--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----946ee3d3b24--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fludwig-a-friendlier-deep-learning-framework-946ee3d3b24&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----946ee3d3b24---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----946ee3d3b24--------------------------------) ·11分钟阅读·2023年6月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F946ee3d3b24&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fludwig-a-friendlier-deep-learning-framework-946ee3d3b24&user=John+Adeojo&userId=f933e1637e40&source=-----946ee3d3b24---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F946ee3d3b24&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fludwig-a-friendlier-deep-learning-framework-946ee3d3b24&source=-----946ee3d3b24---------------------bookmark_footer-----------)![](../Images/ee8f8ddbdb0f202acd51b5a02d1e371e.png)

图片来源：由 Midjourney 生成

# 背景 — 深度学习，过于复杂？

我一直倾向于避开深度学习在工业应用中的使用。这并不是因为缺乏兴趣，而是我发现流行的深度学习框架非常繁琐。我欣赏 [Pytorch](https://pytorch.org/) 和 [TensorFlow](https://www.tensorflow.org/) 是用于研究的绝佳工具，但它们的 API 并不是最用户友好的。在需要为客户快速交付概念验证时，我最不希望做的就是调整 Pytorch 张量。

在伦敦参加人工智能峰会时，我遇到了一个团队，他们声称拥有解决我深度学习问题的方法。他们采用了一种不同的方法，他们称之为“TensorFlow和AutoML之间的中点”，一种叫做Ludwig的框架。

## Ludwig是什么？

由Uber开发的[ Ludwig](https://ludwig.ai/latest/)是一个开源的深度学习模型构建框架。它是声明式的，这意味着你不需要像在TensorFlow中那样一层层构建复杂的模型，而是只需通过配置文件声明模型的结构。这听起来好得令人难以置信，所以我必须亲自去看看。在本文的剩余部分……
