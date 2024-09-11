# 像数据科学家一样解决神秘盒子的问题

> 原文：[`towardsdatascience.com/solve-a-mystery-box-like-a-data-scientist-f9ee9570ba52?source=collection_archive---------15-----------------------#2023-01-13`](https://towardsdatascience.com/solve-a-mystery-box-like-a-data-scientist-f9ee9570ba52?source=collection_archive---------15-----------------------#2023-01-13)

## 获取数据，训练 ViT，最小化问题；方法过于复杂

[](https://dennisbakhuis.medium.com/?source=post_page-----f9ee9570ba52--------------------------------)![Dennis Bakhuis](https://dennisbakhuis.medium.com/?source=post_page-----f9ee9570ba52--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f9ee9570ba52--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9ee9570ba52--------------------------------) [Dennis Bakhuis](https://dennisbakhuis.medium.com/?source=post_page-----f9ee9570ba52--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b8617eb89bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolve-a-mystery-box-like-a-data-scientist-f9ee9570ba52&user=Dennis+Bakhuis&userId=5b8617eb89bb&source=post_page-5b8617eb89bb----f9ee9570ba52---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9ee9570ba52--------------------------------) ·17 分钟阅读·2023 年 1 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff9ee9570ba52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolve-a-mystery-box-like-a-data-scientist-f9ee9570ba52&user=Dennis+Bakhuis&userId=5b8617eb89bb&source=-----f9ee9570ba52---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff9ee9570ba52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolve-a-mystery-box-like-a-data-scientist-f9ee9570ba52&source=-----f9ee9570ba52---------------------bookmark_footer-----------)![](img/b9a4946ac13a6d2ed63ff0370c95ba55.png)

图 1：一个神秘盒子、收集数据的过程，以及最终的一个打开的锁。

当一个数据科学家收到一个以盒子形式呈现的谜题时会发生什么？当然，他会（尝试）把它当作数据问题来解决。在这篇文章中，我将描述整个过程，老实说，这并不像我想象的那么简单。像许多问题一样，你可能会完全迷失，只有通过与几位朋友交流，我才重新回到了正轨。

作为一名数据科学家，我喜欢以数据的方式来处理这个问题。我意识到这种方法远不是最明显的解决方案。但这是一项非常有趣的尝试。收集大量数据，训练一个变换模型从视频中提取值，最终使用一个`minimizer`来找到解决方案。这篇文章是这个（大部分）有趣旅程的总结！

我将这篇文章分成了几个对我来说逻辑清晰的步骤。随意跳到你感兴趣的部分：

1.  你所说的“神秘盒子”是什么

1.  一个正式的问题描述

1.  收集所需的数据

1.  处理数据质量（标签、训练、推断）

1.  分析数据集并找到目标

1.  前往地点
