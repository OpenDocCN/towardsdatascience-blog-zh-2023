# 面向数据科学家的变异函数教程，用于量化空间连续性

> 原文：[`towardsdatascience.com/a-data-scientist-friendly-variogram-tutorial-for-quantifying-spatial-continuity-1d2f29dcfb51?source=collection_archive---------2-----------------------#2023-08-20`](https://towardsdatascience.com/a-data-scientist-friendly-variogram-tutorial-for-quantifying-spatial-continuity-1d2f29dcfb51?source=collection_archive---------2-----------------------#2023-08-20)

## 应用于使用开源 GSLib 和 Python 的合成采矿数据集

[](https://fouadfaraj.medium.com/?source=post_page-----1d2f29dcfb51--------------------------------)![Fouad Faraj](https://fouadfaraj.medium.com/?source=post_page-----1d2f29dcfb51--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d2f29dcfb51--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d2f29dcfb51--------------------------------) [Fouad Faraj](https://fouadfaraj.medium.com/?source=post_page-----1d2f29dcfb51--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb4554eb44266&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientist-friendly-variogram-tutorial-for-quantifying-spatial-continuity-1d2f29dcfb51&user=Fouad+Faraj&userId=b4554eb44266&source=post_page-b4554eb44266----1d2f29dcfb51---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d2f29dcfb51--------------------------------) ·7 分钟阅读·2023 年 8 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1d2f29dcfb51&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientist-friendly-variogram-tutorial-for-quantifying-spatial-continuity-1d2f29dcfb51&user=Fouad+Faraj&userId=b4554eb44266&source=-----1d2f29dcfb51---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1d2f29dcfb51&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientist-friendly-variogram-tutorial-for-quantifying-spatial-continuity-1d2f29dcfb51&source=-----1d2f29dcfb51---------------------bookmark_footer-----------)![](img/04289e391ed2f5914b22f2a145b395c8.png)

照片由 [Sebastian Pichler](https://unsplash.com/@pichler_sebastian?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄

# 介绍

变差函数用于展示空间数据的基于距离的变异性。理解和建模空间连续性非常重要，因为变差函数用于将点测量估算到实际的块中，广泛应用于如矿石品位、石油浓度或环境污染物等领域。

尽管有开源选项可以生成变差函数，但由于其复杂性，大多数用户依赖于昂贵的软件包，这些软件包抽象化了许多细节。本教程旨在简要介绍变差函数以及如何使用开源的地统计学库（GSLib），该库可以独立使用或与 Python 一起使用来开发变差函数。

在这里，我们在一个合成的矿业数据集上开发了一个变差函数模型，但该工作流可以用于任何类型的空间数据，如气象应用中的温度，或环境应用中的污染物追踪。

## 教程要求

我们需要 GSLib，可以从[这里](http://gslib.com/main_gd.html)免费下载，并且还有一些最…
