# 探索 Linux 容器中覆盖文件系统的力量

> 原文：[https://towardsdatascience.com/exploring-the-power-of-overlay-file-systems-in-linux-containers-d846724ec06d?source=collection_archive---------9-----------------------#2023-02-01](https://towardsdatascience.com/exploring-the-power-of-overlay-file-systems-in-linux-containers-d846724ec06d?source=collection_archive---------9-----------------------#2023-02-01)

## 用独特而简单的分层理念解锁容器化的潜力

[](https://dpoulopoulos.medium.com/?source=post_page-----d846724ec06d--------------------------------)[![Dimitris Poulopoulos](../Images/ce535a1679779f5a2ec8b024e6691e50.png)](https://dpoulopoulos.medium.com/?source=post_page-----d846724ec06d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d846724ec06d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d846724ec06d--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----d846724ec06d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-the-power-of-overlay-file-systems-in-linux-containers-d846724ec06d&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----d846724ec06d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d846724ec06d--------------------------------) · 阅读时间 7 分钟 · 2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd846724ec06d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-the-power-of-overlay-file-systems-in-linux-containers-d846724ec06d&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----d846724ec06d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd846724ec06d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-the-power-of-overlay-file-systems-in-linux-containers-d846724ec06d&source=-----d846724ec06d---------------------bookmark_footer-----------)![](../Images/4cc7cae29da36d8e2f7e3699d9915b37.png)

图片由 [Hoach Le Dinh](https://unsplash.com/@hoachld?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/PeRt3uMmjYM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这一系列文章探讨了 Linux 容器在幕后如何运作，以及我们需要哪些工具来构建类似的环境而不依赖 Docker。为什么要这样做？如果你真的想知道执行 `docker run` 时发生了什么，这是你需要迈出的第一步。最后两篇文章关注于 Namespaces 和 Control Groups（控制组）在容器化中的作用。

[](/containers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa?source=post_page-----d846724ec06d--------------------------------) [## 容器：它们如何在幕后运作，以及为何它们正在主导数据科学领域

### 初学者了解 Docker 魔力的指南

towardsdatascience.com](/containers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa?source=post_page-----d846724ec06d--------------------------------) [](/the-power-of-linux-cgroups-how-containers-take-control-of-their-resources-ba564fef13b0?source=post_page-----d846724ec06d--------------------------------) [## Linux 控制组的力量：容器如何掌控其资源

### 使用 Linux 控制组优化容器资源分配

towardsdatascience.com](/the-power-of-linux-cgroups-how-containers-take-control-of-their-resources-ba564fef13b0?source=post_page-----d846724ec06d--------------------------------)

这篇文章总结了这一主题，引入了覆盖文件系统，这是在不依赖 Docker 的情况下创建类似容器环境所需的最后一个构建块。

Linux 容器彻底改变了应用程序的部署方式和…
