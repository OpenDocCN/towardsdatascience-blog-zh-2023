# 使用 scienceplots 和 matplotlib 简化科学绘图

> 原文：[`towardsdatascience.com/creating-scientific-plots-the-easy-way-with-scienceplots-and-matplotlib-d86a62e2ab46?source=collection_archive---------2-----------------------#2023-07-17`](https://towardsdatascience.com/creating-scientific-plots-the-easy-way-with-scienceplots-and-matplotlib-d86a62e2ab46?source=collection_archive---------2-----------------------#2023-07-17)

## 通过几行 Python 代码即时转换你的 Matplotlib 图表

[](https://andymcdonaldgeo.medium.com/?source=post_page-----d86a62e2ab46--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----d86a62e2ab46--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d86a62e2ab46--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d86a62e2ab46--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----d86a62e2ab46--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-scientific-plots-the-easy-way-with-scienceplots-and-matplotlib-d86a62e2ab46&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----d86a62e2ab46---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d86a62e2ab46--------------------------------) ·9 分钟阅读·2023 年 7 月 17 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd86a62e2ab46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-scientific-plots-the-easy-way-with-scienceplots-and-matplotlib-d86a62e2ab46&user=Andy+McDonald&userId=9c280f85f15c&source=-----d86a62e2ab46---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd86a62e2ab46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-scientific-plots-the-easy-way-with-scienceplots-and-matplotlib-d86a62e2ab46&source=-----d86a62e2ab46---------------------bookmark_footer-----------)![](img/99d7369c3c290a5d58a483b7d0b9b0da.png)

照片由 [Braňo](https://unsplash.com/fr/@3dparadise?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

在为学术期刊撰写文章时，图表的布局和风格需符合预定义格式。这确保了所有文章的一致性，并确保任何包含的图表在打印时都具有高质量。

Python 在科学界被广泛使用，并提供了一种很好的方法来创建科学图表。然而，当我们使用 matplotlib 这个 Python 中最受欢迎的绘图库时，默认图表质量较差，需要调整以确保符合要求。

修改 matplotlib 图形的样式可能会非常耗时，这时[scienceplots](https://github.com/garrettj403/SciencePlots)库就非常有用。只需几行代码，我们就可以立即改变图形的外观，而无需花费太多时间去调整图形的各个部分。

[scienceplots](https://github.com/garrettj403/SciencePlots)库允许用户创建简单且信息丰富的图表，类似于学术期刊和研究论文中常见的图表。不仅如此，它还将所需的 DPI 设置为 600（对于某些样式），这是出版物常常要求的，以确保图形的高质量打印。
