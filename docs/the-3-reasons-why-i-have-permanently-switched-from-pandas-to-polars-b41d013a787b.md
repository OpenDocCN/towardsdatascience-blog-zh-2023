# 我永久切换从 Pandas 到 Polars 的 3 个原因

> 原文：[https://towardsdatascience.com/the-3-reasons-why-i-have-permanently-switched-from-pandas-to-polars-b41d013a787b?source=collection_archive---------1-----------------------#2023-03-28](https://towardsdatascience.com/the-3-reasons-why-i-have-permanently-switched-from-pandas-to-polars-b41d013a787b?source=collection_archive---------1-----------------------#2023-03-28)

## 我是因为速度而来，但我却因为语法而留下

[](https://medium.com/@benfeifke?source=post_page-----b41d013a787b--------------------------------)[![Ben Feifke](../Images/dd4eb09e404dbd3a478ea1049cf17c90.png)](https://medium.com/@benfeifke?source=post_page-----b41d013a787b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b41d013a787b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b41d013a787b--------------------------------) [Ben Feifke](https://medium.com/@benfeifke?source=post_page-----b41d013a787b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe489235d93b6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-3-reasons-why-i-have-permanently-switched-from-pandas-to-polars-b41d013a787b&user=Ben+Feifke&userId=e489235d93b6&source=post_page-e489235d93b6----b41d013a787b---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b41d013a787b--------------------------------) ·10分钟阅读·2023年3月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb41d013a787b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-3-reasons-why-i-have-permanently-switched-from-pandas-to-polars-b41d013a787b&user=Ben+Feifke&userId=e489235d93b6&source=-----b41d013a787b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb41d013a787b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-3-reasons-why-i-have-permanently-switched-from-pandas-to-polars-b41d013a787b&source=-----b41d013a787b---------------------bookmark_footer-----------)![](../Images/1b105bb38f3f0c27e6b2974660509eae.png)

[照片](https://unsplash.com/photos/qQWV91TTBrE) 由 [Hans-Jurgen Mager](https://unsplash.com/@hansjurgen007) 提供，发布于 [Unsplash](https://unsplash.com/)

在写这篇文章时，我已经进入数据科学领域六年了。在这六年里，`[Pandas](https://pandas.pydata.org/)` 一直是我所有工作的基础：探索性数据分析、影响分析、数据验证、模型实验，等等。我的职业生涯是建立在 `Pandas` 之上的！

不用多说，我曾经深陷 `Pandas` [锁定](https://en.wikipedia.org/wiki/Vendor_lock-in)。

直到我发现了 `[Polars](https://github.com/pola-rs/polars)`，这款全新的“极快的 DataFrame 库”用于 Python。

在这篇文章中，我将解释：

1.  `Polars` 是什么，它为何如此快速；

1.  我从 `Pandas` 永久切换到 `Polars` 的 3 个理由；

    - `.list` 命名空间；

    - `.scan_parquet()` 和 `.sink_parquet()`；

    - 面向数据的编程。

# 介绍 Polars：你（也许）未曾听闻的最快 Python 数据框库。

也许你听说过 `Polars`，也许没有！无论如何，它正在慢慢占领 Python 数据处理领域，从 [Towards Data Science](https://towardsdatascience.com/) 开始：

+   [Leonie Monigatti](https://medium.com/@iamleonie) 最近…
