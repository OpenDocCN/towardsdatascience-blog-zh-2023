# 什么是负二项分布

> 原文：[https://towardsdatascience.com/failures-trials-and-successes-the-negative-binomial-distribution-explained-944ea2aef936?source=collection_archive---------9-----------------------#2023-08-17](https://towardsdatascience.com/failures-trials-and-successes-the-negative-binomial-distribution-explained-944ea2aef936?source=collection_archive---------9-----------------------#2023-08-17)

## 深入了解一种较少为人知的概率分布

[](https://medium.com/@egorhowell?source=post_page-----944ea2aef936--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----944ea2aef936--------------------------------)[](https://towardsdatascience.com/?source=post_page-----944ea2aef936--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----944ea2aef936--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----944ea2aef936--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffailures-trials-and-successes-the-negative-binomial-distribution-explained-944ea2aef936&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----944ea2aef936---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----944ea2aef936--------------------------------) ·5 min 阅读·2023年8月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F944ea2aef936&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffailures-trials-and-successes-the-negative-binomial-distribution-explained-944ea2aef936&user=Egor+Howell&userId=1cac491223b2&source=-----944ea2aef936---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F944ea2aef936&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffailures-trials-and-successes-the-negative-binomial-distribution-explained-944ea2aef936&source=-----944ea2aef936---------------------bookmark_footer-----------)![](../Images/de00aa6fcf673800a6d98b7c1af5ff49.png)

图片由 [Alperen Yazgı](https://unsplash.com/@armato?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

也许你听说过[**二项分布**](https://pub.towardsai.net/decoding-the-binomial-distribution-a-fundamental-concept-for-data-scientists-81c64c7e4580)，但你听说过它的“亲戚”——[**负二项分布**](https://en.wikipedia.org/wiki/Negative_binomial_distribution)吗？这种离散概率分布在许多行业中都有应用，如保险和制造业（主要是基于计数的数据），因此对于数据科学家来说是一个有用的概念。在这篇文章中，我们将深入探讨这种分布及其能够解决的问题。

# 什么是负二项分布？

要理解负二项分布，首先需要对二项分布有一定的直觉认识。

二项分布测量在给定次数的试验中获得某个特定次数成功的概率，***x***，在这些试验中，***n*** 是试验的总次数。这些试验是[**伯努利**](https://en.wikipedia.org/wiki/Bernoulli_trial)试验，每次试验的结果都是二元的（成功或失败）。如果你对二项分布不熟悉，可以查看我之前关于…
