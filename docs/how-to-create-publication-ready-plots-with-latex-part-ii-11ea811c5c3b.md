# 使用 LaTeX 创建出版级图表：第二部分

> 原文：[https://towardsdatascience.com/how-to-create-publication-ready-plots-with-latex-part-ii-11ea811c5c3b?source=collection_archive---------9-----------------------#2023-04-11](https://towardsdatascience.com/how-to-create-publication-ready-plots-with-latex-part-ii-11ea811c5c3b?source=collection_archive---------9-----------------------#2023-04-11)

## *让我们来看看堆叠图！*

[](https://medium.com/@aspdata?source=post_page-----11ea811c5c3b--------------------------------)[![Aruna Pisharody](../Images/c12797bb32a483da4fe0aff33ff58a63.png)](https://medium.com/@aspdata?source=post_page-----11ea811c5c3b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----11ea811c5c3b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----11ea811c5c3b--------------------------------) [Aruna Pisharody](https://medium.com/@aspdata?source=post_page-----11ea811c5c3b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b205ac0c1dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-publication-ready-plots-with-latex-part-ii-11ea811c5c3b&user=Aruna+Pisharody&userId=5b205ac0c1dc&source=post_page-5b205ac0c1dc----11ea811c5c3b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----11ea811c5c3b--------------------------------) ·8 分钟阅读·2023年4月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F11ea811c5c3b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-publication-ready-plots-with-latex-part-ii-11ea811c5c3b&user=Aruna+Pisharody&userId=5b205ac0c1dc&source=-----11ea811c5c3b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F11ea811c5c3b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-publication-ready-plots-with-latex-part-ii-11ea811c5c3b&source=-----11ea811c5c3b---------------------bookmark_footer-----------)![](../Images/5072c9f7f16ab7a3a23fcc6c4141e73a.png)

图片由 [Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/6EnTPvPPL6I?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

大家好！我又回来了，这次带来了一篇关于用 LaTeX 绘图的文章！如果你错过了我之前的文章，或者想要稍微回顾一下（我知道自从第一篇文章以来已经有一段时间了！），你可以在[这里](/how-to-create-publication-ready-plots-with-latex-4a095eb2f1bd)阅读它。

在本文中，我将介绍一种特定类型的图表：堆叠图（或在LaTeX中称为*groupplots*；在本文中我将这两个术语交替使用）。我发现这些图表在许多场景下非常有用。然而，每种使用情况可能需要不同的定制，我无法在一篇文章中涵盖所有内容。所以我决定只专注于如何在LaTeX中开始使用堆叠图。特别是，我将详细讲述我在第一次使用这种图表时遇到的最令人沮丧的挑战之一：向堆叠图中添加图例！

那么，废话不多说，我们开始吧！

## LaTeX中的堆叠图

让我们使用上一篇文章中的相同的*flights.csv*数据集*（来源：seaborn.pydata.org）*。然而，这次我们将绘制4个连续年份（1949年—1952年）每月乘客数量的变化。使用我们上次学到的知识，我们可以这样绘制场景：

![](../Images/bcae8090cee1fea943140db0d8bec5c3.png)

一个简单的图表（作者提供的图片）

创建此图表的LaTeX代码如下所示。

```py
 \begin{tikzpicture}
\begin{axis}[
    myplotstyle,
    xtick=data,
    table/col sep=comma,
    xticklabels from table={Fig_groupplot_1.csv}{month},
    legend pos=outer north east,
    legend entries={1949,1950,1951,1952},
    xlabel={Month},
    ylabel={No. of passengers},
    x tick label style={rotate=90, /pgf/number format/.cd, set thousands separator={}},
]
\addplot+[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_1.csv};
\addplot+[smooth, color=black,mark=triangle*, mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_2.csv};
\addplot+[smooth, color=black,mark=square*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_3.csv};
\addplot+[smooth, color=black,mark=diamond*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_4.csv};
\end{axis}
\end{tikzpicture}
```

尽管这个图表本身看起来已经很好了，但让我们看看如何将每年的图表分开并将它们堆叠在一起（因为这正是标题中承诺的内容！）。

我们可以使用*groupplot*选项来实现。在这种情况下，我们需要先将`\begin{axis}...\end{axis}`替换为`\begin{groupplot}...\end{groupplot}`。我们还需要为这个groupplot定义一些基本的样式选项；我在这里包括的一些选项有groupplot的大小、x轴和y轴标签及刻度标记的位置，以及groupplot中各个图表之间的垂直距离。接下来，我们需要在每个`\addplot`命令之前添加一个命令`\nextgroupplot`。所以完整代码如下所示：

```py
\begin{tikzpicture}
\begin{groupplot}[
        group style={
        group name=myplot,
        group size=1 by 4,
        xlabels at=edge bottom,
        xticklabels at=edge bottom,
        ylabels at=edge right,
        vertical sep=0pt,
    }, 
    myplotstyle,
    xtick=data,
    table/col sep=comma,
    xticklabels from table={Fig_groupplot_1.csv}{month},
    width=10cm, height=6cm,
    x tick label style={rotate=90, /pgf/number format/.cd},
    xlabel={Month},
    ylabel={No. of passengers},
]
\nextgroupplot
\addplot+[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_1.csv};

\nextgroupplot
\addplot+[smooth, color=black,mark=triangle*, mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_2.csv};

\nextgroupplot
\addplot+[smooth, color=black,mark=square*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_3.csv};

\nextgroupplot
\addplot+[smooth, color=black,mark=diamond*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_4.csv};
\end{groupplot}
\end{tikzpicture}
```

上述代码生成的图表将如下所示：

![](../Images/7dec8d3926d6f29229236ecd05b84d8f.png)

一个没有图例的简单堆叠图（作者提供的图片）

现在让我们继续在这个groupplot中包含图例。对于我们的图表，有三种方法可以做到这一点：

*选项 1：在每个单独的图表中添加图例*

对于这个选项，我们只需在每个` \addplot`之后添加命令` \addlegendentry{*year*}`，就完成了！ *注意：如果你在一个图表中有多个`*\addplot*`命令，只需在每个`*\addplot*`命令之后添加`*\addlegendentry{year}*`命令。*

完整代码如下所示：

```py
\begin{tikzpicture}
\begin{groupplot}[
        group style={
        group name=myplot,
        group size=1 by 4,
        xlabels at=edge bottom,
        xticklabels at=edge bottom,
        ylabels at=edge right,
        vertical sep=0pt,
    }, 
    myplotstyle,
    xtick=data,
    table/col sep=comma,
    xticklabels from table={Fig_groupplot_1.csv}{month},
    width=10cm, height=6cm,
    x tick label style={rotate=90, /pgf/number format/.cd},
    xlabel={Month},
    ylabel={No. of passengers},
]
\nextgroupplot
\addplot+[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_1.csv}; 
\addlegendentry{1949};

\nextgroupplot
\addplot+[smooth, color=black,mark=triangle*, mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_2.csv};
\addlegendentry{1950};

\nextgroupplot
\addplot+[smooth, color=black,mark=square*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_3.csv};
\addlegendentry{1951};

\nextgroupplot
\addplot+[smooth, color=black,mark=diamond*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_4.csv};
\addlegendentry{1952};

\end{groupplot}
\end{tikzpicture}
```

*选项 2：在每个图表中添加一个文本节点以指定相应的年份*

为此，我将所有图表标记更改为相同的。然后，我们为所有单独的图表添加一个节点，指定每个图表的年份。*（注意：我们在上一篇文章中制作的均值图表也做了类似的处理。）* 这个节点可以这样定义（*你可以在我的上一篇文章中详细阅读使用rel axis cs进行节点定位的内容* [*这里*](/how-to-create-publication-ready-plots-with-latex-4a095eb2f1bd)）：

```py
\node [text width=5em] at (rel axis cs: 0.15,0.9){\textbf{Year}}
```

在这里，我还指定了节点的宽度，以方便操作。

我发现这在这种特定情况下是更容易的选项（每个图中只有一列要绘制的数据）。以下是选项2的完整代码示例：

```py
\begin{tikzpicture}
\begin{groupplot}[
        group style={
        group name=myplot,
        group size=1 by 4,
        xlabels at=edge bottom,
        xticklabels at=edge bottom,
        vertical sep=0pt,
    }, 
    myplotstyle,
    xtick=data,
    table/col sep=comma,
    xticklabels from table={Fig_groupplot_1.csv}{month},
    width=10cm, height=6cm,
    x tick label style={rotate=90, /pgf/number format/.cd},
    xlabel={Month},
    ylabel={No. of passengers},
]
\nextgroupplot[]
\node [text width=5em] at (rel axis cs: 0.15,0.9){\textbf{1949}};
\addplot[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_1.csv};

\nextgroupplot[]
\node [text width=5em] at (rel axis cs: 0.15,0.9){\textbf{1950}};
\coordinate (top) at (rel axis cs:0,1);
\addplot[smooth, color=black,mark=*, mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_2.csv};

\nextgroupplot[]
\node [text width=5em] at (rel axis cs: 0.15,0.9){\textbf{1951}};
\coordinate (top) at (rel axis cs:0,1);
\addplot[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_3.csv};

\nextgroupplot[]
\node [text width=5em] at (rel axis cs: 0.15,0.9){\textbf{1952}};
\coordinate (top) at (rel axis cs:0,1);
\addplot[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_4.csv};
\end{groupplot}
\end{tikzpicture}
```

*选项3：将所有图例条目合并到一个图例框中*

在这种情况下，我将使用`\label`命令为所有单独的图设置标签，例如：`\label{plots:plot1}`，`\label{plots:plot2}`，`\label{plots:plot3}`和`\label{plots:plot4}`。接下来，我将定义要放置图例框的位置。这可以通过在`\end{groupplot}`之后但在`\end{tikzpicture}`之前添加以下代码行来完成：

```py
\path (myplot c1r1.north west)--
      coordinate(legendpos)
      (myplot c1r1.north east);
```

在这里，我们所做的就是创建一个路径，跨越`groupplot`中第一个图（或`c1r1`）的两端（`c1r1.north west` — `c1r1.north east`），并在此路径的中点定义我们的图例位置(`legendpos`)。*注意：* `*\path*` *命令绘制一个“不可见的路径”。如果你想在放置节点之前看到你所画的内容，请将* `*\path*` *替换为* `*\path[draw]*` *或* `*\draw*` *命令。*

一旦位置被定义好，我将加入图例。为此，我再次使用`\node`命令来完成（或者，你可以使用`\matrix`，它是`\path node[matrix]`的缩写；任何这些命令都能完成任务）。然而，这次我在我的`\node[]`中使用了一个特殊选项`matrix of nodes`，顾名思义，指定我需要一个节点矩阵。指定此节点的语法如下：

```py
\node[matrix of nodes] at (position) { cell contents };
```

对于这个`groupplot`，我定义了节点如下：

```py
\node[
    matrix of nodes,
    draw,
    inner sep=0.2em,
  ] at ([yshift=1em]legendpos)
  {
    \ref{plots:plot1} & 1949 & [1em]
    \ref{plots:plot2} & 1950 & [1em]
    \ref{plots:plot3} & 1951 & [1em]
    \ref{plots:plot4} & 1952\\};
```

我添加的几个节点选项包括 `draw`（使图例框可见）和 `inner sep`（定义矩阵外边界和内容之间的空间）。我还添加了一个 `yshift` 来将图例放置在图的边界之外。单元格内容都很容易理解。以下是这个选项的完整代码：

```py
\begin{tikzpicture}
\begin{groupplot}[
        group style={
        group name=myplot,
        group size=1 by 4,
        xlabels at=edge bottom,
        xticklabels at=edge bottom,
        ylabels at=edge right,
        vertical sep=0pt,
    }, 
    myplotstyle,
    xtick=data,
    table/col sep=comma,
    xticklabels from table={Fig_groupplot_1.csv}{month},
    width=10cm, height=6cm,
    x tick label style={rotate=90, /pgf/number format/.cd},
    xlabel={Month},
    ylabel={No. of passengers},
]
\nextgroupplot
\addplot+[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_1.csv}; \label{plots:plot1}

\nextgroupplot
\addplot+[smooth, color=black,mark=triangle*, mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_2.csv}; \label{plots:plot2}

\nextgroupplot
\addplot+[smooth, color=black,mark=square*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_3.csv}; \label{plots:plot3}

\nextgroupplot
\addplot+[smooth, color=black,mark=diamond*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_4.csv}; \label{plots:plot4}

\end{groupplot}

\path (myplot c1r1.north west)--
      coordinate(legendpos)
      (myplot c1r1.north east);

\node[
    matrix of nodes,
    draw,
    inner sep=0.2em,
  ]at ([yshift=1em] legendpos)
  {
    \ref{plots:plot1} & 1949 & [1em]
    \ref{plots:plot2} & 1950 & [1em]
    \ref{plots:plot3} & 1951 & [1em]
    \ref{plots:plot4} & 1952\\};
\end{tikzpicture}
```

让我们来看看我们刚刚尝试过的三种图表变体：

![](../Images/1e5cb93e80023f4c4ab978eefcdb0a6d.png)

堆叠图表示出包含图例的三种变体（作者提供的图像）

现在，我想对这个图进行更多的修改，以使其在审美上更令人愉悦。由于每个图的Y轴标签相同，让我们只为它们使用一个统一的Y轴标签。为此，我们可以简单地从`groupplot`样式定义中删除Y轴标签，并在`groupplot`中垂直居中添加一个带标签的节点。我们通过添加以下代码行来完成这个操作：

```py
 \path (myplot c1r1.north west)
          -- (myplot c1r4.south west) 
          node[midway, xshift=-3em, rotate=90] 
          {\textbf{No. of passengers}};
```

这是我们之前使用的`\path`命令的一个变体。在这里，我将从第一个图 (`c1r1`) 的西北角绘制路径，到最后一个图 (`c1r4`) 的西南角，在`groupplot`中放置一个节点。此外，我还将节点移出图的边界，并将其旋转了90度。Y轴标签完成！

这就是完整的代码和图表现在的样子。*注意：为了简洁起见，我只展示了之前提到的一个图表变体（选项 2）的代码/图表。相同的* `*\path*` *命令也适用于其他两个图表选项。*

```py
\begin{tikzpicture}
\begin{groupplot}[
        group style={
        group name=myplot,
        group size=1 by 4,
        xlabels at=edge bottom,
        xticklabels at=edge bottom,
        vertical sep=0pt,
    }, 
    myplotstyle,
    xtick=data,
    table/col sep=comma,
    xticklabels from table={Fig_groupplot_1.csv}{month},
    width=10cm, height=6cm,
    x tick label style={rotate=90, /pgf/number format/.cd},
    xlabel={Month},
]
\nextgroupplot[]
\node [text width=5em] at (rel axis cs: 0.15,0.9){\textbf{1949}};
\addplot[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_1.csv};

\nextgroupplot[]
\node [text width=5em] at (rel axis cs: 0.15,0.9){\textbf{1950}};
\addplot[smooth, color=black,mark=*, mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_2.csv};

\nextgroupplot[]
\node [text width=5em] at (rel axis cs: 0.15,0.9){\textbf{1951}};
\addplot[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_3.csv};

\nextgroupplot[]
\node [text width=5em] at (rel axis cs: 0.15,0.9){\textbf{1952}};
\addplot[smooth, color=black,mark=*,mark options={fill=white,scale=1.5}] table[x expr=\coordindex,y=passengers, col sep=comma] {Fig_groupplot_4.csv};
\end{groupplot}
    \path (myplot c1r1.north west)
          -- (myplot c1r4.south west) node[midway, xshift=-3em, rotate=90] {\textbf{No. of passengers}};
\end{tikzpicture}
```

![](../Images/a842c7dbbab37d41a37fcfb7edc2dbe5.png)

样式化图例（选项 2）和轴标签后的最终叠加图（图像由作者提供）

## 感谢阅读！:)

那么，这篇文章就到这里结束了！

和往常一样，这只是关于组图的冰山一角。你可以用 LaTeX 创建更多自定义和复杂的叠加图，但这应该足够让你朝着正确的方向入门。请在评论中告诉我你希望看到的 LaTeX 绘图内容！所以下次见面之前…

祝学习愉快！
