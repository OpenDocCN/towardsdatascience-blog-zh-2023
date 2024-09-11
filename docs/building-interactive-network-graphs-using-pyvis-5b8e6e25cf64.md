# 使用 pyvis 构建互动网络图

> 原文：[https://towardsdatascience.com/building-interactive-network-graphs-using-pyvis-5b8e6e25cf64?source=collection_archive---------8-----------------------#2023-03-06](https://towardsdatascience.com/building-interactive-network-graphs-using-pyvis-5b8e6e25cf64?source=collection_archive---------8-----------------------#2023-03-06)

## 学习如何让你的网络图生动起来

[](https://weimenglee.medium.com/?source=post_page-----5b8e6e25cf64--------------------------------)[![Wei-Meng Lee](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----5b8e6e25cf64--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5b8e6e25cf64--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5b8e6e25cf64--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----5b8e6e25cf64--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-interactive-network-graphs-using-pyvis-5b8e6e25cf64&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----5b8e6e25cf64---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b8e6e25cf64--------------------------------) ·7 分钟阅读·2023年3月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5b8e6e25cf64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-interactive-network-graphs-using-pyvis-5b8e6e25cf64&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----5b8e6e25cf64---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5b8e6e25cf64&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-interactive-network-graphs-using-pyvis-5b8e6e25cf64&source=-----5b8e6e25cf64---------------------bookmark_footer-----------)![](../Images/43f40d53b2431b9caed15c4d55a2ad24.png)

图片由 [DeepMind](https://unsplash.com/@deepmind?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我之前关于创建网络图的文章中，我展示了如何使用 NetworkX 包来构建网络图。NetworkX 的主要问题是生成的图是静态的。一旦图被绘制出来，用户就无法与之互动（例如重新排列节点等）。如果用户可以与图进行互动，网络图将更具直观性（和趣味性！）。因此，这是本文的主要重点。

[](/plotting-network-graphs-using-python-bc62f0d93b3f?source=post_page-----5b8e6e25cf64--------------------------------) [## 使用 Python 绘制网络图

### 学习如何使用 NetworkX 包来可视化复杂的网络

[towardsdatascience.com](/plotting-network-graphs-using-python-bc62f0d93b3f?source=post_page-----5b8e6e25cf64--------------------------------)

在这篇文章中，我将向你展示如何使用`pyvis`包创建一个互动式网络图。

> `pyvis`包是流行的**visJS** JavaScript 库的封装，它允许你轻松地在 Python 中生成可视化网络图。

# 安装 pyvis

要安装`pyvis`包，请使用`pip`命令：

```py
!pip install pyvis
```

# 创建网络
