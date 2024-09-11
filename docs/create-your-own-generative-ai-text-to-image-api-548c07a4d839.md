# 创建你自己的生成式 AI 文本到图像 API

> 原文：[`towardsdatascience.com/create-your-own-generative-ai-text-to-image-api-548c07a4d839?source=collection_archive---------3-----------------------#2023-04-12`](https://towardsdatascience.com/create-your-own-generative-ai-text-to-image-api-548c07a4d839?source=collection_archive---------3-----------------------#2023-04-12)

## 将你的杂谈转化为杰作，随时应需

[](https://medium.com/@omermx?source=post_page-----548c07a4d839--------------------------------)![Omer Mahmood](https://medium.com/@omermx?source=post_page-----548c07a4d839--------------------------------)[](https://towardsdatascience.com/?source=post_page-----548c07a4d839--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----548c07a4d839--------------------------------) [Omer Mahmood](https://medium.com/@omermx?source=post_page-----548c07a4d839--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F62cd989987f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-your-own-generative-ai-text-to-image-api-548c07a4d839&user=Omer+Mahmood&userId=62cd989987f6&source=post_page-62cd989987f6----548c07a4d839---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----548c07a4d839--------------------------------) ·17 分钟阅读·2023 年 4 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F548c07a4d839&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-your-own-generative-ai-text-to-image-api-548c07a4d839&user=Omer+Mahmood&userId=62cd989987f6&source=-----548c07a4d839---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F548c07a4d839&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-your-own-generative-ai-text-to-image-api-548c07a4d839&source=-----548c07a4d839---------------------bookmark_footer-----------)![](img/738244045935fe00f21f0a7c31c3f546.png)

图片由作者使用 Midjourney 生成，依据通用商业条款。

# 简要说明

+   最近生成式 AI 的进步催生了许多服务，如 DALL-E 2、Midjourney 和 Stability AI，这些服务有可能彻底改变我们处理内容创建的方式。

+   在这篇文章中，我将向你展示如何通过 API 构建和提供你自己的高性能文本到图像服务。基于[Stable Diffusion](https://github.com/CompVis/stable-diffusion)和[HuggingFace](https://medium.com/towards-data-science/whats-hugging-face-122f4e7eb11a)，使用 Vertex AI Workbench 和 Endpoints。

# 🚣🏼 我们是如何走到这一步的

正如乔治·劳顿在他的[文章](https://www.techtarget.com/searchenterpriseai/definition/generative-AI)中提到的：“生成 AI 是一种人工智能技术，可以生成各种类型的内容，包括文本、图像、音频和合成数据。最近关于生成 AI 的热议源于新用户界面简便的特点，使得在几秒钟内就能创建高质量的文本、图形和视频。”[2]

机器学习并不是什么新鲜事，实际上自 1960 年代以来，它以某种形式存在。[1] “但直到 2014 年，引入了[生成对抗网络](https://en.wikipedia.org/wiki/Generative_adversarial_network)（GANs），一种机器学习算法，生成 AI 才得以创建出逼真的真实人物图像、视频和音频。”[2]
