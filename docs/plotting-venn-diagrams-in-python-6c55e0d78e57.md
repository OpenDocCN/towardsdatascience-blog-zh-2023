# 用 Python 绘制维恩图

> 原文：[https://towardsdatascience.com/plotting-venn-diagrams-in-python-6c55e0d78e57?source=collection_archive---------5-----------------------#2023-02-24](https://towardsdatascience.com/plotting-venn-diagrams-in-python-6c55e0d78e57?source=collection_archive---------5-----------------------#2023-02-24)

## 学习如何使用维恩图来显示两个或更多数据集之间的关系

[](https://weimenglee.medium.com/?source=post_page-----6c55e0d78e57--------------------------------)[![Wei-Meng Lee](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----6c55e0d78e57--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6c55e0d78e57--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6c55e0d78e57--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----6c55e0d78e57--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplotting-venn-diagrams-in-python-6c55e0d78e57&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----6c55e0d78e57---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6c55e0d78e57--------------------------------) ·8分钟阅读·2023年2月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6c55e0d78e57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplotting-venn-diagrams-in-python-6c55e0d78e57&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----6c55e0d78e57---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6c55e0d78e57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplotting-venn-diagrams-in-python-6c55e0d78e57&source=-----6c55e0d78e57---------------------bookmark_footer-----------)![](../Images/2ed9f5f1ccb1be0bc142682d0a50dda0.png)

图片来源：[Dustin Humes](https://unsplash.com/@dustinhumes_photography?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在数据可视化中，我们生成的大多数图表属于以下一种或多种类型：

+   条形图

+   饼图

+   折线图

+   直方图

+   时间序列

然而，一种并不常用的图表类型是**文氏图**。**文氏图**是另一种被严重低估的可视化类型。它实际上是一种非常有用的可视化形式，可以让您检视两组不同数据之间的关系。例如，下面的文氏图展示了两组生物间的关系 — A组（左圈内；有两条腿的生物）和B组（右圈内；会飞的生物）。重叠区域包含那些既有两条腿又会飞的生物。

![](../Images/aa03f8a417428d010024c735ad872a4f.png)

来源：[https://zh.wikipedia.org/wiki/文氏图](https://zh.wikipedia.org/wiki/文氏图)

在本文中，我将向您展示如何从样本数据集绘制一个文氏图。我还将向您展示如何自定义文氏图以修改其外观和风格。
