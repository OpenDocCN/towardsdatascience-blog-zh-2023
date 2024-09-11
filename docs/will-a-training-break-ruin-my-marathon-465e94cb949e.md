# 训练中断会毁掉我的马拉松吗？

> 原文：[`towardsdatascience.com/will-a-training-break-ruin-my-marathon-465e94cb949e?source=collection_archive---------7-----------------------#2023-01-13`](https://towardsdatascience.com/will-a-training-break-ruin-my-marathon-465e94cb949e?source=collection_archive---------7-----------------------#2023-01-13)

## 关于马拉松准备期间训练中断的性能成本的大规模数据分析

[](https://barrysmyth.medium.com/?source=post_page-----465e94cb949e--------------------------------)![barrysmyth](https://barrysmyth.medium.com/?source=post_page-----465e94cb949e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----465e94cb949e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----465e94cb949e--------------------------------) [barrysmyth](https://barrysmyth.medium.com/?source=post_page-----465e94cb949e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa995c3b2ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwill-a-training-break-ruin-my-marathon-465e94cb949e&user=barrysmyth&userId=a995c3b2ae8&source=post_page-a995c3b2ae8----465e94cb949e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----465e94cb949e--------------------------------) ·10 分钟阅读·2023 年 1 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F465e94cb949e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwill-a-training-break-ruin-my-marathon-465e94cb949e&user=barrysmyth&userId=a995c3b2ae8&source=-----465e94cb949e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F465e94cb949e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwill-a-training-break-ruin-my-marathon-465e94cb949e&source=-----465e94cb949e---------------------bookmark_footer-----------)![](img/192e5cc4754093229cd06c9cb4422684.png)

[照片](https://www.pexels.com/photo/senior-man-with-beard-resting-peacefully-on-racetrack-5067713/)由[Anna Shvets](https://www.pexels.com/@shvetsa/)提供。

# TLDR;

+   超过 50%的跑者在马拉松训练时会经历至少 7 天的跑步中断。较长的训练中断（≥7 天）也很常见。但仍有希望，因为即使是长时间的中断也不一定会毁掉你的马拉松。

+   然而，中断确实会带来成本，训练中断与完成时间相比，通常会增加 2–9%的完成时间，这比没有长期训练中断时的完成时间要长。

+   完成时间的成本取决于干扰的性质（其持续时间和时机）以及跑步者的类型（男性与女性、年轻与年老、快速与慢速）。

+   通过了解这些成本，跑步者可以在训练中断后调整他们的马拉松目标，以确保更好的比赛结果。

# 介绍

训练马拉松并不容易。在很多方面，它比实际跑马拉松更具挑战性。确实，许多报名并开始训练马拉松的跑步者未能到达起跑线，更不用说完成比赛了。*非起跑者*的估算数量，占所有…
