# 精通容器化：创建类似 Docker 的环境的指南

> 原文：[https://towardsdatascience.com/mastering-containerization-a-guide-to-creating-docker-like-environments-without-docker-121b3f444d2f?source=collection_archive---------3-----------------------#2023-02-04](https://towardsdatascience.com/mastering-containerization-a-guide-to-creating-docker-like-environments-without-docker-121b3f444d2f?source=collection_archive---------3-----------------------#2023-02-04)

## 解锁容器化的力量：一步一步教程，教你构建类似容器的环境。

[](https://dpoulopoulos.medium.com/?source=post_page-----121b3f444d2f--------------------------------)[![Dimitris Poulopoulos](../Images/ce535a1679779f5a2ec8b024e6691e50.png)](https://dpoulopoulos.medium.com/?source=post_page-----121b3f444d2f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----121b3f444d2f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----121b3f444d2f--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----121b3f444d2f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-containerization-a-guide-to-creating-docker-like-environments-without-docker-121b3f444d2f&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----121b3f444d2f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----121b3f444d2f--------------------------------) ·6 分钟阅读·2023年2月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F121b3f444d2f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-containerization-a-guide-to-creating-docker-like-environments-without-docker-121b3f444d2f&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----121b3f444d2f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F121b3f444d2f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-containerization-a-guide-to-creating-docker-like-environments-without-docker-121b3f444d2f&source=-----121b3f444d2f---------------------bookmark_footer-----------)![](../Images/8974063124ae85ec620839fea9da5396.png)

照片由 [Timelab Pro](https://unsplash.com/it/@timelabpro?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

容器化彻底改变了我们部署和管理应用程序的方式，提供了无与伦比的可移植性、可扩展性和一致性。

然而，你不应该被 Docker 的光鲜外表吓到——是时候深入了解使容器化成为可能的机制了。通过了解 Docker 的内部工作原理，你将对这项技术有更深的欣赏，并对操作系统有更广泛的理解。

本系列的最后三篇文章铺平了道路。我们讨论了 Namespace、控制组 (cgroups) 和叠加文件系统。这些是我们今天用来创建容器类环境的构建块。

[](/containers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa?source=post_page-----121b3f444d2f--------------------------------) [## 容器：它们如何在后台工作以及为何它们在数据科学世界中占据主导地位

### 初学者理解 Docker 神奇的指南

[towardsdatascience.com](/containers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa?source=post_page-----121b3f444d2f--------------------------------)
