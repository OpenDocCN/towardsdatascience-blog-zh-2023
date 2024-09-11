# 揭示超越标准差的真实数据离散度的两个指标

> 原文：[`towardsdatascience.com/dispersion-cv-qcd-32849f828434?source=collection_archive---------1-----------------------#2023-07-30`](https://towardsdatascience.com/dispersion-cv-qcd-32849f828434?source=collection_archive---------1-----------------------#2023-07-30)

## 数据科学

## 计算和解释变异系数和分位数离散系数的指南

[](https://medium.ealizadeh.com/?source=post_page-----32849f828434--------------------------------)![Essi Alizadeh](https://medium.ealizadeh.com/?source=post_page-----32849f828434--------------------------------)[](https://towardsdatascience.com/?source=post_page-----32849f828434--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----32849f828434--------------------------------) [Essi Alizadeh](https://medium.ealizadeh.com/?source=post_page-----32849f828434--------------------------------)

·

在[数据科学之路](https://towardsdatascience.com/?source=post_page-----32849f828434--------------------------------)上发布的[文章](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F35a932e89ec1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdispersion-cv-qcd-32849f828434&user=Essi+Alizadeh&userId=35a932e89ec1&source=post_page-35a932e89ec1----32849f828434---------------------post_header-----------) · 阅读时间 8 分钟·2023 年 7 月 30 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F32849f828434&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdispersion-cv-qcd-32849f828434&source=-----32849f828434---------------------bookmark_footer-----------)![](img/01bc45bb1b6fc65f93b1357ec3318b97.png)

图像由作者使用 StockImg.AI 生成

# 介绍

我们都听过这样一句话：“多样性是生活的调味品”，在数据中，这种多样性或多样性通常表现为离散度。

数据离散度通过突出我们 otherwise 不会发现的模式和见解，使数据变得迷人。通常，我们使用以下测量指标来衡量离散度：方差、标准差、范围和四分位距（IQR）。然而，在某些情况下，我们可能需要超越这些典型测量指标来检查数据集的离散度。

这就是变异系数（CV）和四分位数离散系数（QCD）在比较数据集时提供见解的地方。

在本教程中，我们将深入探讨**CV**和**QCD**这两个概念，并回答以下问题：

+   它们是什么，它们是如何定义的？

+   它们如何计算？

+   如何解读结果？
