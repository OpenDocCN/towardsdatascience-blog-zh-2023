# 绿色AI：提高AI可持续性的办法与解决方案

> 原文：[https://towardsdatascience.com/green-ai-methods-and-solutions-to-improve-ai-sustainability-861d69dec658?source=collection_archive---------8-----------------------#2023-06-26](https://towardsdatascience.com/green-ai-methods-and-solutions-to-improve-ai-sustainability-861d69dec658?source=collection_archive---------8-----------------------#2023-06-26)

## 对一个长期被忽视的话题的技术性审视

[](https://pecciaf.medium.com/?source=post_page-----861d69dec658--------------------------------)[![Federico Peccia](../Images/48ad8401c28e87717718f58336cc64cf.png)](https://pecciaf.medium.com/?source=post_page-----861d69dec658--------------------------------)[](https://towardsdatascience.com/?source=post_page-----861d69dec658--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----861d69dec658--------------------------------) [Federico Peccia](https://pecciaf.medium.com/?source=post_page-----861d69dec658--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce527bed0faf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgreen-ai-methods-and-solutions-to-improve-ai-sustainability-861d69dec658&user=Federico+Peccia&userId=ce527bed0faf&source=post_page-ce527bed0faf----861d69dec658---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----861d69dec658--------------------------------) · 9分钟阅读 · 2023年6月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F861d69dec658&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgreen-ai-methods-and-solutions-to-improve-ai-sustainability-861d69dec658&user=Federico+Peccia&userId=ce527bed0faf&source=-----861d69dec658---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F861d69dec658&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgreen-ai-methods-and-solutions-to-improve-ai-sustainability-861d69dec658&source=-----861d69dec658---------------------bookmark_footer-----------)![](../Images/e26e571fe94f4badd859d8df3d7613de.png)

图片由 [Benjamin Davies](https://unsplash.com/@bendavisual?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

如果你打开了这篇文章，你可能已经听说了当前有关大型语言模型（LLMs）安全性和可信度的争议。由计算机科学界知名人物如**史蒂夫·沃兹尼亚克**、**加里·马库斯**和**斯图尔特·拉塞尔**签署的公开信表达了他们对此事的担忧，并要求暂停6个月的LLMs训练。但另一个正在逐渐受到关注的话题，可能会在不久的将来促使另一个公开信的出现，那就是AI模型训练和推理的能源消耗和碳足迹。

据估计，训练流行的GPT-3模型（一个具有1750亿参数的LLM）排放了大约502吨的二氧化碳[1]。甚至有[在线计算器](https://mlco2.github.io/impact/)可以用来估算训练特定模型的排放量。但训练步骤并不是唯一消耗能源的环节。在训练之后，在推理阶段，AI模型每天被执行数千次或数百万次。即使每次执行消耗的能源很少，但在数周、数月和数年内累计的能耗可能会成为一个巨大的问题。
