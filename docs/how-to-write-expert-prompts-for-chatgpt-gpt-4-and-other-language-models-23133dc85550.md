# 如何为 ChatGPT（GPT-4）和其他语言模型编写专业提示

> 原文：[`towardsdatascience.com/how-to-write-expert-prompts-for-chatgpt-gpt-4-and-other-language-models-23133dc85550?source=collection_archive---------0-----------------------#2023-11-01`](https://towardsdatascience.com/how-to-write-expert-prompts-for-chatgpt-gpt-4-and-other-language-models-23133dc85550?source=collection_archive---------0-----------------------#2023-11-01)

## 面向初学者的 LLM 提示工程指南

[](https://nabil-alouani.medium.com/?source=post_page-----23133dc85550--------------------------------)![Nabil Alouani](https://nabil-alouani.medium.com/?source=post_page-----23133dc85550--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23133dc85550--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----23133dc85550--------------------------------) [Nabil Alouani](https://nabil-alouani.medium.com/?source=post_page-----23133dc85550--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e6956110712&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-write-expert-prompts-for-chatgpt-gpt-4-and-other-language-models-23133dc85550&user=Nabil+Alouani&userId=7e6956110712&source=post_page-7e6956110712----23133dc85550---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23133dc85550--------------------------------) ·63 分钟阅读·2023 年 11 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F23133dc85550&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-write-expert-prompts-for-chatgpt-gpt-4-and-other-language-models-23133dc85550&user=Nabil+Alouani&userId=7e6956110712&source=-----23133dc85550---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23133dc85550&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-write-expert-prompts-for-chatgpt-gpt-4-and-other-language-models-23133dc85550&source=-----23133dc85550---------------------bookmark_footer-----------)![](img/1d2088f1c56684c0f65cee8b56f3ca2d.png)

图片由作者通过 Midjourney 提供。

提示工程是一种时髦的说法，即“为 AI 模型编写越来越好的指令，直到它完全按照你的要求执行。”

编写提示有点像骑自行车。你不需要机械物理学博士学位就能学会保持平衡。一些理论有帮助，但最重要的是不断尝试和错误。

将本指南视为帮助你准备骑上 AI 自行车的“理论基础”。你会发现一系列技术，附有解释、示例和你可以在自己喜欢的模型上测试的模板。

本指南专注于 ChatGPT（GPT-4），但下面分享的每一种技术也适用于其他大型语言模型（LLMs），如 Claude 和 LLaMA。

# 目录

```py
 What's in this guide?

💡 Why should you care about Prompt Engineering?
💡 Why prompt is engineering harder than you think?
💡 You don't need prompt ideas, you need problems
💡 Watch out for AI hallucinations

🟢 The Basics of Prompting 
🟢 Specify the context (also called "Priming")
🟢 Specify the desired format
🟢 Use <placeholders>
  ∘ How to use placeholders as parameters
  ∘ How…
```
