# 适应现有 LLM 项目以使用 LangChain

> 原文：[`towardsdatascience.com/adapting-existing-llm-projects-to-use-langchain-cd07028c01b0?source=collection_archive---------6-----------------------#2023-07-01`](https://towardsdatascience.com/adapting-existing-llm-projects-to-use-langchain-cd07028c01b0?source=collection_archive---------6-----------------------#2023-07-01)

## 使用 LangChain 重构 OpenAI 调用，使其更强大

[](https://medium.com/@oaguy1?source=post_page-----cd07028c01b0--------------------------------)![Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----cd07028c01b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd07028c01b0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd07028c01b0--------------------------------) [Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----cd07028c01b0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5389e25ca1bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadapting-existing-llm-projects-to-use-langchain-cd07028c01b0&user=Lily+Hughes-Robinson&userId=5389e25ca1bb&source=post_page-5389e25ca1bb----cd07028c01b0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd07028c01b0--------------------------------) ·6 分钟阅读·2023 年 7 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcd07028c01b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadapting-existing-llm-projects-to-use-langchain-cd07028c01b0&user=Lily+Hughes-Robinson&userId=5389e25ca1bb&source=-----cd07028c01b0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcd07028c01b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadapting-existing-llm-projects-to-use-langchain-cd07028c01b0&source=-----cd07028c01b0---------------------bookmark_footer-----------)![](img/814da28df7832cee3963ba401e7ba764.png)

图片由 [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

恭喜你，你有了一个可以自豪地展示给世界的工作中的 LLM 概念验证！也许你直接使用了 OpenAI 库，或者使用了其他基础模型和 HuggingFace transformers。不管怎样，你都付出了很多努力，现在你在寻找下一步。这可能意味着代码重构、支持多个基础模型，或添加更高级的功能，如代理或向量数据库。这就是 LangChain 的用武之地。

[LangChain](https://langchain.com) 是一个用于处理 LLM 并在其上开发应用程序的开源框架。它有 Python 和 JavaScript 版本，并支持多个基础模型和 LLM 提供商。它还拥有许多实用函数，用于处理常见任务，如文档分块或与向量数据库互动。相信这值得一试？看看这篇优秀的文章，它是开始了解可能性的大好起点：
