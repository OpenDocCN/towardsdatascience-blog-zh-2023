# 机器学习与优化算法的结合

> 原文：[https://towardsdatascience.com/a-marriage-of-machine-learning-and-optimization-algorithms-e6c680454f06?source=collection_archive---------1-----------------------#2023-12-02](https://towardsdatascience.com/a-marriage-of-machine-learning-and-optimization-algorithms-e6c680454f06?source=collection_archive---------1-----------------------#2023-12-02)

## 模式检测和模式利用如何可能将彼此提升到新的水平

[](https://wvheeswijk.medium.com/?source=post_page-----e6c680454f06--------------------------------)[![Wouter van Heeswijk, PhD](../Images/9c996bccd6fdfb6d9aa8b50b93338eb9.png)](https://wvheeswijk.medium.com/?source=post_page-----e6c680454f06--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e6c680454f06--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e6c680454f06--------------------------------) [Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----e6c680454f06--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F33f45c9ab481&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-marriage-of-machine-learning-and-optimization-algorithms-e6c680454f06&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=post_page-33f45c9ab481----e6c680454f06---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e6c680454f06--------------------------------) ·12 分钟阅读·2023年12月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe6c680454f06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-marriage-of-machine-learning-and-optimization-algorithms-e6c680454f06&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=-----e6c680454f06---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe6c680454f06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-marriage-of-machine-learning-and-optimization-algorithms-e6c680454f06&source=-----e6c680454f06---------------------bookmark_footer-----------)![](../Images/a9b9c8c0940005f58cb100e812a31d72.png)

我们应该考虑如何让优化算法和机器学习算法相互增强，而不是相互对比 [照片来源 [Wedding Dreamz](https://unsplash.com/@wedding_dreamz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)]

尽管我们大多数人看不到，优化算法（OAs）却在无处不在地发挥作用。它们为我们的杂货店规划货架陈列，制定机场时刻表，并为我们提供最短的假期路线。**特别是精确算法在利用已知结构方面表现非常出色**——例如，凸结构——即使在具有许多约束的大规模决策空间中也能找到解决方案。在过去几十年中，硬件和算法改进的结合带来了百万级的速度提升。在90年代可能需要几个月才能完成的计划任务，现在可能只需一秒钟。

类似地，机器学习（ML）在过去十年中取得了惊人的进展。MuZero 展现了在不知道游戏规则的情况下学习超人类游戏策略的能力，图神经网络学习了人眼无法感知的复杂关系，而变换器则催生了 ChatGPT 及其竞争对手。**这些算法的共同点是它们都能够从环境中检测模式**，无论是文本数据库还是视频游戏。新颖且高度复杂的架构不断被引入，通常…
