# 为什么说某种程度上是有免费午餐

> 原文：[https://towardsdatascience.com/why-there-kind-of-is-free-lunch-56f3d3c4279f?source=collection_archive---------6-----------------------#2023-06-27](https://towardsdatascience.com/why-there-kind-of-is-free-lunch-56f3d3c4279f?source=collection_archive---------6-----------------------#2023-06-27)

## 关于神经科学和人工智能中模式的普遍性

[](https://manuel-brenner.medium.com/?source=post_page-----56f3d3c4279f--------------------------------)[![Manuel Brenner](../Images/f62843c79a9b378494cb83caf3ddc792.png)](https://manuel-brenner.medium.com/?source=post_page-----56f3d3c4279f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----56f3d3c4279f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----56f3d3c4279f--------------------------------) [Manuel Brenner](https://manuel-brenner.medium.com/?source=post_page-----56f3d3c4279f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fde95441432&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-there-kind-of-is-free-lunch-56f3d3c4279f&user=Manuel+Brenner&userId=1fde95441432&source=post_page-1fde95441432----56f3d3c4279f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----56f3d3c4279f--------------------------------) · 11分钟阅读 · 2023年6月27日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F56f3d3c4279f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-there-kind-of-is-free-lunch-56f3d3c4279f&user=Manuel+Brenner&userId=1fde95441432&source=-----56f3d3c4279f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F56f3d3c4279f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-there-kind-of-is-free-lunch-56f3d3c4279f&source=-----56f3d3c4279f---------------------bookmark_footer-----------)

> **‘没有免费的午餐。’
> 
> - 罗伯特·A·亨莱因**

机器学习领域的“无免费午餐”定理让我想起了数学界的哥德尔不完全性定理。

尽管这些定理经常被引用，但它们很少被深入解释，而且对实际应用的影响通常仍然不明确。就像哥德尔定理成为20世纪初数学家对完整且自洽的形式系统信念中的一个障碍一样，“无免费午餐”定理挑战了我们对通用机器学习算法有效性的信仰。然而，这些定理对日常实际应用的影响通常较小，大多数从业者在这些理论限制下依然畅行无阻。

![](../Images/9b23f215331e0286b91dc5fbb0799a27.png)

机器学习者意识到可能还是有免费的午餐，如DALL-E所设想的那样。

在本文中，我想探讨一下**“无免费午餐”**定理的内容，并深入研究它与视觉、迁移学习、神经科学以及人工通用智能的关联。

**“无免费午餐”**定理，[由Wolpert和Macready于1997年提出](https://ieeexplore.ieee.org/document/585893)，并且常用于机器学习的背景下，**指出没有一种算法能在所有可能的问题中都是最好的**。没有魔法般的…
