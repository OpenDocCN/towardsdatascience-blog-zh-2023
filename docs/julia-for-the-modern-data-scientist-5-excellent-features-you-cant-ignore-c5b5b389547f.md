# 《面向现代数据科学家的 Julia：5 个你不能忽视的优秀特性》

> 原文：[`towardsdatascience.com/julia-for-the-modern-data-scientist-5-excellent-features-you-cant-ignore-c5b5b389547f?source=collection_archive---------10-----------------------#2023-05-24`](https://towardsdatascience.com/julia-for-the-modern-data-scientist-5-excellent-features-you-cant-ignore-c5b5b389547f?source=collection_archive---------10-----------------------#2023-05-24)

## 用趣味和智慧进行了解释

[](https://ibexorigin.medium.com/?source=post_page-----c5b5b389547f--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----c5b5b389547f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c5b5b389547f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c5b5b389547f--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----c5b5b389547f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjulia-for-the-modern-data-scientist-5-excellent-features-you-cant-ignore-c5b5b389547f&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----c5b5b389547f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c5b5b389547f--------------------------------) ·9 分钟阅读·2023 年 5 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc5b5b389547f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjulia-for-the-modern-data-scientist-5-excellent-features-you-cant-ignore-c5b5b389547f&user=Bex+T.&userId=39db050c2ac2&source=-----c5b5b389547f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc5b5b389547f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjulia-for-the-modern-data-scientist-5-excellent-features-you-cant-ignore-c5b5b389547f&source=-----c5b5b389547f---------------------bookmark_footer-----------)![](img/f36f5b7ab356b04818fef14c6844a5a1.png)

图片由我使用 Midjourney 制作。

是的，Python 更为广泛使用。是的，它有更多的库。是的，我通过 Python 维生，但这些并不能证明核心本地语言比 Julia 更好。

这就像 iOS 与 Android 的辩论。仅仅因为更多的设备运行 Android（Python 的多种使用场景），并且它有更多的第三方集成（Python 库），这并不意味着 Android（Python）实际上比 iOS（基础版 Julia）更好。

实际上，尽管 Android 拥有庞大的用户基础，但有很多 iOS 功能在多年里一直让 Android 羡慕。在这篇文章中，我们将探讨一些 Julia 的功能，我相信 Python 开发者也会喜欢。

## 1\. 速度

Julia 用户对其语言的速度感到非常自豪——就像父母对自己孩子的自豪一样。他们的这种自豪感是有理由的。Julia 是历史上最快的语言之一，和 C、C++、Fortran 一起属于[Petaflop 组](https://discourse.julialang.org/t/what-makes-a-language-reach-the-petaflop-mark/79963)。

> Petaflops 是一个计算速度单位，相当于每秒一千亿亿（¹⁰¹⁵）次浮点运算。

现在，大多数人没有可以达到 PetaFlops 的机器，因此我们只能尝试一些基本的事情。让我们从…
