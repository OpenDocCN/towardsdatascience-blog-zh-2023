# 使用Python解决地理旅行推销员问题

> 原文：[https://towardsdatascience.com/solving-geographic-travelling-salesman-problems-using-python-e57284b14cd7?source=collection_archive---------1-----------------------#2023-07-12](https://towardsdatascience.com/solving-geographic-travelling-salesman-problems-using-python-e57284b14cd7?source=collection_archive---------1-----------------------#2023-07-12)

## 使用pyconcorde寻找现实世界路由问题的**最佳解决方案**

[](https://medium.com/@mikedbjones?source=post_page-----e57284b14cd7--------------------------------)[![Mike Jones](../Images/b841e6215298729d051a21fecbd83b12.png)](https://medium.com/@mikedbjones?source=post_page-----e57284b14cd7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e57284b14cd7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e57284b14cd7--------------------------------) [Mike Jones](https://medium.com/@mikedbjones?source=post_page-----e57284b14cd7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F253ada1cc4c9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-geographic-travelling-salesman-problems-using-python-e57284b14cd7&user=Mike+Jones&userId=253ada1cc4c9&source=post_page-253ada1cc4c9----e57284b14cd7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e57284b14cd7--------------------------------) ·12分钟阅读·2023年7月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe57284b14cd7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-geographic-travelling-salesman-problems-using-python-e57284b14cd7&user=Mike+Jones&userId=253ada1cc4c9&source=-----e57284b14cd7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe57284b14cd7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-geographic-travelling-salesman-problems-using-python-e57284b14cd7&source=-----e57284b14cd7---------------------bookmark_footer-----------)![](../Images/2e7334e2d22e1e5749ed0fa324793135.png)

一条**最佳的**汽车驾驶路线，连接79个英国城市。图像由作者提供。地图数据来自 [OpenStreetMap](https://www.openstreetmap.org/copyright)。

著名的[旅行推销员问题（TSP）](https://en.wikipedia.org/wiki/Travelling_salesman_problem)是关于在一组节点（城市）之间找到最佳路线并返回起点。这听起来很简单，但对于大量节点而言，通过蛮力求解是不可能的，因为`n`个城市的可能排序数量为`n!`。这意味着，即使是30个城市，你需要检查的旅行数量也是265,252,859,812,191,058,636,308,480,000,000。即使是强大的计算机也无法通过蛮力解决大型TSP问题。

![](../Images/ffc8bce187e3538ae1b404b70271bbd0.png)

随机生成的10节点TSP：需要检查3,628,800条可能的路线。图片由作者提供。

幸运的是，已经开发出一些算法，大大减少了解决大型TSP所需的计算量。几十年前开发的[Concorde](https://www.math.uwaterloo.ca/tsp/concorde/)软件在学术界得到广泛使用。虽然作为独立软件使用时相当技术性，并且仅面向专业人员，但[*pyconcorde*](https://github.com/jvkersch/pyconcorde)作为Concorde的Python包装器被开发出来。Concorde中使用的算法的解释超出了本文的范围。然而，我们将深入探讨在Python中重现这些问题及其解决方案所需的代码。

# 现实世界的地理TSP

如何解决实际的地理旅行推销员问题？现实世界的点不像上图那样通过简单的2D线连接。相反，地理特征通过各种可能的路线连接，而这些路线会根据是步行、骑自行车还是开车而变化。

数据科学家或软件工程师为何要解决实际的TSP？以下是一些用例示例：

1.  一家公司雇用快递员，需要一种计算城市中最佳路线的方法，以最小化每位司机的道路时间。

1.  旅游运营商需要在有限的时间内找到连接一组目的地的最短路线。

1.  废物处理公司或地方当局需要分配资源，以确保尽可能高效地安排取货。

为了解决实际的TSP，可以使用[*routingpy*](https://github.com/gis-ops/routingpy)库来查找路线、距离（以米为单位）和地理点之间的持续时间（以秒为单位），格式为`[经度, 纬度]`对。本文将描述可用于此类问题的方法。

# 编码讲解

这里概述了使用Python解决地理TSP的指南。问题解决过程的基本结构如下：

1.  获取一个包含*n*个坐标的列表，格式为`[经度, 纬度]`对。

1.  使用路由服务获取一个 (*n* x *n*) 矩阵，表示这些坐标之间的现实世界持续时间，适用于相应的配置文件（步行、骑行、驾车、驾驶重型货车等）。此矩阵将是非对称的（从 A 到 B 的驾驶时间并非 B 到 A 的完全反向）。

1.  将 (*n* x *n*) 矩阵转换为对称矩阵 (*2n* x *2n*)。

1.  将此矩阵输入 Concorde 求解器以找到坐标的最佳顺序。

1.  使用路由服务创建现实世界的路线。

1.  在地图上可视化结果。

1.  可选：创建最终路线的 GPX 文件。

每一步将详细讲解。

## 第 1 步：获取坐标

对于我们的示例，我们将考虑在英国的 79 个城市之间驾车旅行的问题。下图显示了蓝色的英国城市地图。数据科学家可以通过多种方式找到坐标。如果需要，可以使用 Google Maps 或 [Google Earth](https://earth.google.com/web/) 手动查找坐标。

![](../Images/1c577cb11b1cdb53cb3700979657130f.png)

英国的 79 个城市。图像由作者提供。地图数据来自 [OpenStreetMap](https://www.openstreetmap.org/copyright)。

**本示例中使用的代码结构和数据也可以在** [**此 GitHub 仓库**](https://github.com/mikedbjones/Geographic-TSP)**中找到。**

这里是一个包含城市坐标的 CSV 文件（*gb_cities.csv* 在仓库中），以及使用 pandas 导入它所需的代码。

```py
Place Name,Latitude,Longitude
Aberdeen,57.149651,-2.099075
Ayr,55.458565,-4.629179
Basildon,51.572376,0.470009
Bath,51.380001,-2.36
Bedford,52.136436,-0.460739
...
```

```py
import pandas as pd
df = pd.read_csv('gb_cities.csv')
coordinates = df[['Longitude', 'Latitude']].values
names = df['Place Name'].values
```

## 第 2 步：使用路由服务获取持续时间矩阵

routingpy 库提供了多种路由服务。来自 [Graphhopper](https://www.graphhopper.com/) 的 API 包含一个免费层，允许有限的使用。通过 routingpy 还可以使用其他路由器，详细信息见 [文档](https://routingpy.readthedocs.io/en/latest/)。

```py
import routingpy as rp
import numpy as np

api_key = # get a free key at https://www.graphhopper.com/
api = rp.Graphhopper(api_key=api_key)
matrix = api.matrix(locations=coordinates, profile='car')
durations = np.matrix(matrix.durations)
print(durations)
```

这是 `durations`，一个 79 x 79 的矩阵，表示坐标之间的驾驶时间（以秒为单位）：

```py
matrix([[    0, 10902, 30375, ..., 23380, 25233, 19845],
        [10901,     0, 23625, ..., 16458, 18312, 13095],
        [30329, 23543,     0, ...,  8835,  9441, 12260],
        ...,
        [23397, 16446,  9007, ...,     0,  2789,  7924],
        [25275, 18324,  9654, ...,  2857,     0,  9625],
        [19857, 13071, 12340, ...,  8002,  9632,     0]])
```

城市之间的驾驶时间可以通过以下方法确定：

1.  每一行和每一列对应一个城市：阿伯丁是第一行和第一列，艾尔是第二行，巴斯尔顿是第三行，依此类推。

1.  要找到阿伯丁和艾尔之间的时间，请查看第 1 行第 2 列：10,902 秒。反向时间（艾尔到阿伯丁）为 10,901 秒。

1.  一般来说，从第 i 个城市到第 j 个城市的时间位于第 i 行和第 j 列的交点处。

注意矩阵中对角线上的零，因为每个点与自身的距离或持续时间为零。此外，矩阵并非完全对称：城市之间的驾驶时间在两个方向上不太可能完全相同，原因是道路布局和交通热点不同。不过，它们大致相似，这也是预期中的情况。

## 第 3 步：将非对称矩阵转换为对称矩阵

在使用该矩阵生成pyconcorde中的最佳排序之前，我们需要使矩阵对称。有关将非对称TSP转化为对称TSP的方法，请参见[Jonker和Volgenant (1983)：将非对称问题转化为对称旅行商问题，《运筹学快报》，2(4)，161–163](https://www.sciencedirect.com/science/article/abs/pii/0167637783900482)。以下是这种转换的理论。如果需要，可以跳过本节（滚动到标题为*将地理非对称TSP转化为对称TSP*的部分）**。

**Jonker/Volgenant非对称到对称的转换**

以下是具有3个节点的非对称TSP的可视化及其距离矩阵。

![](../Images/6666027086298ed86cb89062ad636fb6.png)

具有3个节点的非对称TSP。图片由作者提供。

```py
matrix([[0, 5, 2],
        [7, 0, 4],
        [3, 4, 0]])
```

下面是将其转化为对称TSP的方法草图：

1.  创建新的*幽灵节点*，A'、B'和C'。将A连接到A'，B连接到B'，C连接到C'，距离为零。

1.  以如下方式连接节点并赋予权重：

    A到B现在由A'到B表示；B到A现在由B'到A表示。

    B到C现在由B'到C表示；C到B现在由C'到B表示。

    C到A现在由C'到A表示；A到C现在由A'到C表示。

1.  将所有其他边的权重设为无限，以便任何算法不会尝试在它们之间进行旅行。由于在使用pyconcorde时这将不切实际，因此将所有其他权重设为远高于我们已有的最高权重。在这种情况下，我们将其设置为99。

![](../Images/df27cbcf58566d17b4115a5d2fe2abcf.png)

等效的对称TSP，具有（3 x 2）节点。图片由作者提供。

这是生成的距离矩阵。矩阵中节点的顺序是：A、B、C、A'、B'、C'。

```py
matrix([[ 0, 99, 99,  0,  7,  3],
        [99,  0, 99,  5,  0,  4],
        [99, 99,  0,  2,  4,  0],
        [ 0,  5,  2,  0, 99, 99],
        [ 7,  0,  4, 99,  0, 99],
        [ 3,  4,  0, 99, 99,  0]])
```

再次注意对角线是零，这符合预期，并且矩阵现在是对称的。原始矩阵位于新矩阵的左下角，其转置矩阵位于右上角。与此同时，左上角和右下角部分包含节点之间非常高的权重。

A、B和C（左上角）不再彼此连接（严格来说，它们是连接的，但具有非常高的权重而不是无限权重，出于实际考虑）。这意味着任何算法都不会尝试在这些节点之间寻找路径。同样，A'、B'和C'（右下角）彼此也没有连接。相反，原始非对称网络的方向性在这里由原始节点A、B和C上的权重以及它们的幽灵节点A'、B'和C'表示。

原始非对称问题的解与新的对称TSP之间存在一一对应关系：

+   A — B — C — A对应于A — A' — B — B' — C — C' — A

+   A — C — B — A对应于A — A' — C — C' — B — B' — A

在每种情况下，幽灵节点A'、B'和C'与原始节点A、B和C交替出现，每个原始节点都与其“伙伴”幽灵节点相邻（A与A'相邻，依此类推）。

**将地理非对称TSP转化为对称TSP**

回到我们的实际示例。我们可以创建一个函数，将不对称 TSP 矩阵转换为对称矩阵：

```py
def symmetricize(m, high_int=None):

    # if high_int not provided, make it equal to 10 times the max value:
    if high_int is None:
        high_int = round(10*m.max())

    m_bar = m.copy()
    np.fill_diagonal(m_bar, 0)
    u = np.matrix(np.ones(m.shape) * high_int)
    np.fill_diagonal(u, 0)
    m_symm_top = np.concatenate((u, np.transpose(m_bar)), axis=1)
    m_symm_bottom = np.concatenate((m_bar, u), axis=1)
    m_symm = np.concatenate((m_symm_top, m_symm_bottom), axis=0)

    return m_symm.astype(int) # Concorde requires integer weights
```

`symmetricize(durations)` 返回：

```py
matrix([[     0, 461120, 461120, ...,  23397,  25275,  19857],
        [461120,      0, 461120, ...,  16446,  18324,  13071],
        [461120, 461120,      0, ...,   9007,   9654,  12340],
        ...,
        [ 23397,  16446,   9007, ...,      0, 461120, 461120],
        [ 25275,  18324,   9654, ..., 461120,      0, 461120],
        [ 19857,  13071,  12340, ..., 461120, 461120,      0]])
```

这个 158 x 158 的矩阵在左下角包含 `durations` 的副本，在右上角包含转置副本。高达 461,120 的值（是 `durations` 中最大值的 10 倍）意味着在实际应用中，具有这种时长的节点是不连接的。

这个矩阵最终可以输入到 pyconcorde 中，以计算最佳路径。

## 第 4 步：使用 Concorde 求解器

**安装 pyconcorde**

运行以下命令来安装 pyconcorde（目前在 Linux 或 Mac OS 上可用，但 Windows 上不可用）：

```py
virtualenv venv                                  # create virtual environment
source venv/bin/activate                         # activate it
git clone https://github.com/jvkersch/pyconcorde # clone git repo
cd pyconcorde                                    # change directory
pip install -e .                                 # install pyconcorde
```

**在 Python 中解决 TSP**

现在我们可以在 Python 脚本中从 `concorde` 导入。

```py
from concorde.problem import Problem
from concorde.concorde import Concorde

def solve_concorde(matrix):
    problem = Problem.from_matrix(matrix)
    solver = Concorde()
    solution = solver.solve(problem)
    print(f'Optimal tour: {solution.tour}')
    return solution
```

我们的对称时长矩阵可以输入到 `solve_concorde()` 中。

```py
durations_symm = symmetricize(durations)
solution = solve_concorde(durations_symm)
```

以下是打印输出：

```py
Optimal tour: [0, 79, 22, 101, 25, 104, 48, 127, 68, 147, 23, 102, 58, 137, 7, 86, 39, 118, 73, 152, 78, 157, 36, 115, 42, 121, 62, 141, 16, 95, 20, 99, 51, 130, 40, 119, 19, 98, 59, 138, 50, 129, 54, 133, 27, 106, 10, 89, 4, 83, 66, 145, 33, 112, 14, 93, 2, 81, 45, 124, 32, 111, 11, 90, 29, 108, 34, 113, 24, 103, 8, 87, 17, 96, 56, 135, 64, 143, 61, 140, 75, 154, 52, 131, 71, 150, 18, 97, 3, 82, 9, 88, 74, 153, 55, 134, 72, 151, 28, 107, 12, 91, 70, 149, 65, 144, 35, 114, 31, 110, 77, 156, 63, 142, 41, 120, 69, 148, 6, 85, 76, 155, 67, 146, 15, 94, 44, 123, 47, 126, 60, 139, 57, 136, 38, 117, 13, 92, 5, 84, 43, 122, 49, 128, 46, 125, 21, 100, 1, 80, 30, 109, 53, 132, 37, 116, 26, 105]
```

这个解决方案展示了最佳旅游路线中节点的排序。请注意，如上所述，该解决方案包含原始节点（编号为 0 到 78）和它们的虚拟节点（79 到 157）交替出现：

+   0 与 79 配对，

+   22 与 101 配对，

+   25 与 104，以此类推……

这表明解决方案已正确工作。

## 第 5 步：创建现实世界的路线

下一步是选择解决方案中的交替元素（对应于原始 79 个城市的节点），然后相应地排序坐标。

```py
# pick alternate elements: these correspond to the originals
tour = solution.tour[::2]

# order the original coordinates and names
coords_ordered = [coordinates[i].tolist() for i in tour]
names_ordered = [names[i] for i in tour]
```

这是 `names_ordered` 中的前几个城市名称，（最佳旅游路线中城市的真实排序）：

```py
['Aberdeen',
 'Dundee',
 'Edinburgh',
 'Newcastle Upon Tyne',
 'Sunderland',
 'Durham',
 ...]
```

现在我们重新加入第一个城市，以形成一个完整的循环路线，并最终使用 Graphhopper 方向 API 获取最终路线。

```py
# add back in the first for a complete loop
coords_ordered_return = coords_ordered + [coords_ordered[0]]

# obtain complete driving directions for the ordered loop
directions = api.directions(locations=coords_ordered_return, profile='car')
```

## 第 6 步：在地图上可视化

在地图上查看最终路线将使我们对结果充满信心，并允许我们在实际设置中使用解决方案。以下代码将显示一个可以保存为 HTML 的 folium 地图。

```py
import folium
def generate_map(coordinates, names, directions):

    # folium needs lat, long
    coordinates = [(y, x) for (x, y) in coordinates]
    route_points = [(y, x) for (x, y) in directions.geometry]
    lat_centre = np.mean([x for (x, y) in coordinates])
    lon_centre = np.mean([y for (x, y) in coordinates])
    centre = lat_centre, lon_centre

    m = folium.Map(location=centre, zoom_start=1, zoom_control=False)

    # plot the route line
    folium.PolyLine(route_points, color='red', weight=2).add_to(m)

    # plot each point with a hover tooltip  
    for i, (point, name) in enumerate(zip(coordinates, names)):
        folium.CircleMarker(location=point,
                      tooltip=f'{i}: {name}',
                      radius=2).add_to(m)

    custom_tile_layer = folium.TileLayer(
        tiles='http://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png',
        attr='CartoDB Positron',
        name='Positron',
        overlay=True,
        control=True,
        opacity=0.7  # Adjust opacity to control the level of greying out
    )

    custom_tile_layer.add_to(m)
    folium.LayerControl().add_to(m)

    sw = (np.min([x for (x, y) in coordinates]), np.min([y for (x, y) in coordinates]))
    ne = (np.max([x for (x, y) in coordinates]), np.max([y for (x, y) in coordinates]))
    m.fit_bounds([sw, ne])

    return m

generate_map(coords_ordered, names_ordered, directions).save('gb_cities.html')
```

结果显示在本文顶部。[点击这里查看互动地图](https://mikedbjones.github.io/Geographic-TSP/gb_cities.html)。可以放大地图以查看更多细节，并悬停在单个城市上以显示它们在旅游序列中的编号。下面是地图的一部分，显示了经过谢菲尔德的路线（在林肯和切斯特菲尔德之间）。

![](../Images/cc9e4fabdeb1f4050d04f4a4bf26bd86.png)

图片由作者提供。地图数据来源于 [OpenStreetMap](https://www.openstreetmap.org/copyright)。

## 第 7 步：可选：创建 GPX 文件

如果需要在现实生活中跟随计算出的路线，例如在具有 GPS 的设备上（如手机或车载导航系统），可以创建 GPX 文件。这不是优化问题的一部分，但如果你想将路线保存到文件中，这是一项可选的附加步骤。GPX 文件是从 `directions` 变量创建的：

```py
def generate_gpx_file(directions, filename):
    gpx_template = """<?xml version="1.0" encoding="UTF-8"?>
    <gpx version="1.1" xmlns="http://www.topografix.com/GPX/1/1"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.topografix.com/GPX/1/1
        http://www.topografix.com/GPX/1/1/gpx.xsd">
        <trk>
            <name>Track</name>
            <trkseg>{}</trkseg>
        </trk>
    </gpx>
    """

    trkseg_template = """
        <trkpt lat="{}" lon="{}"/>
    """

    trkseg_elements = ""
    for point in directions.geometry:
        trkseg_elements += trkseg_template.format(point[1], point[0])

    gpx_data = gpx_template.format(trkseg_elements)

    with open(filename, 'w') as file:
        file.write(gpx_data)

generate_gpx_file(directions, 'gb_cities.gpx')
```

这个问题的 GPX 文件可以 [在这里下载](https://github.com/mikedbjones/Geographic-TSP/blob/master/gb_cities.gpx)。

# 结论

我们已经看到如何结合以下元素来解决现实世界中的地理旅行推销员问题：

1.  从routingpy库获取的方向和持续时间矩阵，指定适当的`profile`（交通模式）。

1.  通过pyconcorde包装器使用高效而强大的Concorde求解器，以提供最佳路线。

1.  使用folium进行可视化，以创建地图。

上述的自驾游路线是对79城旅行推销员问题的一个令人信服的解决方案，根据Concorde求解器，它被证明是“最佳的”。然而，由于我们使用的是实际数据，最终结果的准确性取决于输入数据的质量。我们依赖于从routingpy获得的点对点时间矩阵能够代表实际情况。实际上，步行、骑车或驾车在不同时间段或一周中的不同日期之间所需的时间会有所不同。这是我们所使用的方法的一种限制。增强最终结果可信度的一种方式是使用[替代路线服务](https://routingpy.readthedocs.io/en/latest/#module-routingpy.routers)来尝试相同的方法。每个路线服务（如Graphhopper、ORS、Valhalla等）都有自己的API，可以用于解决像这里描述的TSP问题，并且可以比较不同服务的结果。

尽管解决这样一个问题存在现实世界的限制，上述方法为需要以尽可能高效方式在城市中移动的销售人员或快递员，或者希望在旅行中尽可能多地观光的游客提供了一个良好的起点。通过在互动地图上可视化结果并将路线存储为GPX文件，解决方案对最终用户非常有用，而不仅仅是实现代码的数据科学家。
