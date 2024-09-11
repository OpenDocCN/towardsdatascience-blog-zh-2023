# 你暂时不需要数据域

> 原文：[`towardsdatascience.com/you-dont-need-data-domains-yet-23af8ffc3e69?source=collection_archive---------17-----------------------#2023-05-23`](https://towardsdatascience.com/you-dont-need-data-domains-yet-23af8ffc3e69?source=collection_archive---------17-----------------------#2023-05-23)

## 揭开数据网格域的神秘面纱

[](https://medium.com/@louise.de.leyritz?source=post_page-----23af8ffc3e69--------------------------------)![Louise de Leyritz](https://medium.com/@louise.de.leyritz?source=post_page-----23af8ffc3e69--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23af8ffc3e69--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----23af8ffc3e69--------------------------------) [Louise de Leyritz](https://medium.com/@louise.de.leyritz?source=post_page-----23af8ffc3e69--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa926de8a6b3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyou-dont-need-data-domains-yet-23af8ffc3e69&user=Louise+de+Leyritz&userId=a926de8a6b3f&source=post_page-a926de8a6b3f----23af8ffc3e69---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23af8ffc3e69--------------------------------) ·7 分钟阅读·2023 年 5 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F23af8ffc3e69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyou-dont-need-data-domains-yet-23af8ffc3e69&user=Louise+de+Leyritz&userId=a926de8a6b3f&source=-----23af8ffc3e69---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23af8ffc3e69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyou-dont-need-data-domains-yet-23af8ffc3e69&source=-----23af8ffc3e69---------------------bookmark_footer-----------)![](img/64bb5602719305309c55e5ca400d12e4.png)

你暂时不需要数据域 — 图片来源于 [Castor](https://www.castordoc.com)

数据网格范式提倡使用数据域作为将数据划分为有意义组的一种方式。数据域最初是为了帮助创建明确的所有权结构并实现更好的数据发现。

虽然这种方法可以有效，但也可能会造成混淆。我已经阅读了关于数据网格域的资料一年了，仍然无法完全理解这个概念。因此，我一直在想：我们能否在不涉及数据域的情况下实现更好的所有权和发现？

在某种程度上，我们可以做到。

在这篇文章中，我旨在澄清数据域的目的，并探讨实现这些目标的更简单的替代方法。

数据领域本质上只是组织内数据分组的一种方式。我们为什么要对数据进行分区呢？原因有三：

+   使人们更容易找到他们需要的数据。

+   以便在出现问题时，能够明确分配所有权和责任。

+   提供必要的背景信息，以更好地理解数据。

好消息是，大多数公司无需考虑领域概念就能解决这三个要素。所以，停止阅读你能找到的每一篇文章吧…
