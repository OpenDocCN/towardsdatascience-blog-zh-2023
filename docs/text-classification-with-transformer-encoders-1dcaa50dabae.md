# 使用 Transformer 编码器进行文本分类

> 原文：[https://towardsdatascience.com/text-classification-with-transformer-encoders-1dcaa50dabae?source=collection_archive---------4-----------------------#2023-08-11](https://towardsdatascience.com/text-classification-with-transformer-encoders-1dcaa50dabae?source=collection_archive---------4-----------------------#2023-08-11)

## 利用 Transformer 编码器进行文本分类的逐步说明

[](https://medium.com/@marcellusruben?source=post_page-----1dcaa50dabae--------------------------------)[![Ruben Winastwan](../Images/15ad0dd03bf5892510abdf166a1e91e1.png)](https://medium.com/@marcellusruben?source=post_page-----1dcaa50dabae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1dcaa50dabae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1dcaa50dabae--------------------------------) [Ruben Winastwan](https://medium.com/@marcellusruben?source=post_page-----1dcaa50dabae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5dae9da73c9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-classification-with-transformer-encoders-1dcaa50dabae&user=Ruben+Winastwan&userId=5dae9da73c9b&source=post_page-5dae9da73c9b----1dcaa50dabae---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1dcaa50dabae--------------------------------) · 15 分钟阅读 · 2023年8月11日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1dcaa50dabae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-classification-with-transformer-encoders-1dcaa50dabae&user=Ruben+Winastwan&userId=5dae9da73c9b&source=-----1dcaa50dabae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1dcaa50dabae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-classification-with-transformer-encoders-1dcaa50dabae&source=-----1dcaa50dabae---------------------bookmark_footer-----------)![](../Images/e7af55b2adc4de88204e68d923263996.png)

图片由 [Mel Poole](https://unsplash.com/@melpoole?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/lBsvzgYnzPU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Transformer 无疑是深度学习领域最重要的突破之一。该模型的编码器-解码器架构在跨领域应用中表现出强大的能力。

最初，Transformer 仅用于语言建模任务，如机器翻译、文本生成、文本分类、问答等。然而，最近 Transformer 也被用于计算机视觉任务，如图像分类、物体检测和语义分割。

鉴于其受欢迎程度以及存在许多基于 Transformer 的复杂模型，如 BERT、Vision-Transformer、Swin-Transformer 和 GPT 系列，我们必须深入了解 Transformer 架构的内部工作原理。

在本文中，我们将仅剖析 Transformer 的编码器部分，该部分主要用于分类目的。具体来说，我们将使用 Transformer 编码器来对文本进行分类。事不宜迟，让我们首先看看本文将使用的数据集。

# 关于数据集
