# 使用ChatGPT作为创造性写作伙伴——第一部分：散文

> 原文：[https://towardsdatascience.com/using-chatgpt-as-a-creative-writing-partner-part-1-prose-dc9a9994d41f?source=collection_archive---------0-----------------------#2023-01-04](https://towardsdatascience.com/using-chatgpt-as-a-creative-writing-partner-part-1-prose-dc9a9994d41f?source=collection_archive---------0-----------------------#2023-01-04)

## 最新的OpenAI语言模型如何帮助写诗歌、小说和剧本

[](https://robgon.medium.com/?source=post_page-----dc9a9994d41f--------------------------------)[![Robert A. Gonsalves](../Images/96b4da0f602a1cd9d1e1d2917868cbee.png)](https://robgon.medium.com/?source=post_page-----dc9a9994d41f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dc9a9994d41f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dc9a9994d41f--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----dc9a9994d41f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-as-a-creative-writing-partner-part-1-prose-dc9a9994d41f&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----dc9a9994d41f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dc9a9994d41f--------------------------------) ·15分钟阅读·2023年1月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdc9a9994d41f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-as-a-creative-writing-partner-part-1-prose-dc9a9994d41f&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=-----dc9a9994d41f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdc9a9994d41f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-chatgpt-as-a-creative-writing-partner-part-1-prose-dc9a9994d41f&source=-----dc9a9994d41f---------------------bookmark_footer-----------)![](../Images/c9f6f24f0cb3dc4b5201c051ddc61997.png)

**“一个女人在笔记本电脑上打字，旁边有一个有帮助的玩具机器人，”** 图像由AI图像创建程序Midjourney生成，并由作者编辑

如果你一直在阅读我在Medium上的文章，你会知道我自2020年8月起一直在写关于AI的创造性用途。我经常写关于生成数字艺术的内容，但偶尔也会写关于AI在其他创造性用途上的应用，比如写散文和作曲。当我听说OpenAI发布了一款名为ChatGPT的最新语言模型时，我立刻投入其中，测试它在创造性写作中的表现。

# 概述

这是关于将 ChatGPT 作为写作伙伴的三部分系列中的第一部分。系列将涵盖写作散文、[作曲](/using-chatgpt-as-a-creative-writing-partner-part-2-music-d2fd7501c268)以及创作图画书。

在这篇文章中，我将回顾一些关于 ChatGPT 的背景信息，然后展示我在为各种创意项目生成文本的实验结果：

1.  写俳句

1.  为新小说创建情节概要

1.  为电视节目和电影创作新的剧本

我将通过给出一些关于使用模型的一般观察和未来探索的可能步骤来结束。

# ChatGPT
