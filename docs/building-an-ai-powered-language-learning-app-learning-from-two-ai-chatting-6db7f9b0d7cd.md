# 创建一个 AI 驱动的语言学习应用：从两个 AI 聊天中学习

> 原文：[`towardsdatascience.com/building-an-ai-powered-language-learning-app-learning-from-two-ai-chatting-6db7f9b0d7cd?source=collection_archive---------4-----------------------#2023-06-26`](https://towardsdatascience.com/building-an-ai-powered-language-learning-app-learning-from-two-ai-chatting-6db7f9b0d7cd?source=collection_archive---------4-----------------------#2023-06-26)

## 一个关于使用 Langchain、OpenAI、gTTS 和 Streamlit 创建双聊天机器人语言学习应用的逐步教程

[](https://shuaiguo.medium.com/?source=post_page-----6db7f9b0d7cd--------------------------------)![Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----6db7f9b0d7cd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6db7f9b0d7cd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6db7f9b0d7cd--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----6db7f9b0d7cd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-an-ai-powered-language-learning-app-learning-from-two-ai-chatting-6db7f9b0d7cd&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----6db7f9b0d7cd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6db7f9b0d7cd--------------------------------) · 25 min 阅读 · 2023 年 6 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6db7f9b0d7cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-an-ai-powered-language-learning-app-learning-from-two-ai-chatting-6db7f9b0d7cd&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----6db7f9b0d7cd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6db7f9b0d7cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-an-ai-powered-language-learning-app-learning-from-two-ai-chatting-6db7f9b0d7cd&source=-----6db7f9b0d7cd---------------------bookmark_footer-----------)![](img/6b56edb9154ae8825ea55581c2ab5ee9.png)

DALL-E 提示：两个友好的机器人相互交谈。

当我第一次开始学习一门新语言时，我喜欢买那些“对话体”书籍。我发现这些书籍非常有用，因为它们帮助我理解语言的使用方式——不仅仅是语法和词汇，还有人们在日常生活中如何真正使用它。

现在随着大型语言模型（LLMs）的兴起，我产生了一个想法：我是否可以将这些语言学习书籍复制成一种更互动、动态且可扩展的格式？我是否可以利用 LLM 创建一个工具，为语言学习者生成新鲜的、按需的对话？

这个想法启发了我今天想与大家分享的项目——一个 AI 驱动的语言学习应用，学习者可以观察并从两个 AI 聊天机器人进行的**对话**或**辩论**中学习。

关于使用的技术栈，我使用了 Langchain、OpenAI API、gTTS 和 Streamlit 来创建应用程序，用户可以定义角色、场景或辩论主题，让 AI 生成内容。
