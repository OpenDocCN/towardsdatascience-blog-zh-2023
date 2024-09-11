# 用 Python 构建交互式数据可视化：Plotly 入门

> 原文：[https://towardsdatascience.com/building-interactive-data-visualizations-in-python-an-introduction-to-plotly-3ffdd920fc63?source=collection_archive---------7-----------------------#2023-07-19](https://towardsdatascience.com/building-interactive-data-visualizations-in-python-an-introduction-to-plotly-3ffdd920fc63?source=collection_archive---------7-----------------------#2023-07-19)

## 探索交互式数据可视化在数据分析和机器学习中的强大功能

[](https://federicotrotta.medium.com/?source=post_page-----3ffdd920fc63--------------------------------)[![Federico Trotta](../Images/e997e3a96940c16ab5071629016d82fd.png)](https://federicotrotta.medium.com/?source=post_page-----3ffdd920fc63--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3ffdd920fc63--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3ffdd920fc63--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----3ffdd920fc63--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-interactive-data-visualizations-in-python-an-introduction-to-plotly-3ffdd920fc63&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----3ffdd920fc63---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3ffdd920fc63--------------------------------) ·6分钟阅读·2023年7月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3ffdd920fc63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-interactive-data-visualizations-in-python-an-introduction-to-plotly-3ffdd920fc63&user=Federico+Trotta&userId=654cd4bbe899&source=-----3ffdd920fc63---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3ffdd920fc63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-interactive-data-visualizations-in-python-an-introduction-to-plotly-3ffdd920fc63&source=-----3ffdd920fc63---------------------bookmark_footer-----------)![](../Images/694658ab3db881ef69f4b764e3ef4ea2.png)

图片由 [Gerd Altmann](https://pixabay.com/it/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1237457) 提供，来源于 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1237457)

数据可视化是数据专业人士最重要的任务之一。它帮助我们理解数据，并提出更多问题以进行进一步调查。

数据可视化不仅仅是我们在探索性数据分析阶段需要完成的任务。我们还可能需要向观众展示数据，以帮助他们得出一些结论。

在Python中，我们通常使用`matplotlib`和`seaborn`作为绘制图表的库。

然而，有时我们可能需要一些互动式的可视化。在某些情况下，为了更好地理解数据。在其他情况下，只是为了更好地展示我们的解决方案。

在这篇文章中，我们将讨论`plotly`，这是一个用于制作互动式可视化的Python库。

# 什么是Plotly？

正如我们可以在[他们的网站](https://plotly.com/python/)上阅读的那样：

> Plotly的Python图形库可以制作互动式、出版质量的图表。示例包括如何制作折线图、散点图、面积图、条形图、误差条、箱线图、直方图、热图、子图等……
