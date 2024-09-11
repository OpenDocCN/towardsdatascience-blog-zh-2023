# 容器：它们如何在幕后工作以及为何它们正在主导数据科学领域

> 原文：[https://towardsdatascience.com/containers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa?source=collection_archive---------10-----------------------#2023-01-05](https://towardsdatascience.com/containers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa?source=collection_archive---------10-----------------------#2023-01-05)

## 初学者指南：理解 Docker 的魔力

[](https://dpoulopoulos.medium.com/?source=post_page-----6b94702609aa--------------------------------)[![Dimitris Poulopoulos](../Images/ce535a1679779f5a2ec8b024e6691e50.png)](https://dpoulopoulos.medium.com/?source=post_page-----6b94702609aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b94702609aa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6b94702609aa--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----6b94702609aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontainers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----6b94702609aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b94702609aa--------------------------------) ·7 分钟阅读·2023年1月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6b94702609aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontainers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----6b94702609aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6b94702609aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcontainers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa&source=-----6b94702609aa---------------------bookmark_footer-----------)![](../Images/13ff5f2e19441c4680fe692971465852.png)

容器化城市 — 由 Stable Diffusion 生成的图像

Docker 以惊人的速度席卷了世界，原因很简单。轻量级、可移植的容器使得打包和部署应用程序变得容易，确保它们在任何平台上都能一致且可靠地运行。

那么，Docker 容器到底是什么，它们在底层是如何工作的呢？谷歌搜索会给你数百篇关于容器与虚拟机（VMs）比较的文章，但这并没有回答问题。Docker 做了什么？这是他们发明的东西吗？我们可以在没有 Docker、Podman 或其他你可能使用的平台的情况下创建容器吗？

本系列文章将详细解析容器的概念，并解释 Docker 如何利用容器来革新软件的构建和部署方式。我们将了解什么是 Linux 命名空间，如何使用 `cgroups` 限制容器可以使用的资源，以及为什么覆盖文件系统在创建类似容器的环境中扮演着关键角色。

你准备好了解 Docker 容器的魔力了吗？在本系列文章结束时，你将能够在没有 Docker 的情况下创建自己的类似容器环境。

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=namespaces) 是一份关于……
