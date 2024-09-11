# 数据科学中的高效编码：Pandas 链式操作的简单调试

> 原文：[https://towardsdatascience.com/efficient-coding-in-data-science-easy-debugging-of-pandas-chained-operations-0089f6de920f?source=collection_archive---------9-----------------------#2023-11-15](https://towardsdatascience.com/efficient-coding-in-data-science-easy-debugging-of-pandas-chained-operations-0089f6de920f?source=collection_archive---------9-----------------------#2023-11-15)

## PYTHON 编程

## 如何在链式操作中检查 Pandas 数据框，而不将链式操作拆分成单独的语句

[](https://medium.com/@nyggus?source=post_page-----0089f6de920f--------------------------------)[![Marcin Kozak](../Images/d7faf62e48ed81dab5d8ad92819fff54.png)](https://medium.com/@nyggus?source=post_page-----0089f6de920f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0089f6de920f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----0089f6de920f--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----0089f6de920f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-coding-in-data-science-easy-debugging-of-pandas-chained-operations-0089f6de920f&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----0089f6de920f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0089f6de920f--------------------------------) ·9 分钟阅读·2023年11月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0089f6de920f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-coding-in-data-science-easy-debugging-of-pandas-chained-operations-0089f6de920f&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----0089f6de920f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0089f6de920f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-coding-in-data-science-easy-debugging-of-pandas-chained-operations-0089f6de920f&source=-----0089f6de920f---------------------bookmark_footer-----------)![](../Images/a4416aaa0e5cbb71ac3371cb86b01eae.png)

在不打断链式操作的情况下调试 Pandas 操作是可能的。照片由 [Miltiadis Fragkidis](https://unsplash.com/@_miltiadis_?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

调试是编程的核心。我在以下文章中讨论了这一点：

[## Python中的Bug？Pdb来救援！](https://towardsdatascience.com/bugs-in-python-pdb-to-the-rescue-d88a56a2ca71?source=post_page-----0089f6de920f--------------------------------)

### Pdb 调试器值得学习和使用吗？

[towardsdatascience.com](https://towardsdatascience.com/bugs-in-python-pdb-to-the-rescue-d88a56a2ca71?source=post_page-----0089f6de920f--------------------------------)

这个说法相当一般，独立于语言和框架。当你使用 Python 进行数据分析时，无论是进行复杂的数据分析、编写机器学习软件产品，还是创建 Streamlit 或 Django 应用，你都需要调试代码。

这篇文章讨论了调试 Pandas 代码，或者说调试 Pandas 代码的一种特定场景，即操作链式地组合成一个管道。这种调试方法提出了一个具有挑战性的问题。当你不知道如何操作时，链式 Pandas 操作似乎比普通 Pandas 代码更难调试，即那些使用方括号进行典型赋值的单个 Pandas 操作。
