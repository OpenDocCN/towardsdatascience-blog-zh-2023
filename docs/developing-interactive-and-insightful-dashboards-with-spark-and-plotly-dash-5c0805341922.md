# 使用 Spark 和 Plotly Dash 开发互动且富有洞察力的仪表板

> 原文：[https://towardsdatascience.com/developing-interactive-and-insightful-dashboards-with-spark-and-plotly-dash-5c0805341922?source=collection_archive---------1-----------------------#2023-06-21](https://towardsdatascience.com/developing-interactive-and-insightful-dashboards-with-spark-and-plotly-dash-5c0805341922?source=collection_archive---------1-----------------------#2023-06-21)

## 用于 Python Web 应用的互动大型数据可视化

[](https://medium.com/@jadezhang244?source=post_page-----5c0805341922--------------------------------)[![余黄，医学博士，计算机科学硕士](../Images/081af4b9fb5d1062b3e41b4cf54b2211.png)](https://medium.com/@jadezhang244?source=post_page-----5c0805341922--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5c0805341922--------------------------------)[![数据科学的未来](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5c0805341922--------------------------------) [余黄，医学博士，计算机科学硕士](https://medium.com/@jadezhang244?source=post_page-----5c0805341922--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F759013c23ad5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeveloping-interactive-and-insightful-dashboards-with-spark-and-plotly-dash-5c0805341922&user=Yu+Huang%2C+M.D.%2C+M.S.+in+CS&userId=759013c23ad5&source=post_page-759013c23ad5----5c0805341922---------------------post_header-----------) 发布于 [数据科学的未来](https://towardsdatascience.com/?source=post_page-----5c0805341922--------------------------------) ·11 分钟阅读·2023年6月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5c0805341922&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeveloping-interactive-and-insightful-dashboards-with-spark-and-plotly-dash-5c0805341922&user=Yu+Huang%2C+M.D.%2C+M.S.+in+CS&userId=759013c23ad5&source=-----5c0805341922---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5c0805341922&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeveloping-interactive-and-insightful-dashboards-with-spark-and-plotly-dash-5c0805341922&source=-----5c0805341922---------------------bookmark_footer-----------)![](../Images/a84741208d0c9d75f3aebf2c0c5744d0.png)

作者照片

# 1. 介绍

云数据湖被企业组织广泛采用作为一种可扩展且低成本的所有类型（结构化和非结构化）数据的存储库。在高效分析大规模数据集以获得数据驱动决策所需的有意义见解方面存在许多挑战。一个挑战是数据集的大小往往太大，无法容纳在单台机器上。通常需要一组服务器来处理大数据集。另一个挑战是如何在服务器上将数据分析结果轻松且经济高效地与相关客户/股东共享。

本文使用与 [1] 相同的开源数据集，展示了一个基于开源的 Web 应用框架，用于使用 Spark [2][3] 和 Plotly Dash[4] 开发交互式和有洞察力的仪表盘。该框架使我们能够在服务器上分析和可视化大规模数据集，并将数据分析和可视化的结果作为仪表盘在任何地方共享。

如图 1 所示，新 Web 应用框架由三个主要组件组成：

+   Spark SQL 服务（例如 DataFrame）用于分布式数据处理（见第 2 节）。

+   Plotly 绘图服务用于创建数据可视化图表作为仪表盘（见第 3 节）。

+   Dash Web 服务用于在服务器端 Plotly 绘图服务与仪表盘客户端之间进行交互（见第 4 节）。

![](../Images/5ccf6f48b19b6c81bf5967746c48a31d.png)

**图 1：** 高级应用框架架构。

# 2\. Spark SQL 服务用于分布式数据处理

如 [2] 中所述，PySpark（Spark 的 Python API）可以很容易地用于从云数据湖（如 AWS S3）读取 *csv* 文件。为了简便起见，本文假设本地计算机上有一个数据集 *csv* 文件 *train_data.csv* [1]，而不失一般性。

以下代码用于将 *csv* 文件加载到内存中作为 Spark SQL DataFrame：

```py
import pyspark
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName('hospital-stay').getOrCreate()
spk_df = spark.read.csv('./data/train_data.csv', 
                        header = True, inferSchema = True)
```

数据加载后，可以创建一个全局临时视图，以便动态数据查询。

```py
spk_df.createOrReplaceTempView("dataset_view")
```

一旦创建了数据集视图，我们可以像从数据库中查询常见数据一样使用 Spark SQL 查询数据。例如，以下代码查询所有 *dataset_view* 中年龄在 [21, 30] 范围内的行。

```py
age = "21-30"
sdf = spark.sql(f"SELECT * FROM dataset_view WHERE Age=='{age}'")
```

为了使用 Plotly 从 Spark DataFrame *sdf* 创建数据可视化图表，我们必须将其转换为 Pandas DataFrame *pdf*，因为 Plotly 不直接支持 Spark DataFrame。

```py
pdf = sdf.toPandas()
```

# 3\. 使用 Plotly 创建数据可视化图表

Plotly 支持生成多种不同类型的图表。其中一些适用于从连续的数值特征创建图表，而另一些则适用于从离散的分类特征创建图表。

本文使用 Plotly Express 库创建以下常见图表用于演示。

+   **数值特征图表：** 散点图、直方图和折线图

+   **分类特征图表：** 柱状图、直方图、折线图和饼图

## **3.1 数字特征的图表**

如前所述，常见的三种数字特征图表包括：

+   散点图

+   直方图

+   折线图

给定一对数字特征，散点图使用每对特征值作为坐标，在二维平面上绘制一个点。例如，下面的图展示了年龄在21到30岁之间的两个数字特征*病人ID*和*入院押金*的散点图。特征*入院类型*用于颜色编码。

![](../Images/b63e887e909c2f054cd2bac453813e63.png)

**图2：** 一对数字特征的示例散点图。

假设仪表盘用户选择了年龄范围[21, 30]，一对数字特征*x* = *patientid*和*y* = *Admission_Deposit*，以及*颜色*编码特征 = *入院类型*，以下语句创建了上述散点图。

```py
fig = px.scatter(dff, x = x, y = y, color = color_feature)
```

同样，以下语句用于创建相同数据的直方图：

```py
fig = px.histogram(dff, x = x, y = y, color = color_feature)
```

![](../Images/289d029f2adbe6cd802f307ab96fcf91.png)

**图3：** 一对数字特征的示例直方图。

为了完整性，以下语句用于创建折线图。

```py
fig = px.ine(dff, x = x, y = y, color = color_feature)
```

![](../Images/c2345109444641bd822b1efb276cdef2.png)

**图4：** 一对数字特征的示例折线图。

尽管我们可以很容易地创建折线图，但如上图所示的折线图并没有揭示有用的洞察。折线图的良好使用是将其应用于以有意义的方式排序的数据集，例如按时间顺序排列的数据序列或按计数排序的特征值列表，如第3.2节所示。

## **3.2 分类特征的图表**

本小节展示了四种常见的分类特征图表：

+   柱状图

+   直方图

+   折线图

+   饼图

假设仪表盘用户选择了*年龄* = [*21–30*]，分类*特征* = *停留*，*颜色* = *紫色*，*图表样式* = *柱状*，以下代码可以用来生成下面的柱状图。

```py
dff = spark.sql(f"SELECT * FROM dataset_view WHERE Age=='{age}'").toPandas()
vc = dff[feature].value_counts()
fig = px.bar(vc, x = vc.index, y = vc.values)
```

我注意到直方图与柱状图在分类特征值计数上的结果相同。

![](../Images/1b3fbc600a2ac8c2e18d2e98ec56c4e3.png)

**图5：** 分类特征值计数的示例柱状图。

通过选择*图表样式* = *折线*，我们可以创建如下折线图：

```py
fig = px.line(vc, x = vc.index, y = vc.values)
```

![](../Images/af328602ed38d039677b2fdfe01445b3.png)

**图6：** 分类特征值计数的示例折线图。

如前所述，折线图适合用于可视化分类特征的值计数。

以下代码用于创建同一分类特征*停留*的饼图。

```py
fig = px.pie(vc, x = vc.index, y = vc.values)
```

该饼图使用自动颜色编码，而不是所选颜色*紫色*。

![](../Images/aaf6658b472f925ccc723375a2561dd5.png)

**图7：** 分类特征值计数的示例饼图。

# 4. Dash用于互动数据可视化

前一节描述了如何在 Spark 服务器集群上使用 Plotly Express 库创建不同类型图表的仪表板。本节展示了如何使用 Dash 与 Web 应用程序客户端共享仪表板，并允许客户端以交互方式使用仪表板来以各种方式可视化数据。

可以按照以下步骤开发一个 Web 应用程序的一页仪表板：

+   第 1 步：导入 Dash 库模块

+   第 2 步：创建 Dash 应用程序对象

+   第 3 步：定义 HTML 页面仪表板布局

+   第 4 步：定义回调函数（Web 服务端点）

+   第 5 步：启动服务器

## 4.1 导入 Dash 库模块

首先，按照如下导入 Plotly Dash 库模块，以便于本文的演示。

```py
import plotly.express as px
from dash import Dash, dcc, html, callback, Input, Output
```

## 4.2 创建 Dash 应用程序对象

导入库模块后，下一步是创建 Dash 应用程序对象：

```py
app = Dash(__name__)
```

## 4.3 定义仪表板布局

一旦创建了 Dash 应用程序对象，我们需要定义一个作为 HTML 页面的仪表板布局。

本文中的仪表板 HTML 页面分为两个部分：

+   第 1 部分：数值特征的可视化

+   第 2 部分：分类特征的可视化

仪表板布局第 1 部分的定义如下：

```py
app.layout = html.Div([ # dashboard layout
    html.Div([ # Part 1
        html.Div([
            html.Label(['Age:'], style={'font-weight': 'bold', "text-align": "center"}),
            dcc.Dropdown(
                ['0-10', '11-20', '21-30', '31-40', '41-50', 
                '51-60', '61-70', '71-80', '81-90', '91-100',
                'More than 100 Days'],
                value='21-30',
                id='numerical_age'
            ),
        ],
        style={'width': '20%', 'display': 'inline-block'}),

        html.Div([
            html.Label(['Numerical Feature x:'], style={'font-weight': 'bold', "text-align": "center"}),
            dcc.Dropdown(
                ['patientid', 
                 'Hospital_code',
                 'City_Code_Hospital'],
                value='patientid',
                id='axis_x',
            )
        ], style={'width': '20%', 'display': 'inline-block'}),

        html.Div([
            html.Label(['Numerical Feature y:'], style={'font-weight': 'bold', "text-align": "center"}),
            dcc.Dropdown(
                ['Hospital_code',
                 'Admission_Deposit',
                 'Bed Grade',
                 'Available Extra Rooms in Hospital',
                 'Visitors with Patient',
                 'Bed Grade',
                 'City_Code_Patient'],
                value='Admission_Deposit',
                id='axis_y'
            ),
        ], style={'width': '20%', 'display': 'inline-block'}),

        html.Div([
            html.Label(['Color Feature:'], style={'font-weight': 'bold', "text-align": "center"}),
            dcc.Dropdown(
                ['Severity of Illness',
                 'Stay',
                 'Department',
                 'Ward_Type',
                 'Ward_Facility_Code',
                 'Type of Admission',
                 'Hospital_region_code'],
                value='Type of Admission',
                id='color_feature'
            ),
        ], style={'width': '20%', 'display': 'inline-block'}),

        html.Div([
            html.Label(['Graph Style:'], style={'font-weight': 'bold', "text-align": "center"}),
            dcc.Dropdown(
                ['scatter',
                 'histogram',
                 'line'],
                value='histogram',
                id='numerical_graph_style'
            ),
        ], style={'width': '20%', 'float': 'right', 'display': 'inline-block'})
    ], 
    style={
        'padding': '10px 5px'
    }),

    html.Div([
        dcc.Graph(id='numerical-graph-content')
    ]),
......
```

图 2、3 和 4 是通过使用仪表板布局第 1 部分创建的。

仪表板布局第 2 部分的定义如下：

```py
......
html.Div([ # Part 2
        html.Div([
            html.Label(['Age:'], style={'font-weight': 'bold', "text-align": "center"}),
            dcc.Dropdown(
                ['0-10', '11-20', '21-30', '31-40', '41-50', 
                '51-60', '61-70', '71-80', '81-90', '91-100',
                'More than 100 Days'],
                value='21-30',
                id='categorical_age'
            ),
        ],
        style={'width': '25%', 'display': 'inline-block'}),

        html.Div([
            html.Label(['Categorical Feature:'], style={'font-weight': 'bold', "text-align": "center"}),
            dcc.Dropdown(
                ['Severity of Illness',
                 'Stay',
                 'Department',
                 'Ward_Type',
                 'Ward_Facility_Code',
                 'Type of Admission',
                 'Hospital_region_code'],
                value='Stay',
                id='categorical_feature'
            ),
        ], 
        style={'width': '25%', 'display': 'inline-block'}),

        html.Div([
            html.Label(['Color:'], style={'font-weight': 'bold', "text-align": "center"}),
            dcc.Dropdown(
                ['red',
                 'green',
                 'blue',
                 'orange',
                 'purple',
                 'black',
                 'yellow'],
                value='blue',
                id='categorical_color'
            ),
        ], 
        style={'width': '25%', 'display': 'inline-block'}),

        html.Div([
            html.Label(['Graph Style:'], style={'font-weight': 'bold', "text-align": "center"}),
            dcc.Dropdown([
                 'histogram',
                 'bar',
                 'line',
                 'pie'],
                value='bar',
                id='categorical_graph_style'
            ),
        ], 
        style={'width': '25%', 'float': 'right', 'display': 'inline-block'})
    ], 
    style={
        'padding': '10px 5px'
    }),

    html.Div([
        dcc.Graph(id='categorical-graph-content')
    ])
]) # end of dashboard layout
```

图 5、6 和 7 是通过使用仪表板布局的第 2 部分创建的。

## 4.4 定义回调函数

仪表板布局只创建了一个静态的仪表板 HTML 页面。必须定义回调函数（即 Web 服务端点），以便仪表板用户的操作能够作为 Web 服务请求发送到服务器端回调函数。换句话说，回调函数使得仪表板用户与服务器端仪表板 Web 服务之间能够进行交互，例如在用户请求下创建新的图表（例如，选择下拉选项）。

本文为仪表板布局的两个部分定义了两个回调函数。

仪表板布局第 1 部分的回调函数定义如下：

```py
@callback(
    Output('numerical-graph-content', 'figure'),
    Input('axis_x', 'value'),
    Input('axis_y', 'value'),
    Input('numerical_age', 'value'),
    Input('numerical_graph_style', 'value'),
    Input('color_feature', 'value')
)
def update_numerical_graph(x, y, age, graph_style, color_feature):
    dff = spark.sql(f"SELECT * FROM dataset_view WHERE Age=='{age}'").toPandas()

    if graph_style == 'line':
        fig = px.line(dff, 
                 x = x, 
                 y = y, 
                 color = color_feature
                )
    elif graph_style == 'histogram':
        fig = px.histogram(dff, 
                 x = x, 
                 y = y, 
                 color = color_feature
                )
    else:
        fig = px.scatter(dff, 
                 x = x, 
                 y = y, 
                 color = color_feature
                )

    fig.update_layout(
        title=f"Relationship between {x} vs. {y}",
    )

    return fig
```

仪表板布局第 2 部分的回调函数定义如下：

```py
@callback(
    Output('categorical-graph-content', 'figure'),
    Input('categorical_feature', 'value'),
    Input('categorical_age', 'value'),
    Input('categorical_graph_style', 'value'),
    Input('categorical_color', 'value')
)
def update_categorical_graph(feature, age, graph_style, color):
    dff = spark.sql(f"SELECT * FROM dataset_view WHERE Age=='{age}'").toPandas()
    vc = dff[feature].value_counts()

    if graph_style == 'bar':
        fig = px.bar(vc, 
                 x = vc.index, 
                 y = vc.values
                )
    elif graph_style == 'histogram':
        fig = px.histogram(vc, 
                 x = vc.index, 
                 y = vc.values
                )
    elif graph_style == 'line':
        fig = px.line(vc, 
                 x = vc.index, 
                 y = vc.values
                )
    else:
        fig = px.pie(vc, 
                 names = vc.index, 
                 values = vc.values
                )

    if graph_style == 'line':
        fig.update_traces(line_color=color)
    elif graph_style != 'pie':
        fig.update_traces(marker_color=color)

    fig.update_layout(
        title=f"Feature {feature} Value Counts",
        xaxis_title=feature,
        yaxis_title="Count"
    )

    return fig
```

每个回调函数都与注解 @*callback* 相关联。与回调函数相关联的注解控制哪些 HTML 组件（例如，下拉框）在用户请求时向回调函数提供输入，以及哪个 HTML 组件（例如，*div* 标签中的图表）接收回调函数的输出。

## 4.5 启动服务器

Dash Web 应用程序的最后一步是启动 Web 服务服务器，如下所示：

```py
if __name__ == "__main__":
    app.run_server()
```

下图展示了当仪表板用户在仪表板中选择以下选项时的一种场景：

+   *age* 从 21 到 30

+   数值特征对 *patientid* 和 *Admission_Deposit*

+   分类特征 *Type of Admission* 用于数值特征可视化的颜色编码

+   *散点*图用于数值特征的可视化

+   分类特征*停留时间*用于计算特征值计数

+   条形图、直方图和折线图的颜色为*蓝色*

+   *饼*图带有自动颜色编码，用于可视化分类特征值计数

![](../Images/1efd5dc30411f2bb345f600955dd786a.png)

**图8：** 总体仪表板的一个视图。

作为获得可能有用见解的一个示例，上述仪表板场景揭示了以下见解：

+   大多数21-30岁的患者无论住院时间多长，其押金均在$3,000至$6,000之间

+   大多数21-30岁患者的住院时间为11-30天（27.6%）或21-30天（27.9%）

下图展示了当仪表板用户在仪表板中选择以下选项时的另一种场景：

+   年龄从21到30岁

+   数字特征对*patientid* 和 *Admission_Deposit*

+   用于数字特征可视化的分类特征*入院类型*的颜色编码

+   数字特征的直方图可视化

+   分类特征*入院类型*

+   条形图、直方图和折线图的颜色为*绿色*

+   用于可视化分类特征值计数的条形图

![](../Images/cce81db3eb4bc5e9c8a3b42564d3fa03.png)

**图9：** 总体仪表板的另一种视图。

作为获得可能有用见解的另一个示例，上述仪表板场景揭示了以下见解：

+   急诊患者的总押金高于其他入院类型的患者

+   大多数患者是作为创伤患者入院的

总结来说，仪表板允许用户以灵活的方式互动式地可视化数据，获得各种有用的见解，包括：

+   在给定年龄范围（如0-10岁、11-20岁等）中可视化数字和分类特征

+   在散点图、直方图和/或折线图中可视化任意一对数字特征

+   使用任何分类特征值进行数字特征可视化的颜色编码

+   将任何分类特征的值计数可视化为条形图/直方图、折线图和/或饼图，并进行不同的颜色编码

# 5. 结论

本文介绍了一个基于开源的Python Web应用框架，用于开发使用Spark [3] 和Plotly Dash[4] 的互动式和洞察性的仪表板。该框架允许我们分析来自云数据湖的大规模数据集，在Spark服务器上创建互动式仪表板，并允许用户随时与仪表板互动，以灵活的方式可视化数据，获得各种有用的见解。

# 参考文献

[1] Yu Huang, [预测Covid-19患者住院时间](/predicting-hospitalized-time-of-covid-19-patients-f4e70456db9b)

[2] [PySpark AWS S3读写操作](https://towardsai.net/p/programming/pyspark-aws-s3-read-write-operations)

[3] [Apache Spark示例](https://spark.apache.org/examples.html)

[4] [Dash Python 用户指南](https://dash.plotly.com/?_gl=1*1wl0w5v*_ga*MTQ5MTkwNjIwMi4xNjg3MTkzNTM0*_ga_6G7EE0JNSC*MTY4NzIxNTgwOC4yLjAuMTY4NzIxNTgwOC4wLjAuMA..)
