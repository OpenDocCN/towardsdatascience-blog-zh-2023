# PCA 在多变量时间序列中的应用：动态高维数据的预测

> 原文：[`towardsdatascience.com/pca-for-multivariate-time-series-forecasting-dynamic-high-dimensional-data-ab050a19e8db?source=collection_archive---------12-----------------------#2023-01-31`](https://towardsdatascience.com/pca-for-multivariate-time-series-forecasting-dynamic-high-dimensional-data-ab050a19e8db?source=collection_archive---------12-----------------------#2023-01-31)

## 噪声和序列相关性存在下的系统预测

[](https://medium.com/@cerlymarco?source=post_page-----ab050a19e8db--------------------------------)![Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----ab050a19e8db--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab050a19e8db--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab050a19e8db--------------------------------) [Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----ab050a19e8db--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc843902314c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpca-for-multivariate-time-series-forecasting-dynamic-high-dimensional-data-ab050a19e8db&user=Marco+Cerliani&userId=c843902314c7&source=post_page-c843902314c7----ab050a19e8db---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab050a19e8db--------------------------------) ·5 分钟阅读·2023 年 1 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fab050a19e8db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpca-for-multivariate-time-series-forecasting-dynamic-high-dimensional-data-ab050a19e8db&user=Marco+Cerliani&userId=c843902314c7&source=-----ab050a19e8db---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fab050a19e8db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpca-for-multivariate-time-series-forecasting-dynamic-high-dimensional-data-ab050a19e8db&source=-----ab050a19e8db---------------------bookmark_footer-----------)![](img/d24175ab02c0431b2c94255d1da09b50.png)

图片由 [Viva Luna Studios](https://unsplash.com/@vivalunastudios?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

多步预测多变量时间序列被认为是一项复杂的预测任务。我们必须注意输入和输出的大维度；我们需要适当地处理横截面和时间依赖性；最后但同样重要的是，我们必须确保长期预测的可接受水平。

现在，处理巨量数据的分析应用在时间和维度上非常普遍。因此，所有基于这些系统构建的解决方案必须能够处理大数据集。**在物联网（IoT）时代，处理大量时间序列是很常见的，这些时间序列通常显示出强烈的相关性模式**。这些动态现象在电信、工业制造、金融、电网等领域中非常常见。

假设你是一个数据科学家。我们负责开发一个预测分析应用，该应用提供一个由相关且嘈杂的传感器组成的物联网系统的多步预测。**多变量预测在文献中有广泛讨论**。从统计技术如 VAR（向量自回归模型）到更复杂和最近的深度学习方法…
