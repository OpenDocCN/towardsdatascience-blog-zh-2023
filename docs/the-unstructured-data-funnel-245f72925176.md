# 非结构化数据漏斗

> 原文：[https://towardsdatascience.com/the-unstructured-data-funnel-245f72925176?source=collection_archive---------8-----------------------#2023-12-15](https://towardsdatascience.com/the-unstructured-data-funnel-245f72925176?source=collection_archive---------8-----------------------#2023-12-15)

![](../Images/70a9cff10a032a2a0f68964b01824643.png)

深入多少决定了你的支出。照片由 [Ricardo Gomez Angel](https://unsplash.com/@rgaleriacom?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/black-and-white-round-tunnel-JB3mBjGt94Y?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

## 为什么漏斗是数据领域最强者之间争夺的核心

[](https://medium.com/@hugolu87?source=post_page-----245f72925176--------------------------------)[![Hugo Lu](../Images/045de11463bb16ea70a816ba89118a9e.png)](https://medium.com/@hugolu87?source=post_page-----245f72925176--------------------------------)[](https://towardsdatascience.com/?source=post_page-----245f72925176--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----245f72925176--------------------------------) [Hugo Lu](https://medium.com/@hugolu87?source=post_page-----245f72925176--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F202d118f4cb9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-unstructured-data-funnel-245f72925176&user=Hugo+Lu&userId=202d118f4cb9&source=post_page-202d118f4cb9----245f72925176---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----245f72925176--------------------------------) ·9 min read·2023年12月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F245f72925176&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-unstructured-data-funnel-245f72925176&user=Hugo+Lu&userId=202d118f4cb9&source=-----245f72925176---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F245f72925176&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-unstructured-data-funnel-245f72925176&source=-----245f72925176---------------------bookmark_footer-----------)

*如果你不是 Medium 会员，你可以在此处免费阅读* [*文章*](https://www.getorchestra.io/blog/the-unstructured-data-funnel)*。*

# 介绍

非结构化数据有各种不同的形式。它通常以文本为主，但也可能包含如日期、数字和词典等数据。数据工程师常常遇到的非结构化数据形式是深度 [嵌套的 json](https://www.ibm.com/docs/en/db2/11.5?topic=documents-json-nested-objects)。然而，“非结构化”数据一词实际上指的是任何非表格化的数据；事实上，超过 80% 的 [世界数据是非结构化的](https://www.unleash.so/a/answers/database-management/how-much-data-in-the-world-is-unstructured)。

虽然对我们数据从业者来说，非结构化数据可能看起来无害，但它在宏观层面上正引发巨大的波动。实际上，GPT 模型 [都在训练](https://en.wikipedia.org/wiki/ChatGPT) 非结构化数据。托马斯·通古兹在 Snowflake 的财报电话会议中在一篇 [最近的文章](https://www.linkedin.com/pulse/snow-angels-come-early-data-snowflakes-strength-spells-tomasz-tunguz-bdggc/) 中正确地观察到了这一点：

![](../Images/a8e4d62926abb2208c99028959ff279e.png)

摘自托马斯·通古兹的《雪天使》

在这种金融和宏观经济背景下看待非结构化数据可能显得有些奇怪。我的第一份工作是在投资银行，所以当阅读类似的内容时，我感到有些怀旧。“非结构化数据是增长引擎”对我来说可能是有意义的——这听起来像是一个非常大的市场 [顺风](https://www.peakframeworks.com/post/headwinds-vs-tailwinds)！
