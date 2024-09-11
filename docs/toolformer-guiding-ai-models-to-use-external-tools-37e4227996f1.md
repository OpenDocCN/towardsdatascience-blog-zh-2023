# ToolFormer: 指导 AI 模型使用外部工具

> 原文：[`towardsdatascience.com/toolformer-guiding-ai-models-to-use-external-tools-37e4227996f1?source=collection_archive---------4-----------------------#2023-10-23`](https://towardsdatascience.com/toolformer-guiding-ai-models-to-use-external-tools-37e4227996f1?source=collection_archive---------4-----------------------#2023-10-23)

## Meta 的 LLM 自我学习调用外部 API

[](https://medium.com/@nikoskafritsas?source=post_page-----37e4227996f1--------------------------------)![Nikos Kafritsas](https://medium.com/@nikoskafritsas?source=post_page-----37e4227996f1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----37e4227996f1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----37e4227996f1--------------------------------) [Nikos Kafritsas](https://medium.com/@nikoskafritsas?source=post_page-----37e4227996f1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbec849d9e1d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftoolformer-guiding-ai-models-to-use-external-tools-37e4227996f1&user=Nikos+Kafritsas&userId=bec849d9e1d2&source=post_page-bec849d9e1d2----37e4227996f1---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----37e4227996f1--------------------------------) ·14 min 阅读·2023 年 10 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F37e4227996f1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftoolformer-guiding-ai-models-to-use-external-tools-37e4227996f1&user=Nikos+Kafritsas&userId=bec849d9e1d2&source=-----37e4227996f1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F37e4227996f1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftoolformer-guiding-ai-models-to-use-external-tools-37e4227996f1&source=-----37e4227996f1---------------------bookmark_footer-----------)![](img/180485d9bb3167a1e863f926c118935d.png)

图片由作者使用 Midjourney 创建

**现在尘埃落定，LLMs 的弱点已知。**

即使是强大的 GPT-4 也在数学运算上挣扎。

此外，训练截止时间是每个 LLM 的固有弱点。它们在回答有关新事物的问题时会感到困难。

一个权宜之计是使用外部插件（例如 ChatGPT 插件）。不过，用户仍然需要手动指定一些操作，并且这些插件有时不可靠。

如果有一个模型知道自己的弱点，并且在不确定时能够**原生**调用最佳外部工具，会怎样呢？

这就是 Meta 所做的，通过创建**ToolFormer[1]**。在本文中，我们讨论：

+   什么是 ToolFormer，以及它为什么是一个突破？

+   模型是如何工作的。

+   ToolFormer 的方法论如何应用于任何 LLM。

+   为什么人工智能研究朝着 ToolFormer 的愿景发展。

让我们深入了解。

# 大型语言模型的弱点

在开始描述 ToolFormer 之前，让我们探索现代 LLM 面临的问题：
