# 简化 Transformers：使用你理解的词汇的最先进 NLP——第 1 部分——简介

> 原文：[https://towardsdatascience.com/transformers-part-1-2a2755a2af0e?source=collection_archive---------7-----------------------#2023-07-26](https://towardsdatascience.com/transformers-part-1-2a2755a2af0e?source=collection_archive---------7-----------------------#2023-07-26)

[](https://medium.com/@chenmargalit?source=post_page-----2a2755a2af0e--------------------------------)[![Chen Margalit](../Images/fb37720654b3d1068b448d4d9ad624d5.png)](https://medium.com/@chenmargalit?source=post_page-----2a2755a2af0e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2a2755a2af0e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2a2755a2af0e--------------------------------) [Chen Margalit](https://medium.com/@chenmargalit?source=post_page-----2a2755a2af0e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8e6113b0479&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-part-1-2a2755a2af0e&user=Chen+Margalit&userId=f8e6113b0479&source=post_page-f8e6113b0479----2a2755a2af0e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a2755a2af0e--------------------------------) ·3分钟阅读·2023年7月26日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2a2755a2af0e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-part-1-2a2755a2af0e&source=-----2a2755a2af0e---------------------bookmark_footer-----------)

Transformers 是一种深度学习架构，对 AI 的进步做出了杰出的贡献。它在 AI 和整体技术领域中是一个重要的阶段，但也有点复杂。到目前为止，关于 Transformers 有很多不错的资源，那么为什么还要再做一个呢？两个原因：

1.  我精通自学，从我的经验来看，能够阅读不同人对相同思想的描述，大大增强了理解能力。

1.  我很少读到一篇文章并认为它解释得足够简单。技术内容创作者往往过于复杂化或解释不够充分。应该明确的是，没有什么是火箭科学，即便是火箭科学也不例外。你可以理解任何事物，只需要一个足够好的解释。在这个系列中，我尝试提供足够好的解释。

此外，作为一个将自己的职业生涯归功于文章和开源代码的人，我认为有义务回馈社会。

本系列将尝试为几乎对 AI 一无所知的人**以及**了解机器如何学习的人提供合理的指南。我打算如何做到这一点？首要任务是——解释。我在职业生涯中可能阅读了接近 1000 篇技术论文（如本篇），我面临的主要问题是作者（可能是无意识的）假设你知道很多东西。在本系列中，我计划假设你知道的比我为准备这篇文章而阅读的 Transformer 文章要少。

此外，我将结合直观理解、数学、代码和可视化，因此该系列的设计就像一个糖果店——人人都能找到合适的东西。考虑到这是一个相当复杂领域中的高级概念，我会冒险让你觉得：“哇，这么慢，别解释显而易见的东西”，但如果你对自己说：“他到底在说什么？”的情况会少得多。

## Transformers，值得你花时间了解吗？

这到底是怎么回事？真的这么重要吗？嗯，既然它是一些世界上最先进的 AI 驱动技术工具（例如 GPT 等）的基础，它可能确实重要。

尽管与许多科学进展一样，一些想法以前已经被描述过，但对架构的实际深入、完整描述来自于“[Attention is all you need](https://arxiv.org/pdf/1706.03762.pdf)”论文，该论文声称以下内容是一个“简单的网络架构”。

![](../Images/14f1ba4c207045991554b11ad20ca08a.png)

图片来源于[原始论文](https://proceedings.neurips.cc/paper_files/paper/2017/file/3f5ee243547dee91fbd053c1c4a845aa-Paper.pdf)

如果你像大多数人一样，你**不会**认为这是一种简单的网络架构。因此，我的工作是努力做到，当你读完这系列文章时，你会对自己说：这仍然不简单，但我**确实**明白了。

那么，这个疯狂的图表，究竟是什么？

我们看到的是一种深度学习架构，这意味着每个方块应该被转换为一些代码，而这些代码组合在一起将实现一些目前人们还不知道如何做到的事情。

Transformers 可以应用于许多不同的用例，但可能最著名的是自动化聊天。一个可以谈论很多主题的程序，仿佛它知道很多东西。从某种程度上说，这与[黑客帝国](https://en.wikipedia.org/wiki/The_Matrix)有些相似。

我想让人们只阅读他们实际需要的内容，因此系列将根据我认为 Transformer 故事应该如何讲述来进行拆分。第一部分在[***这里***](https://medium.com/@chenmargalit/transformers-part-2-input-2a8c3a141c7d)，将讨论架构的第一部分**— 输入**。
