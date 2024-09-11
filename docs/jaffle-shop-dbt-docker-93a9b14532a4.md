# 在 Docker 中运行 Jaffle Shop dbt 项目

> 原文：[`towardsdatascience.com/jaffle-shop-dbt-docker-93a9b14532a4?source=collection_archive---------6-----------------------#2023-04-28`](https://towardsdatascience.com/jaffle-shop-dbt-docker-93a9b14532a4?source=collection_archive---------6-----------------------#2023-04-28)

## 一个容器化版本的流行 Jaffle Shop dbt 项目

[](https://gmyrianthous.medium.com/?source=post_page-----93a9b14532a4--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----93a9b14532a4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----93a9b14532a4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----93a9b14532a4--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----93a9b14532a4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjaffle-shop-dbt-docker-93a9b14532a4&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----93a9b14532a4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----93a9b14532a4--------------------------------) · 8 分钟阅读 · 2023 年 4 月 28 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F93a9b14532a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjaffle-shop-dbt-docker-93a9b14532a4&source=-----93a9b14532a4---------------------bookmark_footer-----------)![](img/90bb3052f4f1d0f374def43154a20afe.png)

图片由 [Ryan Howerter](https://unsplash.com/@rhowerter?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/photos/JXIFjYVbAS8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你对数据构建工具 (dbt) 不太熟悉，你可能已经遇到过所谓的 Jaffle Shop，这是一个用于测试的项目。

> `jaffle_shop` 是一个虚构的电子商务商店。这个 dbt 项目将应用数据库中的原始数据转化为适合分析的客户和订单模型。
> 
> - [Jaffle Shop GitHub 项目](https://github.com/dbt-labs/jaffle_shop)

我在 Jaffle Shop 项目中观察到的一个根本问题是，它期望用户，可能是 dbt 的新手，配置和托管一个本地数据库，以便 dbt 模型能够生成数据。

在本教程中，我将演示如何使用 Docker 创建项目的容器化版本。这将使我们能够部署一个 Postgres 实例，并配置 dbt 项目以从该数据库读取和写入数据。我还将提供一个链接，指向我创建的 GitHub 项目，这将帮助你快速启动并运行所有服务。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

## 创建 Dockerfile 和 docker-compose.yml

让我们从定义通过 Docker 运行的服务开始。首先，我们将创建一个…
