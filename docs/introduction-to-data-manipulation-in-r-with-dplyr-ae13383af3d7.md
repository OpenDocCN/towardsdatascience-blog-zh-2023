# R 中的 {dplyr} 数据处理入门

> 原文：[https://towardsdatascience.com/introduction-to-data-manipulation-in-r-with-dplyr-ae13383af3d7?source=collection_archive---------5-----------------------#2023-11-27](https://towardsdatascience.com/introduction-to-data-manipulation-in-r-with-dplyr-ae13383af3d7?source=collection_archive---------5-----------------------#2023-11-27)

## 学习如何使用 R 中的 {dplyr} 包，它帮助你解决最常见的数据处理挑战。

[](https://antoinesoetewey.medium.com/?source=post_page-----ae13383af3d7--------------------------------)[![Antoine Soetewey](../Images/51d7837d18ff15a62cac2343a485e35d.png)](https://antoinesoetewey.medium.com/?source=post_page-----ae13383af3d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ae13383af3d7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ae13383af3d7--------------------------------) [Antoine Soetewey](https://antoinesoetewey.medium.com/?source=post_page-----ae13383af3d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fca32a96e6dc7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-data-manipulation-in-r-with-dplyr-ae13383af3d7&user=Antoine+Soetewey&userId=ca32a96e6dc7&source=post_page-ca32a96e6dc7----ae13383af3d7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae13383af3d7--------------------------------) ·27 min read·2023年11月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fae13383af3d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-data-manipulation-in-r-with-dplyr-ae13383af3d7&user=Antoine+Soetewey&userId=ca32a96e6dc7&source=-----ae13383af3d7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fae13383af3d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-data-manipulation-in-r-with-dplyr-ae13383af3d7&source=-----ae13383af3d7---------------------bookmark_footer-----------)![](../Images/34f6144f88eb797315f364b70afbc1de.png)

图片由 [Claudio Schwarz](https://unsplash.com/@purzlbaum?utm_source=medium&utm_medium=referral) 提供

# 介绍

在之前的一篇文章中，我们展示了如何 [在 R 中处理数据](https://statsandr.com/blog/data-manipulation-in-r/)。特别是，我们说明了如何创建和处理向量、因子、列表和数据框。这作为 R 的入门介绍，面向初学者。此外，只要可能，所有处理都在基础 R 中完成，即无需加载任何包。

在这篇文章中，我们将再次展示如何在R中操作数据，不过这次使用`{dplyr}`包。

`{dplyr}`包由Hadley Wickham及其在posit的同事开发，提供了一整套函数，帮助你解决最常见的数据操作挑战，例如：

+   根据其值过滤观测值

+   根据观测值的值或位置提取观测值

+   根据特定数量或比例抽样观测值

+   根据一个或多个变量对观测值进行排序

+   根据变量的名称或位置选择变量

+   重命名变量
