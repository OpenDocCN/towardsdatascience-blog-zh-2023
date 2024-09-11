# Python 中的列表、元组、字典和数据框：完整指南

> 原文：[`towardsdatascience.com/lists-tuples-dictionaries-and-data-frames-in-python-the-complete-guide-7ab54d4819ee?source=collection_archive---------5-----------------------#2023-05-24`](https://towardsdatascience.com/lists-tuples-dictionaries-and-data-frames-in-python-the-complete-guide-7ab54d4819ee?source=collection_archive---------5-----------------------#2023-05-24)

## 你需要了解的一切，以掌握 Python 中最常用的数据结构

[](https://federicotrotta.medium.com/?source=post_page-----7ab54d4819ee--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----7ab54d4819ee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ab54d4819ee--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ab54d4819ee--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----7ab54d4819ee--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flists-tuples-dictionaries-and-data-frames-in-python-the-complete-guide-7ab54d4819ee&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----7ab54d4819ee---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ab54d4819ee--------------------------------) ·16 分钟阅读·2023 年 5 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7ab54d4819ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flists-tuples-dictionaries-and-data-frames-in-python-the-complete-guide-7ab54d4819ee&user=Federico+Trotta&userId=654cd4bbe899&source=-----7ab54d4819ee---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7ab54d4819ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flists-tuples-dictionaries-and-data-frames-in-python-the-complete-guide-7ab54d4819ee&source=-----7ab54d4819ee---------------------bookmark_footer-----------)![](img/30c2615c37ccc11a8c930496d8e8e34a.png)

图片由 [Pexels](https://pixabay.com/it/users/pexels-2286921/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1284475) 提供，来源于 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1284475)

如果你已经开始学习 Python，无论你是想成为软件工程师还是数据科学家，你都必须掌握数据结构。

Python 有很多数据结构，允许我们存储数据。在这篇文章中，我们将深入探讨最常用的数据结构。因此，如果你刚开始你的职业生涯并需要学习数据结构，那么这篇文章绝对适合你。

这里是你将找到的内容：

```py
**Table of Contents:** 
Lists
  Definitions and creation examples
  Lists manipulation
  List comprehension
  List of lists
Tuples
Dictionaries
  Dictionaries manipulation
  Nested dictionaries
  Dictionary comprehension
Data frames
   Basic data frames manipulations with Pandas
```

# 列表

## 定义和创建示例

在 Python 中，列表是一个有序元素的集合，这些元素可以是任何类型的：字符串、整数、浮点数等…

要创建列表，项必须放在方括号之间，并用逗号分隔。例如，以下是如何创建一个整数列表：
