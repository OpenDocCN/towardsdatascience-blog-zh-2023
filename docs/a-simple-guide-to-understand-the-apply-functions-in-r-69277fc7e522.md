# 理解 R 中的 apply() 函数的简单指南

> 原文：[https://towardsdatascience.com/a-simple-guide-to-understand-the-apply-functions-in-r-69277fc7e522?source=collection_archive---------6-----------------------#2023-10-06](https://towardsdatascience.com/a-simple-guide-to-understand-the-apply-functions-in-r-69277fc7e522?source=collection_archive---------6-----------------------#2023-10-06)

## 学习如何一次性掌握这些有用的函数

[](https://gustavorsantos.medium.com/?source=post_page-----69277fc7e522--------------------------------)[![古斯塔沃·圣托斯](../Images/a19a9f4525cdeb6e7a76cd05246aa622.png)](https://gustavorsantos.medium.com/?source=post_page-----69277fc7e522--------------------------------)[](https://towardsdatascience.com/?source=post_page-----69277fc7e522--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----69277fc7e522--------------------------------) [古斯塔沃·圣托斯](https://gustavorsantos.medium.com/?source=post_page-----69277fc7e522--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-guide-to-understand-the-apply-functions-in-r-69277fc7e522&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----69277fc7e522---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----69277fc7e522--------------------------------) ·8分钟阅读·2023年10月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F69277fc7e522&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-guide-to-understand-the-apply-functions-in-r-69277fc7e522&user=Gustavo+Santos&userId=4429d99b1245&source=-----69277fc7e522---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F69277fc7e522&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-guide-to-understand-the-apply-functions-in-r-69277fc7e522&source=-----69277fc7e522---------------------bookmark_footer-----------)![](../Images/d38e794bd5b719ca31a8e398cfe549a7.png)

[照片来源于凯利·西克马](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/0iKjge_aOVo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

我将通过说明我每天都在使用**R**和**Python**语言来开始这篇文章。老实说，我觉得在Python中使用`apply`函数的方式更容易和直观。

思考其背后的原因，我认为是因为 Python 里没有太多的选择。R 语言提供了许多不同的选项，我喜欢称之为*apply 函数系列*。

我记得曾经看到有人说，他们总是直接使用循环来解决问题，因为他们总是记不住每个 apply 函数的功能，以及哪个函数最适合用于某种情况。

嗯，我希望这些问题在这篇文章之后会有所改善。我希望阅读本文的读者能够对函数系列有一个好的理解，并了解如何以及何时使用它们。

为了进行这些练习，我们快速创建一个示例数据框，没有太多的标准。包括一个 ID、产品名称、销售数量以及两个不同时间段的金额。

```py
# Create dataset
dtf <- data.frame(
  id = 1:100,
  product= sample(c('product A'…
```
