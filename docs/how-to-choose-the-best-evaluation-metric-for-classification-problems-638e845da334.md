# 如何选择最佳的分类问题评估指标

> 原文：[https://towardsdatascience.com/how-to-choose-the-best-evaluation-metric-for-classification-problems-638e845da334?source=collection_archive---------5-----------------------#2023-04-17](https://towardsdatascience.com/how-to-choose-the-best-evaluation-metric-for-classification-problems-638e845da334?source=collection_archive---------5-----------------------#2023-04-17)

## 一份涵盖最常用的监督分类评估指标及其在不同场景下的实用性的综合指南

[![Thomas A Dorfer](../Images/9258a1735cee805f1d9b02e2adf01096.png)](https://thomasdorfer.medium.com/?source=post_page-----638e845da334--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----638e845da334--------------------------------) [Thomas A Dorfer](https://thomasdorfer.medium.com/?source=post_page-----638e845da334--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c54f9b62b90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-choose-the-best-evaluation-metric-for-classification-problems-638e845da334&user=Thomas+A+Dorfer&userId=7c54f9b62b90&source=post_page-7c54f9b62b90----638e845da334---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----638e845da334--------------------------------) ·9分钟阅读·2023年4月17日

--

![](../Images/c6a92a3e2d07ecf632a0435a02146a6d.png)

作者提供的图片。

为了正确评估分类模型，重要的是仔细考虑哪个评估指标最为合适。

本文将涵盖分类任务中最常用的评估指标，包括相关的示例案例，并提供必要的信息帮助你在它们之间做出选择。

# 分类

分类问题的特点在于基于相应特征预测给定观测的类别或类别。选择最合适的评估指标将取决于用户希望优化的模型性能方面。

想象一个旨在诊断特定疾病的预测模型。如果该模型未能检测到疾病，可能导致严重后果，如延迟治疗和患者受害。另一方面，如果模型错误地诊断了健康患者，这也可能通过让健康患者接受不必要的检测而造成昂贵后果…
