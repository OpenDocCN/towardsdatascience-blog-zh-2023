# 稳定扩散作为 API：创建一个去除人物的微服务

> 原文：[https://towardsdatascience.com/stable-diffusion-as-an-api-5e381aec1f6?source=collection_archive---------0-----------------------#2023-02-04](https://towardsdatascience.com/stable-diffusion-as-an-api-5e381aec1f6?source=collection_archive---------0-----------------------#2023-02-04)

![](../Images/a964e08268efee5aaf0f29c6237487db.png)

使用稳定扩散 2 生成的风景图像（作者提供）。

## 使用稳定扩散微服务从照片中去除人物

[](https://medium.com/@masonmcgough?source=post_page-----5e381aec1f6--------------------------------)[![Mason McGough](../Images/4b465e0eef1590b1f12dea23a6f688e1.png)](https://medium.com/@masonmcgough?source=post_page-----5e381aec1f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5e381aec1f6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5e381aec1f6--------------------------------) [Mason McGough](https://medium.com/@masonmcgough?source=post_page-----5e381aec1f6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb2676483d419&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstable-diffusion-as-an-api-5e381aec1f6&user=Mason+McGough&userId=b2676483d419&source=post_page-b2676483d419----5e381aec1f6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e381aec1f6--------------------------------) · 12 min read · 2023年2月4日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5e381aec1f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstable-diffusion-as-an-api-5e381aec1f6&user=Mason+McGough&userId=b2676483d419&source=-----5e381aec1f6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5e381aec1f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstable-diffusion-as-an-api-5e381aec1f6&source=-----5e381aec1f6---------------------bookmark_footer-----------)

# 概述

稳定扩散是一款前沿的开源工具，可以从文本生成图像。稳定扩散 Web UI 通过 API 和交互式 UI 提供了许多功能。我们将首先介绍如何使用这个 API，然后演示如何将其作为一个隐私保护的微服务来去除图像中的人物。

# 生成式 AI 简介

去年，基于机器学习的数据生成器出现了许多创新，可以称2022年为“生成式AI之年”。我们见证了[**DALL-E 2**](https://openai.com/dall-e-2/)，这是OpenAI的文本生成图像模型，能够生成令人惊叹的现实主义图像，如宇航员骑马和穿着人类衣服的狗。[**GitHub Copilot**](https://github.blog/2022-06-21-github-copilot-is-generally-available-to-all-developers/)，这个强大的代码自动完成工具，能够从单个评论中自动完成语句、编写文档以及实现整个函数，已经作为订阅服务公开发布。我们还见证了[**Dream Fields**](https://www.ajayjain.net/dreamfields/)、[**Dream Fusion**](https://dreamfusion3d.github.io/)和[**Magic3D**](https://research.nvidia.com/labs/dir/magic3d/)，这一系列突破性的模型能够仅凭文本生成有纹理的3D模型。最后但绝对不可忽视的是[**ChatGPT**](https://openai.com/blog/chatgpt/)，这一前沿的AI聊天机器人，如今已经无需介绍。
