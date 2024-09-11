# 如何通过提示工程改进 ChatGPT 生成的代码

> 原文：[`towardsdatascience.com/how-to-improve-chatgpts-generated-code-with-prompt-engineering-cd131f9ab55d?source=collection_archive---------16-----------------------#2023-07-21`](https://towardsdatascience.com/how-to-improve-chatgpts-generated-code-with-prompt-engineering-cd131f9ab55d?source=collection_archive---------16-----------------------#2023-07-21)

## 提升 ChatGPT 作为编码助手的性能的简单策略

[](https://medium.com/@aashishnair?source=post_page-----cd131f9ab55d--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----cd131f9ab55d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd131f9ab55d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd131f9ab55d--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----cd131f9ab55d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3087ba81e065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-improve-chatgpts-generated-code-with-prompt-engineering-cd131f9ab55d&user=Aashish+Nair&userId=3087ba81e065&source=post_page-3087ba81e065----cd131f9ab55d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd131f9ab55d--------------------------------) ·7 min read·2023 年 7 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcd131f9ab55d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-improve-chatgpts-generated-code-with-prompt-engineering-cd131f9ab55d&user=Aashish+Nair&userId=3087ba81e065&source=-----cd131f9ab55d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcd131f9ab55d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-improve-chatgpts-generated-code-with-prompt-engineering-cd131f9ab55d&source=-----cd131f9ab55d---------------------bookmark_footer-----------)![](img/bb5ca25ff10ecd443c314b92f6047627.png)

图片来源：cottonbro studio: [`www.pexels.com/photo/hands-typing-on-a-laptop-keyboard-5483077/`](https://www.pexels.com/photo/hands-typing-on-a-laptop-keyboard-5483077/)

## 介绍

尽管 ChatGPT 拥有无数功能，但最吸引程序员的功能是生成代码的能力。

ChatGPT 已证明能够在几秒钟内生成有效的代码。因此，它改变了程序员处理编码项目的方式，许多人通过使用聊天机器人来处理繁琐或复杂的任务，从而节省了时间和精力。

然而，ChatGPT 生成无效代码的记录不佳。事实上，StackOverflow 曾对 ChatGPT 和其他生成 AI 创造的内容实施了[临时禁令](https://meta.stackoverflow.com/questions/421831/temporary-policy-generative-ai-e-g-chatgpt-is-banned)，主要原因是其正确答案的比例较低。

尽管如此，这些结果往往与底层的大型语言模型（LLM）的质量关系不大，而更多地与用户如何与聊天机器人互动有关。

人们在学会利用**提示工程**之后，将从他们的聊天机器人中获得更多好处，这一主题最近受到广泛关注。

在这里，我们简要概述了为什么写得好的提示如此重要…
