# 使用回归进行二元事件的概率预测

> 原文：[`towardsdatascience.com/probabilistic-forecasting-of-binary-events-using-regression-4f8a8022ec37?source=collection_archive---------7-----------------------#2023-03-08`](https://towardsdatascience.com/probabilistic-forecasting-of-binary-events-using-regression-4f8a8022ec37?source=collection_archive---------7-----------------------#2023-03-08)

## 使用累积分布函数预测极值的概率

[](https://vcerq.medium.com/?source=post_page-----4f8a8022ec37--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----4f8a8022ec37--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4f8a8022ec37--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4f8a8022ec37--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----4f8a8022ec37--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobabilistic-forecasting-of-binary-events-using-regression-4f8a8022ec37&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----4f8a8022ec37---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4f8a8022ec37--------------------------------) ·5 分钟阅读·2023 年 3 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4f8a8022ec37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobabilistic-forecasting-of-binary-events-using-regression-4f8a8022ec37&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----4f8a8022ec37---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4f8a8022ec37&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobabilistic-forecasting-of-binary-events-using-regression-4f8a8022ec37&source=-----4f8a8022ec37---------------------bookmark_footer-----------)![](img/fab758c65d153360deece4f407598819.png)

图片来源于 [Silas Baisch](https://unsplash.com/@silasbaisch?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我们将探讨时间序列中二元事件的概率预测。目标是预测时间序列超出临界阈值的概率。

你将学习如何（以及为什么）使用回归模型来计算二元概率。

# 介绍

首先，为什么要使用回归来计算二元概率，而不是使用分类器？

二元事件的概率预测通常被视为一个分类问题。然而，回归方法可能更可取，原因有两个：

1.  对点预测和事件概率都感兴趣；

1.  变化的超越阈值。

## 对点预测和事件概率都感兴趣

有时你可能想预测未来观测值的值以及相关事件的概率。

例如，在预测海洋波浪高度的情况下。海洋波浪是一个有前景的清洁能源来源……
