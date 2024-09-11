# 全面的时间序列探索分析

> 原文：[https://towardsdatascience.com/comprehensive-time-series-exploratory-analysis-78bf40d16083?source=collection_archive---------1-----------------------#2023-11-25](https://towardsdatascience.com/comprehensive-time-series-exploratory-analysis-78bf40d16083?source=collection_archive---------1-----------------------#2023-11-25)

## 深入探讨空气质量数据

[](https://medium.com/@erich.hs?source=post_page-----78bf40d16083--------------------------------)[![Erich Silva](../Images/448dee1644d3f3e092bbbcfbbf07592d.png)](https://medium.com/@erich.hs?source=post_page-----78bf40d16083--------------------------------)[](https://towardsdatascience.com/?source=post_page-----78bf40d16083--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----78bf40d16083--------------------------------) [Erich Silva](https://medium.com/@erich.hs?source=post_page-----78bf40d16083--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cef55fab80c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomprehensive-time-series-exploratory-analysis-78bf40d16083&user=Erich+Silva&userId=2cef55fab80c&source=post_page-2cef55fab80c----78bf40d16083---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----78bf40d16083--------------------------------) ·18 分钟阅读·2023 年 11 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F78bf40d16083&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomprehensive-time-series-exploratory-analysis-78bf40d16083&user=Erich+Silva&userId=2cef55fab80c&source=-----78bf40d16083---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F78bf40d16083&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomprehensive-time-series-exploratory-analysis-78bf40d16083&source=-----78bf40d16083---------------------bookmark_footer-----------)![](../Images/a6c9e51ca7e3e5d265d6379258366dc9.png)

照片由[Jason Blackeye](https://unsplash.com/@jeisblack?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/collections/55366/my-first-collection/981603704225affe48a9007fc5094d84?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄。

这里你有一个按时间戳索引的数据集。你的数据可能涉及存储需求和供应，你的任务是预测战略产品的理想补货间隔。或者，你可能需要将历史销售信息转化为团队的关键可操作见解。也许你的数据是金融的，包括历史利率和一系列股票价格。也许你被要求建模市场波动性，并需要在投资期限内量化货币风险。从社会科学和能源分配，到医疗保健和环境研究，例子不胜枚举。但这些场景有什么共同之处？其一，你手头有一个时间序列任务。其二，你一定会从开始进行**简明而全面的探索性分析**中受益。

## 目录

+   [本文的目标](#d55f)

+   [数据集描述](#f0f7)

+   [库和依赖项](#d791)

+   [开始使用](#85bf)

+   [全景](#b967)

+   [详细视图](#fa99)

+   [缺失值](#9720)

+   [间歇性](#ce22)

+   [季节性](#3002)
