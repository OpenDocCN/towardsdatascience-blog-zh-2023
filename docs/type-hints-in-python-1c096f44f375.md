# Python 中的类型提示

> 原文：[`towardsdatascience.com/type-hints-in-python-1c096f44f375?source=collection_archive---------7-----------------------#2023-07-11`](https://towardsdatascience.com/type-hints-in-python-1c096f44f375?source=collection_archive---------7-----------------------#2023-07-11)

## 你的代码将不再是一个谜

[](https://polmarin.medium.com/?source=post_page-----1c096f44f375--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----1c096f44f375--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c096f44f375--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c096f44f375--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----1c096f44f375--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftype-hints-in-python-1c096f44f375&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----1c096f44f375---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c096f44f375--------------------------------) · 7 分钟阅读 · 2023 年 7 月 11 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1c096f44f375&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftype-hints-in-python-1c096f44f375&user=Pol+Marin&userId=1fa43cc443e7&source=-----1c096f44f375---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1c096f44f375&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftype-hints-in-python-1c096f44f375&source=-----1c096f44f375---------------------bookmark_footer-----------)![](img/645e61c388600f7ac62f741fec4cf3ab.png)

照片由 [Agence Olloweb](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

前几天我试图解读我过去编写的一个脚本的工作原理。我知道它的功能，它的解释和文档都很完善，但理解其实现方式却更具挑战性。

代码繁琐而复杂，虽然有一些零散的注释，但缺乏适当的格式。这时我决定学习 PEP 8[1]并将其融入我的代码中。

如果你不知道 PEP 8 是什么，它基本上是一份提供 Python 代码编写指南、编码规范和最佳实践的文档。

我们难以理解的代码的解决方案就在眼前。然而，我们中的大多数人从未花时间阅读它并将这些指南融入到日常实践中。

这需要时间和很多错误，但相信我，这是值得的。我学到了很多，现在我的代码开始变得更好了。

我最喜欢的发现之一是**类型提示**（或**类型注解**）——这将是今天帖子的主题。实际上，类型提示已经在 2006 年的 PEP 3107[2]中出现过，并在 2014 年的 484[3]版本中被重新审视并全面记录。从那时起，它在新的 PEP 版本中经过几次改进，几乎已经成为经典。
