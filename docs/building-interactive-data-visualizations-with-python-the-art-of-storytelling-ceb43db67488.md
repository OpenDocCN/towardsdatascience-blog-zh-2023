# 用 Python 构建交互式数据可视化——讲故事的艺术

> 原文：[`towardsdatascience.com/building-interactive-data-visualizations-with-python-the-art-of-storytelling-ceb43db67488?source=collection_archive---------1-----------------------#2023-06-04`](https://towardsdatascience.com/building-interactive-data-visualizations-with-python-the-art-of-storytelling-ceb43db67488?source=collection_archive---------1-----------------------#2023-06-04)

## 入门指南

## Seaborn、Bokeh、Plotly 和 Dash 用于有效传达数据见解

[](https://polmarin.medium.com/?source=post_page-----ceb43db67488--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----ceb43db67488--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ceb43db67488--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ceb43db67488--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----ceb43db67488--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-interactive-data-visualizations-with-python-the-art-of-storytelling-ceb43db67488&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----ceb43db67488---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ceb43db67488--------------------------------) ·16 min read·2023 年 6 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fceb43db67488&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-interactive-data-visualizations-with-python-the-art-of-storytelling-ceb43db67488&user=Pol+Marin&userId=1fa43cc443e7&source=-----ceb43db67488---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fceb43db67488&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-interactive-data-visualizations-with-python-the-art-of-storytelling-ceb43db67488&source=-----ceb43db67488---------------------bookmark_footer-----------)![](img/944921b12b326517582e6fc337dd6090.png)

图片由[Nong](https://unsplash.com/de/@californong?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

讲故事的艺术对于数据科学家很重要，对于数据分析师更是至关重要。

与对数据不熟悉、甚至可能没有技术背景的人分享数据见解和亮点是数据分析旅程中最重要的部分之一。

如果他们无法理解或未能参与你所说的内容，他们不在乎你在数据清理和转换方面有多么出色。

可视化因此是最终讲述故事的一部分，它无疑是展示任何事实的最佳方式——这就是为什么大家都在使用它们。

此外，像 Tableau 或 Power BI 这样的工具由于能够轻松创建交互式仪表板而越来越受到青睐。商业人士通常对带有图表和颜色的炫酷仪表板感到惊讶，因此他们已经开始将其作为招聘要求。

今天，我将与大家分享一些 Python 选项，用于创建交互式可视化，特别是对于那些无法或不喜欢/不想使用这些特定数据可视化工具的人。
