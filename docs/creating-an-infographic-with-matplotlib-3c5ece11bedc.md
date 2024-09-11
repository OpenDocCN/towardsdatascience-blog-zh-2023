# 使用 Matplotlib 创建信息图

> 原文：[`towardsdatascience.com/creating-an-infographic-with-matplotlib-3c5ece11bedc?source=collection_archive---------6-----------------------#2023-07-03`](https://towardsdatascience.com/creating-an-infographic-with-matplotlib-3c5ece11bedc?source=collection_archive---------6-----------------------#2023-07-03)

## 挪威大陆架的志克斯坦组地质岩性变化

[](https://andymcdonaldgeo.medium.com/?source=post_page-----3c5ece11bedc--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----3c5ece11bedc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3c5ece11bedc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c5ece11bedc--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----3c5ece11bedc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-an-infographic-with-matplotlib-3c5ece11bedc&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----3c5ece11bedc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3c5ece11bedc--------------------------------) ·14 min 阅读·2023 年 7 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3c5ece11bedc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-an-infographic-with-matplotlib-3c5ece11bedc&user=Andy+McDonald&userId=9c280f85f15c&source=-----3c5ece11bedc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3c5ece11bedc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-an-infographic-with-matplotlib-3c5ece11bedc&source=-----3c5ece11bedc---------------------bookmark_footer-----------)![](img/05d1ac94916e325b290124a12e01d9cd.png)

挪威大陆架岩性变化的径向条形图。图像由作者提供。

创建令人兴奋且引人入胜的数据可视化对于数据工作和数据科学家至关重要。它使我们能够以简洁的形式向读者提供信息，帮助读者在无需查看原始数据值的情况下理解数据。此外，我们还可以使用图表和图形讲述引人入胜的故事，回答有关数据的一个或多个问题。

在 Python 世界中，有许多库允许数据科学家创建可视化图表，而许多人在开始数据科学旅程时首先接触到的就是[matplotlib](https://matplotlib.org/)。然而，在使用[matplotlib](https://matplotlib.org/)一段时间后，许多人会转向其他更现代的库，因为他们认为基础的 matplotlib 图表既单调又基础。

只需投入一点时间、精力、代码，并了解[matplotlib 的](https://matplotlib.org/)功能，我们就能将基础且单调的图表转变为更具吸引力和视觉美感的作品。

在我过去的几篇文章中，我专注于如何通过各种样式方法转变单个图表。如果你想进一步探索提升 matplotlib 数据可视化的技巧，你可以…
