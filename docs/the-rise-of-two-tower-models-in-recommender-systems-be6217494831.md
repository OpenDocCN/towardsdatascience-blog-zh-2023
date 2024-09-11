# 推荐系统中的双塔模型的崛起

> 原文：[`towardsdatascience.com/the-rise-of-two-tower-models-in-recommender-systems-be6217494831?source=collection_archive---------0-----------------------#2023-10-29`](https://towardsdatascience.com/the-rise-of-two-tower-models-in-recommender-systems-be6217494831?source=collection_archive---------0-----------------------#2023-10-29)

## 深入探讨用于消除排名模型偏差的最新技术

[](https://medium.com/@samuel.flender?source=post_page-----be6217494831--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----be6217494831--------------------------------)[](https://towardsdatascience.com/?source=post_page-----be6217494831--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----be6217494831--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----be6217494831--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-rise-of-two-tower-models-in-recommender-systems-be6217494831&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----be6217494831---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----be6217494831--------------------------------) ·7 min 阅读·2023 年 10 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbe6217494831&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-rise-of-two-tower-models-in-recommender-systems-be6217494831&user=Samuel+Flender&userId=ce56d9dcd568&source=-----be6217494831---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbe6217494831&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-rise-of-two-tower-models-in-recommender-systems-be6217494831&source=-----be6217494831---------------------bookmark_footer-----------)![](img/c2ea8cba8c6d13e4bf5926fa05096305.png)

照片由 [Evgeny Smirnov](https://unsplash.com/@smirik) 提供

推荐系统是当今世界上最普遍的机器学习应用之一。然而，基础的排名模型存在着 众多偏差，这些偏差可能严重限制推荐结果的质量。构建无偏排名器的问题——也被称为无偏学习排序（ULTR）——仍然是机器学习领域最重要的研究问题之一，且离解决还有很长的路要走。

在这篇文章中，我们将深入探讨一种相对较新的建模方法，这种方法使得行业能够非常有效地控制偏差，从而构建出远远优于以往的推荐系统：双塔模型，其中一个塔学习相关性，另一个（较浅的）塔学习偏差。

尽管双塔模型可能在行业中已经使用了几年，但首次将其正式引入更广泛的机器学习社区的论文是华为 2019 年的 PAL 论文。

## PAL（华为，2019 年）——原创双塔模型

华为的论文 [PAL](https://github.com/tangxyw/RecSysPapers/blob/main/Debias/%5B2019%5D%5BHuawei%5D%5BPAL%5D%20a%20position-bias%20aware%20learning%20framework%20for%20CTR%20prediction%20in%20live%20recommender%20systems.pdf)（“位置感知排序学习”）在华为应用商店的背景下考虑了位置偏差的问题。
