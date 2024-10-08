# 使用 Python 构建互动数据可视化 — 叙述的艺术

> 原文：[`towardsdatascience.com/building-interactive-data-visualizations-with-python-the-art-of-storytelling-ceb43db67488`](https://towardsdatascience.com/building-interactive-data-visualizations-with-python-the-art-of-storytelling-ceb43db67488)

## 简介指南

## Seaborn、Bokeh、Plotly 和 Dash，有效地传达数据洞察

[](https://polmarin.medium.com/?source=post_page-----ceb43db67488--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----ceb43db67488--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ceb43db67488--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ceb43db67488--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----ceb43db67488--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ceb43db67488--------------------------------) ·阅读时间 16 分钟·2023 年 6 月 4 日

--

![](img/944921b12b326517582e6fc337dd6090.png)

图片由 [Nong](https://unsplash.com/de/@californong?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

掌握叙述的艺术对数据科学家很重要，但对数据分析师尤为关键。

与那些不熟悉数据的人分享数据洞察和亮点，特别是那些可能没有技术背景的人，是数据分析过程中的重要部分。

如果他们不了解你所说的内容或没有被吸引，他们不在乎你是否在数据清洗和转换方面表现最好。

因此，可视化是最终叙述的一部分，它可以说是展示任何事实的最佳方式——这就是为什么每个人都在使用它们。

此外，像 Tableau 或 Power BI 这样的工具因其能够轻松创建互动仪表板而日益受到欢迎。商业人士通常会被带有图表和颜色的酷炫仪表板所震撼，因此他们已开始将其作为招聘要求。

今天，我将与大家分享一些 Python 选项，用于为那些不能或不喜欢/不想使用上述特定数据可视化工具的人创建互动式可视化。

在完成所有分析之后，你将不再需要放弃 Python——你还可以用它来以视觉形式分享你的见解。

更具体地说，我将讨论四个库：Seaborn、Bokeh、Plotly 和 Dash。

因为深入探讨所有这些内容可能会导致帖子极其冗长，所以我会尽量简洁，并重点展示它们能做些什么。在未来的帖子中，我可能会更详细地介绍其中一些（或所有），但会逐一进行。

今天的目录：

1.  **首先是数据**

1.  **Seaborn**

1.  **Bokeh**

1.  **Plotly**

1.  **Dash**

1.  **讲述故事**

> 你可以在这篇文章末尾的 **资源** 部分找到整个代码的链接。

那么我们开始吧。

## 1\. 首先是数据

如果我们没有故事可讲，那我们就无法讲故事，对吧？

由于我喜欢营养和健康领域，我将使用来自世界卫生组织 (WHO) 的公开数据。具体来说是 **成人超重的流行率，BMI >= 25（年龄标准化估计）（%）**[1]。

我们的目标是分析数据并查看我们能发现什么。它包含了从 1975 年到 2016 年按性别、国家和年份划分的流行率值。

> 免责声明：结果不会让你惊讶，因为这是一个广泛讨论的话题。但我们今天不是来发现新东西的。

我将使用笔记本来简化操作，但请记住，我们即将创建的图表也可以嵌入到网站中——这取决于我们使用的库——这才是真正的好处所在。

我稍微清理了一下数据，使其简单并为接下来的步骤做好准备。这里是代码片段，以便你可以按照相同的步骤操作：

```py
import pandas as pd
# Load the data
df = pd.read_csv("data.csv")
# Remove missing data and keep only useful columns
df = df.loc[
    df['Value'] != "No data", 
    ['ParentLocation', 'Location', 'Period', 'Dim1', 'Value']
]
# Rename columns
df.columns = ['ParentLocation', 'Location', 'Period', 'Gender', 'Prevalence']
# Some values in Prevalence contain ranges (like '13.4 [8.7 – 18.9]'), so we just keep the average
df['Prevalence'] = df['Prevalence'].apply(
    lambda x: float(x.split('[')[0]) if '' in x else x
)
```

这就是它的样子：

![

前 5 行 DataFrame — 作者提供的图片

我们准备开始绘图了。

## 2\. Seaborn

我相信你已经熟悉 Seaborn 了。它是与 Matplotlib 一起用于绘图的最常用库之一。你可能不知道的是，我们也可以用它来做动态可视化，而不仅仅是静态的。

但让我介绍一下，以防有人不熟悉。

用他们自己的话来说：“Seaborn 是一个基于 [matplotlib](https://matplotlib.org/) 的 Python 数据可视化库。它提供了一个高级接口，用于绘制吸引人且信息丰富的统计图表。”[2]

所以这就像是 Matplotlib 的改进版。

我将先使用一个简单的 `relplot()` 来绘制不同变量之间的关系：

```py
import seaborn as sns

# Apply the style
sns.set_style("ticks")
sns.despine()

# Prepare df for this graph
grouped_df = df.groupby(
    ['ParentLocation', 'Gender', 'Period']
)['Prevalence'].mean().reset_index()

# Create visualization
sns.relplot(
    data=grouped_df[
        grouped_df['Gender'] != 'Both sexes'
    ],
    kind="line",
    x="Period", 
    y="Prevalence", 
    col="Gender",
    hue="ParentLocation", 
    palette="Paired"
)
```

![](img/cad9dba2b7608a3f3ddccbb9e1905a14.png)

流行率-时间-性别关系 — 作者提供的图片

这里的可视化本身并不是重点。我们将在这篇文章的最后一部分通过模拟讲故事来提取一些见解。

我们得到的是静态图表。如果不通过更改代码，我们无法改变任何内容，这使得图表是静态的。如果我们想在 `ParentLocation` 基础上进行比较，那是可以的，但我们无法在国家级别上进行详细分析。这样做会导致同一张令人困惑的图表中出现大量线条。

这不是我们来这里的目的。

但 Seaborn 不允许我们实现交互式仪表板……不过，我们可以使用 `ipywidgets` 来提高互动性。

> 如果下一段代码在你的系统上不起作用，那是因为你需要通过在终端中运行以下命令启用小部件扩展 `jupyter nbextension enable --py --sys-prefix widgetsnbextension`

现在我们将绘制相同的 `relplot()`，但这次我们希望以可交互的方式比较国家。以下是如何操作：

```py
import ipywidgets as widgets

countries = sorted(pd.unique(df['Location']))

# Creates Dropdown Widget
def create_dd(desc, i=0):
    dd = widgets.Dropdown(
        options=countries, 
        value=countries[i], 
        description=desc
    )
    return dd

# Creates the relplot
def draw_relplot(country1, country2):
    sns.relplot(
        data=df[
            (df['Location'].isin([country1, country2]))
            & (df['Gender'] != 'Both sexes')
        ],
        kind="line",
        x="Period", 
        y="Prevalence", 
        col="Gender",
        hue="Location", 
        palette="Paired"
    )

# Generate the final widget
dd1 = create_dd('Country 1', 0)
dd2 = create_dd('Country 2', 1)
ui = widgets.HBox([dd1, dd2])

# Create the interactive plot and display
out = widgets.interactive_output(
    draw_relplot, 
    {'country1': dd1, 'country2': dd2}
)

display(ui, out)
```

这次较长。但我们所做的只是将一些代码包装到两个函数中 — 一个用于创建下拉菜单，另一个用于创建图表本身 — 然后通过使用 HBox 小部件来创建 UI，它是一个包装器，将传递的小部件分组。然后我们使用 `interactive_output` 函数来创建输出并显示所有内容。

结果：

![](img/aff1a643cc0541e07f525e1a44d7430f.png)

按性别的国家比较 — 图片由作者提供

我们在这里添加了一些互动性和动态性！我们已经可以一次比较两个国家，并查看它们的超重患病率差异。

情况变得更好了。

然而，这种方法有一个限制。图表不可交互，我们只是动态地显示数据。这就是我们想要的吗？也许在某些时候可以，但今天不行。

介绍我们的下一个新朋友……

## 3\. Bokeh

> 使用官方声明：
> 
> “只需几行 Python 代码，Bokeh 就能让你创建在 web 浏览器中显示的交互式 JavaScript 驱动的可视化。”[3]

Bokeh 是一个更加复杂且完整的交互图形和仪表板选项。我们实际上可以用 Bokeh 创建类似 Tableau 的仪表板。

缺点是这不是最用户友好的选项。即使安装对于初学者在 Jupyter Notebook 中使用也可能相当痛苦。

默认情况下，我们创建的图表和仪表板会呈现在 HTML 页面或文件中，但我们可以通过导入并调用 `output_notebook()` 函数将它们内联渲染在笔记本中。

让我们尝试用 Bokeh 复制我们用 Seaborn 创建的第一个图表，生成一个可交互的图形：

```py
from bokeh.plotting import figure
from bokeh.io import show, output_notebook
from bokeh.layouts import row
from bokeh.palettes import Paired

def prepare_figure(gender):
    l = figure(
        title=f"Gender = {gender}", 
        x_axis_label='Period', 
        y_axis_label='Prevalence',
        width=475, 
        outer_width=475,
        height=500,
    )
    for i, loc in enumerate(pd.unique(grouped_df['ParentLocation'])):
        l.line(
            grouped_df[
                (grouped_df['Gender'] == gender) 
                & (grouped_df['ParentLocation']==loc)
            ]['Period'], 
            grouped_df[
                (grouped_df['Gender'] == gender) 
                & (grouped_df['ParentLocation']==loc)
            ]['Prevalence'], 
            legend_label=loc, 
            line_width=2,
            color=Paired[12][i]
        )

    l.legend.location = 'top_left'
    l.legend.click_policy="mute"
    l.legend.label_text_font_size='8px'
    l.legend.background_fill_alpha = 0.4

    return l

# Render the figure inline
output_notebook()

# Create the first figure and input the data
l1=prepare_figure('Male')

# Create the second figure and input the data
l2=prepare_figure('Female')

p = row(l1, l2)

# Show the plot
show(p)
```

![](img/89ca35cac2b5426c05d9276257bc66b7.png)

按性别比较欧洲超重患病率，从 2000 年到 2016 年 — 图片由作者提供

太棒了！我们的第一个交互式数据可视化。但这仍然远未达到仪表板的概念。

此外，我们目前还没有选择显示数据的能力。我们只能显示/隐藏一些线条，并在图形中移动和放大/缩小。很酷，但我们需要更多。

尽管 Bokeh 内置了自己的小部件 — 我们可以创建下拉菜单等 — 我还是先到此为止。

Bokeh 绝对不适合初学者，我不想让任何人感到困惑。在我看来，与接下来的两个选项相比，它是一个较差的替代方案。

就像浪费时间和金钱讨论大米对你的好处一样，而实际上你可以专注于更健康的选择，如糙米，甚至其他蔬菜，如西兰花。

现在进入重点部分。

## 4\. Plotly

> 再次引用 **Plotly 的文档**：
> 
> Plotly Express 是 `plotly` 库的内置部分，是创建大多数常见图形的推荐起点。[…] 这些函数的 API 经过精心设计，尽可能一致且易于学习，使得在数据探索会话中，从散点图切换到条形图、直方图或旭日图都变得容易。[4]

Plotly 是一个令人惊叹的工具，我一再使用它，我可以保证它正如他们所说的那样：易于学习，且从一个图表切换到另一个图表非常简单。

然而，我们今天不会多谈 Plotly Express，因为我们要深入一点。我们将使用 Plotly 的图形对象来完成任务。

我们将直接进入允许我们创建国家比较图表的代码，并包含两个下拉框：

```py
from plotly.subplots import make_subplots
import plotly.graph_objects as go

# Define variables
colors = ['#a6cee3', '#1f78b4']

# Define widgets (using previous function)
dd1 = create_dd('Country 1', 0)
dd2 = create_dd('Country 2', 1)

# Create figure and traces
def create_figure(country1, countr
    fig = make_subplots(
        shared_xaxes=True, 
        shared_yaxes=True, 
        rows=1, 
        cols=2,
        vertical_spacing = 0,
        subplot_titles=("Gender = Female", "Gender = Male"),

    )

    for j, gender in enumerate(['Female', 'Male']):
        for i, loc in enumerate([country1, country2]):
            fig.add_trace(
                go.Scatter(
                    x=df[
                        (df['Gender'] == gender) 
                        & (df['Location'] == loc)
                    ]['Period'], 
                    y=df[
                        (df['Gender'] == gender) 
                        & (df['Location'] == loc)
                    ]['Prevalence'], 
                    name=loc, 
                    line=go.scatter.Line(color=colors[i]), 
                    hovertemplate=None,
                    showlegend=False if j==0 else True
                ), 
                row=1, 
                col=j+1 
            )

    # Prettify
    fig.update_xaxes(showspikes=True, spikemode="across")
    fig.update_layout(
        hovermode="x",
        template='simple_white'
    )

    return fig

fig = create_figure(countries[0], countries[1])

# Create the Figure Widget
g = go.FigureWidget(
    data = fig,
    layout=go.Layout(
        barmode='overlay'
    )
)

# Handle what to do when the DD value changes
def response(change):
    dfs = []
    for gender in ['Female', 'Male']:
        for loc in [dd1.value, dd2.value]:
            dfs.append(
                df[(df['Gender'] == gender)
                  & (df['Location'] == loc)]
            )

    x = [temp_df['Period'] for temp_df in dfs]
    y = [temp_df['Prevalence'] for temp_df in dfs]

    with g.batch_update():
        for i in range(len(g.data)):
            g.data[i].x = x[i]
            g.data[i].y = y[i]
            g.data[i].name = dd1.value if i%2 == 0 else dd2.value

        g.layout.barmode = 'overlay'
        g.layout.xaxis.title = 'Period'
        g.layout.yaxis.title = 'Prevalence'

dd1.observe(response, names="value")
dd2.observe(response, names="value")

container = widgets.HBox([dd1, dd2])
widgets.VBox([container, g])
```

这可能看起来很长，但主要是因为我已经将其格式化为无需水平滚动的形式。如果你仔细观察，你会发现我们实际上做的事情很少！

我们在重用之前片段的部分，例如 `create_dd()` 函数。是的，我们再次使用 `ipywidgets`，但它们与 Plotly 的集成非常顺畅。所有操作都通过他们的 `observe()` 方法和我们编写的 `response()` 函数来处理，每当任一下拉框的值发生变化时，都会执行这个函数。

这是视觉效果：

![](img/74916ab34a2b0698a5ee9d1aca23c83b.png)

使用 plotly 进行数据交互 — 作者提供的图片

这很简单！同样，使用 Bokeh 实现相同的效果是可能的，但在 Plotly 中学习如何做要快得多。无论哪种方式，结果都很棒。

进入我们的最后一个工具……

## Dash

Dash 本身不是一个绘图库。它是一个用于生成仪表盘的出色框架。最近，它在构建互动式基于 Web 的仪表盘方面的受欢迎程度不断上升。

*剧透*：Dash 是由 Plotly 的开发者创建的。所以这里我们仍然会使用 Plotly，但现在与 Dash 框架结合起来，看看我们可以用它们创建什么。

那么，我们如何使用 Dash 构建相同的图表呢？与我之前所做的将整个代码放在这里不同，我想将其分成几个部分，以便更好地理解。

> 正如我在开始时所说的，我会写单独的帖子来深入探讨这些框架。所以如果你感兴趣，请考虑关注我。

我们将从导入开始。请注意，我们将重用已经见过的代码，所以我不会重新导入像 Plotly 这样的框架。

```py
# Install dash and jupyter_dash
!pip install dash
!pip install jupyter-dash

# Import
import dash_core_components as dcc
from dash import html
from jupyter_dash import JupyterDash
```

请注意，我导入了 `JupyterDash`，这是因为我正在使用 Jupyter Notebook。如果你使用的是普通脚本，只需将最后一行替换为 `from dash import Dash`。

接下来我们要做的是创建应用：

```py
app = JupyterDash(__name__)
```

我希望你对 HTML 有所了解，因为我们现在要使用**Dash**的`html`模块做一点这方面的工作。我计划创建一个简单的 3 列 1 行网格，列宽分别为 15%、60%和 25%。第一列将用于下拉框，第二列用于图表，第三列将为空。

第一列将由一个包含标题和另一个包含下拉框的 div 容器组成。以下代码实现了它：

```py
 html.Div([ 
        html.H1('Countries'), 
        html.Div(
            [dcc.Dropdown(
                id='country1_dropdown',
                options=df['Location'].unique().tolist(),
                value='',
                placeholder='Select a country'
            ),
             dcc.Dropdown(
                 id='country2_dropdown',
                 options=df['Location'].unique().tolist(),
                 value='',
                 placeholder='Select a country'
            )]
        )
    ]
)
```

一开始可能会显得有些混乱，但其实非常简单。每个 HTML 元素都有其子元素在列表参数中（在这种情况下，`<div>`包裹着一个`<h1>`和另一个`<div>`，而两个下拉框在内层的`<div>`中）。

现在让我们创建图表代码：

```py
html.Div([
    dcc.Graph(
        id='chart'
    )
])
```

这个比较简单。它只是一个包裹着 ID 为 chart 的图表的`<div>`容器。

然而，如果没有应用样式，仪表盘是不可接受的。我们怎么给这些元素添加一些 CSS 呢？

很简单，每个元素都有一个可选的`style`参数，我们可以用来添加我们的 CSS。下面是最后一段代码如何加上样式：

```py
html.Div([
        dcc.Graph(
            id='chart', 
            style={}
        )
    ], style={
        'grid-column-start': 'second',
        'grid-column-end': 'third',
    }
)
```

我已经听到你说：“当然，我们知道如何创建这两个元素。但我们实际怎么创建布局呢？”很简单，只需将它们全部放入一个`<div>`容器中，并将其分配给`app.layout`：

```py
app.layout = html.Div([
    # Dropdown menu
    html.Div([ 
        html.H1('Countries'), 
        html.Div(
            [dcc.Dropdown(
                id='country1_dropdown',
                options=df['Location'].unique().tolist(),
                value='',
                placeholder='Select a country',
                style={'margin-bottom': '10px', 'max-width': '200px'}
            ),
             dcc.Dropdown(
                 id='country2_dropdown',
                 options=df['Location'].unique().tolist(),
                 value='',
                 placeholder='Select a country',
                 style={'max-width': '200px'}
            )]
        )
    ], style={
        'grid-column-start' : 'first',
        'grid-column-end' : 'second',
        'padding': '2%',
        'justify-self': 'center'
    }),

    # Plot
    html.Div([
        dcc.Graph(
            id='chart', 
            style={}
        )
    ], style={
        'grid-column-start' : 'second',
        'grid-column-end' : 'third',
    })
], style={
    'width': '100vw'
    'display': 'inline-grid',
    'grid-template-columns': '[first] 15% [second] 60% [third] 25%',
    'grid-template-rows': '[row] 100%',
    'grid-gap': '1rem',
    'align-items': 'right',
})
```

如果你尝试运行这个，你会看到空图表。我们可以通过**回调**向它们添加一些数据。这些回调有输入和输出依赖关系，用于更新显示的数据，并达到我们期望的交互效果。

你接下来会看到一个叫做`update_chart()`的函数，它基本上是我们在 Plotly 部分中使用的`create_chart()`函数的复制粘贴。

```py
colors = ['#a6cee3', '#1f78b4'] 

@app.callback(
    dash.dependencies.Output('chart', 'figure'),
    dash.dependencies.Input('country1_dropdown', 'value'),
    dash.dependencies.Input('country2_dropdown', 'value')
)
def update_chart(country1, country2):   

    # Create figure and traces
    fig = make_subplots(
        shared_xaxes=True, 
        shared_yaxes=True, 
        rows=1, 
        cols=2,
        vertical_spacing = 0,
        subplot_titles=("Gender = Female", "Gender = Male"),

    )

    for j, gender in enumerate(['Female', 'Male']):
        for i, loc in enumerate([country1, country2]):
            fig.add_trace(
                go.Scatter(
                    x=df[
                        (df['Gender'] == gender) 
                        & (df['Location'] == loc)
                    ]['Period'], 
                    y=df[
                        (df['Gender'] == gender) 
                        & (df['Location'] == loc)
                    ]['Prevalence'], 
                    name=loc, 
                    line=go.scatter.Line(color=colors[i]), 
                    hovertemplate=None,
                    showlegend=False if j==0 else True
                ), 
                row=1, 
                col=j+1 
            )

    # Prettify
    fig.update_xaxes(showspikes=True, spikemode="across")
    fig.update_layout(
        hovermode="x",
        template='simple_white',
    )

    return fig
```

所以这里的新内容是函数声明上方的部分。我们说回调依赖于两个输入——country1 和 country2——输出是一个`id='chart'`的图表。

最后，我们运行应用：

```py
# Run app
if __name__ == '__main__':
    app.run_server()
```

然后导航到**http://127.0.0.1:8050**或服务器运行的任何 IP，像我这里一样玩一玩：

![](img/49a5ed3cbabb42a8bc20fdc4633b15eb.png)

使用 Dash 和 Plotly 与数据交互 — 图片由作者提供

看看它是多么流畅。我们可以结合 HTML 和 Python 图表的力量，创建漂亮的仪表盘，这都要归功于 Dash。

## 6\. 讲述故事

这一切都是比较基础的。我们没有建立一个仪表盘，也没有讲述一个故事。我们只是看了一些让图表互动的工具。

在这一部分，我将分享一个示例仪表盘，任何人都可以用来向利益相关者讲述故事。它可能不是最漂亮或最完整的，但足以展示 Dash 的能力，并说服你开始使用它来进行未来的可视化。

> **免责声明**：请爱上 Dash 和 Plotly 提供的实用功能，而不是我实现的设计。我的目标是展示 Dash 的能力，而不是试图说服你超重是全球真正的问题。

![](img/bdca1150f00b9393b7264898860d73b5.png)

我为这个项目构建的简单仪表盘 — 图片由作者提供

现在来探讨一些见解：

**一般基础**

许多科学家普遍认为肥胖是一种流行病。它是否符合流行病的实际定义并不重要。重要的是它的流行率以及它对我们健康的负面影响。

超重是肥胖之前的阶段。因此，超重也很重要，因为如果不加以控制，可能会导致未来的肥胖。

如果我们检查 1975 年的世界地图与 2016 年的世界地图，可以清楚地看到后者的颜色更加鲜艳。这并不好，因为颜色越黄，超重流行率越高。所以，整体而言，全球的超重流行率有了很大增加。

全球化、技术和科学的进步以及许多其他因素使大多数国家的食品变得丰富。或者至少，我们可以猜测食品获取在总体上有所增加。这与日益增加的久坐生活方式以及过度消费不健康食品结合起来，导致了超重流行率的巨大增加。

哦，随着超重流行率的增加，肥胖率也随之增加。

**顶级国家**

我们看到瑙鲁或帕劳等国家的流行率极高（~88.7%）。我们尚未确认数据，但一些报告确实坚持解决这些国家的肥胖问题（而且，数据应来自世界卫生组织，应该是可靠的）。

现在聚焦于瑙鲁，“从 1980 年代开始，瑙鲁人过上了久坐的生活方式，饮食不健康，导致了太平洋地区最糟糕的健康状况”。[5]

此外，“瑙鲁约 90%的土地面积覆盖着磷酸盐矿床，大部分已被条带开采且不可耕种。这导致瑙鲁人依赖从澳大利亚和新西兰等大洋洲国家进口的含糖量和脂肪含量都很高的加工食品” [6]

我们可以说瑙鲁是一个异常值，但有几个国家处于类似的位置。至少可以说，这令人担忧。

**时间差异**

一个国家的流行病率是否增加了很多可能取决于几个因素，所有因素都应独立对待。

然而，我对新加坡感到着迷。它们在 41 年里几乎没有变化。几个西欧国家也在增幅最小的榜单上。我相信，如果我们寻找的话，可以提取出一个模式。

在另一端，博茨瓦纳在 1975 年和 2016 年之间的流行率大幅上升。然而，在 2016 年，它们仍然远未达到顶端。这可能是因为近年来它们的食品获取增加，但博茨瓦纳仍然是博茨瓦纳。

**性别**

在大多数国家，男性历史上通常比女性更容易超重。我们通常看到男性的超重情况比女性更多。

我们可以在这里尝试提出不同的假设，但有一点是不可否认的：女性在历史上比男性更关心自己的外貌，可能是由于社会压力。我们生活在一个性别歧视的世界中，这只是其中一些后果。

最近我们才看到像巴西这样的国家，女性的超重率已经超过了男性。这并不是值得高兴的事，但我们可以把它看作是一种女性主义行为吗？可能不能。

但如果我们进行概括，模式仍然很清晰。

**最终结论**

残酷的事实是，超重的普遍性——因此，肥胖——随着时间的推移在增加。而且它影响每个人：所有性别，所有国家。没有一个是安全的。

这真的算是流行病吗？有人可能这么认为。

令人担忧的是，在大多数情况下，超重是由于不良习惯、营养不良、久坐生活方式和睡眠不好所致。加上整体热量摄入已经持续多年增加且似乎没有平稳下来。

所有这些都依赖于个人，但大多数人显然没有采取任何行动。

已经证明肥胖会导致死亡：它与心脏病、中风、糖尿病、癌症等相关……而我们依然放任其发展。

我们必须停止这种情况，但我们只能照顾好自己。所以务必做出明智的选择。

正如 Jerzy Gregorek 所说：

> “艰难的选择，轻松的生活。轻松的选择，艰难的生活。”

# 结束

感谢你读完它！

这是我写过的最长的一篇文章，但我像孩子玩玩具一样享受了它。我希望你也一样。

本文的目的是分享我用来创建互动可视化的一些工具，并通过我创建的仪表板进行一种简短而非正式的讲述，以从我们看到的数据中提取一些见解和假设。

在资源部分，你将看到一个链接，检查用于此帖子的所有代码！[7]

```py
**Thanks for reading the post!** 

I really hope you enjoyed it and found it insightful.

Follow me and subscribe to my mailing list for more 
content like this one, it helps a lot!

**@polmarin**
```

如果你想进一步支持我，请考虑通过下面的链接订阅 Medium 的会员：这不会让你多花一分钱，但会帮助我完成这个过程。

[](https://medium.com/@polmarin/membership?source=post_page-----ceb43db67488--------------------------------) [## 通过我的推荐链接加入 Medium - Pol Marin

### 阅读 Pol Marin 的每一篇故事（以及 Medium 上成千上万其他作者的故事）。你的会员费用直接支持 Pol……

[medium.com](https://medium.com/@polmarin/membership?source=post_page-----ceb43db67488--------------------------------)

## 资源

[1] [成年人超重的流行率，BMI >= 25（年龄标准化估计）（%）](https://www.who.int/data/gho/data/indicators/indicator-details/GHO/prevalence-of-overweight-among-adults-bmi-=-25-(age-standardized-estimate)-(-)) — 世界卫生组织

[2] [seaborn: 统计数据可视化](https://seaborn.pydata.org/)

[3] [Bokeh](https://bokeh.org/)

[4] [Python 中的 Plotly express](https://plotly.com/python/plotly-express/) — Plotly

[5] 西谷高明（2012 年 5 月 27 日）。[“瑙鲁：一个被肥胖和糖尿病困扰的岛屿”](http://globe.asahi.com/feature/article/2012050200007.html)。*朝日新闻*。

[6] “[我在如此小的岛屿上见过太多的葬礼”：瑙鲁的惊人故事，这个世界上 2 型糖尿病发病率最高的小岛国](https://www.diabetes.co.uk/in-depth/i-have-seen-so-many-funerals-for-such-a-small-island-the-astonishing-story-of-nauru-the-tiny-island-nation-with-the-worlds-highest-rates-of-type-2-diabetes/)

[7] [互动可视化仓库](https://github.com/polmarin/interactive_viz) — GitHub
