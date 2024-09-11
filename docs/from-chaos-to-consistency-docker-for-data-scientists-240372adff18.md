# 数据科学中的 Docker

> 原文：[`towardsdatascience.com/from-chaos-to-consistency-docker-for-data-scientists-240372adff18?source=collection_archive---------12-----------------------#2023-05-24`](https://towardsdatascience.com/from-chaos-to-consistency-docker-for-data-scientists-240372adff18?source=collection_archive---------12-----------------------#2023-05-24)

## 对数据科学家的 Docker 介绍和应用

[](https://medium.com/@egorhowell?source=post_page-----240372adff18--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----240372adff18--------------------------------)[](https://towardsdatascience.com/?source=post_page-----240372adff18--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----240372adff18--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----240372adff18--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-chaos-to-consistency-docker-for-data-scientists-240372adff18&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----240372adff18---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----240372adff18--------------------------------) ·7 分钟阅读·2023 年 5 月 24 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F240372adff18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-chaos-to-consistency-docker-for-data-scientists-240372adff18&source=-----240372adff18---------------------bookmark_footer-----------)![](img/b9ddc7ca1478ae7fd299d341e9610337.png)

照片由 [Ian Taylor](https://unsplash.com/@carrier_lost?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄

# 背景

> 但在我的机器上可以运行？

这是科技界的经典笑话，特别是对于那些想要部署他们出色的机器学习模型的数据科学家，却发现生产环境中的操作系统不同。远非理想。

然而…

这得益于这些奇妙的东西，[**容器**](https://www.docker.com/resources/what-container/)和用于控制它们的工具，如[**Docker**](https://en.wikipedia.org/wiki/Docker_%28software%29)**。**

在这篇文章中，我们将深入探讨容器是什么，以及如何使用 Docker 构建和运行它们。容器和 Docker 的使用已经成为数据产品的行业标准和常见实践。作为一名数据科学家，学习这些工具无疑是你工具箱中的一项宝贵资产。

# 什么是 Docker？

> Docker 是一种服务，帮助在容器中构建、运行和执行代码及应用程序。

现在你可能会想，什么是容器？

表面上，容器与[**虚拟机 (VM)**](https://en.wikipedia.org/wiki/Virtual_machine)非常相似。它是一个小型的隔离环境，所有东西都被自我“包含”，可以在任何…
