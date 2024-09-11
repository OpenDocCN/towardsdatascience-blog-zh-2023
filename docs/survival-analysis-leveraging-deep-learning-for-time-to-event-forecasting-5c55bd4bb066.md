# 生存分析：利用深度学习进行时间预测（第二部分）

> 原文：[https://towardsdatascience.com/survival-analysis-leveraging-deep-learning-for-time-to-event-forecasting-5c55bd4bb066?source=collection_archive---------2-----------------------#2023-04-21](https://towardsdatascience.com/survival-analysis-leveraging-deep-learning-for-time-to-event-forecasting-5c55bd4bb066?source=collection_archive---------2-----------------------#2023-04-21)

![](../Images/b9fbc07c43c92a2032ad02c8e6952a78.png)

插图由作者提供

## 实际应用于再住院

[](https://linafaik.medium.com/?source=post_page-----5c55bd4bb066--------------------------------)[![Lina Faik](../Images/24a3aa67a2d9dc3e074ceead04ab4cc8.png)](https://linafaik.medium.com/?source=post_page-----5c55bd4bb066--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5c55bd4bb066--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5c55bd4bb066--------------------------------) [Lina Faik](https://linafaik.medium.com/?source=post_page-----5c55bd4bb066--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb6c0e8e98c84&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsurvival-analysis-leveraging-deep-learning-for-time-to-event-forecasting-5c55bd4bb066&user=Lina+Faik&userId=b6c0e8e98c84&source=post_page-b6c0e8e98c84----5c55bd4bb066---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5c55bd4bb066--------------------------------) ·10分钟阅读·2023年4月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5c55bd4bb066&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsurvival-analysis-leveraging-deep-learning-for-time-to-event-forecasting-5c55bd4bb066&user=Lina+Faik&userId=b6c0e8e98c84&source=-----5c55bd4bb066---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5c55bd4bb066&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsurvival-analysis-leveraging-deep-learning-for-time-to-event-forecasting-5c55bd4bb066&source=-----5c55bd4bb066---------------------bookmark_footer-----------)

生存模型非常适合预测事件发生的时间。这些模型可以应用于广泛的场景，包括预测性维护（预测机器可能出现故障的时间）、市场分析（预测客户流失）、患者监测（预测患者可能会重新住院）等。

通过将机器学习与生存模型相结合，所得模型可以利用前者的高预测能力，同时保留后者的框架和典型输出（例如，生存概率或随时间变化的风险曲线）。有关更多信息，请查看本系列的第一篇文章[这里](/survival-analysis-predict-time-to-event-with-machine-learning-part-i-ba52f9ab9a46)。

然而，在实践中，基于机器学习的生存模型仍然需要广泛的特征工程，因此需要先前的业务知识和直觉才能取得令人满意的结果。那么，为什么不使用深度学习模型来弥补这一差距呢？

## 目标

本文重点介绍如何将深度学习与生存分析框架相结合，以解决例如预测患者被（重新）住院的可能性等用例。
