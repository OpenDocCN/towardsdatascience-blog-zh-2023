# 《LiDAR 城市模型的 3D Python 工作流程：一步步指南》

> 原文：[`towardsdatascience.com/3d-python-workflows-for-lidar-point-clouds-100ff40e4ff0`](https://towardsdatascience.com/3d-python-workflows-for-lidar-point-clouds-100ff40e4ff0)

## 3D Python

[](https://medium.com/@florentpoux?source=post_page-----100ff40e4ff0--------------------------------)![Florent Poux, Ph.D.](https://medium.com/@florentpoux?source=post_page-----100ff40e4ff0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----100ff40e4ff0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----100ff40e4ff0--------------------------------) [Florent Poux, Ph.D.](https://medium.com/@florentpoux?source=post_page-----100ff40e4ff0--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----100ff40e4ff0--------------------------------) ·38 分钟阅读·2023 年 4 月 4 日

--

解锁 3D 城市建模应用的精简工作流程的终极指南。教程涵盖了结合 3D 点云、网格和体素的 Python 自动化，以进行高级分析。

![](img/97c090b15454c5c08f7efcab12fb9808.png)

《LiDAR 城市模型的 3D Python 工作流程：一步步指南》。封面来自我的另一半[Marina](https://www.instagram.com/mimatelier_/)，展示了 3D 城市建模的艺术过程。© [Mimatelier](https://mimatelier.com/)。

你之前遇到过“智能城市”这个词吗？或者“智能某物”？好吧，我们会涉及这个话题！将智能城市想象成一个超能的面包师 🥐：它知道你需要什么，甚至在你提出之前就会提供最直接的建议，帮助你做出美味的选择。不，这个智能城市的比喻并不是我今天唯一要分享的内容。确实，要达到这种“智能”的水平，我们首先得从基础层面入手：3D 城市模型。

如果你曾经想要创建令人惊叹的 3D 城市模型，但发现工作流程令人生畏且复杂，那么我可以帮忙！本文探讨了如何利用 Python 和开源软件定义一个强大的 3D 工作流程，以启动你的 3D 城市建模之旅。向繁琐的手动流程说（几乎）再见，迎接高效、动态且引人注目的创作吧！

我们深入探讨了一个四步策略，描述了环境设置、3D 数据策划与准备以及 3D 几何处理，以提取关键洞察，例如使用点云数据、网格、体素以及一些灰质 🧠，了解你所在社区的建筑覆盖情况。

![](img/dd6cd4c7638e5fd0967612930ab5e1a1.png)

《LiDAR 城市模型的 3D Python 工作流程：一步步指南》。© 作者

如果你已经准备好并充满热情，现在是时候开始 3D 编程了！

🎵**致读者的说明**：这本实践指南是* [***UTWENTE***](https://www.itc.nl/) *与我亲爱的同事们* [***Dr. Sander Oude Elberink***](https://people.utwente.nl/s.j.oudeelberink)*,* [***Dr. Mila Koeva***](https://people.utwente.nl/m.n.koeva)*,* [***Dr. Ville Lehtola***](https://people.utwente.nl/v.v.lehtola)*,* [***Dr. Pirouz Nourian***](https://people.utwente.nl/p.nourian)*,* [***Dr. Paulo Raposo***](https://people.utwente.nl/p.raposo)*. 和* [***Prof G. Vosselman***](https://research.utwente.nl/en/persons/george-vosselman)* 的共同工作之一。我们感谢来自数字双胞胎* [*@ITC*](http://twitter.com/ITC) *项目的资金支持，该项目由特温特大学 ITC 学院授予。*

![](img/a969888432c000fbe6ebde8acd430ae3.png)

我们将在本指南中处理的 3D Python 数据集的摘录。© 作者

# 引言

在进入有趣的部分之前，让我讲一个小故事，为我们将要实现的目标提供一些背景。这始于一个基本的问题：什么是 3D 城市建模，它为何有用？

在一个城市化的世界里，3D 城市建模对于高效管理我们的日常生活至关重要。通过准确地以三维方式呈现我们的城市，我们可以分析和可视化复杂的城市环境，理解提议更改的影响，并做出明智的决策，以改善居民的生活质量。这是一个基本的概念，由那句优美的话很好地表达：城市塑造生活¹。

![](img/8cfddea6a8bbd75cb37a16e5edb3493c.png)

朝向智能城市以改善我们的生活。© 作者

的确，城市是人们生活、形成社区并建立自己身份的地方。它们是如市中心和郊区这样的空间，提供了一种配置和塑造物质世界和自然环境的方式。

想象一下，如果你有一种超级能力，可以对你的城市交通网络进行建模，预测交通模式并识别拥堵区域。这将如何改变你在城市中的生活方式？这只是作为城市“用户”的一个微小例子。但从根本上讲，3D 城市建模为城市规划师、建筑师和政策制定者提供了宝贵的见解，以优化城市基础设施、减少交通拥堵并提高公共安全。

通过创建我们城市的数字化复制品，我们可以更好地规划未来，为子孙后代创造可持续、宜居的社区。²⁻³

> ¹ Chen, X., Orum, A. M., & Paulsen, K. E. (2018). *《城市导论：地方和空间如何塑造人类体验》*。约翰·威利父子公司。
> 
> ² Lehtola, V. V., Koeva, M., Elberink, S. O., Raposo, P., Virtanen, J. P., Vahdatikhaki, F., & Borsci, S. (2022). *《城市数字双胞胎：服务城市需求的技术综述》*。《国际应用地球观察与地理信息杂志》，102915。
> 
> ³ Nourian, P., Ohori, K. A., & Martinez-Ortiz, C. (2018). 城市计算的基本手段：基于网络的城市规划计算平台的规范，旅行者指南。城市规划，3(1)，47–57

# 3D 城市建模：工作流程

是时候认真起来，定义一个一致的 3D 工作流程，我们可以将其作为不同 3D 城市建模操作的灵感。我们的目标是（1）易于设置和运行，（2）为各种场景提供极大的灵活性，以及（3）足够强大以涵盖多模态应用的复杂性。

🦚 **注意**：*不，多模态不是脏话：它只是触及到当处理 3D 城市模型时，我们遇到各种需要考虑的地理空间数据模态。在本教程中，我们将专注于一个美丽的细分领域：* ***点云****，* ***体素****，以及* ***3D 网格****。

如果我们从高层次定义工作流程，我们会遵循四个主要步骤，如下所示。

![](img/35a2f7dccc19d471620463aa5284aeb7.png)

在城市模型背景下的 3D Python LiDAR 工作流程。我们从环境设置（步骤 1）和 3D 数据准备（步骤 2）开始。一旦完成这些步骤，我们进入 Python 自动化（步骤 3），其中一个特定部分处理 3D Python 挑战（步骤 4），例如地块表面或兴趣点查询。

激动了吗？仔细查看流程，你会发现我们从零开始。这允许将提出的结构适应各种场景，这些场景需要不同的环境、数据集或自动化工具。

现在让我们更深入一点。假设我们在荷兰拥有一所房子（可以靠近特文特大学），我们想更好地了解周围的区域。这是我们的起点。现在，为了更好地理解我们房子与周围环境的关系，出现了一些问题：邻里建筑区的密度有多高？房子是否可能面临洪水？我是否遵守了我所拥有地块的建筑比例？

我们应该如何解决这些问题？我们是否应该上网查找？是否应该打开地图？是否应该联系测绘服务？让我们一起探索这个过程，同时在这个背景下解锁一套新的强大技能。

在接下来的内容中，我们详细介绍了回答这些问题的四个步骤：第一步是理想的环境设置。其次，我们获取 3D 点云和作为感兴趣区域网格的城市模型。然后，我们创建一个高度关注自动化的 3D Python 笔记本。最后，我们创建一套 Python 函数以应对具有挑战性的场景。好了，让我们拿起咖啡或茶🍵，深入寻找这些问题的答案吧！

🦚 **注意**：*我设计了下一系列操作，使其易于线性跟随，无需编码技能或可用数据。然而，如果你是经验丰富的编码人员，我提供了有用的优化技巧，以确保代码性能达到巅峰！*

# 第一步：3D 环境设置

在开始动手编写代码和进行 3D 思考之前，一个好的做法是确保我们在适当的环境中工作。我们不会在肮脏的台面上用钝刀和过期的食材做饭（或者至少我们会避免 😁）！让我们遵循下面所示的三个子步骤。

![](img/d2e39d1775e3819afe4a4b05635da305.png)

第一步：3D 环境设置。

## 1\. 1\. 软件堆栈

为了开始，让我们首先安装一个 3D 点云和网格处理软件：CloudCompare。它是一个出色的工具，允许有效地处理点云数据的科学分析（但不限于此）。它是任何迭代实验中的一个重要部分，例如，当你想快速获得关于理论可行性的数据驱动想法时。

要获取 CloudCompare 软件，你可以前往 [CloudCompare.org](https://cloudcompare.org/) 的下载部分，获取适合你操作系统的最新稳定版，如下所示。

![](img/22b4cf7d7ef80b3b341e8aaeebaee4aa.png)

从 [`cloudcompare.org/`](https://cloudcompare.org/) 下载 CloudCompare

下载软件后，按照线性安装步骤操作，直到获得可以打开的工作软件，启动时界面应类似于以下 GUI：

![](img/78d2d6aca43e0ca4a55ee2ca7be1797e.png)

CloudCompare GUI。

然后，我们希望获得一个允许我们专注于实际代码的 Python 发行版。它被称为 Anaconda，可以在各种平台上从 [Anaconda.com](https://www.anaconda.com/) 获得。下载与您的操作系统匹配的软件后，您可以进行安装。提供了一个 GUI（Anaconda Navigator），您可以启动它以快速开始，如下所示。

![](img/0de35f78f0fcbacebf630f4eb0510aea.png)

这是 Anaconda Navigator GUI，允许你管理未来实验的独立 Python 环境。

使用 GUI Anaconda Navigator，转到左侧的“`环境`”标签。然后，我们通过点击“`创建`”来创建一个全新的隔离 Python 环境，如下所示。

![](img/af87f918dfe6a994ed3653fc37485d0d.png)

在 Anaconda Navigator 中，你的左侧有四个标签。在“首页”标签中，你会发现 IDE 处于特定环境中；在“环境”标签中，你可以创建、管理和选择任何环境。

这将使我们能够更好地管理一些我们想要安装的 Python “库”（第 1.3 步），同时避免版本冲突。

🦚 **注意**：*对于本教程，我们选择了 Python 版本 3.9.16，如上所示。创建环境后，点击它，你会看到其名称旁边有一个“播放”图标。这将开启直接在所选环境中启动* ***Anaconda Terminal*** *的可能性。***这是在此环境中安装库或 IDE 的首选方式****。*

如果你已经有一个工作中的 Anaconda Navigator 和一个新创建的环境，我们就可以选择一种编码方式，旨在比文本编辑器更高效。😁

## 1.2\. Python IDE

Python IDE（集成开发环境）是一种软件应用程序，提供了一整套用于开发、测试和调试 Python 代码的工具。它们提供了一个一体化的环境，我们可以在其中编写、编辑和执行代码，管理项目文件，跟踪更改，并与其他开发人员协作。一些流行的桌面 Python IDE 包括 PyCharm、Visual Studio Code 和 Spyder，每个 IDE 都提供了适应不同编程需求的独特功能和能力。

今天，我想重点介绍一个出色的“基于网络”的 IDE：JupyterLab。JupyterLab 的主要优点之一是其笔记本界面，它提供了一个可视化的、互动的编程环境。它使得实时创建、编辑和运行代码单元变得简单，而无需在不同窗口或应用程序之间切换。此外，JupyterLab 支持广泛的数据可视化工具，是处理任何地理数据科学或 3D 机器学习项目的绝佳选择。

![](img/11bbac3dd2b3dbf60e7b3f3a15fd2f2f.png)

JupyterLab IDE 用于 3D Python。

JupyterLab 直观的界面、强大的功能集以及对多种编程语言的支持，使其成为各个技能水平的 Python 开发人员的热门选择。

🦚 **注意**：*JupyterLab IDE 还支持多种编程语言，包括 Python、R 和 Julia，允许在一个环境中使用各种工具和库。这非常酷，因为 R 和 Julia 是很棒的语言。*

在能够使用 JupyterLab 之前，我们需要在当前 Anaconda 环境中安装它。如前所述，我们必须在所选环境中打开 Anaconda Terminal（在我们的例子中是 ITC）。要通过 Anaconda Navigator 实现这一点，从你选择的环境中，（1）点击绿色箭头，（2）选择 *“*`Open Terminal`*”*，如下所示。*

![](img/3ab8549967e0a486f8d32c8a1b6afef4.png)

如何在当前环境中安装依赖。

在打开的控制台（Anaconda Terminal）中，输入以下命令：`conda install -c conda-forge jupyterlab`，然后按“`Enter`”。这将直接安装 JupyterLab。

![](img/302aec290a29f80cbf63ec1609280c43.png)

命令行在 Anaconda Terminal 中执行（我们看到我们在（ITC）环境中）。

请注意，它可能会要求你确认安装一些需要的库，你需要通过输入`y`并按下“`Enter`”键来接受。

![](img/aecfb0e6f732cc0b0b1d8d0c1c85d1b9.png)

经过 30 多秒后，安装需要我们的确认。输入‘`y`’并按`Enter`键。这将下载、解压并安装上述提到的库。

一旦过程完成，你就有一个安装在 ITC Conda 环境中的 JupyterLab IDE。不要关闭控制台；从那里，我们将通过四个简单的步骤测试一切是否顺利。

1.  **启动 JupyterLab**：在相同的控制台中，你可以通过输入命令：“`jupyter lab`”来启动 JupyterLab。这将自动在默认网页浏览器中打开 JupyterLab 界面。

1.  **创建一个新的笔记本**：要创建一个新的笔记本（我们在其中编写代码），请点击 JupyterLab 界面左上角的“`文件`”菜单，并选择“`新建笔记本`”。这将会在新标签页中打开一个新的笔记本。

1.  **编写代码**：现在你可以在笔记本中开始编写代码。要创建一个新的代码单元格，请点击工具栏中的“`+`”按钮或按“`Ctrl + Shift + Enter`”。然后你可以在单元格中编写 Python 代码（例如：`‘This is working’`），并通过按“`Shift + Enter`”运行它。

1.  **保存你的工作**：记得定期保存你的工作，通过点击工具栏中的“`保存`”按钮或使用“`Ctrl + S`”键盘快捷键来保存。

这些是开始使用 JupyterLab 的基本步骤。随着你对界面的熟悉，你可以探索更多高级功能，如添加 Markdown 文本、导入和导出数据以及使用 JupyterLab 扩展。

🦚 **注意**：*对于 ITC—特温特大学的学生，我们很幸运能使用* [***CRIB: A Geospatial Computing Platform***](https://crib.utwente.nl/) *进行 Jupyter Lab。 如果你的计算机在跟随本课程时显示出某些处理限制，我强烈推荐使用这个云计算服务。*

## 1.3\. 3D Python 库

在本教程中，我将介绍五个对 3D 地理空间分析至关重要的库。这些库是`NumPy`、`Pandas`、`Open3D`、`Matplotlib`和`Shapely`。

🦚 **注意**：*如果你想使用上述库，我们必须确保它们在你的环境中已安装并可用。因此，在相同的环境终端中，我们使用公式* “`pip install package-name==version`” *(其中`==version`是可选的，用于固定某些版本)，如下面所示：*

![](img/9265d7a29ec28a42dda3e94ef33194e0.png)

```py
#We install the 5 libraries with the package manager 'pip', one line at a time
pip install numpy
pip install pandas
pip install open3d==0.16.0
pip install matplotlib
pip install shapely
```

**NumPy**：这个库用于处理数组和矩阵。它提供了对大型多维数组的快速高效操作，使其成为科学计算和数据分析的强大工具。一个使用`NumPy`的实际示例是创建一个点云，将其作为 3D 欧几里得空间中的数据点集合。为此，你可以创建一个具有三列的 NumPy 数组，每一行表示点云中的一个点：

```py
import numpy as np
point_cloud = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

#To print the "point_cloud" variable as [Out] from the cell
print(point_cloud) 
```

在这个示例中，点云包含三个坐标为`(1, 2, 3)`、`(4, 5, 6)`和`(7, 8, 9)`的点。简单吧，你说？😁

**Pandas**：这个库更侧重于数据操作和分析。它提供了强大的数据结构和工具，用于处理结构化数据，例如 CSV 文件、电子表格和数据库。虽然它并不是专门为 3D 数据处理设计的，但仍可以用来以表格格式读写点云。为此，你可以创建一个 DataFrame 对象，其中列表示每个点的 X、Y 和 Z 坐标：

```py
import pandas as pd

# create a DataFrame with X, Y, and Z columns
points_df = pd.DataFrame({ 'X': [1, 4, 7], 'Y': [2, 5, 8], 'Z': [3, 6, 9]})

# save the DataFrame as a CSV file at the same place as your script
points_df.to_csv('point_cloud.csv', index=False)
```

在这个示例中，点云包含三个坐标为`(1, 2, 3)`、`(4, 5, 6)`和`(7, 8, 9)`的点，如使用 NumPy 获得的。然后，DataFrame 使用 `to_csv` 函数保存到 CSV 文件中。

🦚 **注意**：*Pandas 可以通过另一个 Python 模块进行扩展：* [***Geopandas***](https://geopandas.org/)*。这个库使得可以直接处理存储在例如 Shapefiles 或 PostGIS 数据库中的空间数据。这扩展了当前教程的范围，但了解这些内容是有益的，因为我们在其他情况下肯定会用到它。* 😉

**Open3D**：这个库专注于 3D 数据处理和可视化。它提供了各种工具和函数，用于处理点云、网格和其他 3D 数据格式，如体素。

🦚 **注意**：*快速安装该库的方法是运行 Anaconda 环境终端并输入以下命令：* `pip install open3d==0.16.0` *，这将安装版本 0.16.0 的* [*Open3D*](http://www.open3d.org/)*，如本教程中使用的版本。*

现在，在 Jupyter Lab IDE 中，让我们使用内置的 Open3D 函数创建一个点云：

```py
import open3d as o3d

# create an empty point cloud object
point_cloud = o3d.geometry.PointCloud()

# add points to the point cloud
points = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
point_cloud.points = o3d.utility.Vector3dVector(points)
```

在这个示例中，点云包含三个具有坐标`(1, 2, 3)`、`(4, 5, 6)`和`(7, 8, 9)`的点。

🦚 **注意**：*在上面的代码块中，我们创建了一个 Open3D PointCloud 对象，然后通过* `o3d.utility.Vector3dVector` *函数将点列表传递给 points 属性，该函数将点列表转换为可以添加到点云对象中的格式。*

**Matplotlib**：这个库用于数据可视化。它提供了许多工具，用于创建高质量的图表、图形和其他可视化内容。虽然它并不是专门为 3D 数据可视化设计的，但它仍然可以创建点云的 3D 散点图。为此，你可以使用 Axes3D 类创建 3D 图，并使用 scatter 函数绘制点：

```py
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import numpy as np

# create a point cloud with 1000 random points
points = np.random.rand(1000, 3)

# create a 3D plot and add the points as a scatter plot
fig = plt.figure()
ax = fig.add_subplot(111, projection=’3d’)
ax.scatter(points[:,0], points[:,1], points[:,2])

# show the plot
plt.show()
```

在这个示例中，使用 NumPy 生成了一个包含 1000 个随机点的点云。然后，使用 `scatter` 函数在 3D 图中绘制这些点。最后，使用 `show` 函数显示图形。

**Shapely**：这个库主要用于 2D 几何操作，用于创建、操作和分析几何对象。它提供了广泛的工具来处理点、线、多边形和其他几何形状。`Shapely`的一个常见用例是创建一个多边形，并检查一个点是否在多边形内。为此，你可以使用定义多边形的坐标列表创建一个 Polygon 对象，然后使用`contains`函数检查点是否在多边形内：

```py
from shapely.geometry import Point, Polygon

# create a polygon with four vertices
polygon = Polygon([(0, 0), (0, 1), (1, 1), (1, 0)])

# create a point
point = Point(0.5, 0.5)

# check if the point is within the polygon
if polygon.contains(point):
 print(‘The point is within the polygon.’)
else:
 print(‘The point is outside the polygon.’)
```

这只是一个简单的例子，但 Shapely 提供了许多其他用于处理几何对象的函数，这些函数对于更复杂的应用可能会很有用。

🦚 **注意**：*在这个例子中，使用一个坐标列表创建了一个具有四个顶点的多边形。然后，使用* `Point` *函数创建一个点。最后，使用* `contains` *函数检查点是否在多边形内，并根据检查结果将消息打印到控制台。*

在这个 3D 城市建模的工作流程中，我们的第一步是巧妙地结合简单而有效且经过验证的工具和库，下面做了视觉总结。

![](img/36edff1699f49afb2c4cc4a1515dcaeb.png)

环境设置的总结。© 作者

现在我们有了明确的软件栈、功能齐全的 Python IDE 和对 Python 库的基本理解，我们可以开始准备数据。

# 第 2 步：3D 数据准备

我们现在进入第二步：3D 数据准备。

![](img/eef72b8db48508cfa76b3ed2f1ecd575.png)

3D 数据准备。我们将下载 3D 数据集，然后开发一些简单的 3D 可视化，通过下采样和导出数据为适合 Python 使用的格式来进行处理。

我们的目标是收集和准备数据集，以便我们用 Python 进行分析。因此，这一步也充当了“3D 数据可视化”阶段，我们将定性评估我们所处理的数据。如果你准备好了，就开始吧。

## 2.1. 下载 3D 数据集

对于 3D 城市模型分析，我们从开放数据源中收集了一些优秀的数据集。我展示了荷兰一个特定的兴趣区域，但我鼓励你研究你自己的房子或任何对你感兴趣的地点（如果你住在荷兰的话，当然，😉）。

**点云数据集**。首先，我们使用[geotiles.nl](https://geotiles.nl/)门户网站收集点云数据集，该网站提供了一些在 CC-BY 4.0 许可下的良好数据集。为此，你可以访问该网站，缩放到感兴趣的瓦片，并获取最新版本的 AHN LiDAR 点云（在我们的案例中是 AHN4），如下图所示。文件将被下载为.laz（LasZip）文件。

![](img/f230047cf879386afe3e52e933855c1c.png)

从 AHN-X 项目下载 3D LiDAR 数据集，通过门户网站 geotiles.nl。

🦚 **注意**：*AHN（Actueel Hoogtebestand Nederland）源自航拍 LiDAR 覆盖，为荷兰所有地区提供数字高程地图。它包含详细且精确的高度数据，每平方米平均有八次测量。水务局、省份和公共工程部门等组织使用 AHN 进行水务和堤坝管理。根据地面高度和高程，决定水是否能从土地上顺利排出，沟渠中的水位可以多高，河流、洪水平原和沟渠中的水是否能得到充分排放，以及堤坝是否仍然足够高且强大。AHN 还用于许多其他类型的管理，如日常管理和堤坝维护、重大维护的规格准备、3D 绘图、许可和执法。市政府、企业和研究人员也使用详细的高程数据。*

如果你喜欢图形，我为你准备了 AHN LiDAR 数据的快速概述：

+   AHN1: 1997–2004, 1 pt/16 m2 至 1 pt/m2

+   AHN2: 2007–2012, 8 pts/m2

+   AHN3: 2014–2019, 8 pts/m2

+   AHN4: 2020–2022, 10–15 pts/m2，

+   AHN5: 2023–2025, 10–15 pts/m2

**网格数据集**。

对于这一部分，我们想在与我们下载的 LiDAR 区域相同的位置找到一个 3D 网格。因此，我们使用由 TUDelft 创建的平台 [3DBAG](http://3dbag.nl)，它允许我们从 [CityGML Specification](https://www.ogc.org/standard/citygml/) 中检索荷兰的 10M 建筑物，具有 LoD1.2、LoD1.3 和 LoD2.2。在这一步，我们主要关注几何形状。虽然我们知道语义和拓扑是 CityGML / City JSON 数据模型的重要方面，但将在后续文章中探讨。😉

![](img/50f588623ef6881757261e292df79872.png)

从门户网站 3Dbag.nl 下载数据集。

🦚 **注意**：*3DBAG 包含多个细节层级的 3D 模型，这些模型是通过结合两个可用数据集生成的：来自* [*BAG*](https://docs.3dbag.nl/en/overview/sources/#BAG) *的建筑数据和来自* [*AHN*](https://docs.3dbag.nl/en/overview/sources/#AHN)*的高度数据。3D BAG 会定期更新，保持最新的开放建筑库存和高程信息*。

下载数据集后，你应该会有一个.laz 文件格式的点云和一个或多个.obj 数据集（以及其附带的.mtl 文件），这些数据集大致描述了相同的范围，但具有不同的细节层级（在我们的例子中是 LoD 1.2、1.3 和 2.2），如下面所示。

![](img/50511df16a6b00c5995b8594721a4391.png)

🧙‍♂️ **向导**： [可选] *如果你想深入了解 3D 数据文件格式，尤其是点云网格的部分，我建议你跟随下面的教程，详细了解如何将点云网格化及其结构。*

[](/5-step-guide-to-generate-3d-meshes-from-point-clouds-with-python-36bad397d8ba?source=post_page-----100ff40e4ff0--------------------------------) ## 5 步指南：使用 Python 从点云生成 3D 网格

### 教程：使用 python 自动从 3D 点云生成 3D 网格（.obj, .ply, .stl, .gltf）。(附赠)…

towardsdatascience.com

为了方便起见，你可以直接从这个 [**Drive 文件夹**](https://drive.google.com/drive/folders/1CFX42bIcl3Y4NBa6qhrtmYZ-IX3o7q4r?usp=sharing) 下载选择项。

## 2.2\. 3D 数据可视化

现在我们将进入`CloudCompare`以确保下载的数据分析没有特殊的惊喜。😁 启动`CloudCompare`后，你可以从本地文件夹加载`.laz`点云。当导入窗口显示时，确保仅导入此应用程序的`“Classification”`额外字段，并接受如下面所示的全局偏移。

![](img/135545f5da43ce67aede78208a48476c.png)

在 CloudCompare 中进行 3D 数据可视化。

🦚 **注意**：*全局偏移是一个临时的偏移，以允许`*CloudCompare*`处理地理参考数据，该数据超出了它可以处理的可视化范围。因此，这一步是透明的*，并将在保存数据时应用*。*

现在我们可以继续导入网格数据。为此，我们执行相同的导入操作，并接受建议的平移偏移，同时确保它与应用于点云的偏移相同。

![](img/1f493402266cfe74dbbebb61afaf2d50.png)

从下载的资产中加载 3D 网格。

现在你可以从数据库树中看到当前项目中导入的不同对象（如果找不到 DBTree，可以查看下图）。`DBTree`的功能有点类似于你的操作系统资源管理器，其中包含不同的点云或网格。每个对象（例如点云或 3D 网格）可以像图层一样可视化地激活 ☑️（或停用）并选择以识别对象属性。

![](img/5835e687656ac5c5a90468fe7fa06d51.png)

CloudCompare 3D 数据处理和分析界面。© 作者

🦚 **注意**：*CloudCompare 默认不保存。为了避免崩溃，如果你担心，可以将所有数据和分析放在 DBTree 中的一个文件夹中（右键点击 > 创建新的空组），并将此文件夹保存为.bin CloudCompare 项目。*

## 2.3\. 3D 数据子采样

现在是深入 3D 数据过滤的时候了。首先，我们从`DBTree`中选择点云，并应用空间子采样函数以每 50 厘米保留一个点，这样可以保留足够的信息，同时不会影响计算速度。为此，我们使用下面所示的`subsample`函数。

![](img/75c60243e4fdfd456fd59b6de354358d.png)

3D 点云子采样

🧙‍♂️ **向导**: *[可选] 在我们的案例中，我们使用了空间子采样函数，每隔 50 cm 平均保留一个点。如果你想探索和深入 3D 点云采样策略，我推荐深入阅读以下文章。*

[](/how-to-automate-lidar-point-cloud-processing-with-python-a027454a536c?source=post_page-----100ff40e4ff0--------------------------------) ## 如何使用 Python 自动化 LiDAR 点云处理

### 关于点云子采样的终极指南，从零开始，用 Python 编写。涵盖 LiDAR I/O、3D 体素网格处理等……

towardsdatascience.com

一旦完成子采样步骤，我们将在 `DBTree` 中得到一个结果子采样点云，可以用于后续处理。

在 Mesh 方面，导入后，我们可以通过点击每个网格对象旁边的小箭头图标从 DBTree 中打开它，这会显示许多不同的子网格元素。这是因为在原始的 `.obj` 文件中，我们有一个“对象”细化，这允许我们保留一些与几何体相关的“语义”分解。每个子网格是一个构建实体的分解。

如果你想查看这些元素，可以打开网格，通过按住 `Ctrl+Shift` 并在第二次鼠标点击时选择所有子元素，然后 `右键点击 > 切换` 以显示它们。你还可以取消选中父网格元素的 `可见` 属性，以确保你看到的仅是子元素，如下所示。

![](img/694d934e30ea666f3dfb863b4b8a4544.png)

CloudCompare 中的 3D 数据准备。

🦚 **注意**: *我们不能使用子网格选择，因为它们也包含网格父项的所有顶点，这意味着，如果我们使用它，我们将不得不将网格分割为仅选中的子网格的顶点。*

很棒，干得好！从那里，你可以取消选择所有元素，只保留你想要考虑的网格（在我们的例子中是 LoD 1.2），并使用例如“横截面”功能，选择你感兴趣的房屋，如下所示。

![](img/f552098ff1c35a90d330c244b378b1d8.png)

CloudCompare 中的 3D 数据选择

一旦完成，我们就准备好导出我们选择的研究站点的两种 3D 模态。

## 2.4\. 3D 数据导出

关于点云数据，我们可以以各种文件格式导出它。由于我们希望保留分类字段以进行一些 Python 分析（例如，了解一个点是否属于地面或建筑物），我们将 3D 点云导出为 ASCII 文件，以便在 Python 中轻松处理，如下所示。

![](img/e375bc8da30926779b138a3c60a9416c.png)

CloudCompare 中的 3D 点云数据导出。

🦚 **注意**：*在保存文件时，我们可以选择修改文件扩展名为* `*.xyz*` *，并在导出对话框打开时勾选“保留列名”选项。这将允许在文件中写入列名。然而，如果你用文本编辑器打开文件，确保删除两个不必要的反斜杠，如上所示。*

我们的第一个 3D 数据集已经准备好，可以作为`.xyz`文件用于 Python。现在，让我们继续进行 3D 网格处理。

选择你感兴趣的房屋/建筑块后，我建议你获取相关的子网格元素名称，并将其用作导出文件的名称。关于导出对话框的选项，你可以选择`.obj`文件扩展名，这是一个安全的选择😉。

![](img/ac17be9b0af97e470aaa69ecb8fa42ff.png)

CloudCompare 中的 3D 网格导出。

3D 网格现在已经准备好，并附带一个材料`.mtl`文件（在我们的案例中不太有用）。

步骤 2 已完成！干得好。我们首先收集了荷兰一个区域的 3D 点云和 3D 网格。然后我们进行了可视化，以检查它们是否符合我们的意图。接着我们过滤了点云，保留每 50 厘米左右一个点，3D 网格只保留一个代表建筑房屋的对象，并将两种数据分别导出为`.xyz`和`.obj` + `.mtl`文件。¹

![](img/4595e2ca0af6a47c7bf76134ee9cf658.png)

步骤 2 的可视化总结：3D 数据准备。© F. Poux

现在让我们将这些数据集放入一个出色的 Python 设置中，以最大化自动化和 3D 分析！

> ¹为了方便，你可以在这个[**Drive 文件夹**](https://drive.google.com/drive/folders/1ANqPB5t_pw_n26QkZggbt5JMI0cZ-cO_?usp=sharing)中找到这些数据集。

# 第 3 步\. Python 自动化

现在真正有趣的部分开始了，是时候用 Python 编程了！🤓

![](img/a314de57983d17100cf5259317f63320.png)

3D Python 自动化。

如上所示，我们将遵循五个阶段的方法，包括导入库、加载数据集、设置我们的 3D Python 可视化工具、定义 3D 挑战的解决方案，然后将结果导出以供 Python 之外使用。让我们先在 Python 中建立我们自动化管道的主体，然后再处理不同的挑战。

## 3.1\. 导入库

如步骤 1 中定义的，我们将坚持使用最少的库，以加深我们对其使用的了解。这些库包括`NumPy`、`Pandas`、`Open3D`、`Matplotlib`和`Shapely`。

我们将从我们的 IDE 中在 Python 笔记本（`.ipynb`）中编写以下代码行。

![](img/b8a497ba7aed223b175d69426e78d072.png)

用于编写我们脚本的 IDE 视图。

我们用以下代码块导入上述提到的库：

```py
import numpy as np
import pandas as pd
import open3d as o3d
import matplotlib.pyplot as plt
from shapely.geometry import Polygon

print(f"Open 3D Version: {o3d.__version__}")
```

这将返回当前的 Open 3D 版本作为字符串：`Open 3D Version: 0.16.0`。我们已经设置好了，可以继续加载数据集。

## 3.2\. 加载数据集

现在我们将定义存储数据集的具体路径。我喜欢将其明确且相对于我的代码文件。这样，一切都相对表达（`../` 表示转到父文件夹），使得在不同机器上编码时，浏览文件夹变得容易。

```py
data_folder="../DATA/"
pc_dataset="30HZ1_18_sampled.xyz"
mesh_dataset="NL.IMBAG.Pand.0637100000139735.obj"
result_folder="../DATA/RESULTS/"
```

**点云数据集**。

我们可以通过首先创建一个名为 `pcd_df` 的 Pandas `DataFrame` 对象来准备点云，该对象将包含点云数据：

```py
pcd_df= pd.read_csv(data_folder+pc_dataset, delimiter=";")
print(pcd_df.columns)
```

这将返回 pcd_df 数据框中的列名，它们是：`[‘X’, ‘Y’, ‘Z’, ‘R’, ‘G’, ‘B’, ‘Classification’]`。这对于仅选择明确的“列”而不是模糊的索引非常有用 😁。这正是我们要做的：仅选择 `[‘X’, ‘Y’, ‘Z’]` 坐标以创建 Open3D `PointCloud` 对象。这是一个很好的方法来理解当我们使用不同的库时，我们必须适应不同的机制以将数据集转换为不同的 Python 对象。在这里，我们从 Pandas DataFrame 转到 Open3D PointCloud，这是使用 Open3D 库中实现的 Open3D 函数所必需的。让我们按以下四个步骤进行：

![](img/7e414dcc4e8af9947fabce7223ab2bb6.png)

Open3D 点云创建的概念工作流程。© 作者。

这个分解机制转化为以下内容：

![](img/8c0ddc98b107fe8264d4b46bc61da2e7.png)

创建 Open3D 点云的代码工作流程。© 作者。

不过，如果我们想要更简洁一点，这个转换可以通过一行代码完成，即：

```py
pcd_o3d=o3d.geometry.PointCloud(o3d.utility.Vector3dVector(np.array(pcd_df[['X','Y','Z']])))
```

我们现在有两个重要的变量：`pcd_o3d` 和 `pcd_df`。我们还可以为 Open3D `PointCloud` 对象添加一些颜色：

```py
pcd_o3d.colors = o3d.utility.Vector3dVector( np.array( pcd_df[[‘R’,’G’,’B’]]) / 255 )
```

我们必须注意将 `R,G,B` 值转换为 `[0,1]` 范围内的浮点值，并确保将 `Vector3dVector` 对象传递给 `colors` 属性。

**网格数据集**。

现在，我们可以使用 `open3d` 的 `read_triangle_mesh()` 方法将 3D 网格加载到 `mesh` 变量中。我们还可以使用 `paint_uniform_color()` 方法为网格上色：

```py
mesh=o3d.io.read_triangle_mesh(data_folder+mesh_dataset)
mesh.paint_uniform_color([0.9,0.9,0.9])
```

我们现在有两个 Open3D 对象：一个包含 7 103 848 个点的 APointCloud 和一个包含 674 个点和 488 个三角形的三角网格。让我们看看这意味着什么，好吗？

## 3.3\. Python 3D 可视化

要在 Open3D 中可视化不同的 3D 对象，我们必须传递一个包含这些 Open3D 对象的 **python 列表**。因此，我们的列表由一个 Open3D `PointCloud` 和一个 Open3D `TriangleMesh` 组成，即 `[pcd_o3d, mesh]`。让我们使用以下代码在独立窗口中可视化这个组合：

```py
o3d.visualization.draw_geometries([pcd_o3d,mesh])
```

🦚 **注意**：*上面的代码行将创建一个交互式的 Open3D 窗口，结合了 3D 点云和 3D 网格。*

要调整显示的颜色，有一个实用的技巧：使用一个颜色变量，该变量将传递给 PointCloud Open3D 对象的颜色属性。此变量应包含从 `0` 到 `1` 的 R、G、B 浮点值。

![](img/18551016caa1227467965bcf70bb8738.png)![](img/3cff291b433da91c893721c7d6b934e7.png)![](img/e5a4d747c0c69f956eeb40c7e9454e05.png)![](img/e12a344c10402c875344a31f1a3f84a7.png)

🦚 **注意**：*你是否注意到两个截图之间的细微差别？右侧我们可以更好地勾勒出边界，并且有更强的深度感（这在交互式操作中更为明显）。这是因为在第二种情况下，我们还使用了法线进行可视化。要做到这一点，你可以在运行可视化部分之前运行* `pcd_o3d.estimate_normals()` *和* `mesh.compute_vertex_normals()` *。享受吧* 😉

假设我们想要根据分类属性可视化点云。我们需要遵循以下四阶段过程：

![](img/3f448bdbc8c3545587c308f4e2c961e2.png)

使用分类作为 Open3D 颜色的概念工作流程。

如果你仔细观察，这将转换为如下代码：

![](img/3fa8055f2ac49a7fb61ac506f2745632.png)

使用分类作为 Open3D 颜色的代码工作流程。

```py
# 1=unclassified, 2=Ground, 6=building, 9=water, 26=rest
pcd_df['Classification'].unique()
colors=np.zeros((len(pcd_df), 3))
colors[pcd_df['Classification'] == 2] = [0,0,0]
colors[pcd_df['Classification'] == 6] = [1,0,0]
colors[pcd_df['Classification'] == 9] = [0,0,0]
colors[pcd_df['Classification'] == 26] = [0,0,0]
pcd_o3d.colors = o3d.utility.Vector3dVector(colors)
```

🦚 **注意**：*因为我们希望对每个类别的颜色进行控制，我们可以根据唯一类别的数量调整上述代码。然后，映射是基于* [*LAS (1.4) 规范*](https://www.asprs.org/wp-content/uploads/2010/12/LAS_Specification.pdf)*进行的。*

![](img/4cb0fbe0068e65ff012e850ddb032d3d.png)

LAS LiDAR 数据规格文件格式的 ASPRS 标准点类别。

我们可以可视化结果，类似于这样（根据输入的`R,G,B`值显示不同的颜色）：

![](img/445de1239d521084b27fe5e0e32d8e8d.png)

Open3D 交互式多模式数据可视化。© 作者。

我们已经加载了变量，可以看到点云和 3D 网格，一切看起来运行顺畅！现在是定义我们想要进行的各种 3D 城市分析任务的时刻！

## 3.4. 3D 城市分析

3D 城市分析指的是使用城市环境的三维（3D）模型来分析和理解建筑环境的各种方面，如建筑能源性能、城市形态和行人流动。这种分析通常涉及使用专业范式和进行分析，以获得有关城市规划和设计的见解。3D 城市分析可供城市规划师、建筑师和工程师使用，以提供有关基础设施布置、公共空间设计和各种环境影响缓解的决策信息。在本教程中，我们将涉及 3D 城市分析的三个主要方面：

1. 城市形态分析：我们可以使用 Python 分析 3D 城市数据集中建筑物的形状和形式，这可以帮助提供有关城市设计和规划的决策。

2\. 地理空间分析：我们可以对城市模型进行地理空间分析，例如根据可达性和环境影响等因素确定新基础设施项目的最佳位置。

3\. 3D 可视化：我们可以创建交互式 3D 可视化，这可以帮助利益相关者更好地理解和参与城市规划和设计项目。

因此，这一步专门用于一个应用程序，我们在这里进行大部分分析。我们在 3D 城市分析中进行演示，并在第四部分通过各种 Python 挑战进行扩展，如下所示。

![](img/bed78e8f4aea28f4d8fcfef513f4121b.png)

城市背景下的 3D 分析及其在第 4 步中的分解：3D Python 挑战（第四部分）。

不过，我们可以遵循一个通用的工作流程，适应于每个应用程序，通常由输入、处理管道和每个特定分析部分的输出组成。

![](img/1e05945cdf9c74f35899860f253e8508.png)

从输入到输出的概念性工作流程，适用于点云数据。

输入可以有所不同，但在大多数情况下，它是一个包含空间信息的 NumPy 数组。输出可以是一个整数、一个列表、另一个 NumPy 数组……你需要的任何东西。😁

🦚 **注意**：*这个特定阶段比较复杂，我们将在笔记本中留出一些空间，用于记录与第 4 步挑战相关的不同分析。现在，让我们定义一些方法来导出我们的数据。*

## 3.5\. 3D 多模态导出

在我们的 3D 管道完全功能后，我们可以将结果保存到一个或多个文件中，以便在 Python 之外处理。为此，我们将使用两种有价值的可能性。

1.  **Numpy（推荐用于复杂输出）：** 我们可以使用以下代码行通过 Numpy 导出：`np.savetxt(result_folder+pc_dataset.split(“.”)[0]+”_selection.xyz”, np.asarray(o3d_parcel_corners),delimiter=’;’, fmt=’%1.9f’)`

1.  **Open3D（推荐仅需空间属性时使用）：** 我们可以使用以下代码行通过 Open3D 导出：`o3d.io.write_point_cloud(result_folder+pc_dataset.split(“.”)[0]+”_result_filtered_o3d.ply”, pcd_selection, write_ascii=False, compressed=False, print_progress=False)`

*🦚* **注意**：*`.split(`.`)`*允许将*`pc_dataset`*字符串对象拆分为两个字符串的列表，分别在*`.`*之前和之后，然后我们仅保留第一个元素，使用*`[0]`*。NumPy 将一个变量*`o3d_parcel_corners`*的阶段以分隔符 ; 导出到一个*.xyz* ASCII 文件中。Open3D 将从 open3d 对象*`pcd_selection`*中写入一个*.ply* 文件。*

哇，干得好！Python 自动化的批量结构已经运行起来了！恭喜！我们已经导入了库，数据集被存储在不同的显式变量中，我们确保可以处理各种可视化而无需离开 Python 的舒适环境。导出步骤已设置完成；剩下的就是解决我们在第 4 步中提出的初始问题：

> 我们房子周围的建筑区域有多密集？房子是否可能会受到洪水影响？我是否遵守了我拥有地块的建筑比例？

# 步骤 4. 3D Python 挑战

为了回答上述问题，我们将找到解决四个挑战和体素化步骤的方案，如下所示。

![](img/4b3d247558729a28d903f154c58610c7.png)

步骤 4：3D Python 挑战。我们研究兴趣点查询、手动边界选择、高点提取、体素化和建筑覆盖提取。

挑战 1 将允许裁剪出所需的研究区域。挑战 2 将允许提取所拥有地块的建筑比例。挑战 3 指导洪水分析。而挑战 4，经过体素化处理后，将允许获取感兴趣区域的建筑覆盖情况。让我们开始吧。

## 4.1. 兴趣点查询

对于这个挑战，我们从一个包含 3D `PointCloud` Open3D 对象和 `TriangleMesh` Open3D 对象的输入开始，如下所示：

![](img/76e8121f7bd21325fbe30c9778a0856b.png)

一个带有 3D 网格对象的 3D 点云。© F. Poux

这个挑战的目标是只保留落在兴趣点（建筑房屋）一定距离内的数据点。我们的输入是点云和网格，输出是经过过滤的点云，符合到 POI 距离的标准，如下所示：

![](img/6a9063effb3862f75a508f44428c39cc.png)

为此，我设置了一个六阶段的过程，如下所示：

![](img/6453feea2a7852e03490303e8fd1d213.png)

3D 点云半径搜索的概念工作流。© 作者。

🎓 **学习笔记**：*目标是尽量自己解决问题，如果遇到困难可以查看下面的解决方案。准备好后，可以阅读下面的解决方案。* 👇

(1) 设置距离阈值时，我们将所需的半径值（例如 `50`）传递给新变量 `dist_POI`。 (2) 然后，我们使用 Mesh 对象的 `get_center()` Open3D 方法从 BAG 数据集中获取 POI。这允许获取网格的中心作为 POI：`POI=mesh.get_center()`。之后，我们可以创建 KD 树 (3)，一种用于组织空间中点的数据结构。KD 树对点云处理非常有用，因为它们允许快速进行最近邻搜索和范围查询。这是好消息，因为这正是我们要做的 😁。实际上，使用 KD 树可以快速找到离给定点最近的点，或者所有在一定距离内的点（我们的 POI），而无需遍历点云中的所有点。

```py
# Creating a KD-Tree Data Structure
pcd_tree = o3d.geometry.KDTreeFlann(pcd_o3d)
```

🦚 **注意**：*KD-Tree 通过递归地将空间划分为较小的区域，每个级别沿一个维度的中位数进行划分（*`X` *是一个维度，* `Y` *是另一个，* `Z` *是另一个* 😉*）。这会生成一个“树”结构，其中每个节点表示一个空间分区，每个叶子节点表示一个单独的点。*

从那里，我们可以使用 Open3D 的 `search_radius_vector_3d()` 方法，并从输出索引中选择点：最后可视化我们的结果（`4`+`5`+`6`）：

```py
# Selecting points by distance from POI (your house) using the KD-Tree 
[k, idx, _] = pcd_tree.search_radius_vector_3d(POI, dist_POI)
pcd_selection=pcd_o3d.select_by_index(idx)
```

我们最终可以可视化我们的结果（6）：

```py
o3d.visualization.draw_geometries([pcd_selection,mesh])
```

![](img/01cce6fda16a7553c4811b3f1eccd17f.png)

半径搜索下的 3D 点云和网格叠加。

总共涉及以下代码管道：

![](img/ca75475c8f727b0c5dd5067e69a82134.png)

半径搜索的代码工作流。

非常好！现在你有了一个有效的解决方案，让我们从那个选择中提取地块面积。

## 4.2\. 手动边界选择

要提取非官方边界，我们需要采用一些半自动和互动的方法。好消息是，我们可以直接在 Python 中使用 Open3D 来完成这项工作。

首先需要创建一个带有以下 `draw_geometries_with_vertex_selection()` 方法的互动 Open3D 窗口：

```py
o3d.visualization.draw_geometries_with_vertex_selection([pcd_selection])
```

然后可以跟随下面的动画部分，这允许选择定义地块角落的点。

![](img/2ec4c396a33dc3162d115db29197734f.png)

手动边界选择的互动过程，使用 Open3D GUI 并按住 MAJ+鼠标选择感兴趣的点。

结果将出现在你的笔记本（或 REPL）中的单元格下方，关闭窗口后即可查看。

🦚 **注意**：*在此步骤中，使用* `R`*、* `G`*、* `B` *着色可能更容易。因此，如果您想使用正确的索引，需要在选择前返回进行更改。* 😉 *如果您想提取地籍边界*，*可以通过导入官方的 2D 矢量图形并根据此数据约束进行裁剪来实现。*

从你的 REPL 中，你可以将选定点的不同索引（例如 `34335`、`979`、`21544`、`19666`、`5924`、`21816`、`38008`）复制并粘贴到 `select_by_index()` 选择方法中，以定义 `o3d_parcel_corners` 变量：

```py
o3d_parcel_corners=pcd_selection.select_by_index([34335 ,979 ,21544 ,19666 ,5924 ,21816 ,38008 ])
```

我们仍然需要进一步准备角落，因为我们想避免考虑 `Z` 值。因此，我们将过滤坐标以去掉 Z 值，但要注意：这样做意味着我们认为我们在一个平坦区域（这在荷兰是事实 😉）。

```py
o3d_parcel_corners=np.array(o3d_parcel_corners.points)[:,:2]
```

从那里，接下来使用 `Shapely` 库计算地块的面积！为此，我们可以直接使用 `Polygon` 函数首先从提供的角落集合中创建一个多边形：

```py
Polygon(o3d_parcel_corners)
```

输出如下：

![](img/ea3f386608dbe0efefe779723d4ad548.png)

错误的几何形状

这看起来有点不对，是吧？如果你不相信我，我们可以计算面积来检查：

```py
pgon = Polygon(o3d_parcel_corners)
print(f"This is the obtained parcel area: {pgon.area} m²")
```

这会输出`43.583 m²`。对于一个大建筑来说，这听起来很奇怪，不是吗？哈哈，一个简单的问题变得有点复杂了！实际上，这里的问题在于计算面积需要一个由多边形组成的闭合形状。这并不总是能实现，因为添加角点的顺序可能会影响数据结构。因此，问题变成了排序坐标以避免边缘交叉。

处理这个问题的一种方法是从中心点的角度来看所有坐标。然后，我们可以计算列表中每个角点与中心点之间的角度。我们这样做是为了了解每个角度的宽度，从而提供排序坐标的手段。将其翻译成代码允许定义一个排序函数，如下所示：

```py
def sort_coordinates(XY):
    cx, cy = XY.mean(0)
    x, y = XY.T
    angles = np.arctan2(x-cx, y-cy)
    indices = np.argsort(-angles)
    return XY[indices]
```

🦚 **注意**：*我使用 NumPy* `arctan2()` *方法对每个坐标进行角度估算。这将返回以弧度表示的角度数组。剩下的工作是将角度按升序排序，以获取正确顺序的索引列表。然后，列表可以使用* `argsort()` *方法修正原始列表的索引。在此步骤中，它不适用于 U 形建筑。*

从那里，我们可以将排序函数应用于角点，创建一个新的排序变量，计算多边形及其相关面积：

```py
np_sorted_2D_corners=sort_coordinates(o3d_parcel_corners)
pgon = Polygon(np_sorted_2D_corners)
Polygon(np_sorted_2D_corners)
print(f"This is the parcel area: {pgon.area} m²")
```

这返回了一个面积为`2 247.14 m²`的多边形，这更为可信。😉

![](img/5d875c3360117687f6b52f9bace96f7e.png)

正确的几何形状

再次做得很好。我们现在对我们地块的面积有了一个不错的了解。现在，让我们在区域内找到高点和低点。

## 4.3\. 查找高点和低点

要获取区域的低点和高点，一种特定的方法是使用 Open3D 的`get_max_bound()`和`get_min_bound()`方法：

```py
pcd_selection.get_max_bound()
pcd_selection.get_min_bound()
```

但是，这样做不会提示哪个点是最高的，哪个点是最低的。我们需要的是点的索引，以便在之后检索坐标。为此，我建议我们这样编码：

1.  我们从 Open3D PointCloud 对象中创建一个 NumPy 数组对象`np_pcd_selection`，该对象仅包含`X`、`Y`和`Z`坐标。

1.  我们使用`argmax()`方法收集`Z`维度上的最小值和最大值的索引，并将结果存储在变量`lowest_point_index`和`highest_point_index`中。

```py
np_pcd_selection=np.array(pcd_selection.points)
lowest_point_index=np.argmin(np_pcd_selection[:,2])
highest_point_index=np.argmax(np_pcd_selection[:,2])
```

让我们通过选择索引，创建`TriangleMesh`球体，将它们转换到点的位置，然后使用 Open3D 进行可视化来检查结果：

```py
# We create 3D Spheres to add them to our visual scene
lp=o3d.geometry.TriangleMesh.create_sphere()
hp=o3d.geometry.TriangleMesh.create_sphere()

# We translate the 3D Spheres to the correct position
lp.translate(np_pcd_selection[lowest_point_index])
hp.translate(np_pcd_selection[highest_point_index]))

# We compute some normals and give color to each 3D Sphere
lp.compute_vertex_normals()
lp.paint_uniform_color([0.8,0.1,0.1])
hp.compute_vertex_normals()
hp.paint_uniform_color([0.1,0.1,0.8])

# We generate the scene
o3d.visualization.draw_geometries([pcd_selection,lp,hp])
```

如下所示，我们现在有了包含高点和低点的 3D 场景。

![](img/3184931216ccb57af6023a5964685cfa.png)

让我们在 350 米的扩展区域内研究建筑覆盖情况。为此，我们将重新执行代码中的某些部分，如下所述。

## 4.4\. 点云体素化

我们想要提取已建立的覆盖范围。为此，我们将采取一种直观的方法，首先将点云的模式转换为 2D 像素的填充模拟：3D 体素。这将允许我们使用以下方法：

![](img/b6ed77cf8ddc61af78175af10bfa9c2f.png)

通过构建 3D 体素数据结构来提取高点。我们首先对点云进行体素化，然后从二进制角度对每个体素上色，最后按自上而下的索引进行筛选。© F. Poux

基本上，我们将（1）生成存在点的体素，然后（2）根据体素所持有的点的分类对体素上色，最后，我们将筛选体素，只保留和计数每个 X,Y 坐标的最高体素。这样，我们可以避免计数那些会偏向已建立覆盖范围的元素。

第一步是创建一个体素网格。这是 Open3D 的强项：通过这些简单的代码行，我们可以将一个体素网格适配到点云中，每个体素是一个 20 cm 的立方体，然后可视化结果：

```py
voxel_grid = o3d.geometry.VoxelGrid.create_from_point_cloud(pcd_selection, voxel_size=2)
o3d.visualization.draw_geometries([voxel_grid])
```

![](img/fd12edaca914ab554fbe335093ad0e41.png)

3D 体素数据结构的视图。© F. Poux

🦚 **注意**：*您看到的结果已经经过体素上色的变化。这个变化意味着，使用 Open3D 时，我们必须更改体素点的颜色。然后，VoxelGrid 方法将平均这些颜色，并将结果保留为体素的颜色。*

现在我们知道如何从点云生成 3D 体素，让我们通过改变它们的颜色方案来绕过颜色平均的问题。为此，一个简单的方法是以二进制方式处理颜色。即黑色（[0,0,0]）或其他颜色（所有其他颜色）。这意味着我们可以将颜色变量初始化为 0，然后选择所有被分类为建筑的点，并赋予它们另一种颜色，例如红色（[1,0,0]）：

```py
colors=np.zeros((len(pcd_df), 3))
colors[pcd_df['Classification'] == 6] = [1, 0, 0]
pcd_o3d.colors = o3d.utility.Vector3dVector(colors)
```

非常好！现在，由于我们对原始点云进行了操作，我们需要根据自己的选择重新定义 POI 和选择。为了提高效率，您会找到可以运行的代码块，以更新体素渲染到新的颜色方案：

```py
# Defining the POI and the center of study
dist_POI=50
POI=mesh.get_center()

# Querying all the points that fall within based on a KD-Tree
pcd_tree = o3d.geometry.KDTreeFlann(pcd_o3d)
[k, idx, _] = pcd_tree.search_radius_vector_3d(POI, dist_POI)
pcd_selection=pcd_o3d.select_by_index(idx)

#Computing the voxel grid and visualizing the results
voxel_grid = o3d.geometry.VoxelGrid.create_from_point_cloud(pcd_selection, voxel_size=2)
o3d.visualization.draw_geometries([voxel_grid])
```

![](img/8fe6067757404ffe3a2d928221c50c0a.png)

二进制着色的点云。© F. Poux

太棒了！现在，我们需要实际获取每个体素的离散整数索引，并将它们列出，还有体素的颜色和边界。这将允许我们之后遍历每个体素及其颜色，检查一个体素是否在“体素列”中是最高的。为此，一个有效的方法是使用列表推导式来向量化我们的计算，避免不必要的循环：

```py
idx_voxels=[v.grid_index for v in voxel_grid.get_voxels()]
color_voxels=[v.color for v in voxel_grid.get_voxels()]
bounds_voxels=[np.min(idx_voxels, axis=0),np.max(idx_voxels, axis=0)]
```

太棒了！在 50 米的选择区域中，我们有一个 49 x 49 x 16 的体素网格，总共有 38,416 个填充的体素。现在是提取已建立的覆盖范围的时候了！

## 4.5\. 提取已建立的覆盖范围

现在我们有了一个具有二进制颜色的体素化点云，我们专注于下图的第三阶段：

![](img/f9bc143b7fc885997f06c2ae9dffff23.png)

正如我们所见，选择仅顶层体素比看起来复杂一些！😁 但幸运的是，我设计了一个简单而强大的管道，可以做到这一点，见下文。

![](img/f0032781dab39f48d3a5d500f98e2246.png)

体素选择工作流以七个子步骤提取已建成的覆盖范围。© F. Poux。

让我们一步一步来。

(1) 首先，我们初始化两个字典来保存最大索引：

```py
max_voxel={}
max_color={}
```

(2 到 5) 我们遍历所有填充的体素，以检查它们是否在体素列中最高或应被丢弃，这给出了以下 for 循环：

```py
for idx, v in enumerate(idx_voxels):
    if (v[0],v[1]) in max_voxel.keys():
        if v[2]>max_voxel[(v[0],v[1])]:
            max_voxel[(v[0],v[1])]=v[2]
            max_color[(v[0],v[1])]=color_voxels[idx]
    else:
        max_voxel[(v[0],v[1])]=v[2]
        max_color[(v[0],v[1])]=color_voxels[idx] 
```

🦚 **注意**: *首先要注意的是循环定义中的* `enumerate()` *方法。这允许在循环* `idx_voxels` *变量的每个值时跟踪列表的索引。很方便！第二点是我们使用“元组”数据类型，如* `(X, Y)`*，这给出了体素的整数位置。这使我们能够确保我们持续检查相同的* `X`*、* `Y` *网格位置。最后，“*`if`*”语句允许测试表达的条件，并在条件返回* `True`*时执行。如果条件不为* `True`*，则执行* `else` *语句（如果存在）或跳过并退出条件检查。*

(6) 我们初始化标记为已建成或未建成的体素的计数，并检查保留的顶部体素的颜色是否为黑色，如果是，我们更新相应类别的体素计数：

```py
count_building_coverage,count_non_building=0,0
for col in list(max_color.values()):
    if np.all(col==0):
        count_non_building+=1
    else:
        count_building_coverage+=1
```

🦚 **注意**: `np.all()` *也是一个布尔检查，只会在所有值都是* `True` *时返回* `True`。在我们的例子中，黑色是* `[0,0,0]`*，因此返回* `True` *，因为所有* `R`*、* `G`* 和* `B` *都设置为* `0`。如果其中一个不是零，则意味着并非所有值都是* `0`*，`np.all()` *将返回* `False`*，这会触发* `else` *语句。简单吧？* 😉

(7) 我们可以提取每种类型（已建成和未建成）的覆盖区域，记得将计数变量乘以实际的 2D 体素面积（在我们的例子中为 4 平方米），并计算比率：

```py
print(f"Coverage of Buildings: {count_building_coverage*4} m²")   
print(f"Coverage of the Rest: {count_non_building*4} m²")
print(f"Built Ratio: {(count_building_coverage*4)/(count_building_coverage*2+count_non_building*2)} m²") 
```

这样，对于 50 米的范围，我们的覆盖率为 17.3%，相当于 1352 平方米的已建成区域和 6456 平方米的其余区域。（350 米查询的覆盖率为 19.2%，73416 平方米和 308280 平方米）。

因此，与其余部分相比，我们在选定的兴趣点上有一个很好的建筑占用平衡！

希望测试我们的 3D Python 工作流并没有让你感到太紧张！随时返回查看代码和工作流片段，掌握隐藏的代码技巧，以便（1）出色地应对挑战和（2）优化我们实现的效率。

![](img/7c041bc073b2d234e2497baf19ce1a09.png)

然而，仍然有一些调整空间，例如使用分类信息结合高低 POI 来隔离水流的地面点，或使用地块表面选择作为过滤技术来提取已建成的覆盖范围。

+   💻 直接访问实操代码：[Google Colab](https://colab.research.google.com/drive/1vvPOYZtUMx2zUxDczakv6XDUq0Ram21y)

+   🪄 直接访问点云处理：[Github](https://github.com/florentPoux/point-cloud-processing)

+   🧙‍♂️ 直接访问点云课程：[3D Academy](https://learngeodata.eu)

# 🔮 结论

恭喜！这在使用 LiDAR 进行城市建模的 3D Python 工作流程领域是一个真正的巨大第一步！我们下面构建的流程非常通用，可以作为你未来分析工作的参考点，这些工作可能会遵循类似的模式。

![](img/35a2f7dccc19d471620463aa5284aeb7.png)

在 LiDAR 城市模型背景下的 3D Python 工作流程。我们从环境设置（步骤 1）和 3D 数据准备（步骤 2）开始。一旦完成这些步骤，我们将进入 Python 自动化（步骤 3），其中一个特定部分涉及 3D Python 挑战（步骤 4），如地块表面或兴趣点查询。© 作者

总结一下你解锁的新技能，你现在可以高效地应对以下挑战：

1.  将开源软件与 Python 结合成一个连贯的工作流程；

1.  收集开放数据集并在处理之前准备好它们；

1.  使用 3D Python 处理 3D 数据模式，特别是 3D 点云、3D 网格和 3D 体素；

1.  使用每种 3D 模式的关键方面进行高级地理空间分析，同时优化代码；

1.  作为城市规划师从分类点云中提取关键洞察，以更好地理解本地区域。

如果你感到自己能够掌握并处理这些不同的方面，那么你正走在成为伟大的 3D 地理空间专业人士的道路上！唯一需要做的就是保持努力，推动现有的边界。

## 🤿 更进一步

但学习旅程并未就此结束。我们的终身探索才刚刚开始，未来的步骤将深入研究 3D 体素工作，探索语义、CityGML、CityJSON，特别是如何从聪明的城市走向智慧城市。此外，我们还将利用深度学习技术分析点云，并解锁高级 3D LiDAR 分析工作流程。有很多令人兴奋的内容！
