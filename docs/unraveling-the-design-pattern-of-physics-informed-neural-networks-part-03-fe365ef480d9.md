# 解开物理信息神经网络的设计模式：第三部分

> 原文：[`towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9?source=collection_archive---------11-----------------------#2023-05-25`](https://towardsdatascience.com/unraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9?source=collection_archive---------11-----------------------#2023-05-25)

## 利用梯度提升训练超充 PINN 的性能

[](https://shuaiguo.medium.com/?source=post_page-----fe365ef480d9--------------------------------)![Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----fe365ef480d9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe365ef480d9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe365ef480d9--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----fe365ef480d9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----fe365ef480d9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe365ef480d9--------------------------------) · 7 分钟阅读 · 2023 年 5 月 25 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffe365ef480d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----fe365ef480d9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffe365ef480d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funraveling-the-design-pattern-of-physics-informed-neural-networks-part-03-fe365ef480d9&source=-----fe365ef480d9---------------------bookmark_footer-----------)![](img/6135269a240dcb1660a418ff2ece06d3.png)

图片由 [Haithem Ferdi](https://unsplash.com/@haithemfrd_off?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

欢迎来到本系列的第三篇博客，我们将继续探索物理信息神经网络（PINN）的设计模式。

在这篇博客中，我们将探讨如何使用梯度提升来训练 PINN，这是一种神经网络与梯度提升算法的激动人心的融合🚀。

一如既往，我将按以下方式组织这篇博客：

+   **问题**，即提出的策略试图解决的具体问题；

+   **解决方案**，即提出策略的关键组成部分，它是如何实施的，以及为什么可能有效；

+   **基准**，即评估了哪些物理问题，以及相关的性能如何；

+   **优点与缺点**，提出策略在何种条件下有效，同时也突出其潜在的局限性；

+   **替代方案**，即为解决类似问题而提出的其他方法，从而提供了潜在解决方案的更广阔视角。

> 随着这一系列的不断扩展，PINN 设计模式的集合变得更加丰富*🙌* 这里是对即将到来的内容的一个小预览……
