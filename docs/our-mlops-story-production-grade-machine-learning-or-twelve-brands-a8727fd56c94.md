# 我们的 MLOps 故事：十二个品牌的生产级机器学习

> 原文：[https://towardsdatascience.com/our-mlops-story-production-grade-machine-learning-or-twelve-brands-a8727fd56c94?source=collection_archive---------7-----------------------#2023-06-05](https://towardsdatascience.com/our-mlops-story-production-grade-machine-learning-or-twelve-brands-a8727fd56c94?source=collection_archive---------7-----------------------#2023-06-05)

## 在荷兰 DPG 媒体用有限资源构建 MLOps 平台时学到的东西

[](https://medium.com/@jeffluppes?source=post_page-----a8727fd56c94--------------------------------)[![Jeffrey Luppes](../Images/f55d5e76fcf7e992582b45096500c215.png)](https://medium.com/@jeffluppes?source=post_page-----a8727fd56c94--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a8727fd56c94--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a8727fd56c94--------------------------------) [Jeffrey Luppes](https://medium.com/@jeffluppes?source=post_page-----a8727fd56c94--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Faaba3653736b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Four-mlops-story-production-grade-machine-learning-or-twelve-brands-a8727fd56c94&user=Jeffrey+Luppes&userId=aaba3653736b&source=post_page-aaba3653736b----a8727fd56c94---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a8727fd56c94--------------------------------) ·12 分钟阅读·2023年6月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa8727fd56c94&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Four-mlops-story-production-grade-machine-learning-or-twelve-brands-a8727fd56c94&user=Jeffrey+Luppes&userId=aaba3653736b&source=-----a8727fd56c94---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa8727fd56c94&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Four-mlops-story-production-grade-machine-learning-or-twelve-brands-a8727fd56c94&source=-----a8727fd56c94---------------------bookmark_footer-----------)

部署一次机器学习模型是一项简单的任务；将机器学习模型反复投入生产则要困难得多。为了解决这个复杂的过程，MLOps（机器学习运维）的概念应运而生。MLOps 代表了 DevOps、机器学习和软件工程实践的融合。这里有几个细微之处，但对 MLOps 到底包含什么的更好定义仍在讨论中，并且是厂商推销其产品的战场。为了简洁起见，我更愿意继续讲述我们的 MLOps 故事。

我们的MLOps旅程始于2021年9月。我们的团队仅在六个月前成立，当时我们开始时只有一些继承的项目。我们团队的目标在纸面上很简单：我们要为*在线服务*提供数据/ML平台，这是DPG Media的一部分，专注于网站和社区。我们的“投资组合”包括十几个品牌，其中包括一个受欢迎的科技新闻网站及其社区、两个招聘门户、一个孕期社区、几个可以购买二手车的网站以及几个类似受众的其他网站。这对后文有一定的相关性。

![](../Images/8961d6ab97f05ce9d2ee41a878ce0f6d.png)

当我们开始构建平台时，我们的目标是支持这十二个（荷兰）品牌。
