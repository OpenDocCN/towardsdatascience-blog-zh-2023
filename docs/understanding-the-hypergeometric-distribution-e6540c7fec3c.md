# 什么是超几何分布

> 原文：[https://towardsdatascience.com/understanding-the-hypergeometric-distribution-e6540c7fec3c?source=collection_archive---------8-----------------------#2023-06-21](https://towardsdatascience.com/understanding-the-hypergeometric-distribution-e6540c7fec3c?source=collection_archive---------8-----------------------#2023-06-21)

## 解析数据科学中较少为人知的分布之一

[](https://medium.com/@egorhowell?source=post_page-----e6540c7fec3c--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----e6540c7fec3c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e6540c7fec3c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e6540c7fec3c--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----e6540c7fec3c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-hypergeometric-distribution-e6540c7fec3c&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----e6540c7fec3c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e6540c7fec3c--------------------------------) ·5分钟阅读·2023年6月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe6540c7fec3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-hypergeometric-distribution-e6540c7fec3c&user=Egor+Howell&userId=1cac491223b2&source=-----e6540c7fec3c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe6540c7fec3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-hypergeometric-distribution-e6540c7fec3c&source=-----e6540c7fec3c---------------------bookmark_footer-----------)![](../Images/bd4a77c4202cc593748342dfdf76aea4.png)

照片由 [Roth Melinda](https://unsplash.com/ko/@rm_photography?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

[**二项分布**](https://medium.com/towards-artificial-intelligence/decoding-the-binomial-distribution-a-fundamental-concept-for-data-scientists-81c64c7e4580)是数据科学内外广为人知的分布。然而，你听说过它不那么受欢迎的“表亲”[**超几何分布**](https://en.wikipedia.org/wiki/Hypergeometric_distribution)吗？如果没有，这篇文章将为你详细解释它是什么以及它为何对我们数据科学家有用。

# 直观感受

超几何分布测量在***n***次试验（一个样本）中获得***k***次成功的概率，*无放回*，前提是对总体有一些信息。这与二项分布非常相似，唯一的关键区别在于采样*无放回*。因此，每次抽样/试验的成功（或结果）概率会发生变化，而在二项分布中，成功（和失败）的概率是固定的。

一个易于理解的例子是确定从一副标准的20张卡牌中随机抽取所有4张国王的概率……
