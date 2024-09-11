# 将 ChatGPT 作为创意写作伙伴 — 第二部分：音乐

> 原文：[`towardsdatascience.com/using-chatgpt-as-a-creative-writing-partner-part-2-music-d2fd7501c268?source=collection_archive---------2-----------------------#2023-01-17`](https://towardsdatascience.com/using-chatgpt-as-a-creative-writing-partner-part-2-music-d2fd7501c268?source=collection_archive---------2-----------------------#2023-01-17)

## 最新的 OpenAI 语言模型如何帮助你为新歌曲编写和弦，音乐由 Band-in-a-Box 提供

[](https://robgon.medium.com/?source=post_page-----d2fd7501c268--------------------------------)![Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----d2fd7501c268--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d2fd7501c268--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2fd7501c268--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----d2fd7501c268--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-as-a-creative-writing-partner-part-2-music-d2fd7501c268&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----d2fd7501c268---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2fd7501c268--------------------------------) ·16 分钟阅读·2023 年 1 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd2fd7501c268&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-as-a-creative-writing-partner-part-2-music-d2fd7501c268&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=-----d2fd7501c268---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd2fd7501c268&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-as-a-creative-writing-partner-part-2-music-d2fd7501c268&source=-----d2fd7501c268---------------------bookmark_footer-----------)![](img/edee1548994dd1f36550a9533a9397b5.png)

**“两位钢琴演奏者，一个人和一个友好的机器人，数字艺术，”** 图像由 AI 图像生成程序 Midjourney 创建，并由作者编辑

在我的[上一篇文章](https://medium.com/towards-data-science/using-chatgpt-as-a-creative-writing-partner-part-1-prose-dc9a9994d41f)中，我讨论了如何将 OpenAI 的 ChatGPT 大型语言模型[1]用作各种散文类型的写作伙伴。在本文中，我将展示如何使用该系统通过生成和弦来帮助作曲。

在简要介绍 ChatGPT 之后，我将展示我使用新系统在以下风格下创作音乐的实验结果：爵士、乡村摇滚和雷鬼。我会总结使用该模型创作音乐的总体观察，并提出未来探索的一些步骤。请注意，本系列的第三篇也是最后一篇将会讨论如何利用该系统和 Midjourney 的帮助来创建图画书。

# ChatGPT

ChatGPT 是 OpenAI 最新的语言模型，旨在通过聊天用户界面与人进行互动。GPT 代表生成预训练变换器，其中变换器是一种 AI 模型。你可以在本系列的[第一篇文章](https://medium.com/towards-data-science/using-chatgpt-as-a-creative-writing-partner-part-1-prose-dc9a9994d41f)中阅读有关该系统的完整背景。
