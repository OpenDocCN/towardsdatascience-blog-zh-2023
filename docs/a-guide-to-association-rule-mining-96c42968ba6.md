# 关联规则挖掘指南

> 原文：[`towardsdatascience.com/a-guide-to-association-rule-mining-96c42968ba6?source=collection_archive---------5-----------------------#2023-04-05`](https://towardsdatascience.com/a-guide-to-association-rule-mining-96c42968ba6?source=collection_archive---------5-----------------------#2023-04-05)

## 使用 Python 通过市场篮子分析从频繁模式中创建洞察

[](https://idilismiguzel.medium.com/?source=post_page-----96c42968ba6--------------------------------)![Idil Ismiguzel](https://idilismiguzel.medium.com/?source=post_page-----96c42968ba6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96c42968ba6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----96c42968ba6--------------------------------) [Idil Ismiguzel](https://idilismiguzel.medium.com/?source=post_page-----96c42968ba6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d965c736f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-association-rule-mining-96c42968ba6&user=Idil+Ismiguzel&userId=6d965c736f2&source=post_page-6d965c736f2----96c42968ba6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----96c42968ba6--------------------------------) · 10 分钟阅读 · 2023 年 4 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F96c42968ba6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-association-rule-mining-96c42968ba6&user=Idil+Ismiguzel&userId=6d965c736f2&source=-----96c42968ba6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F96c42968ba6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-association-rule-mining-96c42968ba6&source=-----96c42968ba6---------------------bookmark_footer-----------)![](img/c4deb918ffc351de2f6044aa03c1a954.png)

照片由 [Matthias Schröder](https://unsplash.com/@trancepole?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

关联规则挖掘是一种基于规则的机器学习技术，用于发现数据集中的频繁模式。频繁模式可能包括通常一起购买的频繁项集或按顺序购买的子序列。例如，饼干和咖啡可以是咖啡馆的*频繁项集*，而笔记本电脑和外接显示器可以是电子产品店的*子序列*。在交易数据库中发现频繁模式并检测项目之间的关联是一个极受欢迎的数据科学应用场景。一些应用领域包括商品推荐、交叉销售、促销设计、客户行为分析和库存管理。

在本文中，我们将涵盖**市场篮子分析**技术用于关联规则挖掘，并将回答包括但不限于以下问题：

+   如果客户购买了 A 项，她还有多大可能购买 B 项？是否存在正相关或负相关，还是完全随机发生？

+   在商店或应用程序的内容页面中，哪些项目应该放在一起？

+   在促销活动中，哪些项目可以捆绑在一起，或者在对某个项目表现出兴趣后可以推荐哪些项目？
