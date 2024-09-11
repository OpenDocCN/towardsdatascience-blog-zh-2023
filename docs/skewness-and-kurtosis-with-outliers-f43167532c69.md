# 偏度和峰度与异常值

> 原文：[`towardsdatascience.com/skewness-and-kurtosis-with-outliers-f43167532c69?source=collection_archive---------6-----------------------#2023-05-10`](https://towardsdatascience.com/skewness-and-kurtosis-with-outliers-f43167532c69?source=collection_archive---------6-----------------------#2023-05-10)

## R 统计系列

[](https://mdsohel-mahmood.medium.com/?source=post_page-----f43167532c69--------------------------------)![Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----f43167532c69--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f43167532c69--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f43167532c69--------------------------------) [Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----f43167532c69--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd4a38d280273&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fskewness-and-kurtosis-with-outliers-f43167532c69&user=Md+Sohel+Mahmood&userId=d4a38d280273&source=post_page-d4a38d280273----f43167532c69---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f43167532c69--------------------------------) · 6 分钟阅读 · 2023 年 5 月 10 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff43167532c69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fskewness-and-kurtosis-with-outliers-f43167532c69&user=Md+Sohel+Mahmood&userId=d4a38d280273&source=-----f43167532c69---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff43167532c69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fskewness-and-kurtosis-with-outliers-f43167532c69&source=-----f43167532c69---------------------bookmark_footer-----------)![](img/c7075583a80e99925747d4c12c5571be.png)

图片由 [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，出处 [Unsplash](https://unsplash.com/photos/6jYoil2GhVk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> **介绍**

现实世界的数据往往包含极端值，这可能导致偏斜的分布。偏斜的数据不适合许多统计分析。单个异常值的存在可以显著改变分布的总体统计。因此，这些极端值需要谨慎处理。如果没有合理的理由说明这些异常值的存在，通常的指导方针是将其剔除。在本文中，我们将深入探讨异常值对偏度以及峰度的影响。

下图展示了一个正态分布数据的示例，这对于多种统计分析非常有用。

![](img/fd5861d5af18c21b0c2fe2b7b718b114.png)

图片由作者提供

> **偏度**

在统计学中，偏度是衡量非对称分布的一个指标。基本上，它描述了钟形曲线从对称形式的扭曲程度。偏度可以分为两种类型：

1.  显示正偏度的分布在分布的右侧有一个比左侧的尾巴更长或更宽的尾巴。分布的均值大于其中位数。
