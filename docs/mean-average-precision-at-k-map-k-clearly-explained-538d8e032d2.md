# K@K的平均准确率（MAP@K）详解

> 原文：[https://towardsdatascience.com/mean-average-precision-at-k-map-k-clearly-explained-538d8e032d2?source=collection_archive---------1-----------------------#2023-01-18](https://towardsdatascience.com/mean-average-precision-at-k-map-k-clearly-explained-538d8e032d2?source=collection_archive---------1-----------------------#2023-01-18)

## 一步一步解释推荐或排序问题的最流行评估指标之一

[](https://konstantin-rink.medium.com/?source=post_page-----538d8e032d2--------------------------------)[![Konstantin Rink](../Images/41bfc069d7382a0fd56f081eea7eb2d9.png)](https://konstantin-rink.medium.com/?source=post_page-----538d8e032d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----538d8e032d2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----538d8e032d2--------------------------------) [Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----538d8e032d2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F337427fde9f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmean-average-precision-at-k-map-k-clearly-explained-538d8e032d2&user=Konstantin+Rink&userId=337427fde9f0&source=post_page-337427fde9f0----538d8e032d2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----538d8e032d2--------------------------------) ·7分钟阅读·2023年1月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F538d8e032d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmean-average-precision-at-k-map-k-clearly-explained-538d8e032d2&user=Konstantin+Rink&userId=337427fde9f0&source=-----538d8e032d2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F538d8e032d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmean-average-precision-at-k-map-k-clearly-explained-538d8e032d2&source=-----538d8e032d2---------------------bookmark_footer-----------)![](../Images/f8af88de2aaea97d6425f70111af801f.png)

图片由 [Joshua Burdick](https://unsplash.com/de/@gadgetpastor) 提供，来源于 [Unsplash](https://unsplash.com/de/Fotos/0xpFNUjzdbw)。

K@K的平均准确率（MAP@K）是推荐系统和其他**排序**相关分类任务中最常用的评估指标之一。由于该指标是不同误差指标或层次的组合，一开始可能不容易理解。

本文逐步解释了 MAP@K 及其组件。文章末尾还会提供**代码片段**，展示如何计算该度量。但在深入每个部分之前，我们先来谈谈**为什么**。

# 为什么使用 MAP@K？

MAP@K 是一种误差度量，可以在你的推荐项的**序列**或**排名**对你的任务**起着重要作用或是任务目标**时使用。通过使用它，你可以得到以下问题的答案：

+   我的生成或预测的推荐是否**相关**？

+   **最相关**的推荐是否在**前几位**？

# 使以下步骤更易于理解
