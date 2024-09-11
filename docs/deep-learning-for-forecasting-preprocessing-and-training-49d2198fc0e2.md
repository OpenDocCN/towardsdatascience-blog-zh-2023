# 深度学习用于预测：预处理与训练

> 原文：[https://towardsdatascience.com/deep-learning-for-forecasting-preprocessing-and-training-49d2198fc0e2?source=collection_archive---------2-----------------------#2023-03-22](https://towardsdatascience.com/deep-learning-for-forecasting-preprocessing-and-training-49d2198fc0e2?source=collection_archive---------2-----------------------#2023-03-22)

## 如何使用多条时间序列训练深度神经网络

[](https://vcerq.medium.com/?source=post_page-----49d2198fc0e2--------------------------------)[![Vitor Cerqueira](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----49d2198fc0e2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----49d2198fc0e2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----49d2198fc0e2--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----49d2198fc0e2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-learning-for-forecasting-preprocessing-and-training-49d2198fc0e2&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----49d2198fc0e2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----49d2198fc0e2--------------------------------) ·8分钟阅读·2023年3月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F49d2198fc0e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-learning-for-forecasting-preprocessing-and-training-49d2198fc0e2&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----49d2198fc0e2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F49d2198fc0e2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-learning-for-forecasting-preprocessing-and-training-49d2198fc0e2&source=-----49d2198fc0e2---------------------bookmark_footer-----------)![](../Images/be89556b550a8794748eb6a4e9d68f9b.png)

图片来源：[Tamara Malaniy](https://unsplash.com/de/@tamarushphotos?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文是对[之前一篇文章](https://medium.com/towards-data-science/how-to-transform-time-series-for-deep-learning-3b6abbbb3726)的后续。在那里，我们学习了如何将时间序列转换为适用于深度学习的格式。

我们继续探索用于预测的深度神经网络。在这篇文章中，我们将：

+   学习如何使用深度学习训练全球预测模型，包括基本的预处理步骤；

+   探索 Keras 回调以驱动神经网络的训练过程。

# 深度学习用于预测

深度神经网络通过自回归方法解决预测问题。 [自回归是一种建模技术，它涉及利用过去的观察值来预测未来的观察值](https://medium.com/towards-data-science/machine-learning-for-forecasting-transformations-and-feature-extraction-bbbea9de0ac2)。

深度神经网络可以以不同的方式设计，例如递归或卷积架构。递归神经网络通常更适用于时间序列数据。除了其他原因，这种类型的网络在建模长期依赖关系方面表现优异。这一特性对预测性能有着显著影响。

下面是如何定义一种特定类型的递归神经网络，称为 LSTM（长短期记忆）。注释提供了每个模型的简要描述……
