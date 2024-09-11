# 改进 Diffusers 包以实现高质量图像生成

> 原文：[`towardsdatascience.com/improving-diffusers-package-for-high-quality-image-generation-a50fff04bdd4?source=collection_archive---------4-----------------------#2023-04-05`](https://towardsdatascience.com/improving-diffusers-package-for-high-quality-image-generation-a50fff04bdd4?source=collection_archive---------4-----------------------#2023-04-05)

## 克服了令牌大小限制、自定义模型加载、LoRa 支持、文本反演支持等问题

[](https://xhinker.medium.com/?source=post_page-----a50fff04bdd4--------------------------------)![Andrew Zhu (Shudong Zhu)](https://xhinker.medium.com/?source=post_page-----a50fff04bdd4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a50fff04bdd4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a50fff04bdd4--------------------------------) [Andrew Zhu (Shudong Zhu)](https://xhinker.medium.com/?source=post_page-----a50fff04bdd4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf0160c6bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-diffusers-package-for-high-quality-image-generation-a50fff04bdd4&user=Andrew+Zhu+%28Shudong+Zhu%29&userId=bf0160c6bb&source=post_page-bf0160c6bb----a50fff04bdd4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a50fff04bdd4--------------------------------) ·15 分钟阅读·2023 年 4 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa50fff04bdd4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-diffusers-package-for-high-quality-image-generation-a50fff04bdd4&user=Andrew+Zhu+%28Shudong+Zhu%29&userId=bf0160c6bb&source=-----a50fff04bdd4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa50fff04bdd4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimproving-diffusers-package-for-high-quality-image-generation-a50fff04bdd4&source=-----a50fff04bdd4---------------------bookmark_footer-----------)![](img/ec28506deb9273669dd315b83ff91aec.png)

告别 Babel，由 Andrew Zhu 使用纯 Python 和 Diffusers 生成

[AUTOMATIC1111 的 Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) 已证明是一个强大的工具，能够使用 Diffusion 模型生成高质量图像。然而，虽然 WebUI 使用起来很方便，但数据科学家、机器学习工程师和研究人员通常需要对图像生成过程有更多控制。这时，来自 huggingface 的 [diffusers](https://github.com/huggingface/diffusers) 包就派上了用场，它提供了一种在 Python 中运行 Diffusion 模型的方法，并允许用户自定义模型和提示，以根据具体需求生成图像。

尽管其潜力巨大，Diffusers 包存在几个限制，阻止其生成与 Stable Diffusion WebUI 产生的图像一样优秀的结果。其中最重要的限制包括：

+   无法使用 `.safetensor` 文件格式的自定义模型；

+   77 个提示令牌的限制；

+   缺乏 LoRA 支持；

+   以及缺乏图像放大功能（也称为在 Stable Diffusion WebUI 中的 HighRes）；

+   默认情况下性能较低且 VRAM 使用量高。
