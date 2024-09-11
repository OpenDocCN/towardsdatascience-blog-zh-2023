# 使用Azure“Prompt Flow”在GPT模式下查询文档语料库

> 原文：[https://towardsdatascience.com/querying-a-corpus-of-documents-in-gpt-mode-with-azure-prompt-flow-3a79ec23f59c?source=collection_archive---------12-----------------------#2023-07-21](https://towardsdatascience.com/querying-a-corpus-of-documents-in-gpt-mode-with-azure-prompt-flow-3a79ec23f59c?source=collection_archive---------12-----------------------#2023-07-21)

## 如何自动向量化内容并创建类似于LangChain的机制，以高效查询*语料库*中的文档

[](https://pl-bescond.medium.com/?source=post_page-----3a79ec23f59c--------------------------------)[![Pierre-Louis Bescond](../Images/bb236055962b420fb3ab22088ab28f11.png)](https://pl-bescond.medium.com/?source=post_page-----3a79ec23f59c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3a79ec23f59c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3a79ec23f59c--------------------------------) [Pierre-Louis Bescond](https://pl-bescond.medium.com/?source=post_page-----3a79ec23f59c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ef7c1e10597&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquerying-a-corpus-of-documents-in-gpt-mode-with-azure-prompt-flow-3a79ec23f59c&user=Pierre-Louis+Bescond&userId=4ef7c1e10597&source=post_page-4ef7c1e10597----3a79ec23f59c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3a79ec23f59c--------------------------------) ·6分钟阅读·2023年7月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3a79ec23f59c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquerying-a-corpus-of-documents-in-gpt-mode-with-azure-prompt-flow-3a79ec23f59c&user=Pierre-Louis+Bescond&userId=4ef7c1e10597&source=-----3a79ec23f59c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3a79ec23f59c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquerying-a-corpus-of-documents-in-gpt-mode-with-azure-prompt-flow-3a79ec23f59c&source=-----3a79ec23f59c---------------------bookmark_footer-----------)![](../Images/f304c4feae9387ea63d4e46bccb0dd14.png)

图片来源：[Kenny Eliason](https://unsplash.com/@neonbrand?utm_source=medium&utm_medium=referral) 由 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

# GPT热潮

全球所有精通技术的人们已经玩了一段时间ChatGPT……

+   他们中的许多人将其用作非常聪明的知识数据库🔎，

+   一些人探索了“提示工程”（或“Prompt Engineering”）以获得更相关的结果，有时使用他们自己的数据🤖，

+   但只有少数人进一步利用了像LangChain这样的解决方案来构建复杂的工作流并创建实际应用📚。

的确，掌握像“嵌入”或“向量存储”这样的概念，加上编程要求，对于许多人来说可能显得复杂，从而阻止他们真正发挥LLM的力量。

这就是“[**Prompt Flow**](https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/overview-what-is-prompt-flow?view=azureml-api-2)” 迎刃而解的地方！

让我们来发现如何在Azure中使用低代码构建强大的问答工具吧！

# 先决条件

我会假设你拥有创建本教程所需资源的必要权限，其中最重要的一项是拥有…
