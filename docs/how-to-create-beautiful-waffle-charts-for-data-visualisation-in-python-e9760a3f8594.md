# 如何在 Python 中创建美观的华夫图进行数据可视化

> 原文：[https://towardsdatascience.com/how-to-create-beautiful-waffle-charts-for-data-visualisation-in-python-e9760a3f8594?source=collection_archive---------6-----------------------#2023-02-08](https://towardsdatascience.com/how-to-create-beautiful-waffle-charts-for-data-visualisation-in-python-e9760a3f8594?source=collection_archive---------6-----------------------#2023-02-08)

## 数据可视化中的一种出色替代图表

[](https://andymcdonaldgeo.medium.com/?source=post_page-----e9760a3f8594--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----e9760a3f8594--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9760a3f8594--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e9760a3f8594--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----e9760a3f8594--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-beautiful-waffle-charts-for-data-visualisation-in-python-e9760a3f8594&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----e9760a3f8594---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9760a3f8594--------------------------------) · 7 分钟阅读 · 2023年2月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe9760a3f8594&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-beautiful-waffle-charts-for-data-visualisation-in-python-e9760a3f8594&user=Andy+McDonald&userId=9c280f85f15c&source=-----e9760a3f8594---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe9760a3f8594&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-beautiful-waffle-charts-for-data-visualisation-in-python-e9760a3f8594&source=-----e9760a3f8594---------------------bookmark_footer-----------)![](../Images/c740dc252169908c5588b10befba75a6.png)

照片由 [Mae Mu](https://unsplash.com/es/@picoftasty?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

华夫图是一种很好的方式来可视化分类数据，它既美观又易于读者理解——这是有效数据可视化的关键目标之一。它们还提供了比饼图更具吸引力的替代方案。

华夫饼图是由较小的方格组成的方形或矩形显示图。最常见的是10 x 10的网格，但你可以根据需要选择任何尺寸，这取决于你要展示的数据。网格中的每个方格都根据类别着色，并代表整体的一部分。通过这些图表，我们可以看到各个类别的贡献或展示向目标的进展。

![](../Images/844d510b8f4f936babcac25b2a44fd6e.png)

显示井内不同岩性的大致图示。图片由作者提供。

华夫饼图有多种用途，包括可视化目标进展、理解组成整体的各个部分，甚至查看支出对毛利润的影响。

## 华夫饼图的优缺点
