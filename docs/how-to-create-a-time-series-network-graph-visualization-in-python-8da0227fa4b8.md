# 如何在 Python 中创建时间序列网络图可视化

> 原文：[`towardsdatascience.com/how-to-create-a-time-series-network-graph-visualization-in-python-8da0227fa4b8`](https://towardsdatascience.com/how-to-create-a-time-series-network-graph-visualization-in-python-8da0227fa4b8)

## 使用 Plotly 和 NetworkX 展示网络如何随时间演变

[](https://ds-claudia.medium.com/?source=post_page-----8da0227fa4b8--------------------------------)![Claudia Ng](https://ds-claudia.medium.com/?source=post_page-----8da0227fa4b8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8da0227fa4b8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8da0227fa4b8--------------------------------) [Claudia Ng](https://ds-claudia.medium.com/?source=post_page-----8da0227fa4b8--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8da0227fa4b8--------------------------------) ·阅读时间 9 分钟·2023 年 10 月 26 日

--

![](img/32d47dc42370ba8c6be98f368dde3c7a.png)

可视化与选定医生的连接随时间变化

在这篇文章中，你将学习如何在 Python 中创建时间序列网络可视化，展示网络中的连接如何随时间发展，如上方动画所示。网络数据对于揭示连接非常有效，而时间序列数据可以用来揭示基础数据中的趋势和异常。

在本文中，我们将使用 Kaggle 上关于医疗服务提供者欺诈的 [数据集](https://www.kaggle.com/datasets/rohitrox/healthcare-provider-fraud-detection-analysis/data?select=Train_Beneficiarydata-1542865627584.csv) 创建一个示例。（该数据集目前在 Kaggle 上的许可证为 CC0: 公众领域。请注意，该数据集可能不准确，仅用于本文演示目的）。

我们将结合提供的链接中的数据，获取与给定主治医生相关的欺诈性索赔的集群，然后根据索赔开始日期绘制该医生与其他实体之间的连接（最多到某个跳数）随时间的变化。

确保在你的 Python 虚拟环境中安装了 [Plotly](https://plotly.com/python/getting-started/) 和 [NetworkX](https://networkx.org/documentation/stable/install.html)。

如果你想学习一种有效的方法来可视化网络如何随时间增长，请继续阅读！

# 数据集发现

该数据集包含总共 82,063 名不同的医生，其中 20,592 名至少有一个欺诈性索赔。虽然大多数这些医生只有少数几个欺诈性索赔，但也有少数人是极其活跃的欺诈者。

前 25% 的医生至少有 5 个索赔，最严重的例子是一位有 2,534 个欺诈性索赔的医生！

尽管在医生层面计算这些统计数据很容易，但时间序列网络可视化是理解该医生实施的诈骗规模及其时间范围的一个好方法。

# 查找连接

给定一个医生，如果我们能够找到其连接，并且这些连接也有欺诈性索赔，最多跳数为一定范围内，会怎样呢？

为此，我们将编写一个函数，该函数接受包含索赔数据的 DataFrame、AttendingPhysician（字符串）和 MaxHops（整数）作为参数，并返回一个 DataFrame，包含与给定 AttendingPhysician 相关的欺诈性索赔，最多跳数为 MaxHops。

从一个医生开始，该函数执行以识别与集群连接的具有欺诈性索赔的受益人和医生。只要迭代次数等于或小于 MaxHops 并且尚未收敛，即前一轮迭代的集群大小小于这一轮迭代的集群大小，函数将继续执行。

以下是一个代码片段，演示如何执行此操作：

```py
def get_fraud_cluster(df, AttendingPhysician, MaxHops):
    """
    Returns fraudulent claims associated with the given AttendingPhysician 
    and beneficiaries served up to a maximum number of hops or convergence

        Parameters:
            df (DataFrame): A Pandas DataFrame with claims data.
            AttendingPhysician (str): ID of attending physician to start with as the root node.
            MaxHops (int): Maximum number of hops.

        Returns:
            df (DataFrame): A Pandas Dataframe containing fraudulent claims associated 
            with supplied AttendingPhysician up to a maximum number of hops away.
    """
    # Initial variables
    prev_cluster_size = 0
    current_cluster_size = 1
    i = 0

    # Add initial physician to set_physicians
    set_physicians = set([AttendingPhysician])
    set_beneficiaries = set()

    while prev_cluster_size < current_cluster_size and i < MaxHops:

        prev_cluster_size = len(set_physicians) + len(set_beneficiaries)

        # Get fraudulent physicians with claims associated with fraudulent beneficiaries
        fraud_physicians = df[
            (df.BeneID.isin(set_beneficiaries)) &
            (df.PotentialFraud == 1)
        ]["AttendingPhysician"].unique()
        new_fraud_physicians = set(fraud_physicians).difference(set_physicians)

        # Update set with new fraudulent physicians
        set_physicians.update(new_fraud_physicians)

        # Get fraudulent beneficiaries with claims associated with fraudulent physicians
        fraud_beneficiaries = df[
            (df.AttendingPhysician.isin(set_physicians)) &
            (df.PotentialFraud == 1)
        ]["BeneID"].unique()
        new_fraud_beneficiaries = set(fraud_beneficiaries).difference(set_beneficiaries)
        # Upate set with new fraudulent beneficiaries
        set_beneficiaries.update(new_fraud_beneficiaries)

        # update variables
        i += 1
        current_cluster_size = len(set_physicians) + len(set_beneficiaries)

    # Final dataset of physicians and beneficiaries in sets with fraudulent claims
    df_results = df[
        (df.AttendingPhysician.isin(set_physicians)) &
        (df.BeneID.isin(set_beneficiaries)) &
        (df.PotentialFraud == 1)
    ]

    return df_results
```

现在你已经知道如何获取与给定医生相关的欺诈性索赔连接，让我们选择一个随机医生。以 AttendingPhysician `PHY379763` 为例，该医生只有 2 个索赔，但这两个索赔都是欺诈性的。

在使用上述函数找到该医生的连接（最多 2 跳）后，结果数据集中包含 1,483 个欺诈性索赔，涉及 920 个独特受益人和 2 位其他医生（总共 3 位不同的医生）。

想象一下，观察到一个仅有 2 个索赔的医生的连接，竟然可以如此迅速地扩展，实在令人惊讶！

# 创建时间序列网络图

在收集完连接数据后，我们现在准备创建可视化。这个过程可以分为 4 个步骤：i) 构建主图，ii) 按时间间隔绘制图形，iii) 按时间间隔绘制图表，iv) 配置图形。

## I. 构建图形

首先，使用 `networkx` 初始化一个空图，并从 DataFrame 中添加数据。在这个示例中，图将包含两组节点：一组是受益人节点，另一组是医生节点。

给节点标注 {type, ID (`BeneID` 或 `AttendingPhysician`), 索赔开始日期 (`ClaimStartDt`)} 是很重要的，因为稍后我们将使用日期来选择节点，以便从我们的月度图表中删除这些节点。

接下来，添加连接之间的边。在这种情况下，边存在于连接的受益人和医生之间，这些边的标签包含 {受益人 ID (`BeneID`), 医生 ID (`AttendingPhysician`), 索赔开始日期 (`ClaimStartDt`)}。同样，我们将使用日期来选择边，以便从月度图表中删除这些边。

以下是本节的内容：

```py
# Initialize empty networkx graph
G = nx.Graph()

# Build graph from df
for row in df.itertuples():
  # Add nodes for beneficiaries
  G.add_node(row.BeneID, label=("BeneID", row.BeneID, row.ClaimStartDt))
  # Assign a different node label (for node color later on) for root Physician
  if RootPhysicianID and row.AttendingPhysician == RootPhysicianID:
      G.add_node(row.AttendingPhysician, label=("RootPhysician", row.AttendingPhysician, row.ClaimStartDt))
  else:
      G.add_node(row.AttendingPhysician, label=("AttendingPhysician", row.AttendingPhysician, row.ClaimStartDt))
  # Add edges
  G.add_edge(row.BeneID, row.AttendingPhysician, label=(row.BeneID, row.AttendingPhysician, row.ClaimStartDt))

# Get positions
pos = nx.spring_layout(G)

print("Finished building graph (G).")
```

请注意，`RootPhysicianID` 是我们开始时使用的给定医生 ID，我们为该节点使用不同的标签，以便在绘图时可以给它分配不同的颜色。

此外，使用 `nx.spring_layout()` 函数获取节点和边缘的位置也很重要，以便在绘图步骤中将这些位置提供给每月图形，以便节点和边缘可以在时间上保持一致的位置。

## II. 每个间隔绘制图形

接下来，决定可视化图表的时间间隔和频率。在这种情况下，我们将使用按月的间隔，因此生成的图表将为数据集中的每个月创建滑块。

首先，通过抓取数据集中唯一的月份来确定需要循环的月份：

```py
# Grab unique months in dataset 
months = [
    i.to_timestamp("D") for i in pd.to_datetime(
        df["ClaimStartDt"]
    ).dt.to_period("M").sort_values().unique()
]
```

然后，遍历每个月以复制原始图表（`G`），并创建一个新的图表（`new_G`），我们可以修改它以显示该月的数据。然后，识别并删除在此月份之后出现的节点和边缘，基于标签中的索赔开始日期。删除这些节点、边缘和任何自环：

```py
# Loop through months
for month in months:
    month_str = month.strftime("%Y-%m")

    # Duplicate original graph for a new graph in current month
    new_G = G.copy()

    # Extract nodes and edges to remove 
    nodes_to_remove = [
        node for node in new_G.nodes() 
        if "label" in new_G.nodes[node] 
        and new_G.nodes[node]["label"][-1] >= month
    ]
    edges_to_remove = [
        (u, v) for u, v, data in new_G.edges(data=True) 
        if "label" in data and data["label"][-1] >= month
    ]

    # Remove self-loops
    for u, v, label in new_G.edges(data=True):
        if u == v:
            new_G.remove_edge(u, v)

    # Remove nodes and edges from new graph
    new_G.remove_edges_from(edges_to_remove)
    new_G.remove_nodes_from(nodes_to_remove)
```

现在我们为每个月创建了一个图形，是时候绘制它们并将它们添加到 Plotly 图中。

## III. 绘制图形

要使用 Plotly 绘制网络图形，首先从前一步中存储在变量 `pos` 中的位置选择节点和边缘的 x 和 y 坐标。

我们还会为不同的节点添加额外的文本和/或颜色，然后将这些边缘和节点添加到图形中。我们将选择并定义不同节点的颜色，并将其存储在一个名为 `color_mappings` 的字典中。键对应于节点标签，值是颜色的十六进制代码。这里有一个有用的 [网站](https://palettes.shecodes.io/)，可以选择搭配好的颜色。

以下是绘制图形的函数：

```py
def plot_graph(G, pos, fig):
    # Define colors with hex codes
    color_mappings = {
        "BeneID": "#66bfbf", # teal green
        "AttendingPhysician": "#fecea8",  # light orange
        "RootPhysician": "#f76b8a", # pinkish red
    }

    # Select nodes
    node_x = [pos[node][0] for node in G.nodes() if node in pos]
    node_y = [pos[node][1] for node in G.nodes() if node in pos]

    # Select edges
    edge_x = []
    edge_y = []
    for u, v, label in G.edges(data=True):
        if u in pos and v in pos:
            x0, y0 = pos[u]
            x1, y1 = pos[v]
            edge_x.extend([x0, x1, None])
            edge_y.extend([y0, y1, None])

    # Additional text to show on hover
    node_text = [data["label"] for node, data in G.nodes(data=True)]
    node_colors = [color_mappings.get(data["label"][0]) for node, data in G.nodes(data=True)]

    # Add nodes and edges to plot
    fig.add_trace(
        go.Scatter(
            x=edge_x, 
            y=edge_y,
            mode="lines",
            line=dict(color="gray"),
            name="Edges"
        )
    )
    fig.add_trace(
        go.Scatter(
            x=node_x,
            y=node_y,
            mode="markers",
            marker=dict(size=10, color=node_colors),
            text=node_text,
            hoverinfo="text",
            opacity=0.8,
            name="Nodes",
        )
    )

    return fig
```

上面的函数很有用，将用于绘制每个月的图形，如下所示：

```py
# Plot new graph for this month and add to figure
fig = plot_graph(G=new_G, pos=pos, fig=fig)
```

## IV. 配置图形

到目前为止，我们有一个包含所有节点和边缘的主图（`G`），多个包含所有节点和边缘（含索赔开始日期到该月的 `new_G`）的每月图形，以及一个包含多个数据集的 Plotly 图（`fig`），绘制了每个月的网络图形。

接下来，我们需要添加滑块的步骤。滑块上应该有每个月的步骤，我们需要配置每个步骤显示的数据显示。

记住，我们每个月有两个数据集，一个是节点，一个是边缘，因此我们将相应地设置这些数据集的可见性：

```py
# Configure slider
steps = []
for i, month in enumerate(months):
    idx = i * 2   # multiply by 2 because one trace is for nodes and one is for edges
    month_str = month.strftime("%Y-%m")
    visibility = [False] * len(months) * 2   # Make all traces not visible by default
    visibility[idx] = True   # Make trace for nodes visible
    visibility[idx + 1] = True    # Make trace for edges visible
    step = dict(
        method="update",
        label="Month: %s" % month_str,
        args=[{"visible": visibility}],
    )
    steps.append(step)

# Make first graph visible
fig.data[1].visible = True
```

在为滑块创建步骤后，我们可以更新图形的布局，添加滑块、标题和其他杂项配置：

```py
# Update figure settings
fig.update_layout(
    title="Time Series Graph Visualization for Root PhysicianID = %s" % RootPhysicianID,
    showlegend=False,
    hovermode="closest",
    xaxis=dict(showgrid=False, zeroline=False, showticklabels=False),
    yaxis=dict(showgrid=False, zeroline=False, showticklabels=False),
    sliders=[
        dict(
            active=0,
            steps=steps,
            pad={"t": 50}
        ),
    ],
)
```

剩下的就是显示图形，你的时间序列网络图应该会出现！

最后，如果你希望将交互式图保存为 HTML 文件，请使用以下命令：

```py
plotly.offline.plot(fig, filename="./graph_viz.html")
```

# 结论

网络可视化非常强大，加入时间序列组件使其更加信息丰富！可视化网络的增长在各种领域都有许多应用，从理解疾病的起源到跟踪服务的扩展等等。

我们看到的例子涉及到查找与选定医生相关的欺诈性索赔，最多两步远，然后绘制这些数据，查看该集群的欺诈行为如何发展。

总结一下，你学会了如何找到与给定实体的连接，最多经过的跳数，并通过以下方式创建时间序列网络图：

i) 构建主要图（`G`），

ii) 为每个时间间隔（在此情况下为每月）绘制新图，

iii) 绘制每个月的图表（多个 `new_G`），并将其添加到图形（`fig`）中，

iv) 配置图形的滑块。

看到医生和受益人之间的联系随着时间的推移而增长，真是令人着迷！

在下面评论你的想法和学习经历。你会尝试用这些数据做什么？在创建可视化过程中你得出了什么见解？

+   链接到这个 [Jupyter notebook](https://github.com/claudian37/DS_Portfolio/tree/master/healthcare_fraud_network_viz) 在 Github 上

*你在分析师角色中感到困惑吗？你可能会感到一种冒名顶替综合症，试图在没有博士学位或计算机科学背景的情况下学习新技能。*

*我在 2020 年转行数据科学，没有技术背景，我希望帮助其他人做到这一点。我创建了一个* [***免费五天数据科学启动课程电子邮件课程***](http://www.ds-claudia.com) *来启动你的职业生涯。🚀*

[www.ds-claudia.com](http://www.ds-claudia.com)
