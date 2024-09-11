# 低代码时间序列分析

> 原文：[https://towardsdatascience.com/low-code-time-series-analysis-2d5d02b5474b?source=collection_archive---------9-----------------------#2023-03-08](https://towardsdatascience.com/low-code-time-series-analysis-2d5d02b5474b?source=collection_archive---------9-----------------------#2023-03-08)

## 使用Darts简化Python时间序列分析的开发

[](https://pierpaoloippolito28.medium.com/?source=post_page-----2d5d02b5474b--------------------------------)[![Pier Paolo Ippolito](../Images/981abb84149adab275473b76bdbde66f.png)](https://pierpaoloippolito28.medium.com/?source=post_page-----2d5d02b5474b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d5d02b5474b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2d5d02b5474b--------------------------------) [Pier Paolo Ippolito](https://pierpaoloippolito28.medium.com/?source=post_page-----2d5d02b5474b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8391a6a5f1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flow-code-time-series-analysis-2d5d02b5474b&user=Pier+Paolo+Ippolito&userId=b8391a6a5f1a&source=post_page-b8391a6a5f1a----2d5d02b5474b---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d5d02b5474b--------------------------------) ·6分钟阅读·2023年3月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2d5d02b5474b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flow-code-time-series-analysis-2d5d02b5474b&user=Pier+Paolo+Ippolito&userId=b8391a6a5f1a&source=-----2d5d02b5474b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2d5d02b5474b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flow-code-time-series-analysis-2d5d02b5474b&source=-----2d5d02b5474b---------------------bookmark_footer-----------)![](../Images/17fb463f57753c82304972f72c3e8f14.png)

图片由[Afif Ramdhasuma](https://unsplash.com/@javaistan?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

时间序列预测是机器学习中的一个独特领域。当处理时间序列时，实际上系列中不同点之间存在固有的时间依赖性，因此不同观察值彼此高度依赖。如果你对时间序列分析的基础知识感兴趣，可以在[我之前的文章](https://pierpaolo28.github.io/blog/blog58/)中找到更多详细信息。

在经典的分类和回归问题中，[***scikit-learn***](https://scikit-learn.org/stable/) 能够提供我们开始时所需的大多数工具（例如数据预处理、低代码模型、评估指标等），但对于时间序列问题，情况却完全不同。多年来，许多专业化的库已被开发出来，以涵盖时间序列分析工作流中的一些关键步骤（例如 [***statsmodels***](https://www.statsmodels.org/stable/index.html)、[***Prophet***](https://facebook.github.io/prophet/)、自定义回测等），但直到 [***Darts***](https://unit8co.github.io/darts/) 的出现，才有可能在一个解决方案中涵盖所有内容。

# 演示

作为本文的一部分，我们将通过实际演示来介绍如何使用 Darts 分析 [Kaggle 上的德里每日气候时间序列数据集](https://www.kaggle.com/datasets/sumanthvrao/daily-climate-time-series-data?select=DailyDelhiClimateTrain.csv) [1]。所有使用的代码……
