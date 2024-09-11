# 降低你的 Cloud Composer 账单（第一部分）

> 原文：[`towardsdatascience.com/reduce-your-cloud-composer-bills-f03e112df689?source=collection_archive---------5-----------------------#2023-03-24`](https://towardsdatascience.com/reduce-your-cloud-composer-bills-f03e112df689?source=collection_archive---------5-----------------------#2023-03-24)

## 使用计划化的 CICD 管道来关闭环境并将其恢复到之前的状态。

[](https://marcgeremie.medium.com/?source=post_page-----f03e112df689--------------------------------)![Marc Djohossou](https://marcgeremie.medium.com/?source=post_page-----f03e112df689--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f03e112df689--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f03e112df689--------------------------------) [Marc Djohossou](https://marcgeremie.medium.com/?source=post_page-----f03e112df689--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd99fd7fe99ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freduce-your-cloud-composer-bills-f03e112df689&user=Marc+Djohossou&userId=d99fd7fe99ef&source=post_page-d99fd7fe99ef----f03e112df689---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f03e112df689--------------------------------) · 9 分钟阅读 · 2023 年 3 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff03e112df689&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freduce-your-cloud-composer-bills-f03e112df689&user=Marc+Djohossou&userId=d99fd7fe99ef&source=-----f03e112df689---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff03e112df689&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freduce-your-cloud-composer-bills-f03e112df689&source=-----f03e112df689---------------------bookmark_footer-----------)![](img/527a433072b680b844334d17b96d7046.png)

照片由 [Sasun Bughdaryan](https://unsplash.com/@sasun1990?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

[Cloud Composer](https://cloud.google.com/composer) 是一个托管和可扩展的流行作业调度器 [Airflow](https://airflow.apache.org/) 的安装版本。该服务在 Google Cloud Platform (GCP) 上提供两种版本：Cloud Composer 1 和 Cloud Composer 2，主要区别在于 Workers Autoscaling 仅在 Cloud Composer 2 中可用。

> 由于我使用该服务已经很多年了，我可以明确地说，它确实值得尝试。然而，一些公司会避开这个服务，这个原因可能不会让你感到惊讶。**金钱**。

在这篇文章中，我将分享一种**有效减少 Cloud Composer 账单**的方法。尽管代码片段仅适用于 Cloud Composer 2，但所倡导的策略对 Cloud Composer 1 用户仍然适用。

请注意，这是一系列两部分的第一部分。第二篇文章可以在这里查阅。

以下是将要讨论的主要主题：

> 理解 Cloud Composer 2 定价（第一部分）
