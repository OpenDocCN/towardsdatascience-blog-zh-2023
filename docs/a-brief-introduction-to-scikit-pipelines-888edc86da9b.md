# SciKit Pipelines 简介

> 原文：[`towardsdatascience.com/a-brief-introduction-to-scikit-pipelines-888edc86da9b?source=collection_archive---------3-----------------------#2023-08-23`](https://towardsdatascience.com/a-brief-introduction-to-scikit-pipelines-888edc86da9b?source=collection_archive---------3-----------------------#2023-08-23)

## 你为什么应该开始使用它们。

[](https://medium.com/@jodancker?source=post_page-----888edc86da9b--------------------------------)![Jonte Dancker](https://medium.com/@jodancker?source=post_page-----888edc86da9b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----888edc86da9b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----888edc86da9b--------------------------------) [Jonte Dancker](https://medium.com/@jodancker?source=post_page-----888edc86da9b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9f4927da760a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-brief-introduction-to-scikit-pipelines-888edc86da9b&user=Jonte+Dancker&userId=9f4927da760a&source=post_page-9f4927da760a----888edc86da9b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----888edc86da9b--------------------------------) ·7 分钟阅读·2023 年 8 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F888edc86da9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-brief-introduction-to-scikit-pipelines-888edc86da9b&user=Jonte+Dancker&userId=9f4927da760a&source=-----888edc86da9b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F888edc86da9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-brief-introduction-to-scikit-pipelines-888edc86da9b&source=-----888edc86da9b---------------------bookmark_footer-----------)![](img/f2987996b1c9e2cc8de667615c02c9c0.png)

图片由 [Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你是否曾训练过一个机器学习模型，并且你的预测看起来好得令人难以置信？但后来你意识到训练数据和测试数据之间存在数据泄漏？

或者你是否经历了许多预处理步骤，以至于将这些步骤从模型训练转移到生产环境中进行实际预测变得困难？

或者你的预处理变得混乱，难以以可读且易于理解的方式共享代码？

那么你可能想尝试`scikit-learn`的 Pipeline。Pipeline 是一个优雅的解决方案，用于设置你的机器学习训练、测试和生产流程，使你的工作更轻松，结果更具可重复性。

那么，Pipeline 是什么？它有什么好处？以及如何设置一个 Pipeline？我将回答这些问题，并给你一些构建模块的代码示例。通过结合这些构建模块，你可以构建出更复杂的 Pipeline，满足你的需求。

# 什么是 Pipeline？

Pipeline 允许你在机器学习工作流中组装多个步骤，这些步骤按顺序转换数据，然后将数据传递给估算器……
