# 使用 Python 进行 GANs 实践：图像生成

> 原文：[https://towardsdatascience.com/hands-on-generative-ai-with-gans-using-python-image-generation-9a62e591c7c6?source=collection_archive---------8-----------------------#2023-03-27](https://towardsdatascience.com/hands-on-generative-ai-with-gans-using-python-image-generation-9a62e591c7c6?source=collection_archive---------8-----------------------#2023-03-27)

![](../Images/8bacad55716d720d37ccf3e4e734e940.png)

作者提供的图像

## 学习如何使用 PyTorch 实现 GANs 以生成合成图像

[](https://medium.com/@marcellopoliti?source=post_page-----9a62e591c7c6--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----9a62e591c7c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9a62e591c7c6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9a62e591c7c6--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----9a62e591c7c6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-generative-ai-with-gans-using-python-image-generation-9a62e591c7c6&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----9a62e591c7c6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9a62e591c7c6--------------------------------) · 7 分钟阅读 · 2023年3月27日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9a62e591c7c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-generative-ai-with-gans-using-python-image-generation-9a62e591c7c6&user=Marcello+Politi&userId=7390355d40fe&source=-----9a62e591c7c6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9a62e591c7c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-generative-ai-with-gans-using-python-image-generation-9a62e591c7c6&source=-----9a62e591c7c6---------------------bookmark_footer-----------)

## **简介**

在我[上一篇文章](https://medium.com/towards-data-science/hands-on-generative-ai-with-gans-using-python-autoencoders-c77232b402fc)中，我们学习了自编码器，现在让我们继续讨论生成式 AI。现在每个人都在谈论它，每个人都对已开发的实际应用感到兴奋。但我们仍在一步步了解这些 AI 的基础。

有几种机器学习模型可以用于构建生成式 AI，例如变分自动编码器（VAE）、自回归模型甚至归一化流模型。然而，在本文中，我们将重点关注 GANs。

## 自动编码器与 GANs

在上一篇文章中，我们讨论了自动编码器，了解了它们的架构、用途以及在 PyTorch 中的实现。

简而言之，自动编码器接收输入 x，将其压缩成一个较小尺寸的向量 z，称为潜在向量，然后从 z 以更或少近似的方式重构 x。

在自动编码器中，我们没有数据生成，只是对输入的近似重构。现在，想象一下我们将自动编码器拆分为两部分，仅考虑第二部分，从中…
