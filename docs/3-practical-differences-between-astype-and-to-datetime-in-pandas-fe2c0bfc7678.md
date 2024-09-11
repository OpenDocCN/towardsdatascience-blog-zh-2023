# 在 Pandas 中，`astype()` 和 `to_datetime()` 之间的 3 个实际区别

> 原文：[https://towardsdatascience.com/3-practical-differences-between-astype-and-to-datetime-in-pandas-fe2c0bfc7678?source=collection_archive---------8-----------------------#2023-07-28](https://towardsdatascience.com/3-practical-differences-between-astype-and-to-datetime-in-pandas-fe2c0bfc7678?source=collection_archive---------8-----------------------#2023-07-28)

## 使用 Python 进行数据科学

## 进行有效数据分析所需了解的区别

[](https://medium.com/@17.rsuraj?source=post_page-----fe2c0bfc7678--------------------------------)[![Suraj Gurav](../Images/f5dca32861f8c1c428e66fbe2174c04b.png)](https://medium.com/@17.rsuraj?source=post_page-----fe2c0bfc7678--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe2c0bfc7678--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fe2c0bfc7678--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----fe2c0bfc7678--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-practical-differences-between-astype-and-to-datetime-in-pandas-fe2c0bfc7678&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----fe2c0bfc7678---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe2c0bfc7678--------------------------------) ·7 分钟阅读·2023年7月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffe2c0bfc7678&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-practical-differences-between-astype-and-to-datetime-in-pandas-fe2c0bfc7678&user=Suraj+Gurav&userId=1fdda183cca2&source=-----fe2c0bfc7678---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffe2c0bfc7678&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-practical-differences-between-astype-and-to-datetime-in-pandas-fe2c0bfc7678&source=-----fe2c0bfc7678---------------------bookmark_footer-----------)![](../Images/d11105bd34759c351e92de644b0f4178.png)

图片由 [Alessandro D’Antonio](https://unsplash.com/@alessandrodantonio?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/qy4vrr2qi3M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

选择正确的数据类型转换方法以实现高效的数据分析！

在我之前的两篇文章中，你可以探索有关处理日期时间或时间序列数据的技巧和提示，分别是[**Python**](/3-powerful-tricks-to-work-with-date-time-data-in-python-67c2d3834338)和[**Pandas**](/3-useful-pandas-tips-to-work-with-datetime-data-424afbec628b)。

在Pandas中处理时间序列数据时，你可以使用`pandas.Series.astype()`或`pandas.to_datetime()`将日期时间字符串转换为`datetime64[ns]`数据类型。这两种方法返回完全相同的输出。

然而，它们在性能、灵活性和错误处理方式上存在**显著差异**。当你了解这些差异时，选择正确的数据类型转换方法将变得更容易。

在本文中，你将了解`pandas.Series.astype()`和`to_datetime()`方法之间的这3个实际差异。以下是你可以在本文中探索的主题的快速概述 —

**·** [**astype()与to_datetime()的性能差异**](#9a52) **·** [**日期和时间的处理**](#5cab) **·** [**错误处理**](#f932)
