# 使用 Docker 在本地部署自己的 MLflow 工作空间

> 原文：[`towardsdatascience.com/deploy-your-own-mlflow-workspace-on-premise-with-docker-b54294676f0b?source=collection_archive---------11-----------------------#2023-04-17`](https://towardsdatascience.com/deploy-your-own-mlflow-workspace-on-premise-with-docker-b54294676f0b?source=collection_archive---------11-----------------------#2023-04-17)

## 像专业人士一样管理你的 ML 模型生命周期

[](https://tinztwinspro.medium.com/?source=post_page-----b54294676f0b--------------------------------)![Janik and Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----b54294676f0b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b54294676f0b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b54294676f0b--------------------------------) [Janik and Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----b54294676f0b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4eb5d9652d9e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-your-own-mlflow-workspace-on-premise-with-docker-b54294676f0b&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=post_page-4eb5d9652d9e----b54294676f0b---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b54294676f0b--------------------------------) ·8 分钟阅读·2023 年 4 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb54294676f0b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-your-own-mlflow-workspace-on-premise-with-docker-b54294676f0b&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=-----b54294676f0b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb54294676f0b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-your-own-mlflow-workspace-on-premise-with-docker-b54294676f0b&source=-----b54294676f0b---------------------bookmark_footer-----------)![](img/fa03e98f888a6fd039fc9344664945b8.png)

照片由[Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[MLflow](https://mlflow.org) 是一个**开源平台**，用于管理机器学习模型的整个生命周期。它跟踪每个机器学习实验的代码、数据和结果，这意味着你可以随时查看所有实验的历史记录。对于每个数据科学家来说，这都是一个梦想。此外，MLflow 是**库无关的**，这意味着你可以使用所有的机器学习库，如 TensorFlow、PyTorch 或 scikit-learn。所有 MLflow 功能都可以通过 [REST API](https://mlflow.org/docs/latest/rest-api.html#rest-api)、[CLI](https://mlflow.org/docs/latest/cli.html)、[Python API](https://mlflow.org/docs/latest/python_api/index.html)、[R API](https://mlflow.org/docs/latest/R-api.html) 和 [Java API](https://mlflow.org/docs/latest/java_api/index.html) 访问。

作为数据科学家，你会花费大量时间来优化机器学习模型。最佳模型通常依赖于最优的超参数或特征选择，而找到最佳组合是一项挑战。此外，你还需要记住所有的实验，这非常耗时。MLflow 是一个高效的平台，用于应对这些挑战。

在这篇文章中，我们简要介绍了 MLflow 的基础知识，并展示如何在本地设置 MLflow 工作区。我们在 Docker 堆栈中设置了 MLflow 环境，以便在所有系统上运行它。在这个过程中，我们有 Postgres 数据库、SFTP 服务器、JupyterLab 和 MLflow 跟踪服务器 UI。让我们开始吧。

# MLflow 基础知识
