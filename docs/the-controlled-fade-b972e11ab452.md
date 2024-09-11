# 控制性衰退

> 原文：[https://towardsdatascience.com/the-controlled-fade-b972e11ab452?source=collection_archive---------4-----------------------#2023-09-03](https://towardsdatascience.com/the-controlled-fade-b972e11ab452?source=collection_archive---------4-----------------------#2023-09-03)

## 为什么正分裂可能是你下次马拉松的最佳选择。

[](https://barrysmyth.medium.com/?source=post_page-----b972e11ab452--------------------------------)[![barrysmyth](../Images/b8a047ec4b651b9a476a3d430c5723f6.png)](https://barrysmyth.medium.com/?source=post_page-----b972e11ab452--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b972e11ab452--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b972e11ab452--------------------------------) [barrysmyth](https://barrysmyth.medium.com/?source=post_page-----b972e11ab452--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa995c3b2ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-controlled-fade-b972e11ab452&user=barrysmyth&userId=a995c3b2ae8&source=post_page-a995c3b2ae8----b972e11ab452---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b972e11ab452--------------------------------) ·13分钟阅读·2023年9月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb972e11ab452&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-controlled-fade-b972e11ab452&user=barrysmyth&userId=a995c3b2ae8&source=-----b972e11ab452---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb972e11ab452&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-controlled-fade-b972e11ab452&source=-----b972e11ab452---------------------bookmark_footer-----------)![](../Images/fb90211901eccd46826e93eaddbf4253.png)

图片由 [Miguel A Amutio](https://unsplash.com/@amutiomi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/QDv-uBc-poY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 休闲跑者是否应该遵循传统观念，即负分裂（第二部分更快）是跑马拉松的最佳方式？

# 关键成果

1.  马拉松跑者通常被建议在比赛中保持配速，以便后半程比前半程（稍微）*更快*——*负分裂*——因为*“*[*这就是精英如何获胜*](https://www.runnersworld.com/training/a20819476/what-world-records-teach-about-marathon-pacing/)*”*以及如何打破记录。然而，这项对超过[400万次马拉松](https://dx.plos.org/10.1371/journal.pone.0251513)的分析显示，几乎90%的比赛都以*较慢*的后半程（*正分裂*）结束。

1.  当我们控制跑者能力时，更均匀配速的比赛与更快的完赛时间和更多的个人最佳成绩相关。然而，最佳的休闲表现与轻微的正分裂相关，这表明[*受控衰退*](https://www.runnersworld.com/advanced/a38070581/why-you-shouldnt-negative-split/)是一种有效的休闲跑者配速策略。

1.  证据还指出，对休闲跑者而言存在*反对负分裂的表现偏差*——例如，10%的正分裂与更好的结果（更多PB，改善的相对完赛时间）相关，而不是10%的负分裂——因为休闲跑者通常在负分裂时配速…
