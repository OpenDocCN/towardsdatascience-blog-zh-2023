# 利用聚合的力量

> 原文：[`towardsdatascience.com/leverage-the-power-of-aggregate-ad5d69810c62?source=collection_archive---------12-----------------------#2023-03-09`](https://towardsdatascience.com/leverage-the-power-of-aggregate-ad5d69810c62?source=collection_archive---------12-----------------------#2023-03-09)

## 使用 pandas 聚合来加快数据分析速度

[](https://bradley-stephen-shaw.medium.com/?source=post_page-----ad5d69810c62--------------------------------)![Bradley Stephen Shaw](https://bradley-stephen-shaw.medium.com/?source=post_page-----ad5d69810c62--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ad5d69810c62--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad5d69810c62--------------------------------) [Bradley Stephen Shaw](https://bradley-stephen-shaw.medium.com/?source=post_page-----ad5d69810c62--------------------------------)

·

[阅读更多](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc5cd0a58b5ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleverage-the-power-of-aggregate-ad5d69810c62&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=post_page-c5cd0a58b5ae----ad5d69810c62---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad5d69810c62--------------------------------) · 8 分钟阅读 · 2023 年 3 月 9 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fad5d69810c62&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleverage-the-power-of-aggregate-ad5d69810c62&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=-----ad5d69810c62---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fad5d69810c62&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleverage-the-power-of-aggregate-ad5d69810c62&source=-----ad5d69810c62---------------------bookmark_footer-----------)![](img/c0d790410680428d95a480276d1a6d9f.png)

图片由 [Dan Hadar](https://unsplash.com/@danhadar?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你是否曾因在相同（分段的）特征上编写重复的代码来计算不同统计数据而感到痛苦？

那么，在一片类似的代码片段中徘徊，所有代码都试图计算不同（分段的）特征上的各种统计数据，是否听起来很熟悉？

我能理解你的痛苦。嗯，我以前也有过——但现在没有了！所以在这篇后续文章中，我们将探讨一些简单（但强大）的方式，你可以使用`agg`来：

1.  将不同的函数应用于相同的特征

1.  将一组函数应用于一组特征

1.  使用元组以获得更大的灵活性

1.  使用其他包中的函数，

我提到了功能的分组或分段。虽然你不必在`groupby`语句中使用`agg`，但我认为这样做提供了更有趣的示例，并且与我之前的内容紧密衔接。说到这一点，如果你想复习一下`groupby`、`apply`和`transform`，我建议阅读一下之前的内容。
