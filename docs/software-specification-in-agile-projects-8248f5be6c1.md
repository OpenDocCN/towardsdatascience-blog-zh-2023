# 敏捷项目中的软件规范

> 原文：[`towardsdatascience.com/software-specification-in-agile-projects-8248f5be6c1?source=collection_archive---------11-----------------------#2023-03-10`](https://towardsdatascience.com/software-specification-in-agile-projects-8248f5be6c1?source=collection_archive---------11-----------------------#2023-03-10)

## 来自 IBM 最大转型项目的原始见解

[](https://medium.com/@thomas.reinecke?source=post_page-----8248f5be6c1--------------------------------)![托马斯·赖内克](https://medium.com/@thomas.reinecke?source=post_page-----8248f5be6c1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8248f5be6c1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8248f5be6c1--------------------------------) [托马斯·赖内克](https://medium.com/@thomas.reinecke?source=post_page-----8248f5be6c1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F286199bbb1eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsoftware-specification-in-agile-projects-8248f5be6c1&user=Thomas+Reinecke&userId=286199bbb1eb&source=post_page-286199bbb1eb----8248f5be6c1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8248f5be6c1--------------------------------) ·8 分钟阅读·2023 年 3 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8248f5be6c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsoftware-specification-in-agile-projects-8248f5be6c1&user=Thomas+Reinecke&userId=286199bbb1eb&source=-----8248f5be6c1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8248f5be6c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsoftware-specification-in-agile-projects-8248f5be6c1&source=-----8248f5be6c1---------------------bookmark_footer-----------)

传统的瀑布模型和 V 模型软件开发方法依赖于大量的前期时间投入来指定解决方案、集成或功能，通常会导致大量的文档。这些方法由于其不灵活性、高成本和无法适应实时变化，不适用于敏捷流程。

然而，通常认为敏捷团队在没有规格说明的情况下工作，因为他们优先考虑适应变化和交付产品，而不是文档。这是不准确的。即使是敏捷项目，无论其规模大小，仍然需要一定程度的**结构**、**指导**、**治理**和**文档**来确保一致性并防止混乱。对于涉及多个开发小组和数百名工程师的大型项目尤其如此。

本文提供了我们在 IBM 实施的一些最大内部转型项目的模式和方法的真实见解。

![](img/a344581968b33bd67a644f9defcbfbc0.png)

图片由 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/DbLlKd8u2Rw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 为什么需要结构、指导、治理？

假设你正在创建一个复杂的产品，涉及多个小组，那么假设敏捷 Scrum 团队是自给自足的，并且任何需求都可以分配给…
