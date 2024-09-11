# 使用GPT-4创作歌曲：第1部分，歌词

> 原文：[https://towardsdatascience.com/writing-songs-with-gpt-4-part-1-lyrics-3728da678482?source=collection_archive---------9-----------------------#2023-04-18](https://towardsdatascience.com/writing-songs-with-gpt-4-part-1-lyrics-3728da678482?source=collection_archive---------9-----------------------#2023-04-18)

## 如何使用OpenAI的最新语言模型来帮助创作原创歌曲的歌词

[](https://robgon.medium.com/?source=post_page-----3728da678482--------------------------------)[![Robert A. Gonsalves](../Images/96b4da0f602a1cd9d1e1d2917868cbee.png)](https://robgon.medium.com/?source=post_page-----3728da678482--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3728da678482--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3728da678482--------------------------------) [Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----3728da678482--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc97e6c73c13c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwriting-songs-with-gpt-4-part-1-lyrics-3728da678482&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=post_page-c97e6c73c13c----3728da678482---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3728da678482--------------------------------) ·16分钟阅读·2023年4月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3728da678482&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwriting-songs-with-gpt-4-part-1-lyrics-3728da678482&user=Robert+A.+Gonsalves&userId=c97e6c73c13c&source=-----3728da678482---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3728da678482&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwriting-songs-with-gpt-4-part-1-lyrics-3728da678482&source=-----3728da678482---------------------bookmark_footer-----------)![](../Images/c21f5b72d85499be2ca95b5507aa469a.png)

**“年轻的说唱歌手在录音棚里对着麦克风唱歌，耳机上戴着耳机，背景有一台电脑屏幕，”** 我*使用AI图像生成程序* Midjourney创建的图像，并由作者编辑。

我在过去三年里一直在撰写关于 OpenAI 大型语言模型的各种迭代，包括使用 GPT-3 [1] 来 [创作音乐](https://medium.com/towards-data-science/ai-tunes-creating-new-songs-with-artificial-intelligence-4fb383218146) 和 ChatGPT [2]，即 GPT-3.5，来 [写诗](https://medium.com/towards-data-science/using-chatgpt-as-a-creative-writing-partner-part-1-prose-dc9a9994d41f)。在尝试最新的语言模型 GPT-4 [3] 后，我发现它可以写出讲述连贯故事且押韵的歌词。

在这篇文章中，作为系列文章的第一部分，我将讨论 GPT-4 的背景，并将其写歌词和音乐的能力与 GPT-3 进行比较。接下来，我会展示 GPT-4 如何阅读当前乐队的歌词并创作符合其风格的歌曲。我将以关于使用 AI 创作音乐的一般讨论和一些下一步实验步骤结束。

在系列的 [第二部分](https://medium.com/towards-data-science/writing-songs-with-gpt-4-part-2-chords-173cfda0e5a1) 中，我将探索 GPT-4 如何为歌曲写和弦，而在 [第三部分](/writing-songs-with-gpt-4-part-3-melodies-848ab3614e7a) 中，我将使用该系统创作新的旋律以配合歌词。

# 介绍 GPT-4

3 月 14 日，我收到了 OpenAI 的邮件，告知我可以使用他们的新语言模型 GPT-4。以下是他们在技术报告中对模型的描述。

> 我们报告了 GPT-4 的发展，这是一款大规模的…
