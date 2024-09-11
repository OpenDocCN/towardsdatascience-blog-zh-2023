# 解锁 ChatGPT 的新维度：文本到语音的集成

> 原文：[`towardsdatascience.com/chatgpt-text-to-speech-artificial-intelligence-python-data-science-52456f51fad6?source=collection_archive---------6-----------------------#2023-05-30`](https://towardsdatascience.com/chatgpt-text-to-speech-artificial-intelligence-python-data-science-52456f51fad6?source=collection_archive---------6-----------------------#2023-05-30)

## 增强 ChatGPT 互动中的用户体验

[](https://medium.com/@andvalenzuela?source=post_page-----52456f51fad6--------------------------------)![Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----52456f51fad6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----52456f51fad6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----52456f51fad6--------------------------------) [Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----52456f51fad6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa6f3f1654c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-text-to-speech-artificial-intelligence-python-data-science-52456f51fad6&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=post_page-a6f3f1654c3----52456f51fad6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----52456f51fad6--------------------------------) ·6 分钟阅读·2023 年 5 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F52456f51fad6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-text-to-speech-artificial-intelligence-python-data-science-52456f51fad6&user=Andrea+Valenzuela&userId=a6f3f1654c3&source=-----52456f51fad6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F52456f51fad6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-text-to-speech-artificial-intelligence-python-data-science-52456f51fad6&source=-----52456f51fad6---------------------bookmark_footer-----------)![](img/37eb3c9be01fd0ffe01fc20efe3aef12.png)

图片由 [Jason Rosewell](https://unsplash.com/@jasonrosewell?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/es/fotos/ASKeuOZqhYU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你进入了这篇文章，我相信你已经使用 ChatGPT 一段时间了。我也是 :)

在过去的几个月里，我一直专注于如何从 ChatGPT 获得更好的输出，即所谓的[*提示工程*](https://medium.com/gitconnected/improve-chatgpt-performance-prompt-engineering-data-science-artificial-intelligence-6fa3953bc5b6) *—* 或构建 [自定义应用程序](https://medium.com/towards-data-science/chatgpt-summarization-llms-chatgpt3-chatgpt4-artificial-intelligence-16cf0e3625ce)，这些应用程序在背后使用大型语言模型（LLM）。然而，最近我一直在思考 **如何提升 ChatGPT 的用户体验**。

网络界面虽然不错，但我们都同意它在几次迭代后并不是那么用户友好。*如果我们能更进一步，为 ChatGPT 提供语音会怎么样？* **想象一下 ChatGPT 像你的私人 AI 助手一样大声回应你。**

在这篇文章中，我们将探讨如何通过为 ChatGPT 的输出添加一个文本转语音（TTS）层来增强你的 ChatGPT 体验，从而获得听 ChatGPT 的所有好处，而不仅仅是阅读。

**让我们为 ChatGPT 提供语音，让你的互动更具吸引力、易于访问和更方便！**

# 文本转语音技术

文本转语音技术已经成为用户体验中的颠覆性工具。正如从术语中可以轻易推断出的那样……
