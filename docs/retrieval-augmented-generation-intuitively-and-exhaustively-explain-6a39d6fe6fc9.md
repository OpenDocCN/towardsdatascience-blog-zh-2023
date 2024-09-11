# 检索增强生成——直观且全面的解释

> 原文：[https://towardsdatascience.com/retrieval-augmented-generation-intuitively-and-exhaustively-explain-6a39d6fe6fc9?source=collection_archive---------1-----------------------#2023-10-12](https://towardsdatascience.com/retrieval-augmented-generation-intuitively-and-exhaustively-explain-6a39d6fe6fc9?source=collection_archive---------1-----------------------#2023-10-12)

## 制作能够查找信息的语言模型

[](https://medium.com/@danielwarfield1?source=post_page-----6a39d6fe6fc9--------------------------------)[![Daniel Warfield](../Images/c1c8b4dd514f6813e08e401401324bca.png)](https://medium.com/@danielwarfield1?source=post_page-----6a39d6fe6fc9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6a39d6fe6fc9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6a39d6fe6fc9--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----6a39d6fe6fc9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretrieval-augmented-generation-intuitively-and-exhaustively-explain-6a39d6fe6fc9&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----6a39d6fe6fc9---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6a39d6fe6fc9--------------------------------) ·12分钟阅读·2023年10月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6a39d6fe6fc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretrieval-augmented-generation-intuitively-and-exhaustively-explain-6a39d6fe6fc9&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----6a39d6fe6fc9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6a39d6fe6fc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretrieval-augmented-generation-intuitively-and-exhaustively-explain-6a39d6fe6fc9&source=-----6a39d6fe6fc9---------------------bookmark_footer-----------)![](../Images/8a64b639851984b11ef2f5e26a773463.png)

“数据检索器”由Daniel Warfield使用MidJourney制作。所有图片均由作者提供，除非另有说明。

在这篇文章中，我们将探讨“检索增强生成”（RAG），一种策略，它允许我们将最新和相关的信息提供给大型语言模型。我们将讨论其理论，然后想象自己是餐厅经营者；我们将实现一个系统，允许我们的客户与AI交流关于菜单、季节性活动和一般信息的内容。

![](../Images/275875b7b031a6af1c337568b3d25062.png)

实际示例的最终结果是一个可以提供有关我们餐厅特定信息的聊天机器人。

**这对谁有用？** 对任何对自然语言处理（NLP）感兴趣的人都有用。

**这篇文章有多高级？** 这是一个非常强大但非常简单的概念；对初学者和专家都很适用。

**先决条件：** 一些关于大型语言模型（LLMs）的基础知识会很有帮助，但不是必需的。

# 问题的核心

训练大型语言模型（LLMs）非常昂贵；Chat GPT-3 仅在计算资源上就花费了320万美元。如果我们开设了一家新餐厅，并希望使用LLM回答有关菜单的问题，那么如果我们每次都不需要花费数百万美元，那就太好了……
