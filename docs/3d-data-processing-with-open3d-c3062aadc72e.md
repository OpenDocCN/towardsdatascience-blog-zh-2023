# 使用Open3D进行3D数据处理

> 原文：[https://towardsdatascience.com/3d-data-processing-with-open3d-c3062aadc72e?source=collection_archive---------1-----------------------#2023-05-08](https://towardsdatascience.com/3d-data-processing-with-open3d-c3062aadc72e?source=collection_archive---------1-----------------------#2023-05-08)

## 使用Python的Open3D库处理3D模型的简洁指南（附带互动式Jupyter Notebook）

[](https://medium.com/@prerak12.agarwal?source=post_page-----c3062aadc72e--------------------------------)[![Prerak Agarwal](../Images/631f00675c83812d32b983fb41b41edf.png)](https://medium.com/@prerak12.agarwal?source=post_page-----c3062aadc72e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c3062aadc72e--------------------------------)[![数据科学之路](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c3062aadc72e--------------------------------) [Prerak Agarwal](https://medium.com/@prerak12.agarwal?source=post_page-----c3062aadc72e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F885f74d5a8c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3d-data-processing-with-open3d-c3062aadc72e&user=Prerak+Agarwal&userId=885f74d5a8c&source=post_page-885f74d5a8c----c3062aadc72e---------------------post_header-----------) 发表在 [数据科学之路](https://towardsdatascience.com/?source=post_page-----c3062aadc72e--------------------------------) ·13分钟阅读·2023年5月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc3062aadc72e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3d-data-processing-with-open3d-c3062aadc72e&user=Prerak+Agarwal&userId=885f74d5a8c&source=-----c3062aadc72e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc3062aadc72e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3d-data-processing-with-open3d-c3062aadc72e&source=-----c3062aadc72e---------------------bookmark_footer-----------)

在这篇文章中，我提供了一个简洁的指南，讲解如何使用Python的[Open3D](http://www.open3d.org/docs/release/index.html)库来探索、处理和可视化3D模型——这是一个开源的3D数据处理库。

![](../Images/d71b3cf9d03ac81a2e4fbbcc7784b555.png)

使用Open3D可视化的3D模型（原始3D模型可在[这里](https://sketchfab.com/3d-models/tesla-model-s-plaid-9de8855fae324e6cbbb83c9b5288c961)找到）。

如果你考虑处理 3D 数据/模型以进行特定任务，如训练 AI 模型进行 3D 模型分类和/或分割，你可能会发现这个讲解很有帮助。互联网上的 3D 模型（如 [ShapeNet](https://shapenet.org/) 数据集中）有多种格式，如 *.obj*、*.glb*、*.gltf* 等。使用像 Open3D 这样的库，这些模型可以轻松处理、可视化并转换为其他格式，例如更易于理解和解释的点云。

本文也提供了 Jupyter Notebook 版本，适合那些希望跟随并在本地运行代码的读者。包含 Jupyter Notebook 及所有其他数据和资产的 zip 文件可以从以下链接下载。

[## 3D 数据处理与 Open3D.zip

### 包含 Jupyter Notebook、3D 模型及所有其他必要文件的 zip 文件。

drive.google.com](https://drive.google.com/file/d/1290xG3_BEYn9WN9TwkYKMmNbNRsrWjGk/view?usp=share_link&source=post_page-----c3062aadc72e--------------------------------)

在本教程中，我将介绍以下任务：

1.  **加载和可视化一个 3D 模型作为 *网格***

1.  **通过采样点将 *网格* 转换为 *点云***

1.  **从 *点云* 中移除隐藏点***

1.  **将 *点云* 转换为数据框**

1.  **保存 *点云* 和数据框**

让我们先导入所有必要的库：

```py
# Importing open3d and all other necessary libraries.

import open3d as o3d
import os
import copy
import numpy as np
import pandas as pd
from PIL import Image

np.random.seed(42)
```

```py
# Checking the installed version on open3d.

o3d.__version__
# Open3D version used in this exercise: 0.16.0
```

# 加载和可视化一个 3D 模型作为 *网格*

可以通过运行以下代码行将 3D 模型读取为网格：

```py
# Defining the path to the 3D model file.
mesh_path = "data/3d_model.obj"

# Reading the 3D model file as a 3D mesh using open3d.
mesh = o3d.io.read_triangle_mesh(mesh_path)
```

要可视化网格，请运行以下代码行：

```py
# Visualizing the mesh.
draw_geoms_list = [mesh]
o3d.visualization.draw_geometries(draw_geoms_list)
```

网格应在新窗口中打开，呈现出下图所示的样子（请注意，网格以静态图像形式打开，而不是这里展示的动画图像）。可以使用鼠标指针旋转网格图像。

![](../Images/70991296dafd91cf39eca3039cabb112.png)

3D 模型作为网格可视化（**在**估计表面法线之前）。

如上所示，汽车网格看起来不像典型的 3D 模型，且被涂成均匀的灰色。这是因为网格没有关于 3D 模型中顶点和表面的 *法线* 信息。

**什么是 *法线*？** — 在给定点的表面上的法线向量是垂直于该点表面的向量。法线向量通常被称为“*法线*”。要了解更多相关内容，可以参考这两个链接：[法线向量](https://mathworld.wolfram.com/NormalVector.html) 和 [点云中的表面法线估计](https://pcl.readthedocs.io/en/latest/normal_estimation.html#)。

可以通过运行以下代码行来估计上述 3D 网格的 *法线*：

```py
# Computing the normals for the mesh.
mesh.compute_vertex_normals()

# Visualizing the mesh.
draw_geoms_list = [mesh]
o3d.visualization.draw_geometries(draw_geoms_list)
```

一旦可视化，网格应如下面图像所示。计算 *法线* 后，汽车模型将正确渲染，看起来像一个 3D 模型。

![](../Images/4118a351aecbd1187a8df69c2276047e.png)

3D模型可视化为网格（**估算表面法线后**）。

让我们创建一个XYZ坐标系，以了解这个车模在欧几里得空间中的方向。XYZ坐标系可以覆盖在上面的3D网格上，通过运行以下代码行进行可视化：

```py
# Creating a mesh of the XYZ axes Cartesian coordinates frame.
# This mesh will show the directions in which the X, Y & Z-axes point,
# and can be overlaid on the 3D mesh to visualize its orientation in
# the Euclidean space.
# X-axis : Red arrow
# Y-axis : Green arrow
# Z-axis : Blue arrow
mesh_coord_frame = o3d.geometry.TriangleMesh.create_coordinate_frame(size=5, origin=[0, 0, 0])

# Visualizing the mesh with the coordinate frame to understand the orientation.
draw_geoms_list = [mesh_coord_frame, mesh]
o3d.visualization.draw_geometries(draw_geoms_list)
```

![](../Images/04e234bb2232060711c09961583c0ff9.png)

3D网格可视化带有XYZ坐标系。X轴：红色箭头，Y轴：绿色箭头，Z轴：蓝色箭头 [容易记住的方式 — XYZ::RGB]

从上述可视化中，我们可以看到这辆车的网格方向如下：

+   XYZ轴的**原点**：**在车模的体积中心**（*在上面的图像中未见，因为它在车模网格内部*）。

+   **X轴**（红色箭头）：沿着汽车的**长度维度**，正X轴指向汽车的引擎盖（*在上面的图像中未见，因为它在车模网格内部*）。

+   **Y轴**（绿色箭头）：沿着汽车的**高度维度**，正Y轴指向汽车的车顶。

+   **Z轴**（蓝色箭头）：沿着汽车的**宽度维度**，正Z轴指向车的右侧。

现在我们来看看这个车模内部的情况。为此，我们将沿Z轴裁剪网格，并移除车的右半部分（正Z轴）。

```py
# Cropping the car mesh using its bouding box to remove its right half (positive Z-axis).
bbox = mesh.get_axis_aligned_bounding_box()
bbox_points = np.asarray(bbox.get_box_points())
bbox_points[:, 2] = np.clip(bbox_points[:, 2], a_min=None, a_max=0)
bbox_cropped = o3d.geometry.AxisAlignedBoundingBox.create_from_points(o3d.utility.Vector3dVector(bbox_points))
mesh_cropped = mesh.crop(bbox_cropped)

# Visualizing the cropped mesh.
draw_geoms_list = [mesh_coord_frame, mesh_cropped]
o3d.visualization.draw_geometries(draw_geoms_list)
```

![](../Images/c66e1e5fba2f67f19c913c3c686aee6e.png)

在Z轴上裁剪的3D网格，右半部分的车被移除（正Z轴）。裁剪后的网格展示了这个3D车模的详细内部结构。

从上述可视化中，我们看到这个车模有详细的内部结构。现在我们已经看到这个3D网格内部的内容，我们可以在移除属于车内部的“隐藏”点之前，将其转换为点云。

# 通过采样点将*网格*转换为*点云*

在Open3D中，将网格转换为点云可以很容易地完成，通过定义我们希望从网格中采样的点的数量。

```py
# Uniformly sampling 100,000 points from the mesh to convert it to a point cloud.
n_pts = 100_000
pcd = mesh.sample_points_uniformly(n_pts)

# Visualizing the point cloud.
draw_geoms_list = [mesh_coord_frame, pcd]
o3d.visualization.draw_geometries(draw_geoms_list)
```

![](../Images/3489992121f80d4da3c30e9043befd92.png)

从3D网格中均匀采样的100,000个点创建的3D点云。

请注意，上面的点云中的颜色仅指示点在Z轴上的位置。

如果我们像对网格一样裁剪点云，它会是这样的：

```py
# Cropping the car point cloud using bounding box to remove its right half (positive Z-axis).
pcd_cropped = pcd.crop(bbox_cropped)

# Visualizing the cropped point cloud.
draw_geoms_list = [mesh_coord_frame, pcd_cropped]
o3d.visualization.draw_geometries(draw_geoms_list)
```

![](../Images/656d506ef643e3a682cf68cb60597adf.png)

在Z轴上裁剪的3D点云，右半部分的车被移除（正Z轴）。与上面的裁剪网格类似，裁剪后的点云也展示了这个3D车模的详细内部结构。

我们在裁剪后的点云的可视化中看到，它还包含了属于车模内部的点。这是预期的，因为这个点云是通过从整个网格中均匀采样点创建的。在下一部分中，我们将移除这些属于车内部并不在点云外表面的“隐藏”点。

# 从*点云*中移除隐藏点

想象你将光线照射到汽车模型的右侧。所有落在 3D 模型右侧外表面的点都会被照亮，而点云中的其他点则不会。

![](../Images/b9c54a72522967a3ed2ccfb7c22c4579.png)

插图展示了 Open3D 的隐藏点移除功能如何在从给定视角查看的点云上工作。所有被照亮的点被认为是“可见”的，而所有其他点被认为是“隐藏”的。

现在让我们将这些被照亮的点标记为“可见”，所有未被照亮的点标记为“隐藏”。这些“隐藏”的点还包括所有属于汽车内部的点。

该操作在 Open3D 中称为*隐藏点移除*。为了在点云上执行此操作，使用 Open3D，运行以下代码行：

```py
# Defining the camera and radius parameters for the hidden point removal operation.
diameter = np.linalg.norm(np.asarray(pcd.get_min_bound()) - np.asarray(pcd.get_max_bound()))
camera = [0, 0, diameter]
radius = diameter * 100

# Performing the hidden point removal operation on the point cloud using the
# camera and radius parameters defined above.
# The output is a list of indexes of points that are visible.
_, pt_map = pcd.hidden_point_removal(camera, radius)
```

使用上述输出的可见点索引列表，我们可以在可视化点云之前，用不同的颜色标记可见点和隐藏点。

```py
# Painting all the visible points in the point cloud in blue, and all the hidden points in red.

pcd_visible = pcd.select_by_index(pt_map)
pcd_visible.paint_uniform_color([0, 0, 1])    # Blue points are visible points (to be kept).
print("No. of visible points : ", pcd_visible)

pcd_hidden = pcd.select_by_index(pt_map, invert=True)
pcd_hidden.paint_uniform_color([1, 0, 0])    # Red points are hidden points (to be removed).
print("No. of hidden points : ", pcd_hidden)

# Visualizing the visible (blue) and hidden (red) points in the point cloud.
draw_geoms_list = [mesh_coord_frame, pcd_visible, pcd_hidden]
o3d.visualization.draw_geometries(draw_geoms_list)
```

![](../Images/f390adb4db7eae032618205b1879c1a0.png)

从上图所示的相机视角下的隐藏点移除操作后的点云。蓝色为“可见”点，而红色为“隐藏”点。

从上面的可视化中，我们可以看到隐藏点移除操作如何在给定的相机视角下工作。该操作从给定的相机视角下消除所有被前景点遮挡的背景点。

为了更好地理解这一点，我们可以再次重复相同的操作，但这次在轻微旋转点云之后。**实际上，我们在这里尝试改变视角。但不是通过重新定义相机参数来改变视角，而是通过旋转点云本身。**

```py
# Defining a function to convert degrees to radians.
def deg2rad(deg):
    return deg * np.pi/180

# Rotating the point cloud about the X-axis by 90 degrees.
x_theta = deg2rad(90)
y_theta = deg2rad(0)
z_theta = deg2rad(0)
tmp_pcd_r = copy.deepcopy(pcd)
R = tmp_pcd_r.get_rotation_matrix_from_axis_angle([x_theta, y_theta, z_theta])
tmp_pcd_r.rotate(R, center=(0, 0, 0))

# Visualizing the rotated point cloud.
draw_geoms_list = [mesh_coord_frame, tmp_pcd_r]
o3d.visualization.draw_geometries(draw_geoms_list)
```

![](../Images/78ac4406b6279930d1595b81f570d332.png)

3D 点云**绕 X 轴旋转了 90 度**。注意，现在，与之前不同的是，**Y 轴**（绿色箭头）沿着汽车的**宽度方向**，而 **Z 轴**（蓝色箭头）沿着汽车的**高度方向**。X 轴（红色箭头）没有变化，仍然沿着汽车的长度方向。

![](../Images/7c39065ff5c12fb88d444f91e97b3920.png)

插图展示了隐藏点移除操作如何在从之前相同视角查看的旋转点云上工作。如前所述，所有被照亮的点被认为是“可见”的，而所有其他点被认为是“隐藏”的。

通过在旋转的汽车模型上重复相同的过程，我们将看到这次所有落在 3D 模型上表面（汽车的车顶）的点会被照亮，而点云中的其他点则不会。

我们可以通过运行以下代码行来重复对旋转点云的隐藏点移除操作：

```py
# Performing the hidden point removal operation on the rotated point cloud
# using the same camera and radius parameters defined above.
# The output is a list of indexes of points that are visible.
_, pt_map = tmp_pcd_r.hidden_point_removal(camera, radius)

# Painting all the visible points in the rotated point cloud in blue,
# and all the hidden points in red.

pcd_visible = tmp_pcd_r.select_by_index(pt_map)
pcd_visible.paint_uniform_color([0, 0, 1])    # Blue points are visible points (to be kept).
print("No. of visible points : ", pcd_visible)

pcd_hidden = tmp_pcd_r.select_by_index(pt_map, invert=True)
pcd_hidden.paint_uniform_color([1, 0, 0])    # Red points are hidden points (to be removed).
print("No. of hidden points : ", pcd_hidden)

# Visualizing the visible (blue) and hidden (red) points in the rotated point cloud.
draw_geoms_list = [mesh_coord_frame, pcd_visible, pcd_hidden]
o3d.visualization.draw_geometries(draw_geoms_list)
```

![](../Images/eb03d7895f7075a8b11f7c162fe8a8d5.png)

从上图所示的相机视角隐藏点去除操作后的旋转点云。再次，可见的点为蓝色，而隐藏的点为红色。

上述旋转点云的可视化清楚地说明了隐藏点去除操作的工作原理。因此，为了从这个汽车点云中删除所有的“隐藏”点，我们可以通过依次绕三个轴线将点云略微旋转从 -90 度到 +90 度来**顺序执行这个隐藏点去除操作**。在每次隐藏点去除操作之后，我们可以聚合出点的索引输出列表。**在所有隐藏点去除操作之后，点的聚合索引列表将包含所有未隐藏的点（即，位于点云外表面上的点）。**以下代码执行这个顺序隐藏点去除操作：

```py
# Defining a function to rotate a point cloud in X, Y and Z-axis.
def get_rotated_pcd(pcd, x_theta, y_theta, z_theta):
    pcd_rotated = copy.deepcopy(pcd)
    R = pcd_rotated.get_rotation_matrix_from_axis_angle([x_theta, y_theta, z_theta])
    pcd_rotated.rotate(R, center=(0, 0, 0))
    return pcd_rotated

# Defining a function to get the camera and radius parameters for the point cloud
# for the hidden point removal operation.
def get_hpr_camera_radius(pcd):
    diameter = np.linalg.norm(np.asarray(pcd.get_min_bound()) - np.asarray(pcd.get_max_bound()))
    camera = [0, 0, diameter]
    radius = diameter * 100
    return camera, radius

# Defining a function to perform the hidden point removal operation on the
# point cloud using the camera and radius parameters defined earlier.
# The output is a list of indexes of points that are not hidden.
def get_hpr_pt_map(pcd, camera, radius):
    _, pt_map = pcd.hidden_point_removal(camera, radius)    
    return pt_map
```

```py
# Performing the hidden point removal operation sequentially by rotating the
# point cloud slightly in each of the three axes from -90 to +90 degrees,
# and aggregating the list of indexes of points that are not hidden after
# each operation.

# Defining a list to store the aggregated output lists from each hidden
# point removal operation.
pt_map_aggregated = []

# Defining the steps and range of angle values by which to rotate the point cloud.
theta_range = np.linspace(-90, 90, 7)

# Counting the number of sequential operations.
view_counter = 1
total_views = theta_range.shape[0] ** 3

# Obtaining the camera and radius parameters for the hidden point removal operation.
camera, radius = get_hpr_camera_radius(pcd)

# Looping through the angle values defined above for each axis.
for x_theta_deg in theta_range:
    for y_theta_deg in theta_range:
        for z_theta_deg in theta_range:

            print(f"Removing hidden points - processing view {view_counter} of {total_views}.")

            # Rotating the point cloud by the given angle values.
            x_theta = deg2rad(x_theta_deg)
            y_theta = deg2rad(y_theta_deg)
            z_theta = deg2rad(z_theta_deg)
            pcd_rotated = get_rotated_pcd(pcd, x_theta, y_theta, z_theta)

            # Performing the hidden point removal operation on the rotated
            # point cloud using the camera and radius parameters defined above.
            pt_map = get_hpr_pt_map(pcd_rotated, camera, radius)

            # Aggregating the output list of indexes of points that are not hidden.
            pt_map_aggregated += pt_map

            view_counter += 1

# Removing all the duplicated points from the aggregated list by converting it to a set.
pt_map_aggregated = list(set(pt_map_aggregated))
```

```py
# Painting all the visible points in the point cloud in blue, and all the hidden points in red.

pcd_visible = pcd.select_by_index(pt_map_aggregated)
pcd_visible.paint_uniform_color([0, 0, 1])    # Blue points are visible points (to be kept).
print("No. of visible points : ", pcd_visible)

pcd_hidden = pcd.select_by_index(pt_map_aggregated, invert=True)
pcd_hidden.paint_uniform_color([1, 0, 0])    # Red points are hidden points (to be removed).
print("No. of hidden points : ", pcd_hidden)

# Visualizing the visible (blue) and hidden (red) points in the point cloud.
draw_geoms_list = [mesh_coord_frame, pcd_visible, pcd_hidden]
# draw_geoms_list = [mesh_coord_frame, pcd_visible]
# draw_geoms_list = [mesh_coord_frame, pcd_hidden]
o3d.visualization.draw_geometries(draw_geoms_list)
```

![](../Images/43f28acf371940f0bdaa0586e51c7fff.png)

从相同相机视角经所有连续隐藏点去除操作后的点云。聚合的“可见”点（即，点云外表面上的点）为蓝色，而“隐藏”点（即，不在点云外表面上的点）为红色。

让我们再次裁剪点云，看看属于汽车内部的点。

```py
# Cropping the point cloud of visible points using bounding box defined
# earlier to remove its right half (positive Z-axis).
pcd_visible_cropped = pcd_visible.crop(bbox_cropped)

# Cropping the point cloud of hidden points using bounding box defined
# earlier to remove its right half (positive Z-axis).
pcd_hidden_cropped = pcd_hidden.crop(bbox_cropped)

# Visualizing the cropped point clouds.
draw_geoms_list = [mesh_coord_frame, pcd_visible_cropped, pcd_hidden_cropped]
o3d.visualization.draw_geometries(draw_geoms_list)
```

![](../Images/df3b98c5c29de1be0d3e19aa573568da.png)

经所有连续隐藏点去除操作后的裁剪点云，显示了所有属于 3D 汽车模型内部的“隐藏”点，标记为红色。

从上述经隐藏点去除操作后的点云可视化中，我们看到所有属于汽车模型内部（红色）的“隐藏”点现在与位于点云外表面（蓝色）的“可见”点分离开来。

# 将*点云*转换为数据框

如预期的那样，点云中每个点的位置可以用三个数值来定义 — *X、Y 和 Z 坐标*。然而，请回忆上面的部分，我们还为 3D 网格中的每个点估计了表面法线。当我们从这个网格中取样点以创建点云时，点云中的每个点还包含与这些表面法线相关的三个附加属性 — *X、Y 和 Z 方向的法线单位向量坐标*。

因此，为了将点云转换为数据框，**点云中的每个点可以通过以下七个属性列在单独的行中表示**：

1.  **X 坐标** (*浮点数*)

1.  **Y 坐标** (*浮点数*)

1.  **Z 坐标** (*浮点数*)

1.  **X 方向法向量坐标** (*浮点数*)

1.  **Y 方向法向量坐标** (*浮点数*)

1.  **Z 方向法向量坐标** (*浮点数*)

1.  **点可见性** (*布尔值 True 或 False*)

通过运行以下代码行，可以将点云转换为数据框：

```py
# Creating a dataframe for the point cloud with the X, Y & Z positional coordinates
# and the normal unit vector coordinates in the X, Y & Z directions of all points.
pcd_df = pd.DataFrame(np.concatenate((np.asarray(pcd.points), np.asarray(pcd.normals)), axis=1),
                      columns=["x", "y", "z", "norm-x", "norm-y", "norm-z"]
                     )

# Adding a column to indicate whether the point is visible or not using the aggregated
# list of indexes of points from the hidden point removal operation above.
pcd_df["point_visible"] = False
pcd_df.loc[pt_map_aggregated, "point_visible"] = True
```

这将返回如下所示的数据框，其中每个点都是由上面解释的七个属性列表示的一行。

![](../Images/8c803e2c89581a325d6cfcda32e90ab1.png)

3D 点云转换为数据框。

# 保存*点云*和数据框

点云（隐藏点移除前和后）以及数据框现在可以通过运行以下代码行来保存：

```py
# Saving the entire point cloud as a .pcd file.
pcd_save_path = "data/3d_model.pcd"
o3d.io.write_point_cloud(pcd_save_path, pcd)

# Saving the point cloud with the hidden points removed as a .pcd file.
pcd_visible_save_path = "data/3d_model_hpr.pcd"
o3d.io.write_point_cloud(pcd_visible_save_path, pcd_visible)

# Saving the point cloud dataframe as a .csv file.
pcd_df_save_path = "data/3d_model.csv"
pcd_df.to_csv(pcd_df_save_path, index=False)
```

![](../Images/ac1332b3826c2f4143e32746af5f5131.png)

3D 模型（上图：完整，下图：裁剪）可视化为 1. 网格，2. 点云和 3. 隐藏点移除后的点云。

就这些！希望本次演示能让你对如何在 Python 中处理 3D 数据有一点清晰的了解。如果你有任何问题或对如何更好地完成这些任务有建议，请告诉我。如果你有需要使用 3D 数据的有趣应用想法，请随时联系我——我很高兴能交流并分享类似的兴趣！同时，也欢迎在 [LinkedIn](https://www.linkedin.com/in/prerakagarwal/) 上与我联系。

本次演示中使用的 3D 汽车模型已从原始文件中稍作修改，以适应本练习的需要。感谢原作者 — *“Tesla Model S Plaid”*（[*https://skfb.ly/oEqT9*](https://skfb.ly/oEqT9)）由 ValentunW 创作，使用了 Creative Commons Attribution 许可协议（[*http://creativecommons.org/licenses/by/4.0/*](http://creativecommons.org/licenses/by/4.0/)）。
