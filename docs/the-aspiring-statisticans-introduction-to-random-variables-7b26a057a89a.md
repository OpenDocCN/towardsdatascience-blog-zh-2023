# *有抱负的统计学家的随机变量入门*

> 原文：[`towardsdatascience.com/the-aspiring-statisticans-introduction-to-random-variables-7b26a057a89a?source=collection_archive---------7-----------------------#2023-05-12`](https://towardsdatascience.com/the-aspiring-statisticans-introduction-to-random-variables-7b26a057a89a?source=collection_archive---------7-----------------------#2023-05-12)

![](img/dcb6c0069cd80d228923f82aebc9de2f.png)

[Pixabay](https://pixabay.com/photos/candy-sweets-sugar-confectionery-1924595/) ([许可证](https://pixabay.com/service/terms/))

## 当谈到大喊“惊喜！”时，宇宙从不感到厌倦。

[](https://timeseriesreasoning.medium.com/?source=post_page-----7b26a057a89a--------------------------------)![Sachin Date](https://timeseriesreasoning.medium.com/?source=post_page-----7b26a057a89a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7b26a057a89a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7b26a057a89a--------------------------------) [Sachin Date](https://timeseriesreasoning.medium.com/?source=post_page-----7b26a057a89a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb75b5b1730f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-aspiring-statisticans-introduction-to-random-variables-7b26a057a89a&user=Sachin+Date&userId=b75b5b1730f3&source=post_page-b75b5b1730f3----7b26a057a89a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7b26a057a89a--------------------------------) ·31 分钟阅读·2023 年 5 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7b26a057a89a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-aspiring-statisticans-introduction-to-random-variables-7b26a057a89a&user=Sachin+Date&userId=b75b5b1730f3&source=-----7b26a057a89a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7b26a057a89a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-aspiring-statisticans-introduction-to-random-variables-7b26a057a89a&source=-----7b26a057a89a---------------------bookmark_footer-----------)

在本文中，我们将对概率的领域进行深思熟虑的探索。我们将了解**随机游走**、**离散**和**连续随机变量**，以及它们的**概率分布**。我们将理解为什么给连续随机变量的值分配一个概率是没有意义的。通过这些，我们将揭示**概率密度**的含义。我们将学习**随机过程**是如何形成的，以及它们与**时间序列模型**的关系。我们将以简短的讨论结束我们的游历，探讨随机变量如何构成所有回归模型的基础。

# 关于实验、结果和样本空间

我们经历变化——不可预测、随机的变化，缺乏任何可观察的模式或理由。它让我们惊讶，它让我们沮丧，它打乱了我们最精心制定的计划。

我们睡过了早晨的闹钟，我们被突如其来的交通堵塞困住，我们迟到参加会议。结果发现会议被推迟了，因为召集会议的人也被困在了交通中。

但随机性并不总是敌人。还记得当心不在焉的咖啡师多倒了几毫米咖啡时的情景吗……
