# 地理空间数据工程：空间索引

> 原文：[https://towardsdatascience.com/geospatial-data-engineering-spatial-indexing-18200ef9160b?source=collection_archive---------0-----------------------#2023-08-31](https://towardsdatascience.com/geospatial-data-engineering-spatial-indexing-18200ef9160b?source=collection_archive---------0-----------------------#2023-08-31)

## 优化查询、提高运行时间和地理空间数据科学应用

[](https://deabardhoshi.medium.com/?source=post_page-----18200ef9160b--------------------------------)[![Dea Bardhoshi](../Images/14ce0986fc2a4a192797a52ed9908d1e.png)](https://deabardhoshi.medium.com/?source=post_page-----18200ef9160b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----18200ef9160b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----18200ef9160b--------------------------------) [Dea Bardhoshi](https://deabardhoshi.medium.com/?source=post_page-----18200ef9160b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd61c58ba988e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-engineering-spatial-indexing-18200ef9160b&user=Dea+Bardhoshi&userId=d61c58ba988e&source=post_page-d61c58ba988e----18200ef9160b---------------------post_header-----------) 在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----18200ef9160b--------------------------------) ·6分钟阅读·2023年8月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F18200ef9160b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-engineering-spatial-indexing-18200ef9160b&user=Dea+Bardhoshi&userId=d61c58ba988e&source=-----18200ef9160b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F18200ef9160b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-engineering-spatial-indexing-18200ef9160b&source=-----18200ef9160b---------------------bookmark_footer-----------)![](../Images/29fefc0d1aca68839035e1184d404196.png)

图片由 [Tamas Tuzes-Katai](https://unsplash.com/@tamas_tuzeskatai?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **介绍：空间索引为何有用？**

在进行地理空间数据科学工作时，优化你编写的代码是非常重要的。如何让拥有数亿行的数据库集合或联接更快？这就是空间索引等概念发挥作用的地方。在这篇文章中，我将讨论空间索引的实现方式，它的好处和局限性，并看看 Uber 的开源 H3 索引库在一些有趣的空间数据科学应用中的表现。我们开始吧！

# 🗺 什么是空间索引？

常规索引就像你在书籍末尾找到的内容：一个单词的列表以及这些单词在文本中出现的位置。这帮助你快速查找在特定文本中你感兴趣的单词的任何参考。如果没有这个方便的工具，你将需要手动翻阅书中的每一页，寻找你想阅读的那一个提及。

在现代数据库中，查询和搜索的问题也非常相关。索引通常比过滤更快地查找数据，你可以基于感兴趣的列创建索引。对于地理空间数据，工程师们通常需要考虑诸如“交集”或“附近”的操作。我们如何制作一个空间索引，使这些操作尽可能快速？首先，让我们看看一些地理空间数据：

![](../Images/c982cddb36a9445d0b96d1be3fa1e2a2.png)

两个不相交的特征（图片来源：作者）

假设我们想运行一个查询来确定这两个形状是否相交。根据构造，空间数据库创建的索引是由包含几何图形的边界框组成的：

![](../Images/44bf89146e3a9b186681beea21640257.png)

制作一个大的边界框（图片来源：作者）

为了回答这两个特征是否相交，数据库将比较两个边界框是否有任何公共区域。如你所见，这可能会迅速导致假阳性。为了解决这个问题，像 PostGIS 这样的空间数据库通常会将这些大的边界框划分成越来越小的部分：

![](../Images/7ef73239cf2fb2f4667a8165ed474620.png)

更小的尺寸：制作子边界框（图片来源：作者）

这些分区存储在 R 树中。R 树是一种层次数据结构：它跟踪大的“父”边界框、它的子节点、子节点的子节点，等等。每个父节点的边界框包含它子节点的边界框：

![](../Images/a1d9e21d516843ed561c8c5e9e0b2245.png)

R 树可视化（图片来源：作者）

操作“交集”是从这种结构中受益的关键操作之一。在查询交集时，数据库会沿着这棵树向下查询，问“当前的边界框是否与感兴趣的特征相交？”。如果是，它会查看该边界框的子节点，并提出相同的问题。通过这种方式，它能够快速遍历树，跳过没有交集的分支，从而提高查询性能。最后，它会返回所需的交集几何图形。

# 🧰 实践中：尝试使用 GeoPandas 的空间索引

现在让我们具体看看使用常规行级过程与空间索引的不同。我将使用代表纽约市人口普查区和市设施的两个数据集（均通过开放数据许可提供，可在[这里](https://data.cityofnewyork.us/City-Government/2010-Census-Tracts/fxpq-c8ku)和[这里](https://data.cityofnewyork.us/City-Government/Facilities-Database-Shapefile/2fpa-bnsx)获得）。首先，让我们在 GeoPandas 中尝试对一个人口普查区几何图形进行“交集”操作。在 GeoPandas 中，“交集”是一个逐行函数，检查每一行的列是否与我们的几何图形相交。

GeoPandas 还提供了一种使用 R 树的空间索引操作，允许我们执行交集操作。以下是对这两种方法在 100 次交集操作中的运行时比较（注意：由于默认的交集函数较慢，我只选择了原始数据集中的大约 100 个几何图形）：

![](../Images/0569dd39053e6799e0c77a2d9b3c0b72.png)

💨 空间索引快了多少？（图片由作者提供）

如你所见，空间索引方法相比普通的交集方法提供了显著的性能提升。实际上，这里是每种方法运行时间的 95% 置信区间：

![](../Images/c2db49b0dc45f54338391bf7ce0cb757.png)![](../Images/5376d53925f26a635b891b3d8d775adb.png)

太好了！那么，为什么我们有时不想使用空间索引呢？是否有些情况下它没有任何好处？确实有。这些限制中的一些是由于空间索引存储数据叶子的方式。结果表明，原始数据的分布方式会影响边界框在 R 树中的位置。具体来说，如果大量数据集中在同一地理空间，它们往往会共享相同的父节点，从而被分组在同一分支中。这可能导致树的不均衡，从而在查询时无法提供太多优化。

## 💻 其他空间索引是什么样的？

其他公司也适应了自己的空间索引。Uber 使用 H3，这是一个将世界划分为等面积六边形的分层索引系统。六边形在建模人们在城市中的移动或解决如计算半径等问题时具有许多优点。地理空间数据被划分到这些六边形中，作为公司的主要分析单位。网格是通过将 122 个六边形单元覆盖到二十面体地图投影上来构建的，支持包括聚合、连接和机器学习应用在内的广泛功能。

这个系统以及它的许多功能是开源的，可以在 GitHub 上进行分析。H3 API 的一个功能是将经纬度点转换为表示唯一六边形的字符串，根据指定的分辨率。我们来对整个设施数据库进行这个操作，并将六边形字符串转换为多边形：

在这些空间数据分析项目中，一个常见的问题是六边形中的项目有多少被某个列，比如“机构”所分类。幸运的是，现在我们已经将数据划分到 H3 六边形中，这很容易计算和可视化：

![](../Images/a45b3cfe9d8f6df4a64ed5e9983db703.png)

哪些机构有最多的设施？（图片来源于作者）

在这种情况下，你可以看到 DCAS（城市行政服务部）和 PARKS（公园与娱乐部）是每个六边形中设施最多的两个机构。这很有道理，因为这两个机构通常会有更多的实体设施（比如行政建筑或像公园这样的休闲区域）。

## 结论

如你所见，空间索引是地理空间数据科学和分析中非常有用的优化工具。在简单的交集查询中，使用空间索引显著提高了查询性能，相比于标准的 GeoPandas 交集函数。这种索引的实现和它的影响有许多细微之处，比如拥有巨大的数据集群分支。如我们所见，各公司已经开发了自己的解决方案：一个例子是 Uber 的 H3 开源索引，它允许我们回答各种空间分析问题。虽然我演示了基于机构的设施计数操作，但 H3 为其他更复杂的机器学习应用提供了基准。

如果你喜欢这类内容，但想要更广泛地了解城市规划技术，我也写了一份名为[“The Zoned Out Chronicles”](https://deabardhoshi.substack.com)的通讯。我鼓励你去看看！

感谢阅读！
