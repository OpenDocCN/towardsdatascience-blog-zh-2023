# LangChain 中的对话作为有向图

> 原文：[`towardsdatascience.com/conversations-as-directed-graphs-with-lang-chain-46d70e1a846c?source=collection_archive---------0-----------------------#2023-09-25`](https://towardsdatascience.com/conversations-as-directed-graphs-with-lang-chain-46d70e1a846c?source=collection_archive---------0-----------------------#2023-09-25)

## 构建一个旨在理解新潜在客户关键信息的聊天机器人。

[](https://medium.com/@danielwarfield1?source=post_page-----46d70e1a846c--------------------------------)![Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----46d70e1a846c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----46d70e1a846c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----46d70e1a846c--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----46d70e1a846c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconversations-as-directed-graphs-with-lang-chain-46d70e1a846c&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----46d70e1a846c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----46d70e1a846c--------------------------------) ·18 分钟阅读·2023 年 9 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F46d70e1a846c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconversations-as-directed-graphs-with-lang-chain-46d70e1a846c&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----46d70e1a846c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F46d70e1a846c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconversations-as-directed-graphs-with-lang-chain-46d70e1a846c&source=-----46d70e1a846c---------------------bookmark_footer-----------)![](img/e4c31cbbc41f201ecdab003a52889978.png)

图片由 Daniel Warfield 使用 MidJourney 制作。除非另有说明，所有图片均为作者所作。

在这篇文章中，我们将使用 LangChain 在房地产背景下进行潜在客户资格审查。我们设想一个场景，其中新潜在客户首次联系房地产代理。我们将设计一个系统，与新潜在客户沟通，以在房地产代理接手之前提取关键信息。

**这对谁有用？** 对于那些有意在实际环境中应用自然语言处理（NLP）的人来说。

**这篇文章的先进性如何？** 这个例子在概念上是直接的，但如果你对 Python 不够熟悉以及对语言模型缺乏一般性的理解，你可能会难以跟上。

**前提条件：** 需要具备基本的 Python 编程知识，以及对语言模型有较高的理解。

# 问题描述

这个用例直接来源于我作为承包商时收到的一项工作请求。潜在客户拥有一家房地产公司，并发现他们的代理人在每次对话开始时都花费大量时间执行相同的重复任务：潜在客户资格认证。
