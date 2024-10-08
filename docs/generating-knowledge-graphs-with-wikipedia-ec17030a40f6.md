# 使用 Wikipedia 生成知识图谱

> 原文：[`towardsdatascience.com/generating-knowledge-graphs-with-wikipedia-ec17030a40f6`](https://towardsdatascience.com/generating-knowledge-graphs-with-wikipedia-ec17030a40f6)

## 快速生成知识图谱的 Python 指南

[](https://jyesr.medium.com/?source=post_page-----ec17030a40f6--------------------------------)![Jye Sawtell-Rickson](https://jyesr.medium.com/?source=post_page-----ec17030a40f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec17030a40f6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec17030a40f6--------------------------------) [Jye Sawtell-Rickson](https://jyesr.medium.com/?source=post_page-----ec17030a40f6--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec17030a40f6--------------------------------) ·5 分钟阅读·2023 年 1 月 1 日

--

![](img/946ca13307226697062b4dccf607ae8d.png)

图片由 [DeepMind](https://unsplash.com/@deepmind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/LIlsk-UFVxk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

知识图谱使我们能够理解不同知识点之间的关系，从而对一个领域或主题有深入的了解。这些图谱帮助我们辨别个别知识点如何结合形成更大的图景。显然，构建和可视化知识图谱可以是许多领域的有效方法。

在本文中，我们描述了一种通过利用处理人类知识的最大公开图谱——Wikipedia 来生成新的知识图谱的过程。我们将使用 Python 完全自动化生成过程，使我们能够为任何感兴趣的领域创建可扩展的知识图谱生成方法。

> 如果你想跟随操作，可以在 [Google Colab](https://colab.research.google.com/drive/1iKsJtRY-7gX_pGAHT2Cu3e75b3LztU63?usp=sharing) 中找到完整的 notebook。

# 方法

我们的方法如下：

+   🔌 使用 Wikipedia API 下载与术语相关的信息

+   🔁 迭代多个术语以建立知识库

+   🔝 根据“重要性”对术语进行排名

+   🌐 使用 networkx 库可视化知识图谱

如果你想与代码一同阅读，可以在 [Google Colab](https://colab.research.google.com/drive/1iKsJtRY-7gX_pGAHT2Cu3e75b3LztU63?usp=sharing) 中找到它。

# Wikipedia API

Wikipedia 通过 API 提供所有知识。此外，还有一个 [很棒的 Python 包](https://github.com/goldsmith/Wikipedia)，使得扫描网站变得简单。使用这个包，我们可以根据搜索词扫描 Wikipedia 页面，如下例所示。

```py
import wikipedia as wp

ds = wp.page("data science")
```

你可以在这篇文章中阅读更多关于该包的信息。

页面对象包含了我们需要的所有信息，以便遍历图谱并理解各种术语之间的关系。需要注意的关键属性有：

+   **links**：页面上链接到其他维基百科页面的外链

+   **content**：页面的实际内容

+   **summary**：页面顶部显示的关键信息。

下面展示了来自数据科学页面的一个示例。

维基百科网站庞大，拥有 700 万篇英语文章 ([Wikipedia, 2022](https://en.wikipedia.org/wiki/Wikipedia:Size_of_Wikipedia))，这意味着扫描每一个页面将成本高昂，并且会覆盖许多与兴趣主题无关的页面。因此，我们需要开发一个算法，仅搜索相关页面。

# 搜索维基百科

搜索算法应从兴趣点开始，然后从那里向外探索，确保尽可能靠近兴趣点，同时也确保捕捉到最重要的页面。

我们将遵循的算法是：

1.  **从覆盖兴趣领域的术语列表开始**。例如，对于“数据科学”的知识图谱，我们可以选择“数据科学”、“机器学习”和“人工智能”。

1.  **使用维基百科 API 从术语列表中获取维基百科页面**。

1.  **查找页面上的所有外链，并为它们计算一个权重**。权重可以基于术语出现的频率、距离文档开始的距离，或是否包含在摘要中。

1.  **将新链接添加到术语列表中**。

1.  **从剩余的术语中找出最重要的术语，并获取该术语的页面**。我们可以通过术语在其他术语中被引用的次数以及这些引用的权重来定义重要性。

1.  **重复步骤 3–5，直到达到足够的深度**。对于以下示例，这大约是几百个术语。

有了这些，我们可以开始构建一个以我们关心的主题为中心的整个维基百科数据库的本地图谱。

这些术语可以以术语列表的形式呈现，按其重要性排序。下面是“数据科学”的一个示例。

![](img/ba3caac80a80630e00b4d50eb4981c3a.png)

在“数据科学”知识图谱中按重要性排名的顶级术语。

列表是处理数据的一个有用工具，但我们还有更多的数据可以利用，因此让我们探索网络图。

# 创建网络图

在定义了网络后，我们可以开始对其进行可视化。鉴于数据的图形特性，最好以图表的形式查看。为此，我们可以使用实用的包 networkx。Networkx 是一个用于创建、操作和研究复杂网络结构、动态和功能的 Python 包 ([Networkx, 2022](https://www.notion.so/fbd0d31f1fdb4f4289f673e0885189ec))。

Networkx 在基本图论的基础上构建图。下面展示了一个示例绘图脚本。

```py
import networkx as nx

G = nx.Graph()
G.add_nodes_from(nodes)
G.add_edges_from(edges)

nx.draw(G)
```

为了绘制网络，我们需要使用比本示例中显示的更复杂的函数。特别地，我们将使用加权节点和加权边，分别基于单个术语的重要性及其连接。

“数据科学”、“物理学”和“生物学”的绘图如下所示。

![](img/abe4f6902a92c8b64d2d0db5e1e71b54.png)![](img/18630da9acd6f7ef5e4ca67c8b3a944c.png)

“数据科学”（左）和“物理学”（右）的知识图谱。

![](img/92a7e51449005848242b954119adec1a.png)

“生物学”术语的知识图谱。

从生物学领域来看，我们看到一个有趣的图表。我不是生物学家，但这似乎相当准确！一些值得关注的点：

+   术语**动物**紧挨着生物学，并且具有类似的重要性，这很合理，因为生物学是研究生物体的学科。

+   左侧我们看到一个与细胞相关的生物学集群：**变形虫**、**细胞壁**、**减数分裂**和**细菌**。由于这些术语之间的强链接，networkx 算法将它们分组在一起。通过这种方式，知识图谱可以建议一起学习的术语。

+   鉴于生物学与其环境之间的强链接，我们看到**地质学**领域通过**地球**和**地层学**等术语显现出来。

+   正如近期事件所预期的那样，我们看到**气候**、**气候变化**和**时间**等问题被提升为重要话题。如果这是 20 年前的知识图谱，情况可能会有所不同。

+   我们看到了一些看似不相关的术语**1**、**数字**和**isbn**。这可能是由于维基百科中的一些奇怪引用，应予以删除。

# 让教育变得更容易

在这里，我们展示了一种从兴趣领域到完整知识图谱的方法。它允许我们获取按重要性排名的术语列表，以及这些术语如何相互适应的可视化。

这样，这些图表可以对教育和学习新领域有用。这对个人学习确实如此，但对于学费和更广泛的教育也适用，因为课程通常会在知识上留下空白。

提醒：完整的笔记本可以在[Google Colab](https://colab.research.google.com/drive/1iKsJtRY-7gX_pGAHT2Cu3e75b3LztU63?usp=sharing)中找到，请随意试用，并告诉我你的发现！
