# 世界地图的多种面貌——地图投影

> 原文：[`towardsdatascience.com/the-world-map-with-many-faces-map-projections-f58a210ff2f7`](https://towardsdatascience.com/the-world-map-with-many-faces-map-projections-f58a210ff2f7)

![](img/89cd72ef5256e73a309da79c2d5c2a6b.png)

作者提供的图像。

[](https://medium.com/@janosovm?source=post_page-----f58a210ff2f7--------------------------------)![米兰·贾诺索夫](https://medium.com/@janosovm?source=post_page-----f58a210ff2f7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f58a210ff2f7--------------------------------)![面向数据科学](https://towardsdatascience.com/?source=post_page-----f58a210ff2f7--------------------------------) [米兰·贾诺索夫](https://medium.com/@janosovm?source=post_page-----f58a210ff2f7--------------------------------)

·发表于 [面向数据科学](https://towardsdatascience.com/?source=post_page-----f58a210ff2f7--------------------------------) ·5 分钟阅读·2023 年 10 月 1 日

--

# 在这篇简短的文章中，我回顾了什么是地图投影，并展示了一系列使用 Python 和自然地球数据投影世界地图的例子。

地球大致上是一个球体，确实是一个三维物体（尽管也有一些挑战），而我们的印刷地图和数字屏幕则是二维的。将球体转换为二维地图的中间步骤，无论是制图册还是高级 GIS 应用程序，都称为地图投影。

将地球的椭球面映射到平面上有很多方法；然而，由于这些是近似模型，通常会有一些不足之处需要注意。在某些投影中，相对角度和多边形（例如国家）形状被保留；而在其他投影中，真实的面积或特定的欧几里得距离被保持不变。这些属性也有助于你选择适合你使用情况的最佳投影。

关于投影类型，有多种方式，例如圆柱投影、圆锥投影、方位投影和伪圆柱投影。相互之间转换的实际方法是改变坐标参考系统（CRS），这时 Python 和 PyProj 以及 GeoPandas 库非常有用！

根据 [Mathematics.com](https://www.mapthematics.com/ProjectionsList.php?Projection=319#Larriv%C3%A9e)，投影类型主要有六种类别：

+   圆柱投影

+   伪圆柱投影

+   方位投影

+   凸透镜投影

+   杂项

在这里，我不会严格按照这样的分类，而是展示九种我认为有趣的地图投影。根据具体的投影，这些步骤通常会导致形状、面积、距离或方向的失真，因此在实际项目中应谨慎选择。为此，这个 [投影系统集合](https://pyproj4.github.io/pyproj/v1.9.6rel/pyproj-pysrc.html) 和下面提供的我的代码应该会有所帮助。那么，不再多说，地图如下：

**1\. 埃克特 II 投影**

*特征*：埃克特 II 投影是一种等面积伪圆柱投影。它保持面积的准确性，但扭曲形状和距离。

*常见用途*：主要用于展示直线等面积网格的世界新奇地图。

![](img/ab4c43b62472d3be8191ff0ee1c31ba5.png)

埃克特 II 投影。图片由作者提供。

**2\. 等距圆柱投影**

*特征*：等距圆柱投影是一种简单的圆柱投影。它保持纬度和经度为直线，但在远离赤道时会扭曲形状和面积。

*常见* 用途：通常用于教育或一般参考及主题世界地图。

![](img/b8f5a86d5280f59374fcc779105477df.png)

等距圆柱投影。图片由作者提供。

**3\. 拉格朗日投影**

*特征*：拉格朗日投影是圆锥形且符合变形的。失真发生在面积、形状和方向上。*常见用途*：根据 ChatGPT，它的稀有用例主要覆盖海洋学。

![](img/fb63e2340807270ee47c4665f7eea266.png)

拉格朗日投影。图片由作者提供。

**4\. 拉里维投影**

*特征*：根据这个 [来源](https://www.mapthematics.com/ProjectionsList.php?Projection=319#Larriv%C3%A9e)，在地图中心不失真，而在大部分情况下会出现面积膨胀的失真

![](img/5c9b120c9c53ecd52702905e17d54c58.png)

拉里维投影。图片由作者提供。

**5\. 莫尔维德投影**

*特征*：莫尔维德投影是一种等面积伪圆柱投影。它保持面积但扭曲形状和角度。它具有独特的椭圆形状。

*常见* *用途*：当面积准确性至关重要时用于全球地图，尤其是在地理学和地球物理学中。

![](img/08c3fd943a953715a08460c587f6c3ae.png)

莫尔维德投影。图片由作者提供。

**6\. 自然地球投影**

*特征*：自然地球投影是一种为世界地图设计的伪圆柱投影。它在大小和形状失真上保持平衡（但不保持任何一方面），提供了一个视觉上令人愉悦的地图。

*常见* *用途*：由于其平衡的失真特性，它在世界地图和地图集上非常受欢迎。

![](img/0734865e3a5bc4edff45d106a17b1ff4.png)

自然地球投影。图片由作者提供。

**7\. 四次阿萨尔投影**

*特点*：Quartic Authalic 投影是一种伪圆柱等面积投影，准确保持面积，但形状、角度和距离会失真。

*常见用途*：用于需要准确面积表示的特殊制图应用。

![](img/5fdf9d6fea509b17a79600896bd1b881.png)

Quartic Authalic 投影。图片由作者提供。

**8. 矩形多锥投影**

*特点*：矩形多锥投影是一种最小化经线失真的圆锥投影。有时称为战争办公室投影。

*常见用途*：主要用于美国军事目的。

![](img/4c217aae8ffcac4f18e40ffa73f32bf2.png)

矩形多锥投影。图片由作者提供。

**9. 正弦投影**

*特点*：伪圆柱等面积地图投影，将极点表示为点，保持面积，但形状会失真。

*常见* *用途*：主要建议用于绘制接近赤道的区域。

![](img/14714070b168c2912ce49cd6d01e8c10.png)

最后，作为可视化的数据源，我使用了来自 Natural Earth 的以下两个数据集：

- [Admin 0 — Countries](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/)

- [海洋](https://www.naturalearthdata.com/downloads/10m-physical-vectors/)

现在，让我们看看代码：

```py
import geopandas as gpd
import matplotlib.pyplot as plt
import pyproj
```

```py
world = gpd.read_file('ne_10m_admin_0_countries')
ocean = gpd.read_file('ne_10m_ocean')
```

```py
# visualize all projections at once
projection_dict = { 'Eckert II': '+proj=eck2 +lon_0=0 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs',
                    'Equirectangular': '+proj=eqc +lon_0=0 +lat_ts=0 +x_0=0 +y_0=0 +a=6378137 +b=6378137 +units=m +no_defs',
                    'Lagrange' : '+proj=lagrng',
                    'Larrivee' : '+proj=larr',
                    'Mollweide': '+proj=moll +lon_0=0 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs',
                    'Natural Earth': '+proj=eqearth +lon_0=0 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs',
                    'Quartic Authalic': '+proj=qua_aut +lon_0=0 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs',
                    'Rectangular Polyconic': '+proj=poly +lat_0=0 +lon_0=0 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs',
                    'Sinusoidal' : '+proj=sinu +lon_0=0 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs'
                  }

f, ax = plt.subplots(3,3,figsize=(15,15))
indicies = [(i, j) for i in range(3) for j in range(3)]

for idx, (projection_name, proj4_string) in enumerate(projection_dict.items()):

    bx = ax[indicies[idx]] 
    world_projected = world.to_crs(proj4_string)
    ocean_projected = ocean.to_crs(proj4_string)
    ocean_projected.plot(ax=bx, color = 'lightblue')
    world_projected.plot(ax=bx, cmap='Greens', edgecolor='k', linewidth = 0.25, alpha = 0.9)
    bx.set_title(f'{projection_name} Projection')
    bx.axis('off')  

plt.tight_layout()
```
