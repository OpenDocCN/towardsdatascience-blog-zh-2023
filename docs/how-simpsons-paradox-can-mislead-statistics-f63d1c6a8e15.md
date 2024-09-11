# 如何让辛普森悖论误导统计学

> 原文：[`towardsdatascience.com/how-simpsons-paradox-can-mislead-statistics-f63d1c6a8e15?source=collection_archive---------8-----------------------#2023-01-18`](https://towardsdatascience.com/how-simpsons-paradox-can-mislead-statistics-f63d1c6a8e15?source=collection_archive---------8-----------------------#2023-01-18)

## 以及为什么机器学习的解释并不总是可靠

[](https://medium.com/@jacky.kaub?source=post_page-----f63d1c6a8e15--------------------------------)![Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----f63d1c6a8e15--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f63d1c6a8e15--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f63d1c6a8e15--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----f63d1c6a8e15--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ccb7065ef90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-simpsons-paradox-can-mislead-statistics-f63d1c6a8e15&user=Jacky+Kaub&userId=7ccb7065ef90&source=post_page-7ccb7065ef90----f63d1c6a8e15---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f63d1c6a8e15--------------------------------) ·12 分钟阅读·2023 年 1 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff63d1c6a8e15&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-simpsons-paradox-can-mislead-statistics-f63d1c6a8e15&user=Jacky+Kaub&userId=7ccb7065ef90&source=-----f63d1c6a8e15---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff63d1c6a8e15&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-simpsons-paradox-can-mislead-statistics-f63d1c6a8e15&source=-----f63d1c6a8e15---------------------bookmark_footer-----------)![](img/bf125e6aafe97bee3a80c16986995bba.png)

图片由 [Jason Leung](https://unsplash.com/es/@ninjason?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

如果有一件事我真的不喜欢听，那就是经典的权威论证“统计数据显示 *插入一个事实*”。

随着统计工具和机器学习的民主化，将数据转化为见解比以往任何时候都更容易。然而，每个人都应该注意不要陷入一些非常违背直觉的陷阱，这些陷阱可能导致偏见研究，这些研究可能过于仓促或缺乏专家的视角。

在本文中，我们将深入探讨一个经典的统计陷阱，即**辛普森悖论**，它可能由于存在隐藏的相关因素而导致对模型中特征解释的误解。

我设计这篇文章是为了那些拥有基本统计学和机器学习知识的人，特别是对悖论不了解的数据科学家和分析师。不过，第二部分中暴露的案例可能会引起任何愿意发现数据可以被操控以呈现正反两面情况的人的兴趣。

# 这个悖论是关于什么的？

你正在对一个数据集进行分析。你有一组特征 X1，… Xn，你正在使用这些特征…
