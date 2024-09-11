# 决策树：介绍与直觉

> 原文：[https://towardsdatascience.com/decision-trees-introduction-intuition-dac9592f4b7f?source=collection_archive---------8-----------------------#2023-02-10](https://towardsdatascience.com/decision-trees-introduction-intuition-dac9592f4b7f?source=collection_archive---------8-----------------------#2023-02-10)

## 使用 Python 做出数据驱动的决策

[](https://shawhin.medium.com/?source=post_page-----dac9592f4b7f--------------------------------)[![Shaw Talebi](../Images/1449cc7c08890e2078f9e5d07897e3df.png)](https://shawhin.medium.com/?source=post_page-----dac9592f4b7f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dac9592f4b7f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dac9592f4b7f--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----dac9592f4b7f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-trees-introduction-intuition-dac9592f4b7f&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----dac9592f4b7f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dac9592f4b7f--------------------------------) ·10分钟阅读·2023年2月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdac9592f4b7f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-trees-introduction-intuition-dac9592f4b7f&user=Shaw+Talebi&userId=f3998e1cd186&source=-----dac9592f4b7f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdac9592f4b7f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-trees-introduction-intuition-dac9592f4b7f&source=-----dac9592f4b7f---------------------bookmark_footer-----------)![](../Images/13f81f75cc709437ff2192216ceadf8c.png)

图片来源：[niko photos](https://unsplash.com/@niko_photos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上，配有思考的表情符号。

这是有关决策树系列文章中的第一篇。在这篇文章中，我介绍了决策树，并描述了如何使用数据来*构建*它们。文章最后提供了示例 Python 代码，展示了如何创建和使用决策树来帮助进行医疗预测。

## 关键点：

+   决策树是一种广泛使用且直观的机器学习技术，用于解决预测问题。

+   我们可以从数据中*构建*决策树。

+   超参数调优可以帮助避免*过拟合*问题。

# **什么是决策树？**

**决策树**是一种**广泛使用且直观的机器学习技术**。通常，它们**用于解决预测问题**。例如，预测明天的天气预报或估计一个人患心脏病的概率。

他们通过一系列是非问题来缩小范围…
