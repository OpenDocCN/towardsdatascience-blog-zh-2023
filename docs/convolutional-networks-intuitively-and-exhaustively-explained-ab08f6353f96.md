# 卷积网络 — 直观且全面地解释

> 原文：[https://towardsdatascience.com/convolutional-networks-intuitively-and-exhaustively-explained-ab08f6353f96?source=collection_archive---------3-----------------------#2023-10-26](https://towardsdatascience.com/convolutional-networks-intuitively-and-exhaustively-explained-ab08f6353f96?source=collection_archive---------3-----------------------#2023-10-26)

## 解读一个核心建模策略

[](https://medium.com/@danielwarfield1?source=post_page-----ab08f6353f96--------------------------------)[![Daniel Warfield](../Images/c1c8b4dd514f6813e08e401401324bca.png)](https://medium.com/@danielwarfield1?source=post_page-----ab08f6353f96--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab08f6353f96--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ab08f6353f96--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----ab08f6353f96--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-networks-intuitively-and-exhaustively-explained-ab08f6353f96&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----ab08f6353f96---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab08f6353f96--------------------------------) · 11分钟阅读 · 2023年10月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fab08f6353f96&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-networks-intuitively-and-exhaustively-explained-ab08f6353f96&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----ab08f6353f96---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fab08f6353f96&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-networks-intuitively-and-exhaustively-explained-ab08f6353f96&source=-----ab08f6353f96---------------------bookmark_footer-----------)![](../Images/eaf12909ea93c0a6b57e7c294ea991c2.png)

“由作者使用MidJourney生成”。所有图片均由作者提供，除非另有说明。

卷积神经网络是计算机视觉、信号处理和大量其他机器学习任务中的重要组成部分。它们相对直接，因此很多人往往没有*真正*理解它们。在这篇文章中，我们将直观且全面地讲解卷积网络的理论，并探讨它们在一些应用案例中的使用。

**这对谁有用？** 适用于任何对计算机视觉、信号分析或机器学习感兴趣的人。

**这篇文章的难度如何？** 这是一个非常强大但又非常简单的概念，非常适合初学者。这也可能是经验丰富的数据科学家的一个很好的复习，尤其是在考虑各种维度的卷积时。

**前提条件：** 对反向传播和密集神经网络的基本了解可能有用，但不是必需的。我在这篇文章中涵盖了这两点：
