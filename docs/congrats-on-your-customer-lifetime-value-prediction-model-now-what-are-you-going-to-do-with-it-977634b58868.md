# 恭喜你完成了 CLV 预测模型 —— 那么你打算怎么利用它？

> 原文：[https://towardsdatascience.com/congrats-on-your-customer-lifetime-value-prediction-model-now-what-are-you-going-to-do-with-it-977634b58868?source=collection_archive---------4-----------------------#2023-08-07](https://towardsdatascience.com/congrats-on-your-customer-lifetime-value-prediction-model-now-what-are-you-going-to-do-with-it-977634b58868?source=collection_archive---------4-----------------------#2023-08-07)

## 一本详尽的《客户终身价值》技术和实际应用指南

[](https://katherineamunro.medium.com/?source=post_page-----977634b58868--------------------------------)[![Katherine Munro](../Images/8013140495c7b9bd25ef08d712f097bf.png)](https://katherineamunro.medium.com/?source=post_page-----977634b58868--------------------------------)[](https://towardsdatascience.com/?source=post_page-----977634b58868--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----977634b58868--------------------------------) [Katherine Munro](https://katherineamunro.medium.com/?source=post_page-----977634b58868--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb84716d39740&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcongrats-on-your-customer-lifetime-value-prediction-model-now-what-are-you-going-to-do-with-it-977634b58868&user=Katherine+Munro&userId=b84716d39740&source=post_page-b84716d39740----977634b58868---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----977634b58868--------------------------------) ·10 min read·2023年8月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F977634b58868&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcongrats-on-your-customer-lifetime-value-prediction-model-now-what-are-you-going-to-do-with-it-977634b58868&user=Katherine+Munro&userId=b84716d39740&source=-----977634b58868---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F977634b58868&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcongrats-on-your-customer-lifetime-value-prediction-model-now-what-are-you-going-to-do-with-it-977634b58868&source=-----977634b58868---------------------bookmark_footer-----------)![](../Images/64d3f8c83d6c821f19ef2fb8cc282fdf.png)

我是一名数据科学家，分享我所知道的关于客户终身价值的所有内容：如何建模、预测以及实际应用。这是三部分中的第二部分，主要讲述 CLV 预测的使用案例。你的任务是接受这些想法并加以实施。图片来源：作者提供。

笑话，我挑战自己创建一份最详尽的客户生命周期价值（CLV）指南。代号为“其他教程遗漏的所有内容”，我将分享我在实际数据科学团队中处理这个主题时获得的所有想法和经验，包括不完美的数据和复杂的客户需求。

我的 [上一篇文章](https://example.org/from-analytics-to-actual-application-the-case-of-customer-lifetime-value-91e482561c21?sk=37a49fc0f966a29330372136ead4ede6) 涉及一个经常被忽视的话题：历史 CLV 计算的使用案例。它有点火，所以我想我找到了一些东西。在这篇文章中，我将讨论：

+   一些基本术语

+   CLV 预测的目标

+   CLV 预测使用（超越标准示例，我保证！）

在接下来的文章中，我们将讨论 CLV [计算](https://example.org/methods-for-modelling-customer-lifetime-value-the-good-stuff-and-the-gotchas-445f8a6587be?sk=4946eaaf845059f0256fe62627412d76) 和预测方法，它们的优缺点，以及如何正确使用它们的经验教训。

有很多内容需要涵盖，所以让我们开始吧！

# 奠定基础

无论你是数据科学家、分析师还是营销人员，当你开始时都需要领域知识…
