# 大学橄榄球会议重组 — Python中的探索性数据分析

> 原文：[https://towardsdatascience.com/college-football-conference-realignment-exploratory-data-analysis-in-python-6f4a74037572?source=collection_archive---------7-----------------------#2023-08-05](https://towardsdatascience.com/college-football-conference-realignment-exploratory-data-analysis-in-python-6f4a74037572?source=collection_archive---------7-----------------------#2023-08-05)

[](https://medium.com/@gspmalloy?source=post_page-----6f4a74037572--------------------------------)[![乔瓦尼·马洛伊](../Images/e94218e244fab1af845c68e63e5706a1.png)](https://medium.com/@gspmalloy?source=post_page-----6f4a74037572--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6f4a74037572--------------------------------)[![数据科学的方向](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6f4a74037572--------------------------------) [乔瓦尼·马洛伊](https://medium.com/@gspmalloy?source=post_page-----6f4a74037572--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa0442a984e63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-football-conference-realignment-exploratory-data-analysis-in-python-6f4a74037572&user=Giovanni+Malloy&userId=a0442a984e63&source=post_page-a0442a984e63----6f4a74037572---------------------post_header-----------) 发表在 [数据科学的方向](https://towardsdatascience.com/?source=post_page-----6f4a74037572--------------------------------) · 8分钟阅读 · 2023年8月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6f4a74037572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-football-conference-realignment-exploratory-data-analysis-in-python-6f4a74037572&user=Giovanni+Malloy&userId=a0442a984e63&source=-----6f4a74037572---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6f4a74037572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcollege-football-conference-realignment-exploratory-data-analysis-in-python-6f4a74037572&source=-----6f4a74037572---------------------bookmark_footer-----------)

这是我最喜欢的季节：秋天，这意味着大学橄榄球的到来。我一直喜欢大学体育。在成长过程中，我生活在一个大十/东南会议家庭和一个大东（现在是ACC）小镇，这意味着从8月的第一次开球到4月的最后一声哨响，电视屏幕上充斥着大量的大学体育。最近，数据分析已主导了这两项运动，但既然现在是橄榄球季节，那我们就从这里开始吧。

![](../Images/4c16d025c892a07347b84b6945060bc7.png)

图片来源：[David Ireland](https://unsplash.com/@davidirelandmagnetic?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

过去两个赛季的大学体育界充满了NIL、转会门户和会议重组的新闻。我认为大多数球迷的情绪可以通过Dr. Pepper的“[混乱来到了Fansville](https://www.youtube.com/watch?v=Gq-Ye91fAm0)”广告来体现。我开始注意到，每次谈到会议重组时，特别是充满了猜测，并且由直觉驱动。然而，普遍的信念是某个强大的大学橄榄球奥兹正在 crunch 数字，以决定哪个队伍值得加入哪个会议。我仍然没有机会见到那位幕后的神秘人物，所以在此之前，我想尝试提出一种基于数据的会议重组方案。

这是一个四部分的博客，希望能作为一种有趣的方式来学习一些新的数据科学工具：

1.  [大学橄榄球会议重组 —](https://medium.com/towards-data-science/college-football-conference-realignment-exploratory-data-analysis-in-python-6f4a74037572)
