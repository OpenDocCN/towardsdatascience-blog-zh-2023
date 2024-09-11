# 在 Transformer 架构中，Query、Key 和 Value 是什么？它们为何被使用？

> 原文：[https://towardsdatascience.com/what-are-query-key-and-value-in-the-transformer-architecture-and-why-are-they-used-acbe73f731f2?source=collection_archive---------2-----------------------#2023-10-05](https://towardsdatascience.com/what-are-query-key-and-value-in-the-transformer-architecture-and-why-are-they-used-acbe73f731f2?source=collection_archive---------2-----------------------#2023-10-05)

## 对 Transformer 架构中 Key、Query 和 Value 概念的直觉分析及其用途。

[](https://ebrahimpichka.medium.com/?source=post_page-----acbe73f731f2--------------------------------)[![Ebrahim Pichka](../Images/8add6e8e875d9e921caf7f5eaa77d545.png)](https://ebrahimpichka.medium.com/?source=post_page-----acbe73f731f2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----acbe73f731f2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----acbe73f731f2--------------------------------) [Ebrahim Pichka](https://ebrahimpichka.medium.com/?source=post_page-----acbe73f731f2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcf08d1e97a71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-query-key-and-value-in-the-transformer-architecture-and-why-are-they-used-acbe73f731f2&user=Ebrahim+Pichka&userId=cf08d1e97a71&source=post_page-cf08d1e97a71----acbe73f731f2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----acbe73f731f2--------------------------------) ·10 min 阅读·2023年10月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Facbe73f731f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-query-key-and-value-in-the-transformer-architecture-and-why-are-they-used-acbe73f731f2&user=Ebrahim+Pichka&userId=cf08d1e97a71&source=-----acbe73f731f2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Facbe73f731f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-query-key-and-value-in-the-transformer-architecture-and-why-are-they-used-acbe73f731f2&source=-----acbe73f731f2---------------------bookmark_footer-----------)![](../Images/36bd08a0784d8f29ebfaca4463809896.png)

图片由作者提供 — 由 [**Midjourney**](https://www.midjourney.com/) 生成

# 介绍

近年来，Transformer 架构在自然语言处理（NLP）领域引起了轰动，在包括机器翻译、语言建模、文本摘要等各种任务中取得了最先进的成果，以及其他 AI 领域，如视觉、语音、强化学习等。

Vaswani 等人（2017）在他们的论文 *《Attention Is All You Need》* 中首次介绍了 Transformer，其中他们使用了自注意力机制，而没有引入递归连接，同时模型可以有选择地关注输入序列的特定部分。

![](../Images/5c8d9226a835a3ef8723261c98b8ff73.png)

Transformer 模型架构——图片来自 Vaswani 等人（2017）的论文（来源：[**arXiv:1706.03762v7**](https://arxiv.org/abs/1706.03762v7)）

特别是，以前的序列模型，如递归编码器-解码器模型，在捕捉长期依赖关系和并行计算方面存在限制。事实上，在 Transformer 论文于 2017 年发表之前，大多数 NLP…
