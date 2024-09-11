# 室内建模的3D点云形状检测

> 原文：[https://towardsdatascience.com/3d-point-cloud-shape-detection-for-indoor-modelling-70e36e5f2511?source=collection_archive---------0-----------------------#2023-09-07](https://towardsdatascience.com/3d-point-cloud-shape-detection-for-indoor-modelling-70e36e5f2511?source=collection_archive---------0-----------------------#2023-09-07)

## [动手教程](https://towardsdatascience.com/tagged/hands-on-tutorials)，3D Python

## 一个10步Python指南，用于自动化3D形状检测、分割、聚类和体素化，以进行室内点云数据集的空间占用3D建模。

[](https://medium.com/@florentpoux?source=post_page-----70e36e5f2511--------------------------------)[![Florent Poux, Ph.D.](../Images/74df1e559b2edefba71ffd0d1294a251.png)](https://medium.com/@florentpoux?source=post_page-----70e36e5f2511--------------------------------)[](https://towardsdatascience.com/?source=post_page-----70e36e5f2511--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----70e36e5f2511--------------------------------) [Florent Poux, Ph.D.](https://medium.com/@florentpoux?source=post_page-----70e36e5f2511--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8ba7bf4ad784&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3d-point-cloud-shape-detection-for-indoor-modelling-70e36e5f2511&user=Florent+Poux%2C+Ph.D.&userId=8ba7bf4ad784&source=post_page-8ba7bf4ad784----70e36e5f2511---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----70e36e5f2511--------------------------------) · 28分钟阅读 · 2023年9月7日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F70e36e5f2511&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3d-point-cloud-shape-detection-for-indoor-modelling-70e36e5f2511&user=Florent+Poux%2C+Ph.D.&userId=8ba7bf4ad784&source=-----70e36e5f2511---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F70e36e5f2511&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3d-point-cloud-shape-detection-for-indoor-modelling-70e36e5f2511&source=-----70e36e5f2511---------------------bookmark_footer-----------)

如果你有点云或数据分析的经验，你知道识别模式是多么重要。识别具有相似模式的“对象”或数据点对于获取更有价值的洞察至关重要。我们的视觉认知系统可以轻松完成这个任务，但通过计算方法复制这种人类能力是一项重大挑战。

目标是利用人类视觉系统的自然倾向来分组元素。 👀

![](../Images/f7a3a366e3f8d7cc6c3e1b7a11307505.png)

3D 点云的分割阶段结果示例。© F. Poux

## 但它为什么有用呢？

首先，它通过将数据分组为不同的片段，使你可以轻松访问和处理特定的数据部分。其次，通过关注区域而非单个点，它使数据处理变得更快。这可以节省大量时间和精力。最后，分割有助于你发现一些仅通过查看原始数据无法看到的模式和关系。🔍 总体而言，分割对于从点云数据中获取有用信息至关重要。
