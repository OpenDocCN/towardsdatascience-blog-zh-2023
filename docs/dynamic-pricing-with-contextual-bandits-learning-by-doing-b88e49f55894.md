# **基于上下文的动态定价：通过实践学习**

> 原文：[`towardsdatascience.com/dynamic-pricing-with-contextual-bandits-learning-by-doing-b88e49f55894?source=collection_archive---------3-----------------------#2023-10-05`](https://towardsdatascience.com/dynamic-pricing-with-contextual-bandits-learning-by-doing-b88e49f55894?source=collection_archive---------3-----------------------#2023-10-05)

## 为你的动态定价问题增加上下文可以带来更多机会和挑战

[](https://medium.com/@massi.costacurta?source=post_page-----b88e49f55894--------------------------------)![Massimiliano Costacurta](https://medium.com/@massi.costacurta?source=post_page-----b88e49f55894--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b88e49f55894--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b88e49f55894--------------------------------) [Massimiliano Costacurta](https://medium.com/@massi.costacurta?source=post_page-----b88e49f55894--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F233cb43234c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-pricing-with-contextual-bandits-learning-by-doing-b88e49f55894&user=Massimiliano+Costacurta&userId=233cb43234c3&source=post_page-233cb43234c3----b88e49f55894---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b88e49f55894--------------------------------) ·17 min read·2023 年 10 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb88e49f55894&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-pricing-with-contextual-bandits-learning-by-doing-b88e49f55894&user=Massimiliano+Costacurta&userId=233cb43234c3&source=-----b88e49f55894---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb88e49f55894&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-pricing-with-contextual-bandits-learning-by-doing-b88e49f55894&source=-----b88e49f55894---------------------bookmark_footer-----------)![](img/6e3e13d9902455d3cb57bfb4f8ba38a1.png)

照片由 [Artem Beliaikin](https://unsplash.com/@belart84?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 从多臂老虎机到上下文老虎机

在我的[上一篇文章](https://medium.com/towards-data-science/dynamic-pricing-with-multi-armed-bandit-learning-by-doing-3e4550ed02ac)中，我对使用简单的多臂老虎机解决动态定价问题的最流行策略进行了详细分析。如果你是从那篇文章过来的，首先，感谢你。这绝不是一篇容易读的文章，我非常感谢你对这个主题的热情。其次，做好准备，这篇新文章可能会更加具有挑战性。然而，如果这是你对该主题的第一次接触，我强烈建议从上一篇文章开始阅读。在那里，我介绍了基础概念，我将在本讨论中假设读者已经熟悉这些概念。

总之，简要回顾：之前的分析旨在模拟一个动态定价情境。主要目标是尽快评估各种价格点，以找到带来最高累积奖励的价格点。我们探讨了四种不同的算法：贪婪算法、ε-贪婪算法、汤普森采样和 UCB1，详细描述了每种算法的优缺点。尽管文章中使用的方法在理论上是可靠的，但它存在一些在更复杂的现实世界中不适用的过度简化。
