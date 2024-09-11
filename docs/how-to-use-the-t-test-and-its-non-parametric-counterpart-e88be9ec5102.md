# 如何使用 T 检验及其非参数对应方法

> 原文：[`towardsdatascience.com/how-to-use-the-t-test-and-its-non-parametric-counterpart-e88be9ec5102?source=collection_archive---------3-----------------------#2023-09-17`](https://towardsdatascience.com/how-to-use-the-t-test-and-its-non-parametric-counterpart-e88be9ec5102?source=collection_archive---------3-----------------------#2023-09-17)

## 你真的理解这个你可能在高中学过的检验方法吗？

[](https://murtaza5152-ali.medium.com/?source=post_page-----e88be9ec5102--------------------------------)![Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----e88be9ec5102--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e88be9ec5102--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e88be9ec5102--------------------------------) [Murtaza Ali](https://murtaza5152-ali.medium.com/?source=post_page-----e88be9ec5102--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F607fa603b7ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-the-t-test-and-its-non-parametric-counterpart-e88be9ec5102&user=Murtaza+Ali&userId=607fa603b7ce&source=post_page-607fa603b7ce----e88be9ec5102---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e88be9ec5102--------------------------------) · 5 分钟阅读 · 2023 年 9 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe88be9ec5102&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-the-t-test-and-its-non-parametric-counterpart-e88be9ec5102&user=Murtaza+Ali&userId=607fa603b7ce&source=-----e88be9ec5102---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe88be9ec5102&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-the-t-test-and-its-non-parametric-counterpart-e88be9ec5102&source=-----e88be9ec5102---------------------bookmark_footer-----------)![](img/b74118249cd96f3f39b287a7ca93713c.png)

图片由[Isabella Fischer](https://unsplash.com/@izzyfisch_?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

作为数据科学家，你需要了解一些统计学知识。毕竟，它是该领域的基石之一。

这是一个系列文章中的第一篇，旨在提供对不同统计检验及其应用场景的简明实用概述。信息将保持平衡——既不会冗长复杂到难以理解，也不会过于简短以至于无用。

在这篇文章中，我将讨论一个较为广泛知晓的统计检验：t 检验。即使你不太了解它是什么，你也很可能听说过这个术语。到这篇文章结束时，你应该能理解这个检验的工作原理以及何时使用它。

如果你对统计检验设计一般不熟悉，我强烈推荐你查看本系列的引言文章，*你需要开始进行统计检验的基础概念入门**。*

那么，让我们开始吧。

## 什么是 t 检验？

从本质上讲，t 检验用于比较来自*两个*不同数据样本的数据。它考虑两个因素…
