# 如何使用 Plotly 图形对象构建瀑布图

> 原文：[`towardsdatascience.com/how-to-build-waterfall-charts-with-plotly-graph-objects-a8354543c42e`](https://towardsdatascience.com/how-to-build-waterfall-charts-with-plotly-graph-objects-a8354543c42e)

## Plotly Express 不支持瀑布图，但我们可以创建一个利用 Plotly 图形对象的辅助函数

[](https://medium.com/@alan-jones?source=post_page-----a8354543c42e--------------------------------)![Alan Jones](https://medium.com/@alan-jones?source=post_page-----a8354543c42e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a8354543c42e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a8354543c42e--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----a8354543c42e--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a8354543c42e--------------------------------) ·阅读时间 8 分钟·2023 年 9 月 14 日

--

![](img/7e1bc52e691c1d6e2bec6531912b3380.png)

作者提供的图片

Plotly 提供了两种绘制图表的方法：图形对象和 Plotly Express。前者是一组低级函数，为创建图表提供最大的灵活性，而 Plotly Express 则提供了一组易于使用的方法，来实现最常用的图表。

Plotly Express 函数本质上是对 Plotly 图形对象的封装。

但 Plotly Express 中没有瀑布图方法，因此我们将介绍一个简单易用的瀑布图函数，它适用于最常见的用例，并且具有应对更复杂用法的灵活性。

## 瀑布图

瀑布图有点像被分成多列的条形图。它们通常用于显示一个值随时间的增加和减少。例如，考虑以下数据。

```py
labels = ["Start balance", "Consulting", "Net revenue", 
          "Purchases", "Other expenses", "Profit before tax"]
data = [20, 80, 10, -40, -20, 0 ]
```

标签表示不同类别中已接收或已支出的现金金额，数据则是实际金额（以美元、百元、千元…等为单位）。

我们可以将这些数据有效地表示为瀑布图，其中第一列是起点，最后一列是终点，中间的列展示了导致最终结果的现金流。

![](img/81a6702a8bf88267e2a53847b3ffeaa8.png)

作者提供的图片

通常，瀑布图通过颜色区分正负金额——在本例中，正数为绿色，负数为红色。最终一列使用第三种颜色，因为这代表的是最终结果，而不是正变或负变。

这是一个典型的用例，尽管更复杂的图表也是可能的。

在她的书《*数据讲故事*》[1]中，Cole Nussbaumer Knaflic (CNK) 将瀑布图视为她的 12 个必备图表之一，并给出了如何用它描述随时间变化的就业情况的示例。这是我对她图表的版本。

![](img/7136b6890eebf61a22016fa80dec42b6.png)

作者提供的图像

你可以看到，我们从 100 开始，这个数字因各种原因增加和减少，直到最终人数为 116。

CNK 偏好所有列使用单一颜色，严格来说，前面图表中的颜色确实是多余的，因为从标签和列的方向（向上或向下）可以清楚地看出变化是正向还是负向。

我理解 CNK 的观点，用她的风格呈现的图表可能比多彩的图表更好看，但我也看到，尤其是在更复杂的图表中，颜色可能会很有帮助。这是一个人数图表的变体，其中正负变化混合在一起，你可能会同意红绿版本稍微更清晰（如果，也许，不如多彩版本那么美观）。

![](img/ba4d606d37f63eaf4557fd6fa385f514.png)

作者提供的图像

![](img/cfe2e84b4ddbc4befc86d09ce73a4a84.png)

作者提供的图像

我们将开发的解决方案默认为多彩版本，但如果你愿意，也可以更改颜色方案。

## 使用函数

我在一个单独的文件`plotlyhelper.py`中实现了瀑布图功能，以便可以作为库函数使用。导入库后，简单用法如下：

```py
fig = ph.waterfall(labels,data, title)
```

我们可以在任何可以渲染 Plotly 图表的环境中使用这个函数，例如，在 Flask 应用程序、Jupyter Notebook 或 Streamlit 应用程序中。我们稍后会查看实现，但这是在 Streamlit 应用程序中使用它的方法。

```py
import streamlit as st
import plotlyhelper as ph

st.set_page_config(layout='wide')

st.title("Waterfall graphs with Plotly")

col1, col2 = st.columns(2)

labels = ["Beginning HC", "Hires", "Transfers in", "Transfers out", 
          "Exits", "Ending HC"]
data = [100, 30, 8, -12, -10, 0 ]
title = "Headcount"

fig = ph.waterfall(labels,data, title)

col1.plotly_chart(fig)

fig = ph.waterfall(labels,data, title, color = 'blue')

col2.plotly_chart(fig)
```

这将给你两个“人数”图表版本，分为两列。

![](img/45393a19fee2db3832ad4badee45801f.png)

作者提供的图像

必须的参数是标签和数据，在上述示例中，我们还指定了一个标题（如果不设置，默认为空字符串）。使用这些参数，我们将得到默认的——多彩——版本的图表。

## 更改颜色方案

在第二个示例中，我们还设置了`color`参数，它定义了图表中所有条形的颜色。这可以是 Plotly 接受的任何颜色值。

我们还可以使用参数`bicolor, dcolor, tcolor,` 和 `ccolor`来改变每个条形的颜色，这些参数分别定义了递增条形、递减条形、最终条形和条形之间连接线的颜色。它们的默认颜色分别为“绿色”、“红色”、“蓝色”和“深灰色”。

如果你设置了`color`参数，它将覆盖你可能设置的任何其他颜色。

这是如何设置单个颜色的示例。

```py
fig = ph.waterfall(labels,data, title, icolor = 'orange', 
                                       dcolor = 'pink', 
                                       tcolor = 'yellow', 
                                       ccolor='red')
st.plotly_chart(fig)
```

这将绘制出这个。

![](img/b300de157c96670f56188bafa64bfef5.png)

作者提供的图像

也许我们可以举办最炫目颜色方案的比赛。

我们还可以探索其他几个参数。

## 注释

单个条形图上的标签默认为数据，但可以使用 `annotation` 参数进行自定义。这个数组可以是字符串或数字值，将显示在条形图上。例如，这里只有第一个和最后一个条形图被标记。

```py
annotation = ["Start", "", "", "", "", "End"]

labels = ["Start balance", "Consulting", "Net revenue", "Purchases", "Other expenses", "Profit before tax"]
data = [20, 80, 10, -40, -20, 0 ] 
title = "Sales revenue"

fig = ph.waterfall(labels,data, title, annotation=annotation)
```

![](img/d6c7d4b3d9d14607ff162ebc033b9968.png)

图片由作者提供

如果不需要标签，可以将 `annotation` 设置为空列表。

## 多个时间段

有时我们想用中间总计表示一系列时间段。我们可以通过指定每个条形图的类型来实现这一点。在默认图表中，除了最后一个条形图之外，所有条形图都是“相对”的，这意味着它们会相对于当前的累计总值显示为正值或负值。最后一个条形图是“总计”类型，这意味着它会显示当前的累计总值。

为了表示多个时间段，我们使用中间的“总计”条形图。看这个例子。

![](img/66e7ad5fcfb578cc3ad57df6dbd1fdb3.png)

图片由作者提供

在这里，我们展示了两个季度的利润和亏损金额，其中“Q1”有一个中间总计，“Q2”有一个最终总计。

为了实现这一点，我们指定一个条形图类型的列表，并将其作为参数 `measure` 传递。

```py
labels = ["Year beginning", "Profit1", "Loss1", "Q1", "Profit2", "Loss2", "Q2"]
data = [100, 50, -20, 0, 40, -10, 0 ]
annotation = []
measure = ['relative','relative', 'relative','total', 
           'relative','relative','total']
title = "Profit/Loss"

fig = ph.waterfall(labels,data, title, 
                   annotation = annotation, 
                   measure=measure)

st.plotly_chart(fig)
```

你应该注意到，映射到中间总计的数据数组元素被赋予了正确的值 130，但是，与其他条形图不同的是，这个值*不会*用于计算条形图的长度；条形图的长度是根据先前的值自动计算的，这个条形图的数据值仅作为注释使用。

## 瀑布图辅助函数

这些都引出了实际的实现。

虽然 Plotly Express 没有实现瀑布图，但 Graph Objects (GO) 包含这样的功能。使用起来有点繁琐——这也是我开发这个简单封装器的原因。如下面的代码所示，GO 瀑布图功能要求你设置许多参数。辅助函数通过设置默认值和/或计算值消除了这一需求——用户只需设置两个或三个基本参数。

```py
import plotly.graph_objects as go

def waterfall(labels, data, title="", annotation=None, 
              icolor="Green", dcolor="Red", 
              tcolor="Blue", ccolor='Dark Grey', 
              color=None, measure=None):
    """
    Create a waterfall chart using Plotly.

    Parameters:
        labels (list): A list of labels for the data points.
        data (list): A list of numerical values representing the data points.
        title (str, optional): The title of the chart. Defaults to an empty string.
        annotation (list, optional): A list of annotations for each data point. Defaults to None.
        icolor (str, optional): Color for increasing values. Defaults to "Green".
        dcolor (str, optional): Color for decreasing values. Defaults to "Red".
        tcolor (str, optional): Color for the total value. Defaults to "Blue".
        ccolor (str, optional): Connector line color. Defaults to 'Dark Grey'.
        color (str, optional): Common color for all elements. Defaults to None.
        measure (list, optional): A list specifying whether each data point is 'relative' or 'total'. Defaults to None.

    Returns:
        plotly.graph_objs._figure.Figure: A Plotly Figure containing the waterfall chart.
    """
    # Set default measure values if not provided
    if measure is None:
        measure = ['relative'] * (len(labels) - 1)
        measure.append('total')

    # Set default annotation values if not provided
    if annotation is None:
        annotation = data[:-1]
        annotation.append(sum(data))

    # Create the waterfall chart figure
    fig = go.Figure(go.Waterfall(
        orientation="v",
        measure=measure,
        textposition="outside",
        text=annotation,
        y=data,
        x=labels,
        connector={"line": {"color": ccolor}},
        decreasing={"marker": {"color": dcolor}},
        increasing={"marker": {"color": icolor}},
        totals={"marker": {"color": tcolor}}
    )).update_layout(
        title=title
    )

    return fig
```

你可以将这段代码剪切并粘贴到自己的代码中，或者访问我的网站下载这个函数和一个实现了上述所有示例的 Streamlit 应用程序。作为额外奖励，下载的 Streamlit 代码还将包括一个在 Matplotlib 中实现的瀑布图版本。

我希望你觉得这个关于瀑布图及其在 Plotly Graph Objects 中实现的介绍有用。如果你在辅助库中包含这样的功能，Plotly Express 缺少瀑布图并不重要，它在多个项目中使用时会使工作变得简单得多。

感谢阅读，请访问我的[网站](http://alanjones2.github.io)，在这里你可以找到其他文章和代码的链接。你也可以订阅我的偶尔发布的[通讯](http://technofile.substack.com)，我会在上面发布一些完整的文章以及我在 Medium 上发布的内容的链接。

## 注意事项

1.  [*用数据讲故事：商业专业人士的数据可视化指南*](https://amzn.to/3dJlMaS)，Cole Nussbaumer Knaflic，Wiley，2015（附属链接）

1.  英式拼写——我完全意识到我在这篇文章中混用了英式和美式拼写，但我保持了一致——老实说，我确实是这样。我在文本中使用‘colour’的英式拼写，因为……好吧，我是英国人。但是在我的代码中使用‘color’的美式拼写，因为我是程序员——我习惯于大多数编程语言使用美式英语。
