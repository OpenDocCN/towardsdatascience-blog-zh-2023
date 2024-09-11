# 10棵决策树优于1棵

> 原文：[https://towardsdatascience.com/10-decision-trees-are-better-than-1-719406680564?source=collection_archive---------2-----------------------#2023-02-27](https://towardsdatascience.com/10-decision-trees-are-better-than-1-719406680564?source=collection_archive---------2-----------------------#2023-02-27)

## 详细解析bagging、boosting、随机森林和AdaBoost

[](https://shawhin.medium.com/?source=post_page-----719406680564--------------------------------)[![Shaw Talebi](../Images/1449cc7c08890e2078f9e5d07897e3df.png)](https://shawhin.medium.com/?source=post_page-----719406680564--------------------------------)[](https://towardsdatascience.com/?source=post_page-----719406680564--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----719406680564--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----719406680564--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-decision-trees-are-better-than-1-719406680564&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----719406680564---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----719406680564--------------------------------) ·9分钟阅读·2023年2月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F719406680564&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-decision-trees-are-better-than-1-719406680564&user=Shaw+Talebi&userId=f3998e1cd186&source=-----719406680564---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F719406680564&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F10-decision-trees-are-better-than-1-719406680564&source=-----719406680564---------------------bookmark_footer-----------)

这是决策树系列的第二篇文章。在[上一篇文章](https://medium.com/towards-data-science/decision-trees-introduction-intuition-dac9592f4b7f)中，我们介绍了决策树，并讨论了如何使用数据*生长*它们。尽管它们是一种直观的机器学习方法，决策树却容易过拟合。这里我们探讨了通过**决策树集成**来解决这个过拟合问题。

## 关键点：

+   决策树集成将多个决策树组合成一个单一的估计器

+   决策树集成比单一决策树更不容易过拟合

# **决策树**

在这系列的[上一篇文章](https://medium.com/towards-data-science/decision-trees-introduction-intuition-dac9592f4b7f)中，我回顾了决策树以及我们如何利用它们进行预测。然而，对于许多现实世界的问题，单一的决策树往往容易受到偏差和过拟合的影响。

我们在上一篇博客的例子中看到了这一点，即使经过了一些超参数调优，我们的决策树仍然有35%的时间是错误的。
