# 作为 Python 初学者必须学习的 4 个关键技巧

> 原文：[https://towardsdatascience.com/4-essential-techniques-you-must-learn-as-a-python-beginner-84a64ece3461?source=collection_archive---------3-----------------------#2023-02-09](https://towardsdatascience.com/4-essential-techniques-you-must-learn-as-a-python-beginner-84a64ece3461?source=collection_archive---------3-----------------------#2023-02-09)

## 以及如何使用它们，让你在下次面试中轻松脱颖而出

[](https://murtaza5152-ali.medium.com/?source=post_page-----84a64ece3461--------------------------------)[![Murtaza Ali](../Images/2aecff50999761022af29f9b30e2f925.png)](https://murtaza5152-ali.medium.com/?source=post_page-----84a64ece3461--------------------------------)[](https://towardsdatascience.com/?source=post_page-----84a64ece3461--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----84a64ece3461--------------------------------) [Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----84a64ece3461--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F607fa603b7ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-essential-techniques-you-must-learn-as-a-python-beginner-84a64ece3461&user=Murtaza+Ali&userId=607fa603b7ce&source=post_page-607fa603b7ce----84a64ece3461---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----84a64ece3461--------------------------------) ·8 分钟阅读·2023年2月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F84a64ece3461&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-essential-techniques-you-must-learn-as-a-python-beginner-84a64ece3461&user=Murtaza+Ali&userId=607fa603b7ce&source=-----84a64ece3461---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F84a64ece3461&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-essential-techniques-you-must-learn-as-a-python-beginner-84a64ece3461&source=-----84a64ece3461---------------------bookmark_footer-----------)![](../Images/384232c5c565694cf27955acb6a266aa.png)

图片由 [Carl Heyerdahl](https://unsplash.com/@carlheyerdahl?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## Lambda 函数

假设你正在 Jupyter notebook 中处理一些数据，进行一些快速探索和分析。你仍处于数据清理和处理的早期阶段，离任何生产就绪的模型、可视化或应用还远。然而你确实有一个截止日期需要遵守，所以你在快速而高效地探索，利用你出色的 Python 技能。

在你的冒险过程中，你遇到了一个需要转换的数据列。你只需要对列中的数字进行平方。这并不复杂，但不幸的是，它也是那些简单到足够快速，但复杂到没有内置函数的奇怪需求之一。

因此，你决定使用 [Pandas](https://pandas.pydata.org/docs/reference/api/pandas.Series.apply.html) 的 `[apply](https://pandas.pydata.org/docs/reference/api/pandas.Series.apply.html)` [函数来转换数据](https://pandas.pydata.org/docs/reference/api/pandas.Series.apply.html) 列，使用你自己定制的函数 [1]。为此，你需要编写一个平方数的函数，你用唯一知道的方法完成了这个任务：

```py
def square(num):
    return num * num
```

这能完成任务，但有点烦人和混乱，特别是对于 Jupyter notebook。它与单行代码不太融合…
