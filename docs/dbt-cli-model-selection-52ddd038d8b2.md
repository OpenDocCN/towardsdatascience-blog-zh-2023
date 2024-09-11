# dbt CLI 的模型选择

> 原文：[https://towardsdatascience.com/dbt-cli-model-selection-52ddd038d8b2?source=collection_archive---------10-----------------------#2023-01-25](https://towardsdatascience.com/dbt-cli-model-selection-52ddd038d8b2?source=collection_archive---------10-----------------------#2023-01-25)

## 一个完整的 cheatsheet，用于在运行 dbt 命令时选择特定模型

[](https://gmyrianthous.medium.com/?source=post_page-----52ddd038d8b2--------------------------------)[![Giorgos Myrianthous](../Images/ff4b116e4fb9a095ce45eb064fde5af3.png)](https://gmyrianthous.medium.com/?source=post_page-----52ddd038d8b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----52ddd038d8b2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----52ddd038d8b2--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----52ddd038d8b2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdbt-cli-model-selection-52ddd038d8b2&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----52ddd038d8b2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----52ddd038d8b2--------------------------------) ·7 min read·2023年1月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F52ddd038d8b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdbt-cli-model-selection-52ddd038d8b2&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----52ddd038d8b2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F52ddd038d8b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdbt-cli-model-selection-52ddd038d8b2&source=-----52ddd038d8b2---------------------bookmark_footer-----------)![](../Images/86526646e5b957d9c7aba56a61a39a50.png)

由 [Yulia Matvienko](https://unsplash.com/@yuliamatvienko?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供的照片，来源于 [Unsplash](https://unsplash.com/photos/kgz9vsP5JCU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在进行 dbt 项目时，你需要确保用于运行或测试模型、种子和快照的 CLI 命令仅涵盖相关资源（或其子集）。换句话说，你需要能够针对特定模型、测试、种子或快照，以避免浪费资源和金钱。当你处理处理大量数据的较大模型时，这一点尤为重要。

默认情况下，`dbt run|test|seed|snapshot` 将执行依赖图中的所有相应节点（即 `dbt run` 将运行所有模型，`dbt test` 将运行所有模型测试，等等）。在本文中，我们将介绍在通过 dbt 命令行接口（CLI）运行或测试模型、种子或快照时可以利用的所有可能的模型选择简写。

如果你打算尝试我们将在接下来的几个部分中介绍的命令，可以随意在本地创建一个示例 dbt 项目。你可以通过[遵循这个逐步指南，在这里你也可以找到一个容器化的 dbt 环境](/create-local-dbt-project-e12c31bd3992)（可能不到两分钟）。

## 运行 dbt 项目中的所有资源
