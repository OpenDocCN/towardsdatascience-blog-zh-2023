# 初学者教程：在 Microsoft Azure 中连接 GPT 模型与公司数据

> 原文：[`towardsdatascience.com/beginner-tutorial-connect-gpt-models-with-company-data-in-microsoft-azure-81177929da18?source=collection_archive---------4-----------------------#2023-10-15`](https://towardsdatascience.com/beginner-tutorial-connect-gpt-models-with-company-data-in-microsoft-azure-81177929da18?source=collection_archive---------4-----------------------#2023-10-15)

## 使用 OpenAI Studio、认知搜索和存储账户

[](https://medium.com/@ebbeberge?source=post_page-----81177929da18--------------------------------)![Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----81177929da18--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81177929da18--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----81177929da18--------------------------------) [Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----81177929da18--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7722f981eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeginner-tutorial-connect-gpt-models-with-company-data-in-microsoft-azure-81177929da18&user=Eirik+Berge%2C+PhD&userId=7722f981eb&source=post_page-7722f981eb----81177929da18---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----81177929da18--------------------------------) ·10 分钟阅读·2023 年 10 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F81177929da18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeginner-tutorial-connect-gpt-models-with-company-data-in-microsoft-azure-81177929da18&user=Eirik+Berge%2C+PhD&userId=7722f981eb&source=-----81177929da18---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F81177929da18&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeginner-tutorial-connect-gpt-models-with-company-data-in-microsoft-azure-81177929da18&source=-----81177929da18---------------------bookmark_footer-----------)![](img/d22b9b079dd194d6d903faa1cdd85800.png)

图片由 [Volodymyr Hryshchenko](https://unsplash.com/@lunarts?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 概述

1.  引言

1.  数据设置

1.  创建索引和部署模型

1.  其他考虑

1.  总结

# 引言

在过去一年中，关于 GPT 模型和生成性 AI 的炒作很多。虽然对全面技术革命的承诺可能有些夸大，但**GPT 模型在许多方面确实令人印象深刻**。然而，GPT 模型的真正价值在于将其与内部文档连接起来。这是为什么呢？🤔

当你使用 OpenAI 提供的普通 GPT 模型时，它们并不真正理解你公司内部的运作。如果你向它提问，它将**根据它可能从其他公司获得的信息来回答**。这在你想用 GPT 模型问如下问题时会成为一个问题：

+   内部程序中我必须遵循的步骤是什么？

+   我公司与某个特定客户之间的完整互动历史是什么？

+   如果我遇到任何问题，我应该联系谁…
