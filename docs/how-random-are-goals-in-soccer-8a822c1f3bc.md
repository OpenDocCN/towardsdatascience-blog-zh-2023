# 足球进球有多随机？

> 原文：[https://towardsdatascience.com/how-random-are-goals-in-soccer-8a822c1f3bc?source=collection_archive---------2-----------------------#2023-07-29](https://towardsdatascience.com/how-random-are-goals-in-soccer-8a822c1f3bc?source=collection_archive---------2-----------------------#2023-07-29)

## 体育分析

## 通过频率统计了解进球事件

[](https://polmarin.medium.com/?source=post_page-----8a822c1f3bc--------------------------------)[![Pol Marin](../Images/a4f69a96717d453db9791f27b8f85e86.png)](https://polmarin.medium.com/?source=post_page-----8a822c1f3bc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8a822c1f3bc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8a822c1f3bc--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----8a822c1f3bc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-random-are-goals-in-soccer-8a822c1f3bc&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----8a822c1f3bc---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a822c1f3bc--------------------------------) ·7分钟阅读·2023年7月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8a822c1f3bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-random-are-goals-in-soccer-8a822c1f3bc&user=Pol+Marin&userId=1fa43cc443e7&source=-----8a822c1f3bc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8a822c1f3bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-random-are-goals-in-soccer-8a822c1f3bc&source=-----8a822c1f3bc---------------------bookmark_footer-----------)![](../Images/4592cd967691b0d4b57c5e91a050e62a.png)

足球进球 — 图片由 [Chaos Soccer Gear](https://unsplash.com/@chaossoccergear?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

足球（或美国读者所说的soccer）是一项了不起的运动。它不可能因为偶然而成为世界上最受欢迎的运动。

足球把人们聚集在一起，它是从忙碌生活中脱离的借口，因为比赛时间是乐趣时间。我们点一些快餐，一边吃着它，一边看梅西用球创造魔法——我们能够欣赏到他的表现真是太幸运了。而且我们还能观看像2010年的巴萨或2023年的曼城这样的惊人球队。

很多人会说没有比赛是相等的。这就是足球，没有什么能比得上它。但我认为这是错误的。

尽管它非常出色，但仍然被数学主导，就像其他一切一样。

生活充满了数学模型，足球也不例外。

我一生都是狂热的**巴萨**球迷。再加上我目前的职业状况，结果就是对**体育分析**产生了真正的兴趣——显然倾向于足球。

这篇文章是我关于体育分析的第一篇，所以我会保持相对简单。然而，我计划写更多文章，深入了解数学如何应用于足球（以及…
