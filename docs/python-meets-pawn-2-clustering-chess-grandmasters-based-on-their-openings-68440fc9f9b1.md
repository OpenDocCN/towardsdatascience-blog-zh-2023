# Python 遇见 Pawn 2：基于开局对国际象棋棋大师进行聚类

> 原文：[https://towardsdatascience.com/python-meets-pawn-2-clustering-chess-grandmasters-based-on-their-openings-68440fc9f9b1?source=collection_archive---------13-----------------------#2023-12-22](https://towardsdatascience.com/python-meets-pawn-2-clustering-chess-grandmasters-based-on-their-openings-68440fc9f9b1?source=collection_archive---------13-----------------------#2023-12-22)

## 在这篇博客中，我将指导你如何使用 Python 分析国际象棋大师的开局。

[](https://mikayilahad.medium.com/?source=post_page-----68440fc9f9b1--------------------------------)[![Mikayil Ahadli](../Images/fdef02ad48534456e2bd67e6f22d7705.png)](https://mikayilahad.medium.com/?source=post_page-----68440fc9f9b1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----68440fc9f9b1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----68440fc9f9b1--------------------------------) [Mikayil Ahadli](https://mikayilahad.medium.com/?source=post_page-----68440fc9f9b1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe55caf00c472&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-meets-pawn-2-clustering-chess-grandmasters-based-on-their-openings-68440fc9f9b1&user=Mikayil+Ahadli&userId=e55caf00c472&source=post_page-e55caf00c472----68440fc9f9b1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----68440fc9f9b1--------------------------------) ·7 分钟阅读·2023 年 12 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F68440fc9f9b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-meets-pawn-2-clustering-chess-grandmasters-based-on-their-openings-68440fc9f9b1&user=Mikayil+Ahadli&userId=e55caf00c472&source=-----68440fc9f9b1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F68440fc9f9b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-meets-pawn-2-clustering-chess-grandmasters-based-on-their-openings-68440fc9f9b1&source=-----68440fc9f9b1---------------------bookmark_footer-----------)![](../Images/ad0563bab95de27dccaf51c4c6936779.png)

图片由 Midjourney 创建

+   [我将回答哪些问题](#7230)

+   [第 1 部分：获取数据](#1af5)

+   [第 2 部分：特征工程](#f93b)

+   [第 3 部分：聚类](#9291)

+   [结果和有趣的事实](#0426)

## 我将回答哪些问题

我对国际象棋的热情不是什么秘密，[在这里](https://medium.com/@mikayil.ahadli/python-meets-pawn-decoding-my-chess-openings-with-data-analysis-097a34cef20a)，我分享了自己对棋局开局的分析。但今天，我冒险进入一个新的领域：特级大师的世界。他们常用什么开局？他们的选择有多么多样？我对这些开局在不同特级大师中的分布感兴趣。顶级棋手是否偏好类似的开局？是否可以根据他们的偏好将他们分组？我不知道——让我们一探究竟！

## 第一部分：获取数据

国际象棋的一个很棒的方面是数据的可获取性。有很多来源，包括[pgnmentor](https://pgnmentor.com/files.html)，你可以在这里查看和下载关于开局和棋手的数据（免费）。这些数据每年更新几次，包括便携式棋局符号（PGN）格式的棋局，这是国际象棋中最流行的格式……
