# 图像分类与 Vision Transformer

> 原文：[https://towardsdatascience.com/image-classification-with-vision-transformer-8bfde8e541d4?source=collection_archive---------0-----------------------#2023-04-13](https://towardsdatascience.com/image-classification-with-vision-transformer-8bfde8e541d4?source=collection_archive---------0-----------------------#2023-04-13)

## 如何借助基于 Transformer 的模型进行图像分类

[](https://medium.com/@marcellusruben?source=post_page-----8bfde8e541d4--------------------------------)[![Ruben Winastwan](../Images/15ad0dd03bf5892510abdf166a1e91e1.png)](https://medium.com/@marcellusruben?source=post_page-----8bfde8e541d4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8bfde8e541d4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8bfde8e541d4--------------------------------) [Ruben Winastwan](https://medium.com/@marcellusruben?source=post_page-----8bfde8e541d4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5dae9da73c9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-classification-with-vision-transformer-8bfde8e541d4&user=Ruben+Winastwan&userId=5dae9da73c9b&source=post_page-5dae9da73c9b----8bfde8e541d4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8bfde8e541d4--------------------------------) · 13分钟阅读 · 2023年4月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8bfde8e541d4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-classification-with-vision-transformer-8bfde8e541d4&user=Ruben+Winastwan&userId=5dae9da73c9b&source=-----8bfde8e541d4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8bfde8e541d4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-classification-with-vision-transformer-8bfde8e541d4&source=-----8bfde8e541d4---------------------bookmark_footer-----------)![](../Images/a404a96d42476c650eab290a3eb6734f.png)

图片来源：[drmakete lab](https://unsplash.com/@drmakete?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/hsg538WrP0Y?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

自2017年推出以来，Transformer 被广泛认可为解决几乎任何语言建模任务的强大编码器-解码器模型。

BERT、RoBERTa 和 XLM-RoBERTa 是一些在语言处理领域中使用 Transformer 编码器堆叠作为其架构核心的最先进模型的例子。ChatGPT 和 GPT 系列也使用 Transformer 的解码器部分来生成文本。可以说，几乎任何最先进的自然语言处理模型都将 Transformer 融入其架构中。

Transformer 的表现如此出色，以至于不将其用于自然语言处理之外的任务似乎有些浪费，比如计算机视觉。然而，关键问题是：我们是否真的可以将其用于计算机视觉任务？

事实证明，Transformer 也在计算机视觉任务中展现了良好的潜力。2020年，谷歌大脑团队介绍了一种基于 Transformer 的模型，称为视觉 Transformer（ViT），它可以用于解决图像分类任务。与传统的 CNN 相比，它在多个图像分类基准测试中的表现非常具有竞争力。
