# Python 到 SQL — 我现在可以将数据加载速度提升 20 倍

> 原文：[https://towardsdatascience.com/fast-load-data-to-sql-from-python-2d67aea946c0?source=collection_archive---------1-----------------------#2023-03-20](https://towardsdatascience.com/fast-load-data-to-sql-from-python-2d67aea946c0?source=collection_archive---------1-----------------------#2023-03-20)

## 上传大量数据的好方法、坏方法和丑陋的方法

[](https://thuwarakesh.medium.com/?source=post_page-----2d67aea946c0--------------------------------)[![Thuwarakesh Murallie](../Images/44f1a14a899426592bbd8c7f73ce169d.png)](https://thuwarakesh.medium.com/?source=post_page-----2d67aea946c0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d67aea946c0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2d67aea946c0--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----2d67aea946c0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffast-load-data-to-sql-from-python-2d67aea946c0&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----2d67aea946c0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d67aea946c0--------------------------------) ·6分钟阅读·2023年3月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2d67aea946c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffast-load-data-to-sql-from-python-2d67aea946c0&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----2d67aea946c0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2d67aea946c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffast-load-data-to-sql-from-python-2d67aea946c0&source=-----2d67aea946c0---------------------bookmark_footer-----------)![](../Images/8276c9e69e0172eae178b74760c9fee1.png)

[Matheo JBT](https://unsplash.com/@matheo_jbt?utm_source=medium&utm_medium=referral) 的照片，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

速度很重要！在数据管道中，和在其他地方一样。

处理海量数据集是大多数数据专业人士的日常工作。如果这些数据流入数据库或数据仓库，则不会有问题。

但有时我们需要上传的数据量较大，会导致工作站在数小时内无法使用。去散步，喝些水，这对你有好处！

但如果你真的想缩短这个任务的时间，我们需要最优化的数据加载方式。

如果是预格式化文件，我更愿意使用客户端库来完成这项任务。例如，以下的 shell 命令可以将你的 CSV 文件上传到远程 Postgres 数据库。

[## 使用 Streamlit 在几分钟内创建 GPT3 驱动的应用](https://levelup.gitconnected.com/streamlit-openai-gpt3-example-app-b333da955ceb?source=post_page-----2d67aea946c0--------------------------------)

### 学习构建智能应用，而不必过多担心软件开发的问题。

[点击这里查看详细内容](https://levelup.gitconnected.com/streamlit-openai-gpt3-example-app-b333da955ceb?source=post_page-----2d67aea946c0--------------------------------)
