# 如何结合集合模型的预测结果

> 原文：[https://towardsdatascience.com/how-to-combine-the-forecasts-of-an-ensemble-11022e5cac25?source=collection_archive---------5-----------------------#2023-01-19](https://towardsdatascience.com/how-to-combine-the-forecasts-of-an-ensemble-11022e5cac25?source=collection_archive---------5-----------------------#2023-01-19)

## 使用动态预测集合应对时间序列的变化

[](https://vcerq.medium.com/?source=post_page-----11022e5cac25--------------------------------)[![维托尔·塞尔凯拉](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----11022e5cac25--------------------------------)[](https://towardsdatascience.com/?source=post_page-----11022e5cac25--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----11022e5cac25--------------------------------) [维托尔·塞尔凯拉](https://vcerq.medium.com/?source=post_page-----11022e5cac25--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-combine-the-forecasts-of-an-ensemble-11022e5cac25&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----11022e5cac25---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----11022e5cac25--------------------------------) ·6分钟阅读·2023年1月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F11022e5cac25&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-combine-the-forecasts-of-an-ensemble-11022e5cac25&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----11022e5cac25---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F11022e5cac25&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-combine-the-forecasts-of-an-ensemble-11022e5cac25&source=-----11022e5cac25---------------------bookmark_footer-----------)![](../Images/67181e5ede0e2d3fce12e999390d23fe.png)

[克里斯·劳顿](https://unsplash.com/@chrislawton?utm_source=medium&utm_medium=referral)的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在[上一篇文章](https://medium.com/towards-data-science/introduction-to-forecasting-ensembles-f63877a2498)中，我们探讨了构建集合模型的主要步骤。

在这篇文章中：

+   我们深入研究了预测集合模型；

+   我们讨论了集合模型如何结合多个预测结果；

+   我们探讨了用于预测组合的动态加权平均数；

+   我们使用Python对案例进行动态集合应用；

# 介绍

结合多个模型的预测结果可以[提高预测性能](https://medium.com/towards-data-science/introduction-to-forecasting-ensembles-f63877a2498)。这些方法可以通过动态组合规则进一步改进。

构建预测集成的方法有很多。然而，标准方法没有考虑时间序列的动态特性。

在时间序列中，由于存在非平稳性，事物会随时间演变。例如，不同的机制或季节性效应。这些会导致相对性能的变化。不同的预测模型在不同的时期表现更好。

性能的这种变化应在组合预测时体现出来。在每一个时间步骤中，每个模型应根据你的期望进行加权。
