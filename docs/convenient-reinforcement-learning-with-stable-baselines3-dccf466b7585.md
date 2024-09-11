# 使用 Stable-Baselines3 进行便捷的强化学习

> 原文：[`towardsdatascience.com/convenient-reinforcement-learning-with-stable-baselines3-dccf466b7585?source=collection_archive---------1-----------------------#2023-12-09`](https://towardsdatascience.com/convenient-reinforcement-learning-with-stable-baselines3-dccf466b7585?source=collection_archive---------1-----------------------#2023-12-09)

## [强化学习](https://medium.com/tag/reinforcement-learning)

## 无需繁琐代码的强化学习

[](https://dr-robert-kuebler.medium.com/?source=post_page-----dccf466b7585--------------------------------)![Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----dccf466b7585--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dccf466b7585--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dccf466b7585--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----dccf466b7585--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvenient-reinforcement-learning-with-stable-baselines3-dccf466b7585&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----dccf466b7585---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dccf466b7585--------------------------------) · 10 分钟阅读 · 2023 年 12 月 9 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdccf466b7585&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvenient-reinforcement-learning-with-stable-baselines3-dccf466b7585&source=-----dccf466b7585---------------------bookmark_footer-----------)![](img/b3a8b5fb8229e66fec0ddd0fbefcafc4.png)

由作者创建，使用**Leonardo Ai**。

在我之前关于强化学习的文章中，我向你展示了如何仅使用一些 numpy 和 TensorFlow 实现（深度）Q 学习。虽然这对于理解这些算法如何在内部工作是一个重要步骤，但代码往往会变得冗长——而且我仅仅实现了最基本版本的深度 Q 学习。

[](/hands-on-deep-q-learning-9073040ce841?source=post_page-----dccf466b7585--------------------------------) ## 实践深度 Q 学习

### 提升你的智能体，以赢得更困难的游戏！

[前往数据科学网站

根据本文的解释，理解代码应该相当直接。然而，如果我们*真的*想要完成任务，我们应该依赖于文档完善、维护良好且经过优化的库。正如我们不希望一遍遍实现线性回归一样，我们也不希望在强化学习中重复做同样的事。

在这篇文章中，我将向你展示一个名为[**Stable-Baselines3**](https://stable-baselines3.readthedocs.io/)的强化学习库，它的使用方式和 scikit-learn 一样简单。不过，我们训练的不是用于预测标签的模型，而是…
