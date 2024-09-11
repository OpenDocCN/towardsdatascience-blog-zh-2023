# 16 位、8 位和 4 位浮点格式——它是如何工作的？

> 原文：[`towardsdatascience.com/16-8-and-4-bit-floating-point-formats-how-does-it-work-d157a31ef2ef?source=collection_archive---------0-----------------------#2023-09-30`](https://towardsdatascience.com/16-8-and-4-bit-floating-point-formats-how-does-it-work-d157a31ef2ef?source=collection_archive---------0-----------------------#2023-09-30)

## 让我们深入探讨位和字节

[](https://dmitryelj.medium.com/?source=post_page-----d157a31ef2ef--------------------------------)![Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----d157a31ef2ef--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d157a31ef2ef--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d157a31ef2ef--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----d157a31ef2ef--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F16-8-and-4-bit-floating-point-formats-how-does-it-work-d157a31ef2ef&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----d157a31ef2ef---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d157a31ef2ef--------------------------------) · 13 分钟阅读 · 2023 年 9 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd157a31ef2ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F16-8-and-4-bit-floating-point-formats-how-does-it-work-d157a31ef2ef&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----d157a31ef2ef---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd157a31ef2ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F16-8-and-4-bit-floating-point-formats-how-does-it-work-d157a31ef2ef&source=-----d157a31ef2ef---------------------bookmark_footer-----------)![](img/476c842d2b1c3c19c59cc3d6b6898aa1.png)

图片来自 Adrien Converse 在[Unsplash](https://unsplash.com/@adrienconverse)

50 年来，从 Kernighan、Ritchie 及其《C 语言》第一版开始，人们知道单精度的“float”类型具有 32 位大小，而双精度类型则有 64 位。还有一种 80 位的“long double”类型，具有扩展的精度，所有这些类型几乎涵盖了浮点数据处理的所有需求。然而，近年来，随着大型神经网络模型的出现，开发者们需要进入另一部分领域，并尽可能地缩小浮点类型的大小。

说实话，当我发现 4 位浮点格式存在时感到很惊讶。究竟怎么可能呢？最好的方法是亲自测试。在本文中，我们将探讨最流行的浮点格式，创建一个简单的神经网络，并观察它的工作原理。

让我们开始吧。

## “标准”32 位浮点数

在深入“极端”格式之前，先回顾一个标准格式。[IEEE 754](https://en.wikipedia.org/wiki/IEEE_754)浮点运算标准由电气和电子工程师协会（IEEE）于 1985 年制定。一个典型的 32 位浮点数如下所示：
