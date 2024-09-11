# 如何在 Python 中创建出版质量的热力图

> 原文：[https://towardsdatascience.com/how-to-create-a-publication-quality-heatmap-in-python-e4a7feb3c079?source=collection_archive---------6-----------------------#2023-08-28](https://towardsdatascience.com/how-to-create-a-publication-quality-heatmap-in-python-e4a7feb3c079?source=collection_archive---------6-----------------------#2023-08-28)

## 一份关于 Python 热力图的教程指南

[](https://medium.com/@stephenfordham?source=post_page-----e4a7feb3c079--------------------------------)[![Stephen Fordham](../Images/470e298c01fd910835cabebea346e614.png)](https://medium.com/@stephenfordham?source=post_page-----e4a7feb3c079--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e4a7feb3c079--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e4a7feb3c079--------------------------------) [Stephen Fordham](https://medium.com/@stephenfordham?source=post_page-----e4a7feb3c079--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d3f46276e7e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-publication-quality-heatmap-in-python-e4a7feb3c079&user=Stephen+Fordham&userId=5d3f46276e7e&source=post_page-5d3f46276e7e----e4a7feb3c079---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e4a7feb3c079--------------------------------) ·6分钟阅读·2023年8月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe4a7feb3c079&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-publication-quality-heatmap-in-python-e4a7feb3c079&user=Stephen+Fordham&userId=5d3f46276e7e&source=-----e4a7feb3c079---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe4a7feb3c079&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-publication-quality-heatmap-in-python-e4a7feb3c079&source=-----e4a7feb3c079---------------------bookmark_footer-----------)![](../Images/f7245fadca629866393efbef00ba5822.png)

## 介绍

热力图可以作为信息图表来传达定量数据。它们可以以易于阅读的格式提供简洁的数据总结。

Python 有许多工具可以帮助生成出版质量的热力图。这些工具包括 Seaborn 和 Matplotlib 库，以及 subplot2grid 库，这些库可以提供一种方便的方式来组织热力图中的数据。

在本教程中，我将详细介绍制作一个关注关键元素存在/缺失的热图所需的步骤。为此，我将使用一个包含虚构细菌分离株数据的CSV文件。这些细菌菌株具有多种特征，包括抗生素耐药基因、致病基因以及某些胶囊类型。热图将允许快速检查和比较不同菌株。

虽然所使用的示例集中于细菌菌株，但所应用的技术可以更广泛地用于其他数据集，帮助你通过热图可视化数据。在接下来的教程中，所有图像均由作者提供。

## 目标
