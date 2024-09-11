# 如何使用 GitHub Actions 构建简单的 ETL 管道

> 原文：[`towardsdatascience.com/etl-github-actions-cron-383f618704b6?source=collection_archive---------4-----------------------#2023-05-05`](https://towardsdatascience.com/etl-github-actions-cron-383f618704b6?source=collection_archive---------4-----------------------#2023-05-05)

## ETL 不必复杂。如果是这样的话，可以使用 GitHub Actions。

[](https://thuwarakesh.medium.com/?source=post_page-----383f618704b6--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----383f618704b6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----383f618704b6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----383f618704b6--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----383f618704b6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fetl-github-actions-cron-383f618704b6&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----383f618704b6---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----383f618704b6--------------------------------) · 6 分钟阅读 · 2023 年 5 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F383f618704b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fetl-github-actions-cron-383f618704b6&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----383f618704b6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F383f618704b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fetl-github-actions-cron-383f618704b6&source=-----383f618704b6---------------------bookmark_footer-----------)![](img/a24a64f85f7450f2509f91f3562ef550.png)

图片来自 [Roman Synkevych 🇺🇦](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你从事软件开发，你会知道什么是 GitHub Actions。它是 GitHub 提供的一个工具，用于自动化开发任务。或者用流行的说法，就是一个 DevOps 工具。

但人们很少使用它来构建 ETL 管道。

提到 ETL 时，首先想到的是 Airflow、Prefect 或其他相关工具。毫无疑问，它们在任务编排方面是市场上最好的。然而，我们构建的许多 ETL 都很简单，为它们托管一个单独的工具通常是不必要的。

你可以改用 GitHub Actions。

本文重点介绍 GitHub Actions。不过，如果你使用的是 Bitbucket 或 GitLab，也可以使用它们各自的替代方案。

[](/black-with-git-hub-actions-4ffc5c61b5fe?source=post_page-----383f618704b6--------------------------------) ## 使用 Black 和 GitHub Actions 维护干净的 Python 代码。

### 没有人愿意处理混乱的代码库；也很少有人有耐心去清理它。

towardsdatascience.com

我们可以在 GitHub Actions 上运行 Python、R 或 Julia 脚本。因此，作为数据科学家，你无需为此学习新的语言或工具。你甚至可以在任何 ETL 任务失败时收到邮件通知。
