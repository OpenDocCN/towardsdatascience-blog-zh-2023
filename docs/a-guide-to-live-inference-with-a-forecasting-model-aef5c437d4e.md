# 实时推断与预测模型指南

> 原文：[`towardsdatascience.com/a-guide-to-live-inference-with-a-forecasting-model-aef5c437d4e?source=collection_archive---------10-----------------------#2023-02-22`](https://towardsdatascience.com/a-guide-to-live-inference-with-a-forecasting-model-aef5c437d4e?source=collection_archive---------10-----------------------#2023-02-22)

## 超越离线训练和测试预测

[](https://vcerq.medium.com/?source=post_page-----aef5c437d4e--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----aef5c437d4e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aef5c437d4e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aef5c437d4e--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----aef5c437d4e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-live-inference-with-a-forecasting-model-aef5c437d4e&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----aef5c437d4e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aef5c437d4e--------------------------------) ·6 分钟阅读·2023 年 2 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faef5c437d4e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-live-inference-with-a-forecasting-model-aef5c437d4e&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----aef5c437d4e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faef5c437d4e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-live-inference-with-a-forecasting-model-aef5c437d4e&source=-----aef5c437d4e---------------------bookmark_footer-----------)![](img/5712aa3104517bd883b73db69383a67c.png)

图片来源：[Fringer Cat](https://unsplash.com/@nittygritty_photo?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

网上有许多关于使用机器学习进行预测的资源。然而，这些资源侧重于离线训练和测试预测。

在这里，你将学习如何创建模型并使用它来预测*实际*的未来观测值。

# 介绍

预测资源经常忽略模型在实时预测中的应用。

有大量关于将机器学习应用于预测的信息。但大多数集中在预测生命周期的特定阶段。例如，数据预处理或模型构建。

这些资源通常缺乏关于如何在实际中应用模型的信息。即，如何将模型从离线环境扩展到实时环境。

在时间序列中，这一点尤其重要，因为观察值是相关的，它们的顺序很关键。

让我们深入探讨如何构建模型并使用它进行实时预测。

# 案例研究 — 预测波浪高度
