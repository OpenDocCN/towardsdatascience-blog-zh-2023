# Python Pandas 转 Polars：数据筛选

> 原文：[https://towardsdatascience.com/python-pandas-to-polars-data-filtering-a67ccb70a8b3?source=collection_archive---------10-----------------------#2023-04-18](https://towardsdatascience.com/python-pandas-to-polars-data-filtering-a67ccb70a8b3?source=collection_archive---------10-----------------------#2023-04-18)

## 你可能需要尽快完成过渡

[](https://sonery.medium.com/?source=post_page-----a67ccb70a8b3--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----a67ccb70a8b3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a67ccb70a8b3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a67ccb70a8b3--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----a67ccb70a8b3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-pandas-to-polars-data-filtering-a67ccb70a8b3&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----a67ccb70a8b3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a67ccb70a8b3--------------------------------) ·5分钟阅读·2023年4月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa67ccb70a8b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-pandas-to-polars-data-filtering-a67ccb70a8b3&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----a67ccb70a8b3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa67ccb70a8b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-pandas-to-polars-data-filtering-a67ccb70a8b3&source=-----a67ccb70a8b3---------------------bookmark_footer-----------)![](../Images/e58c7f96ccd6dea3680769f176f614b2.png)

图片由 [Daphné Be Frenchie](https://unsplash.com/@daphne_befrenchie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/filtering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

我欣赏 Pandas。我从学习数据科学的第一天起就开始使用它。Pandas 足以应对我在数据清理、预处理和分析中的大部分任务。

我唯一对 Pandas 的问题是当处理大型数据集时。Pandas 进行内存中的分析，因此当数据量非常大时，它的性能会下降。

与数据大小相关的另一个缺点是某些操作会生成中间副本。因此，数据集的大小应该相对小于内存，以便能够高效地工作。

对于如此大的数据集，有不同的替代方案。其中最近获得显著关注的替代方案之一是 Polars。

有许多文章专注于 Polars 相较于 Pandas 的速度，但对实际应用方面的解释较少，比如如何使用 Polars 执行常见的数据清理和操作。

在这一系列文章中，我将向你展示 Polars 版本的常用 Pandas 函数。第一个主题是数据过滤操作。在我们开始例子之前，让我们简要介绍一下 Polars 的特点。

# Polars 提供了什么？
