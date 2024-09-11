# 如何使用Seaborn和Matplotlib创建美丽的条形图（包括动画）

> 原文：[https://towardsdatascience.com/how-to-create-beautiful-bar-charts-with-seaborn-and-matplotlib-including-animation-c2bc9ade1d7c?source=collection_archive---------6-----------------------#2023-06-15](https://towardsdatascience.com/how-to-create-beautiful-bar-charts-with-seaborn-and-matplotlib-including-animation-c2bc9ade1d7c?source=collection_archive---------6-----------------------#2023-06-15)

## 图表教程

## 一个Python数据可视化教程

[](https://medium.com/@oscarleo?source=post_page-----c2bc9ade1d7c--------------------------------)[![Oscar Leo](../Images/7733c9147bad2875a35155fca3903aa8.png)](https://medium.com/@oscarleo?source=post_page-----c2bc9ade1d7c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c2bc9ade1d7c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c2bc9ade1d7c--------------------------------) [Oscar Leo](https://medium.com/@oscarleo?source=post_page-----c2bc9ade1d7c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd7e5c1ca65b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-beautiful-bar-charts-with-seaborn-and-matplotlib-including-animation-c2bc9ade1d7c&user=Oscar+Leo&userId=d7e5c1ca65b7&source=post_page-d7e5c1ca65b7----c2bc9ade1d7c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c2bc9ade1d7c--------------------------------) ·7 min read·2023年6月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc2bc9ade1d7c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-beautiful-bar-charts-with-seaborn-and-matplotlib-including-animation-c2bc9ade1d7c&user=Oscar+Leo&userId=d7e5c1ca65b7&source=-----c2bc9ade1d7c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc2bc9ade1d7c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-beautiful-bar-charts-with-seaborn-and-matplotlib-including-animation-c2bc9ade1d7c&source=-----c2bc9ade1d7c---------------------bookmark_footer-----------)![](../Images/0a763b4611b2c631030faccd3c320cbe.png)

图表由作者创建

你好，欢迎来到我的第一次Matplotlib和Seaborn教程。今天，我将展示如何将一个默认的条形图转换成一个惊艳的视觉效果，配有图标和动画。

如果你喜欢这种内容，请告诉我。如果是这样，我可以创作更多类似的内容！ :)

你可以在这个仓库中找到代码和预处理数据：[simple-bar-chart-tutorial](https://github.com/oscarleoo/simple-bar-chart-tutorial)。

让我们开始吧。

## 步骤 1：创建一个默认的条形图

在本教程中，我创建了一个简单的数据集，记录了三个流行开源深度学习框架（`Tensorflow`，`PyTorch`，和`Keras`）随时间的星级总数。

![](../Images/fbc9983d68b8d3d52d22ac17ccfe9c15.png)

作者截图

这里有一个简单的函数，可以为 DataFrame 中的一行创建一个简单的条形图。

```py
def create_bar_chart(row, color):    
    return sns.barplot(
        y=row.index.str.capitalize().values,
        x=row.values,
        orient="h"…
```
