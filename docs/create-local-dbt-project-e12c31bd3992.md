# 创建本地 dbt 项目

> 原文：[https://towardsdatascience.com/create-local-dbt-project-e12c31bd3992?source=collection_archive---------1-----------------------#2023-01-12](https://towardsdatascience.com/create-local-dbt-project-e12c31bd3992?source=collection_archive---------1-----------------------#2023-01-12)

## 如何使用 Docker 创建一个带有测试用虚拟数据的本地 dbt 项目

[](https://gmyrianthous.medium.com/?source=post_page-----e12c31bd3992--------------------------------)[![Giorgos Myrianthous](../Images/ff4b116e4fb9a095ce45eb064fde5af3.png)](https://gmyrianthous.medium.com/?source=post_page-----e12c31bd3992--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e12c31bd3992--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e12c31bd3992--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----e12c31bd3992--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-local-dbt-project-e12c31bd3992&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----e12c31bd3992---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e12c31bd3992--------------------------------) · 7 分钟阅读 · 2023年1月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe12c31bd3992&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-local-dbt-project-e12c31bd3992&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----e12c31bd3992---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe12c31bd3992&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-local-dbt-project-e12c31bd3992&source=-----e12c31bd3992---------------------bookmark_footer-----------)![](../Images/481b848f924c744cbd3a67c44800e6fb.png)

照片由 [Daniel K Cheung](https://unsplash.com/@danielkcheung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄于 [Unsplash](https://unsplash.com/photos/ZqqlOZyGG7g?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[dbt（数据构建工具）是数据工程和分析领域最热门的技术之一](/dbt-55b35c974533)。最近，我正在处理一个任务，该任务对 dbt 生成的工件进行了一些后处理，并希望编写一些测试。为此，我不得不创建一个可以本地（或在 Docker 容器中）运行的示例项目，以便我不必与实际的数据仓库进行交互。

在本文中，我们将详细介绍创建dbt项目并将其连接到容器化Postgres实例的逐步过程。你可以将这些项目用于测试目的，甚至可以用来尝试dbt的功能或练习技能。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

## 第一步：创建一个dbt项目

我们将向Postgres数据库中填充一些数据，因此我们首先需要从PyPI安装dbt Postgres适配器：

```py
pip install dbt-postgres==1.3.1
```

请注意，该命令还会安装`dbt-core`包以及运行dbt所需的其他依赖项。
