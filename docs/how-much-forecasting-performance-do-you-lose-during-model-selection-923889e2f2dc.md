# 你在模型选择过程中会损失多少预测性能？

> 原文：[https://towardsdatascience.com/how-much-forecasting-performance-do-you-lose-during-model-selection-923889e2f2dc?source=collection_archive---------23-----------------------#2023-01-06](https://towardsdatascience.com/how-much-forecasting-performance-do-you-lose-during-model-selection-923889e2f2dc?source=collection_archive---------23-----------------------#2023-01-06)

## 交叉验证有多频繁能够选出最佳预测模型？当它无法做到这一点时会发生什么？

[](https://vcerq.medium.com/?source=post_page-----923889e2f2dc--------------------------------)[![Vitor Cerqueira](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----923889e2f2dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----923889e2f2dc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----923889e2f2dc--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----923889e2f2dc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-much-forecasting-performance-do-you-lose-during-model-selection-923889e2f2dc&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----923889e2f2dc---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----923889e2f2dc--------------------------------) ·5分钟阅读·2023年1月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F923889e2f2dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-much-forecasting-performance-do-you-lose-during-model-selection-923889e2f2dc&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----923889e2f2dc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F923889e2f2dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-much-forecasting-performance-do-you-lose-during-model-selection-923889e2f2dc&source=-----923889e2f2dc---------------------bookmark_footer-----------)![](../Images/39b7e74a9c3a5f831df25ddcdefda989.png)

图片由[Héctor J. Rivas](https://unsplash.com/@hjrc33?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

假设你有一个预测问题。你需要选择一个模型来解决它。[你可能想用交叉验证测试一些备选方案](/4-things-to-do-when-applying-cross-validation-with-time-series-c6a5674ebf3a).

你是否曾经想过，交叉验证选择最佳模型的机会有多大？如果没有，所选模型的表现会差到什么程度？

让我们来找出答案。

# 介绍

交叉验证，无论是时间序列还是其他情况，都解决了两个问题：

+   **性能估计。** 模型在新数据上的表现如何？你可以使用这些估计来评估模型是否可以部署；

+   **模型选择。** 利用上述估计对一组可用模型进行排序。例如，不同的学习算法配置用于超参数调优。在这种情况下，你选择性能估计最好的模型。

等等！这两个问题不是一样的吗？

实际上并不是这样。给定的方法（例如，[TimeSeriesSplits](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html)）可能在平均情况下提供良好的性能估计。但它在排名可用模型时可能效果不佳……
