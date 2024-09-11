# 为什么数据科学家应该采用机器学习（ML）管道

> 原文：[https://towardsdatascience.com/why-should-data-scientists-adopt-machine-learning-ml-pipelines-8fc5e24920dd?source=collection_archive---------11-----------------------#2023-02-01](https://towardsdatascience.com/why-should-data-scientists-adopt-machine-learning-ml-pipelines-8fc5e24920dd?source=collection_archive---------11-----------------------#2023-02-01)

## 意见

## MLOps 实践 — 作为数据科学家，你是否将笔记本还是 ML 管道交给 ML 工程师或 DevOps 工程师，以便将 ML 模型部署到生产环境中？

[](https://medium.com/@weiyunna91?source=post_page-----8fc5e24920dd--------------------------------)[![YUNNA WEI](../Images/ffd0dd5c697dd2b4640ade49274d2bf9.png)](https://medium.com/@weiyunna91?source=post_page-----8fc5e24920dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8fc5e24920dd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8fc5e24920dd--------------------------------) [YUNNA WEI](https://medium.com/@weiyunna91?source=post_page-----8fc5e24920dd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4b47aa84fc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-should-data-scientists-adopt-machine-learning-ml-pipelines-8fc5e24920dd&user=YUNNA+WEI&userId=4b47aa84fc4&source=post_page-4b47aa84fc4----8fc5e24920dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8fc5e24920dd--------------------------------) ·9分钟阅读·2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8fc5e24920dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-should-data-scientists-adopt-machine-learning-ml-pipelines-8fc5e24920dd&user=YUNNA+WEI&userId=4b47aa84fc4&source=-----8fc5e24920dd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8fc5e24920dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-should-data-scientists-adopt-machine-learning-ml-pipelines-8fc5e24920dd&source=-----8fc5e24920dd---------------------bookmark_footer-----------)

## 背景

在我之前的文章中：

+   [学习 MLOps 的核心 — 构建机器学习（ML）管道](https://medium.com/towards-data-science/learn-the-core-of-mlops-building-machine-learning-ml-pipelines-7242b77520b7)

+   [MLOps 实践 — 将 ML 解决方案架构拆解为 10 个组件](/mlops-in-practice-de-constructing-an-ml-solution-architecture-into-10-components-c55c88d8fc7a?sk=a14ce7ead68a2f90868d7a063eea84e3)

我讨论了构建机器学习管道的重要性。在今天的文章中，我将深入探讨机器学习管道的主题，并详细解释：

+   为什么构建机器学习管道是必要且重要的？

+   机器学习管道的关键组成部分是什么？

+   为什么以及如何让数据科学家采用机器学习管道？

![](../Images/b81b7321cd7004472582896baf58d71b.png)

图片来源：[Jon Tyson](https://unsplash.com/@jontyson?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 为什么构建机器学习管道是必要且重要的？

数据科学家在笔记本环境中开始开发机器学习模型是相当常见的。在笔记本中，他们会尝试不同的数据集、不同的特征…
