# 使用 Python 的时间差异：第一个基于样本的强化学习算法

> 原文：[https://towardsdatascience.com/temporal-differences-with-python-first-sample-based-reinforcement-learning-algorithm-54c11745a0ee?source=collection_archive---------4-----------------------#2023-01-27](https://towardsdatascience.com/temporal-differences-with-python-first-sample-based-reinforcement-learning-algorithm-54c11745a0ee?source=collection_archive---------4-----------------------#2023-01-27)

## 使用 Python 编写和理解 TD(0) 算法

[](https://eligijus-bujokas.medium.com/?source=post_page-----54c11745a0ee--------------------------------)[![Eligijus Bujokas](../Images/061fd30136caea2ba927140e8b3fae3c.png)](https://eligijus-bujokas.medium.com/?source=post_page-----54c11745a0ee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54c11745a0ee--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----54c11745a0ee--------------------------------) [Eligijus Bujokas](https://eligijus-bujokas.medium.com/?source=post_page-----54c11745a0ee--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd61597e07b4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftemporal-differences-with-python-first-sample-based-reinforcement-learning-algorithm-54c11745a0ee&user=Eligijus+Bujokas&userId=d61597e07b4d&source=post_page-d61597e07b4d----54c11745a0ee---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----54c11745a0ee--------------------------------) ·13 分钟阅读·2023年1月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F54c11745a0ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftemporal-differences-with-python-first-sample-based-reinforcement-learning-algorithm-54c11745a0ee&user=Eligijus+Bujokas&userId=d61597e07b4d&source=-----54c11745a0ee---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F54c11745a0ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftemporal-differences-with-python-first-sample-based-reinforcement-learning-algorithm-54c11745a0ee&source=-----54c11745a0ee---------------------bookmark_footer-----------)![](../Images/36f274e9692bf335a901977271968109.png)

图片由 [Kurt Cotoaga](https://unsplash.com/@kydroon?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是我之前文章的续篇：

[](/first-steps-in-the-world-of-reinforcement-learning-using-python-b843b76538e3?source=post_page-----54c11745a0ee--------------------------------) [## 使用 Python 进行强化学习的第一步

### 原始 Python 实现了如何在强化学习的基本世界之一中找到最佳去处……

[towardsdatascience.com](/first-steps-in-the-world-of-reinforcement-learning-using-python-b843b76538e3?source=post_page-----54c11745a0ee--------------------------------)

在这篇文章中，我想让读者熟悉强化学习（**RL**）中的基于样本的算法逻辑。为此，我们将创建一个有孔的网格世界（类似于缩略图中的世界），并让我们的代理自由地在这个世界中游历。

希望在代理完成旅程时，它能学会在世界中哪些地方是好去处，哪些地方应该避免。为了帮助我们的代理进行学习，我们将使用著名的**TD(0)**算法。

在深入了解算法之前，让我们定义一下我们想要解决的目标。

在这篇文章中，我们将创建一个有5行7列的网格世界，这意味着我们的代理将能够处于35个状态中的一个。移动规则是：

+   代理不能离开……
