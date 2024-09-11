# 论文解析——基于潜在扩散模型的高分辨率图像合成

> 原文：[`towardsdatascience.com/paper-explained-high-resolution-image-synthesis-with-latent-diffusion-models-f372f7636d42?source=collection_archive---------1-----------------------#2023-03-30`](https://towardsdatascience.com/paper-explained-high-resolution-image-synthesis-with-latent-diffusion-models-f372f7636d42?source=collection_archive---------1-----------------------#2023-03-30)

## 尽管 OpenAI 在自然语言处理领域凭借其生成文本模型占据了主导地位，但其图像生成对应物 DALL·E 现在面临着一个有效的开源竞争者——Stable Diffusion。本文深入探讨了基于 Latent Diffusion 的 Stable Diffusion 论文。

[](https://mnslarcher.medium.com/?source=post_page-----f372f7636d42--------------------------------)![Mario Larcher](https://mnslarcher.medium.com/?source=post_page-----f372f7636d42--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f372f7636d42--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f372f7636d42--------------------------------) [Mario Larcher](https://mnslarcher.medium.com/?source=post_page-----f372f7636d42--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd2b72f39ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpaper-explained-high-resolution-image-synthesis-with-latent-diffusion-models-f372f7636d42&user=Mario+Larcher&userId=cd2b72f39ad4&source=post_page-cd2b72f39ad4----f372f7636d42---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f372f7636d42--------------------------------) · 10 分钟阅读 · 2023 年 3 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff372f7636d42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpaper-explained-high-resolution-image-synthesis-with-latent-diffusion-models-f372f7636d42&user=Mario+Larcher&userId=cd2b72f39ad4&source=-----f372f7636d42---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff372f7636d42&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpaper-explained-high-resolution-image-synthesis-with-latent-diffusion-models-f372f7636d42&source=-----f372f7636d42---------------------bookmark_footer-----------)![](img/d785dcaef1afb14a4814109f8991004c.png)

来自 [基于潜在扩散模型的高分辨率图像合成](https://arxiv.org/abs/2112.10752) 的图 13 部分，生成的提示为“潜在空间的油画”。

# 引言

写这篇文章的时候，OpenAI 的聊天机器人 ChatGPT 在 Microsoft 产品中得到了广泛应用，该产品已被超过十亿人使用。尽管 Google 最近也推出了自己的 AI 助手 Bard，其他公司在这个领域也在取得进展，但 OpenAI 仍然保持领先地位，没有明显的竞争对手。人们可能会认为 OpenAI 的图像生成模型 DALL·E，会在条件和非条件图像生成领域也同样占据主导地位。然而，实际上是一个开源的替代方案 Stable Diffusion，在流行度和创新方面**领先**一步。

**本文深入研究了 Stable Diffusion 背后的科学论文**，旨在提供一个清晰和...
