# 层次化变换器 — 第 1 部分

> 原文：[https://towardsdatascience.com/hierarchical-transformers-54f6d59fa8fc?source=collection_archive---------5-----------------------#2023-10-04](https://towardsdatascience.com/hierarchical-transformers-54f6d59fa8fc?source=collection_archive---------5-----------------------#2023-10-04)

## 更高效的语言模型

[](https://medium.com/@mina.ghashami?source=post_page-----54f6d59fa8fc--------------------------------)[![Mina Ghashami](../Images/745f53b94f5667a485299b49913c7a21.png)](https://medium.com/@mina.ghashami?source=post_page-----54f6d59fa8fc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54f6d59fa8fc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----54f6d59fa8fc--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----54f6d59fa8fc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhierarchical-transformers-54f6d59fa8fc&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----54f6d59fa8fc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----54f6d59fa8fc--------------------------------) · 9 分钟阅读 · 2023 年 10 月 4 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F54f6d59fa8fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhierarchical-transformers-54f6d59fa8fc&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----54f6d59fa8fc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F54f6d59fa8fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhierarchical-transformers-54f6d59fa8fc&source=-----54f6d59fa8fc---------------------bookmark_footer-----------)![](../Images/1b3d6e9ce3c3e6c0dec3c8326bd0faf9.png)

图片来自 [unsplash.com](https://unsplash.com/photos/x22UAIdif_k)

在这篇文章中，我们将探讨层次化变换器：它们是什么，如何工作，它们与标准变换器有何不同以及它们的好处。让我们开始吧。

# 什么是层次化变换器

> “层次化变换器”指的是一种在输入序列的多个尺度或分辨率上操作的变换器架构。

## **我们为什么需要层次化变换器？**

尽管标准的 Transformers 非常惊艳，但它们却非常耗时。 Transformers 内部的注意力机制在处理 n 个 token 的输入序列时需要 O(n²) 的时间。这意味着 Transformers 对于长序列并不实用。解决这种低效的方法之一是采用层次化的 Transformers。这是唯一的方法吗？不是！另一种方法是提高注意力机制的效率。但这将是另一个话题。

## 层次结构在 Transformers 中如何发挥作用？

层次化 Transformers 使模型能够在输入的不同层次上操作，例如：词语、句子、段落等。这也符合人类处理文本的方式。这迫使注意力机制在不同的层次之间建模关系……
