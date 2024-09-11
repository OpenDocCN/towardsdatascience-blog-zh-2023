# 无监督学习中的关联规则挖掘

> 原文：[https://towardsdatascience.com/association-rule-mining-in-unsupervised-learning-df86170160de?source=collection_archive---------11-----------------------#2023-01-25](https://towardsdatascience.com/association-rule-mining-in-unsupervised-learning-df86170160de?source=collection_archive---------11-----------------------#2023-01-25)

## 数据挖掘中的模式发现术语和概念

[](https://kayjanwong.medium.com/?source=post_page-----df86170160de--------------------------------)[![Kay Jan Wong](../Images/28e803eca6327d97b6aa97ee4095d7bd.png)](https://kayjanwong.medium.com/?source=post_page-----df86170160de--------------------------------)[](https://towardsdatascience.com/?source=post_page-----df86170160de--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----df86170160de--------------------------------) [Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----df86170160de--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffee8693930fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fassociation-rule-mining-in-unsupervised-learning-df86170160de&user=Kay+Jan+Wong&userId=fee8693930fb&source=post_page-fee8693930fb----df86170160de---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----df86170160de--------------------------------) ·5分钟阅读·2023年1月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdf86170160de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fassociation-rule-mining-in-unsupervised-learning-df86170160de&user=Kay+Jan+Wong&userId=fee8693930fb&source=-----df86170160de---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdf86170160de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fassociation-rule-mining-in-unsupervised-learning-df86170160de&source=-----df86170160de---------------------bookmark_footer-----------)![](../Images/a6785c84b324d9b94cdc3e0fc0f84db0.png)

由 [Kier... in Sight](https://unsplash.com/@kierinsight?utm_source=medium&utm_medium=referral) 摄影，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

模式发现尝试在大规模数据集中揭示模式，这为许多数据挖掘任务奠定了基础，如关联、相关性和因果分析、聚类分析等。

本文介绍了关联规则挖掘中的常见术语，接着介绍了频繁模式和序列模式的关联规则挖掘技术。

# 目录

+   [术语：支持度、置信度、提升度、杠杆度、确信度](https://medium.com/p/df86170160de/#2842)

## **频繁模式**

+   [Apriori 算法](https://medium.com/p/df86170160de/#035c)

+   [等效类变换（ECLAT）](https://medium.com/p/df86170160de/#5f42)

+   [频繁模式挖掘（FP-Growth）](https://medium.com/p/df86170160de/#a37a)

## 序列模式

+   [广义序列模式（GSP）](https://medium.com/p/df86170160de/#e8ad)

+   [前缀投影序列模式挖掘（PrefixSpan）](https://medium.com/p/df86170160de/#4226)

# 术语

+   **支持度**：项`X`出现的概率，表示为`P(X)`，衡量该项的受欢迎程度

+   **置信度**：获取项`X`的条件概率在获取项`Y`之后，表示为`P(X|Y)`
