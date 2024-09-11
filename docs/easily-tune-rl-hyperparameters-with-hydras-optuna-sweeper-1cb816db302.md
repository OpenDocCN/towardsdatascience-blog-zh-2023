# 使用 Hydra 的 Optuna 扫描器调优 RL 超参数

> 原文：[`towardsdatascience.com/easily-tune-rl-hyperparameters-with-hydras-optuna-sweeper-1cb816db302?source=collection_archive---------12-----------------------#2023-02-01`](https://towardsdatascience.com/easily-tune-rl-hyperparameters-with-hydras-optuna-sweeper-1cb816db302?source=collection_archive---------12-----------------------#2023-02-01)

## 使用 Hydra 和 Optuna 轻松配置你的 Stable-Baselines3 调优管道

[](https://marc-velay.medium.com/?source=post_page-----1cb816db302--------------------------------)![Marc Velay](https://marc-velay.medium.com/?source=post_page-----1cb816db302--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1cb816db302--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cb816db302--------------------------------) [Marc Velay](https://marc-velay.medium.com/?source=post_page-----1cb816db302--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F60f512a0b95a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasily-tune-rl-hyperparameters-with-hydras-optuna-sweeper-1cb816db302&user=Marc+Velay&userId=60f512a0b95a&source=post_page-60f512a0b95a----1cb816db302---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cb816db302--------------------------------) · 8 分钟阅读 · 2023 年 2 月 1 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1cb816db302&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasily-tune-rl-hyperparameters-with-hydras-optuna-sweeper-1cb816db302&user=Marc+Velay&userId=60f512a0b95a&source=-----1cb816db302---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1cb816db302&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasily-tune-rl-hyperparameters-with-hydras-optuna-sweeper-1cb816db302&source=-----1cb816db302---------------------bookmark_footer-----------)

强化学习 (RL) 代理对其超参数非常敏感。相同的代理在训练后可能会从完全无用变为排行榜的顶端，只需正确的超参数！然而，没有合适的工具，找到正确的组合可能非常耗时甚至徒劳。本文的主要目标是通过一些理论和应用与您分享*如何*调优 RL 超参数。

你可以在我的 [GitHub](https://github.com/Marc-Velay/hydra_optuna_tutorial) 上找到这个项目的代码：快来 fork 吧！

![](img/287f7595e51b7ca3130686d9eb26f4db.png)

图片由[Leonel Fernandez](https://unsplash.com/@leonelfdez?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

强化学习是一种机器学习类型，涉及训练智能体与环境互动。智能体进行一系列动作，遵循策略来决定他们在每种情况下的反应。他们的目标是通过与环境的互动来最大化他们获得的奖励，并通过经验逐渐学习最佳的动作。在[之前的文章](https://velaylearning.com/markov-decision-process-in-plain-english/)中，我对强化学习的概念进行了更深入的探讨。

强化学习（RL）算法依赖于许多超参数，有些用于学习，有些用于底层的神经网络。有许多杠杆可以使学习变得更加稳定、快速……
