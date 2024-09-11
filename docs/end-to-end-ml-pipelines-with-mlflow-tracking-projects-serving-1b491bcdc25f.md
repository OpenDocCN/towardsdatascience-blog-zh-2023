# 端到端 ML 流水线与 MLflow：跟踪、项目与服务

> 原文：[https://towardsdatascience.com/end-to-end-ml-pipelines-with-mlflow-tracking-projects-serving-1b491bcdc25f?source=collection_archive---------3-----------------------#2023-02-16](https://towardsdatascience.com/end-to-end-ml-pipelines-with-mlflow-tracking-projects-serving-1b491bcdc25f?source=collection_archive---------3-----------------------#2023-02-16)

## 适用于 MLflow 高级使用的终极教程

[](https://medium.com/@antonsruberts?source=post_page-----1b491bcdc25f--------------------------------)[![Antons Tocilins-Ruberts](../Images/363a4f32aa793cca7a67dea68e76e3cf.png)](https://medium.com/@antonsruberts?source=post_page-----1b491bcdc25f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1b491bcdc25f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1b491bcdc25f--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----1b491bcdc25f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9dee50b0374b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fend-to-end-ml-pipelines-with-mlflow-tracking-projects-serving-1b491bcdc25f&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=post_page-9dee50b0374b----1b491bcdc25f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b491bcdc25f--------------------------------) ·9 分钟阅读·2023年2月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1b491bcdc25f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fend-to-end-ml-pipelines-with-mlflow-tracking-projects-serving-1b491bcdc25f&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=-----1b491bcdc25f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1b491bcdc25f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fend-to-end-ml-pipelines-with-mlflow-tracking-projects-serving-1b491bcdc25f&source=-----1b491bcdc25f---------------------bookmark_footer-----------)![](../Images/537bead127eea662380418af3a0ce3b1.png)

图片由 [Jeswin Thomas](https://unsplash.com/@jeswinthomas?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

MLflow 是一个功能强大的工具，它经常因其实验跟踪能力而受到关注。很容易理解为什么 —— 它是一个用户友好的平台，用于记录所有机器学习实验的重要细节，从超参数到模型。但你知道 MLflow 不仅仅提供实验跟踪功能吗？这个多功能框架还包括如 MLflow Projects、模型注册表和内置部署选项等功能。在这篇文章中，我们将探索如何利用这些功能来创建一个完整而高效的 ML 流水线。

对于完全的 MLflow 初学者来说，这个教程可能有点过于复杂，因此我强烈建议你在深入阅读之前观看这两个视频！
