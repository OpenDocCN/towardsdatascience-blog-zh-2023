# 使用 GPT-3 制作文本总结器

> 原文：[`towardsdatascience.com/make-a-text-summarizer-with-gpt-3-f0917a07189e?source=collection_archive---------1-----------------------#2023-01-23`](https://towardsdatascience.com/make-a-text-summarizer-with-gpt-3-f0917a07189e?source=collection_archive---------1-----------------------#2023-01-23)

## 使用 Python、OpenAI 的 GPT-3 和 Streamlit 的快速教程

![Jay Peterman](https://medium.com/@jaypeterman?source=post_page-----f0917a07189e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0917a07189e--------------------------------) [Jay Peterman](https://medium.com/@jaypeterman?source=post_page-----f0917a07189e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9731dc608e6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-a-text-summarizer-with-gpt-3-f0917a07189e&user=Jay+Peterman&userId=9731dc608e6c&source=post_page-9731dc608e6c----f0917a07189e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0917a07189e--------------------------------) ·11 分钟阅读·2023 年 1 月 23 日

--

![](img/e852944464412bdf76d016dcde8c7f12.png)

照片由[Ed Robertson](https://unsplash.com/@eddrobertson?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们可能处在语言模型的早期阶段，未来几年将会开发出许多有用的集成。我最近对文本总结产生了兴趣，并发现 OpenAI 提供的解决方案在这方面表现出色。

本教程的目标是向读者介绍 OpenAI 生态系统，并给出从构思到部署的实际构建示例。我们将通过构建一个使用 OpenAI 的 GPT-3 模型来总结文本的 Streamlit 应用程序，并将该应用程序部署到 Streamlit Cloud 来实现这一目标。

如果你有兴趣了解如何在 Python 中使用语言模型，这将是一个很好的第一步。如果你有兴趣构建自己的集成方案，可以将其作为模板来实现自己的想法。

虽然对 Python、Git/Github 和命令行的一些知识会有帮助，但本教程可以作为一个指南，帮助新人在学习过程中了解这些内容。

## 步骤 1：在 Github 上设置 git 仓库，并将其克隆到本地

如果你还没有 Github 账户，你需要注册一个。登录后，创建一个名为`text-summarizer`的新仓库……
