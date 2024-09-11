# 使用这个新工具提升你的数据清理技巧

> 原文：[https://towardsdatascience.com/supercharge-your-data-cleaning-game-with-this-new-tool-d43e99cdc6a5?source=collection_archive---------13-----------------------#2023-04-19](https://towardsdatascience.com/supercharge-your-data-cleaning-game-with-this-new-tool-d43e99cdc6a5?source=collection_archive---------13-----------------------#2023-04-19)

## PYTHON | 数据 | 分析

## 一份利用 pandas_dq 轻松进行数据清理的指南

[](https://david-farrugia.medium.com/?source=post_page-----d43e99cdc6a5--------------------------------)[![David Farrugia](../Images/082ed61e24c7c26a4ae1c77343a87824.png)](https://david-farrugia.medium.com/?source=post_page-----d43e99cdc6a5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d43e99cdc6a5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d43e99cdc6a5--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----d43e99cdc6a5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3916826092a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharge-your-data-cleaning-game-with-this-new-tool-d43e99cdc6a5&user=David+Farrugia&userId=3916826092a6&source=post_page-3916826092a6----d43e99cdc6a5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d43e99cdc6a5--------------------------------) ·5分钟阅读·2023年4月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd43e99cdc6a5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharge-your-data-cleaning-game-with-this-new-tool-d43e99cdc6a5&user=David+Farrugia&userId=3916826092a6&source=-----d43e99cdc6a5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd43e99cdc6a5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharge-your-data-cleaning-game-with-this-new-tool-d43e99cdc6a5&source=-----d43e99cdc6a5---------------------bookmark_footer-----------)![](../Images/7062261f0283364355b203a6d4dcad89.png)

图片由 [JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果这篇文章的标题引起了你的兴趣，那么你一定意识到数据清理和预处理步骤在整体分析项目中的关键重要性。

如果你正在准备训练一个机器学习模型，或者你只是想进行一些探索性数据分析，脏数据无疑是你面前的一大障碍。你可能听说过这样一句话，准备数据占了工作量的**80%**。

数据清洗过程可能是数据分析过程中最耗时、最耗费精力且常常令人沮丧的部分。寻找重复数据、多重共线性、缺失值或无穷大值，仅举几例，都是从理解数据和得出可操作见解中挤出来的宝贵时间。

在这篇文章中，我们将深入讨论那个令人惊叹的 Python 包 `pandas_dq`，以及它如何提升你下一个数据清洗任务的速度和质量。
