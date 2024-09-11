# 如何在 PySpark 中实现随机森林回归

> 原文：[`towardsdatascience.com/how-to-implement-random-forest-regression-in-pyspark-9582f4964285?source=collection_archive---------3-----------------------#2023-09-25`](https://towardsdatascience.com/how-to-implement-random-forest-regression-in-pyspark-9582f4964285?source=collection_archive---------3-----------------------#2023-09-25)

## PySpark 关于使用随机森林进行回归建模的教程

[](https://medium.com/@yazihejazi?source=post_page-----9582f4964285--------------------------------)![Yasmine Hejazi](https://medium.com/@yazihejazi?source=post_page-----9582f4964285--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9582f4964285--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9582f4964285--------------------------------) [Yasmine Hejazi](https://medium.com/@yazihejazi?source=post_page-----9582f4964285--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe2d3b1ba6be6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-implement-random-forest-regression-in-pyspark-9582f4964285&user=Yasmine+Hejazi&userId=e2d3b1ba6be6&source=post_page-e2d3b1ba6be6----9582f4964285---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9582f4964285--------------------------------) · 6 分钟阅读 · 2023 年 9 月 25 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9582f4964285&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-implement-random-forest-regression-in-pyspark-9582f4964285&source=-----9582f4964285---------------------bookmark_footer-----------)![](img/68a991e06a39b6a7830a528d8b9a5fb6.png)

图片由 [Jachan DeVol](https://unsplash.com/@jachan_devol?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

PySpark 是一个强大的数据处理引擎，建立在 Apache Spark 之上，旨在进行大规模数据处理。它提供了可扩展性、速度、多功能性、与其他工具的集成、易用性、内置机器学习库和实时处理能力。它是高效、有效处理大规模数据任务的理想选择，其用户友好的界面允许用 Python 轻松编写代码。

使用 [Diamonds 数据](https://ggplot2.tidyverse.org/reference/diamonds.html)（数据来源于 ggplot2 ([source](https://plotly.com/ggplot2/), [license](https://ggplot2.tidyverse.org/LICENSE.html))），我们将演示如何实现一个随机森林回归模型，并用 PySpark 分析结果。如果你想了解如何在 PySpark 中将线性回归应用于同一数据集，可以 在这里查看！

本教程将涵盖以下步骤：

1.  加载并准备数据，转换为向量输入

1.  使用 MLlib 中的 RandomForestRegressor 训练模型

1.  使用 MLlib 中的 RegressionEvaluator 评估模型性能

1.  绘制并分析特征重要性以提高模型透明度
