# 使用 Matplotlib 创建引人注目的径向条形图

> 原文：[https://towardsdatascience.com/create-eye-catching-radial-bar-charts-with-matplotlib-fd03ff732048?source=collection_archive---------12-----------------------#2023-03-06](https://towardsdatascience.com/create-eye-catching-radial-bar-charts-with-matplotlib-fd03ff732048?source=collection_archive---------12-----------------------#2023-03-06)

## 轻松在 Python 中创建视觉上吸引人的圆形条形图

[](https://andymcdonaldgeo.medium.com/?source=post_page-----fd03ff732048--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----fd03ff732048--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fd03ff732048--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fd03ff732048--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----fd03ff732048--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-eye-catching-radial-bar-charts-with-matplotlib-fd03ff732048&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----fd03ff732048---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd03ff732048--------------------------------) ·9分钟阅读·2023年3月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffd03ff732048&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-eye-catching-radial-bar-charts-with-matplotlib-fd03ff732048&user=Andy+McDonald&userId=9c280f85f15c&source=-----fd03ff732048---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffd03ff732048&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-eye-catching-radial-bar-charts-with-matplotlib-fd03ff732048&source=-----fd03ff732048---------------------bookmark_footer-----------)![](../Images/daff7ea922e980791aef37e03a134f32.png)

使用 matplotlib 创建的径向条形图。图片由作者提供。

径向条形图是传统条形图的视觉上更具吸引力的替代方案。数据在极坐标系统中绘制，而不是传统的笛卡尔坐标系统。这使得条形图可以通过环形而非垂直条形来表示。

径向条形图可以制作出既引人注目又视觉上令人愉悦的图形，适合用于演示或海报中，如果你希望吸引读者的注意。然而，与许多数据可视化一样，它们也有自己的缺点。其中一个问题是它们可能难以解读。这是因为我们的视觉系统在比较类别之间的值时，更擅长解读直线。此外，径向条形图中心的环比外侧的环更难读取和比较。

本文将展示如何使用来自[Python](https://www.python.org/)的[matplotlib](https://matplotlib.org/)库创建一个视觉上吸引人的径向条形图。

提到[matplotlib](https://matplotlib.org/)时，大多数人会想到基本图表，这些图表格式差，并且需要多行尴尬或令人困惑的代码来显示一个优秀的图表。

# 使用 Matplotlib 创建径向条形图
