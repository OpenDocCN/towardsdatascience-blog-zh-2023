# 使用Python检测真实世界数据中的幂律

> 原文：[https://towardsdatascience.com/detecting-power-laws-in-real-world-data-with-python-b464190fade6?source=collection_archive---------3-----------------------#2023-11-24](https://towardsdatascience.com/detecting-power-laws-in-real-world-data-with-python-b464190fade6?source=collection_archive---------3-----------------------#2023-11-24)

## 用示例代码分解基于最大似然方法的方法

[](https://shawhin.medium.com/?source=post_page-----b464190fade6--------------------------------)[![Shaw Talebi](../Images/1449cc7c08890e2078f9e5d07897e3df.png)](https://shawhin.medium.com/?source=post_page-----b464190fade6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b464190fade6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b464190fade6--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----b464190fade6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetecting-power-laws-in-real-world-data-with-python-b464190fade6&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----b464190fade6---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b464190fade6--------------------------------) ·10 分钟阅读·Nov 24, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb464190fade6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetecting-power-laws-in-real-world-data-with-python-b464190fade6&user=Shaw+Talebi&userId=f3998e1cd186&source=-----b464190fade6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb464190fade6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetecting-power-laws-in-real-world-data-with-python-b464190fade6&source=-----b464190fade6---------------------bookmark_footer-----------)![](../Images/ba666f53ff8d436d975c6890cecc0147.png)

[Luke Chesser](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是关于幂律和胖尾的系列文章中的第二篇。在上一篇文章中，我给出了一个[适合初学者的介绍](https://medium.com/towards-data-science/pareto-power-laws-and-fat-tails-0355a187ee6a)，讲解了幂律，并提出了我们标准统计工具在分析这些问题时的三个问题。虽然提高意识可以帮助我们避免这些问题，但在实际应用中，某些数据遵循什么分布并不总是清楚。在本文中，我将描述如何从真实世界的数据中客观地检测幂律，并分享一个具体的社交媒体数据示例。

*注意：如果你不熟悉像幂律分布或胖尾分布这样的术语，请参考本系列的* [*第一篇文章*](https://medium.com/towards-data-science/pareto-power-laws-and-fat-tails-0355a187ee6a) *作为入门。*

# 幂律打破统计学101

在[上一篇文章](https://medium.com/towards-data-science/pareto-power-laws-and-fat-tails-0355a187ee6a)中，我们重点讨论了两种分布：高斯分布和幂律分布。我们看到这些分布具有完全相反的统计特性。也就是说，**幂律是由稀有事件驱动的**…
