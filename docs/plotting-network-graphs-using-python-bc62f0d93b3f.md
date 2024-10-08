# 使用 Python 绘制网络图

> 原文：[`towardsdatascience.com/plotting-network-graphs-using-python-bc62f0d93b3f`](https://towardsdatascience.com/plotting-network-graphs-using-python-bc62f0d93b3f)

## 了解如何使用 NetworkX 包来可视化复杂网络

[](https://weimenglee.medium.com/?source=post_page-----bc62f0d93b3f--------------------------------)![Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----bc62f0d93b3f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bc62f0d93b3f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bc62f0d93b3f--------------------------------) [魏孟李](https://weimenglee.medium.com/?source=post_page-----bc62f0d93b3f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bc62f0d93b3f--------------------------------) ·阅读时间 8 分钟·2023 年 3 月 6 日

--

![](img/4b615d98b00885dc02862a0e401d3331.png)

图片由 [Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

**网络图**是一种可视化形式，使你能够可视化和分析实体之间的关系。例如，下图展示了 2013 年夏季一个月内维基百科编辑对各种维基百科语言版本的贡献。

![](img/dc83a60b182ae123e6d82facacec2ea3.png)

来源: [`en.wikipedia.org/wiki/Graph_theory#/media/File:Wikipedia_multilingual_network_graph_July_2013.svg`](https://en.wikipedia.org/wiki/Graph_theory#/media/File:Wikipedia_multilingual_network_graph_July_2013.svg)

从网络图中，你可以得出一些观察结论：

+   英语 (**en**) 是主要的语言，所有其他语言都翻译成英语；同时，许多英语材料也被翻译成其他语言

+   中文 (**zh**) 被翻译成日语 (**ja**)，但反向翻译则不然

+   中文和日语材料都被翻译成英语，反之亦然

在这篇文章中，我将向你展示如何使用**NetworkX**包绘制网络图的基础知识。

# 安装 NetworkX

要安装 NetworkX 包，请使用`pip`命令：

```py
!pip install networkx
```

# 一些图的术语

在开始绘制你的网络图之前，了解一些基本的网络图术语是很有用的。

下图展示了一个*有向图*（也称为*有向图*；图中的边有方向，如箭头所示）：

![](img/48b49908fd2c9dd6a2b211a83f2c279e.png)

所有图片均由作者提供

+   ***节点***是图中的基本单元。节点也通常被称为***顶点***。

+   ***边***是图中两个节点之间的连接。

与此同时，*无向图*没有节点之间的方向（因此没有箭头），而且边是双向的。

# 创建图形

现在开始创建一个网络图。我们将逐步进行。

首先，创建一个`networkx.classes.graph.Graph`对象：

```py
import networkx as nx

G = nx.Graph()
print(G)

# Graph with 0 nodes and 0 edges
```

`nx.Graph()`类创建一个无向图。如果你想创建一个有向图，请使用`nx.DiGraph(directed=True)`，它返回一个`networkx.classes.digraph.DiGraph`对象。

> 我们将在本文后面讨论有向图。

## 添加节点

创建图形（`G`）后，你现在需要向其中添加一些节点：

```py
G.add_node("Singapore")
G.add_node("San Francisco")
G.add_node("Tokyo")

print(G)
# Graph with 3 nodes and 0 edges
```

上述代码片段向图形添加了三个节点，但尚未定义边。除了使用`add_node()`函数添加单个节点外，你还可以使用`add_nodes_from()`函数一次性添加多个节点：

```py
G.add_nodes_from(["Riga", "Copenhagen"])
print(G)
# Graph with 5 nodes and 0 edges
```

目前你的图形有五个节点。

## 添加边

定义了节点后，你现在可以添加边以连接它们：

```py
G.add_edge("Singapore","San Francisco")
G.add_edge("San Francisco","Tokyo")
G.add_edges_from(
    [
        ("Riga","Copenhagen"),
        ("Copenhagen","Singapore"),
        ("Singapore","Tokyo"),
        ("Riga","San Francisco"),
        ("San Francisco","Singapore"),
    ]
)

print(G)
# Graph with 5 nodes and 6 edges
```

与节点一样，你可以使用`add_edge()`函数添加单独的边，或者使用`add_edges_from()`函数添加多个边（只需提供表示每条边的元组列表）。

## 绘制图形

在图形中添加了节点和边后，你现在可以使用`draw()`函数来可视化图形：

```py
nx.draw(G)
```

你应该会看到类似这样的结果：

![](img/bdf06548d0ee93b061ab306c7363777a.png)

> 请注意，每次调用`draw()`函数时，你都会得到不同的图形。

这是同一图形的另一种变体：

![](img/e61a884b17785ab53730a11f07f4384e.png)

## 显示标签

显然，没有标签的图形不是很有用（如果完全可用的话！）。所以让我们绘制一个带有节点标签的图：

```py
nx.draw(G, with_labels = True)
```

> 上述`draw()`函数与`with_labels`参数等同于调用以下函数：

+   `nx.draw_networkx_nodes()` — 绘制图形中的所有节点

+   `nx.draw_networkx_labels()` — 在每个节点上绘制标签

+   `nx.draw_networkx_edges()` — 绘制连接节点的边

> 那么使用`draw()`函数与其他函数相比有什么优势呢？实际上，使用各个函数绘图可以让你自定义单个节点、标签和边的外观。

现在你可以看到每个节点的标签：

![](img/de02687a5212888561c6a3b8c0bb473a.png)

## 使用布局

记住`draw()`函数每次绘制图形时都会使用不同的布局？好吧，你可以指定你想要用于图形的布局。这里是一个例子：

```py
pos = nx.circular_layout(G)
nx.draw(G, pos, with_labels = True)
```

所有节点现在将以圆形方式排列：

![](img/941e4b87b5680c46e65200805c688901.png)

另外，你也可以通过`nx.draw_circular()`函数（而不是`nx.draw()`函数）使用圆形布局绘制图形：

```py
nx.draw_circular(G, with_labels = True)
```

你可以尝试其他布局：

+   `nx.draw_kamada_kawai(G, with_labels = True)`

+   `nx.draw_planar(G, with_labels = True)`

+   `nx.draw_random(G, with_labels = True)`

+   `nx.draw_spectral(G, with_labels = True)`

+   `nx.draw_spring(G, with_labels = True)`

+   `nx.draw_shell(G, with_labels = True)`

## 标记边

在给节点标记的情况下，您可能还想标记边。您可以通过 `nx.draw_networkx_edge_labels()` 函数来实现。

```py
pos = nx.circular_layout(G)
nx.draw(G, pos, with_labels = True)
nx.draw_networkx_edge_labels(
    G, 
    pos,
    edge_labels={
        ("Singapore","Tokyo"): '2 flights daily', 
        ("San Francisco","Singapore"): '5 flights daily',
    },
    font_color='red'
)
```

上述代码片段标记了三个节点的两条边：

![](img/a4d0b7797bb8adc614d03a3e9bf19cb4.png)

## 有向图

到目前为止，我们的图是无向的。在某些情况下，绘制有向图可能会很有用。例如，在我们的示例中，边可能表示两个城市之间的航班。使用有向图可以让我们直观地检查哪些航班从一个城市到另一个城市。以下代码片段显示了我们示例现在被绘制为有向图：

```py
import networkx as nx

#---directed graph---
G = nx.DiGraph(directed=True)

# add nodes
G.add_node("Singapore")
G.add_node("San Francisco")
G.add_node("Tokyo")
G.add_nodes_from(["Riga", "Copenhagen"])

# add edges
G.add_edge("Singapore","San Francisco")
G.add_edge("San Francisco","Tokyo")
G.add_edges_from(
    [
        ("Riga","Copenhagen"),
        ("Copenhagen","Singapore"),
        ("Singapore","Tokyo"),
        ("Riga","San Francisco"),
        ("San Francisco","Singapore"),
    ]
)
# set layout
pos = nx.circular_layout(G)

# draw graph
nx.draw(G, pos, with_labels = True)

# draw edge labels
nx.draw_networkx_edge_labels(
    G, pos,
    edge_labels={
        ("Singapore","Tokyo"): '2 flights daily', 
        ("San Francisco","Singapore"): '5 flights daily',
    },
    font_color='red'
)
```

现在您可以观察到从新加坡到旧金山的航班，反之亦然。另一方面，从里加到旧金山有航班，但反向则没有。

![](img/2aef039c9ba80b04d54e899d4088e0d6.png)

## 定制节点

默认情况下，节点是蓝色的，大小相当小。您可以通过将字典传递给 `draw()` 函数来定制节点以及边的颜色：

```py
options = {
    'node_color': 'yellow',     # color of node
    'node_size': 3500,          # size of node
    'width': 1,                 # line width of edges
    'arrowstyle': '-|>',        # array style for directed graph
    'arrowsize': 18,            # size of arrow
    'edge_color':'blue',        # edge color
}

nx.draw(G, pos, with_labels = True, arrows=True, **options)
```

节点现在是黄色的（并且更大），边是蓝色的：

![](img/75b33eba5bf2795e4dc774925b5c5045.png)

## 给节点添加轮廓

如果您想为节点添加轮廓，需要使用 `matplotlib` 手动完成。以下代码片段将图形大小设置为 10 英寸 x 10 英寸（*宽度* x *高度*），然后使用 `set_edgecolor()` 函数为每个节点绘制黑色轮廓：

```py
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 10))

pos = nx.circular_layout(G)

options = {
    'node_color': 'yellow',
    'node_size': 8500,
    'width': 1,
    'arrowstyle': '-|>',
    'arrowsize': 18,
}

nx.draw(G, pos, with_labels = True, arrows=True, **options)

ax = plt.gca()
ax.collections[0].set_edgecolor("#000000")
```

每个节点现在都有一个黑色轮廓：

![](img/10d7bb9d8c98a009b4cfd8d48de30d95.png)

如果您没有设置图形大小，您的图表可能看起来像这样：

![](img/4eadd1d8e05870baf6435f421e980f74.png)

## 设置节点颜色

要为每个节点指定不同的颜色，您可以指定一个颜色调色板，例如来自 `bokeh`，并将颜色设置在传递给 `draw()` 函数的字典中的 `node_color` 键中：

```py
from networkx import *
import matplotlib.pyplot as plt
from bokeh.palettes import Spectral

plt.figure(figsize=(8, 8))

pos = nx.circular_layout(G)

options = {
    'node_color': Spectral[5],  # first 5 colors from the Spectral palette
    'node_size': 8500,
    'width': 1,
    'arrowstyle': '-|>',
    'arrowsize': 18,
}

nx.draw(G, pos=pos, with_labels = True, arrows=True, **options)

ax = plt.gca()
ax.collections[0].set_edgecolor("#000000")
```

现在图表将为每个节点使用不同的颜色：

![](img/689d80a8b84713a3af42043fedf1657d.png)

如果您想指定自己的颜色，可以在字典中手动设置它们：

```py
options = {
    'node_color': ['yellow','magenta','lightblue','lightgreen','pink'],
    'node_size': 8500,
    'width': 1,
    'arrowstyle': '-|>',
    'arrowsize': 18,
}
```

## 相关文章

[](/building-interactive-network-graphs-using-pyvis-5b8e6e25cf64?source=post_page-----bc62f0d93b3f--------------------------------) ## 使用 pyvis 构建互动网络图

### 了解如何让您的网络图栩栩如生

towardsdatascience.com [](/visualizing-geospatial-network-graphs-using-basemap-and-mplleaflet-76a7f3d0c923?source=post_page-----bc62f0d93b3f--------------------------------) ## 使用 Basemap 和 mplleaflet 可视化地理空间网络图

### 了解如何在地图上绘制网络图

towardsdatascience.com

**如果你喜欢阅读我的文章，并且这些文章对你的职业/学习有帮助，请考虑注册成为 Medium 会员。每月$5，注册后可以无限访问 Medium 上所有文章（包括我的）。如果你通过以下链接注册，我将获得一小笔佣金（不会增加你的额外费用）。你的支持意味着我能花更多时间撰写类似的文章。**

[](https://weimenglee.medium.com/membership?source=post_page-----bc62f0d93b3f--------------------------------) [## 使用我的推荐链接加入 Medium - Wei-Meng Lee

### 阅读 Wei-Meng Lee 的每一个故事（以及 Medium 上其他成千上万位作者的故事）。你的会员费将直接支持…

weimenglee.medium.com](https://weimenglee.medium.com/membership?source=post_page-----bc62f0d93b3f--------------------------------)

# 摘要

在这篇文章中，我探讨了网络图的基础知识以及它如何帮助你可视化数据集中不同实体之间的关系。本文重点介绍了如何使用 NetworkX 包来绘制图形。在我的下一篇文章中，我将利用航班延误数据集来可视化不同机场之间的联系。敬请关注！
