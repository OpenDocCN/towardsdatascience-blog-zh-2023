# 一个使用 ChatGPT 代码解释器的数据科学项目

> 原文：[`towardsdatascience.com/a-data-science-project-with-chatgpt-code-interpreter-e9beb8705dac?source=collection_archive---------2-----------------------#2023-07-20`](https://towardsdatascience.com/a-data-science-project-with-chatgpt-code-interpreter-e9beb8705dac?source=collection_archive---------2-----------------------#2023-07-20)

## 使用 ChatGPT 的最新插件构建一个端到端的数据科学项目。

[](https://natassha6789.medium.com/?source=post_page-----e9beb8705dac--------------------------------)![Natassha Selvaraj](https://natassha6789.medium.com/?source=post_page-----e9beb8705dac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9beb8705dac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9beb8705dac--------------------------------) [Natassha Selvaraj](https://natassha6789.medium.com/?source=post_page-----e9beb8705dac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6a2ef1b1f09d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-science-project-with-chatgpt-code-interpreter-e9beb8705dac&user=Natassha+Selvaraj&userId=6a2ef1b1f09d&source=post_page-6a2ef1b1f09d----e9beb8705dac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9beb8705dac--------------------------------) · 12 分钟阅读 · 2023 年 7 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe9beb8705dac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-science-project-with-chatgpt-code-interpreter-e9beb8705dac&user=Natassha+Selvaraj&userId=6a2ef1b1f09d&source=-----e9beb8705dac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe9beb8705dac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-science-project-with-chatgpt-code-interpreter-e9beb8705dac&source=-----e9beb8705dac---------------------bookmark_footer-----------)![](img/2e8aaf8196668f54a02ff892d70ad943.png)

照片由 [Firmbee.com](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/data-analysis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

作为一个目前在全职数据科学工作和多个自由职业项目之间奔波的人，我通常是第一个尝试那些可能缩短我的周转时间的工具的人。

当 ChatGPT 上周开始向订阅者推出代码解释器插件时，我迫不及待地想将其纳入我的数据科学项目中。

如果你还没有听说过这个工具，[代码解释器](https://openai.com/blog/chatgpt-plugins)是一个插件，它允许你在 ChatGPT 界面内上传文档并运行 Python 程序**。**

过去我们手动将数据复制粘贴到 ChatGPT 并等待响应的日子已经一去不复返了。

使用代码解释器，你只需上传数据集，就可以让工具在几分钟内分析数据、构建机器学习模型并生成可视化结果。

在这篇文章中，我将展示如何使用代码解释器执行一个端到端的数据科学项目。

# 任务 — 客户细分

在我之前的公司，我担任市场数据科学家。
