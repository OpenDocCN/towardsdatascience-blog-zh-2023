# 视觉化网络图的新最佳 Python 包

> 原文：[`towardsdatascience.com/the-new-best-python-package-for-visualising-network-graphs-e220d59e054e?source=collection_archive---------0-----------------------#2023-11-23`](https://towardsdatascience.com/the-new-best-python-package-for-visualising-network-graphs-e220d59e054e?source=collection_archive---------0-----------------------#2023-11-23)

## 关于谁应该使用它、何时使用它、如何使用它以及为什么我之前错了的指南…

[](https://medium.com/@bl3e967?source=post_page-----e220d59e054e--------------------------------)![Benjamin Lee](https://medium.com/@bl3e967?source=post_page-----e220d59e054e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e220d59e054e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e220d59e054e--------------------------------) [Benjamin Lee](https://medium.com/@bl3e967?source=post_page-----e220d59e054e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4d3d41f467d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-new-best-python-package-for-visualising-network-graphs-e220d59e054e&user=Benjamin+Lee&userId=4d3d41f467d1&source=post_page-4d3d41f467d1----e220d59e054e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e220d59e054e--------------------------------) ·10 分钟阅读·2023 年 11 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe220d59e054e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-new-best-python-package-for-visualising-network-graphs-e220d59e054e&user=Benjamin+Lee&userId=4d3d41f467d1&source=-----e220d59e054e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe220d59e054e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-new-best-python-package-for-visualising-network-graphs-e220d59e054e&source=-----e220d59e054e---------------------bookmark_footer-----------)![](img/777d06e439c1fcdd3bf93623df5bf919.png)

图片来源：[Chris Ried](https://unsplash.com/@cdr6934?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在这篇文章中，我将向你介绍一个我偶然发现的 Python 包，它在我谦逊的意见中，是我见过的*最佳*网络图可视化工具。

对于需要紧凑而强大的可视化包来快速原型设计、探索数据分析或调试网络模型的数据科学家而言，下面的内容最为适用。

我们将要检查的包叫做：[gravis](https://robert-haas.github.io/gravis-docs/)

[](https://robert-haas.github.io/gravis-docs/?source=post_page-----e220d59e054e--------------------------------) [## gravis — gravis 0.1.0 文档

### 编辑描述

[robert-haas.github.io](https://robert-haas.github.io/gravis-docs/?source=post_page-----e220d59e054e--------------------------------)

我个人在日常工作中经常使用图神经网络，坦率地说，我很遗憾之前没有早一点了解到这个包，因为这本可以节省我很多时间和精力，免去为绕过我在这里提到的包（`ipysigma` 和 `pyvis`）的不足之处而做的努力。
