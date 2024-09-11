# 大学橄榄球联盟重新调整——聚类分析

> 原文：[`towardsdatascience.com/college-football-conference-realignment-clustering-6ca16840ed3d?source=collection_archive---------11-----------------------#2023-08-07`](https://towardsdatascience.com/college-football-conference-realignment-clustering-6ca16840ed3d?source=collection_archive---------11-----------------------#2023-08-07)

[](https://medium.com/@gspmalloy?source=post_page-----6ca16840ed3d--------------------------------)![Giovanni Malloy](https://medium.com/@gspmalloy?source=post_page-----6ca16840ed3d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ca16840ed3d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ca16840ed3d--------------------------------) [Giovanni Malloy](https://medium.com/@gspmalloy?source=post_page-----6ca16840ed3d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa0442a984e63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-football-conference-realignment-clustering-6ca16840ed3d&user=Giovanni+Malloy&userId=a0442a984e63&source=post_page-a0442a984e63----6ca16840ed3d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ca16840ed3d--------------------------------) ·12 分钟阅读·2023 年 8 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6ca16840ed3d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-football-conference-realignment-clustering-6ca16840ed3d&user=Giovanni+Malloy&userId=a0442a984e63&source=-----6ca16840ed3d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6ca16840ed3d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-football-conference-realignment-clustering-6ca16840ed3d&source=-----6ca16840ed3d---------------------bookmark_footer-----------)

欢迎来到这一系列关于联盟重新调整的第三部分！这篇博客文章中我们将开始使用数据集来指导重新调整决策。一个常见的抱怨是联盟重新调整摧毁了传统的对抗赛和大学橄榄球的区域性特点。确实，大学体育往往具有区域性。这甚至体现在联盟的名称中：太平洋 12、亚特兰大海岸、东南和大东联盟等。一些联盟在包括 FCS 时甚至更为具体：俄亥俄河谷联盟。当然，在 FBS 中，区域联盟的时代早已过去。最近几天，Pac 12 也似乎成为了过去的遗迹。

该系列分为四部分（完整动机见第一部分）：

1.  大学橄榄球联赛重新调整——Python 中的探索性数据分析

1.  大学橄榄球联赛重新调整——回归分析

1.  [大学橄榄球联赛重新调整——聚类分析](https://medium.com/p/6ca16840ed3d#0733-ef9637a21b53)

1.  [大学橄榄球联赛重新调整——node2vec](https://medium.com/towards-data-science/college-football-conference-realignment-node2vec-ba2e931bb1c)

![](img/7d2b1f585bc37cced342233b8418b0fd.png)

图片由[Gene Gallin](https://unsplash.com/@genefoto?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

希望该系列的每一部分都能为你提供对大学橄榄球这项受人喜爱的运动未来的全新视角。对于那些没有阅读第一部分或第二部分的读者，简要概述是我创建了自己的…
