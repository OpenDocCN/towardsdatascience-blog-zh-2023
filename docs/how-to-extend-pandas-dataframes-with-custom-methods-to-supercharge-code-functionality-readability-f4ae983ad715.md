# 如何通过自定义方法扩展 Pandas DataFrames，以增强代码功能和可读性

> 原文：[https://towardsdatascience.com/how-to-extend-pandas-dataframes-with-custom-methods-to-supercharge-code-functionality-readability-f4ae983ad715?source=collection_archive---------4-----------------------#2023-10-10](https://towardsdatascience.com/how-to-extend-pandas-dataframes-with-custom-methods-to-supercharge-code-functionality-readability-f4ae983ad715?source=collection_archive---------4-----------------------#2023-10-10)

## **逐步指南：通过自定义方法扩展 Pandas DataFrames，包括如何实现条件概率和期望值扩展的完整示例**

[](https://grahamharrison-86487.medium.com/?source=post_page-----f4ae983ad715--------------------------------)[![Graham Harrison](../Images/c6bfe00c6e0cfcdf3bd042c7fdc03554.png)](https://grahamharrison-86487.medium.com/?source=post_page-----f4ae983ad715--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f4ae983ad715--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f4ae983ad715--------------------------------) [Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----f4ae983ad715--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbd1c93739f33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-extend-pandas-dataframes-with-custom-methods-to-supercharge-code-functionality-readability-f4ae983ad715&user=Graham+Harrison&userId=bd1c93739f33&source=post_page-bd1c93739f33----f4ae983ad715---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f4ae983ad715--------------------------------) · 6 min 阅读 · 2023年10月10日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff4ae983ad715&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-extend-pandas-dataframes-with-custom-methods-to-supercharge-code-functionality-readability-f4ae983ad715&source=-----f4ae983ad715---------------------bookmark_footer-----------)![](../Images/a15437590c86b85c8418a6056b45fb95.png)

照片由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/hvSr_CVecVI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

# 介绍

## 问题

Pandas 数据框提供了多种内置函数，但 Python 程序员总是希望以新的方式查询和转换数据。

## 机会

一种常见的方法是编写函数并将数据框作为参数传递，但这可能会变得笨重，而最佳方法是直接将新方法和属性添加到 pandas 类中。

## 前进的道路

通过自定义方法和属性扩展 pandas 或其他库中的类功能非常容易，这样产生的代码也容易阅读、理解和重用。

# 背景

我一直在为一个因果推断库编写一些涉及复杂数据操作的代码，且持续…
