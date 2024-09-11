# 为什么数据科学家应该考虑使用 Fortran

> 原文：[https://towardsdatascience.com/why-you-should-consider-using-fortran-as-a-data-scientist-5511e05ef89?source=collection_archive---------2-----------------------#2023-05-16](https://towardsdatascience.com/why-you-should-consider-using-fortran-as-a-data-scientist-5511e05ef89?source=collection_archive---------2-----------------------#2023-05-16)

## 探索 Fortran 为数据科学和机器学习带来的好处

[](https://medium.com/@egorhowell?source=post_page-----5511e05ef89--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----5511e05ef89--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5511e05ef89--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5511e05ef89--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----5511e05ef89--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-should-consider-using-fortran-as-a-data-scientist-5511e05ef89&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----5511e05ef89---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5511e05ef89--------------------------------) ·8分钟阅读·2023年5月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5511e05ef89&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-should-consider-using-fortran-as-a-data-scientist-5511e05ef89&user=Egor+Howell&userId=1cac491223b2&source=-----5511e05ef89---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5511e05ef89&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-you-should-consider-using-fortran-as-a-data-scientist-5511e05ef89&source=-----5511e05ef89---------------------bookmark_footer-----------)![](../Images/772008d4d916c3876973f8c2bc8645f4.png)

图片由 [Federica Galli](https://unsplash.com/@fedechanw?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

Python 被广泛认为是数据科学的金标准语言，与数据科学相关的各种包、文献和资源始终可用。这不一定是坏事，因为这意味着你可能遇到的任何数据相关问题都有大量的文档解决方案。

然而，随着更大数据集的出现和更复杂模型的兴起，也许是时候探索其他语言了。这时，老牌语言[**Fortran**](https://en.wikipedia.org/wiki/Fortran)可能会再次受到欢迎。因此，今天的数据科学家了解它并尝试实现一些解决方案是值得的。

# 什么是Fortran？

Fortran，简称为公式翻译器，是20世纪50年代首次广泛使用的编程语言。尽管已经有些年头，但它仍然是一种高性能的计算语言，并且[可能比C和C++更快](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/ifc-gpp.html)。

最初为科学家和工程师设计，以运行流体动力学和有机化学等领域的大规模模型和模拟，Fortran至今仍然被频繁使用……
