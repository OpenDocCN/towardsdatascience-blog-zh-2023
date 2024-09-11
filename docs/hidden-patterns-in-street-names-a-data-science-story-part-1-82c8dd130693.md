# 街道名称中的隐藏模式：数据科学故事 [第1部分]

> 原文：[https://towardsdatascience.com/hidden-patterns-in-street-names-a-data-science-story-part-1-82c8dd130693?source=collection_archive---------4-----------------------#2023-01-29](https://towardsdatascience.com/hidden-patterns-in-street-names-a-data-science-story-part-1-82c8dd130693?source=collection_archive---------4-----------------------#2023-01-29)

[](https://deabardhoshi.medium.com/?source=post_page-----82c8dd130693--------------------------------)[![Dea Bardhoshi](../Images/14ce0986fc2a4a192797a52ed9908d1e.png)](https://deabardhoshi.medium.com/?source=post_page-----82c8dd130693--------------------------------)[](https://towardsdatascience.com/?source=post_page-----82c8dd130693--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----82c8dd130693--------------------------------) [Dea Bardhoshi](https://deabardhoshi.medium.com/?source=post_page-----82c8dd130693--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd61c58ba988e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhidden-patterns-in-street-names-a-data-science-story-part-1-82c8dd130693&user=Dea+Bardhoshi&userId=d61c58ba988e&source=post_page-d61c58ba988e----82c8dd130693---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----82c8dd130693--------------------------------) ·6分钟阅读·2023年1月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F82c8dd130693&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhidden-patterns-in-street-names-a-data-science-story-part-1-82c8dd130693&user=Dea+Bardhoshi&userId=d61c58ba988e&source=-----82c8dd130693---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F82c8dd130693&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhidden-patterns-in-street-names-a-data-science-story-part-1-82c8dd130693&source=-----82c8dd130693---------------------bookmark_footer-----------)![](../Images/024b14857164d6a413d164623793c12f.png)

照片由 [Alexandr Bormotin](https://unsplash.com/@bormot?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你好！

最近我花了一些时间编制数据集，并阅读了关于我祖国阿尔巴尼亚的研究，试图从数据驱动的角度理解它。除了我自己的经验，我还希望有一种系统的方法来分析该国的社会、政治和生活背景。最近引起我好奇的一个方面是街道名称及其模式。在网上查找时，我找不到一个现有的数据集来查看该国的道路和街道是如何命名的，所以我决定创建并分析一个数据集。

对于这个故事，我将使用 Open Street Map 数据来分析阿尔巴尼亚首都地拉那的街道名称中的性别分布，以及其他历史或地理空间模式。为此，我还会使用**pandas**、**seaborn**和一个名为**contextily**的地图绘制库来制作更美观的地图。第一部分将重点关注性别和街道名称，但我会在接下来的几周内继续研究第二部分，关注活动年份以及贡献区域。

那么，开始吧！

## 数据标注与可视化

这是 Open Street Map 提供的数据的一个快照，指定了一组坐标以覆盖整个地拉那区域：

![](../Images/13a988aaa933795dafe86588ffc73538.png)

图片由作者提供

数据包含 26950 行，大多数行有几何形状和其他关联的道路类型属性。有趣的是，同一条街道在数据中出现了多次，可能是因为同一条街道的不同部分在不同时间被映射。由于数据中没有分类名字性别的变量，并且没有简单的方法通过代码将阿尔巴尼亚语的名字与特定性别匹配，我利用自己对语言的知识手动标记了大约一半的数据框的行。该列包含以下标签之一：

+   **W** = 女性名字

+   **M** = 男性名字

+   **O** = 其他类型的名字（例如历史事件、抽象概念或著名家族的姓氏）

这里是每个性别的计数情况：

![](../Images/ed9ee260da69281cf3eb13d6234085ff.png)

图片由作者提供

如你所见，女性街道名称在地拉那的街道名称中占约3.3%，而男性名称则占约71%。这种情况在阿尔巴尼亚并非独特：[巴黎有2%的街道以女性命名](https://www.fodors.com/world/europe/france/experiences/news/why-are-all-the-streets-in-france-named-after-men)，而在[罗马这个比例是3.5%](https://www.bloomberg.com/news/articles/2015-11-04/mapping-the-sexism-of-street-names-in-major-cities)。在这些主要城市中，部分原因是制定这些决定的市政委员会历史上往往是男性和白人的主导。事实上，全球地方政府成员的平均女性比例仅为[36%](https://www.unwomen.org/sites/default/files/2022-01/Womens-representation-in-local-government-en.pdf)，许多国家甚至远未达到这一值，例如只有约25个国家在地方政府中有40%的女性代表。尽管如此，最近的地拉那市议会立法机构的男女比例为50–50%，这是朝着正确方向迈出的一步。

## 街道类型和长度

OpenStreetMap 数据还包括描述每条街道类型的标签，以下是按性别比较这些类型的可视化图：

![](../Images/98a4140ec771c6dfc790ca91554fa2e5.png)

作者提供的图片

有趣的是，存在如此多的“住宅”街道。根据 OpenStreetMap 的定义，**住宅**标签是“用于提供访问或在住宅区内的道路，但通常不作为通行路线使用”。“**生活**”街道也非常常见，定义为“具有较低的速度限制，相较于使用住宅标签的街道，有特殊的交通和停车规则”。这些是地拉那街道中主导的两种类型，即使它们不是主要街道，而是较窄且交通较少的街道。因此，研究这些街道如何命名可能会很有趣。

让我们看看按性别分组的所有街道总长度。为此，我将线串几何体投影到使用米的投影坐标系统中，并按性别平均了它们的长度：

![](../Images/5633bf52ebb77cc5d4b2c9609e570c53.png)

作者提供的图片

有趣的是，“其他”街道的平均长度最长，而以女性命名的街道略短于以男性命名的街道。还发现“其他”街道多位于远离地拉那城市核心的高速公路或外围街道上：

![](../Images/f15ed897df0ae8f8359222e2c267f77b.png)

作者提供的图片

这可以通过高速公路或其他城市间街道的名称来解释，如“Tirane-Durres”，它标示了它们连接的两个城市中心，或者仅仅是“SH1”这样的高速公路代码。鉴于此，我将这些街道标记为“其他”。

这里是以男性和女性命名的街道地图：

以下是自定义地图的示例代码

![](../Images/a2ca6d246a999f2b033a751ab9c7865b.png)

以男性命名的街道地图（图像由作者提供）

![](../Images/dc7492865b3580b171971869861d82cf.png)

以女性命名的街道地图（图像由作者提供）

## 职业和工作

每位女性的贡献领域是什么？我将职业分为几个类别，结果如下（有些人物在网上没有可用的信息）：

我的分类大致为：

+   **艺术、教师/作家/研究员、政治、人道主义和宗教与战争**（为提供一些背景，许多这些女性在二战中与男性并肩作战，她们代表了“战争”类别中大多数女性）：

![](../Images/ff0f0e2e6633ddb73b38bc692ee7a1dc.png)

图像由作者提供

## 邻里

除了贡献领域，街道名称在不同邻里中的分布是否存在模式？地拉那有14个行政区域，将城市划分为不同的区域。使用来自OpenStreetMaps的GeoJSON文件，该文件显示了这些区域的多边形，我们可以将这个数据集与街道名称的数据集进行空间连接。这是14个区域的地图：

创建下图的代码，带有用户定义的图例

![](../Images/88b451e286e29dfb7432c0b66374eaeb.png)

我对每个行政区域中街道名称中女性的比例感兴趣：

![](../Images/db6bba6fc50799c8c941d646b65890c9.png)

有一些有趣的发现（至少对我来说 :) ）。女性名字的比例存在一些差异，如第4区的街道中几乎有10%以女性命名，而第8区仅有0.08%。另一方面，两个区域（12、14）没有以女性命名的街道。再次说，探讨这些命名选择背后的决策过程将是有趣的。

## 结论

总结来说，这个故事探讨了地拉那（阿尔巴尼亚）的街道命名情况，重点关注了性别构成以及城市不同区域和贡献领域的模式。稍后的第二部分将会讨论街道名称中代表的历史人物的其他方面，但现在这里有 [**Jupyter Notebook**](https://github.com/DeaBardhoshi/AlbaniaExplorations/blob/main/Albania%20Street%20Analysis%20%5BPart%201%5D.ipynb) 和 [**数据集**](https://github.com/DeaBardhoshi/AlbaniaExplorations/blob/main/Street_Names.csv)** 的链接。**

感谢阅读！
