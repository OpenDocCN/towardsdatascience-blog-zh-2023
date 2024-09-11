# 回到未来：使用马尔可夫转移矩阵分析时间序列数据

> 原文：[https://towardsdatascience.com/time-series-data-markov-transition-matrices-7060771e362b?source=collection_archive---------1-----------------------#2023-02-01](https://towardsdatascience.com/time-series-data-markov-transition-matrices-7060771e362b?source=collection_archive---------1-----------------------#2023-02-01)

## 概念概述及实际应用

[](https://kcpub21.medium.com/?source=post_page-----7060771e362b--------------------------------)[![Chinmay Kakatkar](../Images/873531f24cd4ee8a927669a211d61ae8.png)](https://kcpub21.medium.com/?source=post_page-----7060771e362b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7060771e362b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7060771e362b--------------------------------) [Chinmay Kakatkar](https://kcpub21.medium.com/?source=post_page-----7060771e362b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F87ab8c40e0ed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-data-markov-transition-matrices-7060771e362b&user=Chinmay+Kakatkar&userId=87ab8c40e0ed&source=post_page-87ab8c40e0ed----7060771e362b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7060771e362b--------------------------------) ·11分钟阅读·2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7060771e362b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-data-markov-transition-matrices-7060771e362b&user=Chinmay+Kakatkar&userId=87ab8c40e0ed&source=-----7060771e362b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7060771e362b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-data-markov-transition-matrices-7060771e362b&source=-----7060771e362b---------------------bookmark_footer-----------)![](../Images/6620ab2c70cc6c9e711ca7cedefb60d8.png)

图片由 [Oto Godfrey](http://OtoGodfrey.com) 和 [Justin Morton](http://JMortonPhoto.com) 提供，来自 [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:TeamTimeCar.com-BTTF_DeLorean_Time_Machine-OtoGodfrey.com-JMortonPhoto.com-07.jpg)：根据 [CC-BY-SA-4.0](https://commons.wikimedia.org/wiki/Category:CC-BY-SA-4.0) 许可自由使用

在本文中，我们将探讨如何通过使用马尔科夫转移矩阵重新构造时间序列数据，来获得有趣的描述性洞察以及优雅的预测、回溯和收敛分析方法。就像科幻经典《回到未来》中Doc改装的DeLorean时间机器一样，前进和后退时间。

**注意：** 以下章节中的所有方程式和图示均由本文作者创建。

# 基本构建块

设 *E* 定义组成时间序列数据的 *k* 个独特事件。例如，一个时间序列可能由以下三个基本且独特的事件组成，这些事件代表在离散时间步上绘制数据时观察到的路径轨迹类型：*下跌*、*平稳* 和 *上涨*。设 *S* 定义一个长度为 *n*（表示离散时间步）的序列，包含在 *E* 中定义的事件，代表部分或全部数据。例如，序列 [*上涨*、*下跌*、*上涨*、*平稳*、*上涨*] 代表五个时间步的数据。

现在可以定义一个维度为*k*²的马尔科夫转移矩阵 *M*，其中每个元素 *M(i, j)* 描述了从时间步 *t* 的事件 *E(i)* 转移到时间步 *t+1* 的事件 *E(j)* 的概率。换句话说，*M(i, j)* 表示在连续时间步之间转移的条件概率。从图论的角度看，事件 *E(i)* 和 *E(j)* 可以被视作通过有向边 *E(i)* → *E(j)* 连接的节点，如果 *E(i)* 在时间序列数据中被 *E(j)* 跟随；那么马尔科夫转移矩阵 *M* 本质上表示了图中节点所描绘事件的邻接矩阵（或共现矩阵）的归一化版本。

接下来，让我们看看这些基本构建块可以做些什么。

# 转移矩阵的应用：一个简单的例子

假设我们有以下涵盖 11 个连续时间步的原始时间序列数据：[1, 2, -2, -1, 0, 0, 2, 2, 1, 2, 3]。使用上述描述的路径轨迹简化视图，我们可以将数据转换为描述相邻时间步之间转移的以下 10 个事件序列：[*上涨*、*下跌*、*上涨*、*上涨*、*平稳*、*上涨*、*平稳*、*下跌*、*上涨*、*上涨*]。

我们现在可以构造以下邻接矩阵，以捕捉事件序列中共现的模式：

![](../Images/bfd60f317863273123ab593a7bef8816.png)

元素 *A(i, j)* 表示在我们的事件序列中，在某个时间步 *t* 上发生事件 *i* 后，接着发生事件 *j* 在时间步 *t+1* 的次数；*i* 和 *j* 分别是行和列索引。请注意，行表示事件的顺序为 *up*、*flat* 和 *down*，从上到下，而列表示从左到右的顺序相同。例如，*A* 的左上角元素表示在给定的事件序列中，一个 *up* 事件后面跟随另一个 *up* 事件的次数为两次。*A* 的中心右侧元素表示在事件序列中，一个 *flat* 事件后面跟随一个 *down* 事件的次数为一次。依此类推。

我们可以通过行或列标准化矩阵 *A* 来得到转移矩阵。如果我们使用基于行的标准化，则元素 *M(i, j)* 将描述在时间步 *t* 给定事件 *E(i)* 的情况下，在时间步 *t+1* 看到事件 *E(j)* 的概率。因此，每一行中的概率应加起来等于 1。在我们的示例中，行标准化矩阵如下所示：

![](../Images/d169d384275fcfb0feda908ffe8ba511.png)

类似地，如果我们使用基于列的标准化，则元素 *M(i, j)* 将描述在时间步 *t* 给定事件 *E(j)* 的情况下，在时间步 *t-1* 发生事件 *E(i)* 的概率。现在每一列中的概率应加起来等于 1。在我们的示例中，列标准化矩阵如下所示：

![](../Images/08eac104561338f9b91b673ba4a5881b.png)

请注意，行标准化的条件概率（名义上是向前看）可能与列标准化的条件概率（向后看）不同。

## Python 代码示例

要尝试这些概念，这里有一些基本的 Python 代码，它捕捉了上述示例中的内容。

首先确保你已经安装了 Pandas 包：

```py
pip install pandas==0.25.2
```

然后运行以下代码：

```py
import pandas as pd

# Define helper functions
def get_transition_tuples(ls):
    ''' Converts a time series into a list of transition tuples
    '''
    return [(ls[i-1], ls[i]) for i in range(1, len(ls))]

def get_transition_event(tup):
    ''' Converts a tuple into a discrete transition event
    '''
    transition_event = 'flat'
    if tup[0] < tup[1]:
        transition_event = 'up'
    if tup[0] > tup[1]:
        transition_event = 'down'
    return transition_event

# Generate raw time series data
ls_raw_time_series = [1, 2, -2, -1, 0, 0, 2, 2, 1, 2, 3]

# Derive single-step state transition tuples
ls_transitions = get_transition_tuples(ls_raw_time_series)

# Convert raw time series data into discrete events
ls_events = [get_transition_event(tup) for tup in ls_transitions]
ls_event_transitions = get_transition_tuples(ls_events)

# Create an index (list) of unique event types
ls_index = ['up', 'flat', 'down']

# Initialize Markov transition matrix with zeros
df = pd.DataFrame(0, index=ls_index, columns=ls_index)

# Derive transition matrix (or co-occurrence matrix)
for i, j in ls_event_transitions:
    df[j][i] += 1  # Update j-th column and i-th row

''' Derive row-normalized transition matrix:
- Elements are normalized by row sum (fill NAs/NaNs with 0s)
- df.sum(axis=1) sums up each row, df.div(..., axis=0) then divides each column element
'''
df_rnorm = df.div(df.sum(axis=1), axis=0).fillna(0.00)

''' Derive column-normalized transition matrix:
- Elements are normalized by column sum (fill NAs/NaNs with 0s)
- df.sum(axis=0) sums up each col, df.div(..., axis=1) then divides each row element
'''
df_cnorm = df.div(df.sum(axis=0), axis=1).fillna(0.00)
```

这应该生成以下转移矩阵：

```py
>>> df  # Transition matrix with raw event co-occurrences up    flat  down
up    2     2     1
flat  1     0     1
down  2     0     0 >>> df_rnorm  # Row-normalized transition matrix up    flat  down
up    0.4   0.4   0.2
flat  0.5   0.0   0.5
down  1.0   0.0   0.0 >>> df_cnorm  # Column-normalized transition matrix up    flat  down
up    0.4   1.0   0.5
flat  0.2   0.0   0.5
down  0.4   0.0   0.0
```

一种很好的可视化转移矩阵的方法是将它们描绘为有向加权图，使用像 [Graphviz](https://graphviz.org/) 或 [NetworkX](https://networkx.org/) 这样的绘图包。

我们将在这里使用 Graphviz，因此你需要安装该包以便跟随操作：

```py
pip install graphviz==0.13.2
```

值得浏览简短而精炼的 [官方安装指南](https://graphviz.readthedocs.io/en/stable/manual.html#installation)，以确保你正确安装了该包，特别是对于可能需要执行一些额外安装步骤的 Windows 用户。

一旦 Graphviz 设置好了，创建一些用于绘图的辅助函数：

```py
from graphviz import Digraph

# Define functions to visualize transition matrices as graphs

def get_df_edgelist(df, ls_index):
    ''' Derive an edge list with weight values
    '''
    edgelist = []
    for i in ls_index:
        for j in ls_index:
            edgelist.append([i, j, df[j][i]])
    return pd.DataFrame(edgelist, columns=['src', 'dst', 'weight'])

def edgelist_to_digraph(df_edgelist):
    ''' Convert an edge list into a weighted directed graph
    '''
    g = Digraph(format='jpeg')
    g.attr(rankdir='LR', size='30')
    g.attr('node', shape='circle')
    nodelist = []
    for _, row in df_edgelist.iterrows():
        node1, node2, weight = [str(item) for item in row]
        if node1 not in nodelist:
            g.node(node1, **{'width': '1', 'height': '1'})
            nodelist.append(node1)
        if node2 not in nodelist:
            g.node(node2, **{'width': '1', 'height': '1'})
            nodelist.append(node2)
        g.edge(node1, node2, label=weight)
    return g

def render_graph(fname, df, ls_index):
    ''' Render a visual graph and saves it to disk
    '''
    df_edgelist = get_df_edgelist(df, ls_index)
    g = edgelist_to_digraph(df_edgelist)
    g.render(fname, view=True)
```

现在你可以生成每个转移矩阵。默认情况下，输出的图形将存储在你的工作目录中。

```py
# Generate graph of transition matrix (raw co-occurrences)
render_graph('adjmat', df, ls_index)

# Generate graph of row-normalized transition matrix
render_graph('transmat_rnorm', df_rnorm, ls_index)

# Generate graph of column-normalized transition matrix
render_graph('transmat_cnorm', df_cnorm, ls_index)
```

原始共现：

![](../Images/f25db0921cb6c38575e637365c7edc17.png)

行标准化的转移概率：

![](../Images/c5973da7968dbced303123fb0d1b6395.png)

列标准化的转移概率：

![](../Images/ca757e426361a367cbfc9fae62501773.png)

# 实际应用

## 描述性见解

我们可以用转移矩阵做的第一件最明显的事情是通过检查矩阵及其可视化图形表示来获得描述性见解。例如，利用我们前一节的示例输出，我们可以得到如下高层次的见解：

+   在 9 种可能的事件转移中，有 3 种在我们的样本中从未发生过（*平稳* → *平稳*、*下降* → *下降* 和 *下降* → *平稳*）。连续的平稳事件的低概率可能表明时间序列数据所跟踪的系统中的波动性。

+   *上升* 事件是唯一一个有非零概率（0.4）连续发生的事件类型。事实上，这个转移概率在我们的数据中是最高的之一，可能指向数据背后的系统中的强化效应。

+   在我们的案例中，基于行和列的归一化产生了不同的矩阵，尽管有些重叠。这告诉我们我们的时间序列在时间上本质上是不对称的，即我们观察到的模式在回顾或前瞻时会有所不同。

## 预测与回溯

通过将转移矩阵的副本连接起来，我们可以生成未来和过去的事件发生概率；这可以分别称为 *预测* 和 *回溯*。这里的一个核心假设是“历史无关紧要”；无论我们选择哪个时间步 *t* 作为参考点，我们都假设转移矩阵为 *t+1*（如果是行归一化）和 *t-1*（如果是列归一化）提供相关的概率。结果是，我们可以使用转移矩阵从任何任意时间步进行预测和回溯。特别地，我们可以使用行归一化的转移矩阵进行预测，使用列归一化的转移矩阵进行回溯。

以上述示例中计算的矩阵为例，假设我们在时间步 *t* = 25 观察到一个 *上升* 事件，我们希望预测在时间步 *t* = 27 时最可能发生的事件是什么。通过检查行归一化转移矩阵的顶行，我们可以直接看到，在紧接着的时间步 *t* = 26，观察到 *上升*、*平稳* 和 *下降* 事件的概率分别为 0.4、0.4 和 0.2。为了推导时间步 *t* = 27（即距离我们参考点两个时间步）的类似事件概率，我们需要将转移矩阵自身相乘，如下所示：

![](../Images/efbfdf13aee5b7747fb1981d5e0331e5.png)

注意观察事件概率相对于我们的参考时间步长的变化。例如，假设在*t* = 25时出现一个*上升*事件，那么在*t* = 26时观察到另一个*上升*事件的概率为0.4（向未来进一步一步），并且在*t* = 27时增加到0.56（向未来两步）。与此同时，在*t* = 26时观察到*平稳*事件的概率也为0.4，但在*t* = 27时减少到0.16。关键是，矩阵乘法支持预测和回溯。通常来说，向前或向后推测事件发生的概率*n*步，我们可以分别计算该转移矩阵的n次幂的行规范化或列规范化。

转移矩阵也可以用于预测原始的基础时间序列数据。假设*上升*或*下降*事件对时间序列数据造成了一单位的变化。现在假设时间序列从1到2（一个*上升*事件）在*t* = 25时发生，并且我们希望预测*t* = 26和*t* = 27时时间序列的进展。在*上升*事件之后，*上升*和*平稳*事件在*t* = 26时都有最高的发生概率（0.4）。因此，我们可以预测，在*t* = 26时，时间序列很可能是[1, 2, 3]或[1, 2, 2]，两者都有可能在*t* = 27时产生两种更可能的情况：[1, 2, 3]可能变成[1, 2, 3, 4]或[1, 2, 3, 3]（概率均为0.4），而[1, 2, 2]可能变成[1, 2, 2, 3]或[1, 2, 2, 1]（概率均为0.5）。一般来说，我们期望所用于生成转移矩阵的数据集越大越丰富，能够捕捉更多事件链，因此每步的预测精度越高。

转移矩阵的乘法链导致了越来越复杂但完全可分解的原始事件转移概率的组合。这种可分解性可以帮助我们更深入地了解构成时间序列数据（或随机过程）的事件相互之间的关系。

## 收敛分析

连接转移矩阵的概念自然地引出了一个有趣的问题：*转移矩阵M的概率会收敛吗*？具体地说，是否存在一个稳定的转移矩阵*M*使得*MM*=M*？如果是，那么*lim(n → ∞)⁡Mⁿ = M*，也就是说我们期望由矩阵乘法链*⁡Mⁿ*表示的马尔可夫过程在某个时间点收敛到一个稳定状态*M*；在这种情况下，该过程是收敛的并因此稳定。假设我们的转移矩阵是行规范化的，元素*M*(i, j)给出了事件*i*之后紧随事件*j*的稳定长期概率。然而，如果找不到稳定矩阵*M*，那么该过程就不是收敛的也不是稳定的。

利用之前部分的运行示例，我们可以简要描述一下马尔可夫过程的收敛是如何通过分析得出的。

首先，让我们假设存在一个稳定的转移矩阵*M*，使得*MM*=*M*，并且*M*是行归一化的。由于我们知道*M*的样子，我们可以把矩阵乘法写成如下形式：

![](../Images/a30457610de605c2f15446363f1cdbbb.png)

然后我们有下面的线性方程组：

![](../Images/04e2d2c9faf7b112f3cda4d545d929c8.png)

如果这个方程组存在解（我们可以通过高斯消元等方法来检查），那么我们也可以得到一个收敛和稳定的转移矩阵。

# 包裹

一旦你掌握了这个技巧，使用马尔可夫转移矩阵来重构时间序列数据可以成为你数据科学工具箱中的一个有用部分。就像你通常用折线图来可视化时间序列数据以更好地把握整体趋势一样，转移矩阵提供了数据的一个互补表述，它高度压缩但在使用场景上非常多样化。当将其可视化为有向图时，转移矩阵已经可以用于获取高层次的描述性见解。当嵌入到更大的工作流程中时，转移矩阵可以成为预测和逆向预测更复杂方法的基础。此外，虽然我们在上述部分运行的简单例子将转移矩阵视为静态实体，但我们可以针对不同的时间间隔推导出不同的矩阵；这在分析显示数据中体现出明显的趋势逆转，反映在数据中具有显著U形或弯头形式模式的时间序列数据中尤为有用。显然，上面讨论的观点有几种可能的延伸，所以继续尝试吧——它们在你下一个数据科学项目中可能派上用场。
