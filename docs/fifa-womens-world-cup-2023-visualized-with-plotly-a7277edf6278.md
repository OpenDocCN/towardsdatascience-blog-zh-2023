# 2023 年女子世界杯用 Plotly 可视化

> 原文：[`towardsdatascience.com/fifa-womens-world-cup-2023-visualized-with-plotly-a7277edf6278?source=collection_archive---------10-----------------------#2023-08-22`](https://towardsdatascience.com/fifa-womens-world-cup-2023-visualized-with-plotly-a7277edf6278?source=collection_archive---------10-----------------------#2023-08-22)

## 数据科学家的五个图表评论

[](https://medium.com/@caroline.arnold_63207?source=post_page-----a7277edf6278--------------------------------)![Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----a7277edf6278--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a7277edf6278--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7277edf6278--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----a7277edf6278--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9367198e7a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffifa-womens-world-cup-2023-visualized-with-plotly-a7277edf6278&user=Caroline+Arnold&userId=9367198e7a3c&source=post_page-9367198e7a3c----a7277edf6278---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7277edf6278--------------------------------) · 5 min read · 2023 年 8 月 22 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa7277edf6278&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffifa-womens-world-cup-2023-visualized-with-plotly-a7277edf6278&source=-----a7277edf6278---------------------bookmark_footer-----------)![](img/130507d599a7ec632508ddb49fd74304.png)

图片由 [Your Lifestyle Business](https://unsplash.com/@ylblife?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

2023 年 7 月和 8 月，澳大利亚和新西兰联合举办了 FIFA 女子世界杯。共有 32 支国家队参赛，西班牙首次获得冠军。大型体育赛事总是会产生大量数据，我借此机会学习如何使用 Plotly。

[Plotly](https://plot.ly) 是一个开源图形库，用于创建交互式图表。它可以离线或在线使用，并与各种编程语言集成。我使用 Python，因为我最熟悉它，并且创建静态图表。代码可以在 [GitHub](https://github.com/crlna16/medium_notebooks/blob/384a0d07e0aa65e35e7086bf8fe67c1d8e5e679e/plotly/fifa23.ipynb) 上找到。

在五个数据故事中，我们将尝试不同的 plotly 功能，并展示关于世界杯历史和今年比赛的有趣事实：

1.  历史上的世界杯参与情况

1.  球员年龄与比赛表现

1.  球员的俱乐部

1.  送出最多活跃球员的国家

1.  男子和女子世界杯奖金对比

## 1: 参与女子世界杯

从历史上看，许多国家曾禁止女性踢足球。德国足球协会…
