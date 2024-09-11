# 紧密性与社区：使用 Python 和 NetworkX 分析社交网络 — 第三部分

> 原文：[https://towardsdatascience.com/closeness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223?source=collection_archive---------13-----------------------#2023-06-26](https://towardsdatascience.com/closeness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223?source=collection_archive---------13-----------------------#2023-06-26)

## 了解如何使用 Python 和 NetworkX 分析社交网络中的社区和紧密中心性

[](https://christineegan42.medium.com/?source=post_page-----c19feeb38223--------------------------------)[![Christine Egan](../Images/d0a11bde52ceaa53d7162f2dd77c8041.png)](https://christineegan42.medium.com/?source=post_page-----c19feeb38223--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c19feeb38223--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c19feeb38223--------------------------------) [Christine Egan](https://christineegan42.medium.com/?source=post_page-----c19feeb38223--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e9b4d1cb38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcloseness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223&user=Christine+Egan&userId=8e9b4d1cb38&source=post_page-8e9b4d1cb38----c19feeb38223---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c19feeb38223--------------------------------) ·6分钟阅读·2023年6月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc19feeb38223&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcloseness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223&user=Christine+Egan&userId=8e9b4d1cb38&source=-----c19feeb38223---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc19feeb38223&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcloseness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223&source=-----c19feeb38223---------------------bookmark_footer-----------)

在 [第2部分](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57)中，我们通过绘制**Smashing Pumpkins**和**Zwan**成员之间的关系，扩展了对社交网络分析的理解。随后，我们研究了像度中心性和中介中心性这样的指标，以调查不同乐队成员之间的关系。同时，我们讨论了领域知识如何帮助我们理解结果。

在第3部分，我们将介绍**接近中心性**的基础知识及其计算方法。接着，我们将演示如何使用 NetworkX 计算接近中心性，以比利·科根的网络作为例子。

![](../Images/fb811d2aacaf00261efd632e95ee8e21.png)

在我的 [GitHub](https://github.com/christine-egan42/networkx-graphs/blob/29f54974ecbc08ea0949e53edbd0fe687dbe84f2/notebooks/NetworkX-Graph-with-Connected-Communities.ipynb) 上获取生成此图的代码。⭐️ 以便于参考。

***在开始之前…***

1.  *你是否有基本的* ***Python*** *知识？如果没有，* [*从这里开始*](https://medium.com/towards-data-science/virtual-environments-for-python-data-science-projects-on-mac-os-big-sur-with-pyenv-and-virtualenv-60db5516bf06)*。*

1.  *你对社交网络分析中的* ***基本概念*** *如节点和边感到熟悉吗？如果没有，* [*从这里开始*](/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-efeb82ab853e)*。*

1.  *你对* ***度中心性*** *和* ***中介中心性*** *感到熟悉吗？如果没有，* [*从这里开始*](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57)*。*

# 接近中心性与社区
