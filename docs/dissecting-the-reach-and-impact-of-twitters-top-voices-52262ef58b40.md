# 解剖推特顶尖声音的影响力和覆盖面

> 原文：[`towardsdatascience.com/dissecting-the-reach-and-impact-of-twitters-top-voices-52262ef58b40?source=collection_archive---------13-----------------------#2023-04-06`](https://towardsdatascience.com/dissecting-the-reach-and-impact-of-twitters-top-voices-52262ef58b40?source=collection_archive---------13-----------------------#2023-04-06)

## 利用数据科学绘制推特影响力的全景图

## 深入探讨塑造推特界最有影响力声音的关系和模式

[](https://johnadeojo.medium.com/?source=post_page-----52262ef58b40--------------------------------)![John Adeojo](https://johnadeojo.medium.com/?source=post_page-----52262ef58b40--------------------------------)[](https://towardsdatascience.com/?source=post_page-----52262ef58b40--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----52262ef58b40--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----52262ef58b40--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdissecting-the-reach-and-impact-of-twitters-top-voices-52262ef58b40&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----52262ef58b40---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----52262ef58b40--------------------------------) ·16 min read·2023 年 4 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F52262ef58b40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdissecting-the-reach-and-impact-of-twitters-top-voices-52262ef58b40&user=John+Adeojo&userId=f933e1637e40&source=-----52262ef58b40---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F52262ef58b40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdissecting-the-reach-and-impact-of-twitters-top-voices-52262ef58b40&source=-----52262ef58b40---------------------bookmark_footer-----------)![](img/877907422ebf12aa0b47cd80c3a6882f.png)

图片由作者使用 Midjourney 提示“描绘 2023 年推特状态的巴洛克风格杰作”生成

# 引言

Twitter 一直处于众多争议的中心，因为平台上某些最大账户的影响力。前 100 个 Twitter 账户（按关注人数排序）总关注人数估计约为 41 亿。我们已经看到，Twitter 的顶级声音如何影响政治观点、金融市场、引领潮流，甚至煽动仇恨。作为一名数据科学家，我自然对通过深入分析这些账户的推文可以揭示出什么模式感到好奇。

本文其余部分将深入探讨我理解这些账户影响力的努力，通过检查它们之间的关系。我将使用聚类算法和统计分析来揭示群体内外的模式。我希望能更深入地了解这些顶级声音所具有的影响力的本质。

*前 100 个 Twitter 账户* [*按关注人数排序*](https://socialblade.com/twitter/top/100) *的数据来源于 Social Blade（2023 年 3 月）。*
