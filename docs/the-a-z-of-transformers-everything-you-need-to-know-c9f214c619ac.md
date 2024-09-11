# Transformers 的 A-Z：你需要知道的一切

> 原文：[https://towardsdatascience.com/the-a-z-of-transformers-everything-you-need-to-know-c9f214c619ac?source=collection_archive---------1-----------------------#2023-10-25](https://towardsdatascience.com/the-a-z-of-transformers-everything-you-need-to-know-c9f214c619ac?source=collection_archive---------1-----------------------#2023-10-25)

## 关于 Transformers 的所有必要信息，以及如何实现它们

[](https://medium.com/@francoisporcher?source=post_page-----c9f214c619ac--------------------------------)[![François Porcher](../Images/9ddb233f8cadbd69026bd79e2bd62dea.png)](https://medium.com/@francoisporcher?source=post_page-----c9f214c619ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c9f214c619ac--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c9f214c619ac--------------------------------) [François Porcher](https://medium.com/@francoisporcher?source=post_page-----c9f214c619ac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e8e73046f53&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-a-z-of-transformers-everything-you-need-to-know-c9f214c619ac&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=post_page-8e8e73046f53----c9f214c619ac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c9f214c619ac--------------------------------) ·16分钟阅读·2023年10月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc9f214c619ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-a-z-of-transformers-everything-you-need-to-know-c9f214c619ac&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=-----c9f214c619ac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc9f214c619ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-a-z-of-transformers-everything-you-need-to-know-c9f214c619ac&source=-----c9f214c619ac---------------------bookmark_footer-----------)![](../Images/be68574b989226c3e6ac62c933050cf5.png)

作者提供的图片

# 为什么还要写一个关于 Transformers 的教程？

你可能已经听说过 Transformers，每个人都在谈论它，那么为什么还要写一篇新的文章呢？

好吧，我是一名研究人员，这要求我对所使用的工具有非常深入的理解（因为如果你不了解它们，你怎么能找出它们的错误在哪里以及如何改进它们，对吧？）。

当我深入探索Transformers的世界时，发现自己被大量资源淹没。尽管如此，尽管阅读了很多材料，我对架构的理解仍然只是一般的，并且留下了一系列悬而未决的问题。

在本指南中，我旨在弥补这些知识差距。这是一份将为你提供关于Transformers的强大直觉、对其架构的深入分析，以及从零实现的指南。

我强烈建议你跟随[Github](https://github.com/FrancoisPorcher/awesome-ai-tutorials/tree/main/NLP/007%20-%20Transformers%20From%20Scratch)上的代码：
