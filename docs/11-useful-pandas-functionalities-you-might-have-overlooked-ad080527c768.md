# 11 个你可能忽略的有用 Pandas 功能

> 原文：[https://towardsdatascience.com/11-useful-pandas-functionalities-you-might-have-overlooked-ad080527c768?source=collection_archive---------5-----------------------#2023-04-19](https://towardsdatascience.com/11-useful-pandas-functionalities-you-might-have-overlooked-ad080527c768?source=collection_archive---------5-----------------------#2023-04-19)

![](../Images/f04b4d78572c570020ca2d3c003a525f.png)

使用 Midjourney 生成的图像

## 探索 Pandas 隐藏宝石系列的第三部分

[](https://eryk-lewinson.medium.com/?source=post_page-----ad080527c768--------------------------------)[![Eryk Lewinson](../Images/56e09e19c0bbfecc582da58761d15078.png)](https://eryk-lewinson.medium.com/?source=post_page-----ad080527c768--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ad080527c768--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ad080527c768--------------------------------) [Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----ad080527c768--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44bc27317e6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F11-useful-pandas-functionalities-you-might-have-overlooked-ad080527c768&user=Eryk+Lewinson&userId=44bc27317e6b&source=post_page-44bc27317e6b----ad080527c768---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad080527c768--------------------------------) ·9 分钟阅读·2023年4月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fad080527c768&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F11-useful-pandas-functionalities-you-might-have-overlooked-ad080527c768&user=Eryk+Lewinson&userId=44bc27317e6b&source=-----ad080527c768---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fad080527c768&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F11-useful-pandas-functionalities-you-might-have-overlooked-ad080527c768&source=-----ad080527c768---------------------bookmark_footer-----------)

我很确定 `pandas` 不需要介绍。在本系列的第三篇文章中，我们将继续深入探讨一些你可能未曾听说过的 `pandas` 实用功能。

如果你想查看之前的部分，可以在下面找到它们：

[](/9-useful-pandas-methods-you-probably-have-not-heard-about-28ff6c0bceee?source=post_page-----ad080527c768--------------------------------) [## 9 个你可能未听说过的有用 Pandas 方法

### 它们可以使你的日常工作变得更轻松、更高效。

towardsdatascience.com](/9-useful-pandas-methods-you-probably-have-not-heard-about-28ff6c0bceee?source=post_page-----ad080527c768--------------------------------) [](/8-more-useful-pandas-functionalities-for-your-analyses-ef87dcfe5d74?source=post_page-----ad080527c768--------------------------------) [## 8 个对你的分析更有用的 Pandas 功能

### 它们可以使你的日常工作更轻松、更高效

towardsdatascience.com](/8-more-useful-pandas-functionalities-for-your-analyses-ef87dcfe5d74?source=post_page-----ad080527c768--------------------------------)

让我们直接开始吧！

## 简短的设置

由于我们将使用简单的玩具示例来探索`pandas`的功能，因此我们不需要长长的库列表来完成这项任务。

```py
import pandas as pd
import numpy as np
```
