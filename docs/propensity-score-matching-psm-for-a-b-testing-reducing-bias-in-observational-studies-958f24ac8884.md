# 用于A/B测试的倾向得分匹配（PSM）：减少观察研究中的偏差

> 原文：[https://towardsdatascience.com/propensity-score-matching-psm-for-a-b-testing-reducing-bias-in-observational-studies-958f24ac8884?source=collection_archive---------10-----------------------#2023-04-26](https://towardsdatascience.com/propensity-score-matching-psm-for-a-b-testing-reducing-bias-in-observational-studies-958f24ac8884?source=collection_archive---------10-----------------------#2023-04-26)

## 这是一个全面的指南，介绍如何使用你的实验数据实施PSM，包括Python代码

[](https://frankphopkins.medium.com/?source=post_page-----958f24ac8884--------------------------------)[![Frank Hopkins](../Images/5ebfe2f31e7d5cb94581207ee3f469db.png)](https://frankphopkins.medium.com/?source=post_page-----958f24ac8884--------------------------------)[](https://towardsdatascience.com/?source=post_page-----958f24ac8884--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----958f24ac8884--------------------------------) [Frank Hopkins](https://frankphopkins.medium.com/?source=post_page-----958f24ac8884--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F342a4846c139&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpropensity-score-matching-psm-for-a-b-testing-reducing-bias-in-observational-studies-958f24ac8884&user=Frank+Hopkins&userId=342a4846c139&source=post_page-342a4846c139----958f24ac8884---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----958f24ac8884--------------------------------) ·12分钟阅读·2023年4月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F958f24ac8884&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpropensity-score-matching-psm-for-a-b-testing-reducing-bias-in-observational-studies-958f24ac8884&user=Frank+Hopkins&userId=342a4846c139&source=-----958f24ac8884---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F958f24ac8884&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpropensity-score-matching-psm-for-a-b-testing-reducing-bias-in-observational-studies-958f24ac8884&source=-----958f24ac8884---------------------bookmark_footer-----------)![](../Images/01bc4249555c702c1e0dd3e4dae0787e.png)

AI生成图像“以瓦西里·康定斯基风格的PSM”，使用DALL:E 2 — 版权归**Frank Hopkins**（作者）

A/B 测试是一种广泛使用的实验设计，其中比较两个或更多干预措施对感兴趣结果的影响。A/B 测试的目标是估计干预措施对结果的因果效应，同时控制潜在的混杂变量。随机化通常用于在处理组和对照组之间实现平衡，但可能并不总是可行或足以在所有相关协变量上实现平衡。因此，估计的处理效应可能由于处理组和对照组之间的特征差异而存在偏差。

倾向评分匹配（PSM）是一种统计方法，旨在通过基于倾向评分创建可比的处理组和对照组来减少估计处理效应的偏差。倾向评分是给定一组观察到的协变量时接受处理的条件概率，它总结了与估计处理效应相关的协变量信息。PSM 将处理组和对照组中具有相似倾向评分的个体进行匹配，从而可以平衡……
