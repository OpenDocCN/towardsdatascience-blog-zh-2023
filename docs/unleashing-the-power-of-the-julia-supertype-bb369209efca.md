# 释放Julia超类型的力量

> 原文：[https://towardsdatascience.com/unleashing-the-power-of-the-julia-supertype-bb369209efca?source=collection_archive---------6-----------------------#2023-10-11](https://towardsdatascience.com/unleashing-the-power-of-the-julia-supertype-bb369209efca?source=collection_archive---------6-----------------------#2023-10-11)

## 使用和运用抽象来用Julia语言做有趣的事情

[](https://emmaccode.medium.com/?source=post_page-----bb369209efca--------------------------------)[![艾玛·布德罗](../Images/f7201d012b733643d6e97957f73fd1fa.png)](https://emmaccode.medium.com/?source=post_page-----bb369209efca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb369209efca--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bb369209efca--------------------------------) [艾玛·布德罗](https://emmaccode.medium.com/?source=post_page-----bb369209efca--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fea170050148c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-the-julia-supertype-bb369209efca&user=Emma+Boudreau&userId=ea170050148c&source=post_page-ea170050148c----bb369209efca---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb369209efca--------------------------------) ·7分钟阅读·2023年10月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb369209efca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-the-julia-supertype-bb369209efca&user=Emma+Boudreau&userId=ea170050148c&source=-----bb369209efca---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb369209efca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funleashing-the-power-of-the-julia-supertype-bb369209efca&source=-----bb369209efca---------------------bookmark_footer-----------)![](../Images/709919bb56394b82ea33ff5474146f8f.png)

图片来源：作者

## 介绍

我发现探索不同编程语言最吸引人的地方之一就是不同范式解决不同类型问题的方式。在编程中，有一些特性是现代编程语言所期望的。这些特性的例子包括作用域、多态性和抽象。一些范式在特定领域的应用会更加突出，数据科学也不例外。这些特性通过多种不同的方法在编程范式的范围内得到了实现。当一个具有不典型范式的语言实现这些类型的通用编程概念时，这个话题对我来说就更加有趣了。这就引出了 Julia 编程语言。

Julia 编程语言是近年来编程领域中较为有趣的发展之一。编程世界中已经习惯了几种经过几十年打磨的选定范式，这些范式创造了一些非常强大的意识形态方法来解决编程问题。虽然 Julia 借鉴了许多更通用的…
