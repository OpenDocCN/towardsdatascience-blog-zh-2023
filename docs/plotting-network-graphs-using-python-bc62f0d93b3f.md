# 使用 Python 绘制网络图

> 原文：[https://towardsdatascience.com/plotting-network-graphs-using-python-bc62f0d93b3f?source=collection_archive---------4-----------------------#2023-03-06](https://towardsdatascience.com/plotting-network-graphs-using-python-bc62f0d93b3f?source=collection_archive---------4-----------------------#2023-03-06)

## 学习如何使用 NetworkX 包来可视化复杂网络

[](https://weimenglee.medium.com/?source=post_page-----bc62f0d93b3f--------------------------------)[![魏盟·李](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----bc62f0d93b3f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bc62f0d93b3f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bc62f0d93b3f--------------------------------) [魏盟·李](https://weimenglee.medium.com/?source=post_page-----bc62f0d93b3f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplotting-network-graphs-using-python-bc62f0d93b3f&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----bc62f0d93b3f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bc62f0d93b3f--------------------------------) ·8分钟阅读·2023年3月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbc62f0d93b3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplotting-network-graphs-using-python-bc62f0d93b3f&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----bc62f0d93b3f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbc62f0d93b3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplotting-network-graphs-using-python-bc62f0d93b3f&source=-----bc62f0d93b3f---------------------bookmark_footer-----------)![](../Images/4b615d98b00885dc02862a0e401d3331.png)

图片由 [Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**网络图**是一种可视化形式，使你能够可视化和分析实体之间的关系。例如，下图显示了2013年夏季一个月内维基百科编辑对各种维基百科语言版本的贡献。

![](../Images/dc83a60b182ae123e6d82facacec2ea3.png)

来源：[https://en.wikipedia.org/wiki/Graph_theory#/media/File:Wikipedia_multilingual_network_graph_July_2013.svg](https://en.wikipedia.org/wiki/Graph_theory#/media/File:Wikipedia_multilingual_network_graph_July_2013.svg)

从网络图中，你可以得到一些观察结果：

+   英文（**en**）是所有其他语言翻译成的主要语言；与此同时，许多英文材料也被翻译成其他语言。

+   中文（**zh**）被翻译成日文（**ja**），但反向翻译则不适用。

+   中文和日文材料被翻译成英文，反之亦然。

在这篇文章中，我将向你展示使用**NetworkX**包绘制网络图的基础知识。

# 安装 NetworkX。

要安装 NetworkX 包，请使用`pip`命令：

```py
!pip install networkx
```
