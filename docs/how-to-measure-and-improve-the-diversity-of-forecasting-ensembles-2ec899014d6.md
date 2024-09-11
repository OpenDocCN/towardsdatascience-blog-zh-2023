# 如何衡量和提高预测集合的多样性

> 原文：[https://towardsdatascience.com/how-to-measure-and-improve-the-diversity-of-forecasting-ensembles-2ec899014d6?source=collection_archive---------19-----------------------#2023-01-31](https://towardsdatascience.com/how-to-measure-and-improve-the-diversity-of-forecasting-ensembles-2ec899014d6?source=collection_archive---------19-----------------------#2023-01-31)

## 使用偏差-方差-协方差分解分析预测集合

[](https://vcerq.medium.com/?source=post_page-----2ec899014d6--------------------------------)[![Vitor Cerqueira](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----2ec899014d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2ec899014d6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2ec899014d6--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----2ec899014d6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-measure-and-improve-the-diversity-of-forecasting-ensembles-2ec899014d6&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----2ec899014d6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2ec899014d6--------------------------------) · 5 分钟阅读 · 2023年1月31日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2ec899014d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-measure-and-improve-the-diversity-of-forecasting-ensembles-2ec899014d6&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----2ec899014d6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2ec899014d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-measure-and-improve-the-diversity-of-forecasting-ensembles-2ec899014d6&source=-----2ec899014d6---------------------bookmark_footer-----------)![](../Images/d94f4cf481cefb4c8be97700d5b30f16.png)

图片由 [henry perks](https://unsplash.com/@hjkp?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本文中，你将学习偏差-方差-协方差分解。

回归模型的误差可以通过偏差-方差权衡来分析。对于集合预测，这个误差还可以通过协方差项进一步分解。

下面是你如何利用这种分解方法来改善预测集合的介绍。

# 介绍

[个体模型之间的多样性是构建成功集成的重要因素](https://medium.com/towards-data-science/introduction-to-forecasting-ensembles-f63877a2498)。

每个模型都应该做出准确的预测。但这些预测也应该与其他模型有所不同。因此，组合预测能减少个体错误的影响。

这引出了两个问题：

1.  我们如何衡量集成的多样性？

1.  我们如何在集成中引入多样性？

让我们深入探讨这些问题。

# 衡量多样性

[偏差-方差权衡](https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff)是分析回归模型的标准方法。偏差与预测值与实际值之间的平均距离有关……
