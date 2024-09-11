# 如何使用 Python 识别数据中的异常值

> 原文：[https://towardsdatascience.com/how-to-identify-outliers-in-data-with-python-a9fe40235064?source=collection_archive---------13-----------------------#2023-05-12](https://towardsdatascience.com/how-to-identify-outliers-in-data-with-python-a9fe40235064?source=collection_archive---------13-----------------------#2023-05-12)

## *一篇探讨数据集异常值检测技术的文章。学习如何使用数据可视化、z 分数和聚类技术来识别数据集中的异常值*

[](https://medium.com/@theDrewDag?source=post_page-----a9fe40235064--------------------------------)[![Andrea D'Agostino](../Images/58c7c218815f25278aae59cea44d8771.png)](https://medium.com/@theDrewDag?source=post_page-----a9fe40235064--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a9fe40235064--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a9fe40235064--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----a9fe40235064--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-identify-outliers-in-data-with-python-a9fe40235064&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----a9fe40235064---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a9fe40235064--------------------------------) · 8分钟阅读 · 2023年5月12日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa9fe40235064&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-identify-outliers-in-data-with-python-a9fe40235064&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----a9fe40235064---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa9fe40235064&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-identify-outliers-in-data-with-python-a9fe40235064&source=-----a9fe40235064---------------------bookmark_footer-----------)![](../Images/c9cb3fd47a2778d757dee6bf94bdaf8c.png)

图片来源：[Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

纳西姆·塔勒布写到，“尾部”事件定义了世界上现象成功（或失败）的很大一部分。

> 每个人都知道预防胜于治疗，但很少有人对预防行为给予奖励。
> 
> N. Taleb — 《黑天鹅》

尾部事件是指那些发生概率在分布尾部的罕见事件，无论是在左侧还是右侧。

![](../Images/9d93fce8d5248c9bf0289e3d026f1df2.png)

[https://www.researchgate.net/figure/A-normal-distribution-curve-with-its-two-tails-Note-that-an-observed-result-is-likely-to_fig2_50196301](https://www.researchgate.net/figure/A-normal-distribution-curve-with-its-two-tails-Note-that-an-observed-result-is-likely-to_fig2_50196301)

根据塔勒布的说法，我们的生活主要关注最可能发生的事件，即最有可能发生的事情。这样做的**结果是，我们没有准备好应对那些可能发生的罕见事件**。

当罕见事件发生时（尤其是负面的），它们会让我们感到意外，而我们通常采取的常规措施往往无效。

想想我们在罕见事件发生时的行为，比如FTX的破产……
