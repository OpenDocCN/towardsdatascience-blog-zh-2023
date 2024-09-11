# 回填精通：提升数据工程技能

> 原文：[https://towardsdatascience.com/backfilling-mastery-elevating-data-engineering-expertise-a6873fb7ed2e?source=collection_archive---------12-----------------------#2023-11-17](https://towardsdatascience.com/backfilling-mastery-elevating-data-engineering-expertise-a6873fb7ed2e?source=collection_archive---------12-----------------------#2023-11-17)

## 数据工程

## 数据工程师应对回填迷宫的终极指南

[](https://tamimi-naser.medium.com/?source=post_page-----a6873fb7ed2e--------------------------------)[![Naser Tamimi](../Images/8d43c66ea3c0ef9b49c7d33dbc008c28.png)](https://tamimi-naser.medium.com/?source=post_page-----a6873fb7ed2e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a6873fb7ed2e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a6873fb7ed2e--------------------------------) [Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----a6873fb7ed2e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff94e8b97c7aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbackfilling-mastery-elevating-data-engineering-expertise-a6873fb7ed2e&user=Naser+Tamimi&userId=f94e8b97c7aa&source=post_page-f94e8b97c7aa----a6873fb7ed2e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6873fb7ed2e--------------------------------) ·7分钟阅读·2023年11月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa6873fb7ed2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbackfilling-mastery-elevating-data-engineering-expertise-a6873fb7ed2e&user=Naser+Tamimi&userId=f94e8b97c7aa&source=-----a6873fb7ed2e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa6873fb7ed2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbackfilling-mastery-elevating-data-engineering-expertise-a6873fb7ed2e&source=-----a6873fb7ed2e---------------------bookmark_footer-----------)![](../Images/ca03f6688b8f11402878f079d3beac93.png)

图片由[Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 什么是回填？

想象一下你开始一个新的数据管道，并从你以前从未解析过的源（例如，从API或现有的hive表中提取信息）获取数据。现在，你的任务是让它看起来像是你早已收集了这些数据。这就是我们在数据工程中所称的回填的一个例子。

但这不仅仅是关于启动新的数据管道或表格。你可能有一个已经积累了数据的表格，突然需要更改数据（例如由于新的指标定义），或者从新的数据源中添加更多数据。或者也许你的数据中有一个尴尬的空白，你只是想修补它。所有这些情况都是数据回填的例子。共同点是回到时间的“过去”并用一些历史数据“填充”你的表格。

下图（图1）展示了一个直接的回填场景。在这种情况下，每日任务从两个上游源（一个用于平台A，另一个用于平台B）检索数据。数据集的结构是第一个分区为‘ds’，第二个分区（或…
