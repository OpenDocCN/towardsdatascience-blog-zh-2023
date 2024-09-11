# 从乏味到惊艳：Matplotx 如何提升你的 Matplotlib 图表

> 原文：[https://towardsdatascience.com/from-dull-to-stunning-how-matplotx-can-improve-your-matplotlib-charts-f370cbf7acc?source=collection_archive---------8-----------------------#2023-04-03](https://towardsdatascience.com/from-dull-to-stunning-how-matplotx-can-improve-your-matplotlib-charts-f370cbf7acc?source=collection_archive---------8-----------------------#2023-04-03)

## 简化使用 Matplotx 创建令人惊叹的图表的过程

[](https://andymcdonaldgeo.medium.com/?source=post_page-----f370cbf7acc--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----f370cbf7acc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f370cbf7acc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f370cbf7acc--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----f370cbf7acc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-dull-to-stunning-how-matplotx-can-improve-your-matplotlib-charts-f370cbf7acc&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----f370cbf7acc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f370cbf7acc--------------------------------) ·6分钟阅读·2023年4月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff370cbf7acc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-dull-to-stunning-how-matplotx-can-improve-your-matplotlib-charts-f370cbf7acc&user=Andy+McDonald&userId=9c280f85f15c&source=-----f370cbf7acc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff370cbf7acc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-dull-to-stunning-how-matplotx-can-improve-your-matplotlib-charts-f370cbf7acc&source=-----f370cbf7acc---------------------bookmark_footer-----------)![](../Images/09bb6a98db16792a11f8976944d23011.png)

应用 matplotx 主题的 matplotlib 散点图示例。图片由作者提供。

Matplotlib 是 Python 世界中最受欢迎的数据可视化库之一。然而，多年来，它因创建乏味的图形和使用起来不够方便而声名狼藉。

正如我在最近的文章中所见，将基本的 matplotlib 图表转换为视觉上更吸引人的图形确实需要几行代码。不要误会我；我喜欢使用 matplotlib，因为它非常可定制，但有时我只想要一个时尚的图表而不想费太多劲。

这就是 [**matplotx 库**](https://github.com/nschloe/matplotx) 的作用。只需两行代码——一个 `import` 语句和一个 `with` 语句——我们就可以立即将我们的基本 matplotlib 图形转换成更具视觉吸引力的图形。

matplotx 库提供了一种简便的方法来即时美化你的 matplotlib 图形。这个库已经存在了一段时间，已经被下载了将近 60,000 次，并且 [**每月平均下载 3,000 次**](https://pepy.tech/project/matplotx)（截至撰写本文时）。

在本文中，我们将看到如何使用这个库来为我们的 matplotlib 图形增添一些风格。

# 安装 Matplotx
