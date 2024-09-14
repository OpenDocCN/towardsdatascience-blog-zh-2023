# 室内建模的 3D 点云形状检测

> 原文：[`towardsdatascience.com/3d-point-cloud-shape-detection-for-indoor-modelling-70e36e5f2511`](https://towardsdatascience.com/3d-point-cloud-shape-detection-for-indoor-modelling-70e36e5f2511)

## [动手教程](https://towardsdatascience.com/tagged/hands-on-tutorials)，3D Python

## 10 步 Python 指南，用于自动化 3D 形状检测、分割、聚类和体素化，以实现室内点云数据集的空间占用 3D 建模。

[](https://medium.com/@florentpoux?source=post_page-----70e36e5f2511--------------------------------)![Florent Poux, Ph.D.](https://medium.com/@florentpoux?source=post_page-----70e36e5f2511--------------------------------)[](https://towardsdatascience.com/?source=post_page-----70e36e5f2511--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----70e36e5f2511--------------------------------) [Florent Poux, Ph.D.](https://medium.com/@florentpoux?source=post_page-----70e36e5f2511--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----70e36e5f2511--------------------------------) ·阅读时长 28 分钟·2023 年 9 月 7 日

--

如果你有点云或数据分析的经验，你知道识别模式有多重要。识别具有相似模式的数据点或“对象”对获取有价值的见解至关重要。我们的视觉认知系统可以轻松完成这项任务，但通过计算方法复制这种人类能力是一个重大挑战。

目标是利用人类视觉系统自然的元素分组倾向。👀

![](img/f7a3a366e3f8d7cc6c3e1b7a11307505.png)

3D 点云分割阶段结果示例。© F. Poux

## 但这有什么用呢？

首先，它通过将数据分组到不同的段中，让你轻松访问和处理数据的特定部分。其次，通过查看区域而不是单个点，它使数据处理更快。这可以节省大量时间和精力。最后，分割可以帮助你发现通过查看原始数据无法看到的模式和关系。🔍 总的来说，分割对于从点云数据中获取有用信息至关重要。如果你不确定如何做，别担心——我们会一起搞定的！🤿

## 策略

在我们以有效的解决方案处理项目之前，让我们框定整体方法。本教程遵循一个由十个简单步骤组成的策略，如下方的策略图所示。

![](img/49c746a4375f4f284fe81f501c11fb4f.png)

本指南展示了 3D 点云室内建模的工作流程。© F. Poux

策略已列出，下面你可以找到不同步骤的快捷链接：

```py
Step 1\. Environment Setup
Step 2\. 3D Data Preparation
Step 3\. Data Pre-Processing
Step 4\. Parameter Setting
Step 5\. RANSAC Planar Detection
Step 6\. Multi-Order RANSAC
Step 7\. Euclidean Clustering Refinement
Step 8\. Voxelization Labelling
Step 9\. Indoor Spatial Modelling
Step 10\. 3D Workflow Export
```

既然我们已做好准备，那就直接开始吧。

🎵**读者注意**：此动手指南是* [***UTWENTE***](https://www.itc.nl/) *联合工作的一部分，由* ***F. Poux*** *和* ***V. Lehtola*** *共同作者。我们感谢来自数字双胞胎* [*@ITC*](http://twitter.com/ITC) *项目的资助，该项目由特温特大学 ITC 系提供。***所有图像 © Florent Poux***。*

# 1\. 设置您的 Python 环境。

![](img/b862dfa664dd54db82f77e3449a9a117.png)

在下面的上一篇文章中，我们展示了如何快速设置 Anaconda 环境并使用 IDE JupyterLab 管理代码。如果您继续以这种方式设置自己，您将成为一名完整的 Python 应用程序开发者 😆。

[](/3d-python-workflows-for-lidar-point-clouds-100ff40e4ff0?source=post_page-----70e36e5f2511--------------------------------) ## 3D Python 工作流程用于 LiDAR 城市模型：逐步指南

### 解锁 3D 城市建模应用程序的精简工作流程的终极指南。教程涵盖了 Python...

towardsdatascience.com

🦊 **Florent**：*我强烈推荐使用桌面设置或 IDE，避免使用 Google Colab，尤其是当您需要使用提供的库可视化 3D 点云时，因为它们在最佳情况下会不稳定，最糟糕的情况下则无法工作（不幸的是…）。*

🤠 **Ville**：*我们猜您是在 Windows 上运行？这没问题，但如果您想进入计算方法领域，Linux 是首选！*

好吧，我们将采取一种“快速成果”的方法。实际上，我们将通过遵循最简化的编码方法来实现卓越的分割💻。这意味着我们对底层库非常挑剔！我们将使用三个非常强大的库，分别是`numpy`、`matplotlib`和`open3d`。

好的，要在一个全新的虚拟环境中安装上述库包，我们建议您从`cmd`终端运行以下命令：

```py
conda install numpy
conda install matplotlib
pip install open3d==0.16.0
```

🍜 ***免责声明***：*我们选择了 Python，而不是 C++ 或 Julia，所以性能就是那样 😄。希望它能满足您的应用需求 😉，对于我们所谓的“离线”过程（非实时）。*

在您的 Python IDE 中，请确保导入将被频繁使用的三个库：

```py
#%% 1\. Library setup
import numpy as np
import open3d as o3d
import matplotlib.pyplot as plt
```

就这样！我们准备好进行室内点云建模工作流程了！

# 2\. 点云数据准备

![](img/e18d10ffc4d22a7dfa415f9b85211b8a.png)

在之前的教程中，我们演示了如何处理点云并在使用 AHN4 LiDAR 活动获取的 3D 数据集上进行网格化。这一次，我们将使用一个通过地面激光扫描仪收集的数据集：ITC 新建筑。它被组织成三个不同的集合供您实验。紫色的是户外部分。红色是地面层，绿色是第一层，如下图所示。

![](img/a3a4446fe2fae77b40da248fa8e3081b.png)

使用的 ITC 数据集的 3D 点云部分。© F. Poux

你可以从这里下载数据：[ITC 数据集](https://drive.google.com/drive/folders/1sCBT1lc9A8Zn4grpxwFrBrvos86c0HZR?usp=share_link)。一旦你在本地对数据有了牢固的掌握，你可以用两行简单的代码将数据集加载到你的 Python 执行环境中：

```py
DATANAME = "ITC_groundfloor.ply"
pcd = o3d.io.read_point_cloud("../DATA/" + DATANAME)
```

🦊 **Florent**: *这段 Python 代码片段使用* `*Open3D*` *库来读取位于目录“*`*../DATA/*`*”中的点云数据文件* `*ITC_groundfloor.ply*` *，并将其赋值给变量* `*pcd*`*。*

变量现在保存着你的点云 `pcd`，你将用以下代码进行操作：

![](img/6ac078d0d708734f455c9e8907b18642.png)

一旦点云数据成功加载到 Open3D 中，下一步是应用各种预处理技术来提升其质量并提取有意义的信息。

# 3\. 点云预处理

![](img/0bb8b750bb4d32115dac7abbc406fbde.png)

如果你打算在 `open3d` 中可视化点云，建议你将点云移动以绕过大坐标的近似，这样可以避免产生晃动的可视化效果。要对你的 `pcd` 点云应用这种移动，首先获取点云的中心，然后通过从原始变量中减去它来进行平移：

```py
pcd_center = pcd.get_center()
pcd.translate(-pcd_center)
```

现在可以通过以下代码行进行交互式可视化：

```py
o3d.visualization.draw_geometries([pcd])
```

🦚 **注意**: `o3d.visualization.draw_geometries([pcd])` *调用了* `*draw_geometries()*` *函数，这个函数属于 Open3D 的* `*visualization*` *模块。该函数接受一个几何体列表作为参数，并在可视化窗口中显示它们。在这种情况下，列表包含一个几何体，即表示点云的* `*pcd*` *变量。* `*draw_geometries()*` *函数创建一个 3D 可视化窗口并渲染点云。你可以与可视化窗口互动以旋转、缩放，并从不同角度探索点云。*

![](img/38196ff8a2b3f0d77fa32690deefe8ba.png)![](img/1b23848831179d63eccaa3cf24969e6a.png)

太棒了👌，我们已经准备好测试一些采样策略来统一我们的下游处理。

## 3.1\. 点云随机采样

让我们考虑那些可以有效减少点云大小同时保持整体结构完整性和代表性的方法。如果我们将点云定义为一个矩阵 (m x n)，那么通过保留该矩阵每隔 n 行的一个行，我们可以得到一个**简化的点云**：

![](img/43048e0ad321a1ec9cb3b8607d68133a.png)

一个点云简化策略。© F. Poux

在矩阵级别，简化操作是通过根据 n 因子保留每隔 n 行的点来进行的。当然，这取决于点在文件中的存储方式。用 `open3d` 切片点云是相当直接的。为了简化和参数化表达式，你可以写出以下代码：

```py
retained_ratio = 0.2
sampled_pcd = pcd.random_down_sample(retained_ratio)
```

🦚 **注意**：`sampled_pcd = pcd.random_down_sample(retained_ratio)` *对原始点云* `*pcd*` *应用随机下采样，使用的是* `random_down_sample()` *函数，该函数由 Open3D 提供。* `retained_ratio` *参数决定了下采样后保留的点的比例。例如，如果* `retained_ratio` *设置为 0.5，则大约 50% 的点将被随机选择并保留在采样后的点云中。*

```py
o3d.visualization.draw_geometries([sampled_pcd], window_name = "Random Sampling")
```

![](img/f4d15aa96e162454499be387b499b6e3.png)

点云的下采样结果。© F. Poux

🌱 **发展**：*在研究 3D 点云时，随机采样有其局限性，可能会导致遗漏重要信息和分析不准确。它没有考虑点之间的空间组件或关系。因此，使用其他方法以确保更全面的分析是至关重要的。*

尽管这种策略快速，但随机采样可能不适合“标准化”用例。下一步是通过统计异常值去除技术来处理潜在的异常值，以确保数据的质量和可靠性，以便进行后续分析和处理。

## 3.2\. 统计异常值去除

对 3D 点云数据使用异常值过滤器可以帮助识别和去除任何显著不同于数据集其余部分的数据点。这些异常值可能是由于测量误差或其他因素造成的，这些因素可能会影响分析。通过去除这些异常值，我们可以获得更有效的数据表示，并更好地调整算法。然而，我们需要小心不要删除有价值的点。

我们将定义一个 `statistical_outlier_removal` 过滤器，以去除与其邻居相比远离平均值的点。它需要两个输入参数：

+   `nb_neighbors`，用于指定在计算给定点的平均距离时考虑多少个邻居。

+   `std_ratio`，允许根据点云中平均距离的标准差设置阈值水平。这个数值越低，过滤器的作用就越强。

这包括以下内容：

```py
nn = 16
std_multiplier = 10

filtered_pcd, filtered_idx = pcd.remove_statistical_outlier(nn, std_multiplier)
```

🦚 **注意**：*`*remove_statistical_outlier()*` *函数通过指定数量的最近邻（*`*nn*`*）和一个标准差乘数（*`*std_multiplier*`*）来对点云应用统计异常值去除。该函数返回两个值：* `*filtered_pcd*` *和* `*filtered_idx*`*。* `*filtered_pcd*` *表示经过滤的点云，其中已去除统计异常值。* `*filtered_idx*` *是一个索引数组，对应于在异常值去除过程中保留的原始点云* `*pcd*` *中的点。*

为了可视化这种过滤技术的结果，我们将异常值标记为红色，并将其添加到我们希望可视化的点云对象列表中：

```py
outliers = pcd.select_by_index(filtered_idx, invert=True)
outliers.paint_uniform_color([1, 0, 0])
o3d.visualization.draw_geometries([filtered_pcd, outliers])
```

![](img/6ef3e983fc62a7eda4eaf143ce2330a5.png)

异常值在点云中用红色标记和可视化。© F. Poux

在从点云中移除统计异常值之后，下一步是应用基于体素的采样技术，进一步下采样数据，以便高效处理和分析，同时保留点云的基本结构特征。

## 3.3. 点云体素（网格）采样

网格下采样策略基于将 3D 空间划分为规则的立方体单元，称为体素。对于这个网格的每个单元，我们只保留一个代表性点，并且可以用不同的方式选择这个点。当下采样时，我们保留该单元最接近重心的点。

![](img/802a55909c5aa28b959e4357b8e2f9e4.png)

体素网格采样示例。© F. Poux

具体而言，我们定义一个`voxel_size`，然后用它来过滤我们的点云：

```py
voxel_size = 0.05
pcd_downsampled = filtered_pcd.voxel_down_sample(voxel_size = voxel_size)
```

🦚 **注意**：*此行对过滤后的点云执行体素下采样，* `*filtered_pcd*`*，使用* `*voxel_down_sample()*` *函数。* `*voxel_size*` *参数指定体素下采样点云的每个体素的大小。较大的体素尺寸会导致点云密度显著减少。下采样操作的结果分配给变量* `*pcd_downsampled*`*，代表下采样后的点云*。

现在是仔细可视化下采样后分布的时间：

```py
o3d.visualization.draw_geometries([pcd_downsampled])
```

![](img/f65a10e76a7e48feae0b94ffc246e5db.png)

应用在点云上的采样策略结果。© F. Poux

在此阶段，我们有一组未在进一步处理时触及的异常点和一个下采样后的点云，构成了后续过程的新主题。

## 3.4. 点云法线提取

点云法线指的是 3D 点云中特定点处表面的方向。它可以用于通过将点云划分为具有相似法线的区域来进行分割。例如，在我们的案例中，法线将有助于识别点云中的物体和表面，使其更容易可视化。这也是介绍一种半自动计算法线的方法的绝佳机会。我们首先定义点云中每个点与其邻居之间的平均距离：

```py
nn_distance = 0.05
```

然后，我们利用这些信息在半径为`radius_normals`的范围内提取有限的`max_nn`个点，为 3D 点云中的每个点计算法线：

```py
radius_normals=nn_distance*4
pcd_downsampled.estimate_normals(search_param=o3d.geometry.KDTreeSearchParamHybrid(radius=radius_normals, max_nn=16), fast_normal_computation=True)
```

现在，`pcd_downsampled`点云对象很高兴地拥有法线，准备展示其最漂亮的一面😊。此时你知道该做什么：

```py
pcd_downsampled.paint_uniform_color([0.6, 0.6, 0.6])
o3d.visualization.draw_geometries([pcd_downsampled,outliers])
```

![](img/92aa19172478994bb05bcd86ef4bd146.png)

用于后续实验的点云。

完成点云的体素下采样后，下一步是配置点云形状检测和聚类的参数，这在将相似点分组以及从下采样点云数据中提取有意义的结构或对象中起着关键作用。

# 4\. 点云参数设置

![](img/4d43adb22c40912f783a2cb44c0064b4.png)

在本教程中，我们为您精选了两种最有效且可靠的 3D 形状检测和聚类方法：RANSAC 和使用 DBSCAN 的欧几里得聚类。然而，在使用这些方法之前，了解参数的同时，理解基本概念是至关重要的。

## RANSAC

RANSAC 算法，RANdom SAmple Consensus 的缩写，是一个强大的工具，用于处理包含异常值的数据，这在使用现实世界传感器时常常会遇到。该算法通过将数据点分为两类：内点和外点来工作。通过识别并忽略外点，您可以专注于处理可靠的内点，从而使分析更加有效。

![](img/a5c122fcaa8c0f6453ae2b2363de65b9.png)

在 3D 点云中使用 RANSAC 进行平面检测。© F. Poux

让我用一个简单的例子来说明 RANSAC 的工作原理。假设我们想通过下面的点云拟合一个平面。我们怎么做呢？

![](img/bb85346e13ae187ec137bef80050cce3.png)

在随机点云中进行的 RANSAC 平面检测模拟。© F. Poux

首先，我们从数据中创建一个平面，为此，我们从点云中随机选择 3 个点来建立平面。然后，我们简单地检查剩余的点中有多少点落在该平面上（达到某个阈值），这将为提案打分。

![](img/4162c310eb51619b469d062fc8078732.png)

RANSAC 评分系统说明。您可以看到，每次迭代都会随机抽取 3 个点，从中创建一个平面，然后选择落在平面上的点。在这里，第 159 次迭代会是最佳候选。© F. Poux

然后，我们用 3 个新的随机点重复这个过程，看看效果如何。是更好了吗？更差了吗？然后，我们反复进行这个过程，比如说 10 次、100 次、1000 次，然后选择得分最高的平面模型（即具有最佳“支持”的剩余数据点）。这将是我们的解决方案：支持点加上我们采样的三个点构成我们的**内点集**，其余部分是我们的**外点集**。够简单吧 😁？

哈哈，对于那些怀疑者，你们难道没有一个上升的问题吗？我们如何实际确定应该重复多少次过程？我们应该多频繁地尝试？好吧，这实际上是可以计算的，但我们先把它放到一边，专注于当前的问题：点云分割😉。

## RANSAC 参数设置

我们希望使用 RANSAC 来检测点云中的 3D 平面形状。使用 RANSAC 时，我们需要定义三个参数：一个距离阈值`distance_threshold`，用于将点标记为内点或外点；一个最小点数`ransac_n`，用于拟合几何模型；一个迭代次数`num_iterations`。

**距离阈值的确定**：我们可能会因为需要一些领域知识来设置未来的分割和建模阈值而受到限制。因此，尝试绕过这一点以使方法对非专家开放将是令人兴奋的。我们将与你分享一个简单的想法，这可能会有用。如果我们计算数据集中点之间的平均距离，并将其作为设置阈值的基础，会怎样呢？

好吧，这是一个值得探索的想法。为了确定这样的值，我们使用 `KD-Tree` 来加速每个点的最近邻查询过程。从那里开始，我们可以查询点云中每个点的 k 个最近邻，然后将其打包到下面显示的 `open3d` 函数中：

```py
nn_distance = np.mean(pcd.compute_nearest_neighbor_distance())
```

毫无意外，它接近 5 cm，因为我们将点云采样到这个值。这意味着如果我们考虑最近邻，我们会有 51 mm 的平均距离。

🌱 **成长**：*从这里开始，我们可以将 RANSAC 参数设置为从 nn_distance 变量得出的值，这样可以根据考虑的数据集进行调整，而不依赖于领域知识。你会怎么处理这个问题？*

**点数的确定**：这里很简单。我们想要找出平面，因此我们将取定义 3D 平面所需的最小点数：3。

**迭代次数的确定**：迭代次数越多，你的 3D 形状检测就越稳健，但所需时间也越长。目前，我们可以将其设置为 1000，这样可以获得良好的结果。我们将在未来的教程中探索更多巧妙的方法来找出点云的噪声比例。😉

🧙‍♂️ **专家**：*有一种自动获取每次迭代次数的方法。如果我们希望以概率 p（例如 99%）成功，数据中的离群点比例是 e（例如 60%），我们需要 s 个点来定义我们的模型（这里是 3）。以下公式给出了预期的迭代次数：*

![](img/f34e9a8ff5e9d1dd5f22f9c860ef44df.png)

现在我们已经定义了 RANSAC 参数，我们可以研究第一次分割过程。

# 5\. 使用 RANSAC 进行点云分割

![](img/17e07a01da7ba28e3c757053c097f9b7.png)

首先将不同的阈值设置为非自动值进行测试：

```py
distance_threshold = 0.1
ransac_n = 3
num_iterations = 1000
```

从那里开始，我们可以使用 RANSAC 对点云进行分割以检测平面，具体如下：

```py
plane_model, inliers = pcd.segment_plane(distance_threshold=distance_threshold,ransac_n=3,num_iterations=1000)
[a, b, c, d] = plane_model
print(f”Plane equation: {a:.2f}x + {b:.2f}y + {c:.2f}z + {d:.2f} = 0")
```

我们将结果收集到两个变量中：`plane_model`，它包含平面的参数 `a`,`b`,`c`,`d`，以及 `inliers` 作为点索引。

这使得可以使用索引将点云分割为 `inlier_cloud` 点集（我们将其标记为红色）和 `outlier_cloud` 点集（我们将其标记为灰色）：

```py
inlier_cloud = pcd.select_by_index(inliers)
outlier_cloud = pcd.select_by_index(inliers, invert=True)

#Paint the clouds
inlier_cloud.paint_uniform_color([1.0, 0, 0])
outlier_cloud.paint_uniform_color([0.6, 0.6, 0.6])

#Visualize the inliers and outliers
o3d.visualization.draw_geometries([inlier_cloud, outlier_cloud]) 
```

🦊 **Florent** *：`*invert=True*` *参数允许选择第一个参数的反义，即所有不在* `*inliers*`* 中的索引。如果你缺少阴影，记得计算法线，如上所示。*

![](img/36f15121cce1019bae4e2e1fe3e1ebcc.png)

🌱 **成长**：*尝试调整各种参数，并通过定性分析研究其影响。首先记住要去相关变化（一次一个变量）；否则你的分析可能会有偏差* 😊。

![](img/385ce6263c78e78ca2ce72c1d56b7849.png)![](img/d0cf065a6a1ee3ea0f54091a3c00d1fb.png)

很棒！你知道如何将点云分割为内点集和外点集 🥳！现在，让我们研究如何找到彼此接近的一些簇。让我们想象一下，一旦我们检测到大的平面部分，我们有一些“漂浮”的物体需要 delineate。怎么做呢？（是的，这是一个假问题，我们有答案给你 😀）

# 6\. 扩展 3D 分割：多阶 RANSAC

![](img/24389692747d404df62d0d88855514cd.png)

我们的理念将非常简单。我们将首先多次运行 RANSAC（假设 `n` 次）以提取构成场景的不同平面区域。然后，我们将通过欧几里得聚类（DBSCAN）处理“漂浮元素”。这意味着我们必须确保有一种方法在迭代过程中存储结果。准备好了吗？

好的，让我们实例化一个空字典，以保存迭代结果（`segment_models`中的平面参数和`segments`中的点云平面区域）：

```py
segment_models={}
segments={}
```

然后，我们需要确保可以影响以后迭代的次数，以检测平面。为此，让我们创建一个变量`max_plane_idx`来保存迭代次数：

```py
max_plane_idx=10
```

🦊 **Florent***：在这里，我们说我们希望迭代 10 次以找到 10 个平面，但有更聪明的方法来定义这样的参数。这实际上扩展了文章的范围，将在另一节中讨论。*

现在让我们进入一个工作循环 😁，我将首先快速说明。

![](img/75431eb5d58c5a9972a73c461b4868ee.png)

执行以在 RANSAC 过程中进行分割的循环。© F. Poux

在第一次迭代（循环`i=0`）中，我们将内点与外点分开。我们将内点存储在`segments`中，然后我们只对存储在`rest`中的剩余点感兴趣，这将成为循环 n+1（循环`i=1`）的研究对象。这意味着我们希望将上一阶段的外点作为基础点云，直到达到上述的迭代阈值（与 RANSAC 迭代不同）。这转化为以下内容：

```py
rest=pcd
for i in range(max_plane_idx):
    colors = plt.get_cmap("tab20")(i)
    segment_models[i], inliers = rest.segment_plane(
    distance_threshold=0.1,ransac_n=3,num_iterations=1000)
    segments[i]=rest.select_by_index(inliers)
    segments[i].paint_uniform_color(list(colors[:3]))
    rest = rest.select_by_index(inliers, invert=True)
    print("pass",i,"/",max_plane_idx,"done.")
```

就是这样！现在，为了可视化整体，我们通过循环中的第一行将每个检测到的分段涂上来自`tab20`的颜色（`colors = plt.get_cmap("tab20")(i)`），你只需写：

```py
o3d.visualization.draw_geometries([segments[i] for i in range(max_plane_idx)]+[rest])
```

🦚 **注意**：我们传递给函数`o3d.visualization.draw_geometries()`的列表`[segments[i] for i in range(max_plane_idx)]`实际上是一个“列表推导式”🤔。它等同于编写一个`for`循环，将第一个元素`segments[i]`追加到列表中。方便的是，我们可以将`[rest]`添加到这个列表中，`draw.geometries()`方法会理解我们想要绘制一个点云。这不是很酷吗？

![](img/149829534a11885af97f3ff19dea8f8f.png)

DBSCAN 对 3D 点云的第一次扫描结果

哈！我们以为完成了……但我们真的完成了吗？你注意到这里有什么奇怪的地方吗？如果你仔细看，会发现一些奇怪的伪影，比如实际切割一些平面元素的“红色线条/平面”。为什么？🧐

![](img/1c67e5140011a746f7e66524ef332420.png)

实际上，由于我们将所有点拟合到 RANSAC 平面候选者（在欧几里得空间中没有限制范围）而不考虑点的密度连续性，因此根据平面的检测顺序，我们会出现这些“线”伪影。所以下一步是防止这种行为！为此，我建议在迭代过程中包含一个基于欧几里得聚类的条件，以在连续的簇中细化内点集。为此，我们将依赖于 DBSCAN 算法。

## 欧几里得聚类（DBSCAN）

在点云数据集中，我们经常需要将空间上连续的点集合（即在 3D 空间中物理上接近或相邻）进行分组，如下所示。但我们如何有效地做到这一点呢？

![](img/2d3ff3a05173c7b35dfab22f6883f9a2.png)

在这张图中，很明显我们想要将彼此接近的点分组，找到 5 组点。© F. Poux

DBSCAN（基于密度的空间聚类应用与噪声）算法于 1996 年为此目的而提出。该算法被广泛使用，因此在 2014 年获得了经得起时间考验的科学贡献奖。

DBSCAN 算法涉及扫描数据集中的每个点，并基于密度构建一个可达点集合。这是通过分析每个点的邻域并在其包含足够点时将其包含在区域内来实现的。这个过程对每个邻近点重复，直到簇无法再扩展。没有足够邻居的点被标记为噪声，使得该算法对离群点具有鲁棒性。这不是很令人印象深刻吗？😆

![](img/5f884ed20535463901f44913fbc652f1.png)

DBSCAN 算法过程和两个参数ϵ和 min_points 对结果的影响的示意图。你可以看到，值越大，组成的簇就越少。© F. Poux

啊，我们差点忘了。参数选择（ϵ用于邻域，n_min 用于最小点数）也可能很棘手：设置参数时必须小心，以确保创建足够的内部点（如果 n_min 太大或ϵ太小，就不会发生）。特别是，这意味着 DBSCAN 在发现不同密度的簇时会遇到困难。但 DBSCAN 有一个很大的优势，就是计算效率高，不需要像 Kmeans 那样预定义簇的数量。最后，它允许发现任意形状的簇。现在我们准备深入探讨参数的黑暗面 💻

## DBSCAN 用于 3D 点云聚类

让我详细说明逻辑过程（激活猛兽模式👹）。首先，我们需要定义运行 DBSCAN 的参数：

```py
epsilon = 0.15
min_cluster_points = 5
```

🌱 **成长**：*这些参数的定义是一个需要探索的问题。你必须找到一种方法来平衡过度分割和不足分割的问题。最终，你可以基于 RANSAC 的初始距离定义使用一些启发式方法。这是一个值得探索的方面* 😉。

在我们之前定义的 for 循环中，我们将在分配内点（`segments[i]=rest.select_by_index(inliers)`）后立即运行 DBSCAN，方法是紧接着添加以下一行：

```py
labels = np.array(segments[i].cluster_dbscan(eps=epsilon, min_points=min_cluster_points))
```

然后，我们将计算每个找到的簇包含多少个点，使用一种奇怪的符号表示法，该表示法利用了列表推导。结果存储在变量`candidates`中：

```py
candidates=[len(np.where(labels==j)[0]) for j in np.unique(labels)]
```

那现在呢？我们需要找到“最佳候选者”，通常是包含最多点的簇！为此，这里是这行代码：

```py
best_candidate=int(np.unique(labels)[np.where(candidates== np.max(candidates))[0]])
```

好吧，很多技巧在这里发生，但本质上，我们利用 Numpy 的熟练度来搜索并返回属于最大簇的点的索引。从这里开始，接下来的过程就简单了，我们只需确保在每次迭代中将任何剩余簇纳入后续 RANSAC 迭代中（🔥 推荐阅读 5 遍以消化！）：

```py
rest = rest.select_by_index(inliers, invert=True) + segments[i].select_by_index(list(np.where(labels!=best_candidate)[0]))
segments[i]=segments[i].select_by_index(list(np.where(labels== best_candidate)[0]))
```

🦚 **注意**：*`rest`* 变量现在确保包含 RANSAC 和 DBSCAN 剩余的点。当然，内点现在被筛选为原始 RANSAC 内点集中最大的簇*。

当循环结束时，你会得到一组干净的段，这些段包含空间上连续的点集，符合平面形状，你可以使用以下代码行以不同颜色可视化：

```py
colors = plt.get_cmap("tab20")(i)
segments[i].paint_uniform_color(list(colors[:3]))
```

结果应该类似于这个：

![](img/e91a792806f2bc948d988ded50f423c9.png)![](img/111103f10b8da9050953ae4423df66d6.png)

但这就结束了吗？不，不可能的 😄！一旦对点云应用了多阶 RANSAC 分割，下一阶段就是通过利用 DBSCAN 来细化剩余的未分割点，这有助于进一步提升点云分析的粒度，增加更多的簇。

🦚 **注意**：*粒度是一个在数据科学中经常使用的学术词汇。数据的粒度意味着数据组织或建模的详细程度。在这里，我们可以从点云中建模的对象越小，我们的表示的粒度就越细。（很高大上，对吧？！）*

# 7\. 欧几里得聚类精炼

![](img/664bd457a80f786f5d09aa8239aaeca7.png)

好了，是时候摆脱循环，处理分配给 `rest` 变量的剩余点，这些点尚未分配给任何段。让我们首先对我们讨论的内容有一个视觉上的了解：

```py
o3d.visualization.draw_geometries([rest])
```

![](img/76fdada74949c1d91e41829464bb87af.png)

我们可以使用 DBSCAN 进行简单的欧几里得聚类，并将结果捕获到一个 `labels` 变量中。你知道怎么做的：

```py
labels = np.array(pcd.cluster_dbscan(eps=0.1, min_points=10))
```

🌱 **生长：** *我们使用 10 cm 的半径来“生长”簇，只有在此步骤之后至少有 10 个点时才考虑一个簇。但这是正确的选择吗？* 🤔*可以随意实验以找到一个好的平衡点，理想情况下，找到一种自动化的方法*😀。

标签的范围在 `-1` 和 `n` 之间，其中 `-1` 表示“噪声”点，`0` 到 `n` 的值则是分配给相应点的簇标签。请注意，我们希望将标签作为 NumPy 数组获取。

![](img/3eb8a5f384d733521bb73466db0acea9.png)![](img/4f479ccf0472670ad4b5a84553201700.png)

左侧：参数定义不佳。右侧，我们对物体的轮廓有了更好的划分，以便进行后续处理。

很好。现在我们已经定义了每个点的标签组，让我们为结果着色。这是可选的，但对于迭代过程以搜索合适的参数值非常有用。为此，我们建议使用 Matplotlib 库获取特定的[颜色范围](https://matplotlib.org/stable/tutorials/colors/colormaps.html)，例如 tab20：

```py
max_label = labels.max()
colors = plt.get_cmap("tab20")(labels / (max_label 
if max_label > 0 else 1))
colors[labels < 0] = 0
rest.colors = o3d.utility.Vector3dVector(colors[:, :3])
o3d.visualization.draw_geometries([rest])
```

🦚 **注意**：*`max_label`* *应该是直观的：它存储标签列表中的最大值。这允许将其用作着色方案的分母，同时处理一个特殊情况，其中聚类被扭曲，仅产生噪声+一个簇。之后，我们确保将这些噪声点的标签设置为* `-1` *（黑色*（`0`）*）。*然后，我们将点云* `pcd` *的* `colors` *属性设置为表示 R、G、B 的 3 列的 2D NumPy 数组。*

就这样！我采用了与之前相同的方法，没有任何魔法！我只是确保使用一致的参数来进行精细的聚类，以获得你一直梦想的美丽彩虹场景 🥳！

![](img/0e7ebe11fc52adac83a2b93374501062.png)![](img/727dec15d27cdf31f1fcf08a86dd71b1.png)![](img/a4fd6c629333ca0ddd5943b6fd1c602b.png)

在使用 DBSCAN 精炼点云聚类结果之后，焦点转向体素化技术，这涉及将数据组织成有意义的空间结构，从而实现对点云信息的高效建模。

# 8\. 体素化和标记

![](img/b83044b78cead7000efc9944696472ca.png)

现在我们有了一个带有分段标签的点云，查看我们是否能适应室内建模工作流将会非常有趣。一种方法是使用体素来适应 O-Space 和 R-Space。通过将点云划分为小立方体，可以更容易地理解模型的占用和空白空间。让我们深入了解一下。

## 9.1\. 体素网格生成

创建准确且详细的空间 3D 模型意味着生成一个紧凑的体素网格。这种技术将点云划分为具有自己坐标系统的小立方体或体素。

![](img/6ddeb653af9fa95611230f33d792e393.png)

体素网格生成以结构化带有分段信息的 3D 点云。© F. Poux

要创建这样的结构，我们首先定义我们新实体的大小：体素：

```py
voxel_size=0.5
```

现在，我们想知道我们需要堆叠多少个这样的立方体才能填充由点云定义的边界框。这意味着我们首先需要计算点云的范围：

```py
min_bound = pcd.get_min_bound()
max_bound = pcd.get_max_bound()
```

很好，现在我们使用`o3d.geometry.VoxelGrid.create_from_point_cloud()`函数对任何选择的点云进行操作，以便在其上拟合体素网格。但是等等，我们想区分哪个点云以便进一步处理？

好的，让我们举例说明你想拥有“结构性”元素的体素与不属于结构性元素的杂物体素的情况。没有标签的情况下，我们可以根据它们是否属于 RANSAC 分段或其他分段来指导我们的选择；这意味着首先将 RANSAC 处理的分段进行拼接：

```py
pcd_ransac=o3d.geometry.PointCloud()
for i in segments:
 pcd_ransac += segments[i]
```

🦚 **注意**: *在这个阶段，你有能力用体素稍后拾取的均匀颜色为点云着色。如果这是你想要的，你可以使用：* `pcd_ransac.paint_uniform_color([1, 0, 0])`

然后，我们可以简单地提取我们的体素网格：

```py
voxel_grid_structural = o3d.geometry.VoxelGrid.create_from_point_cloud(pcd_ransac, voxel_size=voxel_size)
```

并对剩余元素做同样的处理：

```py
rest.paint_uniform_color([0.1, 0.1, 0.8])
voxel_grid_clutter = o3d.geometry.VoxelGrid.create_from_point_cloud(rest, voxel_size=voxel_size)
```

最后一步是可视化我们的结果：

```py
o3d.visualization.draw_geometries([voxel_grid_clutter,voxel_grid_structural])
```

![](img/a710cdc6698553309602a38d1349b361.png)

使用体素表示的空间的语义表示。© F. Poux

这太棒了！它看起来像那些整个世界由立方体构成的计算机游戏！而且我们实际上可以使用分段标签来指导我们的体素建模！这开启了许多视角！但问题依然存在。使用 Open3D，提取未填充的体素是困难的；因此，完成体素化点云的结构化后，下一步涉及探索体素空间建模技术，以提供分析体素化数据空间关系和属性的替代视角，为高级点云建模开辟新途径。

🤠 **Ville**: *体素化是对相同点云数据的不同空间表示。如果机器人需要避开碰撞，这种表示非常有用！比如在模拟…火灾演习时也非常有用！*

# 9\. 空间建模

![](img/8f45765461494fbd3c658993a1b48144.png)

在室内建模应用中，基于体素的点云表示在捕捉和分析复杂环境的几何属性方面起着关键作用。随着点云数据集的规模和复杂性的增加，深入探讨体素分割技术变得至关重要，以提取有意义的结构并促进更高层次的分析。因此，让我们定义一个函数来拟合体素网格，并返回填充和空白区域：

```py
def fit_voxel_grid(point_cloud, voxel_size, min_b=False, max_b=False):
# This is where we write what we want our function to do.
return voxel_grid, indices
```

在函数内部，我们将 (1) 确定点云的最小和最大坐标，(2) 计算体素网格的尺寸，(3) 创建一个空的体素网格，(4) 计算已占据体素的索引以及 (5) 将占据体素标记为 True。

![](img/c8495062282952b988f871455aef9eb1.png)

从点云创建占据网格的算法工作流程。© F. Poux

这转化为我们希望在此函数中包含的以下代码：

```py
# Determine the minimum and maximum coordinates of the point cloud
 if type(min_b) == bool or type(max_b) == bool:
   min_coords = np.min(point_cloud, axis=0)
   max_coords = np.max(point_cloud, axis=0)
 else:
   min_coords = min_b
   max_coords = max_b
 # Calculate the dimensions of the voxel grid
 grid_dims = np.ceil((max_coords - min_coords) / voxel_size).astype(int)
 # Create an empty voxel grid
 voxel_grid = np.zeros(grid_dims, dtype=bool)
 # Calculate the indices of the occupied voxels
 indices = ((point_cloud - min_coords) / voxel_size).astype(int)
 # Mark occupied voxels as True
 voxel_grid[indices[:, 0], indices[:, 1], indices[:, 2]] = True
```

我们已经定义了函数，现在可以使用它提取体素，按结构、杂乱和空体素进行分段：

```py
voxel_size = 0.3

#get the bounds of the original point cloud
min_bound = pcd.get_min_bound()
max_bound = pcd.get_max_bound()

ransac_voxels, idx_ransac = fit_voxel_grid(pcd_ransac.points, voxel_size, min_bound, max_bound)
rest_voxels, idx_rest = fit_voxel_grid(rest.points, voxel_size, min_bound, max_bound)

#Gather the filled voxels from RANSAC Segmentation
filled_ransac = np.transpose(np.nonzero(ransac_voxels))

#Gather the filled remaining voxels (not belonging to any segments)
filled_rest = np.transpose(np.nonzero(rest_voxels))

#Compute and gather the remaining empty voxels
total = pcd_ransac + rest
total_voxels, idx_total = fit_voxel_grid(total.points, voxel_size, min_bound, max_bound)
empty_indices = np.transpose(np.nonzero(~total_voxels))
```

🦚 **注意**：*NumPy 中的 `*nonzero()*`* 函数查找 `*ransac_voxels*` 变量中非零元素的索引。`*nonzero()*`* 函数返回一个数组的元组，每个数组对应于特定轴上非零元素的索引。然后，我们将 `*np.transpose()*`* NumPy 函数应用于从 `*np.nonzero(ransac_voxels)*`* 获得的结果。`*transpose()*`* 函数最终会排列数组的轴（有效地交换行和列）。通过结合这些操作，代码行对 `*ransac_voxels*`* 的非零元素索引进行转置，生成一个转置数组，其中每行表示原始 `*ransac_voxels*`* 数组中非零元素的坐标或索引。😊

这种体素建模方法提供了对体素化数据空间关系和属性的宝贵见解。为了在 Python 之外以透明度可视化结果，我们需要导出数据。

![](img/26688159b8fd845b7e77716526a41603.png)![](img/b5cf1042943b2491366570bf5785fea4.png)![](img/ac7654c650a1171f5b0bc0629506989d.png)

占据网格匹配的结果。左侧是填充体素和空体素，中间是填充体素，右侧是空体素。© F. Poux

在实现体素化点云的结构化后，下一步涉及将点云和体素数据导出为外部格式，以便实现互操作性，并与其他软件工具和工作流程无缝集成。此导出过程确保结构化的体素数据和原始点云可以轻松共享、可视化或用于进一步分析，从而促进对点云数据利用的协作和多功能方法。

# 10\. 导出 3D 数据集

![](img/61eb9b7f6baa27577836ecc3290f343b.png)

让我们首先关注导出点云分割数据集。

## 10.1\. 分割点云导出

要导出分割后的点云，我们必须确保可以在可读的 ASCII 文件中为每个点写入标签。为此，我们将创建一个 XYZ 片段列表，并将标签特征附加到该列表。这可以通过以下`for`循环完成：

```py
xyz_segments=[]
for idx in segments:
 print(idx,segments[idx])
 a = np.asarray(segments[idx].points)
 N = len(a)
 b = idx*np.ones((N,3+1))
 b[:,:-1] = a
 xyz_segments.append(b)
```

从那里，我们不想忘记 DBSCAN 聚类中的剩余元素，对其应用相同的原则：

```py
rest_w_segments=np.hstack((np.asarray(rest.points),(labels+max_plane_idx).reshape(-1, 1)))
```

最后，我们将其附加到片段列表中：

```py
 xyz_segments.append(rest_w_segments)
```

然后我们所需要做的就是使用 numpy 导出数据集并进行外部可视化：

```py
np.savetxt("../RESULTS/" + DATANAME.split(".")[0] + ".xyz", np.concatenate(xyz_segments), delimiter=";", fmt="%1.9f")
```

![](img/9a6913b5b3158a617aa100ac50f293b6.png)![](img/3f317d5083906d7a4c6ef6e961bc04df.png)![](img/3d6ef345ce0c619c4c2e337ea05070c8.png)![](img/678224d1204a44582879919bd30270e1.png)![](img/a40abe9f177b691d00b9bc7991871a61.png)

在成功导出分割后的点云数据集后，现在的重点是将体素数据集导出为`.obj`文件。

## 10.2\. 体素模型导出

要导出体素，我们首先必须为每个体素生成一个立方体；这些立方体然后在`voxel_assembly`中组合在一起，堆叠生成最终文件。我们创建了`voxel_modelling(filename, indices, voxel_size)`函数来完成这个任务：

```py
def voxel_modelling(filename, indices, voxel_size):
    voxel_assembly=[]
    with open(filename, "a") as f:
        cpt = 0
        for idx in indices:
            voxel = cube(idx,voxel_size,cpt)
            f.write(f"o {idx}  \n")
            np.savetxt(f, voxel,fmt='%s')
            cpt += 1
            voxel_assembly.append(voxel)
    return voxel_assembly
```

🦚 **注意**：*函数* `[cube()](https://drive.google.com/file/d/1kPu85YHl66gQH8Qumxlyd-Sp4PjgvBVm/view?usp=sharing)` *读取提供的索引，根据索引和体素大小生成体素立方体，将体素立方体写入文件，并跟踪在* `*voxel_assembly*` *列表中的组装体素立方体，最终由函数返回。*

然后用来导出三种不同的体素组合：

```py
vrsac = voxel_modelling("../RESULTS/ransac_vox.obj", filled_ransac, 1)
vrest = voxel_modelling("../RESULTS/rest_vox.obj", filled_rest, 1)
vempty = voxel_modelling("../RESULTS/empty_vox.obj", empty_indices, 1)
```

![](img/2d627d5ec536ba3c638629a25ce4f5f5.png)![](img/7860e71bb83efd674302b5c044716254.png)![](img/442975beca59c66312b3cdbbcfc21310.png)

分割和体素建模的结果。© F. Poux

+   💻 在这里获取代码访问权限：[代码示例](https://drive.google.com/drive/folders/1-sGlVvsPcyp9VZ8cw-J8kbhj4-aK_8p8?usp=sharing)

+   🍇 在这里获取数据访问权限：[3D 数据集](https://drive.google.com/drive/folders/1sCBT1lc9A8Zn4grpxwFrBrvos86c0HZR?usp=sharing)

+   👨‍🏫3D 数据处理和 AI 课程：[3D 学院](https://learngeodata.eu/)

+   📖 订阅以获得 3D 教程的提前访问权限：[3D AI 自动化](https://medium.com/@florentpoux/subscribe)

![](img/ceab061ce5f1ea698c109d269cc41fd2.png)

# 结论

🦊 **Florent**：大规模祝贺🎉！你刚刚学会了如何开发一个自动形状检测、聚类、体素化和室内建模程序，用于处理由数百万个点组成的 3D 点云，并采用不同的策略！真心地，做得很好！但道路显然不止于此，因为你刚刚解锁了一个巨大的智能过程潜力，能够在片段级别进行推理！

🤠 **Ville**：那么，你是否在想我们是否可以让 3D 建模成为完全无需人工干预的过程？凭借我们目前的技术，我们已经完成了一半。为什么？我们的技术可以找到，例如，平面模型的参数。这就是为什么学术界的人称之为参数建模。然而，我们仍然需要仔细选择其他一些参数，比如 RANSAC 的参数。我鼓励你尝试在不同的点云上应用你的代码！

![](img/4b6c35c1665bd5dbdfac5fc7031fb72a.png)

基于地面语义的 3D 网格示例。© F. Poux

# 更进一步

学习之旅并未结束。我们的终身探索才刚刚开始，未来的步骤将深入到 3D Voxel 工作，探索语义和点云分析与深度学习技术。我们将解锁高级的 3D LiDAR 分析工作流程。令人兴奋的事情还有很多！

1.  **Lehtola, V.**，Nikoohemat, S.，& Nüchter, A.（2020）。室内 3D：扫描和重建方法概述。大地空间数据手册，55–97\. [`doi.org/10.1007/978-3-030-55462-0_3`](https://doi.org/10.1007/978-3-030-55462-0_3)

1.  **Poux, F.**，& Billen, R.（2019）。基于体素的 3D 点云语义分割：无监督几何和关系特征与深度学习方法。*ISPRS 国际地理信息杂志*。8(5)，213；[`doi.org/10.3390/ijgi8050213`](https://doi.org/10.3390/ijgi8050213) — Jack Dangermond 奖（[新闻报道链接](https://www.geographie.uliege.be/cms/c_5724437/en/florent-poux-and-roland-billen-winners-of-the-2019-jack-dangermond-award)）

1.  Bassier, M., Vergauwen, M., **Poux, F.**，（2020）。建筑内部分类中的点云与网格特征。*遥感*。12，2224\. https://doi:10.3390/rs12142224
