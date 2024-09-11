# 使用 NetworkX 进行社交网络分析：初学者指南

> 原文：[`towardsdatascience.com/social-network-analysis-with-networkx-a-gentle-introduction-6123eddced3?source=collection_archive---------4-----------------------#2023-09-28`](https://towardsdatascience.com/social-network-analysis-with-networkx-a-gentle-introduction-6123eddced3?source=collection_archive---------4-----------------------#2023-09-28)

## 了解像 Facebook 和 LinkedIn 这样的公司如何从网络中提取见解

[](https://medium.com/@riccardo.andreoni?source=post_page-----6123eddced3--------------------------------)![Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----6123eddced3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6123eddced3--------------------------------)![数据科学进阶](https://towardsdatascience.com/?source=post_page-----6123eddced3--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----6123eddced3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76784541161c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsocial-network-analysis-with-networkx-a-gentle-introduction-6123eddced3&user=Riccardo+Andreoni&userId=76784541161c&source=post_page-76784541161c----6123eddced3---------------------post_header-----------) 发布于 [数据科学进阶](https://towardsdatascience.com/?source=post_page-----6123eddced3--------------------------------) ·8 分钟阅读·2023 年 9 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6123eddced3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsocial-network-analysis-with-networkx-a-gentle-introduction-6123eddced3&user=Riccardo+Andreoni&userId=76784541161c&source=-----6123eddced3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6123eddced3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsocial-network-analysis-with-networkx-a-gentle-introduction-6123eddced3&source=-----6123eddced3---------------------bookmark_footer-----------)![](img/43b579957937d01605dc5f2d6c4e31f1.png)

图片来源：[pixabay.com](https://pixabay.com/photos/play-stone-network-networked-1237457/)

流媒体平台如 Netflix 是如何 **推荐完美电影** 以供你晚上观看的？我们如何优化 **运动中的球员位置**，例如足球？我们如何 **识别金融网络中的欺诈活动**？我们如何 **预测约会应用中的配对**，如 Tinder？

所有这些挑战都有一个共同的解决方案： [**社交网络分析**](https://en.wikipedia.org/wiki/Social_network_analysis)。

社会网络分析（SNA）是一个强大的数据科学领域，它可以探索网络实体之间复杂的关系，发现隐藏的模式并捕捉洞察。网络实体可以是任何事物：它们可以是社交网络上的人，如 Instagram 上的用户，互联网上的网站，甚至是比赛网络中的运动队。

在接下来的章节中，我将帮助你理解数据科学中的网络是什么，我会描述最常见的类型，并且我会实现几个算法以从网络中提取有价值的信息。

所有理论部分将结合实际示例。你将会看到几个**Python 代码**片段，以便你熟悉[**NetworkX**](https://networkx.org/) Python 库。

# 理解…
