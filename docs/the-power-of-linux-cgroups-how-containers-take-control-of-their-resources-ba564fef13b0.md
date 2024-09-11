# Linux 控制组的力量：容器如何控制自己的资源

> 原文：[`towardsdatascience.com/the-power-of-linux-cgroups-how-containers-take-control-of-their-resources-ba564fef13b0?source=collection_archive---------6-----------------------#2023-01-10`](https://towardsdatascience.com/the-power-of-linux-cgroups-how-containers-take-control-of-their-resources-ba564fef13b0?source=collection_archive---------6-----------------------#2023-01-10)

## 使用 Linux 控制组优化容器资源分配

[](https://dpoulopoulos.medium.com/?source=post_page-----ba564fef13b0--------------------------------)![Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----ba564fef13b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ba564fef13b0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba564fef13b0--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----ba564fef13b0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-linux-cgroups-how-containers-take-control-of-their-resources-ba564fef13b0&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----ba564fef13b0---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba564fef13b0--------------------------------) ·8 分钟阅读·2023 年 1 月 10 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fba564fef13b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-linux-cgroups-how-containers-take-control-of-their-resources-ba564fef13b0&source=-----ba564fef13b0---------------------bookmark_footer-----------)![](img/b0e5159a60f501ca4b4aa41481bdd327.png)

照片由[Joshua Hoehne](https://unsplash.com/@mrthetrain?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

上一篇文章探讨了如何使用 Linux Namespaces 在单一 Linux 系统中创建隔离环境。这篇文章是我们深入了解容器工作原理的努力的一部分，通过揭示其内部机制。

[](/containers-how-they-work-under-the-hood-and-why-theyre-taking-over-the-data-science-world-6b94702609aa?source=post_page-----ba564fef13b0--------------------------------) ## 容器：它们如何在幕后运作以及为何它们正在主导数据科学领域

### 初学者指南：理解 Docker 的魔力

[towardsdatascience.com

命名空间是我们旅程的第一步。我们看到如何创建一个`PID`命名空间，构建一个世界，其中运行的进程假设它们是唯一存在的，但如何限制它们可以消耗的资源数量呢？这时就需要 Linux cgroups。

Linux 控制组，或称 cgroups，是在 Linux 系统中管理和分配资源的强大工具。它们允许管理员限制进程或进程组使用的资源，确保关键系统服务始终能够获得运行所需的资源。
