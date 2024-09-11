# 你的数据科学可视化将不再相同——Plotly 和 Dash

> 原文：[https://towardsdatascience.com/your-data-science-visualizations-will-never-be-the-same-plotly-dash-6327d07d9efb?source=collection_archive---------2-----------------------#2023-10-24](https://towardsdatascience.com/your-data-science-visualizations-will-never-be-the-same-plotly-dash-6327d07d9efb?source=collection_archive---------2-----------------------#2023-10-24)

## 数据可视化

## 使用 Plotly 和 Dash 创建交互式仪表板

[](https://polmarin.medium.com/?source=post_page-----6327d07d9efb--------------------------------)[![Pol Marin](../Images/a4f69a96717d453db9791f27b8f85e86.png)](https://polmarin.medium.com/?source=post_page-----6327d07d9efb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6327d07d9efb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6327d07d9efb--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----6327d07d9efb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-data-science-visualizations-will-never-be-the-same-plotly-dash-6327d07d9efb&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----6327d07d9efb---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6327d07d9efb--------------------------------) ·14分钟阅读·2023年10月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6327d07d9efb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-data-science-visualizations-will-never-be-the-same-plotly-dash-6327d07d9efb&user=Pol+Marin&userId=1fa43cc443e7&source=-----6327d07d9efb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6327d07d9efb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-data-science-visualizations-will-never-be-the-same-plotly-dash-6327d07d9efb&source=-----6327d07d9efb---------------------bookmark_footer-----------)![](../Images/3e1a35e12cadcbc748b7a5aa4bc77bfb.png)

照片由[艾萨克·史密斯](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

不久前，我写了一篇关于四个 Python 数据可视化库的简单介绍，展示了它们的优缺点，并使用实际示例展示了它们的能力。

由于我们将深入探讨我最喜欢的库，我强烈建议你先查看那篇文章，因为这篇文章将扩展其中展示的内容：

[](/building-interactive-data-visualizations-with-python-the-art-of-storytelling-ceb43db67488?source=post_page-----6327d07d9efb--------------------------------) [## 使用 Python 构建交互式数据可视化——讲故事的艺术

### Seaborn、Bokeh、Plotly 和 Dash 用于有效地传达数据洞察

towardsdatascience.com](/building-interactive-data-visualizations-with-python-the-art-of-storytelling-ceb43db67488?source=post_page-----6327d07d9efb--------------------------------)

今天我们将重点关注**Plotly**[1] 和 **Dash**[2]。为什么是两个？因为它们是相辅相成的。正如我在上述链接的文章中所述，“Dash 并不是一个绘图库。它是一个用于生成仪表板的绝妙框架。”

所以，Plotly 是我们用来绘制图表的库，而 Dash 是我们用来从这些图表生成酷炫交互式仪表板的框架。

这是我们今天构建仪表板的步骤：

+   设置和安装——让我们进入适当的…
