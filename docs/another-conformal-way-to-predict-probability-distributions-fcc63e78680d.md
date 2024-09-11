# 另一种（保形）预测概率分布的方法

> 原文：[`towardsdatascience.com/another-conformal-way-to-predict-probability-distributions-fcc63e78680d?source=collection_archive---------8-----------------------#2023-03-08`](https://towardsdatascience.com/another-conformal-way-to-predict-probability-distributions-fcc63e78680d?source=collection_archive---------8-----------------------#2023-03-08)

## **Catboost**的**保形多分位回归**。

[](https://harrisonfhoffman.medium.com/?source=post_page-----fcc63e78680d--------------------------------)![Harrison Hoffman](https://harrisonfhoffman.medium.com/?source=post_page-----fcc63e78680d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fcc63e78680d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fcc63e78680d--------------------------------) [**Harrison Hoffman**](https://harrisonfhoffman.medium.com/?source=post_page-----fcc63e78680d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38889d0801d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanother-conformal-way-to-predict-probability-distributions-fcc63e78680d&user=Harrison+Hoffman&userId=38889d0801d0&source=post_page-38889d0801d0----fcc63e78680d---------------------post_header-----------) 发表在 [**Towards Data Science**](https://towardsdatascience.com/?source=post_page-----fcc63e78680d--------------------------------) · 11 min 阅读 · 2023 年 3 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffcc63e78680d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanother-conformal-way-to-predict-probability-distributions-fcc63e78680d&user=Harrison+Hoffman&userId=38889d0801d0&source=-----fcc63e78680d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffcc63e78680d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanother-conformal-way-to-predict-probability-distributions-fcc63e78680d&source=-----fcc63e78680d---------------------bookmark_footer-----------)![](img/01cf92583ef3188b5333a9ebc8b32e34.png)

德克萨斯州。图像作者提供。

在[上一篇文章](https://medium.com/towards-data-science/a-new-way-to-predict-probability-distributions-e7258349f464)中，我们探讨了 Catboost 的多分位数损失函数的能力，该函数允许使用单一模型预测多个分位数。这种方法巧妙地克服了传统分位数回归的一个局限性，即需要为每个分位数开发一个单独的模型，或者将整个训练集存储在模型中。然而，分位数回归还有另一个缺点，我们将在本文中讨论：预测的分位数可能存在偏差，无法保证校准和覆盖。本文将展示一种使用符合性多分位数回归来克服这个问题的方法。我鼓励那些没有跟随本系列的人在阅读之前回顾以下文章：

[](/a-new-way-to-predict-probability-distributions-e7258349f464?source=post_page-----fcc63e78680d--------------------------------) ## 一种预测概率分布的新方法

### 使用 Catboost 探索多分位数回归

towardsdatascience.com
