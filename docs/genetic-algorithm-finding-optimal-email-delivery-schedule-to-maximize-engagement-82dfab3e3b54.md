# 遗传算法：寻找优化的电子邮件发送时间表以最大化参与度

> 原文：[https://towardsdatascience.com/genetic-algorithm-finding-optimal-email-delivery-schedule-to-maximize-engagement-82dfab3e3b54?source=collection_archive---------5-----------------------#2023-11-01](https://towardsdatascience.com/genetic-algorithm-finding-optimal-email-delivery-schedule-to-maximize-engagement-82dfab3e3b54?source=collection_archive---------5-----------------------#2023-11-01)

## 使用进化算法优化消费者银行的D2C活动

[](https://abhijeetstalaulikar.medium.com/?source=post_page-----82dfab3e3b54--------------------------------)[![Abhijeet Talaulikar](../Images/073f89914ec4b541d76a14b23d48279b.png)](https://abhijeetstalaulikar.medium.com/?source=post_page-----82dfab3e3b54--------------------------------)[](https://towardsdatascience.com/?source=post_page-----82dfab3e3b54--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----82dfab3e3b54--------------------------------) [Abhijeet Talaulikar](https://abhijeetstalaulikar.medium.com/?source=post_page-----82dfab3e3b54--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F92e9f5319ba1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenetic-algorithm-finding-optimal-email-delivery-schedule-to-maximize-engagement-82dfab3e3b54&user=Abhijeet+Talaulikar&userId=92e9f5319ba1&source=post_page-92e9f5319ba1----82dfab3e3b54---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----82dfab3e3b54--------------------------------) · 9分钟阅读 · 2023年11月1日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F82dfab3e3b54&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenetic-algorithm-finding-optimal-email-delivery-schedule-to-maximize-engagement-82dfab3e3b54&source=-----82dfab3e3b54---------------------bookmark_footer-----------)![](../Images/0d31fcceb333d2b64b85bacbbed6f77f.png)

作者使用Bing图像创作者生成的图像

某些电子邮件发送时间是否会导致更高的参与度？

电子邮件营销人员面临的最常见问题之一是何时发送电子邮件以最大化打开率、点击率和转化率。这个问题没有明确的答案，因为不同的受众可能有不同的偏好和行为。他们处于哪个时区？他们使用什么设备查看电子邮件？他们的日常生活和日程安排是什么？他们检查电子邮件的频率如何？这些因素会影响他们何时最有可能打开和互动你的电子邮件。

你可以使用A/B测试或分割测试工具来比较在不同时间发送的电子邮件活动的表现。你还可以使用分析工具，如Google Analytics或Mailchimp，来跟踪电子邮件活动的指标，如打开率、点击率、跳出率和转化率。通过分析数据，你可以确定最适合你的受众和目标的最佳发送时间。

当你对客户在不同时间的点击率和打开率有了良好的理解时，…
