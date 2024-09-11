# 在 DALL-E 3 翻译中的迷失

> 原文：[https://towardsdatascience.com/lost-in-dall-e-3-translation-b85a3958b9d6?source=collection_archive---------6-----------------------#2023-11-02](https://towardsdatascience.com/lost-in-dall-e-3-translation-b85a3958b9d6?source=collection_archive---------6-----------------------#2023-11-02)

## 用多种语言生成的 AI 图像会产生不同的结果

[](https://medium.com/@artfish?source=post_page-----b85a3958b9d6--------------------------------)[![Yennie Jun](../Images/b635e965f21c3d55833269e12e861322.png)](https://medium.com/@artfish?source=post_page-----b85a3958b9d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b85a3958b9d6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b85a3958b9d6--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----b85a3958b9d6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F12ca1ab81192&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flost-in-dall-e-3-translation-b85a3958b9d6&user=Yennie+Jun&userId=12ca1ab81192&source=post_page-12ca1ab81192----b85a3958b9d6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b85a3958b9d6--------------------------------) · 11 分钟阅读 · 2023年11月2日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb85a3958b9d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flost-in-dall-e-3-translation-b85a3958b9d6&source=-----b85a3958b9d6---------------------bookmark_footer-----------)![](../Images/77d913b783ec6e95f9ffeecc49a33bca.png)

使用 DALL-E 3 生成的图像，展示了“一个人的图像”这一提示下的六种语言版本。图形由作者创建。

*本文最初发表在* [*artfish intelligence*](https://www.artfish.ai/p/lost-in-dalle3-translation)

# 介绍

OpenAI 最近推出了 [DALL-E 3](https://openai.com/blog/dall-e-3-is-now-available-in-chatgpt-plus-and-enterprise)，这是他们最新的 AI 图像生成模型。

但正如[近期媒体报道](https://restofworld.org/2023/ai-image-stereotypes/)和[研究](https://arxiv.org/abs/2303.11408)所揭示的，这些 AI 模型带有偏见和刻板印象的包袱。例如，像 Stable Diffusion 和 Midjourney 这样的 AI 图像生成模型往往放大现有的关于[种族、性别](https://www.bloomberg.com/graphics/2023-generative-ai-bias/)和[国籍](https://restofworld.org/2023/ai-image-stereotypes/)的刻板印象。

然而，这些研究大多主要使用英语提示来测试模型。这就引出了一个问题：这些模型会如何响应非英语提示？

在这篇文章中，我深入探讨了 DALL-E 3 在处理来自不同语言的提示时的行为。借鉴了我[之前的研究](https://www.artfish.ai/p/all-languages-are-not-created-tokenized)的主题，我提供了对最新 AI 图像生成模型的多语言视角。

# DALL-E 3 的工作原理：提示转换

与之前的 AI 图像生成模型不同，这个最新版本的 DALL-E 模型不会直接生成你输入的内容。相反，DALL-E 3 融入了**自动提示转换**，这意味着它**转换你的**…
