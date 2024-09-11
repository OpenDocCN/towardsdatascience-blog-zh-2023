# 生成式 AI 中的任务概念：智能系统的构建模块

> 原文：[https://towardsdatascience.com/decoding-tasks-in-generative-ai-the-building-blocks-of-intelligent-systems-f677e8e2ee22?source=collection_archive---------10-----------------------#2023-09-07](https://towardsdatascience.com/decoding-tasks-in-generative-ai-the-building-blocks-of-intelligent-systems-f677e8e2ee22?source=collection_archive---------10-----------------------#2023-09-07)

## 大型企业的生成式 AI：从治理到 API 聚合，第 1 部分

[](https://eavanvalkenburg.medium.com/?source=post_page-----f677e8e2ee22--------------------------------)[![Eduard van Valkenburg](../Images/c0ab8a94cecc4ce247e345a60e9314f1.png)](https://eavanvalkenburg.medium.com/?source=post_page-----f677e8e2ee22--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f677e8e2ee22--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f677e8e2ee22--------------------------------) [Eduard van Valkenburg](https://eavanvalkenburg.medium.com/?source=post_page-----f677e8e2ee22--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e3bedf21d4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-tasks-in-generative-ai-the-building-blocks-of-intelligent-systems-f677e8e2ee22&user=Eduard+van+Valkenburg&userId=7e3bedf21d4d&source=post_page-7e3bedf21d4d----f677e8e2ee22---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f677e8e2ee22--------------------------------) ·8 分钟阅读·2023 年 9 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff677e8e2ee22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-tasks-in-generative-ai-the-building-blocks-of-intelligent-systems-f677e8e2ee22&user=Eduard+van+Valkenburg&userId=7e3bedf21d4d&source=-----f677e8e2ee22---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff677e8e2ee22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-tasks-in-generative-ai-the-building-blocks-of-intelligent-systems-f677e8e2ee22&source=-----f677e8e2ee22---------------------bookmark_footer-----------)

欢迎，各位科技爱好者和商业领袖！我很高兴开始一系列专注于一个在全球引发热议的话题——生成式 AI 的博客文章。

当我深入探索企业生成式 AI 的迷人世界时，我不仅会探讨治理、安全性和可审计性等高层次概念，还将提供关于 API 聚合和理解生成式 AI 架构等主题的实用指导。

![](../Images/2b41403c9cc164c92a040855db7d2004.png)

图像由 OpenAI 的 DALL-E 模型生成，提示词为：模仿维米尔风格的写作机器人

无论你是 AI 领域的资深专家还是充满好奇的新手，本系列旨在揭示大型企业如何利用生成 AI 的力量推动创新、提高效率和创造价值。我将处理复杂问题，解读术语，并提供可操作的见解，帮助你自信地导航 AI 领域。因此，系好安全带，和我一起踏上这段激动人心的商业技术未来之旅吧。让我们一起揭开 AI 的神秘面纱！

***免责声明***：本文概述了不特定于 Azure 的建筑概念，但有时使用 Azure 服务进行说明，因为我是一名解决方案专家*…
