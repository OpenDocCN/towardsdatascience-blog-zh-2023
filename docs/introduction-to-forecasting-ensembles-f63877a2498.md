# 预测集成介绍

> 原文：[https://towardsdatascience.com/introduction-to-forecasting-ensembles-f63877a2498?source=collection_archive---------8-----------------------#2023-01-12](https://towardsdatascience.com/introduction-to-forecasting-ensembles-f63877a2498?source=collection_archive---------8-----------------------#2023-01-12)

## **一个提高预测性能的廉价技巧**

[](https://vcerq.medium.com/?source=post_page-----f63877a2498--------------------------------)[![Vitor Cerqueira](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----f63877a2498--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f63877a2498--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f63877a2498--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----f63877a2498--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-forecasting-ensembles-f63877a2498&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----f63877a2498---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f63877a2498--------------------------------) ·5 min read·2023年1月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff63877a2498&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-forecasting-ensembles-f63877a2498&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----f63877a2498---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff63877a2498&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-forecasting-ensembles-f63877a2498&source=-----f63877a2498---------------------bookmark_footer-----------)![](../Images/41f1f4833476c2b810d89fef95d3e395.png)

图片由 [Natalie Pedigo](https://unsplash.com/@nataliepedigo?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你需要挤出额外的性能，预测组合可能正是你所寻找的。

预测组合是将多个模型的预测结果结合起来的过程。这种技术也称为集成预测。

在这里，你将学习创建预测集成的3个主要步骤。

# 为什么使用集成？

没有一种预测方法是完美的。

创建预测模型有几种技术。例如，经典方法包括ARIMA或指数平滑，或机器学习方法，如决策树或神经网络。你可以 [查看我的上一篇文章](https://medium.com/towards-data-science/machine-learning-for-forecasting-transformations-and-feature-extraction-bbbea9de0ac2)，了解如何使用时间序列进行监督学习。

每种方法对数据有自己的假设，而这些假设并不总是成立。每种方法都有其优点和局限性。管理这两者是集成方法的一个关键动机。

组合多个模型通常可以带来更准确的预测。这是因为它减少了选择错误模型的可能性。

此外，集成方法可以有效地传达未来观察的**不确定性**。高预测…
