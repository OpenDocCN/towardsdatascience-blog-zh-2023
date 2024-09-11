# 强化学习的基石——TD(λ) 与 3 个大名鼎鼎的人物

> 原文：[https://towardsdatascience.com/a-cornerstone-of-rl-td-%CE%BB-and-3-big-names-2e547b37c05?source=collection_archive---------8-----------------------#2023-09-14](https://towardsdatascience.com/a-cornerstone-of-rl-td-%CE%BB-and-3-big-names-2e547b37c05?source=collection_archive---------8-----------------------#2023-09-14)

## 如何从 TD(λ) 推导 Monte Carlo、SARSA 和 Q-learning

[](https://medium.com/@byjameskoh?source=post_page-----2e547b37c05--------------------------------)[![James Koh, PhD](../Images/8e7af8b567cdcf24805754801683b426.png)](https://medium.com/@byjameskoh?source=post_page-----2e547b37c05--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2e547b37c05--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2e547b37c05--------------------------------) [James Koh, PhD](https://medium.com/@byjameskoh?source=post_page-----2e547b37c05--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F780706b02d58&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-cornerstone-of-rl-td-%CE%BB-and-3-big-names-2e547b37c05&user=James+Koh%2C+PhD&userId=780706b02d58&source=post_page-780706b02d58----2e547b37c05---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2e547b37c05--------------------------------) · 13 分钟阅读 · 2023 年 9 月 14 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2e547b37c05&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-cornerstone-of-rl-td-%25CE%25BB-and-3-big-names-2e547b37c05&user=James+Koh%2C+PhD&userId=780706b02d58&source=-----2e547b37c05---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2e547b37c05&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-cornerstone-of-rl-td-%25CE%25BB-and-3-big-names-2e547b37c05&source=-----2e547b37c05---------------------bookmark_footer-----------)![](../Images/e98fb2371203cb541956d91ac9f47d66.png)

照片由 [Loïc Barré](https://unsplash.com/@loicbarre?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

基础知识是最重要的。在深入了解强化学习（RL）中的现代算法之前，掌握它们所基于的基础原则至关重要。

在强化学习（RL）领域，这意味着我们必须理解时间差分（TD）学习的概念，它可以推广到 TD(λ)。使用一个*单一*的代码库和几行代码，我将展示如何通过

1.  Monte Carlo，

1.  SARSA，

1.  Q-learning 和

1.  0 < λ < 1 的 TD(λ)。

结果以 gif 的形式呈现，使用了可以轻松重用的工具函数。作为预告，你将在本文结束时能够自己生成以下内容！

我们的代理（由一个笑脸 😃 表示）从蓝色网格开始，尝试到达黄色网格。红色网格会导致严重的负奖励并终止…
