# 《PySpark 线性回归初学者指南》

> 原文：[`towardsdatascience.com/beginners-guide-to-linear-regression-with-pyspark-bfc39b45a9e9?source=collection_archive---------5-----------------------#2023-01-11`](https://towardsdatascience.com/beginners-guide-to-linear-regression-with-pyspark-bfc39b45a9e9?source=collection_archive---------5-----------------------#2023-01-11)

## 一步步的教程，教你如何使用 PySpark 代码构建线性回归模型

[](https://medium.com/@yazihejazi?source=post_page-----bfc39b45a9e9--------------------------------)![Yasmine Hejazi](https://medium.com/@yazihejazi?source=post_page-----bfc39b45a9e9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bfc39b45a9e9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bfc39b45a9e9--------------------------------) [Yasmine Hejazi](https://medium.com/@yazihejazi?source=post_page-----bfc39b45a9e9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe2d3b1ba6be6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeginners-guide-to-linear-regression-with-pyspark-bfc39b45a9e9&user=Yasmine+Hejazi&userId=e2d3b1ba6be6&source=post_page-e2d3b1ba6be6----bfc39b45a9e9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bfc39b45a9e9--------------------------------) · 5 分钟阅读 · 2023 年 1 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbfc39b45a9e9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeginners-guide-to-linear-regression-with-pyspark-bfc39b45a9e9&user=Yasmine+Hejazi&userId=e2d3b1ba6be6&source=-----bfc39b45a9e9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbfc39b45a9e9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeginners-guide-to-linear-regression-with-pyspark-bfc39b45a9e9&source=-----bfc39b45a9e9---------------------bookmark_footer-----------)![](img/aa8d595a8c5e4b04bbfacc0474adb58d.png)

照片由 [eskay lim](https://unsplash.com/@eskaylim?utm_source=medium&utm_medium=referral) 提供，刊登在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

PySpark 是 Apache Spark 的 Python API。它允许我们使用高级编程语言进行编码，同时享受分布式计算的好处。通过内存计算、利用并行处理的分布式计算和本机机器学习库，我们解锁了对数据处理效率的巨大提升，这对于数据扩展至关重要。

本教程将逐步介绍如何使用在[ggplot2](https://ggplot2.tidyverse.org/reference/#data)上找到的[Diamonds data](https://ggplot2.tidyverse.org/reference/diamonds.html)创建 PySpark 线性回归模型。Databricks 在[Databricks Utilities (](https://docs.databricks.com/dev-tools/databricks-utils.html)`[dbutils](https://docs.databricks.com/dev-tools/databricks-utils.html)`[)](https://docs.databricks.com/dev-tools/databricks-utils.html)上托管了此数据集，以便于加载。该数据包含了数值型和类别型特征，我们可以利用这些特征来构建我们的模型。我们将处理如何预处理数据，以便我们可以简单地训练模型并生成预测结果。而且，我们会在不接触 Pandas 的情况下完成这一过程！

![](img/712aad12eeba673e8726d581a96c4995.png)

[Tahlia Doyle](https://unsplash.com/@tahliaclaire?utm_source=medium&utm_medium=referral)拍摄的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你已经熟悉在 Pandas 中进行线性回归的编码，过程是类似的。回归模型将预测**价格**。
