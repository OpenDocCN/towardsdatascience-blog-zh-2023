# 如何有效使用 loc Pandas 方法来处理你的数据框

> 原文：[`towardsdatascience.com/how-to-use-the-loc-pandas-method-to-efficiently-to-work-with-your-dataframe-880a068869df?source=collection_archive---------11-----------------------#2023-06-27`](https://towardsdatascience.com/how-to-use-the-loc-pandas-method-to-efficiently-to-work-with-your-dataframe-880a068869df?source=collection_archive---------11-----------------------#2023-06-27)

## PYTHON

## 使用 Pandas 探索和清理新数据集的技巧，包括代码示例和解释

[](https://byrondolon.medium.com/?source=post_page-----880a068869df--------------------------------)![Byron Dolon](https://byrondolon.medium.com/?source=post_page-----880a068869df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----880a068869df--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----880a068869df--------------------------------) [Byron Dolon](https://byrondolon.medium.com/?source=post_page-----880a068869df--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b5d063df5dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-the-loc-pandas-method-to-efficiently-to-work-with-your-dataframe-880a068869df&user=Byron+Dolon&userId=6b5d063df5dd&source=post_page-6b5d063df5dd----880a068869df---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----880a068869df--------------------------------) ·9 分钟阅读·2023 年 6 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F880a068869df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-the-loc-pandas-method-to-efficiently-to-work-with-your-dataframe-880a068869df&user=Byron+Dolon&userId=6b5d063df5dd&source=-----880a068869df---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F880a068869df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-the-loc-pandas-method-to-efficiently-to-work-with-your-dataframe-880a068869df&source=-----880a068869df---------------------bookmark_footer-----------)![](img/169efc98bb27204c164a53266439f37e.png)

这是 Pandas 探索数据的外部视角 — 图片由 [Jim Bread](https://unsplash.com/@jim_bread?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

与新数据集工作的关键部分是理解它。

理解数据中的基本内容，如列名、原始数据类型和数据的描述性统计，对于后续的正确数据处理非常重要。

Pandas 具有许多内置方法，你可以立即在笔记本中使用这些方法来探索数据。在进行数据探索的初步步骤时，你可以同时开始使数据适用于进一步分析或为训练机器学习模型做好准备。

在这篇文章中，我们将使用一组大学数据来回答以下问题，以展示在同时探索和清理数据时的情况：

+   **哪些大学只提供面对面的授课？**

+   **最早和最新创办的大学之间的年份范围是多少？**

我们主要将使用`loc`方法，并结合其他一些内置的 Pandas 方法来回答这些问题。首先，我们将…
