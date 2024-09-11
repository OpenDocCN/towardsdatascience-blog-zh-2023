# 你需要注意的 3 个隐性 Pandas 错误

> 原文：[`towardsdatascience.com/3-silent-pandas-mistakes-you-should-be-aware-of-80d0112de6b5?source=collection_archive---------3-----------------------#2023-08-15`](https://towardsdatascience.com/3-silent-pandas-mistakes-you-should-be-aware-of-80d0112de6b5?source=collection_archive---------3-----------------------#2023-08-15)

## 以及这些错误如何导致隐藏的失败

[](https://sonery.medium.com/?source=post_page-----80d0112de6b5--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----80d0112de6b5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----80d0112de6b5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----80d0112de6b5--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----80d0112de6b5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-silent-pandas-mistakes-you-should-be-aware-of-80d0112de6b5&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----80d0112de6b5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----80d0112de6b5--------------------------------) · 5 min read · 2023 年 8 月 15 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F80d0112de6b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-silent-pandas-mistakes-you-should-be-aware-of-80d0112de6b5&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----80d0112de6b5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F80d0112de6b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-silent-pandas-mistakes-you-should-be-aware-of-80d0112de6b5&source=-----80d0112de6b5---------------------bookmark_footer-----------)![](img/b465b8e4d26e6b8aacd9bf4a3442c2e5.png)

[Malik Earnest](https://unsplash.com/@resolvxd?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/photos/xgxzqRpK0UE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> “愚人的错误被世人皆知，却不被其本人所知。智者的错误只有自己知道，却不被世人所知。” — 查尔斯·凯勒布·科尔顿

不知道我们在编程中犯的错误并不一定会让我们显得愚蠢。然而，这可能会导致不良后果。

有些错误像钻石一样闪耀，可以从远处识别出来。即使你没有注意到它们，编译器（或解释器）也会通过抛出错误来提醒我们。

另一方面，还存在一些“沉默”的错误，这些错误难以察觉，但可能会引发严重问题。

它们不会导致任何错误，但会使函数或操作以不同于你预期的方式执行。因此，结果会发生变化而你未必察觉。

我们将学习这类问题中的三种。

你是一名在零售公司工作的数据分析师。你被要求分析最近进行的一系列促销活动的结果。这个分析中的一个任务是计算每个促销活动的总销售数量和…
