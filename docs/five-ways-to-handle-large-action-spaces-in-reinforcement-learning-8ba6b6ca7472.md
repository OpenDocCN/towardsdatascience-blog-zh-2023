# 处理大型行动空间的五种方法

> 原文：[`towardsdatascience.com/five-ways-to-handle-large-action-spaces-in-reinforcement-learning-8ba6b6ca7472?source=collection_archive---------2-----------------------#2023-08-18`](https://towardsdatascience.com/five-ways-to-handle-large-action-spaces-in-reinforcement-learning-8ba6b6ca7472?source=collection_archive---------2-----------------------#2023-08-18)

## 行动空间，尤其是在组合优化问题中，可能会变得异常庞大。本文讨论了处理这些问题的五种策略。

[](https://wvheeswijk.medium.com/?source=post_page-----8ba6b6ca7472--------------------------------)![Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----8ba6b6ca7472--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8ba6b6ca7472--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ba6b6ca7472--------------------------------) [Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----8ba6b6ca7472--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F33f45c9ab481&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffive-ways-to-handle-large-action-spaces-in-reinforcement-learning-8ba6b6ca7472&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=post_page-33f45c9ab481----8ba6b6ca7472---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ba6b6ca7472--------------------------------) ·14 分钟阅读·2023 年 8 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8ba6b6ca7472&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffive-ways-to-handle-large-action-spaces-in-reinforcement-learning-8ba6b6ca7472&user=Wouter+van+Heeswijk%2C+PhD&userId=33f45c9ab481&source=-----8ba6b6ca7472---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8ba6b6ca7472&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffive-ways-to-handle-large-action-spaces-in-reinforcement-learning-8ba6b6ca7472&source=-----8ba6b6ca7472---------------------bookmark_footer-----------)![](img/faed2a6acffe7d9d787734e7815479db.png)

开始吧！[照片由 [Jakob Owens](https://unsplash.com/@jakobowens1?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)]

在强化学习中，处理大规模动作空间仍然是一个相当开放的问题。研究人员在处理大规模*状态*空间方面取得了巨大进展，卷积网络和变压器是一些近期的高调例子。然而，有三个所谓的**维度诅咒**：状态、结果和动作[1]。到目前为止，后者仍然研究不足。

尽管如此，仍然有越来越多的方法尝试处理大规模动作空间。本文介绍了五种在规模上处理后者的方法，特别关注**高维离散动作空间**，这些空间常在组合优化问题中遇到。

# 回顾：三种维度诅咒

有必要对三种维度诅咒做一个简要回顾。假设我们将手头的问题表示为[贝尔曼方程](https://medium.com/towards-data-science/why-reinforcement-learning-doesnt-need-bellman-s-equation-c9c2e51a0b7)系统，请注意有**三组需要评估**—在实践中表现为嵌套循环—每一组可能都过于庞大。
