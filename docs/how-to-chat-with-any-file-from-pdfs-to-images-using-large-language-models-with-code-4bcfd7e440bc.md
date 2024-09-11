# 如何使用大型语言模型与任何 PDF 和图像文件进行聊天 —— 以及代码

> 原文：[`towardsdatascience.com/how-to-chat-with-any-file-from-pdfs-to-images-using-large-language-models-with-code-4bcfd7e440bc?source=collection_archive---------0-----------------------#2023-08-05`](https://towardsdatascience.com/how-to-chat-with-any-file-from-pdfs-to-images-using-large-language-models-with-code-4bcfd7e440bc?source=collection_archive---------0-----------------------#2023-08-05)

## 完整指南：构建一个可以回答任何文件问题的 AI 助手

[](https://zoumanakeita.medium.com/?source=post_page-----4bcfd7e440bc--------------------------------)![Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----4bcfd7e440bc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4bcfd7e440bc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4bcfd7e440bc--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----4bcfd7e440bc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-chat-with-any-file-from-pdfs-to-images-using-large-language-models-with-code-4bcfd7e440bc&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----4bcfd7e440bc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4bcfd7e440bc--------------------------------) ·9 min read·2023 年 8 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4bcfd7e440bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-chat-with-any-file-from-pdfs-to-images-using-large-language-models-with-code-4bcfd7e440bc&user=Zoumana+Keita&userId=e6ae785a30d&source=-----4bcfd7e440bc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4bcfd7e440bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-chat-with-any-file-from-pdfs-to-images-using-large-language-models-with-code-4bcfd7e440bc&source=-----4bcfd7e440bc---------------------bookmark_footer-----------)

# 介绍

许多有价值的信息被困在 PDF 和图像文件中。幸运的是，我们拥有这些强大的大脑，能够处理这些文件以找到特定的信息，这实际上非常棒。

> 但有多少人内心深处希望拥有一个能够回答关于给定文档的任何问题的工具呢？

这就是本文的全部目的。我将逐步解释如何构建一个可以与任何 PDF 和图像文件进行聊天的系统。

> 如果你更喜欢观看视频，可以查看下面的链接：

## 项目的整体工作流程

对于正在构建的系统的主要组件有清晰的理解总是很好的。让我们开始吧。
