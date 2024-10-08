# 接近中心性与社区：使用 Python 和 NetworkX 分析社交网络 — 第三部分

> 原文：[`towardsdatascience.com/closeness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223`](https://towardsdatascience.com/closeness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223)

## 了解社交网络分析中的社区和接近中心性，使用 Python 和 NetworkX

[](https://christineegan42.medium.com/?source=post_page-----c19feeb38223--------------------------------)![Christine Egan](https://christineegan42.medium.com/?source=post_page-----c19feeb38223--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c19feeb38223--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c19feeb38223--------------------------------) [Christine Egan](https://christineegan42.medium.com/?source=post_page-----c19feeb38223--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c19feeb38223--------------------------------) ·阅读时间 6 分钟·2023 年 6 月 26 日

--

在 [第二部分](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57)，我们通过绘制 [Smashing Pumpkins](https://smashingpumpkins.com/) 和 [Zwan](https://en.wikipedia.org/wiki/Zwan) 成员之间的关系图，扩展了对社交网络分析的理解。然后，我们考察了度中心性和中介中心性等指标，以调查不同乐队成员之间的关系。同时，我们讨论了领域知识如何帮助我们理解结果。

在第三部分，我们将涵盖**接近中心性**的基础知识及其计算方法。然后，我们将展示如何使用 NetworkX 计算接近中心性，以比利·科根（Billy Corgan）的网络为例。

![](img/fb811d2aacaf00261efd632e95ee8e21.png)

获取生成此图的代码，请访问我的 [GitHub](https://github.com/christine-egan42/networkx-graphs/blob/29f54974ecbc08ea0949e53edbd0fe687dbe84f2/notebooks/NetworkX-Graph-with-Connected-Communities.ipynb)。 ⭐️ 以便于参考。

***开始之前…***

1.  *你有基本的* ***Python*** *知识吗？如果没有，[*从这里开始*](https://medium.com/towards-data-science/virtual-environments-for-python-data-science-projects-on-mac-os-big-sur-with-pyenv-and-virtualenv-60db5516bf06)*。*

1.  *你熟悉* ***社交网络分析中的基础概念***，比如节点和边吗？如果不熟悉，*从这里开始**。*

1.  *你对* ***度中心性*** *和* ***中介中心性*** *感到熟悉吗？如果不熟悉，[*从这里开始*](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57)*。*

# 亲密中心性与社区

## 亲密中心性

**亲密中心性**是社会网络分析中的一种度量，量化了一个节点在网络中与所有其他节点的***最短路径距离***。

亲密中心性关注的是网络中信息或资源流动的效率。其理念是，具有较高亲密中心性的节点能够更快、更高效地到达其他节点，因为它们与网络中其他部分的平均距离更短。

节点的亲密中心性是通过从该节点到网络中所有其他节点的最短路径距离（SPD）之和的倒数来计算的。

> ***亲密中心性 = 1 / (从节点到所有其他节点的 SPD 之和)***

更高的值表示网络中信息流动的中心性和效率更高。

## 计算亲密中心性

让我们用一个简单的八节点网络来解析一下。

1.  计算节点 A 到所有其他节点的最短路径距离（SPD）。在我们的示例中，我们将使用简单的示例距离。实际中，这将使用诸如[广度优先搜索](https://www.hackerearth.com/practice/algorithms/graphs/breadth-first-search/tutorial/\)或[戴克斯特拉算法](https://brilliant.org/wiki/dijkstras-short-path-finder/)的最短路径算法来完成。

2\. 计算从节点 A 到所有其他节点的最短路径距离之和。

3\. 应用亲密中心性公式。

## 亲密性和社区

我们可以将社区视为在自身内部更密集连接的节点组，相比之下与组外节点的连接较少。社区体现了网络中粘合的子组或模块的概念，其中同一社区内的节点彼此有更强的连接。社区的特征是社区内部连接密集而社区之间连接相对稀疏。

![](img/55b2e600a2201c18d8de9ff5b1f54aa9.png)

获取在[我的 GitHub](https://github.com/christine-egan42/networkx-graphs/blob/8f9093cdc3aa2edd27ff4d76d17895d59b7c62a9/notebooks/NetworkX-Community-Graphs.ipynb)上生成此图的代码。⭐️ 以便于参考！

当我们考虑乐队 Smashing Pumpkins 和 Zwan 的成员时，很容易想象这些乐队是如何通过共享的成员相互连接的。这展示了每个乐队内部成员之间的**组内连接**，以及两个乐队之间的**组间连接**。

尽管接近中心性衡量了单个节点的重要性和信息流动效率，但社区捕捉了具有密集连接的紧密子群体。它们共同有助于理解信息流动的动态和网络的组织结构。

让我们讨论几种使用接近中心性和社区来解读网络动态的方法。

1.  **社区内的接近中心性**

属于同一社区的节点通常在社区内具有较高的接近中心性值。这表明社区内的节点彼此紧密连接，并且在最短路径距离方面可以迅速到达对方。社区内较高的接近中心性反映了子群体内信息流动和沟通的高效性。

![](img/22b2ee636d306cfc839d4cbca6062eb0.png)

获取生成此图的代码在 [我的 GitHub](https://github.com/christine-egan42/networkx-graphs/blob/8f9093cdc3aa2edd27ff4d76d17895d59b7c62a9/notebooks/NetworkX-Community-Graphs.ipynb)。⭐️ 以便于参考！

**2\. 用接近中心性桥接社区**

连接不同社区或在社区之间充当桥梁的节点可能具有比单个社区内节点更高的接近中心性。这些节点在连接分离的社区、促进它们之间的沟通和信息流动中发挥着至关重要的作用。

![](img/6f71397ce94f405b1e02268a1c5a6b01.png)

获取生成此图的代码在 [我的 GitHub](https://github.com/christine-egan42/networkx-graphs/blob/8f9093cdc3aa2edd27ff4d76d17895d59b7c62a9/notebooks/NetworkX-Community-Graphs.ipynb)。⭐️ 以便于参考！

**3\. 使用接近中心性进行社区层级分析**

接近中心性也可以在社区层面上用于分析网络中社区的重要性。通过聚合社区内节点的接近中心性值，可以评估社区内信息流动的整体效率。具有较高平均接近中心性的社区可能被认为在网络中更具中心性和影响力，因为它们能够更有效地访问和传播信息。

![](img/b40c1f356ae7ed699da8899069e5bc0b.png)

获取生成此图的代码在 [我的 GitHub](https://github.com/christine-egan42/networkx-graphs/blob/8f9093cdc3aa2edd27ff4d76d17895d59b7c62a9/notebooks/NetworkX-Community-Graphs.ipynb)。⭐️ 以便于参考！

接近中心性衡量了单个节点的重要性和信息流动效率，而社区捕捉了具有密集连接的紧密子群体。它们共同有助于理解信息流动的动态和网络的组织结构。

在考虑 Billy Corgan 的影响范围时，接近中心性可以提供 Smashing Pumpkins 和 Zwan 的成员如何直接和间接影响 Billy Corgan 网络中其他音乐家的见解。我们可以用社区的概念来描述每个乐队，也可以用来描述两个乐队的总和。实际上，1990 年代的另类摇滚音乐圈非常庞大，当我们将更多乐队添加到网络中时，会出现更多的社区。

![](img/44af361a0d5c949ede7c1c8b190b0e56.png)

Billy Corgan 大约在 1991 年 — [由 Barb Vest, CC BY-SA 4.0](https://commons.wikimedia.org/w/index.php?curid=99339690)

# 使用 Python 和 NetworkX 计算接近中心性

1.  就像 [我们在第二部分做的那样](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57)，我们将创建一个函数来生成每个乐队所有成员的组合。

2\. 接下来，我们定义每个乐队，并应用函数生成元组列表。然后，我们将列表合并，并使用列表推导式去除重复项。

3\. 现在我们可以绘制图形了。

它应该看起来像这样：

![](img/24010914bf66ddc999448305ac2ffd6b.png)

4\. 最后，让我们计算接近中心性并分析这些值。

输出应该看起来像这样：

那么我们对这些数值能说些什么呢？

+   Billy Corgan 和 Jimmy Chamberlin 的接近中心性为 1.00，表明他们在迅速联系其他成员方面是最中心的成员。

+   James Iha、Katie Cole、D’arcy Wretzky、Melissa Auf der Maur、Ginger Pooley、Mike Byrne 和 Nicole Fiorentino 的接近中心性值相同，为 0.785714\。这表明这些成员紧密相连，可以快速相互联系。

+   Paz Lenchantin、David Pajo 和 Matt Sweeney 的接近中心性略低，为 0.611111\。这表明他们在联系其他成员方面可能不如前一组，但他们在网络中仍然相对较好地连接。

由于我们仍在处理相对简单的网络，这些结果并没有揭示比计算 Billy Corgan 网络的度中心性和中介中心性时更多的信息。在第四部分，我们将通过引入更多乐队和音乐家到网络中来增加复杂性。作为奖励，我们将介绍一些 [Matplotlib](https://matplotlib.org/) 的高级技巧，使你的 [NetworkX](https://networkx.org/) 图形更具吸引力！

***如果你想要*** [***完整注释的 Python 教程***](https://github.com/christine-egan42/sna-billy-corgan/blob/main/SNA-2-Centrality-Measures.ipynb)***，请访问我的*** [***GitHub***](https://github.com/christine-egan42)***！***

👩🏻‍💻 [Christine Egan](https://christine-egan42.github.io/) | [medium](https://christineegan42.medium.com/) | [github](https://github.com/christine-egan42) | [linkedin](https://www.linkedin.com/in/christineegan42/)

[](https://medium.com/@christineegan42/membership?source=post_page-----c19feeb38223--------------------------------) [## 通过我的推荐链接加入 Medium - Christine Egan

### 成为 Medium 会员后，你的会员费用的一部分将用于支持你阅读的作者，同时你可以获得对每个故事的全面访问权限……

medium.com](https://medium.com/@christineegan42/membership?source=post_page-----c19feeb38223--------------------------------)
