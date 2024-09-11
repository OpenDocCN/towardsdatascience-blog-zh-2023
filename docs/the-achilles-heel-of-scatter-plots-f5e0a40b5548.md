# 散点图的致命弱点

> 原文：[`towardsdatascience.com/the-achilles-heel-of-scatter-plots-f5e0a40b5548?source=collection_archive---------3-----------------------#2023-02-05`](https://towardsdatascience.com/the-achilles-heel-of-scatter-plots-f5e0a40b5548?source=collection_archive---------3-----------------------#2023-02-05)

## 使用散点图的替代方法来可视化具有隐藏趋势的大型数据集

[](https://nrlewis929.medium.com/?source=post_page-----f5e0a40b5548--------------------------------)![Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----f5e0a40b5548--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f5e0a40b5548--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f5e0a40b5548--------------------------------) [Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----f5e0a40b5548--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa4cd35f7b702&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-achilles-heel-of-scatter-plots-f5e0a40b5548&user=Nicholas+Lewis&userId=a4cd35f7b702&source=post_page-a4cd35f7b702----f5e0a40b5548---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f5e0a40b5548--------------------------------) ·5 min read·2023 年 2 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff5e0a40b5548&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-achilles-heel-of-scatter-plots-f5e0a40b5548&user=Nicholas+Lewis&userId=a4cd35f7b702&source=-----f5e0a40b5548---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff5e0a40b5548&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-achilles-heel-of-scatter-plots-f5e0a40b5548&source=-----f5e0a40b5548---------------------bookmark_footer-----------)![](img/99820d457e17125ea9fe0f5d11291b10.png)

图片由 [Luke Chesser](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

想一下这个说法：每当你有 x 和 y 数据时，最简单且最有用的可视化方式就是散点图。

这是真的吗？错误的？大部分正确？在什么情况下它不有用甚至令人困惑？你的图表是否传达了你试图沟通的故事或信息，没有任何歧义？这些是在制作数据可视化时你需要考虑的一些问题。

在这篇文章中，我想展示给你一个我学到的非常巧妙的小技巧。作为一名数据科学家，你很可能需要不断处理大量数据，而可视化成为传达你发现的关键。虽然散点图非常适合显示趋势和相关性，但事实是，数据越多，离群值就越多。在散点图中，每一个点都是平等表示的；离群值与那些有助于趋势的点一样清晰地显示出来，如果数据量足够多，它们可能会完全遮挡重要数据。

作为一名数据科学家，你可能会认为首先清理数据的选项是通过一些机器学习算法过滤所有数据，然后绘制结果图，而不是原始数据。虽然这确实很有用，但它并不是…
