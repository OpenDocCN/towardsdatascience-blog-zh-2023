# 理解马赛克图

> 原文：[`towardsdatascience.com/understanding-mosaic-plots-fcf148315f4b?source=collection_archive---------19-----------------------#2023-06-13`](https://towardsdatascience.com/understanding-mosaic-plots-fcf148315f4b?source=collection_archive---------19-----------------------#2023-06-13)

## PYTHON | 数据 | 可视化

## 一份全面的指南，教你如何使用 statsmodels 和 Matplotlib 有效地绘制多变量数据集

[](https://david-farrugia.medium.com/?source=post_page-----fcf148315f4b--------------------------------)![David Farrugia](https://david-farrugia.medium.com/?source=post_page-----fcf148315f4b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fcf148315f4b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fcf148315f4b--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----fcf148315f4b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3916826092a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-mosaic-plots-fcf148315f4b&user=David+Farrugia&userId=3916826092a6&source=post_page-3916826092a6----fcf148315f4b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fcf148315f4b--------------------------------) · 7 分钟阅读 · 2023 年 6 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffcf148315f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-mosaic-plots-fcf148315f4b&user=David+Farrugia&userId=3916826092a6&source=-----fcf148315f4b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffcf148315f4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-mosaic-plots-fcf148315f4b&source=-----fcf148315f4b---------------------bookmark_footer-----------)![](img/1c4dabd212163540cd516eceede2f4b4.png)

照片由 [Dimitry B](https://unsplash.com/fr/@dimitry_b?utm_source=medium&utm_medium=referral) 拍摄，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们生活在一个充满数据的世界——一个不断扩展的数字海洋。但在这片海洋中，隐藏着等待被发现的珍贵洞察。

寻找这些珍珠的关键是什么？数据可视化——将原始数据以可视化的方式呈现，使其更易于理解和解读的过程。

通过数据可视化，你为那些原始数字注入了生命，将它们转变为揭示隐藏模式、潜在趋势和数据可能隐藏的关键联系的形式。

在我们用于数据可视化的工具库中，有一个著名的工具——Matplotlib。

这个强大的 Python 库是多功能且稳健的。

在 Matplotlib 的工具包中隐藏着一个你可能未曾遇见的宝石——马赛克图。

这些图提供了一种强大的方式来可视化跨多个维度的分类数据。
