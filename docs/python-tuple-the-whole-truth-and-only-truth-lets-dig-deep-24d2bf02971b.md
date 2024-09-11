# Python 元组，整个真相和唯一的真相：深入探讨

> 原文：[https://towardsdatascience.com/python-tuple-the-whole-truth-and-only-truth-lets-dig-deep-24d2bf02971b?source=collection_archive---------2-----------------------#2023-01-27](https://towardsdatascience.com/python-tuple-the-whole-truth-and-only-truth-lets-dig-deep-24d2bf02971b?source=collection_archive---------2-----------------------#2023-01-27)

## PYTHON 编程

## 学习元组的复杂细节。

[](https://medium.com/@nyggus?source=post_page-----24d2bf02971b--------------------------------)[![Marcin Kozak](../Images/d7faf62e48ed81dab5d8ad92819fff54.png)](https://medium.com/@nyggus?source=post_page-----24d2bf02971b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----24d2bf02971b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----24d2bf02971b--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----24d2bf02971b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4762f0cff9b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-tuple-the-whole-truth-and-only-truth-lets-dig-deep-24d2bf02971b&user=Marcin+Kozak&userId=4762f0cff9b2&source=post_page-4762f0cff9b2----24d2bf02971b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----24d2bf02971b--------------------------------) ·24分钟阅读·2023年1月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F24d2bf02971b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-tuple-the-whole-truth-and-only-truth-lets-dig-deep-24d2bf02971b&user=Marcin+Kozak&userId=4762f0cff9b2&source=-----24d2bf02971b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F24d2bf02971b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-tuple-the-whole-truth-and-only-truth-lets-dig-deep-24d2bf02971b&source=-----24d2bf02971b---------------------bookmark_footer-----------)![](../Images/e10e34210758fdd1c9b2276ce2c67e81.png)

元组的不可变性可能会让人感到困惑和头疼。照片由 [Aarón Blanco Tejedor](https://unsplash.com/@innernature?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在上一篇文章中，我们讨论了元组的基础知识：

[](/python-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d?source=post_page-----24d2bf02971b--------------------------------) [## Python 元组，整个真相，仅有的真相：你好，元组！

### 学习元组的基础知识及其使用方法

[towardsdatascience.com](https://towardsdatascience.com/python-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d?source=post_page-----24d2bf02971b--------------------------------)

我向你展示了什么是元组，它提供了哪些方法，最重要的是，我们讨论了元组的不变性。但元组的内容远不止于此，这篇文章是对上一篇文章的继续。你将在这里学习到元组类型的以下方面：

+   元组的复杂性：不变性对复制元组的影响，以及元组类型提示。

+   从元组继承。

+   元组性能：执行时间和内存。

+   元组相对于列表的优势（?）：清晰性、性能，以及作为字典键的元组。

+   元组推导（?）

+   命名元组

# 元组的复杂性
