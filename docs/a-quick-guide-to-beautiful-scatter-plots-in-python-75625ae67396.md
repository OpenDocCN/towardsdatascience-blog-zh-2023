# Python中美观散点图的快速指南

> 原文：[https://towardsdatascience.com/a-quick-guide-to-beautiful-scatter-plots-in-python-75625ae67396?source=collection_archive---------10-----------------------#2023-01-12](https://towardsdatascience.com/a-quick-guide-to-beautiful-scatter-plots-in-python-75625ae67396?source=collection_archive---------10-----------------------#2023-01-12)

## 可视化全球预期寿命与人均GDP

[](https://hair-parra.medium.com/?source=post_page-----75625ae67396--------------------------------)[![Hair Parra](../Images/71a377f0415096dc6fb0a10e64b3b28e.png)](https://hair-parra.medium.com/?source=post_page-----75625ae67396--------------------------------)[](https://towardsdatascience.com/?source=post_page-----75625ae67396--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----75625ae67396--------------------------------) [Hair Parra](https://hair-parra.medium.com/?source=post_page-----75625ae67396--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8ab75afba448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-quick-guide-to-beautiful-scatter-plots-in-python-75625ae67396&user=Hair+Parra&userId=8ab75afba448&source=post_page-8ab75afba448----75625ae67396---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----75625ae67396--------------------------------) · 9分钟阅读 · 2023年1月12日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F75625ae67396&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-quick-guide-to-beautiful-scatter-plots-in-python-75625ae67396&user=Hair+Parra&userId=8ab75afba448&source=-----75625ae67396---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F75625ae67396&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-quick-guide-to-beautiful-scatter-plots-in-python-75625ae67396&source=-----75625ae67396---------------------bookmark_footer-----------)![](../Images/960686bb3dcfd9fced9a5a64ae1af07d.png)

图片由作者提供，使用 Python Matplotlib 绘制

所以你已经掌握了一些 Python 和 matplotlib。也许你和我一样，喜欢复杂、美观且富有洞察力的图表。然而，当你尝试自己复现一些基本示例时，如在 [这个文档页面](https://matplotlib.org/stable/gallery/lines_bars_and_markers/scatter_masked.html#sphx-glr-gallery-lines-bars-and-markers-scatter-masked-py) 中所见，你可能会看到如下内容：

这将生成以下图表：

![](../Images/1639ccdc8f7156a3a66b6382425a4058.png)

src: [https://matplotlib.org/stable/gallery/lines_bars_and_markers/scatter_masked.html#sphx-glr-gallery-lines-bars-and-markers-scatter-masked-py](https://matplotlib.org/stable/gallery/lines_bars_and_markers/scatter_masked.html#sphx-glr-gallery-lines-bars-and-markers-scatter-masked-py)

虽然这个图表非常多彩，但相当简单且不太具有洞察力，代码确实解释了它的目的。在这篇文章中，我将展示如何创建像你在本文开头看到的那样美丽且富有洞察力的散点图。

本教程的代码笔记本[可以在这里找到](https://colab.research.google.com/drive/1jJd7fjS_1T5OhOVBdb8nhkQPF7KXBgh0?usp=sharing)，我们将使用的数据集可以在[这个链接](https://ourworldindata.org/grapher/life-expectancy-vs-gdp-per-capita)中找到。请注意，在本教程中，我已将数据挂载到驱动器中，因此你可以…
