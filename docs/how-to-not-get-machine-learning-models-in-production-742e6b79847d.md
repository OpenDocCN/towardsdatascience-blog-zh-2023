# 如何*避免*将机器学习模型投入生产

> 原文：[https://towardsdatascience.com/how-to-not-get-machine-learning-models-in-production-742e6b79847d?source=collection_archive---------3-----------------------#2023-07-08](https://towardsdatascience.com/how-to-not-get-machine-learning-models-in-production-742e6b79847d?source=collection_archive---------3-----------------------#2023-07-08)

## 一份讽刺性的指南，避免将生产流程靠近你的机器学习模型

[](https://medium.com/@ebbeberge?source=post_page-----742e6b79847d--------------------------------)[![Eirik Berge, PhD](../Images/7507374e75980fd0c1056af3cd299eaa.png)](https://medium.com/@ebbeberge?source=post_page-----742e6b79847d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----742e6b79847d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----742e6b79847d--------------------------------) [Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----742e6b79847d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7722f981eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-not-get-machine-learning-models-in-production-742e6b79847d&user=Eirik+Berge%2C+PhD&userId=7722f981eb&source=post_page-7722f981eb----742e6b79847d---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----742e6b79847d--------------------------------) ·8分钟阅读·2023年7月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F742e6b79847d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-not-get-machine-learning-models-in-production-742e6b79847d&user=Eirik+Berge%2C+PhD&userId=7722f981eb&source=-----742e6b79847d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F742e6b79847d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-not-get-machine-learning-models-in-production-742e6b79847d&source=-----742e6b79847d---------------------bookmark_footer-----------)![](../Images/0d63b46131824d9b39a8fc7793989034.png)

图片由[Unpad Street Feeding Animal Friend](https://unsplash.com/@unpadstreetfeeding?utm_source=medium&utm_medium=referral)拍摄，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 你旅程的概述

1.  [介绍 — 无生产，无问题！](#a6cb)

1.  [笔记本可以用于一切！](#b893)

1.  [为什么在有时间的时候还要自动化？](#8f4f)

1.  [测试？只要永远不犯错误！](#15c2)

1.  [我的头脑中的依赖管理！](#677b)

1.  [总结](#c801)

# 1 — 介绍 — 无生产，无问题！

作为数据科学家、数据工程师和机器学习工程师，我们被关于机器学习模型在生产中的信息轰炸。数百个视频和成千上万的博客都尽力帮助我们避免现在的境地。但无济于事。**现在，我痛心地说，全球各地都有机器学习模型在生产中**。它们为数百万不知情的人创造价值。在每一个街角。我们容忍这一切，只因为这已经很普遍。

当我去参加会议时，我会听到紧张的数据科学家在大观众面前谈论生产情况。从他们额头上的汗水和湿漉漉的手中可以看出，情况相当严重。这种情况已经持续了很久……
