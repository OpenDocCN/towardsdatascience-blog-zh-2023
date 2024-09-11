# 在框架之外绘图——用Python替代矩形图的8种替代圆形图

> 原文：[https://towardsdatascience.com/plot-outside-the-box-8-alternative-circle-charts-with-python-to-replace-rectangular-charts-36732a98364a?source=collection_archive---------11-----------------------#2023-04-11](https://towardsdatascience.com/plot-outside-the-box-8-alternative-circle-charts-with-python-to-replace-rectangular-charts-36732a98364a?source=collection_archive---------11-----------------------#2023-04-11)

## 使用Python绘制圆形图

[](https://medium.com/@borih.k?source=post_page-----36732a98364a--------------------------------)[![Boriharn K](../Images/1b23a79640f5272c1382918bfdba03b0.png)](https://medium.com/@borih.k?source=post_page-----36732a98364a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----36732a98364a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----36732a98364a--------------------------------) [Boriharn K](https://medium.com/@borih.k?source=post_page-----36732a98364a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe20a7f1ba78f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplot-outside-the-box-8-alternative-circle-charts-with-python-to-replace-rectangular-charts-36732a98364a&user=Boriharn+K&userId=e20a7f1ba78f&source=post_page-e20a7f1ba78f----36732a98364a---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----36732a98364a--------------------------------) ·9 min read·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F36732a98364a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplot-outside-the-box-8-alternative-circle-charts-with-python-to-replace-rectangular-charts-36732a98364a&user=Boriharn+K&userId=e20a7f1ba78f&source=-----36732a98364a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F36732a98364a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplot-outside-the-box-8-alternative-circle-charts-with-python-to-replace-rectangular-charts-36732a98364a&source=-----36732a98364a---------------------bookmark_footer-----------)![](../Images/8f6ebd1b4063f25dd53e954b022dcd21.png)

图片由[Daniel Roe](https://unsplash.com/@danielroe)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在数据可视化中，通常会在矩形区域中绘制图表，例如典型的条形图。使用这些图表的优点是可以占据最大空间。

顺便提一下，显示过多的矩形图表彼此靠近，例如创建一个仪表板，可能会使结果不吸引人或过于密集。

由于矩形图表并不是唯一的选择，一些圆形的替代方案可以替代它们。使用不同的图表风格可能会产生美观的效果。

![](../Images/c547b7061fc21944b882d10ca7f7d172.png)![](../Images/2aed370e64a24ddd7342c223278a5c87.png)

这是一个可以作为矩形图表替代方案的圆形图表示例。图片来源：作者。

在继续之前，我想澄清一下，这篇文章并不反对使用矩形图表。主要目的是提供一些想法，以便读者可以决定哪种最适合他们的用途。

让我们开始吧。

# 替代图表

在这篇文章中，将介绍三种矩形图表及其八种替代方案：

## **1\. 堆叠条形图的替代方案。**
