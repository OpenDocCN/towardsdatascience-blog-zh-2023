# 使用 Plotly 创建动态区域图可视化

> 原文：[`towardsdatascience.com/creating-dynamic-choropleth-visualizations-using-plotly-61d266cbe9b4`](https://towardsdatascience.com/creating-dynamic-choropleth-visualizations-using-plotly-61d266cbe9b4)

## 使用易于学习的包来创建复杂的可视化

[](https://hd2zm.medium.com/?source=post_page-----61d266cbe9b4--------------------------------)![Hari Devanathan](https://hd2zm.medium.com/?source=post_page-----61d266cbe9b4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----61d266cbe9b4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----61d266cbe9b4--------------------------------) [Hari Devanathan](https://hd2zm.medium.com/?source=post_page-----61d266cbe9b4--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----61d266cbe9b4--------------------------------) ·12 min 阅读·2023 年 12 月 18 日

--

![](img/17c9e259f0a21a20f9cdd5fc479fc7db.png)

图片来源：[NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

可视化数据是数据科学家经常忽略的一步。它帮助我们通过分析和整理数据来讲述故事，使其易于理解。通过去除所有技术细节和噪音，突出关键信息，数据科学家可以向非技术经理和高管解释其工作的意义。

现在有许多工具可以帮助可视化数据。多年来，Microsoft Excel 主导了静态可视化市场。随着时间的推移，我们转向了动态可视化，灵活展示更多数据。两种类型的工具帮助创建了动态可视化。

+   **商业智能和分析软件：** Tableau，PowerBI

+   **开源编程库：** D3.js，Plotly Dash

像 Tableau 和 PowerBI 这样的第三方软件工具对于非技术人员非常优秀。拖放界面和抽象功能使分析师能够轻松创建动态仪表板。缺点是

+   软件工具价格昂贵

+   学习这些工具需要一些时间

+   可视化设计的限制；软件可能不允许某些组件

开源编程库对技术人员非常优秀。那些熟悉软件工程的人可以轻松地按照文档创建灵活的动态可视化。此外，这些软件包是免费的（Plotly 为其企业 Dash 组件提供了付费版本）。

D3.js 和 Plotly 的区别如下

+   D3.js 使用 JavaScript 设计，Plotly 使用 Python 设计

+   D3.js 的出现时间比 Plotly 要早，因此拥有更好的社区支持和更成熟的生态系统（丰富的示例和教程）。

+   D3.js 要求工程师了解 Web 开发的低级细节（HTML、CSS、JavaScript）才能有效使用它。Plotly 将这些细节抽象为易于使用的 Python 类。

+   D3.js 由于其 JavaScript 特性（异步、领域对象模型、函数）有较陡的学习曲线，但可以构建各种复杂的动态可视化。Plotly 因其 Python 特性学习曲线较小，但在用户可以构建的可视化类型上有所限制。

如果是在 2019 年，我会推荐 D3.js。当时，Plotly 在我想要构建的可视化类型上受到了严重限制。

然而，我再次查看了 Plotly，并对其改进印象深刻。清晰的界面使其可以与一些 D3.js 的自定义可视化相抗衡。此外，我不需要学习 JavaScript 的复杂细节就能有效使用这个包。

实际上，我仅用 147 行代码就创建了一个复杂的可视化。下面是该可视化的视频。

**视频 1**：由作者创建的名称仪表盘 [视频](https://www.youtube.com/watch?v=NmDFOqCduts&t=4s)

那么，这个可视化做了什么？

+   它有一张地图，显示有多少人使用特定的名字。特定名字基于最右侧的下拉菜单**选择名称以查看每个州的流行度**。

+   **选择名称以查看每个州的流行度**下拉菜单推荐的名称是美国最受欢迎的 N 个名称。这些 N 个名称由中间的下拉菜单**前 N 名名称**筛选，其中 N 可以是 5、10 或 15。

+   这些名称也通过**性别**进行筛选。**性别**筛选器位于最左侧，用户可以选择男性或女性。

+   一旦通过**性别、前 N 名名称**和**选择名称以查看每个州的流行度**来深入可视化，地图将显示按州和年份划分的选定名称的流行度。用户可以通过拖动年份筛选器来即时显示所选名称的流行度。

+   如果用户点击一个州，下方的条形图会更新。条形图根据点击时的州和年份显示该州和年份的前 N 名流行名称。前 N 名名称基于筛选器**前 N 名名称**。

+   它有一个加载旋转器，用于提示当前数据正在加载，从而改善用户体验。

惊人的部分是什么？这个复杂的可视化仅用***148 行*** Python 代码完成。一些复杂的部分仅用 1-3 行代码添加！请查看下面的代码库。

[](https://github.com/hd2zm/Data-Science-Projects/blob/master/Name-Dashboard/name_dashboard.py?source=post_page-----61d266cbe9b4--------------------------------) [## 数据科学项目/名称仪表盘/name_dashboard.py 在 master 分支 · hd2zm/数据科学项目]

### 通过在 GitHub 上创建账户来为 hd2zm/Data-Science-Projects 开发做出贡献。

[github.com](https://github.com/hd2zm/Data-Science-Projects/blob/master/Name-Dashboard/name_dashboard.py?source=post_page-----61d266cbe9b4--------------------------------)

我对这个包的直观性感到惊讶。开发者在设计时考虑了很多，使得即便是编程经验有限的人也能轻松使用。

虽然文档很简单明了，但我会详细介绍如何构建每个组件。

# Plotly 组件

## 加载数据

我们正在加载两个数据集

+   **names_by_state_cleaned.csv** — 一个自定义数据集，列出了一个人的所有名字、拥有这种名字的人数、年份和州。该数据集由[社会保障管理局网站](https://www.ssa.gov/oact/babynames/)下载并清理。这些数据集[可用于商业用途](https://www.ssa.gov/oact/babynames/limits.html)。数据集以包含多个.txt 文件的文件夹形式下载，并通过自定义脚本合并为一个 CSV 文件。这个 CSV 文件大小为 150 MB，上传到 GitHub 有点大。为方便起见，我附上了数据集的示例截图。

![](img/3f83048ed8a40109994a39ae2687666f.png)

**图 1**：**names_by_state_cleaned.csv**的示例数据集

+   **us-states.json** — 一个包含美国所有州地理坐标的数据集。这用于创建分级地图。文件大小为 88KB，可以轻松加载到内存中。[我已经将 json 文件上传到包含可视化代码的 Git 仓库](https://github.com/hd2zm/Data-Science-Projects/blob/master/Name-Dashboard/us-states.json)。

创建应用程序后，我将两个数据集加载到内存中。免责声明：我会在稍后解释为什么从生产角度来看，将 CSV 文件加载到内存中不是一个好主意。现在，我将保持现状。

```py
app = Dash(__name__)

df = pd.read_csv('names_by_state_cleaned.csv')
f = open("us-states.json")
us_states = json.load(f)
```

## HTML 布局

这是应用程序的 HTML 设计

```py
app.layout = html.Div(className = 'dashboard', children = [
    html.H1(children='Popular Names Per US State', style={'textAlign':'center'}),
    html.Div(className='filters', children=[
        html.Div(children=[
            html.H2(['Gender'], style={'font-weight': 'bold', "text-align": "center","offset":1}),
            dcc.RadioItems(
                id='sex-radio-selection', 
                options=[{'label': k, 'value': k} for k in ["Male", "Female"]],
                value=initial_sex,
                inline=True,
                style={"text-align": "center"}
            )], style=dict(width='33.33%')),
        html.Div(children=[
            html.H2(['Top N Names'], style={'font-weight': 'bold', "text-align": "center","offset":1}),
            dcc.RadioItems(
                id='rank-dropdown-selection', 
                options=[5, 10, 15],
                value=initial_rank,
                inline=True,
                style={"text-align": "center"}
            )], style=dict(width='33.33%')),
        html.Div(children=[
            html.H3(['Choose Name To See Popularity Per State On Map'], style={'font-weight': 'bold', "text-align": "center","offset":6}),
            dcc.Dropdown(
                options=[{'label': k, 'value': k} for k in names_in_initial_rank],
                id='top-rank-name-dropdown-selection',
                value=names_in_initial_rank[0],
                style={"text-align": "center"}
            ),
            html.Label(['**if map is fully gray, make sure a name is selected in dropdown above'], style={"text-align": "center","offset":6}),
            ], style=dict(width='15%')),
            html.Div(children=[
                html.Label(['Click on state in map below to view Top <N> Popular <Gender> Names in bar graph below the map.'], style={"text-align": "right"}),
            ],  style=dict(width='33.33%')),
        ], style=dict(display='flex')),
     dcc.Loading(
        id="loading-2",
        children=[
            dcc.Graph(
                id='map'
            ),
            dcc.Graph(id='bar_top_n')
        ],
        type="circle"
     )
]) 
```

下面是生成的布局截图。

![](img/243917335446fc96376687007e016925.png)

**图 2**：仪表板的截图

对于那些刚接触 Web 开发的人来说，Plotly 抽象了 HTML 和 CSS 的所有复杂性。以下是应用程序中的所有主要组件。

+   标题 1，即可视化的标题。

![](img/6e9a72c0af8806367699504f7dfce714.png)

可视化标题的截图

+   类名为`filters`的 HTML Div。这包括性别标题（`html.Div`）、以单选按钮形式呈现的性别选项（`dcc.RadioItems`）、Top N Names 标题（`html.Div`）、以单选按钮形式呈现的 Top N Names 选项（`dcc.RadioItems`）、选择姓名标题（`html.Div`）和选择姓名下拉框（`dcc.Dropdown`）

![](img/87cf842acc605b11af111fb65d25bbcb.png)

**图 3**：可视化的不同筛选器的截图。性别的单选按钮筛选器、Top N Names 的单选按钮筛选器和流行姓名的下拉筛选器

+   一个包含两个图形（`dcc.Graph`）的加载动画（`dcc.Loading`）：一个图形的 id 为 `map`（见**图 2**中的地图），另一个图形的 id 为 `bar_top_n`（见**图 2**中的条形图）。

![](img/32aa74c7107cc08e778a1cda5319ee58.png)

**图 4：** 可视化中的加载动画截图。

我对创建一个加载动画只需 4 行代码感到印象深刻。这种复杂性需要网站中 CSS、HTML 和/或 JavaScript 的组合。Plotly 抽象化了这些细节，使非技术用户也能轻松创建用户友好的体验。

此外，我喜欢 Plotly 保留了 HTML DOM 结构的同时命名元素。作为一名前网页开发人员，我由于 HTML 的熟悉感很快学会了 Plotly。此外，我喜欢他们利用 `children` 参数来以有序的方式重新排列元素。作为软件开发人员，编写可维护的代码比编写高效但难以阅读的代码要好。

你可能会注意到 `dcc` 元素中一些值如 `initial_rank` 或 `initial_sex`。这些是我在应用 HTML 布局之前创建的自定义变量。以下是这些变量。

```py
year_values = df["year"].unique()
rank_values = df["rank"].unique()

initial_rank = 5
initial_sex = "Male"
initial_year = 1910
initial_state = "AK"

ranks = []
for i in range(0, initial_rank):
    ranks.append(i+1)

names_in_initial_rank = df[(df["rank"].isin(ranks)) & (df["sex"] == initial_sex[0]) & (df["year"] == initial_year) & (df["state"] == initial_state)]["name"].unique()
```

这就是为什么你看到**图 2**中的条形图在加载时设置为默认标题：1910 年 AK 州的前 5 大受欢迎男性名字。

![](img/7e6043f482788b9810bdaa835ac18559.png)

**图 5：** 仪表板加载时的初始值条形图。

目标是让用户首次加载仪表板时填充默认选项。然后，用户可以通过点击进行导航。

现在我们有了 HTML 布局，是时候添加操作了。当我们点击下拉菜单、单选按钮或地图时会发生什么？我们使用 JavaScript 回调函数。

## 回调函数

JavaScript 回调是指在另一个函数完成执行后要执行的函数。JavaScript 是异步的，这允许它在应用程序运行时在后台运行函数。回调用于在某些事件（如鼠标点击或键入）之后执行逻辑。

Plotly 的回调函数模板如下所示：

```py
@callback {
   Output[],
   Inputs[]
}
def callback_function(Inputs[]) {
  return Outputs[]
} 
```

输入和输出接受两个参数：

+   div 的 id

+   从 div 接收的输入类型

从 HTML 布局中，我们有以下 div 的 id：

+   sex-radio-selection

+   rank-dropdown-selection

+   top-rank-name-dropdown-selection

+   loading-2

+   map

+   bar_top_n

输入类型可以是 `dcc` 元素（`dcc.Dropdown`、`dcc.RadioButton`）、`clickData`、`hoverData` 等。以下是可视化的回调函数。

```py
@callback(
    [
        Output('map', 'figure'),
        Output('bar_top_n', 'figure'),
        Output("top-rank-name-dropdown-selection", "options")
    ],
    [
        Input('sex-radio-selection', 'value'),
        Input('rank-dropdown-selection', 'value'),
        Input('top-rank-name-dropdown-selection', 'value'),
        Input('map', 'clickData')
    ]
)
def update_graphs(sex_value, rank_value, name_for_map_value, click_data):

    df_filter = df[df["sex"] == sex_value[0]]
    ranks = []
    for i in range(0, rank_value):
        ranks.append(i+1)

    state = initial_state
    year = initial_year
    if click_data:
        state = click_data['points'][0]['location']
        year = click_data['points'][0]['customdata'][0]

    top_rank_name_dropdown_options = df[(df["rank"].isin(ranks)) & (df["sex"] == sex_value[0]) & (df["state"] == state)]["name"].unique()

    if not name_for_map_value:
        name_for_map_value = top_rank_name_dropdown_options[0]

    RENAMED_COUNT_COLUMN = "Population with Name"
    df_filter = df_filter.rename(columns={"count": RENAMED_COUNT_COLUMN})

    df_filter = df_filter.sort_values(by=["year"])

    df_map_filter = df_filter[(df_filter["rank"].isin(ranks)) & (df_filter["name"] == name_for_map_value)]

    df_bar_filter = df_filter[(df_filter["state"] == state) & (df_filter["rank"].isin(ranks)) & (df_filter["year"] == year)]
    df_bar_filter = df_bar_filter.sort_values(by=["rank"])

    fig_map = px.choropleth(df_map_filter, geojson=us_states, 
                            locations='state', 
                            color=RENAMED_COUNT_COLUMN,
                            animation_frame='year',
                            animation_group='state',
                            hover_name="state",
                            custom_data='year',
                            color_continuous_scale="Reds",
                            range_color=(0, 1100),
                            scope="usa"
                          )

    fig_map.update_layout(margin={"r":0,"t":0,"l":0,"b":0})

    bar_chart_title = "Top %i Popular %s Names In State %s Of %i"%(rank_value, sex_value, state, year)

    fig_bar = px.bar(df_bar_filter, 
                     x="name", 
                     y=RENAMED_COUNT_COLUMN, 
                     title=bar_chart_title)
    fig_bar.update_traces(marker_color='#D62728')
    fig_bar.update_layout(margin={"r":50,"t":50,"l":0,"b":0})

    return [fig_map, fig_bar, top_rank_name_dropdown_options]
```

使用 plotly express 图形创建了 choropleth 地图和条形图，并作为输出的一部分返回。返回的输出顺序很重要，因为它应与回调模板中指定的输出顺序相对应。输入同样适用此规则。

choropleth 地图具有一个动画帧参数，该参数接受 'year'。这创建了按年显示的动画滑块，如**图 6**和**图 7**所示。

![](img/5e4c031725d713f5d8f02fef9190469c.png)

**图 6：** 1910 年 John 姓名人数的热图

![](img/f124b4edcb1f97c0b32722ef353fbe4a.png)

**图 7：** 将滑块移动到 1940 年后的 John 姓名人数热图

状态和年份变量设置为`inital_state`和`initial_year`，这些在上面已指定。然而，如果用户点击了某个特定的州和年份，我们希望更改这些变量。因此，if 语句检查`click_data`是否为真，指示`clickData`事件是否被触发。

如果`click_data`为真，我们将`state=click_data[points][0]['location']`和`year=clic_data['points'][0]['customdata']`。此外，我们将输入`click_data`定义为`Input("map", "clickData")`。这意味着我们在地图上发生点击时会做出响应，并根据地图的`location`和`customdata`属性更新状态和年份。这些是在创建热图时定义的参数（`locations=’state'`和`custom_data='year'`）。

![](img/055dbba79d7100d5b76da23ce7405def.png)

**图 8：** 热图和条形图，悬停在科罗拉多州时显示工具提示。工具提示显示当前年份、悬停时的州和 John 姓名人数。

![](img/64494862f1dc08ef0621afc1018e744d.png)

**图 9：** 点击**图 8**中的科罗拉多州后的加载旋转器。

![](img/defc1d42da670dcd937396a485566e90.png)

**图 10：** 更新的条形图，包含 1910 年科罗拉多州前 5 名流行男性姓名。这是在用户点击了**图 7**和**图 8**中的科罗拉多州后创建的。您可以看到科罗拉多州与**图 7**中的阿肯色州相比流行的名字有所不同（查尔斯和威廉在科罗拉多州流行，而保罗和罗伯特在阿肯色州流行）。

下拉框中的其他输入用于筛选初始加载的姓名数据框。下面是过滤器在回调函数修改后如何操作的。

# 性别筛选器

![](img/ff87a41c2e2f93f60c79a617d5ef806d.png)

**图 11：** 从**图 6**中点击“女性”单选按钮后的仪表板。热图为空，因为它需要将流行姓名筛选器（下拉框）中的姓名从男性姓名替换为女性姓名。默认姓名 John 不在女性姓名列表中，因此您需要从下拉框中选择一个女性姓名以填充地图。

![](img/c04aaa0bc5eb9a3d80eb866f45bfc0b9.png)

**图 12：** 从流行姓名下拉筛选器中选择 Helen 后的仪表板，显示为图 11。现在，条形图填充了 1910 年阿肯色州的前 5 名流行女性姓名，而不是男性姓名。地图上的灰色状态表示该年份和州没有人叫 Helen。

# 顶级排名筛选器

![](img/118303e6926f7b09e6a9c28c221c12c8.png)

**图 13：** 1967 年 John 的受欢迎程度地图可视化。1967 年德克萨斯州前 5 名流行男性姓名的条形图。

![](img/2f110e7bab58ab2ba8bb220f7097f8ad.png)

**图 14:** 点击“前 N 名”单选按钮 15 后的加载旋转图标，而不是 5

![](img/9d5061bd61331a16ee4904fa0d9136d5.png)

**图 15:** 与 **图 13** 相同的地图，只是条形图更新为 1967 年德克萨斯州的前 15 个名字。条形图还具有“查看更多”选项，以查看 1967 年德克萨斯州名为 William 的人数。

# 流行名字筛选器

![](img/74a2f15fae71472841146654193eda8e.png)

**图 16:** 仪表板显示性别为男性的前 15 个名字，约翰的流行度地图，以及 1910 年德克萨斯州前 15 个流行男性名字的条形图。流行度地图的下拉菜单显示了条形图中填充的前 15 个名字，当前悬停在 Willie 上。

![](img/c501a3b2ef1f22450a2eebe3e67b2257.png)

**图 17:** 点击“流行名字筛选器”中的 Willie 后，**图 16** 中的仪表板。地图更新了 Willie 在各州和年份的流行度。

# 将所有内容整合在一起

# 改善可视化的提示

## 使用 SQL 数据库和查询进行优化

你可能已经注意到，视频中加载旋转图标需要 4 秒以上来加载任何筛选器的更改数据，除了年份。这是因为 Plotly Dash 应用程序从 CSV 文件中读取了所有 150MB 的数据到内存中，并且在应用程序运行时查询选择数据。因此，处理数据框需要一些时间。

从数据库中读取数据并使用 SQL 查询选择数据要快得多。现在，你不需要将所有数据加载到内存中，只需一些部分。此外，SQL 数据库比 CSV 文件更紧凑，占用的内存更少。

## 使条形图更具动态性

可视化的核心组件是单个名字在地点和时间上的流行度地图。单一年份或时间的前 N 名条形图可能是多余的，因为它包含了与 top_rank_name_dropdown 筛选器相同的信息。

可以移除 top_rank_name_dropdown 筛选器，条形图可以突出当前地图上显示的名字。如果用户点击另一个名字的条形，地图会更新。如果用户点击地图上的一个州，条形图将更新为选定州的前 N 名名字和滑块最后设置的年份。

## 使条形图在地图上的年份滑块更新

地图可以通过年份滑块进行筛选，但条形图不能，这似乎有些混乱。如果用户点击地图上的一个州，条形图可以按州和年份更新。如果条形图来自与滑块上的地图年份不同的年份，这可能会造成混淆。

可能有办法为地图和条形图设置相同的年份筛选，但我尚未找到这种解决方案。

我最好的猜测是查看在移动年份滑块时是否触发了事件。然后在回调中，更新柱状图以反映当前年份。然而，目前的方法并没有做到这一点，因为我们在内存中加载了 150MB 的数据并在内存中进行过滤。因此，在移动滑块时会有巨大的延迟。

## 修订 Top N 过滤器

数据集中有些州和年份只有 8 个流行名字（如 1910 年的阿肯色州）。这对 Top N 过滤器的表现不佳，因为它即使在选择了 10 或 15 的单选按钮时也显示 8 个柱状图。或许在这种特殊情况下，需要隐藏这些按钮，并用 Top 8 单选按钮替代。

# 结论

本教程展示了如何使用 Plotly 创建复杂的动态可视化。虽然有改进可视化的方式，但这仍然显示了使用这个开源包的便捷。随着越来越多的技术工程师和数据/业务分析师转向数据科学，我们需要更好的工具来帮助他们以清晰的方式向高层传达数据。基于免费成本和较低的学习曲线，Plotly 成为有志数据科学家应当保留在工具库中的最佳包之一。

感谢阅读！如果你想阅读更多我的作品，请查看我的 [目录](https://hd2zm.medium.com/table-of-contents-read-this-first-a124146f566c)。

如果你不是 Medium 的付费会员，但有兴趣订阅以阅读像这样的教程和文章，[点击这里](https://hd2zm.medium.com/membership) 以加入会员。通过这个链接加入会员意味着我会因为推荐你而获得报酬。
