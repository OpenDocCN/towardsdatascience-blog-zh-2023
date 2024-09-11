# PPO 带裁剪的工作原理是什么？

> 原文：[`towardsdatascience.com/how-does-ppo-with-clipping-work-eff71a7a974a?source=collection_archive---------8-----------------------#2023-10-07`](https://towardsdatascience.com/how-does-ppo-with-clipping-work-eff71a7a974a?source=collection_archive---------8-----------------------#2023-10-07)

## 直觉 + 数学 + 代码，为从业者

[](https://medium.com/@byjameskoh?source=post_page-----eff71a7a974a--------------------------------)![James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----eff71a7a974a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eff71a7a974a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eff71a7a974a--------------------------------) [James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----eff71a7a974a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F780706b02d58&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-ppo-with-clipping-work-eff71a7a974a&user=James+Koh%2C+PhD&userId=780706b02d58&source=post_page-780706b02d58----eff71a7a974a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eff71a7a974a--------------------------------) · 9 分钟阅读 · 2023 年 10 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feff71a7a974a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-ppo-with-clipping-work-eff71a7a974a&user=James+Koh%2C+PhD&userId=780706b02d58&source=-----eff71a7a974a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feff71a7a974a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-does-ppo-with-clipping-work-eff71a7a974a&source=-----eff71a7a974a---------------------bookmark_footer-----------)![](img/847c27062f8d23472ca40f8a61a45a03.png)

图片由 [Tamanna Rumee](https://unsplash.com/@tamanna_rumee?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在强化学习中，**近端策略优化（PPO）**常被作为策略方法的例子，与 DQN（基于价值的方法）和包括 TD3 和 SAC 在内的演员-评论家方法大家族相比。

我回忆起不久前，当我第一次学习它时，我并不信服。许多教师采用了一种敷衍的方式。我不认同这种方法，你也不应如此。

在这篇文章中，我将尝试解释 PPO 的工作原理，结合直观和代码来支持数学原理。你可以尝试不同的场景，亲自验证它不仅在理论上有效，也在实践中有效，并且没有选择性偏差。

# 为什么要费心？

PPO 和其他 SOTA 模型可以使用 stable-baselines3（sb3）在几分钟内实现。任何遵循文档的人都可以运行它，而无需了解底层模型。

然而，无论你是从业者还是理论家，基础知识都是重要的。如果你只是把 PPO（或任何模型）当作一个黑箱，你怎么能期望你的用户对你提供的结果有信心呢？

我将在本月稍晚时进行详细的代码演示，编写一个包装器，以便任何人……
