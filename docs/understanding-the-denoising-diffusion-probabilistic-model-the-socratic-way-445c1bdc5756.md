# 以**苏格拉底式的方法**理解去噪扩散概率模型（DDPMs）

> 原文：[https://towardsdatascience.com/understanding-the-denoising-diffusion-probabilistic-model-the-socratic-way-445c1bdc5756?source=collection_archive---------2-----------------------#2023-02-25](https://towardsdatascience.com/understanding-the-denoising-diffusion-probabilistic-model-the-socratic-way-445c1bdc5756?source=collection_archive---------2-----------------------#2023-02-25)

## 深入探讨去噪扩散模型背后的动机及损失函数的详细推导

[](https://jasonweiyi.medium.com/?source=post_page-----445c1bdc5756--------------------------------)[![Wei Yi](../Images/24b7a438912082519f24d18e11ac9638.png)](https://jasonweiyi.medium.com/?source=post_page-----445c1bdc5756--------------------------------)[](https://towardsdatascience.com/?source=post_page-----445c1bdc5756--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----445c1bdc5756--------------------------------) [魏毅](https://jasonweiyi.medium.com/?source=post_page-----445c1bdc5756--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1b4bd5317a6e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-denoising-diffusion-probabilistic-model-the-socratic-way-445c1bdc5756&user=Wei+Yi&userId=1b4bd5317a6e&source=post_page-1b4bd5317a6e----445c1bdc5756---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----445c1bdc5756--------------------------------) ·69分钟阅读·2023年2月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F445c1bdc5756&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-denoising-diffusion-probabilistic-model-the-socratic-way-445c1bdc5756&user=Wei+Yi&userId=1b4bd5317a6e&source=-----445c1bdc5756---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F445c1bdc5756&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-denoising-diffusion-probabilistic-model-the-socratic-way-445c1bdc5756&source=-----445c1bdc5756---------------------bookmark_footer-----------)![](../Images/207ffc71c7a7072f719d371f7f6f951f.png)

照片由 [Chaozzy Lin](https://unsplash.com/@chaozzy?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[去噪扩散概率模型](https://arxiv.org/pdf/2006.11239.pdf) 由 Jonathan Ho 等人撰写，是一篇很棒的论文。但我在理解上遇到困难。因此，我决定深入研究模型并完成所有推导。在这篇文章中，我将重点关注理解论文的两个主要障碍。

1.  为什么去噪扩散模型被设计为正向过程、正向过程后验和反向过程？这些过程之间有什么关系？顺便提一下，在这篇文章中，我称正向过程后验为“正向过程的反向”，因为我发现“后验”这个词让我困惑，或者下意识地想避免这个词，因为每次出现时，事情就变得复杂起来。

1.  如何推导神秘的损失函数。在论文中，推导损失函数 *Lₛᵢₘₚₗₑ* 时有许多步骤被跳过了。我通过所有推导填补了这些缺失的步骤。现在我意识到 *Lₛᵢₘₚₗₑ* 的解析公式推导讲述了一个真正美丽的贝叶斯故事。经过填补所有步骤后，整个故事……
