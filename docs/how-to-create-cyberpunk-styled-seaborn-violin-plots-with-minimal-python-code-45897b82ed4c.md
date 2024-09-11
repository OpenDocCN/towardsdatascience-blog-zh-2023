# 如何使用最少的 Python 代码创建 Cyberpunk 风格的 Seaborn 小提琴图

> 原文：[https://towardsdatascience.com/how-to-create-cyberpunk-styled-seaborn-violin-plots-with-minimal-python-code-45897b82ed4c?source=collection_archive---------14-----------------------#2023-06-26](https://towardsdatascience.com/how-to-create-cyberpunk-styled-seaborn-violin-plots-with-minimal-python-code-45897b82ed4c?source=collection_archive---------14-----------------------#2023-06-26)

## 一个简单的教程，教你如何轻松提升 Seaborn 小提琴图的效果

[](https://andymcdonaldgeo.medium.com/?source=post_page-----45897b82ed4c--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----45897b82ed4c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----45897b82ed4c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----45897b82ed4c--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----45897b82ed4c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-cyberpunk-styled-seaborn-violin-plots-with-minimal-python-code-45897b82ed4c&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----45897b82ed4c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----45897b82ed4c--------------------------------) · 6 分钟阅读 · 2023年6月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F45897b82ed4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-cyberpunk-styled-seaborn-violin-plots-with-minimal-python-code-45897b82ed4c&user=Andy+McDonald&userId=9c280f85f15c&source=-----45897b82ed4c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F45897b82ed4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-cyberpunk-styled-seaborn-violin-plots-with-minimal-python-code-45897b82ed4c&source=-----45897b82ed4c---------------------bookmark_footer-----------)![](../Images/284478862de6863165b69a286d6f6a82.png)

Cyberpunk 风格的小提琴图，展示了井内不同岩性遇到的密度变化。图片由作者提供。

小提琴图是一种常见的数据可视化图表，它将箱形图和密度图的功能结合在一个图表中。这使我们能够在单个图形中可视化更多的信息。例如，我们可以查看箱形图中的基本统计数据，识别可能的异常值，并查看数据的分布。这可以帮助我们理解数据是否存在偏斜或多模态分布。

在我最新的一系列文章中，我一直在探索如何通过各种主题来改进和增强基本的 matplotlib 图形，包括赛博朋克风格。该风格为图形提供了未来感的霓虹般的外观，只需几行代码即可应用于 matplotlib 和 seaborn 图形。

如果你想了解更多，你可以查看我在下面的文章中如何将其应用于 matplotlib 图形。

[## 赛博朋克风格的 Matplotlib 图形

### 用几行代码将你的 Matplotlib 图形从无聊变得有趣

[towardsdatascience.com](/cyberpunking-your-matplotlib-figures-96f4d473185d?source=post_page-----45897b82ed4c--------------------------------)
