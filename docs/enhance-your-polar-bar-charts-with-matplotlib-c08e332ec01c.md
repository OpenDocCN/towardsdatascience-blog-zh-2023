# 使用Matplotlib提升您的极坐标条形图

> 原文：[https://towardsdatascience.com/enhance-your-polar-bar-charts-with-matplotlib-c08e332ec01c?source=collection_archive---------5-----------------------#2023-04-24](https://towardsdatascience.com/enhance-your-polar-bar-charts-with-matplotlib-c08e332ec01c?source=collection_archive---------5-----------------------#2023-04-24)

## 通过一些简单的调整来提升您的极坐标条形图数据可视化

[](https://andymcdonaldgeo.medium.com/?source=post_page-----c08e332ec01c--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----c08e332ec01c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c08e332ec01c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c08e332ec01c--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----c08e332ec01c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhance-your-polar-bar-charts-with-matplotlib-c08e332ec01c&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----c08e332ec01c---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c08e332ec01c--------------------------------) ·8分钟阅读·2023年4月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc08e332ec01c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhance-your-polar-bar-charts-with-matplotlib-c08e332ec01c&user=Andy+McDonald&userId=9c280f85f15c&source=-----c08e332ec01c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc08e332ec01c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhance-your-polar-bar-charts-with-matplotlib-c08e332ec01c&source=-----c08e332ec01c---------------------bookmark_footer-----------)![](../Images/117d3fa97589d9986c02fffa1aa742da.png)

使用matplotlib创建的极坐标条形图，并使用mplcyberpunk主题。图像由作者提供。

极坐标条形图是传统条形图的极佳替代选择。这些图表不是在笛卡尔坐标系中水平或垂直排列条形，而是使用极坐标网格来表示它们。

以这种方式绘制数据可以帮助可视化周期性数据，并提供更引人注目和更具美感的数据可视化效果。

极坐标条形图可以轻松地在[matplotlib](https://matplotlib.org/)中创建；然而，默认的图表存在不足，需要稍作修改以使其更具视觉吸引力。

在本简短的教程中，我们将看到如何通过更改样式，甚至为数据应用赛博朋克主题，从而改进基本图表，使其更具视觉吸引力。

# 导入库和设置数据

第一步是导入我们需要的库。在本教程中，我们将使用[numpy](https://numpy.org/)来执行一些计算，并使用[matplotlib](https://matplotlib.org/)来显示我们的图表。

```py
import numpy as np
import matplotlib.pyplot as plt
```

一旦导入了库，我们就可以创建一些数据进行处理。
