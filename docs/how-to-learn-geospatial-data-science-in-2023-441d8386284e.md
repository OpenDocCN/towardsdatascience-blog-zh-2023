# 如何在 2023 年学习地理空间数据科学

> 原文：[https://towardsdatascience.com/how-to-learn-geospatial-data-science-in-2023-441d8386284e?source=collection_archive---------1-----------------------#2023-02-21](https://towardsdatascience.com/how-to-learn-geospatial-data-science-in-2023-441d8386284e?source=collection_archive---------1-----------------------#2023-02-21)

## 对于那些想要学习使用 Python 进行地理空间数据分析的步骤指南 

[](https://cordmaur.medium.com/?source=post_page-----441d8386284e--------------------------------)[![Maurício Cordeiro](../Images/1ec750bf68bbaa0331fabdebebf28eb5.png)](https://cordmaur.medium.com/?source=post_page-----441d8386284e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----441d8386284e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----441d8386284e--------------------------------) [Maurício Cordeiro](https://cordmaur.medium.com/?source=post_page-----441d8386284e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8878c77fe1a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-learn-geospatial-data-science-in-2023-441d8386284e&user=Maur%C3%ADcio+Cordeiro&userId=8878c77fe1a3&source=post_page-8878c77fe1a3----441d8386284e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----441d8386284e--------------------------------) ·11 分钟阅读·2023 年 2 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F441d8386284e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-learn-geospatial-data-science-in-2023-441d8386284e&user=Maur%C3%ADcio+Cordeiro&userId=8878c77fe1a3&source=-----441d8386284e---------------------clap_footer-----------)

-- 

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F441d8386284e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-learn-geospatial-data-science-in-2023-441d8386284e&source=-----441d8386284e---------------------bookmark_footer-----------)![](../Images/ed30e52db3fd6f9bee19e96c4a69c338.png)

使用 Dall-E 2 创建的数字图像。说明：一名科学家手持地球的照片。

# 介绍

为什么这个话题很重要？众所周知，数据科学职业如今需求量很大。当你将地理空间分析的维度加进去时，可能性变得更加令人兴奋。气候变化、食品生产以及向零碳经济的过渡只是需要深入理解地理空间数据的众多重要问题中的几个。通过结合卫星和无人机图像、矢量数据集以及实地测量，我们可以获得更深刻的见解，并推动有意义的变革。

虽然有很多资源可以用于学习使用 Python 进行地理空间数据分析，但这个领域正在快速发展。在过去，我管理了一个 GIS 团队，我们在水资源映射和水文学分析中严重依赖 ArcGIS 或 ENVI 等软件。然而，我与使用 Python 进行地理空间分析的旅程始于 2019 年，当时我开始了我的博士学习。

本周我偶然发现了《Python Geospatial Development》这本书。起初，我认为这将是提升技能的绝佳机会。然而，在查看了目录后，我感到很惊讶。除了 GDAL（我在开始时简要使用过，但很快被 Rasterio 取代）外，我几乎没有使用第 3 章——地理空间开发的 Python 库中讨论的任何 Python 库。这种差异可以归因于这本书首次出版于 2010 年，且在 2013 年和 2016 年仅进行了小幅更新。在计算机科学的世界里，那已经是很久的时间了。

尽管由于其驱动程序的原因仍然需要安装 GDAL，但其 Python 绑定已经不再必要，并且使用起来可能会令人困惑（姑且这么说）。此外，我从未听说过如 Mapnik 这样的其他库。此外，像 GeoPandas 或 XArray 这样的基本包在书中甚至没有提及。最后，Google Earth Engine（这一领域的真正变革者）在书籍出版时甚至尚不存在，这突显了实际开发书籍的一个常见问题——它们很快会变得过时。

嗯，我并不是反对书籍。书籍仍然对构建特定学科的坚实基础至关重要。然而，要成为一名实用的地理空间分析师，学习如何在大量在线信息中导航并理解这些信息至关重要，因为对于新手来说，这些信息可能会让人感到不知所措。

考虑到这一点，本文是为那些希望在 2023 年学习使用 Python 进行地理空间数据分析的人编写的，提供了有关主要技能和主题的快速指南，并给出了一些如何有效导航大量在线信息和避免某些陷阱的建议。请注意，这份指南基于我自己的经验，可能并不完全符合每个人的期望。

# 避免陷阱

在开始之前，你需要注意一些陷阱。我列出了四个需要注意的陷阱，但我相信在你的学习过程中还会遇到更多：

**1-** **信息过载：** 地理空间数据科学的世界广阔，知道从哪里开始可能会很具挑战性。如果你开始搜索互联网，你会被大量的信息、文章和课程所吓倒。我知道这令人不知所措。突然间，你发现自己只是从一个网站跳到另一个网站，整整一天过去了却没有实际学到任何东西。为了避免感到畏惧，专注于一个主题一次，并避免干扰。使用你喜欢的方法：番茄工作法、Flowtime，或 Deepwork。没关系，只要你保持专注！

![](../Images/8707a3938f76e3f156869adaae71e36f.png)

照片由 [Usman Yousaf](https://unsplash.com/pt-br/@usmanyousaf?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**2- 过时的资源：** 这个问题与我在介绍中提到的内容相关……如果你得到一个过时的资源，你可能只是浪费时间，毫无进展。甚至更糟，你可能会回到……过去！

> **注意：** 如果你在学习一些基础知识如统计学或物理学，这点可能不太相关，但如果你涉及某些技术方面，它可能会起到重要作用。

**3- 尊重你的学习曲线：** 我开始时遇到的第三个问题是，我试图解决一些高级话题，例如创建一个云中的虚拟机来托管地图服务器，但甚至不了解 Docker 或 Git 是什么。结果，我花了很多时间去解决超出我能力范围的问题。例如，如今，当我想发布一张地图时，我只需将 COG（云优化的 Geotiff）直接发布到 Github 上（例如，[https://cordmaur.github.io/Fastai2-Medium/occurrence_map.html](https://cordmaur.github.io/Fastai2-Medium/occurrence_map.html)），无需设置地图服务器等繁琐操作。但我只有在研究该主题一段时间后才知道 COG 的存在。

![](../Images/98f871369700b5807563ee0f9990c354.png)

图 1: 直接托管在 Github 上的 COG 文件。图片由作者提供。

**4- 不要试图从头到尾理解一切：** 在这里，我们必须使用一些推理。掌握主要话题并迅速补充任何可能缺失的重要概念是至关重要的。但你应该做到这一点，及时地。例如，假设你正在运行一个在过程中使用导数的机器学习模型。如果你像我一样，你会被诱惑去寻找最好的微积分课程，即使你在大学里已经学过两年。请不要陷入这个陷阱；深入理解所有潜在主题是不可能的。相反，进行快速回顾，以保持进度。

# 如何开始

如前所述，掌握地理空间数据科学所需的一切可能会让人感到不知所措，并且需要时间。如果你试图按照“正确”的顺序学习所有内容，你可能会感到气馁并放弃。在这方面，我同意 fast.ai 的 Jeremy Howard 的观点，他建议采取自上而下的方法。这样，即使你不完全理解所有的底层内容，也可以看到有意义的结果。

所以这是我如果从头开始并尽快学习的话，会考虑的“模糊”顺序。

![](../Images/638fbed3096fa5e84df5554afe97b60f.png)

使用 Dall-E 2 创建的数字图像。标题：一只泰迪熊爬楼梯的照片，背景是地球。

## 1- 基础 Python 编程

这是一个至关重要的主题。如果没有它，学习矢量数据集或栅格数据集等内容将毫无意义，因为一切都会显得抽象，你也没有工具去探索它。我知道你可以使用 QGIS 或其他 GIS 软件。但这与将原始数据导入到 **numpy** 数组或 **geojson** 文件并自己探索是不同的。此外，首先学习 Python 编程可以打开你以前没有想象过的其他门。

> **注意：** 请记住，在开始时，你不会编写程序。你只是依赖第三方库来进行分析。因此，此时不建议使用为程序员设计的资源。你可以阅读[“如何正确学习 Python 数据科学”](https://www.kdnuggets.com/2019/06/python-data-science-right-way.html)来理解其中的区别。

因此，为了学习最基本的内容，我建议选择像 Codecademy 的‘[**学习 Python 3**](https://www.codecademy.com/learn/learn-python-3)’这样的课程。该课程只需 25 小时即可完成，时间不足一周，并且提供了对语言的快速概述。此外，我在我的 YouTube 频道上发布了四节名为‘[**科学家 Python 入门**](https://www.youtube.com/watch?v=oQaoj6LE5E4&list=PLw8LEAbnPlchA1fO8QMnqUCTP_71VwpGg)’的课程，这也可以是一个很好的主题入门。

![](../Images/fe4a17c0b31803fba03e3246f6d1838c.png)

使用 Dall-E 2 创建的数字图像。标题：一个小孩在老式计算机上编程的铅笔画。

之后，我建议学习 Jake VanderPlas 编写的《Python 数据科学手册》，可以在作者的 GitHub 上免费获取（[https://jakevdp.github.io/PythonDataScienceHandbook/](https://jakevdp.github.io/PythonDataScienceHandbook/)）。这本书并不专注于讲解 Python 语言的内部工作，如创建复杂函数、定义类、继承或其他面向对象的方面。相反，它提供了对主要库的快速概述，如 **numpy**、**pandas** 和 **matplotlib**，这些都是数据分析中必不可少的工具。

## 2- GEE 和 GIS 基础

现在你已经对 Python 及其主要的数据分析库有了基本了解，是时候开始接触一些 GIS 概念了。参加一个通用的 GIS 课程可能会让你被关于卫星数据收集、大气中的光路径以及这些如何影响传感器读数的信息轰炸。虽然这些主题对于研究很重要，但对于地理空间数据分析的实际入门可能并不必要。

![](../Images/1bd3df1f32cd59b027d4727976fc8bbb.png)

照片来自于[Krzysztof Hepner](https://unsplash.com/@nsx_2000?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

从实际角度出发，我建议通过一个使用 Python 的**Google Earth Engine**实践课程来学习 GIS 基础知识。这样，你将练习 Python 并可以快速访问地理空间数据（包括矢量数据和栅格数据），而无需担心下载或设置复杂的环境。你将立即开始进行首次空间分析。

这样的课程可以在**Udemy**上找到：[Google Earth Engine Python API中的空间数据分析](https://www.udemy.com/course/spatial-data-analysis-with-earth-engine-python-api/)或[完整的 Google Earth Engine Python API & Colab 培训营](https://www.udemy.com/course/complete-google-earth-engine-python-api-colab/)。另一个选择是参加一个完整的 GEE 课程，该课程将使用 JavaScript 环境，但一切可以通过 Python 进行复制，语法仅需稍作调整。

## 3- 地理空间 Python 库

Google Earth Engine (GEE) 功能强大，提供大量现成的数据，但也有一些缺点。一切都必须在 Google 云端运行。虽然它提供免费访问其资源，但对于大规模处理可能会产生费用。因此，理想的解决方案是使用开源 Python 库来处理来自任何来源的数据。主要库包括：

+   rasterio：用于读取栅格数据并将其加载到数组中。

+   geopandas：类似于 pandas，但具有用于矢量数据的空间字段。

+   xarray：类似于 numpy，但支持坐标和尺度。

+   shapely：用于矢量处理、剪切、交集等。

+   fiona：用于读取标准的 shapefiles。

+   leafmap：用于动态地图可视化。

## 4- 项目和 Python 环境管理

假设你在没有 Python 编程基础的情况下达到了这一点。在这种情况下，你可能有一个你不完全理解的混乱工作环境，以及一堆不相关的 Jupyter notebooks，使得很难找到你上周发现的重要代码片段。如果你处于类似情况，不要惊慌！每个新手都会遇到这种情况。现在是时候退一步整理你的工作环境了。为此，你需要了解：

+   包管理工具如 Pip 和 Conda 的重要性。你还可以探索 Miniconda 和 Mamba，以获得更快的性能；

+   如何为你的 Jupyter notebooks 创建一个内核；

+   如何使用 Git 控制你的工作版本并通过 GitHub 进行备份。

## 5- 深入了解 Python 编程

到现在为止，你可能一直在使用 Jupyter notebooks 进行实验。它是一个出色的数据探索和分析工具，但不要指望用它创建一个完全功能的软件（除非你采用了 [**nbdev**](https://nbdev.fast.ai/)）。此外，编写一个执行特定分析的 notebook 与编写一个将部署到服务器上的完全操作的软件是不同的。我在之前的一篇文章 [**"为什么数据科学家应该适度使用 Jupyter Notebooks"**](https://medium.com/towards-data-science/why-data-scientists-should-use-jupyter-notebooks-with-moderation-808900a69eff) 中讨论了这个问题。

基本上，你需要一些 IDE（如 PyCharm 或 VSCode）来开发函数、类和包。Jupyter notebooks 将用于调用你创建的内容并展示结果。记住列表中的第一个项目，当我提到不要使用针对程序员的 Python 资源时？现在是时候提升你的编程技能了。

## 6- 高级话题（天空是极限）

接下来，你会很快意识到地理空间分析中的许多任务是易于扩展的，这取决于可用的数据量和处理能力。然而，要进行扩展，你需要了解如何部署具有更多资源的云服务器以及如何进行分布式计算。在这些情况下，了解新技术如以下内容很重要：

+   Dask 库：Dask 是一个用于分布式计算的 Python 库。它可以帮助你处理那些无法完全装入基础内存的大量数据。

+   Docker：Docker 允许你构建、打包和部署作为容器的环境和应用。这些容器可以部署到云服务器上，以提高效率和可扩展性。

![](../Images/903c66c883fb25600fe8d527e08d1bcb.png)

使用 Dall-E 2 创建的数字图像。标题：牛仔骑着火箭飞往月球

## **7- 数据科学中的数学和统计**

你确定吗？你可能会认为这不是一切的基础？其实，这个话题与陷阱 #4 紧密相关。如果你有良好的数学基础，你应该只挑选那些你真正遇到问题的主题，准确地说。为此，**Khan Academy** ([https://en.khanacademy.org](https://en.khanacademy.org/)) 是一个很棒的免费资源，讲座被分成了大约 10 分钟的短视频。

如果你认为你的问题仅在于统计学，你可以考虑 David Forsyth 编写的《计算机科学的概率与统计》这本书，由 Springer 出版。

另一方面，如果你没有扎实的数学基础，应该考虑购买一个专注于数据科学数学的课程。在 CodeAcademy（[数据科学基础数学](https://www.codecademy.com/learn/paths/fundamental-math-for-data-science)）或 Udemy（[数据科学、数据分析和机器学习数学](https://www.udemy.com/course/math-for-data-sciencedata-analysis-and-python-programming/)）等平台上有很多这样的课程。这些课程价格不贵，并且可以给你一个良好的概述。

# 结论

总结来说，我相信地理空间数据科学是我们时代最激动人心和重要的领域之一。通过将 Python 的强大功能与丰富的地理空间数据结合起来，我们有潜力在气候变化、食品生产和公共健康等领域推动有影响的变化。然而，要成为这一领域的熟练从业者，专注于正确的主题和工具非常重要。根据我的经验，这意味着要涵盖以下步骤：

1- 基础 Python 编程

2- GEE 和 GIS 基础

3- 地理空间 Python 库

4- 项目管理和 Python 环境

5- Python 编程深入探讨

6- 高级主题（天高地阔）

7- 数据科学中的数学和统计

在这段旅程中，你还应该注意那些可能会拖慢你进度的常见陷阱和误区。拥有这些技能和资源，我相信任何人都可以学习地理空间数据科学，并在世界上产生真正的影响。

# 保持联系

*如果你喜欢这篇文章，考虑成为一个* [*Medium 会员*](https://cordmaur.medium.com/membership) *，解锁像这样的数千篇文章。费用仅为每月 $5。*

[](http://cordmaur.medium.com/membership?source=post_page-----441d8386284e--------------------------------) [## 使用我的推荐链接加入 Medium - Maurício Cordeiro

### 阅读 Maurício Cordeiro（以及 Medium 上其他成千上万位作者）的每个故事。你的会员费用直接…

cordmaur.medium.com](http://cordmaur.medium.com/membership?source=post_page-----441d8386284e--------------------------------)
