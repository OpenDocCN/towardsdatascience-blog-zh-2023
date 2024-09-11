# 从数据科学的角度分析 FC 巴萨的防守

> 原文：[`towardsdatascience.com/analyzing-fc-barcelonas-defense-from-a-data-science-perspective-76797018b0b3?source=collection_archive---------9-----------------------#2023-08-10`](https://towardsdatascience.com/analyzing-fc-barcelonas-defense-from-a-data-science-perspective-76797018b0b3?source=collection_archive---------9-----------------------#2023-08-10)

## 体育分析

## 通过视觉数据对比来说明巴萨防守的缺陷

[](https://polmarin.medium.com/?source=post_page-----76797018b0b3--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----76797018b0b3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----76797018b0b3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----76797018b0b3--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----76797018b0b3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-fc-barcelonas-defense-from-a-data-science-perspective-76797018b0b3&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----76797018b0b3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----76797018b0b3--------------------------------) ·13 分钟阅读·2023 年 8 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F76797018b0b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-fc-barcelonas-defense-from-a-data-science-perspective-76797018b0b3&user=Pol+Marin&userId=1fa43cc443e7&source=-----76797018b0b3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F76797018b0b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyzing-fc-barcelonas-defense-from-a-data-science-perspective-76797018b0b3&source=-----76797018b0b3---------------------bookmark_footer-----------)![](img/41dccd6550cdb8ac77bbb2c55df7110e.png)

图片由 [Alessio Patron](https://unsplash.com/@alessiop?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 引言

几天前我发布了第一篇体育分析文章。对这个话题仍然充满兴趣，这里我再次写关于足球的内容。

在那篇文章中 — 链接见下方 — 我使用了频率统计方法来展示进球事件的随机性。但我进一步探讨了这个问题。文中解释的随机模型 — 受泊松分布影响 — 适用于许多与足球无关的其他领域。

[](/how-random-are-goals-in-soccer-8a822c1f3bc?source=post_page-----76797018b0b3--------------------------------) ## 足球中的进球有多随机？

### 通过频率统计理解进球事件

[towardsdatascience.com

今天我们将更进一步，虽然过程将以足球为中心，但我们将探讨的过程和知识对任何数据科学家都具有相关性。

在足球方面，我们将重点关注防守，分析巴萨的表现，看看在哪里可以做得更好，无论是团队层面还是个人层面。

防守是一个广泛的术语——它包括铲球、扑救、拦截等许多方面……
