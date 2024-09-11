# 使用分支定界法寻找最优解

> 原文：[https://towardsdatascience.com/finding-optimal-solutions-with-branch-and-bound-70a64692a0dd?source=collection_archive---------5-----------------------#2023-12-05](https://towardsdatascience.com/finding-optimal-solutions-with-branch-and-bound-70a64692a0dd?source=collection_archive---------5-----------------------#2023-12-05)

![](../Images/fb71f12a4ae02bdeddd651a2c8aa6cd8.png)

Robocat 和猫咪一起玩耍。图像由作者使用 Dall·E 创建。

## 解决离散优化问题的强大算法

[](https://hennie-de-harder.medium.com/?source=post_page-----70a64692a0dd--------------------------------)[![Hennie de Harder](../Images/3e4f2cccd6cb976ca3f8bf15597daea8.png)](https://hennie-de-harder.medium.com/?source=post_page-----70a64692a0dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----70a64692a0dd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----70a64692a0dd--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----70a64692a0dd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-optimal-solutions-with-branch-and-bound-70a64692a0dd&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----70a64692a0dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----70a64692a0dd--------------------------------) · 8 分钟阅读 · 2023年12月5日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F70a64692a0dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-optimal-solutions-with-branch-and-bound-70a64692a0dd&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----70a64692a0dd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F70a64692a0dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-optimal-solutions-with-branch-and-bound-70a64692a0dd&source=-----70a64692a0dd---------------------bookmark_footer-----------)

**分支定界法是许多混合整数规划 (MIP) 求解器的核心算法。它是你的数学优化工具包中的绝佳补充，特别适用于较小的问题或当问题有许多约束时。此外，它的简单性使其易于使用，不需要复杂的数学公式。**

在这篇动手实践的文章中，我们将深入探讨一个数学优化问题。我们将使用分支界限算法来解决这个问题，这是一种解决此类问题的极佳技术。我们将重点讨论一个以猫为主题的问题——因为说实话，谁不喜欢猫呢？不过，如果你更喜欢狗，每当你看到“猫”时，可以在脑海中替换成“狗”。原理和方法都是一样的！

# 问题介绍

想象一下你是一个猫收容所的老板。每天，宠物主人可以带来他们的猫，你负责照顾它们。许多人在COVID期间收养了猫，但现在每个人都需要回到办公室。因此，你的公司发展得非常好。

实际上，可能有点过于复杂。你在将所有的猫安置到你的房间中遇到了困难……
