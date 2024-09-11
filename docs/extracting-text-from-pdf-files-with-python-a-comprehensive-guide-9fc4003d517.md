# 使用 Python 从 PDF 文件中提取文本：全面指南

> 原文：[`towardsdatascience.com/extracting-text-from-pdf-files-with-python-a-comprehensive-guide-9fc4003d517?source=collection_archive---------0-----------------------#2023-09-21`](https://towardsdatascience.com/extracting-text-from-pdf-files-with-python-a-comprehensive-guide-9fc4003d517?source=collection_archive---------0-----------------------#2023-09-21)

## 从 PDF 文件中提取表格、图像和纯文本的完整过程

[](https://medium.com/@george.stavrakis.1996?source=post_page-----9fc4003d517--------------------------------)![George Stavrakis](https://medium.com/@george.stavrakis.1996?source=post_page-----9fc4003d517--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9fc4003d517--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9fc4003d517--------------------------------) [George Stavrakis](https://medium.com/@george.stavrakis.1996?source=post_page-----9fc4003d517--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffbbd4313532a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fextracting-text-from-pdf-files-with-python-a-comprehensive-guide-9fc4003d517&user=George+Stavrakis&userId=fbbd4313532a&source=post_page-fbbd4313532a----9fc4003d517---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9fc4003d517--------------------------------) ·17 分钟阅读·2023 年 9 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9fc4003d517&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fextracting-text-from-pdf-files-with-python-a-comprehensive-guide-9fc4003d517&user=George+Stavrakis&userId=fbbd4313532a&source=-----9fc4003d517---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9fc4003d517&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fextracting-text-from-pdf-files-with-python-a-comprehensive-guide-9fc4003d517&source=-----9fc4003d517---------------------bookmark_footer-----------)![](img/b9b1eb9c7f8d41e7811c0bdffa0143f2.png)

图片来源：[Giorgio Trovato](https://unsplash.com/@giorgiotrovato?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在大型语言模型（LLMs）及其[广泛应用](https://bit.ly/49i8JoP)的时代，从简单的文本摘要和翻译到基于情感和财务报告主题预测股票表现，文本数据的重要性前所未有。

许多类型的文档都包含这种非结构化信息，从网页文章和博客帖子到手写信件和诗歌。然而，这些文本数据中的相当一部分以 PDF 格式存储和传输。更具体地说，每年在 Outlook 中打开的 PDF 数量超过 20 亿，而每天在 Google Drive 和电子邮件中保存的新 PDF 文件则达到 7300 万（2）。

因此，开发一种更系统化的方式来处理这些文档并从中提取信息，将使我们能够实现自动化流程，更好地理解和利用这些庞大的文本数据。当然，对于这个任务，我们的最佳助手非**Python**莫属。

然而，在我们开始流程之前，我们需要明确不同类型的……
