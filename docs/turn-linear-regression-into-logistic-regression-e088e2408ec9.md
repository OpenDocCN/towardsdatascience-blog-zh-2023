# 将线性回归转化为逻辑回归

> 原文：[https://towardsdatascience.com/turn-linear-regression-into-logistic-regression-e088e2408ec9?source=collection_archive---------12-----------------------#2023-03-27](https://towardsdatascience.com/turn-linear-regression-into-logistic-regression-e088e2408ec9?source=collection_archive---------12-----------------------#2023-03-27)

## 从零开始实现逻辑回归的全面指南

[](https://zubairhossain.medium.com/?source=post_page-----e088e2408ec9--------------------------------)[![Md. Zubair](../Images/1b983a23226ce7561796fa5b28c00d65.png)](https://zubairhossain.medium.com/?source=post_page-----e088e2408ec9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e088e2408ec9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e088e2408ec9--------------------------------) [Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----e088e2408ec9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fdaeaeeea52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fturn-linear-regression-into-logistic-regression-e088e2408ec9&user=Md.+Zubair&userId=2fdaeaeeea52&source=post_page-2fdaeaeeea52----e088e2408ec9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e088e2408ec9--------------------------------) ·10分钟阅读·2023年3月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe088e2408ec9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fturn-linear-regression-into-logistic-regression-e088e2408ec9&user=Md.+Zubair&userId=2fdaeaeeea52&source=-----e088e2408ec9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe088e2408ec9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fturn-linear-regression-into-logistic-regression-e088e2408ec9&source=-----e088e2408ec9---------------------bookmark_footer-----------)![](../Images/764eaf14aa790304658c9ca236dfedb0.png)

照片由 [Rutger Leistra](https://unsplash.com/ko/@rutgerleistra?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 动机

如果你阅读了我之前关于[*简单线性回归*](https://medium.com/towards-data-science/deep-understanding-of-simple-linear-regression-3776afe34473)和[*多重线性回归*](https://medium.com/towards-data-science/multiple-linear-regression-a-deep-dive-f104c8ede236)的文章，你会了解到线性回归预测的是连续值。但并非所有的实际预测问题都与连续值相关。有时我们需要根据特征对对象或数据进行分类。线性回归算法无法解决这些问题。在这种情况下，逻辑回归的必要性就体现出来了。算法的标题‘**逻辑回归**’包含了**‘回归’**这个词。它是线性回归的修改版，以便可以预测离散的类别值，而不是连续值。

`所以，本文将解释逻辑回归如何生成从线性回归得出的类别预测值。`

## 目录

1.  `[**是什么使得线性回归变成逻辑回归？**](#b874)`

1.  `[**哪个函数起关键作用？**](#a923)`

1.  `[**线性回归如何转化为逻辑回归？**](#8b86)`

1.  `[**生成损失函数**](#7623)`

1.  `[**为什么我们不能使用均方误差作为成本函数？**](#7222)`
