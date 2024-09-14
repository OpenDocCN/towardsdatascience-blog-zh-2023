# 使用 Matplotlib 创建 3-D 银河艺术

> 原文：[`towardsdatascience.com/create-3-d-galactic-art-with-matplotlib-a7534148a319`](https://towardsdatascience.com/create-3-d-galactic-art-with-matplotlib-a7534148a319)

## 更有趣的对数螺旋

[](https://medium.com/@lee_vaughan?source=post_page-----a7534148a319--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----a7534148a319--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a7534148a319--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7534148a319--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----a7534148a319--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a7534148a319--------------------------------) ·阅读时间 11 分钟·2023 年 10 月 4 日

--

![](img/a088a775770e8db4d621ccb4aa4398ed.png)

螺旋星系的 3-D 模拟（作者制作）

在[之前的一篇文章](https://medium.com/towards-data-science/create-galactic-art-with-tkinter-e0418a59b215)中，我展示了如何使用 Python 的 Tkinter GUI 模块，通过对数螺旋的方程制作*2-D*银河艺术。在这篇文章中，我们将进一步深入，利用 Python 的主要绘图库 matplotlib，生成吸引人的*3-D*螺旋星系模拟。除了有趣，这个项目也是向学生展示对数螺旋概念的绝佳方式。

# 对数螺旋的极坐标方程

*对数*螺旋是自然物体几何生长时产生的。一些例子包括银河系、飓风和松果。在银河系中，这些螺旋由星星密集的*螺旋臂*表示。

由于螺旋从中心点或*极点*辐射出来，它们可以使用*极坐标*绘制。在这个系统中，更熟悉的笛卡尔坐标系中的（*x, y*）坐标被（*r,* Ɵ）所替代，其中*r*是距离极点（0, 0）的距离，而Ɵ（theta）是从 x 轴逆时针测量的角度。

![](img/e69f4225cac1548df350025c279c3e95.png)

示例极坐标系统（摘自《不切实际的 Python 项目》[2]）

使用这些术语，对数螺旋的极坐标方程是：

![](img/b58ebfab4af1e7c476c69a98524a3767.png)

其中*e*是自然对数的底数，*a*是控制大小的缩放因子，*b*是控制螺旋“开放度”和生长方向的增长因子。

# 编程策略

如前一篇文章所述，我们将通过使用对数方程绘制一个螺旋，然后旋转并重新绘制螺旋三次，以模拟一个四臂螺旋星系。为了模拟星系的中央核心，我们将点随机分布在一个球体内。为了捕捉星系的“背景光辉”，我们将随机分布小“恒星”在星系盘周围。

最后，为了使一切呈现三维效果，我们将随机移动星系盘中的恒星上下，然后使用 matplotlib 在 3-D 图表中显示结果。

# 安装 Matplotlib

我们将需要 matplotlib 来完成这个项目。你可以通过以下两种方式之一来安装它：

`conda install matplotlib`

或

`pip install matplotlib`

# 代码

以下代码在 JupyterLab 中编写，并由单元格描述。你可以从这个[Gist](https://gist.github.com/rlvaugh/a49bf875890581f338a000c2b5c3a2bb)下载完整的脚本。

## 导入库并设置显示

为了处理对数螺旋方程和随机移动恒星位置，我们将使用`math`、`random`和`numpy`库。绘图时，我们将使用`matplotlib`。

我们将结果显示在*Qt*窗口中，该窗口将快速更新，并允许我们缩放、平移、保存等。由于我们在外层空间工作，我们将设置绘图的背景样式为黑色。

最后，我们将用一个名为`SCALE`的常量表示我们的星系盘的半径。200 到 700 之间的值将得到最佳结果。你可以通过使用银河系作为参考，将这些值与现实世界单位相关联。银河系的半径为 50,000 光年。因此，使用`SCALE`值为 500 意味着你屏幕上的每个像素将等于 50,000/500 = 100 光年。

```py
import math
from random import randint, uniform, random
import numpy as np
import matplotlib.pyplot as plt

plt.style.use('dark_background')
%matplotlib qt

# Set the radius of the galactic disc (scaling factor):
SCALE = 350  # Use range of 200 - 700.
```

## 定义构建螺旋臂的函数

接下来，我们将定义一个函数，通过对数螺旋方程构建单个螺旋臂。这个方程将绘制一条弯曲的线，但我们将通过改变恒星的大小和位置（“模糊”），并通过为每个臂复制螺旋并稍微向后移动同时缩小其恒星，来“丰满”它。这将创建一个明亮的“前缘”和一个暗淡的“后缘”。

![](img/7ec00614194a6ad6abf54fb026c9439a.png)

通过移动螺旋和改变恒星位置来填充螺旋臂（来自《不切实际的 Python 项目》 [2]）

对数螺旋方程输出二维坐标点。为了给我们的星系增加 3-D 的“高度”，我们将创建一个`z`变量并赋予它一个根据`SCALE`常量调整到星系大小的随机值。

该函数将返回一个单个螺旋臂的(x, y, z)位置列表。

```py
def build_spiral_stars(b, r, rot_fac, fuz_fac):
    """Return list of (x,y,z) points for a logarithmic spiral.

    b = constant for spiral direction and "openness"
    r = scale factor (galactic disc radius)
    rot_fac = factor to rotate each spiral arm
    fuz_fac = randomly shift star position; applied to 'fuzz' variable
    """
    fuzz = int(0.030 * abs(r))  # Scalable initial amount to shift locations.
    num_stars = 1000
    spiral_stars = []
    for i in range(0, num_stars):
        theta = math.radians(i)
        x = r * math.exp(b*theta) * math.cos(theta - math.pi * rot_fac)\
            - randint(-fuzz, fuzz) * fuz_fac
        y = r * math.exp(b*theta) * math.sin(theta - math.pi * rot_fac)\
            - randint(-fuzz, fuzz) * fuz_fac
        z = uniform((-SCALE / (SCALE * 3)), (SCALE / (SCALE * 3)))
        spiral_stars.append((x, y, z))
    return spiral_stars
```

这里需要注意的是，螺旋星系在 x 和 y 方向上较宽，但在 z 方向上较薄。Matplotlib 的默认设置对此处理不佳，因此在绘图之前，我们将手动限制 z 轴值在-15 到 15 之间（-20 到 20 也会得到良好的结果）。

![](img/3e1a545fe6c3777a16e2c2d62fd89471.png)

带有框架的银河系模拟。请注意 z 轴上使用的垂直夸张（作者）

我在缩放 z 方向时考虑了这个夸张的 z 轴。因此，如果你手动强制所有轴保持相同的比例，可能会看到一些失真。

## 分配螺旋臂参数

为了制作一个四臂螺旋星系，我们需要调用之前的 `build_spiral_stars()` 函数八次（每个螺旋体绘制前缘和后缘各两次）。为此，让我们构建一个包含我们需要传递的最后三个参数的元组列表。由于每个臂的 `b` 参数保持不变，我们将在调用函数时直接传递它。我使用的参数是通过反复试验确定的。

```py
# Assign scale factor, rotation factor, and fuzz factor for spiral arms.
# Each arm is a pair: leading arm + trailing arm:
arms_info = [(SCALE, 1, 1.5), (SCALE, 0.91, 1.5), 
             (-SCALE, 1, 1.5), (-SCALE, -1.09, 1.5),
             (-SCALE, 0.5, 1.5), (-SCALE, 0.4, 1.5), 
             (-SCALE, -0.5, 1.5), (-SCALE, -0.6, 1.5)]
```

## 定义一个构建螺旋臂的函数

下一个函数接收我们刚刚制作的 `arms_info` 列表和一个 `b` 值，并返回一个包含每个四个螺旋臂的前缘星星的列表，以及一个类似的后缘列表。我们需要单独的列表，因为在绘制后缘时我们会使用更小的标记大小。

该函数通过检查 `arms_info` 索引是奇数还是偶数来区分前缘和后缘。

```py
def build_spiral_arms(b, arms_info):
    """Return lists of point coordinates for galactic spiral arms.

    b = constant for spiral direction and "openness"
    arms_info = list of scale, rotation, and fuzz factors
    """
    leading_arms = []
    trailing_arms = []
    for i, arm_info in enumerate(arms_info):
        arm = build_spiral_stars(b=b, 
                                 r=arm_info[0], 
                                 rot_fac=arm_info[1], 
                                 fuz_fac=arm_info[2])
        if i % 2 != 0:
            leading_arms.extend(arm)
        else:
            trailing_arms.extend(arm)            
    return leading_arms, trailing_arms
```

## 定义一个生成球面坐标的函数

尽管螺旋星系的大部分由一个扁平的盘组成，但中心往往会向外“[膨胀](https://en.wikipedia.org/wiki/Galactic_bulge)” [3]。

![](img/d941553eb52c92383e3fbf596b46c10a.png)

对银河系中心膨胀的艺术家印象（由 NASA 通过 Wikimedia Commons [4] 提供）

为了在我们的模拟中创建一个膨胀体，我们将定义一个生成球形点云的函数。它将接受我们想要的点的数量以及半径作为参数，然后生成随机的 x、y 和 z 点，并将它们作为元组列表返回。

我们将使用 NumPy 的 `random.normal()` 函数生成这些点，该函数从正态分布中抽取随机样本。它接受均值、标准差和输出形状（维度）作为参数。然后，我们将这些点乘以 `radius` 参数以将其缩放到模拟中。

为了弥补 matplotlib 显示中的垂直夸张，我们将通过将每个 `coords` 元组中索引 `[2]` 处的 `z` 值乘以 0.02 来减少这些值。 （这个值有点任意；可以随意调整。）

```py
def spherical_coords(num_pts, radius):
    """Return list of uniformly distributed points in a sphere."""
    position_list = []
    for _ in range(num_pts):
        coords = np.random.normal(0, 1, 3)
        coords *= radius
        coords[2] *= 0.02  # Reduce z range for matplotlib default z-scale.
        position_list.append(list(coords))
    return position_list
```

## 定义一个构建核心星星的函数

接下来，我们将定义一个函数，调用之前的函数来构建并返回外核和内核的坐标列表。我们将核心分为两个体积，以便在密集的中央区域提供视觉上的渐变。

```py
def build_core_stars(scale_factor):
    """Return lists of point coordinates for galactic core stars."""
    core_radius = scale_factor / 15
    num_rim_stars = 3000
    outer_stars = spherical_coords(num_rim_stars, core_radius)
    inner_stars = spherical_coords(int(num_rim_stars/4), core_radius/2.5)
    return (outer_stars + inner_stars)
```

## 定义一个散布星星雾气的函数

螺旋臂之间的空间并非没有星星，因此我们需要一个函数在整个星系模型上随机分布点。把它看作是你在星系照片中看到的“雾气”或“光晕”。

我们将包括半径（`r_mult`）和 z 值（`z_mult`）的标量。这将允许我们通过调用该函数两次来“加厚”银河盘，从而生成“内部”和“外部”点集。

```py
def haze(scale_factor, r_mult, z_mult, density):
    """Generate uniform random (x,y,z) points within a disc for 2-D display.

    scale_factor = galactic disc radius
    r_mult = scalar for radius of disc
    z_mult = scalar for z values
    density = multiplier to vary the number of stars posted
    """
    haze_coords = []
    for _ in range(0, scale_factor * density):
        n = random()
        theta = uniform(0, 2 * math.pi)
        x = round(math.sqrt(n) * math.cos(theta) * scale_factor) / r_mult
        y = round(math.sqrt(n) * math.sin(theta) * scale_factor) / r_mult
        z = np.random.uniform(-1, 1) * z_mult
        haze_coords.append((x, y, z))
    return haze_coords
```

这是星际雾霾与无星际雾霾的影响：

![](img/6f8d2496f4668b8f4e243ba06b99964b.png)

星际雾霾对星系模拟的影响（作者）

## 生成星体坐标并绘制星系

以下代码通过调用构建星体坐标列表的函数，然后将它们传递给 matplotlib 的`scatter()`方法，完成了程序。

为了准备绘图数据，我们使用了展开运算符（`*`），如`*zip(*leading_arm)`中所示。这种语法将元组列表“解压”，有效地将行转置为列。元组被解包，每个元素作为单独的参数传递给`zip()`函数。这为 x、y 和 z 值创建了单独的元组，每个元组中包含正确的对应值。以下是示例：

**a_list = [(x1, y1, z1), (x2, y2, z2)]**

***zip(*a_list) => (x1, x2), (y1, y2), (z1, z2)**

```py
# Create lists of star positions for galaxy:
leading_arm, trailing_arm = build_spiral_arms(b=-0.3, arms_info=arms_info)
core_stars = build_core_stars(SCALE)
inner_haze_stars = haze(SCALE, r_mult=2, z_mult=0.5, density=5)
outer_haze_stars = haze(SCALE, r_mult=1, z_mult=0.3, density=5) 

# Plot stars in 3D using matplotlib:
fig, ax = plt.subplots(1, 1, 
                       subplot_kw={'projection': '3d'}, 
                       figsize=(12, 12))
ax.set_axis_off()
ax.set_zlim (-15, 15)

ax.scatter(*zip(*leading_arm), c='w', marker='.', s=5)
ax.scatter(*zip(*trailing_arm), c='w', marker='.', s=2)
ax.scatter(*zip(*core_stars), c='w', marker='.', s=1)
ax.scatter(*zip(*inner_haze_stars), c='w', marker='.', s=1)
ax.scatter(*zip(*outer_haze_stars), c='lightgrey', marker='.', s=1)
```

![](img/e33087f15980784a9ca302eab7d3800b.png)

程序的输出（作者）

那是一幅漂亮的星系。我想住在那里！

正如你可能已经注意到的，我们的代码包括几个参数，你可以用来调整输出，发挥你内心的艺术家。

例如，更改`b`参数在`build_spiral_arms()`函数中会改变前后臂的外观。这里是`b`设置为`-0.5`的示例：

![](img/ea4eee459e1515f5f34f733e8487b11c.png)

b = -0.5 的输出（作者）

更改`SCALE`常量会影响银河隆起的大小。以下是使用不同值的一些示例：

![](img/345d8d31492de3d636c21d2f075c5b50.png)

使用不同 SCALE 值的星系剖面视图（作者）

在之前的图中，请注意使用浅灰色表示外部雾霾星星会产生类似于真实星系剖面中的“尘带”效果。

你还可以更改标记样式。例如，如果你想使用实际的星形标记，可以添加以下代码：

```py
import matplotlib.path as mpath
star = mpath.Path.unit_regular_star(6)
```

然后将`star`作为`ax.scatter()`调用中的`marker`参数传递。`(6)`表示*顶点*的数量。值为 5 产生五角星，值为 6 产生六个点。

## 奖励：生成球状星团

我们已经完成了螺旋星系项目，但如果我不向你展示如何构建一种天文奇观——[*球状星团*](https://en.wikipedia.org/wiki/Globular_cluster)[5]，那将是遗憾。这些是球形的恒星集合，绕着大多数螺旋星系，如我们的银河系旋转。它们是星系中最古老的特征之一，包含数百万颗紧密排列的恒星[6]。

我们定义的用于构建球形核心恒星的函数非常适合构建球状星团，为什么不使用它呢？以下是代码：

```py
def spherical_coords(num_pts, radius):
    """Return list of uniformly distributed points in a sphere."""
    position_list = []
    for _ in range(num_pts):
        coords = np.random.normal(0, 1, 3) 
        coords *= radius 
        position_list.append(list(coords))
    return position_list

rim_radius = 2
num_rim_stars = 3000
rim_stars = spherical_coords(num_rim_stars, rim_radius)
core_stars = spherical_coords(int(num_rim_stars/4), rim_radius/2.5)

fig, ax = plt.subplots(1, 1, subplot_kw={'projection':'3d'})
ax.axis('off')
ax.scatter(*zip(*core_stars), s=0.5, c='white')
ax.scatter(*zip(*rim_stars), s=0.1, c='white')
ax.set_xlim(-(rim_radius * 4), (rim_radius * 4))
ax.set_ylim(-(rim_radius * 4), (rim_radius * 4))
ax.set_zlim(-(rim_radius * 3), (rim_radius * 3))
ax.set_aspect('auto')
```

以下是结果：

![](img/791f92c95984e50786d9d886001ea37b.png)

球状星团模拟（作者）

# 摘要

使用 Python、matplotlib 和一个简单的方程式，我们能够轻松生成螺旋星系的 3D 模拟。除了产生一些有趣的数字艺术，这个项目也是向学生介绍对数螺旋的一个很好的方式，适用于包括数学、天文学和编程等多个学科领域。

# 引用

1.  Vaughan, L.（2023 年），“用 Tkinter 创建银河艺术：用对数螺旋模拟自然母亲，” *Towards Data Science*，`towardsdatascience.com/create-galactic-art-with-tkinter-e0418a59b215`。

1.  Vaughan, Lee, 2018, [*不切实际的 Python 项目：有趣的编程活动让你更聪明*](https://a.co/d/5e2NG9b), No Starch Press, 旧金山。

1.  维基百科贡献者。（2023 年 10 月 3 日）。银河系膨胀。在*维基百科，自由百科全书*。检索于 2023 年 10 月 3 日 16:23，来自 [`en.wikipedia.org/w/index.php?title=Galactic_bulge&oldid=1178373003`](https://en.wikipedia.org/w/index.php?title=Galactic_bulge&oldid=1178373003)。

1.  ESO/NASA/JPL-Caltech/M. Kornmesser/R. Hurt, CC BY 4.0 <[`creativecommons.org/licenses/by/4.0`](https://creativecommons.org/licenses/by/4.0/)>, 通过维基共享资源，[文件：银河系中心膨胀的艺术家印象.jpg — 维基共享资源](https://commons.wikimedia.org/wiki/File:Artist%27s_impression_of_the_central_bulge_of_the_Milky_Way.jpg)。

1.  维基百科贡献者。（2023 年 9 月 24 日）。球状星团。在*维基百科，自由百科全书*。检索于 2023 年 10 月 4 日 00:56，来自 [`en.wikipedia.org/w/index.php?title=Globular_cluster&oldid=1176809907`](https://en.wikipedia.org/w/index.php?title=Globular_cluster&oldid=1176809907)。

1.  Vaughan, Lee, 2023, [*科学家用 Python 工具：Anaconda、JupyterLab 和 Python 科学库的入门*](https://a.co/d/e7ltmEN), No Starch Press, 旧金山。

# 谢谢！

感谢阅读，请关注我，以便未来获取更多*快速成功数据科学*项目。
