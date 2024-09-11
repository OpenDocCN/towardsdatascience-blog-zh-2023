# 过拟合、欠拟合与正则化

> 原文：[https://towardsdatascience.com/overfitting-underfitting-and-regularization-7f83dd998a62?source=collection_archive---------9-----------------------#2023-02-15](https://towardsdatascience.com/overfitting-underfitting-and-regularization-7f83dd998a62?source=collection_archive---------9-----------------------#2023-02-15)

## 与人工智能交朋友

## 偏差-方差权衡，第三部分中的第二部分

[](https://kozyrkov.medium.com/?source=post_page-----7f83dd998a62--------------------------------)[![Cassie Kozyrkov](../Images/ad18dd12979a4a3ec130bdf8b889af23.png)](https://kozyrkov.medium.com/?source=post_page-----7f83dd998a62--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7f83dd998a62--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7f83dd998a62--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----7f83dd998a62--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fccb851bb5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foverfitting-underfitting-and-regularization-7f83dd998a62&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=post_page-2fccb851bb5e----7f83dd998a62---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f83dd998a62--------------------------------) ·4分钟阅读·2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7f83dd998a62&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foverfitting-underfitting-and-regularization-7f83dd998a62&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=-----7f83dd998a62---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7f83dd998a62&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foverfitting-underfitting-and-regularization-7f83dd998a62&source=-----7f83dd998a62---------------------bookmark_footer-----------)

在[第一部分](http://bit.ly/quaesita_bivar1)中，我们介绍了许多基本术语以及有关偏差-方差公式（**MSE = Bias² + Variance**）的一些关键见解，包括来自[安娜·卡列尼娜](https://www.goodreads.com/work/quotes/2507928)的这段话：

> 所有完美的模型都是相似的，但每个不完美的模型都有其独特的不幸。

为了最大化*这篇*文章的价值，我建议先阅读[第一部分](http://bit.ly/quaesita_bivar1)，以确保你能更好地理解本文。

![](../Images/6073e1755019bc63fad47ad5d1bed129.png)

欠拟合与过拟合…… 图片由作者提供。

# 过拟合/欠拟合与此有何关系？

假设你有一个和你所拥有的信息最匹配的[模型](http://bit.ly/quaesita_emperorm)。

要拥有一个更好的模型，你需要更好的[数据](http://bit.ly/quaesita_hist)。换句话说，更多的数据（数量）或*更相关*的数据（质量）。

当我说“尽可能好”时，我指的是在数据上，你的模型之前没有见过的情况下的[均方误差](http://bit.ly/quaesita_msefav)表现。（它应该是[*预测*](http://bit.ly/quaesita_parrot)，而不是*后验推断*。）你已经在从你所拥有的信息中尽可能地获取了所需的东西——剩下的误差是你无法用你现有的信息解决的。
