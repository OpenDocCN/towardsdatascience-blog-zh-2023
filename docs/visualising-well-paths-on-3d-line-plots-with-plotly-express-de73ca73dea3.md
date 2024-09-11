# 使用 Plotly Express 可视化 井路径

> 原文：[https://towardsdatascience.com/visualising-well-paths-on-3d-line-plots-with-plotly-express-de73ca73dea3?source=collection_archive---------7-----------------------#2023-06-04](https://towardsdatascience.com/visualising-well-paths-on-3d-line-plots-with-plotly-express-de73ca73dea3?source=collection_archive---------7-----------------------#2023-06-04)

## 使用 Plotly Express 绘制 3D 线路图

[](https://andymcdonaldgeo.medium.com/?source=post_page-----de73ca73dea3--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----de73ca73dea3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----de73ca73dea3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----de73ca73dea3--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----de73ca73dea3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualising-well-paths-on-3d-line-plots-with-plotly-express-de73ca73dea3&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----de73ca73dea3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----de73ca73dea3--------------------------------) ·7 分钟阅读·2023年6月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fde73ca73dea3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualising-well-paths-on-3d-line-plots-with-plotly-express-de73ca73dea3&user=Andy+McDonald&userId=9c280f85f15c&source=-----de73ca73dea3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fde73ca73dea3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualising-well-paths-on-3d-line-plots-with-plotly-express-de73ca73dea3&source=-----de73ca73dea3---------------------bookmark_footer-----------)![](../Images/fec699f8b9ec7e99ce706be52f31fe85.png)

使用 Plotly Express 可视化 3D 井路径。图像由作者提供。

可视化是我们理解井记录数据和地下数据的重要任务之一。这包括在井记录图、散点图和直方图上查看数据。通过这样做，我们可以对数据有一个扎实的理解。然而，仅仅使用 2D 图形有时是不够的，我们需要通过转向 3D 来获得额外的维度。

在岩石物理学和地球科学中，3D 可视化的一个优秀应用案例是可视化井的路径。

在石油和天然气勘探的早期，井是垂直钻入地下的。然而，随着技术的发展，行业从垂直钻探转向水平钻探，最终转向利用地质引导创新的复杂井路径钻探。

这种井路径几何的演变强调了在三维中可视化井路径的重要性。这样做使我们能够更好地理解井如何穿透地质层，并计划未来的井以避免碰撞或问题。

在本文中，我们将探讨如何使用井测量数据，该数据记录了井的位置，并利用 Plotly Express 的 3D 线图进行展示。
