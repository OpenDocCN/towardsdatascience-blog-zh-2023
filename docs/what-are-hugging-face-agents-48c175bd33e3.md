# 如何使用 Hugging Face Agents 进行 NLP 任务

> 原文：[`towardsdatascience.com/what-are-hugging-face-agents-48c175bd33e3?source=collection_archive---------3-----------------------#2023-05-17`](https://towardsdatascience.com/what-are-hugging-face-agents-48c175bd33e3?source=collection_archive---------3-----------------------#2023-05-17)

## 一步一步的教程，教你如何使用 Transformers 工具和代理。

[](https://medium.com/@fmnobar?source=post_page-----48c175bd33e3--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----48c175bd33e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----48c175bd33e3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----48c175bd33e3--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----48c175bd33e3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-hugging-face-agents-48c175bd33e3&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----48c175bd33e3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----48c175bd33e3--------------------------------) ·11 分钟阅读·2023 年 5 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F48c175bd33e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-hugging-face-agents-48c175bd33e3&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=-----48c175bd33e3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F48c175bd33e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-hugging-face-agents-48c175bd33e3&source=-----48c175bd33e3---------------------bookmark_footer-----------)![](img/ba93162cbfcba76e15991d90518ec9c6.png)

图片由 [Alev Takil](https://unsplash.com/@alevisionco?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，[Unsplash](https://unsplash.com/photos/3syTDiVAc7w?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[Hugging Face](https://huggingface.co)是一个为机器学习从业者提供开源 AI 社区的组织，最近将工具和代理的概念整合进了其流行的[Transformers](https://huggingface.co/docs/transformers/index)库。如果你已经使用 Hugging Face 进行自然语言处理（NLP）、计算机视觉以及音频/语音处理任务，你可能会想知道工具和代理为生态系统带来了什么价值。代理为用户提供了可以说是显著的便利——让我来解释一下。

假设我想使用来自 Hugging Face Hub 的模型将英文翻译成法文。在这种情况下，我需要进行一些研究以找到一个好的模型，然后弄清楚如何实际使用这个模型，最后编写代码生成翻译。但如果你已经有一个 Hugging Face 专家在你身边，他已经知道所有这些信息了呢？换句话说，你只需告诉专家你想将一句话从英文翻译成法文，然后专家会负责寻找合适的模型、编写代码并返回结果——而且专家的速度远远快于你我。这正是代理的作用！我们用简单的英语向代理描述我们的需求，然后代理会查找可用的工具……
