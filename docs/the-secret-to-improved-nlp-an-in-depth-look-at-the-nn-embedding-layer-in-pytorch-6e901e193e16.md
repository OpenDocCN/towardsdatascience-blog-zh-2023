# 改进 NLP 的秘密：深入了解 PyTorch 中的 nn.Embedding 层

> 原文：[`towardsdatascience.com/the-secret-to-improved-nlp-an-in-depth-look-at-the-nn-embedding-layer-in-pytorch-6e901e193e16?source=collection_archive---------0-----------------------#2023-01-24`](https://towardsdatascience.com/the-secret-to-improved-nlp-an-in-depth-look-at-the-nn-embedding-layer-in-pytorch-6e901e193e16?source=collection_archive---------0-----------------------#2023-01-24)

## 解析 PyTorch 中的 `nn.Embedding` 层以及它的完整工作指南

[](https://medium.com/@will.badr?source=post_page-----6e901e193e16--------------------------------)![Will Badr](https://medium.com/@will.badr?source=post_page-----6e901e193e16--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6e901e193e16--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6e901e193e16--------------------------------) [Will Badr](https://medium.com/@will.badr?source=post_page-----6e901e193e16--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F551ba3f6b67d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-secret-to-improved-nlp-an-in-depth-look-at-the-nn-embedding-layer-in-pytorch-6e901e193e16&user=Will+Badr&userId=551ba3f6b67d&source=post_page-551ba3f6b67d----6e901e193e16---------------------post_header-----------) 发表在 [数据科学的秘密](https://towardsdatascience.com/?source=post_page-----6e901e193e16--------------------------------) · 8 分钟阅读 · 2023 年 1 月 24 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6e901e193e16&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-secret-to-improved-nlp-an-in-depth-look-at-the-nn-embedding-layer-in-pytorch-6e901e193e16&source=-----6e901e193e16---------------------bookmark_footer-----------)![](img/78883177665ed8578c3715ef39211129.png)

OpenAI DALL-E 生成的图像

你可能在多个涉及自然语言处理（NLP）的神经网络架构中见过著名的 PyTorch *nn.Embedding()* 层。设计先进的 NLP 架构时，这是最简单且最重要的层之一。让我用简单的术语来解释一下它是什么。

在花了一些时间查看其 C++ 源代码后，我发现了以下内容。*nn.Embedding* 层是一个简单的查找表，它将索引值映射到某个维度的权重矩阵。这个简单的操作是许多高级 NLP 架构的基础，使得在连续空间中处理离散输入符号成为可能。在训练过程中，神经网络中 *nn.Embedding* 层的参数会被调整，以优化模型的性能。具体来说，通过反向传播更新嵌入矩阵，以最小化损失函数。这可以被视为从离散输入标记（如单词）到高维空间中连续嵌入向量的映射学习，其中向量被优化以表示输入标记的含义或上下文，关联到任务的…
