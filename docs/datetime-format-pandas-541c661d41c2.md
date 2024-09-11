# 如何更改 Pandas 中的日期时间格式

> 原文：[https://towardsdatascience.com/datetime-format-pandas-541c661d41c2?source=collection_archive---------12-----------------------#2023-01-06](https://towardsdatascience.com/datetime-format-pandas-541c661d41c2?source=collection_archive---------12-----------------------#2023-01-06)

## 在 Python 和 pandas 中处理日期时间

[](https://gmyrianthous.medium.com/?source=post_page-----541c661d41c2--------------------------------)[![Giorgos Myrianthous](../Images/ff4b116e4fb9a095ce45eb064fde5af3.png)](https://gmyrianthous.medium.com/?source=post_page-----541c661d41c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----541c661d41c2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----541c661d41c2--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----541c661d41c2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdatetime-format-pandas-541c661d41c2&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----541c661d41c2---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----541c661d41c2--------------------------------) ·4 min read·2023年1月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F541c661d41c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdatetime-format-pandas-541c661d41c2&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----541c661d41c2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F541c661d41c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdatetime-format-pandas-541c661d41c2&source=-----541c661d41c2---------------------bookmark_footer-----------)![](../Images/985c477abc99a4c06172f1b8a1734c98.png)

图片由 [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/LPRrEJU2GbQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Pandas 是 Python 生态系统中最常用的包之一，提供了丰富的功能，帮助开发者在内存中分析和转换数据。

pandas DataFrame中的日期时间构造的表示有时可能会变得有些具有挑战性，特别是如果你对库如何处理这些对象不是很熟悉的话。根据你想要将日期时间表示为pandas的`object` dtype(`string`)或者`datetime`，在处理这样的列时你可能会发现自己来回犹豫。

本文将演示如何在pandas中使用不同的日期时间对象表示以及如何更改格式甚至数据类型。

首先，让我们创建一个示例DataFrame，在整个教程中我们将引用它来演示一些概念。

```py
import pandas as pd

df = pd.DataFrame(
  [
    (1, 'A', '12/02/1993', True),
    (2, 'C', '01/01/2000', False),
    (3, 'A', '10/05/2010', False),
    (4, 'B', '12/03/1967', True),
    (5, 'B', '19/10/2001', True),
    (6, 'D', '12/03/2002', True),
    (7, 'B', '04/12/2011', False),
    (8, 'A', '01/06/1989', True),
    (9, 'C', '30/11/1980', False),
    (10…
```
