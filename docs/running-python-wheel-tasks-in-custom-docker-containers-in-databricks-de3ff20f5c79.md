# 在 Databricks 中运行自定义 Docker 容器中的 Python Wheel 任务

> 原文：[`towardsdatascience.com/running-python-wheel-tasks-in-custom-docker-containers-in-databricks-de3ff20f5c79?source=collection_archive---------8-----------------------#2023-06-29`](https://towardsdatascience.com/running-python-wheel-tasks-in-custom-docker-containers-in-databricks-de3ff20f5c79?source=collection_archive---------8-----------------------#2023-06-29)

## 一步步教程，教你如何在 Databricks 的自定义 Docker 镜像中构建和运行 Python Wheel 任务（包括 Poetry 和 Typer CLI）

[](https://johschmidt42.medium.com/?source=post_page-----de3ff20f5c79--------------------------------)![Johannes Schmidt](https://johschmidt42.medium.com/?source=post_page-----de3ff20f5c79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----de3ff20f5c79--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----de3ff20f5c79--------------------------------) [Johannes Schmidt](https://johschmidt42.medium.com/?source=post_page-----de3ff20f5c79--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb5022ff2e428&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-python-wheel-tasks-in-custom-docker-containers-in-databricks-de3ff20f5c79&user=Johannes+Schmidt&userId=b5022ff2e428&source=post_page-b5022ff2e428----de3ff20f5c79---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----de3ff20f5c79--------------------------------) ·13 分钟阅读·2023 年 6 月 29 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fde3ff20f5c79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frunning-python-wheel-tasks-in-custom-docker-containers-in-databricks-de3ff20f5c79&source=-----de3ff20f5c79---------------------bookmark_footer-----------)![](img/fb9e8f76720dd5326d06b7b8b62a10ad.png)

图片由 [Lluvia Morales](https://unsplash.com/@hi_lluvia?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据工程师设计和构建流水线来运行 ETL 工作负载，以便数据可以在下游用于解决业务问题。在 Databricks 中，对于这样的流水线，通常是通过创建一个**集群**、一个**笔记本/脚本**，并编写一些**Spark**代码来开始。一旦有了工作原型，就使其可以投入生产，这样你的代码可以作为 Databricks 作业执行，例如使用 REST API。对于 Databricks 而言，这意味着通常需要在 Databricks 文件系统上有一个 Python 笔记本/脚本，或者连接到工作区的远程 Git 存储库。但如果你不想执行以上任何操作呢？在不将任何文件上传到 Databricks 工作区或[连接到远程 Git 存储库](https://docs.databricks.com/repos/index.html)的情况下，还有另一种在 Databricks 中运行 Python 脚本作为作业的方式：利用声明入口点的**Python wheel 任务**和**Databricks 容器服务**，可以启动使用容器注册表中 Docker 镜像的作业运行。

因此，本教程将向您展示如何做到这一点：在 Databricks 中运行**Python 作业**（Python wheel 任务）并使用**自定义 Docker 镜像**。
