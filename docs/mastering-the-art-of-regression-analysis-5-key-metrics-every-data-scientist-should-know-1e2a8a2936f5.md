# 精通回归分析的艺术：每位数据科学家应该了解的5个关键指标

> 原文：[https://towardsdatascience.com/mastering-the-art-of-regression-analysis-5-key-metrics-every-data-scientist-should-know-1e2a8a2936f5?source=collection_archive---------2-----------------------#2023-02-20](https://towardsdatascience.com/mastering-the-art-of-regression-analysis-5-key-metrics-every-data-scientist-should-know-1e2a8a2936f5?source=collection_archive---------2-----------------------#2023-02-20)

## 关于回归分析中使用的指标的知识权威指南

[](https://federicotrotta.medium.com/?source=post_page-----1e2a8a2936f5--------------------------------)[![Federico Trotta](../Images/e997e3a96940c16ab5071629016d82fd.png)](https://federicotrotta.medium.com/?source=post_page-----1e2a8a2936f5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e2a8a2936f5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1e2a8a2936f5--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----1e2a8a2936f5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-the-art-of-regression-analysis-5-key-metrics-every-data-scientist-should-know-1e2a8a2936f5&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----1e2a8a2936f5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e2a8a2936f5--------------------------------) · 14分钟阅读 · 2023年2月20日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1e2a8a2936f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-the-art-of-regression-analysis-5-key-metrics-every-data-scientist-should-know-1e2a8a2936f5&source=-----1e2a8a2936f5---------------------bookmark_footer-----------)![](../Images/32e89ae41525384c0ef802641c329339.png)

由作者在 Dall-E 上创建的图像，提示语为“一个未来主义的机器人在黑板上教授数学”。

在监督学习的情况下，我们可以将机器学习问题细分为两个子组：回归问题和分类问题。

在本文中，我们将讨论在回归分析中用来判断模型是否适合解决特定机器学习问题的五个指标。

不过，首先，让我们回顾一下什么是回归分析。

**回归分析**是一种数学技术，用于寻找因变量与一个或多个自变量之间的函数关系。

在机器学习中，我们将“特征”定义为自变量，将“标签”（或“目标”）定义为因变量，因此回归分析的目标是找到特征和标签之间的估计（一个好的估计！）。

```py
**Table Of Contents**

[The residuals](#23ad)
1\. [The mean squared error (MSE)](#4bc5)
2\. [The root mean square error (RMSE)](#61cd)
3\. [The mean absolute error (MAE)](#ee39)
4\. [The Coefficient of Determination (R²)](#2db2)
5\. [The adjusted R²](#b164)
[Calculating all the Metrics in Python](#d3ed)
```

# 残差
