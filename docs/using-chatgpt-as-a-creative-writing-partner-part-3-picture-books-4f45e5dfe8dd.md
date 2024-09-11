# 使用 ChatGPT 作为创意写作伙伴——第三部分：图画书

> 原文：[https://towardsdatascience.com/using-chatgpt-as-a-creative-writing-partner-part-3-picture-books-4f45e5dfe8dd?source=collection_archive---------2-----------------------#2023-02-07](https://towardsdatascience.com/using-chatgpt-as-a-creative-writing-partner-part-3-picture-books-4f45e5dfe8dd?source=collection_archive---------2-----------------------#2023-02-07)

## 如何利用 OpenAI 最新的语言模型帮助你编写儿童书籍，并使用 Midjourney 创建插图

[](https://robgon.medium.com/?source=post_page-----4f45e5dfe8dd--------------------------------)[![Robert A. Gonsalves](../Images/96b4da0f602a1cd9d1e1d2917868cbee.png)](https://robgon.medium.com/?source=post_page-----4f45e5dfe8dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4f45e5dfe8dd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4f45e5dfe8dd--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----4f45e5dfe8dd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-as-a-creative-writing-partner-part-3-picture-books-4f45e5dfe8dd&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----4f45e5dfe8dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4f45e5dfe8dd--------------------------------) · 17 分钟阅读 · 2023年2月7日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4f45e5dfe8dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-as-a-creative-writing-partner-part-3-picture-books-4f45e5dfe8dd&source=-----4f45e5dfe8dd---------------------bookmark_footer-----------)![](../Images/aff3bc027cf7c904009ab3f0da6aabf6.png)

“**一个男人和一个友好的机器人在画架前用刷子和颜料绘画的画面**，”图像由 AI 图像生成程序 Midjourney 创建，并由作者编辑

这是我关于使用 OpenAI 的 ChatGPT 语言模型 [1] 创作写作的三篇文章系列的第三篇，也是最后一篇。在[第一篇文章](/using-chatgpt-as-a-creative-writing-partner-part-1-prose-dc9a9994d41f)中，我描述了如何使用 ChatGPT 来创作散文、诗歌、小说和剧本。在[第二篇文章](/using-chatgpt-as-a-creative-writing-partner-part-2-music-d2fd7501c268)中，我展示了如何利用该系统创建和弦序列来创作不同风格的音乐。

我最近的实验是使用 ChatGPT 创建一本图画书。因为系统不能直接生成图像，我让它描述场景，然后我使用 Midjourney [2] 进行了图像渲染，Midjourney 是一个我在[早期文章](/exploring-midjourney-v4-for-creating-digital-art-4d20980a96f7)中探讨过的文本到图像创作系统。

# 概述

我将从 ChatGPT 和 Midjourney 的背景信息开始，然后展示如何使用这两个系统为儿童创作一本新的图画书。我将以对使用这些系统的一般讨论结束，并提供一些未来探索的步骤。

## ChatGPT

ChatGPT 是 OpenAI 开发的大型语言模型。它基于其 GPT-3 模型 [3] 的一种变体，...
