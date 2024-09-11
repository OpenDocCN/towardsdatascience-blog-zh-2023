# 你应该使用 Slots 吗？Slots 如何影响你的类，以及何时和如何使用它们

> 原文：[https://towardsdatascience.com/should-you-use-slots-how-slots-affect-your-class-when-and-how-to-use-ab3f118abc71?source=collection_archive---------6-----------------------#2023-08-12](https://towardsdatascience.com/should-you-use-slots-how-slots-affect-your-class-when-and-how-to-use-ab3f118abc71?source=collection_archive---------6-----------------------#2023-08-12)

## 一行代码带来 20% 的性能提升？

[](https://mikehuls.medium.com/?source=post_page-----ab3f118abc71--------------------------------)[![Mike Huls](../Images/8f9f55a0d25db00799c5d37383b7f5b6.png)](https://mikehuls.medium.com/?source=post_page-----ab3f118abc71--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab3f118abc71--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ab3f118abc71--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----ab3f118abc71--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-you-use-slots-how-slots-affect-your-class-when-and-how-to-use-ab3f118abc71&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----ab3f118abc71---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab3f118abc71--------------------------------) ·6 分钟阅读·2023年8月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fab3f118abc71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-you-use-slots-how-slots-affect-your-class-when-and-how-to-use-ab3f118abc71&user=Mike+Huls&userId=7ffb62c607ee&source=-----ab3f118abc71---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fab3f118abc71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-you-use-slots-how-slots-affect-your-class-when-and-how-to-use-ab3f118abc71&source=-----ab3f118abc71---------------------bookmark_footer-----------)![](../Images/a7016fdf3f746ca53c9033b17ddc4026.png)

(图片由 [Sébastien Goldberg](https://unsplash.com/@sebastiengoldberg) 提供，来源于 [Unsplash](https://unsplash.com/photos/J8N79W2EdB8))

Slots 是一种机制，它允许你声明类属性并限制其他属性的创建。你可以确定你的类具有哪些属性，从而防止开发者动态添加新属性。这通常会导致**20% 的速度提升**。

Slots在程序中尤其有用，特别是当你有大量具有已知属性的类实例时。想想视频游戏或物理模拟；在这些情况下，你需要跟踪大量的实体。

你可以通过**添加一行代码**来将slots添加到你的类中，但这总是一个好主意吗？在本文中，我们将探讨**为什么**和**如何**使用**slots**来使你的类更快，以及何时使用它们。总体目标是更好地理解Python类的内部工作原理。让我们开始编码吧！

# Slots使Python类更快

你可以通过让类使用`slots`来提高其内存使用效率和性能。一个使用了slots的类会占用更少的内存并执行得更快。

## 如何让我的类使用slots？
