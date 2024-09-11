# 动态预测组合：使用 Python 调用 R

> 原文：[https://towardsdatascience.com/dynamic-forecast-combination-using-r-from-python-afcdf6adf85b?source=collection_archive---------14-----------------------#2023-01-25](https://towardsdatascience.com/dynamic-forecast-combination-using-r-from-python-afcdf6adf85b?source=collection_archive---------14-----------------------#2023-01-25)

## 探索 rpy2 从 Python 调用 R 方法

[](https://vcerq.medium.com/?source=post_page-----afcdf6adf85b--------------------------------)[![Vitor Cerqueira](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----afcdf6adf85b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----afcdf6adf85b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----afcdf6adf85b--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----afcdf6adf85b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-forecast-combination-using-r-from-python-afcdf6adf85b&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----afcdf6adf85b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----afcdf6adf85b--------------------------------) ·6 min read·2023年1月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fafcdf6adf85b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-forecast-combination-using-r-from-python-afcdf6adf85b&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----afcdf6adf85b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fafcdf6adf85b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-forecast-combination-using-r-from-python-afcdf6adf85b&source=-----afcdf6adf85b---------------------bookmark_footer-----------)![](../Images/bd1918e4b14c3cdaf4f5ae5c2e4a0479.png)

图片来自 [Louis Hansel](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，你将学习如何使用库 rpy2 从 Python 调用 R 方法。

我们将讨论一个与预测相关的示例。我们将定义并运行结合了基于 Python 的模型所做的预测的 R 函数。

# 介绍

即使 Python 是你首选的语言，R 有时仍然可能很有用。

我不想讨论 Python 和 R 的优劣。如今我主要使用 Python。然而，许多优秀的方法仅在 R 中可用。重新从头实现这些方法实在是太麻烦了。

库 rpy2 已经为我们做好了准备。它允许你在 Python 中运行 R 代码。像 *matrix* 或 *data.frame* 这样的 R 数据结构会被转换为 numpy 或 pandas 对象。将自定义 R 函数集成到你的 Python 工作流程中也很简单。

那么，rpy2 是如何工作的呢？

# 使用 Opera 的示例

我们将重点介绍使用 R 包 opera。你可以使用这个包来组合预测。

在深入 rpy2 之前，让我们回顾一下我们要解决的问题。

## 预测集合的入门指南
