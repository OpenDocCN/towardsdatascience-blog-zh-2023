# 解码 LLMs：从零开始在 Python 中创建 Transformer 编码器和多头注意力层

> 原文：[`towardsdatascience.com/decoding-llms-creating-transformer-encoders-and-multi-head-attention-layers-in-python-from-scratch-631429553ce8?source=collection_archive---------2-----------------------#2023-12-01`](https://towardsdatascience.com/decoding-llms-creating-transformer-encoders-and-multi-head-attention-layers-in-python-from-scratch-631429553ce8?source=collection_archive---------2-----------------------#2023-12-01)

## 探索大型语言模型中编码器、多头注意力和位置编码的复杂性

[](https://medium.com/@luisroque?source=post_page-----631429553ce8--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----631429553ce8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----631429553ce8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----631429553ce8--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----631429553ce8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-llms-creating-transformer-encoders-and-multi-head-attention-layers-in-python-from-scratch-631429553ce8&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----631429553ce8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----631429553ce8--------------------------------) ·13 分钟阅读·2023 年 12 月 1 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F631429553ce8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-llms-creating-transformer-encoders-and-multi-head-attention-layers-in-python-from-scratch-631429553ce8&source=-----631429553ce8---------------------bookmark_footer-----------)

*本文由 Rafael Nardi 共同撰写。*

# 介绍

今天，计算自然语言处理（NLP）是一个快速发展的领域，其中计算能力与语言学相结合。其语言学方面主要归功于 John Rupert Firth 的*分布式语义*理论。他曾说过以下话：

*“你可以通过词语的上下文了解它的含义”*

因此，词语的语义表示由其使用的上下文决定。正是基于这一假设，Ashish Vaswani 等人所著的论文“Attention is all you need”[[1]](https://github.com/zaai-ai/large-language-models-math/blob/main/attention_is_all_you_need.md#attention) 才具备了其开创性的意义。它将变换器架构确立为诸如 BERT、GPT4、Llama 等许多快速增长的工具的核心。

在本文中，我们深入探讨了变换器架构中编码器部分核心的关键数学运算。

![](img/4ac257ecfec0d1b7c2570c38e383cfbc.png)

图 1：自注意力机制复杂（图像作者提供）
