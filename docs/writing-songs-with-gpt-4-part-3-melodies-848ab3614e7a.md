# 使用 GPT-4 创作歌曲：第三部分，旋律

> 原文：[https://towardsdatascience.com/writing-songs-with-gpt-4-part-3-melodies-848ab3614e7a?source=collection_archive---------6-----------------------#2023-06-14](https://towardsdatascience.com/writing-songs-with-gpt-4-part-3-melodies-848ab3614e7a?source=collection_archive---------6-----------------------#2023-06-14)

## 如何使用 OpenAI 最新的语言模型来帮助创作新歌曲的旋律

[](https://robgon.medium.com/?source=post_page-----848ab3614e7a--------------------------------)[![Robert A. Gonsalves](../Images/96b4da0f602a1cd9d1e1d2917868cbee.png)](https://robgon.medium.com/?source=post_page-----848ab3614e7a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----848ab3614e7a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----848ab3614e7a--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----848ab3614e7a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwriting-songs-with-gpt-4-part-3-melodies-848ab3614e7a&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----848ab3614e7a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----848ab3614e7a--------------------------------) ·11分钟阅读·2023年6月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F848ab3614e7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwriting-songs-with-gpt-4-part-3-melodies-848ab3614e7a&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=-----848ab3614e7a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F848ab3614e7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwriting-songs-with-gpt-4-part-3-melodies-848ab3614e7a&source=-----848ab3614e7a---------------------bookmark_footer-----------)![](../Images/2fd003f55bc5aa702c3c6d2e59705b42.png)

**“一位站着的女性，戴着耳机，注视着电脑屏幕，同时对着专业麦克风唱歌，”** 我*使用 AI 图像生成程序* Midjourney 创建，并由作者编辑

这是我关于使用 GPT-4 [1] 作曲系列文章的第三篇，也是最后一篇。我的[第一篇文章](/writing-songs-with-gpt-4-part-1-lyrics-3728da678482)涵盖了为歌曲编写歌词的内容，而我的[第二篇文章](https://medium.com/towards-data-science/writing-songs-with-gpt-4-part-2-chords-173cfda0e5a1)则深入探讨了与歌词配套的和弦编写。在这篇文章中，简要介绍了 GPT-4 后，我将展示如何引导模型为歌曲创作旋律，同时搭配和弦和歌词。我将总结一下，并提供一些使用 AI 作曲的下一步建议。

# GPT-4 简介

GPT-4 是 OpenAI 最新的大型语言模型。它在他们的 ChatGPT 服务的付费版本中提供，每月费用为 20 美元。该模型在之前的版本 GPT-3.5 的基础上有所改进。以下是 OpenAI 在其[GPT-4 着陆页](https://openai.com/gpt-4)上对新模型的描述摘录。

> GPT-4 比以往更具创造性和协作性。它可以生成、编辑并与用户一起迭代创意和技术写作任务，如作曲、编写剧本或学习用户的写作风格……我们花了 6 个月的时间使 GPT-4 更安全、更符合要求。GPT-4 对于不允许内容的请求的响应可能性降低了 82%，而产生事实信息的可能性提高了 40%。
