# Bi-LSTM+Attention用于建模EHR数据

> 原文：[https://towardsdatascience.com/bi-lstm-attention-for-modeling-ehr-data-5dc7b05d4a10?source=collection_archive---------18-----------------------#2023-01-31](https://towardsdatascience.com/bi-lstm-attention-for-modeling-ehr-data-5dc7b05d4a10?source=collection_archive---------18-----------------------#2023-01-31)

## 基于注意力机制的Bi-LSTM网络在医疗保健领域的诊断预测指南

[](https://satyam-kumar.medium.com/?source=post_page-----5dc7b05d4a10--------------------------------)[![Satyam Kumar](../Images/2360baa87ea7a20f41589c5f8d783288.png)](https://satyam-kumar.medium.com/?source=post_page-----5dc7b05d4a10--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5dc7b05d4a10--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5dc7b05d4a10--------------------------------) [Satyam Kumar](https://satyam-kumar.medium.com/?source=post_page-----5dc7b05d4a10--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3d8bf96a415f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbi-lstm-attention-for-modeling-ehr-data-5dc7b05d4a10&user=Satyam+Kumar&userId=3d8bf96a415f&source=post_page-3d8bf96a415f----5dc7b05d4a10---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5dc7b05d4a10--------------------------------) · 7分钟阅读 · 2023年1月31日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5dc7b05d4a10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbi-lstm-attention-for-modeling-ehr-data-5dc7b05d4a10&user=Satyam+Kumar&userId=3d8bf96a415f&source=-----5dc7b05d4a10---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5dc7b05d4a10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbi-lstm-attention-for-modeling-ehr-data-5dc7b05d4a10&source=-----5dc7b05d4a10---------------------bookmark_footer-----------)![](../Images/1afe40d7a671e4ddf480d8041a9b0ffc.png)

图片由 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=6939537) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=6939537)

使用电子健康记录（EHR）预测未来的健康信息或疾病是医疗保健领域研究的关键用例。EHR数据包括诊断代码、药房代码和程序代码。由于数据的高维度，建模和解释EHR数据是一项繁琐的任务。

在本文中，我们将讨论一篇流行的研究论文，[DIPOLE](https://arxiv.org/abs/1706.05764)，该论文于2019年6月发布，使用了Bi-LSTM+Attention网络。

```py
**Contents:**
1) Limitations of Linear/Tree-based for Modeling
2) Why RNN models?
3) Essential guide to LSTM & Bi-LSTM network
4) Essential guide to Attention
5) Implementation
```

# 线性/树模型的局限性：

之前的实现是一个**随机森林模型**，使用一组固定的超参数来建模**汇总的成员级**索赔/药品/人口统计特征。

1.  在疾病预测的情况下，输出依赖于**随时间推移的事件序列**。这些时间序列信息在随机森林模型中会被**丢失**。因此，想法是尝试基于时间序列的事件预测。候选方法包括……
