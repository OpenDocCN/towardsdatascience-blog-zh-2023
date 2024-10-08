# 探索聚类领域

> 原文：[`towardsdatascience.com/navigating-the-clustering-landscape-b7930ac44147`](https://towardsdatascience.com/navigating-the-clustering-landscape-b7930ac44147)

## 聚类 | 机器学习 | Python

## 主要聚类算法的比较及实用 Python 示例

[](https://david-farrugia.medium.com/?source=post_page-----b7930ac44147--------------------------------)![David Farrugia](https://david-farrugia.medium.com/?source=post_page-----b7930ac44147--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b7930ac44147--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b7930ac44147--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----b7930ac44147--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b7930ac44147--------------------------------) ·阅读时间 7 分钟·2023 年 5 月 29 日

--

![](img/95b6eab856ff3a4e575767f07f2d4ebe.png)

照片来源：[Aleks Dorohovich](https://unsplash.com/ja/@doctype?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

机器学习领域不断发展，近年来引起了极大的兴趣。尤其是在当前 AI 热潮的影响下，数据科学和机器学习的神秘世界无疑受益于广泛的公众曝光。

在本文中，我们将把焦点放在机器学习的一个迷人子领域——聚类上。

让我们尝试描绘一个图景，好吗？

假设你正站在一个繁忙的城市中，周围是成千上万的人。在那一刻，每个人都是 100%独特的。他们是各自独立的个体——然而，你们都有共同的特征或属性，将你们作为一个群体连接在一起。也许是你们的时尚感、职业、政治或宗教信仰，甚至是你们喜欢的食物。

这就是聚类的本质——试图在大量数据点中发现隐藏的模式和分组。在机器学习的世界里，聚类技术是数据宇宙的制图师，描绘出原本看似随机或脱节的信息星座。

在本文中，我们将深入探讨主要的聚类技术类型，揭示它们的独特特性、优势以及应用。不论你是经验丰富的数据专业人士还是好奇的读者，我邀请你加入我这次激动人心的探索之旅。

# K 均值聚类

让我们假设一下，我们是养蜂人。我们有一群蜜蜂在嗡嗡作响，我们的目标是将它们分成 K 个不同的蜂巢（即我们的聚类）。

首先，我们随机选择 K 只蜜蜂。这些蜜蜂将作为我们的簇质心。

就像蜜蜂被吸引到蜜糖一样，每只蜜蜂（数据点）都会向最近的蜂巢（质心）靠拢。

当所有的蜜蜂都找到一个蜂巢后，我们将确定每个蜂巢的新质心（更新质心）。我们会不断重复这个过程，直到蜜蜂安定下来，不再交换蜂巢。

啪嗒，那就是 K-means 聚类的本质！

![](img/20e946c5190d3a24157b1ec8c2053fab.png)

K-means 聚类流程图。图片由作者提供。

## *优点*

1.  简单易懂

1.  易于扩展

1.  在计算成本方面效率高

## *缺点*

1.  需要提前指定 K

1.  对初始质心选择敏感

1.  假设簇是球形且大小相等（这可能并不总是如此）

## 完美的应用场景

1.  市场细分

1.  文档聚类

1.  图像分割

1.  异常检测

## Python 示例

```py
from sklearn.cluster import KMeans
import numpy as np

# Let's assume we have some data
X = np.array([[1, 2], [1, 4], [1, 0], [4, 2], [4, 4], [4, 0]])

# We initialize KMeans with the number of clusters we want
kmeans = KMeans(n_clusters=2, random_state=0).fit(X)

# We can get the labels of the data points
print(kmeans.labels_)

# And we can predict the clusters for new data points
print(kmeans.predict([[0, 0], [4, 4]]))

# The cluster centers (the mean of all the points in that cluster) can be accessed with
print(kmeans.cluster_centers_)
```

# 层次聚类（凝聚型聚类）

假设你正在参加一个大家庭的婚礼，亲属关系不明确。

你的第一个任务是识别直系亲属，如兄弟姐妹或父母和子女，并将他们聚在一起。

接下来，你寻找与这些已建立的群体有密切关系的其他亲属，并将他们纳入其中。

你继续这个过程，逐渐拼凑出整个家庭和朋友的网络，直到每个人都互相连接。

啪嗒，那就是层次聚类的本质！

![](img/175da4150e35165ce0a466ca7226fb6e.png)

层次聚类流程图。图片由作者提供。

## 优点

1.  无需指定簇的数量

1.  提供一个簇的层次结构，这可能是有用的

## 缺点

1.  对大型数据集计算成本高

1.  对距离度量的选择敏感

## 完美的应用场景

1.  基因测序

1.  社会网络分析

1.  构建分类树

## Python 示例

```py
from sklearn.cluster import AgglomerativeClustering
import numpy as np

# Let's assume we have some data
X = np.array([[1, 2], [1, 4], [1, 0], [4, 2], [4, 4], [4, 0]])

# We initialize AgglomerativeClustering with the number of clusters we want
clustering = AgglomerativeClustering(n_clusters=2).fit(X)

# We can get the labels of the data points
print(clustering.labels_)
```

# 基于密度的空间聚类算法（DBSCAN）

想象一下你是一个狂热的鸟类观察者。

拿着你可靠的双筒望远镜，你四处扫描地平线，寻找空中鸟群。每当你发现一只鸟时，你会环顾四周，看看是否在给定半径内（数学上定义为 epsilon）有特定数量的鸟（我们称这个概念为最小点数或 MinPts）。

如果你的观察结果验证了，恭喜！你已识别出一个鸟群！

接下来，你寻找更多在这个簇范围内的鸟，并将它们纳入群体。这个模式会持续，直到所有的鸟都被标记为群体的一部分或被确定为孤立飞行者（异常值）。

啪嗒，那就是 DBSCAN 的本质！

![](img/57c867fc2ec63e1eb1ba30f002647a45.png)

DBSCAN 聚类流程图。图片由作者提供。

## *优点*

1.  无需指定簇的数量。

1.  可以发现任意形状的簇。

1.  擅长将高密度簇与低密度噪声分开。

## *缺点*

1.  不适合具有不同密度簇的数据。

1.  对 epsilon 和 MinPts 的选择敏感。

## 完美的用例

1.  空间数据分析

1.  异常检测

1.  图像分割。

## Python 示例

```py
from sklearn.cluster import DBSCAN
import numpy as np

# Let's assume we have some data
X = np.array([[1, 2], [2, 2], [2, 3], [8, 7], [8, 8], [25, 80]])

# We initialize DBSCAN with the epsilon and minPts we want
clustering = DBSCAN(eps=3, min_samples=2).fit(X)

# We can get the labels of the data points
print(clustering.labels_)
```

# 均值漂移聚类

想象一下你身处一个漆黑的房间，只能用手电筒，你的目标是找到你的朋友们。

一旦你打开手电筒，它会照亮附近的一些人。然后你评估他们的平均（均值）位置，并将手电筒的光束调整到那个方向。

你继续重复这个过程，每次都将光调整到计算出的均值位置，直到你发现自己在朋友们中间。

而这就是均值漂移聚类的精髓！

![](img/067422ea18324623144c6bb74d79cd27.png)

均值漂移聚类流程图。图片由作者提供。

## *优点*

1.  无需指定簇的数量

1.  可以发现任意形状的簇

## *缺点*

1.  对大数据集来说计算开销较大。

1.  带宽参数可以极大地影响结果。

## 完美的用例

1.  图像分割

1.  视频追踪

1.  计算机视觉

## Python 示例

```py
from sklearn.cluster import MeanShift
import numpy as np

# Let's assume we have some data
X = np.array([[1, 1], [2, 1], [1, 0], [4, 7], [3, 5], [3, 6]])

# We initialize MeanShift with the bandwidth we want
clustering = MeanShift(bandwidth=2).fit(X)

# We can get the labels of the data points
print(clustering.labels_)

# The cluster centers can be accessed with
print(clustering.cluster_centers_)
```

# 谱聚类

想象你是一个音乐指挥，任务是从一群音乐家中组建一个乐团。

你首先听每位音乐家的表演。你记录他们音乐风格的相似性，创建一种“音乐兼容性图表”——你的相似性矩阵。

这个图表作为模板（和谐蓝图——使用拉普拉斯方法计算），你可以根据它来识别出相似的不同簇。从数学上讲，这是通过特征值分解实现的。

最终，每位音乐家根据这种分析（使用 K 均值聚类）找到自己在特定组中的位置。

而这就是谱聚类的精髓！

![](img/25ef580a5efd36fbd36d54c0cb75375e.png)

谱聚类流程图。图片由作者提供。

## *优点*

1.  可以发现任意形状的簇。

1.  对于小数量的簇效果良好。

## *缺点*

1.  对大数据集来说计算开销较大。

1.  相似性度量和簇的数量选择会极大地影响结果。

## 完美的用例

1.  图像分割

1.  社交网络分析

1.  基因表达分析

## Python 示例

```py
from sklearn.cluster import SpectralClustering
import numpy as np

# Let's assume we have some data
X = np.array([[1, 1], [2, 1], [1, 0], [4, 7], [3, 5], [3, 6]])

# We initialize SpectralClustering with the number of clusters we want
clustering = SpectralClustering(n_clusters=2, assign_labels="discretize", random_state=0).fit(X)

# We can get the labels of the data points
print(clustering.labels_)
```

# 结论

在这篇文章中，我们深入探讨了一些在机器学习领域至关重要的聚类算法。我们审视了这些方法如何实现目标，讨论了它们的优缺点，并强调了它们最有效的场景。

选择合适的聚类技术就像选择适当的工具来完成特定任务一样。重要的是要考虑你处理的数据类型和你要解决的具体问题。例如，处理层次数据时，使用基于密度的聚类方法是不明智的。

机器学习中的聚类领域是一个充满活力的景观，充满了探索、发现和突破性想法的可能性。

当我们持续深入探索和掌握这一领域时，我们不仅仅是在使数据更易理解；我们还在为开创性的进步奠定基础，这些进步可能会重塑我们与周围世界的互动方式。因此，请保持探索的精神，继续丰富你的理解，永远不要停止聚类！

**你喜欢这篇文章吗？每月$5，你可以成为会员，解锁对 Medium 的无限访问权限。这将直接支持我和你在 Medium 上喜欢的其他作者。非常感谢！**

[## 通过我的推荐链接加入 Medium - David Farrugia](https://david-farrugia.medium.com/membership?source=post_page-----b7930ac44147--------------------------------)

### 获得我所有⚡优质⚡内容和 Medium 上的无限访问权限。通过请我喝一杯咖啡来支持我的工作…

[david-farrugia.medium.com](https://david-farrugia.medium.com/membership?source=post_page-----b7930ac44147--------------------------------)

# 想联系我吗？

我很想听听你对这个话题，或者对任何 AI 和数据的看法。

如果你希望联系我，请发邮件到***davidfarrugia53@gmail.com***。

[Linkedin](https://www.linkedin.com/in/david-farrugia/)
