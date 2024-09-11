# Python 列表：处理有序数据集合的终极指南

> 原文：[https://towardsdatascience.com/python-lists-the-definitive-guide-for-working-with-ordered-collections-of-data-53b06a194826?source=collection_archive---------11-----------------------#2023-07-19](https://towardsdatascience.com/python-lists-the-definitive-guide-for-working-with-ordered-collections-of-data-53b06a194826?source=collection_archive---------11-----------------------#2023-07-19)

## Python 列表的全面指南

[](https://federicotrotta.medium.com/?source=post_page-----53b06a194826--------------------------------)[![Federico Trotta](../Images/e997e3a96940c16ab5071629016d82fd.png)](https://federicotrotta.medium.com/?source=post_page-----53b06a194826--------------------------------)[](https://towardsdatascience.com/?source=post_page-----53b06a194826--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----53b06a194826--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----53b06a194826--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-lists-the-definitive-guide-for-working-with-ordered-collections-of-data-53b06a194826&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----53b06a194826---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----53b06a194826--------------------------------) ·10 min read·2023年7月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F53b06a194826&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-lists-the-definitive-guide-for-working-with-ordered-collections-of-data-53b06a194826&user=Federico+Trotta&userId=654cd4bbe899&source=-----53b06a194826---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F53b06a194826&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-lists-the-definitive-guide-for-working-with-ordered-collections-of-data-53b06a194826&source=-----53b06a194826---------------------bookmark_footer-----------)![](../Images/f7fec4e1603355e89c91a3e935b6ae4c.png)

图片由 [Jill Wellington](https://pixabay.com/it/users/jillwellington-334088/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1902136) 提供，来源于 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1902136)

在编程时，我们总是要处理数据结构。我的意思是我们需要将信息存储在某处，以便以后可以重复使用它。

Python 是一种非常灵活的编程语言，提供了使用不同类型数据结构的可能性。

在这篇文章中，我们将分析 Python 列表。因此，如果你是 Python 初学者，并且在寻找关于列表的综合指南，那么这篇文章绝对适合你。

你将在这里学到以下内容：

```py
Table of Contents:

What is a list in Python?
The top 9 features in Python lists, with examples
  How to create a list in Python
  Accessing list elements
  Modifying the elements of a list
  Adding elements to a list
  Removing elements from a list
  Concatenating lists
  Calculating the lenght of a list
  Sorting the elements of a list
  List comprehension
```

# Python 中的列表是什么？

在 Python 中，列表是一种内置的数据结构，允许我们以文本或数字的形式存储和操作数据。

列表以有序的方式存储数据，这意味着列表的元素可以是…
