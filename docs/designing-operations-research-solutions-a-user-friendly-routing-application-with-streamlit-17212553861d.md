# 设计运筹学解决方案：一个用户友好的 Streamlit 路由应用

> 原文：[https://towardsdatascience.com/designing-operations-research-solutions-a-user-friendly-routing-application-with-streamlit-17212553861d?source=collection_archive---------2-----------------------#2023-09-30](https://towardsdatascience.com/designing-operations-research-solutions-a-user-friendly-routing-application-with-streamlit-17212553861d?source=collection_archive---------2-----------------------#2023-09-30)

## 从数学模型到 Python 软件工程

[](https://medium.com/@bruscalia12?source=post_page-----17212553861d--------------------------------)[![Bruno Scalia C. F. Leite](../Images/1042cd04be047c0811fef79ecd04e69c.png)](https://medium.com/@bruscalia12?source=post_page-----17212553861d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----17212553861d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----17212553861d--------------------------------) [Bruno Scalia C. F. Leite](https://medium.com/@bruscalia12?source=post_page-----17212553861d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3ce9b7482ef0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdesigning-operations-research-solutions-a-user-friendly-routing-application-with-streamlit-17212553861d&user=Bruno+Scalia+C.+F.+Leite&userId=3ce9b7482ef0&source=post_page-3ce9b7482ef0----17212553861d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----17212553861d--------------------------------) · 12分钟阅读 · 2023年9月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F17212553861d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdesigning-operations-research-solutions-a-user-friendly-routing-application-with-streamlit-17212553861d&user=Bruno+Scalia+C.+F.+Leite&userId=3ce9b7482ef0&source=-----17212553861d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F17212553861d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdesigning-operations-research-solutions-a-user-friendly-routing-application-with-streamlit-17212553861d&source=-----17212553861d---------------------bookmark_footer-----------)![](../Images/6f8bc76917461608408820ef00925fab.png)

照片由 [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在运筹学和数据科学中，弥合理论与应用之间的差距至关重要。虽然理论基础形成了优化解决方案的核心，因为它们提供了解决复杂问题的方法，但我们也应关注如何使这些概念变得可访问并且可用于实际应用。

旅行商问题（TSP）无疑是组合优化中研究最广泛的问题（Rego et al., 2011）。它的描述很简单（至少口头上），并且可以用来展示一些现代路由API的可能组件。因此，我实在无法想到一个更好的替代方案来在这个故事中使用。

在本教程中，你将学习如何使用Python库*Streamlit*构建一个基于用户提供输入数据来解决TSP的Web应用程序。由于我们关注实际应用，因此解决方案不仅限于欧几里得距离。它应该能够使用坐标提取实际的道路行驶距离，并将这些距离纳入优化过程。为此，将使用OpenStreetMap API。

如果你对深入了解数值优化的理论方面感兴趣，你可能会想查看我关于[*线性规划*](/linear-programming-theory-and-applications-c67600591612)和[*车辆路径问题*](https://medium.com/towards-data-science/the-vehicle-routing-problem-exact-and-heuristic-solutions-c411c0f4d734)（这是TSP的一个推广）的文章。

你准备好动手了吗？来看看我们的最终结果吧……

![](../Images/32127326f034bc751079e6edc3782952.png)

最终应用程序的屏幕截图。（动画由作者制作）。

由于整个代码可能过长，未能在此故事中全部包含，部分代码已引用但省略。不过，你可以在[这个库](https://github.com/bruscalia/tsp-app/tree/main)中查看完整代码。

# 旅行商问题

TSP的目标是找到连接*N*个点的最短路径。随着考虑的点数增加，由于问题的组合性质，找到一个最佳或近似最佳的解决方案可能变得非常具有挑战性。

![](../Images/4c59efcdeda1e2ef5f4f28f99e63ac43.png)

使用自定义启发式算法获得的1000个地点的TSP优质解决方案。（图片由作者提供）。

从精确方法到启发式和元启发式，解决TSP的技术必须考虑元素之间的成对距离以获得解决方案。虽然在这篇文章中，我不会深入讨论解决方法的细节，但可以考虑一个通用函数来解决TSP，其签名如下：

```py
solve_tsp(distances: np.ndarray) -> List[int]
```

你可以在原始[代码库](https://github.com/bruscalia/tsp-app)中找到一些解决方案策略的实现方法。然而，在这里，我们将重点讨论如何将类似于`solve_tsp`的组件视为软件开发中的一个组成部分。

# 库文件夹结构

在本教程中，将采用一个简单的扁平文件夹结构，其中包含以下元素：

+   *.streamlit*: 这个文件夹是存放一般 Streamlit 设置的地方。

+   *data*: 存储在开发过程中使用的数据集的文件夹，可能包含常规示例。

+   *assets*: 这个文件夹存储在主应用脚本中引用的元素，如图像、徽标等。

+   *optimizer*: 应用程序中使用的内部 Python 包。

+   *app.py*: 主要的 Streamlit 脚本。

+   *Dockerfile*: 构建应用程序 Docker 镜像的说明。

+   *requirements.txt*: 运行应用程序所需的 Python 依赖项。

+   *README.md*: 项目描述。

+   *.dockerignore* 和 *.gitignore*: 顾名思义，分别是 Docker 构建和 Git 忽略的文件。

```py
tsp-app/
│
├── .streamlit/                # General streamlit configuration
│   └── config.toml
│
├── data/                      # Directory for data if any
│   ├── dataset.csv
│   └── ...
│
├── assets/                    # These will be referenced in the app
│   ├── dataset.csv
│   └── ...
│
├── optimizer/                 # TSP optimizer internal package
│   ├── __init__.py
│   └── ...
│
├── app.py                     # Main Streamlit script
│
├── Dockerfile                 # Docker configuration file
├── .dockerignore
│
├── .gitignore                 # File ignored in Git
├── requirements.txt           # Python dependencies
└── README.md                  # Project description
```

更复杂的应用程序可能会为 Streamlit 界面的实用函数创建一个单独的包。

```py
...
├── optimizer/                 # TSP optimizer internal package
│   ├── __init__.py
│   └── ...
│
├── interface/                 # Utility functions for the interface
│   ├── __init__.py
│   └── ...
│
├── app.py                     # Main Streamlit script
...
```

只需小心并确保在项目中定义路径时，以从运行脚本或启动应用程序的目录的相对位置为准。

首先，确保你在 Python 环境中安装了所有的依赖项。

```py
pip install -r requirements.txt
```

在 TSP 应用程序的情况下，我们可能会遇到一些依赖冲突，因此建议你单独使用 `--no-deps` 选项安装 ortools。

```py
pip install --no-deps ortools==9.7.2996
```

使用所提议的扁平结构，你可以通过运行以下命令行从目录的根目录运行应用程序：

```py
streamlit run app.py
```

或者，你可以使用以下命令，利用我的代码库中提供的 [Dockerfile](https://github.com/bruscalia/tsp-app/blob/main/Dockerfile) 来构建一个 docker 镜像。

```py
docker build -t tsp_app:latest .
```

你可以将 `tsp_app` 替换为你想要的任何仓库名称，将 `latest` 替换为任何版本。

然后你可以通过运行以下命令来执行你的应用程序

```py
docker run -p 8501:8501 tsp_app:latest
```

让我们更好地理解如何使用 `app.py` 文件。

# 一般结构

`app.py` 脚本的一般结构可以通过以下伪代码总结：

```py
Imports

Definition of utility functions (could have been in a separate file)
Declaring constants
Declaring default session_state attributes
Parsing solver parameters
Parsing input file

if input is not None:
    read (and cache) input data

    if execute button is pressed:
        solve model based on input
        store solution as a session_state attribute

if solution is not None:
    make output available
    plot results
```

尽管看起来很简单，但这个伪代码的一些点是关键的：

当按下一个 *button* 时，Streamlit 运行与之相关的代码并重置其状态。因此，请遵循 *Streamlit* 指南，并避免将这些元素保留在按钮的条件内部：

+   显示的项目应在用户继续操作时保持不变。

+   其他会导致脚本重新运行的控件。

+   不修改会话状态或写入文件/数据库的进程。

这就是为什么我们将尽可能多地探索 `session_state` 属性。优化完成后，即使我们按下下载按钮或更改一些配置而不按“执行”按钮，我们也希望继续探索地图。因此，显示解决方案的条件仅限于其在 session_state 中的存在，而不是“执行”按钮。

此外，一些操作可能会很昂贵，缓存其结果以避免浪费时间进行重新运行是有用的。记住：*Streamlit 每次用户交互或代码更改时都会从上到下运行你的脚本*。

# 基本配置

如前所述，我们可以在`.streamlit`文件夹中的`config.toml`文件中定义一般的Streamlit设置。在TSP示例中，我使用它来定义颜色和字体。这是一种浅紫色布局，但你可以尝试不同的颜色。只需确保在[Hex颜色代码](https://htmlcolorcodes.com/)中定义它们。

```py
[theme]
primaryColor = "#5300A5"
backgroundColor = "#403E43"
secondaryBackgroundColor = "#1C1B1E"
textColor = "#F5F3F7"
font = "sans serif"
base = "dark"
```

让我们开始用Python导入填充`app.py`的内容。将使用*os*包来管理我们操作系统中的文件路径；*BytesIO*将用于模拟一个保存在内存中的可下载输出文件，而不是磁盘；*json*将用于序列化我们的输出解决方案；*List*只是一个类型提示。

处理数据框时，将使用*pandas*；求解器*Highs*从*pyomo*导入以解决我们的任务（如果是MIP）；*streamlit*是界面的基础；*streamlit_folium*将用于将*folium*地图插入到应用程序中。还导入了在内部包中定义的附加自定义函数。

```py
import os
from io import BytesIO
import json
from typing import List

import pandas as pd
from pyomo.contrib.appsi.solvers.highs import Highs
import streamlit as st
from streamlit_folium import st_folium

from optimize.tsp import get_distance_xy, build_mip, solve_mip, TSP, plot_tour, request_matrix,\
    plot_map, get_coord_path
```

`session_state`属性将存储当前解决方案的*tour*、给定输入文件的*pandas*数据框、该解决方案的值以及路线坐标路径（从OpenStreetMap获取）。

```py
# Create current solution as session_state
if "tour" not in st.session_state:
    st.session_state.tour = None

if "dataframe" not in st.session_state:
    st.session_state.dataframe = None

if "sol" not in st.session_state:
    st.session_state.sol = None

if "route_path" not in st.session_state:
    st.session_state.route_path = None
```

使用的一些实用函数包括：

+   driving_distances: 从OpenStreetMap获取距离矩阵，提供一个数据框（在此处缓存结果非常重要）。

+   upload_callback: 在加载新输入文件时重置`session_state`属性。

+   update_path: 给定一个旅行路线，获取相应的驾驶路径的地图坐标以绘制结果。

```py
# Finds distance matrix
@st.cache
def driving_distances(dataframe: pd.DataFrame):
    return request_matrix(dataframe)["distances"] / 1000

# Callback uploading a new file
def upload_callback():
    st.session_state.tour = None
    st.session_state.sol = None
    st.session_state.route_path = None

# Update route path after solution
def update_path(tour: List[int], dataframe: pd.DataFrame):
    coord_rt = dataframe.loc[tour, :]
    path = get_coord_path(coord_rt)
    st.session_state.route_path = path
```

然后让我们配置页面布局。在以下脚本中，我们为网页包含一个图标和一个标题；然后我们在侧边栏中包含相同的图标，并用Markdown样式写一个介绍。我建议你运行`streamlit run app.py`并检查到目前为止的结果。

```py
# Path to icon
icon_path = os.path.join("assets", "icon_tsp.png")

# Set the page config to wide mode
st.set_page_config(
    page_title="TSP",
    page_icon=icon_path,
    layout="wide",
)

st.sidebar.image(icon_path)

st.title("TSP")
st.write("Welcome to the Traveling Salesman Problem solver.")
```

我定义的一个实用函数读取`README.md`文件中的内容，并在用户选择此选项时显示这些内容。由于我希望将显示内容的条件保持独立于可能的重新运行，因此我使用了*selectbox*而不是*button*来实现。

```py
display_tutorial = st.checkbox("Display tutorial")
if display_tutorial:
    section = st.selectbox("Choose a section", ["Execution", "Solutions", "Contact"], index=1)
    tutorial = read_section(section)
    st.markdown(tutorial)
```

然后让我们定义求解器参数并上传一个输入文件……

# 输入数据和求解器参数

在这个示例中，我包括了两种输入类型的选项：

+   ‘xy’: 使用欧几里得距离，输入数据必须具有‘x’和‘y’列。

+   ‘lat-long’: 使用道路驾驶距离，输入数据必须具有‘lat’和‘long’列。

我还包括了两个求解器选项：

+   ‘MIP’: 使用*pyomo*创建的模型，并用HiGHS求解。

+   ‘Heuristic’: 使用Google OR-Tools路由算法，基于构造性 + 局部搜索启发式与多次启动。

这些参数以及求解时间限制将通过以下代码放置在应用程序侧边栏中：

```py
problem_type = st.sidebar.selectbox("Choose an input type:", ["xy", "lat-long"], index=0)
method = st.sidebar.selectbox("Choose a strategy:", ["MIP", "Heuristic"], index=0)
time_limit = st.sidebar.number_input("Time limit", min_value=0, value=5, step=1)
```

为了上传文件，将使用`file_uploader`函数。注意，我们用于重置`session_state`属性的函数会在输入更改时被调用。

```py
file = st.file_uploader("Upload input file", type=["csv"], on_change=upload_callback)
```

如果输入文件不为空（None），我们应该读取它并准备距离矩阵以待优化…

```py
if file is not None:
    dataframe = pd.read_csv(file)
    st.session_state.dataframe = dataframe
    distances = FORMATS[problem_type](dataframe)
    start_button = st.button("Optimize")
```

注意，之前`FORMATS`被定义为一个字典，字典中的函数返回一个给定数据框和问题类型的距离矩阵。这些函数在内部模块中定义并导入到应用程序中。

```py
FORMATS = {
    "xy": get_distance_xy,
    "lat-long": driving_distances
}
```

# 执行

现在执行依赖于点击“执行”按钮。这是一个每次点击一次的过程。根据这个条件，优化器将被执行，结果将存储在`session_state`属性中。

```py
# Run if start is pressed
if file is not None and start_button:

    # Solve MIP
    if method == "MIP":
        solver = Highs()
        solver.highs_options = {"time_limit": time_limit}
        model = build_mip(distances)
        tour = solve_mip(model, solver)
        st.session_state.tour = tour

    # Solve Heuristic
    elif method == "Heuristic":
        model = TSP(distances)
        tour = model.solve(time_limit=time_limit)
        st.session_state.tour = tour

    # Display the results
    sol = model.obj()
    st.session_state.sol = sol

    # Update path in case of lat-long
    if problem_type == "lat-long":
        update_path(tour, dataframe)
```

你可以在这些函数调用中包含额外的小部件。例如，一个*spinner*或*progress bar*可能在执行过程中看起来不错。

然后，在按钮的执行条件之外，我们可以根据其存在性来提供我们的输出。

```py
# Compute current state variables
sol = st.session_state.sol
tour = st.session_state.tour
dataframe = st.session_state.dataframe

if sol is not None and tour is not None:
    col_left, col_right = st.columns(2)  # Divide space into two columns
    col_left.write(f"Current solution: {sol:.3f}")
    col_right.download_button(
        label="Download Output",
        data=json.dumps(tour),
        file_name='output.json',
        mime='json',
    )
```

在这个简单的例子中，我们提供了一个包含旅游过程中访问点序列的*json*文件。如果我们需要生成更复杂的输出，如Excel文件，*BytesIO*可能是一个有趣的替代方案。假设你想创建一个下载按钮，使得一个* pandas* 数据框字典可用。你可以使用如下内容：

```py
buffer = BytesIO()
with pd.ExcelWriter(buffer) as writer:
    for name, df in dataframes.items():
        df.to_excel(writer, sheet_name=name)

st.download_button(
    label="Download Output",
    data=buffer,
    file_name='output.xlsx',
    mime='xlsx',
)
```

# 使用OpenStreetMap的详细路线

在应用程序的早期，我们使用了在内部包中定义的`request_matrix`函数，通过OpenStreetMap API和*Python*库*requests*来获取问题的距离矩阵。在这个函数中，后台执行的请求类似于：

```py
f"https://router.project-osrm.org/table/v1/driving/{points}?sources={sources_str}&destinations={dest_str}&annotations={annotations}"
```

其中：

+   points：一个字符串，连接*longitude*和*latitude*的对，用“，”分隔同一对的坐标，用“;”分隔不同的对。

+   sources：一个整数列表，对应于来源（from）节点。

+   destinations：类似于sources，但包含目的地（to）节点。

+   annotations：‘distance’，‘duration’或‘duration,distance’。

现在我们想获取解决方案路线的详细坐标以可视化结果。为此，我们将使用不同的请求。考虑两个函数，它们接收作为输入的已排序的旅游元素数据框。通过API获取的输出，我们将创建一个包含*latitude*、*longitude*的元组列表，并将其传递给*folium*以创建地图。

```py
def get_request_points(coordinates: pd.DataFrame) -> List[str]:
    return coordinates.apply(lambda x: f"{x['long']},{x['lat']}", axis=1).to_list()

def get_coord_path(coordinates: pd.DataFrame) -> List[Tuple[float, float]]:
    pts = get_request_points(coordinates)
    pts_req = ";".join(pts)
    r = session.get(
        f"http://router.project-osrm.org/route/v1/car/{pts_req}?overview=false&steps=true"
    )
    result = r.json()
    first_route = result["routes"][0]
    coords = [(p["location"][1], p["location"][0])
              for l in first_route["legs"]
              for s in l["steps"]
              for p in s["intersections"]]
    return coords
```

# 使用Folium绘制结果

现在考虑我们有通过函数`get_coord_path`获得的元组列表。我们现在必须使用它来创建我们的*folium*地图。

```py
def plot_map(
    path: List[Tuple[float, float]],
    color: Union[str, tuple] = "darkblue",
    zoom_start=8,
    **kwargs
):
    # Coordinates from path
    lat, long = zip(*path)

    # Create map
    m = folium.Map(zoom_start=zoom_start)
    new_line = folium.PolyLine(
        path, weight=4, opacity=1.0,
        color=color, tooltip=f"Route",
        **kwargs
    )
    new_line.add_to(m)

    # Trim zoom
    sw = [min(lat), min(long)]
    ne = [max(lat), max(long)]

    m.fit_bounds([sw, ne])
    return m
```

最后，我们可以将这个地图与*streamlit_folium*库结合起来，创建一个令人惊叹的结果可视化。

```py
if tour is not None and dataframe is not None:

    # Plot solution
    if problem_type == "xy":
        pass  # Not really in the app, but skipping here

    elif problem_type == "lat-long":
        map = plot_map(st.session_state.route_path)
        st_folium(map, width=700, height=500, returned_objects=[])
```

使用我在[代码仓库](https://github.com/bruscalia/tsp-app)中提供的输入文件，你可以找到并可视化经过所有美国大陆州首府的最短驾驶路线。

![](../Images/fc16f439b8452dc192af6ad452477f04.png)

在所有大陆美国州首府之间的最短驾驶路线。（作者提供的图像）。

# 部署

到目前为止，我们的解决方案在本地执行时运行顺利。然而，你可能希望将应用程序分享给更广泛的受众。这时部署变得至关重要。你可以访问已部署的 TSP 应用程序 [这里](https://tsp-app-p7zrnpfj5q-uc.a.run.app)。

对于这个项目，我选择使用 [Google Cloud Run](https://cloud.google.com/run) 部署应用程序，因为我的想法是部署一个容器化应用程序（使用 Docker），并且我对该环境比较熟悉。类似的简洁而有用的教程可以在这个 [其他故事](https://medium.com/google-cloud/deploying-containers-to-cloud-run-in-5mins-b03f1d8d4a64) 中找到。Streamlit 还提供了一个关于这个主题的快速指南 “[如何使用 Docker 部署 Streamlit](https://docs.streamlit.io/knowledge-base/tutorials/deploy/docker)”。

对于 TSP 应用程序，我在 Dockerfile 中解决了 `ortools` 和 `streamlit` 之间的依赖冲突。其他部署方法严重依赖 `requirements.txt` 文件，这可能使得这种特定情况变得更加困难。然而，值得检查这些“[3 种简单的方法来在线部署你的 Streamlit Web 应用](/3-easy-ways-to-deploy-your-streamlit-web-app-online-7c88bb1024b1)”。

祝你在部署你自己的运筹学解决方案的旅程中一切顺利！

# 进一步阅读

与我之前的 Medium 故事不同，这篇文章强调了优化和运筹学作为软件开发中的组成部分，探讨了它们如何与其他工具集成，以创建简单而有影响力的应用程序。对于那些渴望深入了解数值优化机制的人，我推荐探索我关于它的综合列表，你可以在其中找到几个经典问题、建模策略、求解器的使用和理论方面的内容。

![布鲁诺·斯卡利亚 C. F. 莱特](../Images/0c7396e41d4b598be2349eaea982c984.png)

[布鲁诺·斯卡利亚 C. F. 莱特](https://medium.com/@bruscalia12?source=post_page-----17212553861d--------------------------------)

## 优化时代的故事

[查看列表](https://medium.com/@bruscalia12/list/tales-of-the-optimization-age-c15faf64a6ca?source=post_page-----17212553861d--------------------------------)15 个故事![](../Images/848ca03a7d7366b8a040f720f5d51f5c.png)![](../Images/b79fd62ce301f6295199d983f7633588.png)![](../Images/a6e8cbe0e088f4e7b1edcf27c524b072.png)

# 结论

在这个故事中，探索了优化与软件开发的结合，使用了简单而有效的网页开发框架*Streamlit*和开源路由 API *OpenStreetMap*。通过将运筹学的核心概念与网页应用的易用性相结合，可以创建出引导决策过程的惊人工具。

# 参考文献

Rego, C., Gamboa, D., Glover, F., & Osterman, C., 2011\. 旅行商问题启发式算法：主要方法、实现和最新进展。*欧洲运筹学杂志*，*211*(3)，427–441。
