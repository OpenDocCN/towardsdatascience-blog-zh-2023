# 关于 Hugging Face 及其实现 6 个 NLP 任务的简介

> 原文：[https://towardsdatascience.com/implement-nlp-tasks-using-hugging-face-77dfdcad65fd?source=collection_archive---------8-----------------------#2023-04-18](https://towardsdatascience.com/implement-nlp-tasks-using-hugging-face-77dfdcad65fd?source=collection_archive---------8-----------------------#2023-04-18)

## Hugging Face 使用简介教程

[](https://medium.com/@fmnobar?source=post_page-----77dfdcad65fd--------------------------------)[![Farzad Mahmoodinobar](../Images/2d75209693b712300e6f0796bd2487d0.png)](https://medium.com/@fmnobar?source=post_page-----77dfdcad65fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----77dfdcad65fd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----77dfdcad65fd--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----77dfdcad65fd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-nlp-tasks-using-hugging-face-77dfdcad65fd&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----77dfdcad65fd---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----77dfdcad65fd--------------------------------) · 12 分钟阅读 · 2023年4月18日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F77dfdcad65fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-nlp-tasks-using-hugging-face-77dfdcad65fd&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=-----77dfdcad65fd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F77dfdcad65fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-nlp-tasks-using-hugging-face-77dfdcad65fd&source=-----77dfdcad65fd---------------------bookmark_footer-----------)![](../Images/8a227cd780125742369e775c5e6ccf84.png)

图片来源 [Duy Pham](https://unsplash.com/@miinyuii?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](https://unsplash.com/photos/Cecb0_8Hx-o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[Hugging Face](https://huggingface.co/) 是一个开源的人工智能社区，由机器学习从业者创建，专注于自然语言处理（NLP）、计算机视觉和音频/语音处理任务。无论你现在是否在这些领域工作，还是希望未来进入这个领域，你都将从学习如何使用 Hugging Face 工具和模型中受益。

在这篇文章中，我们将利用Hugging Face上可用的预训练模型，回顾六个最常用的自然语言处理任务，如下所示：

1.  文本生成（又称为语言建模）

1.  问答系统

1.  情感分析

1.  文本分类

1.  文本摘要

1.  机器翻译

在进入任务之前，我们先花一点时间讨论一下“训练”和“推断”这两个机器学习中的重要概念，以便澄清我们今天要做的工作。

让我们开始吧！
