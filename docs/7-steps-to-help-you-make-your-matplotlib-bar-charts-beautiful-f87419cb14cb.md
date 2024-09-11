# 7 个步骤帮助您让 Matplotlib 条形图更美观

> 原文：[`towardsdatascience.com/7-steps-to-help-you-make-your-matplotlib-bar-charts-beautiful-f87419cb14cb?source=collection_archive---------5-----------------------#2023-03-27`](https://towardsdatascience.com/7-steps-to-help-you-make-your-matplotlib-bar-charts-beautiful-f87419cb14cb?source=collection_archive---------5-----------------------#2023-03-27)

## 轻松提升您的 Matplotlib 数据可视化质量，只需几个简单的调整

[](https://andymcdonaldgeo.medium.com/?source=post_page-----f87419cb14cb--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----f87419cb14cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f87419cb14cb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f87419cb14cb--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----f87419cb14cb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-steps-to-help-you-make-your-matplotlib-bar-charts-beautiful-f87419cb14cb&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----f87419cb14cb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f87419cb14cb--------------------------------) · 13 分钟阅读 · 2023 年 3 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff87419cb14cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-steps-to-help-you-make-your-matplotlib-bar-charts-beautiful-f87419cb14cb&user=Andy+McDonald&userId=9c280f85f15c&source=-----f87419cb14cb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff87419cb14cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F7-steps-to-help-you-make-your-matplotlib-bar-charts-beautiful-f87419cb14cb&source=-----f87419cb14cb---------------------bookmark_footer-----------)![](img/0b82852b468b478357866615394725b8.png)

Matplotlib 横向条形图在更改了几个特性后变得更具视觉吸引力。图像由作者提供。

条形图是常用的数据可视化工具，其中分类特征通过不同长度/高度的条形表示。条形的高度或长度对应于该类别所代表的值。

条形图可以很容易地在 [**matplotlib**](https://matplotlib.org/) 中创建。然而，[**matplotlib**](https://matplotlib.org/) 库通常被认为是一个生成无聊图表并且可能很难使用的库。然而，通过毅力、探索以及几行额外的 [**Python**](https://www.python.org/) 代码，我们可以生成独特、美观且信息丰富的图形。

如果你想看看 matplotlib 经过一点额外工作后能做什么，那么你可能会对查看我之前的文章感兴趣：

[](/3-unique-charts-created-with-matplotlib-you-probably-havent-seen-before-421ab8cdd36f?source=post_page-----f87419cb14cb--------------------------------) ## 3 个你不会想到是用 Matplotlib 创建的独特图表

### 利用 Python 的 Matplotlib 创建高级数据可视化

towardsdatascience.com

在这篇文章中，我们将看到如何从像这样的无聊图表：
