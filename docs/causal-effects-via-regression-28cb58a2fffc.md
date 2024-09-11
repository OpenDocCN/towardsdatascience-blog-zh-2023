# 通过回归的因果效应

> 原文：[`towardsdatascience.com/causal-effects-via-regression-28cb58a2fffc?source=collection_archive---------7-----------------------#2023-01-10`](https://towardsdatascience.com/causal-effects-via-regression-28cb58a2fffc?source=collection_archive---------7-----------------------#2023-01-10)

## 3 种流行的技术与 Python 示例代码

[](https://shawhin.medium.com/?source=post_page-----28cb58a2fffc--------------------------------)![Shaw Talebi](https://shawhin.medium.com/?source=post_page-----28cb58a2fffc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----28cb58a2fffc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----28cb58a2fffc--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----28cb58a2fffc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcausal-effects-via-regression-28cb58a2fffc&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----28cb58a2fffc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----28cb58a2fffc--------------------------------) ·8 分钟阅读·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F28cb58a2fffc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcausal-effects-via-regression-28cb58a2fffc&user=Shaw+Talebi&userId=f3998e1cd186&source=-----28cb58a2fffc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F28cb58a2fffc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcausal-effects-via-regression-28cb58a2fffc&source=-----28cb58a2fffc---------------------bookmark_footer-----------)

这是关于[因果效应](https://shawhin.medium.com/understanding-causal-effects-37a054b2ec3b)系列文章中的第 5 篇。在之前的文章中，我们讨论了从数据中计算处理效应的不同方法。在这里，我介绍了一种通过三种流行的回归方法估计因果效应的替代方法。文章最后附上了如何在实践中使用这些技术的 Python 示例代码。

**关键点：**

+   回归是一种通过数据学习变量之间关系的方法

+   三种基于回归的流行方法用于估计因果效应：**线性回归**、**双重机器学习**和**元学习器**

![](img/da426069a321bd774489946b0979accc.png)

线性回归的因果效应示例。图片由作者提供。

# **什么是回归？**

**回归分析**是**一种通过数据学习变量之间关系的方法**。例如，帕布亚新几内亚[胡安树袋熊](https://www.zoo.org/view.image?Id=5582)的身高和体重之间的关系。

回归过程的输出称为**模型**。这本质上是**我们可以用来进行预测的东西**，例如……
