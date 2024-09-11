# 将机器学习模型生产化，与无服务器容器服务

> 原文：[`towardsdatascience.com/productionize-machine-learning-models-with-serverless-container-services-9323fdb8f60c?source=collection_archive---------15-----------------------#2023-01-09`](https://towardsdatascience.com/productionize-machine-learning-models-with-serverless-container-services-9323fdb8f60c?source=collection_archive---------15-----------------------#2023-01-09)

## *如何为您的机器学习模型创建无服务器容器化推理端点*

[](https://medium.com/@edwin.tan?source=post_page-----9323fdb8f60c--------------------------------)![Edwin Tan](https://medium.com/@edwin.tan?source=post_page-----9323fdb8f60c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9323fdb8f60c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9323fdb8f60c--------------------------------) [Edwin Tan](https://medium.com/@edwin.tan?source=post_page-----9323fdb8f60c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F484e02c96aa7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fproductionize-machine-learning-models-with-serverless-container-services-9323fdb8f60c&user=Edwin+Tan&userId=484e02c96aa7&source=post_page-484e02c96aa7----9323fdb8f60c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9323fdb8f60c--------------------------------) ·7 分钟阅读·2023 年 1 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9323fdb8f60c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fproductionize-machine-learning-models-with-serverless-container-services-9323fdb8f60c&user=Edwin+Tan&userId=484e02c96aa7&source=-----9323fdb8f60c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9323fdb8f60c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fproductionize-machine-learning-models-with-serverless-container-services-9323fdb8f60c&source=-----9323fdb8f60c---------------------bookmark_footer-----------)![](img/b3117ecde779f597268070c22634e534.png)

图片由 [Jan Canty](https://unsplash.com/@jancanty?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

无服务器容器架构是一种构建和运行容器化应用程序和服务的方法，用户无需管理底层基础设施。在这种架构中，容器用于打包和部署应用程序，这些容器在云服务提供商提供的完全托管环境中运行。

云服务提供商负责运行容器所需的基础设施，例如硬件和操作系统。开发者无需担心设置或维护这些基础设施，而是可以专注于编写代码和构建应用程序。容器通常在一个集群中运行，云服务提供商会根据需求自动扩展或缩减容器的数量。这使得应用程序能够应对流量波动，无需人工干预。无服务器架构通常比传统架构更具成本效益，因为用户只需为实际使用的资源付费，而不是为固定数量的计算能力付费。
