# 如何使用 DiagrammeR 包在 R 中绘制图表

> 原文：[`towardsdatascience.com/how-to-plot-graphs-using-the-diagrammer-package-in-r-3fe4642091cc`](https://towardsdatascience.com/how-to-plot-graphs-using-the-diagrammer-package-in-r-3fe4642091cc)

## DiagrammeR , grViz()

[](https://indhumathychelliah.medium.com/?source=post_page-----3fe4642091cc--------------------------------)![Indhumathy Chelliah](https://indhumathychelliah.medium.com/?source=post_page-----3fe4642091cc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3fe4642091cc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3fe4642091cc--------------------------------) [Indhumathy Chelliah](https://indhumathychelliah.medium.com/?source=post_page-----3fe4642091cc--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3fe4642091cc--------------------------------) ·6 分钟阅读·2023 年 2 月 15 日

--

![](img/80964a39441eb55c198a4dd56ff83581.png)

图片由 Lukas 提供: [`www.pexels.com/photo/person-holding-blue-pen-590014/`](https://www.pexels.com/photo/person-holding-blue-pen-590014/)

DiagrammeR 是一个 R 包，用于使用 graphviz 和 mermaid 创建图表和流程图。

在本文中，我们将探讨如何使用 DiagrammeR 包中的 grViz() 创建图表。

1.  什么是 DiagrammeR？

1.  什么是 grViz()？

1.  如何使用 grViz() 创建节点、边、标签及其替代和连接

1.  如何编织到 HTML

# 什么是 DiagrammeR

DiagrammeR 是一个在 htmlwidgets 中的 R 包，用于生成使用 graphviz 和 mermaid 库的图表。

## 1\. 创建一个 RMarkdown 文档。

在 RStudio -> 转到 File → New File → R Markdown，然后输入标题并选择输出格式为 HTML。

![](img/c3c92c437d98aab11c22d91b85c9e646.png)

R Markdown 文档将会在顶部显示文档标题和其他信息。

![](img/94f2a45a20c4772e297295909a780952.png)

下面会有 r chunks，带有定界符 ```py` ```{r} ```py` and ```` ```py ````

当你渲染 R Markdown 文档时，它会运行每个代码块，并将结果嵌入在代码块下方，或者你可以运行每个代码块，结果将显示在该代码块下。

我们可以将 R Markdown 文档编织成 HTML 输出。让我们看看如何在本文中做到这一点。

## 2\. 安装 DiagrammeR 包

在 RStudio 右侧点击 Packages → Install，输入包名 DiagrammeR 并点击 Install。

![](img/506d97d6347c9ac92b730356f86f1e9a.png)

# 2\. 什么是 grViz？

DiagrammeR 包使用 grViz() 函数生成 Graphviz 图表。我们来看看如何使用 grViz() 函数创建图表。

# 3\. 如何使用 grViz() 创建节点、边、标签及其替代和连接？

在创建图时，我们必须提及布局、节点属性、边属性和连接。

![](img/82cf2c07876f79e5eb90b3bfb716d92f.png)

作者提供的图片

让我们来看一下如何创建上述显示的图。在上面的图中，* * 绿色圆圈被称为节点。A=15，B=10，等等-> 是节点的标签。箭头标记称为边。

## 1. 提及布局

默认布局是 dot。

```py
layout = dot
```

**2. 提及节点属性：**

我们可以提到节点的形状、颜色、样式、宽度等。

```py
node [shape = circle,color=green,style=filled,fixedsize=True,width=0.6]
```

**3. 提及边的属性：**

边的属性包括颜色、箭头头部、箭头尾部、笔宽、方向等。

```py
edge[color=grey,arrowhead=vee]
```

**4. 提及标签和替代项：**

我们必须提及节点的标签和替代项。

```py
A[label = 'A= @@1']
B[label = 'B=@@2']
C[label = 'C=@@3']
D[label = 'D=@@4']
E[label = 'E=@@5']
```

这里 A -> 节点名称

A=@@1 → 这是标签。

```py
[1]:15
[2]:10
[3]:20
[4]:2
[5]:2
```

@@1 -> 是替代。这里 1 是脚注编号。当图被渲染时，值 15 将替代 1。

**5. 提及连接细节**

在我的图中，A 连接到 B 和 C。B 连接到 D，C 连接到 E。

```py
A->B
A->C
B->D
C->E
```

**完整 R 代码：**

```py
```{r , echo=FALSE}

library(DiagrammeR)

grViz("

digraph {

layout = dot

    节点 [shape = circle,color=green,style=filled,fixedsize=True,width=0.6]

    edge[color=grey,arrowhead=vee]

    A[label = 'A= @@1']

    B[label = 'B=@@2']

    C[label = 'C=@@3']

    D[label = 'D=@@4']

    E[label = 'E=@@5']

    A->B

    A->C

    B->D

    C->E

}

[1]:15

[2]:10

[3]:20

[4]:2

[5]:2

")

```py

**Output Graph:**

![](img/82cf2c07876f79e5eb90b3bfb716d92f.png)

Image by Author

Let’s create another graph where nodes A and C are in the same rank and B, and E are in the same rank.

If we mention rank=same {A->C}. A and C nodes will be placed in the same line.

```

rank=same {A->C}

rank=same {B->E}

```py

**Full code**

```

```py{r , echo=FALSE}
library(DiagrammeR)
grViz("
  digraph {
  layout = dot
    node [shape = circle,color=green,style=filled,fixedsize=True]
    edge[color=grey,arrowhead=vee,minlen = 1]
    A[label = 'A=@@1']
    B[label = 'B=@@2']
    C[label = 'C=@@3']
    D[label = 'D=@@4']
    E[label = 'E=@@5']
    A->B
    B->D
    edge [minlen = 2]
    rank=same {A->C}
    rank=same {B->E}

  }
  [1]:15
  [2]:10
  [3]:20
  [4]:2
  [5]:2
   ")
```

## 输出图：

![](img/a5fd989f0f752da1684ecfdd47f6a76f.png)

作者提供的图片

让我们从另一个节点的下箭头创建一个节点。

![](img/962d5d77523140e19853c88b0e14aca5.png)

作者提供的图片

让我们看看如何创建上述提到的图。

首先，我们需要创建一个空节点。

```py
A -> blank1;
blank1 -> B[minlen=10];
  {{ rank = same; blank1 B }}
blank1 -> C 
```

```py
C -> blank2;
blank2 -> D[minlen=1];
  {{ rank = same; blank2 E }}
blank2 -> E [minlen=10]
```

**输出：**

![](img/48875fc8e70010dde0a205495050e31b.png)

作者提供的图片

之后，我们可以将空节点的标签设为“ ”，并将宽度和高度设置为非常小（0.01）

```py
blank1[label = '', width = 0.01, height = 0.01]
blank2[label = '', width = 0.01, height = 0.01]
```

**完整代码**

```py
```{r , echo=FALSE}

library(DiagrammeR)

grViz("

digraph {

layout = dot

    节点 [shape = circle,color=green,style=filled,fontsize=45,fixedsize=True,width=4.0]

    edge[color=grey,arrowhead=vee,penwidth=5,arrowsize=5]

    A[label = 'A= @@1']

    B[label = 'B=@@2']

    C[label = 'C=@@3']

    D[label = 'D=@@4']

    E[label = 'E=@@5']

blank1[label = '', width = 0.01, height = 0.01]

A -> blank1;

blank1 -> B[minlen=10];

{{ rank = same; blank1 B }}

blank1 -> C

```py

```

blank2[label = '', width = 0.01, height = 0.01]

C -> blank2;

blank2 -> D[minlen=1];

{{ rank = same; blank2 E }}

blank2 -> E [minlen=10]

}

[1]:15

[2]:10

[3]:20

[4]:2

[5]:2

")```py
```

**输出：**

![](img/3f9bb201e8790b2d5a3124188300732a.png)

作者提供的图片

下一步是去除箭头标记。可以通过提及边的属性“dir=none”来去除。

```py
A -> blank1[dir=none];
C -> blank2[dir=none];
```

**完整代码：**

```py
```{r , echo=FALSE}

library(DiagrammeR)

grViz("

digraph {

layout = dot

    节点 [shape = circle,color=green,style=filled,fontsize=45,fixedsize=True,width=4.0]

    edge[color=grey,arrowhead=vee,penwidth=5,arrowsize=5]

    A[label = 'A= @@1']

    B[label = 'B=@@2']

    C[label = 'C=@@3']

    D[label = 'D=@@4']

    E[label = 'E=@@5']

blank1[label = '', width = 0.01, height = 0.01]

A -> blank1[dir=none];

blank1 -> B[minlen=10];

{{ rank = same; blank1 B }}

blank1 -> C

```py

```

blank2[label = '', width = 0.01, height = 0.01]

C -> blank2[dir=none];

blank2 -> D[minlen=1];

{{ rank = same; blank2 E }}

blank2 -> E [minlen=10]

}

[1]:15

[2]:10

[3]:20

[4]:2

[5]:2

")```py
```

## 输出：

![](img/f7f401c9037a35f787f62e05f746fd19.png)

图片由作者提供

# 4\. 编织到 HTML

选择选项编织 -> 编织到 HTML

![](img/c4fe1fccaa6fe03de549fdd75ff158fe.png)

整个 R-Markdown 文档将被渲染为 HTML 文档。

如果不希望在 HTML 输出中显示 R 代码，请在 R-chunk 代码中设置 echo =False。

```py
```{r , echo=FALSE}```py
```

这个 HTML 页面可以从我的 GitHub 链接下载。

[`github.com/IndhumathyChelliah/R-Projects`](https://github.com/IndhumathyChelliah/R-Projects)

# 结论：

DiagrammeR 是一个 R 包，允许使用 Graphviz 和 mermaid 样式创建图形。在本文中，我们介绍了如何使用 GraphViz 样式。我们介绍了如何使用 DiagrammeR 包中的 grViz() 函数创建节点、标签、边、连接和图形布局。我们还介绍了节点属性、边属性、标签及其替代。本文仅涵盖了 dot 布局，这是默认布局。

感谢阅读，希望大家喜欢！

# 参考文献：

[](https://rich-iannone.github.io/DiagrammeR/?source=post_page-----3fe4642091cc--------------------------------) [## DiagrammeR

### DiagrammeR 是一个 R 包，允许你使用类似 Markdown 的文本创建流程图、图示和图形。

[rich-iannone.github.io](https://rich-iannone.github.io/DiagrammeR/?source=post_page-----3fe4642091cc--------------------------------) [](https://rich-iannone.github.io/DiagrammeR/graphviz_and_mermaid.html?source=post_page-----3fe4642091cc--------------------------------) [## DiagrammeR — 文档

### Graphviz 支持是 DiagrammeR 包的重要组成部分。Graphviz 包含图形描述语言…

[rich-iannone.github.io](https://rich-iannone.github.io/DiagrammeR/graphviz_and_mermaid.html?source=post_page-----3fe4642091cc--------------------------------)  [## dot

### Graphviz dot 是处理有方向边的默认工具。布局算法将边对齐在同一…

[graphviz.org](https://graphviz.org/docs/layouts/dot/?source=post_page-----3fe4642091cc--------------------------------)
