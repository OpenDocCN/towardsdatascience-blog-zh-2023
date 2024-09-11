# 正则表达式在数据科学中的实用性

> 原文：[`towardsdatascience.com/the-utility-of-regular-expressions-in-data-science-babf94285966?source=collection_archive---------15-----------------------#2023-01-05`](https://towardsdatascience.com/the-utility-of-regular-expressions-in-data-science-babf94285966?source=collection_archive---------15-----------------------#2023-01-05)

## 使用 Python 进行常见应用的示例

[](https://thomasdorfer.medium.com/?source=post_page-----babf94285966--------------------------------)![Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----babf94285966--------------------------------)[](https://towardsdatascience.com/?source=post_page-----babf94285966--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----babf94285966--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----babf94285966--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c54f9b62b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-utility-of-regular-expressions-in-data-science-babf94285966&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=post_page-7c54f9b62b90----babf94285966---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----babf94285966--------------------------------) ·7 分钟阅读·2023 年 1 月 5 日

--

[！](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbabf94285966&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-utility-of-regular-expressions-in-data-science-babf94285966&source=-----babf94285966---------------------bookmark_footer-----------)![](img/5966cb182cb7e1ef7df827d76ab910c0.png)

[Kevin Ku](https://unsplash.com/@ikukevk) 摄影，图片来源于 [Unsplash](https://unsplash.com/photos/w7ZyuGYNpRQ)

# 介绍

数据科学家们经常面临这样一种情况：他们需要确定数据中的特定字段是否符合所需的文本格式，或者某个特定的字符串是否存在。在其他情况下，他们可能需要用另一个字符串替换数据中的某个特定字符串。为了实现这一点，他们利用了已经成为这些问题标准解决方案的**正则表达式**。

本文将简要介绍正则表达式是什么，介绍一些用于形成相应搜索模式的基本字符，说明在 Python 中常用的一些函数，最后讲解一些数据科学家日常工作中经常遇到的实际应用案例。

# 什么是正则表达式？

正则表达式，或称 Regex，是一组字符，它可以用于搜索和——如果需要——替换特定的文本模式。这是一种极其便利的技术，数据科学家可以运用它来绕过繁琐且繁重的手动搜索任务。
