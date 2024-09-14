# 使用 pyvis 构建互动网络图

> 原文：[`towardsdatascience.com/building-interactive-network-graphs-using-pyvis-5b8e6e25cf64`](https://towardsdatascience.com/building-interactive-network-graphs-using-pyvis-5b8e6e25cf64)

## 学习如何让你的网络图栩栩如生

[](https://weimenglee.medium.com/?source=post_page-----5b8e6e25cf64--------------------------------)![魏梦李](https://weimenglee.medium.com/?source=post_page-----5b8e6e25cf64--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5b8e6e25cf64--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b8e6e25cf64--------------------------------) [魏梦李](https://weimenglee.medium.com/?source=post_page-----5b8e6e25cf64--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b8e6e25cf64--------------------------------) ·阅读时间 7 分钟·2023 年 3 月 6 日

--

![](img/43f40d53b2431b9caed15c4d55a2ad24.png)

图片由 [DeepMind](https://unsplash.com/@deepmind?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我之前关于创建网络图的文章中，我展示了如何使用 NetworkX 包构建一个图。NetworkX 的主要问题是生成的图是静态的。一旦图被绘制出来，用户就无法与之互动（例如重新排列节点等）。如果用户可以与图进行互动，网络图会更直观（也更有趣！）。这也是本文的主要关注点。

[](/plotting-network-graphs-using-python-bc62f0d93b3f?source=post_page-----5b8e6e25cf64--------------------------------) ## 使用 Python 绘制网络图

### 学习如何使用 NetworkX 包可视化复杂网络

towardsdatascience.com

在这篇文章中，我将展示如何使用 `pyvis` 包创建一个互动网络图。

> `pyvis` 包是流行的 **visJS** JavaScript 库的一个封装，它使你可以轻松地在 Python 中生成可视化网络图。

# 安装 pyvis

要安装 `pyvis` 包，请使用 `pip` 命令：

```py
!pip install pyvis
```

# 创建网络

首先，使用 `pyvis` 中的 `Network` 类创建一个新图：

```py
from pyvis.network import Network

net = Network(
    notebook=True,
)
```

要在 Jupyter Notebook 中显示图形，将 `notebook` 参数设置为 `True`。上述代码片段创建了一个*无向*图。

## 添加节点

现在你可以向图中添加节点了：

```py
net.add_node("Singapore")
net.add_node("San Francisco")
net.add_node("Tokyo")
net.add_nodes(["Riga", "Copenhagen"],
              color=['lightgreen', 'yellow'])
```

`add_node()` 函数添加一个节点，而 `add_nodes()` 函数则可以向图中添加多个节点。你还可以为这两个函数设置可选的 `color` 参数，以设置节点的颜色。

要显示图形，请调用`show()`函数并为输出指定名称：

```py
net.show('mygraph.html')
```

节点现在应该显示出来：

![](img/054504a7fea06b0bbeaace242c8724ec.png)

所有图片由作者提供

## 添加边

在图中添加了节点后，现在可以添加边来连接这些节点：

```py
net.add_edge("Singapore","San Francisco") 
net.add_edge("San Francisco","Tokyo")
net.add_edges(
    [
        ("Riga","Copenhagen"),
        ("Copenhagen","Singapore"),
        ("Singapore","Tokyo"),
        ("Riga","San Francisco"),
        ("San Francisco","Singapore"),
    ]
)

net.show('mygraph.html')
```

`add_edge()`函数添加一个连接两个节点的边，而`add_edges()`函数接受一个包含多个节点的元组列表。

图现在应该显示连接各个节点的边。试着拖动每个节点，看看它们在释放后是如何被拉回的：

所有视频由作者提供

## 有向图

如果你想要一个有向图，你应该在`Network`类中设置`directed`参数：

```py
net = Network(
    notebook=True,
    directed=True
)
```

如果你修改了之前的代码并重新运行所有代码片段，你现在应该能看到一个有向图：

![](img/cf5d33bdd7b98f191c2c309688e95cb0.png)

## 修改图的物理效果

如果你点击并拖动图中的节点，你会注意到节点会四处弹跳。当你放开鼠标时，节点会弹回到原来的位置。所有这些行为非常像由弹簧（边）束缚的真实球体（节点）。你可以使用`repulsion()`函数自定义图形背后的物理效果（它们如何弹回、弹簧的阻尼等）。以下语句显示了`repulsion()`函数中所有参数的默认值：

```py
net.repulsion(
    node_distance=100,
    central_gravity=0.2,
    spring_length=200,
    spring_strength=0.05,
    damping=0.09,
)
```

这里是各种参数的用途：

+   `node_distance` — 这是排斥力的影响范围。

+   `central_gravity` — 使整个网络被吸引到中心的引力。

+   `spring_length` — 边的静态长度。

+   `spring_strength` — 边的弹簧强度。

+   `damping` — 一个从 0 到 1 的值，表示前一次物理模拟迭代中的速度有多少会延续到下一次迭代。

> 来源: [`pyvis.readthedocs.io/en/latest/documentation.html?highlight=repulsion#pyvis.network.Network.repulsion`](https://pyvis.readthedocs.io/en/latest/documentation.html?highlight=repulsion#pyvis.network.Network.repulsion)

理解各种参数的最佳方法是尝试一下。以下示例设置了`spring_length`和`damping`参数：

```py
net.repulsion(
    spring_length=400,
    damping=0.01,
)
net.show('mygraph.html')
```

这是图的样子：

![](img/98ad5ba6dd1c474699c94c05a853f28a.png)

以下视频展示了拖动和释放节点时图的行为：

你还可以使用`show_buttons()`函数显示 UI 以动态改变图的物理效果：

```py
net.show_buttons(filter_='physics')
net.show('mygraph.html')
```

![](img/97aa01db27dea1b0778b64a5093b3932.png)

`filter_`参数可以选择以下选项之一：

`show_buttons(filter_=['nodes', 'edges', 'physics'])`

如果你想显示所有过滤器，将其设置为`True`：

```py
net.show_buttons(filter_= True)
```

> 我将留给你尝试这些过滤器的效果以及它们的工作方式。可视化航班延误数据集

现在你已经熟悉了使用`pyvis`包的基础知识，我们将使用它来可视化**2015 年航班延误**数据集中的各种机场之间的航班。

> **2015 年航班延误数据集** (*airports.csv*)。*来源*: [`www.kaggle.com/datasets/usdot/flight-delays`](https://www.kaggle.com/datasets/usdot/flight-delays)。*许可* — [CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)

首先，将**flights.csv**文件加载到 Pandas DataFrame 中。由于这个 CSV 文件很大，我将只加载执行工作所需的三列：

```py
import pandas as pd
df = pd.read_csv('flights.csv', 
                 usecols = ["ORIGIN_AIRPORT", "DESTINATION_AIRPORT","YEAR"])
```

一旦数据框加载完成，我将继续统计从一个机场到另一个机场的航班数量：

```py
df_between_airports = df.groupby(by=["ORIGIN_AIRPORT", "DESTINATION_AIRPORT"]).count()
df_between_airports = df_between_airports['YEAR'].rename('COUNT').reset_index() 
df_between_airports = df_between_airports.query('ORIGIN_AIRPORT.str.len() <= 3 & DESTINATION_AIRPORT.str.len() <= 3')
df_between_airports = df_between_airports.sort_values(by="COUNT", ascending=False)
df_between_airports
```

结果输出如图所示：

![](img/5dc31a4c158e8e1397830805dd6d20ef.png)

由于航班组合超过 4500 种，我们只选择前 130 种组合：

```py
top = 130
df_between_airports = df_between_airports.head(top)
df_between_airports['COUNT'] = df_between_airports['COUNT'] / 5000
df_between_airports
```

注意，我将**COUNT**列中的值除以 5000，因为稍后我将使用**COUNT**列中的值作为连接两个机场的边的线宽。因此，值需要缩小到一个较小的范围。前 130 种组合现在如图所示：

![](img/21f3816a60e8338561c22dafb6af59c9.png)

接下来，我将汇总每个机场的起始航班（记住之前的计数已经标准化）：

```py
node_sizes = df_between_airports.groupby('ORIGIN_AIRPORT').COUNT.agg(sum)
node_sizes
```

每个机场起始航班的数量将作为节点的大小：

> 从一个机场起飞的航班越多，节点越大。

```py
ORIGIN_AIRPORT
ANC     1.2766
ATL    20.2544
BOS     6.3382
BWI     1.1674
CLT     1.2614
DAL     1.2524
DCA     4.0138
DEN    11.5638
DFW     5.5244
EWR     2.0252
FLL     2.5436
HNL     5.1544
HOU     1.2592
JAX     1.0192
JFK     6.1684
KOA     1.2694
LAS     6.8754
LAX    21.0822
LGA     7.3132
LIH     1.1710
MCO     2.7096
MIA     2.2936
MSP     2.3608
MSY     1.1186
OAK     1.2562
OGG     1.6626
ORD    12.6836
PHL     2.3876
PHX     7.2886
SAN     2.4130
SEA     7.3736
SFO    12.2998
SJC     1.2678
SLC     3.4424
SMF     1.1148
TPA     1.4166
Name: COUNT, dtype: float64
```

## 绘制图表

你现在可以绘制图表：

```py
from pyvis.network import Network

net = Network(
    notebook = True,
    directed = True,            # directed graph
    bgcolor = "black",          # background color of graph 
    font_color = "yellow",      # use yellow for node labels
    cdn_resources = 'in_line',  # make sure Jupyter notebook can display correctly
    height = "1000px",          # height of chart
    width = "100%",             # fill the entire width    
    )

# get all the nodes from the two columns
nodes = list(set([*df_between_airports['ORIGIN_AIRPORT'], 
                  *df_between_airports['DESTINATION_AIRPORT']
                 ]))

# extract the size of each airport
values = [node_sizes[node] for node in nodes]

# extract the edges between airports
edges = df_between_airports.values.tolist()

# use this if you don't need to set the width of the edges
# edges = df_between_airports.iloc[:,:2].values.tolist()

# add the nodes, the value is to set the size of the nodes
net.add_nodes(nodes, value = values)

# add the edges
net.add_edges(edges)

net.show('flights.html')
```

图表如下所示：

![](img/f4be8cae91d8f5d04a89dffa1c8b7d9c.png)

让我们稍微放大一下：

![](img/421fab7738489e738fd5cd003b5a260a.png)

你可以看到**ATL**和**LAX**的起始航班最多（它们是两个最大节点）。你可以通过将这两个节点的颜色更改为红色来突出显示它们。为此，你可以使用`nodes`属性遍历所有节点，并检查`value`键的值。如果值大于 20，则使用`color`键将节点颜色设置为红色：

```py
...
...

# add the edges
net.add_edges(edges)

# color the nodes red if their count is more than 20
for n in net.nodes:
    if n['value'] > 20:
        n['color'] = 'red'

net.show('flights.html')
```

**ATL**和**LAX**节点现在显示为红色：

![](img/d4618d9fa7e20fe2e8d5d7aeea687c44.png)

**如果你喜欢阅读我的文章，并且它对你的职业/学习有所帮助，请考虑注册成为 Medium 会员。每月 5 美元，能够无限访问 Medium 上的所有文章（包括我的）。如果你使用以下链接注册，我将赚取一小部分佣金（对你没有额外费用）。你的支持意味着我可以投入更多时间撰写类似的文章。**

[](https://weimenglee.medium.com/membership?source=post_page-----5b8e6e25cf64--------------------------------) [## 使用我的推荐链接加入 Medium — Wei-Meng Lee

### 阅读 Wei-Meng Lee 的每个故事（以及 Medium 上其他成千上万的作者）。你的会员费直接支持…

[weimenglee.medium.com](https://weimenglee.medium.com/membership?source=post_page-----5b8e6e25cf64--------------------------------)

# 摘要

在这篇文章中，你学习了如何使用`pyvis`包创建交互式网络图。`pyvis`包最有趣的地方在于它能够让你的网络图生动起来。交互式网络图非常适合用于社交网络、公司结构或其他你希望可视化实体之间关系的网络。享受使用`pyvis`的乐趣，并告诉我你使用它处理的数据类型！
