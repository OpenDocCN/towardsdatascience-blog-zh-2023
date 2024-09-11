# 提升建模——数据科学家的信用卡续订活动优化指南

> 原文：[https://towardsdatascience.com/uplift-modeling-a-data-scientists-guide-to-optimizing-a-credit-card-renewal-campaign-9f02f863067a?source=collection_archive---------4-----------------------#2023-07-13](https://towardsdatascience.com/uplift-modeling-a-data-scientists-guide-to-optimizing-a-credit-card-renewal-campaign-9f02f863067a?source=collection_archive---------4-----------------------#2023-07-13)

## 应用因果机器学习来缩小活动目标受众——第1部分/2

[](https://abhijeetstalaulikar.medium.com/?source=post_page-----9f02f863067a--------------------------------)[![Abhijeet Talaulikar](../Images/073f89914ec4b541d76a14b23d48279b.png)](https://abhijeetstalaulikar.medium.com/?source=post_page-----9f02f863067a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f02f863067a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9f02f863067a--------------------------------) [Abhijeet Talaulikar](https://abhijeetstalaulikar.medium.com/?source=post_page-----9f02f863067a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F92e9f5319ba1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuplift-modeling-a-data-scientists-guide-to-optimizing-a-credit-card-renewal-campaign-9f02f863067a&user=Abhijeet+Talaulikar&userId=92e9f5319ba1&source=post_page-92e9f5319ba1----9f02f863067a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f02f863067a--------------------------------) ·9分钟阅读·2023年7月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9f02f863067a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuplift-modeling-a-data-scientists-guide-to-optimizing-a-credit-card-renewal-campaign-9f02f863067a&user=Abhijeet+Talaulikar&userId=92e9f5319ba1&source=-----9f02f863067a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9f02f863067a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuplift-modeling-a-data-scientists-guide-to-optimizing-a-credit-card-renewal-campaign-9f02f863067a&source=-----9f02f863067a---------------------bookmark_footer-----------)![](../Images/c3b5f56654297f88414720f8a6e95c4c.png)

由作者使用 Bing 图像创作者生成

**第1部分：** 在信用卡续订活动中缩小目标受众

**第2部分：** [识别信用卡现金返还活动中的最佳客户旅程](https://pub.towardsai.net/discrete-time-markov-chains-identifying-winning-customer-journeys-in-a-cashback-campaign-39b62eb8a6fe)

作为一名初露头角的数据科学家，我的学术背景教会了我将准确性视为成功项目的标志。而行业则关注短期和长期的盈利与节省。**这篇文章是关于投资回报率（ROI）——业务行动的圣杯**。

许多推广活动针对的是客户的细分群体，而不是直接面对个人。例如，付费搜索、展示广告、付费社交等。而直接面向消费者（D2C）的活动则直接针对个人客户。这些包括直邮、电子邮件、短信甚至推送通知。银行和金融科技领域的企业能够进行大规模的D2C活动，因为每个人都有应用程序。但如今，这些企业正寻求在推广开支上提高效率（怎么做？）。

# 理解问题
