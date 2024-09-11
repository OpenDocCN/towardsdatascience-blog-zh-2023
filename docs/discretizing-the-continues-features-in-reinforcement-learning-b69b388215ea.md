# 离散化强化学习中的连续特征

> 原文：[`towardsdatascience.com/discretizing-the-continues-features-in-reinforcement-learning-b69b388215ea?source=collection_archive---------17-----------------------#2023-03-13`](https://towardsdatascience.com/discretizing-the-continues-features-in-reinforcement-learning-b69b388215ea?source=collection_archive---------17-----------------------#2023-03-13)

## 如何使用平铺编码和 Python 将无限变量转换为离散空间

[](https://eligijus-bujokas.medium.com/?source=post_page-----b69b388215ea--------------------------------)![Eligijus Bujokas](https://eligijus-bujokas.medium.com/?source=post_page-----b69b388215ea--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b69b388215ea--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b69b388215ea--------------------------------) [Eligijus Bujokas](https://eligijus-bujokas.medium.com/?source=post_page-----b69b388215ea--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd61597e07b4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdiscretizing-the-continues-features-in-reinforcement-learning-b69b388215ea&user=Eligijus+Bujokas&userId=d61597e07b4d&source=post_page-d61597e07b4d----b69b388215ea---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b69b388215ea--------------------------------) ·6 min read·2023 年 3 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb69b388215ea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdiscretizing-the-continues-features-in-reinforcement-learning-b69b388215ea&user=Eligijus+Bujokas&userId=d61597e07b4d&source=-----b69b388215ea---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb69b388215ea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdiscretizing-the-continues-features-in-reinforcement-learning-b69b388215ea&source=-----b69b388215ea---------------------bookmark_footer-----------)![](img/bdde8e9b4d25e844f699ec37d46e776f.png)

照片由 [Ehud Neuhaus](https://unsplash.com/@paramir?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文是强化学习系列的延续。回顾以前的文章，请访问这些链接：

[](/first-steps-in-the-world-of-reinforcement-learning-using-python-b843b76538e3?source=post_page-----b69b388215ea--------------------------------) [## 使用 Python 进行强化学习的第一步

### 介绍了如何在强化学习的基本世界之一中找到最佳位置的原始 Python 实现……

[Python 中的时间差异——基于样本的强化学习算法的第一次实现](https://towardsdatascience.com/temporal-differences-with-python-first-sample-based-reinforcement-learning-algorithm-54c11745a0ee?source=post_page-----b69b388215ea--------------------------------) 

### 使用 Python 编写和理解 TD(0)算法

[使用 Q 学习进行强化学习中的行动价值](https://towardsdatascience.com/the-values-of-actions-in-reinforcement-learning-using-q-learning-cb4b03be5c81?source=post_page-----b69b388215ea--------------------------------) 

### 用 Python 从头实现的 Q-learning 算法

[使用 Q 学习进行强化学习中的行动价值](https://towardsdatascience.com/the-values-of-actions-in-reinforcement-learning-using-q-learning-cb4b03be5c81?source=post_page-----b69b388215ea--------------------------------)

上一篇关于 Q 学习的文章探讨了将数字分配给状态-动作对的概念：

使用的状态是可以列出并写入表格的状态。例如，我们对迷宫中所有可用的位置进行了索引，我们的代理可以处于这些位置中。即使在……
