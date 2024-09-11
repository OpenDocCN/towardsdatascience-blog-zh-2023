# 如何使用 Plotly Graph Objects 构建瀑布图

> 原文：[https://towardsdatascience.com/how-to-build-waterfall-charts-with-plotly-graph-objects-a8354543c42e?source=collection_archive---------6-----------------------#2023-09-14](https://towardsdatascience.com/how-to-build-waterfall-charts-with-plotly-graph-objects-a8354543c42e?source=collection_archive---------6-----------------------#2023-09-14)

## Plotly Express 不支持瀑布图，但我们可以创建一个利用 Plotly Graph Objects 的辅助函数

[](https://medium.com/@alan-jones?source=post_page-----a8354543c42e--------------------------------)[![艾伦·琼斯](../Images/359379fab1d6685ff08080b98173e67c.png)](https://medium.com/@alan-jones?source=post_page-----a8354543c42e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a8354543c42e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a8354543c42e--------------------------------) [艾伦·琼斯](https://medium.com/@alan-jones?source=post_page-----a8354543c42e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d3f5fb94faa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-waterfall-charts-with-plotly-graph-objects-a8354543c42e&user=Alan+Jones&userId=7d3f5fb94faa&source=post_page-7d3f5fb94faa----a8354543c42e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a8354543c42e--------------------------------) ·8 分钟阅读·2023年9月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa8354543c42e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-waterfall-charts-with-plotly-graph-objects-a8354543c42e&user=Alan+Jones&userId=7d3f5fb94faa&source=-----a8354543c42e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa8354543c42e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-waterfall-charts-with-plotly-graph-objects-a8354543c42e&source=-----a8354543c42e---------------------bookmark_footer-----------)![](../Images/7e1bc52e691c1d6e2bec6531912b3380.png)

图片由作者提供

Plotly 提供了两种绘制图表的方式：Graph Objects 和 Plotly Express。前者是一组低级函数，提供最大灵活性以创建图表，而 Plotly Express 则提供了一组易于使用的方法，实现了最常用的图表。

Plotly Express 函数本质上是 Plotly Graph Objects 的包装器。

但在 Plotly Express 中没有 Waterfall Chart 方法，所以我们将展示一个简单易用的瀑布图函数，这可能是最常见的用例，并且具有处理更复杂用法的灵活性。

## 瀑布图

瀑布图有点像被拆分成多个列的条形图。它们通常用于显示某个值随时间的增减情况。例如，以下是一些数据。

```py
labels = ["Start balance", "Consulting", "Net revenue", 
          "Purchases", "Other expenses", "Profit before tax"]
data = [20, 80, 10, -40, -20, 0 ]
```
