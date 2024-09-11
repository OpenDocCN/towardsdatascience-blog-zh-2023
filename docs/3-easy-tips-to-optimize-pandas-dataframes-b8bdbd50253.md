# 优化 Pandas DataFrames 的 3 个简单技巧

> 原文：[https://towardsdatascience.com/3-easy-tips-to-optimize-pandas-dataframes-b8bdbd50253?source=collection_archive---------5-----------------------#2023-05-17](https://towardsdatascience.com/3-easy-tips-to-optimize-pandas-dataframes-b8bdbd50253?source=collection_archive---------5-----------------------#2023-05-17)

## 使用经过验证的技巧来简化你的数据工作流程，以优化 Pandas DataFrames

[](https://guillaume-weingertner.medium.com/?source=post_page-----b8bdbd50253--------------------------------)[![Guillaume Weingertner](../Images/fbfb34af986a7788394b6033c6954d57.png)](https://guillaume-weingertner.medium.com/?source=post_page-----b8bdbd50253--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b8bdbd50253--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b8bdbd50253--------------------------------) [Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----b8bdbd50253--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ebea49e580e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-easy-tips-to-optimize-pandas-dataframes-b8bdbd50253&user=Guillaume+Weingertner&userId=4ebea49e580e&source=post_page-4ebea49e580e----b8bdbd50253---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b8bdbd50253--------------------------------) ·6 min read·2023年5月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb8bdbd50253&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-easy-tips-to-optimize-pandas-dataframes-b8bdbd50253&user=Guillaume+Weingertner&userId=4ebea49e580e&source=-----b8bdbd50253---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb8bdbd50253&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-easy-tips-to-optimize-pandas-dataframes-b8bdbd50253&source=-----b8bdbd50253---------------------bookmark_footer-----------)![](../Images/502a498c1d1efe1e1d5b036eff648ac7.png)

通过正确的数据管理技巧来获得空间 — 图片作者提供

# #0 介绍与基础案例

想象一下，你正在使用 Pandas 处理一个巨大的数据集，兴奋地期待发现有价值的洞察并做出数据驱动的决策。然而，当你深入分析时，遇到了一道障碍——内存使用。数据集越大，数据转换操作变得越慢，这妨碍了你的进展，让你感到困惑。

通过应用本文中介绍的简单最佳实践，你可以优化内存消耗并提高数据转换的性能。因此，让我们深入探讨如何高效地管理Pandas中的内存使用，让你能够无缝处理大型数据集并实现更快的数据处理。

![](../Images/470aa7774d01f3eda84d8c2dfd4207ed.png)

灵感时刻 — 图片由[Undraw](https://undraw.co/)提供

让我们直接进入正题！

首先，为了说明这一点，让我们构建一个虚拟的DataFrame，其中包含1,000,000个虚构的足球运动员数据：

```py
import pandas as pd
import numpy as np

def create_df(n):
    df…
```
