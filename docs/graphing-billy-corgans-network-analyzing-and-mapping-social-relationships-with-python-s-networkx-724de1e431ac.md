# [比利·科根网络图谱：使用 Python 的 NetworkX 库分析和绘制社会关系——第四部分](https://towardsdatascience.com/graphing-billy-corgans-network-analyzing-and-mapping-social-relationships-with-python-s-networkx-724de1e431ac)

> 原文：[`towardsdatascience.com/graphing-billy-corgans-network-analyzing-and-mapping-social-relationships-with-python-s-networkx-724de1e431ac`](https://towardsdatascience.com/graphing-billy-corgans-network-analyzing-and-mapping-social-relationships-with-python-s-networkx-724de1e431ac)

## 继续学习如何使用 NetworkX 和 Python 进行社会网络分析

[](https://christineegan42.medium.com/?source=post_page-----724de1e431ac--------------------------------)![Christine Egan](https://christineegan42.medium.com/?source=post_page-----724de1e431ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----724de1e431ac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----724de1e431ac--------------------------------) [Christine Egan](https://christineegan42.medium.com/?source=post_page-----724de1e431ac--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----724de1e431ac--------------------------------) ·阅读时间 11 分钟·2023 年 7 月 21 日

--

[在我们对比利·科根影响力领域的调查开始时](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-efeb82ab853e)，我们介绍了社会网络分析以及像节点和边这样的基本概念。在[第二部分](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57)中，我们通过绘制 [Smashing Pumpkins](https://smashingpumpkins.com/) 和 [Zwan](https://en.wikipedia.org/wiki/Zwan) 乐队成员之间的关系，扩展了对社会网络分析的理解。接着，我们考察了度数中心性和中介中心性等指标，以调查不同乐队成员之间的关系。同时，我们讨论了领域知识如何帮助我们理解结果。

在[第三部分](https://medium.com/towards-data-science/closeness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223)中，我们介绍了第三种中心性度量——[接近中心性](https://neo4j.com/docs/graph-data-science/current/algorithms/closeness-centrality/)。我们还开始讨论社区和子群体的概念，并展示了不同的社区图以及我们如何利用接近中心性来指导我们的解释。通过使用曾经是 Zwan 和 Smashing Pumpkins 乐队成员的音乐网络，我们对成员之间的关系做出了推测。

这一次，我们将通过扩展网络和添加更多乐队使结果更有趣。同时，我们将扩展对中心性度量和社区概念的理解，同时提升 Matplotlib 技能，使你的 NetworkX 图表更加引人入胜。

![](img/98ff5a512ec3f8d30e0fbb5bb4bc4023.png)

[Tool — By Lugnuts — 自制作品, CC BY-SA 4.0](https://commons.wikimedia.org/w/index.php?curid=117730065)

# 向网络中添加复杂性

在之前的内容中，我们涵盖了社交网络分析中的三个基本指标：**度中心性**、**中介中心性**和**接近中心性**。我们还讨论了**社区**的概念，并描述了如何将这一框架应用于理解构成 Billy Corgan 网络的社区/乐队之间的网络动态。

![](img/24010914bf66ddc999448305ac2ffd6b.png)

还记得上次的内容吗？这是我们用 Matplotlib 创建的 NetworkX 图。

尽管即使是小型音乐团体也能展示有趣的网络动态，但网络的复杂性不足使我们的结果变得不那么有趣，也没有提供超出常识的见解。这一次，我们将向网络中添加更多的乐队和音乐人，以观察社区的出现以及音乐人之间的连接如何影响网络动态。

## 另一种影响范围

另一个在 1990 年代出现的受欢迎的摇滚乐队是[Tool](https://loudwire.com/tags/tool/)——以其独特和前卫的摇滚音乐风格闻名。Tool 常常结合另类金属、前卫摇滚和艺术摇滚元素，形成了独特而实验性的声音。他们的歌曲常常融入隐喻和寓言式的叙事，留给听众解读的空间。

从数据科学的角度来看，Tool 是一个有趣的选择，因为他们在音乐中以各种方式融入了数学主题。虽然他们没有明确使用数学公式或方程（换句话说——我们不能完全称他们为[数学摇滚乐队](https://en.wikipedia.org/wiki/List_of_math_rock_groups)），但他们经常使用复杂的拍号和节奏模式，这些可以被视为受数学启发的。[Tool 音乐中更著名的例子之一](https://www.youtube.com/watch?v=nI63B3cY7q0)是歌曲[Lateralus](https://en.wikipedia.org/wiki/Lateralus_(song))中的斐波那契数列和黄金比例。

![](img/af3ad3f641069c3a1179b2985e17763c.png)

Maynard James Keenan © Markus Felix | PushingPixels ([联系我](https://commons.wikimedia.org/wiki/User_talk:MarkusFelix)), CC BY-SA 4.0, 通过维基媒体公用领域

粉丝们会注意到，Tool 和 A Perfect Circle 的主唱 Maynard James Keenan 还有其他项目，我们将留到未来的内容中。但这些与 Billy Corgan 有什么关系呢？让我们用中心性度量来调查一下。

## 开始之前…

1.  *你对***Python****有基本了解吗？如果不熟悉，* [*从这里开始*](https://medium.com/towards-data-science/virtual-environments-for-python-data-science-projects-on-mac-os-big-sur-with-pyenv-and-virtualenv-60db5516bf06)*。*

1.  *怎么样* ***Pandas****？如果不熟悉，* [*从这里开始*](https://medium.com/towards-data-science/pandas-i-read-csv-head-tail-info-and-describe-43b9b2736490)*。*

1.  *你对***社会网络分析的基本概念****如节点和边缘是否熟悉？如果不熟悉，* *从这里开始**。*

1.  *你对社会网络分析的概念是否感到舒适****，*** *比如* ***度中心性*** *和* ***介数中心性****？如果不熟悉，* [*从这里开始*](https://medium.com/towards-data-science/visualizing-social-networks-for-better-insights-analyzing-and-mapping-social-relationships-with-f4a9cf6b6d57)*！怎么样* ***社区****？如果不熟悉，* [*从这里开始*](https://medium.com/towards-data-science/closeness-and-communities-analyzing-social-networks-with-python-and-networkx-part-3-c19feeb38223)*。*

[](https://christineegan42.medium.com/membership?source=post_page-----724de1e431ac--------------------------------) [## 使用我的推荐链接加入 Medium - Christine Egan

### 作为 Medium 会员，你的一部分会员费用将用于支持你阅读的作者，你将可以完全访问每一篇故事……

christineegan42.medium.com](https://christineegan42.medium.com/membership?source=post_page-----724de1e431ac--------------------------------)

# 使用 Python 和 NetworkX 计算中心性测量

在下面的代码单元中，我们将通过将 **Tool** 和 **A Perfect Circle** 添加到我们的 1990 年代音乐家网络中来扩展我们的网络。然后，我们将使用中心性测量来分析网络。

## 1\. 构建社区

在下面的代码单元中，我们导入 NetworkX 和 Matplotlib。就像我们在 第三部分 中所做的那样，我们使用 NetworkX 创建一个图对象，并列出每个乐队的所有成员。然后，我们将所有乐队合并，并将其分配给变量 `communities`。

***注意：当前和前成员都包括在成员列表中。***

接下来，我们将使用 for 循环遍历乐队成员列表。然后，我们使用 `.add_nodes_from()` 将每个乐队成员添加到网络图中。

为了连接同一 `community`（即乐队）的成员，我们输入一个包含每个乐队成员的列表推导式，并消除任何重复（例如，`(Billy Corgan, Billy Corgan)`）到 `.add_edges_from()` 中。

## 2\. 计算中心性测量

数据准备好，网络图填充完成后，我们可以调用 NetworkX 中的方法来计算中心性测量并分析网络。结果将存储在 [Pandas 数据框](https://medium.com/towards-data-science/pandas-i-read-csv-head-tail-info-and-describe-43b9b2736490)中。

输出应类似于下面的样子：

这比我们上一集的结果有趣得多，上集的结果如下：

在我们使用 Matplotlib 构建 NetworkX 图的可视化之前，让我们消化一下关于我们生成的表格的观察结果。为此，我们将描述每个中心性指标，然后根据我们的数据结果进行解释。

## 1\. 节点介数中心性

*(量化一个节点在信息或交互流动中作为桥梁或中介的频率。)*

当我们的网络仅包含两个乐队（Smashing Pumpkins 和 Zwan）时，Billy Corgan 和 Jimmy Chamberlain 在节点介数中心性中排名最高。由于 Tool 和 A Perfect Circle 共享一个共同的主唱，我们看到 Maynard James Keenan 取代了一些介数，降低了 Billy 和 Jimmy 之间的介数。然而，新的最高介数中心性的乐队成员是 **James Iha**。这表明 James Iha 是连接这些*子组*的节点。利用领域知识，这一判断是正确的。作为 Smashing Pumpkins 和 A Perfect Circle 的成员，他是 Billy Corgan 的乐队成员和 Maynard James Keenan 的乐队成员之间的共同联系。

## 2\. 度中心性

*(衡量一个节点（个体）在网络中具有的直接连接（边）的数量。)*

**James Iha** 拥有最高的度中心性分数（0.782609），这表明他与网络中其他个体有着最多的直接连接。这是因为与 Zwan 和 Tool 不同，Smashing Pumpkins 和 A Perfect Circle 有一个轮换的成员阵容，所有这些成员都与 James Iha 有联系。

![](img/30a30b93675d91e30253bb6e8e0a0b32.png)

James Iha by Tiffany Bauer, [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/), via [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:James_Iha.jpg)

## 3\. 接近中心性

接近中心性衡量一个节点与网络中所有其他节点的接近程度，考虑了最短路径。在这种情况下，James Iha 拥有最高的接近中心性分数（0.821429），这表明他能够迅速有效地接触到网络中的其他个体。

这些中心性指标提供了一种识别关键个体的方法，这些个体具有重要的联系，充当桥梁，并能够有效地接触到网络中的其他个体。

现在我们已经计算了中心性指标，我们可以使用 Matplotlib 创建一个 NetworkX 图。这一次，我们将通过添加颜色、标签、标题等，使图表更加引人入胜。

# 使用 Matplotlib 可视化 NetworkX 图

我们在上一节中创建的表格提供了有关网络中有影响力的音乐家的重要信息。为了使这些结果更直观，我们可以使用 [Matplotlib](https://matplotlib.org/) 和 NetworkX 创建图形的可视化。在之前的章节中，我们保持了图形的相对简单。这一次，我们将使用一些高级功能来使我们的可视化特别吸引人。

提醒一下，这就是我们之前网络图的样子：

![](img/24010914bf66ddc999448305ac2ffd6b.png)

来自 Closeness 和 Communities 的 Smashing Pumpkins 和 Zwan 的 NetworkX 图

## 1\. 使用 Matplotlib 添加颜色

如果你查看 [NetworkX 文档](https://networkx.org/documentation/stable/reference/generated/networkx.drawing.nx_pylab.draw_networkx_nodes.html) 中的 `.draw_networkx_nodes()` 方法，你会注意到 `node_color` 参数接受 `color` 或 `array of colors`。在我们的情况下，我们想使用 `array of colors` 以便可以根据成员所在的乐队为每个节点分配颜色。

在进入这些细节之前，让我们选择一些颜色。 🎨

在使用 Matplotlib 创建图表时，你可以使用 [命名颜色或十六进制代码](https://matplotlib.org/stable/tutorials/colors/colors.html)。有关颜色选择和可用的多种技术的更多信息，请查看 [文档](https://matplotlib.org/stable/tutorials/colors/colors.html#comparison-between-x11-css4-and-xkcd-colors)（他们甚至提供了代码，以便你可以自己生成这个图表 🤩）。

![](img/2935b189a87dc15e38b5f051c09d1e09.png)

[用于 Matplotlib 的命名颜色](https://matplotlib.org/stable/gallery/color/named_colors.html)

在我们的案例中，目标是创建一个乐队名称的字典，这些乐队名称映射到我们选择的颜色。然后，我们希望遍历节点，并将该节点的选定颜色添加到列表中。由于我们想强调社区之间的连接，我们将为出现在多个乐队中的成员选择一种特殊颜色。

为了实现这一点，我们使用 `for-loop` 迭代网络中的每个节点。然后，我们使用一系列 `if-else` 语句将特殊颜色分配给在 `flat_communities` 中出现多次的成员。如果该成员的节点仅出现一次，则根据成员所属的乐队分配颜色。

使用上面的代码块，我们生成了二十四种颜色的列表。这直接映射到网络中的二十四个节点列表。

这个输出将作为 `.node_colors()` 的输入，当我们调用 `.draw_networkx_nodes()` 开始填充我们图形的可视化时使用。

## 2\. 绘制图形和计算节点位置

为了开始绘制可视化图，我们调用 Matplotlib 的 `.figure()` 并将 `figsize` 设置为 20 x 12，为我们的网络图提供空间。

然后，我们使用 NetworkX 的 `.spring_layout()` 来计算网络中每个节点的位置。这将使我们能够在可视化中绘制节点。

## 3. 绘制节点和边

接下来，我们将使用 `.draw_networkx_nodes()` 和 `.draw_networkx_edges()` 来绘制每个节点及其连接。让我们讨论每个参数的意义。

**.draw_networkx_nodes()**

+   `G`：NetworkX 图对象（`G`）

+   `pos`：我们上面生成的每个节点的位置（`pos`）

+   `node_color`：我们生成的颜色列表（`node_colors`）

+   `node_size`：每个节点应该有多大？

+   `alpha`：每个节点的透明度应该是多少？

**.draw_networkx_edges()**

+   `G`：NetworkX 图对象（`G`）

+   `pos`：我们上面生成的每个节点的位置（`pos`）

+   `edge_color`：边的颜色应该是什么？

+   `alpha`：每条边的透明度应该是多少？

在 Python 中，它的样子是这样的

## 4. 为 NetworkX 图添加标签

要添加标签，我们可以使用 NetworkX 方法 `.draw_networkx_labels`。

让我们逐一了解每个参数的意义。我们将跳过最近回顾过的参数，如 `G` 和 `pos`。然后，我会向你展示如何将它们结合起来。

1.  `font_size`：此参数设置节点标签的字体大小。它指定了代表节点标签的文本应该有多大或多小。

1.  `font_color`：此参数设置节点标签的颜色。你可以使用各种格式来指定颜色，如命名颜色（‘red’，‘blue’ 等）或十六进制值（‘#RRGGBB’）。

1.  `verticalalignment`：此参数控制节点标签相对于节点的垂直对齐。例如，将其设置为 `’top’` 会将标签对齐到节点的上方。

1.  `bbox`：此参数是一个字典，包含节点标签的边界框（bbox）的属性。它通常用于定义一个背景框或一个围绕标签的矩形，以提高可见性和可读性。

1.  `labels`：这是一个可选参数，允许你指定一个包含自定义节点标签的字典。字典的键应为节点 ID（通常为整数），值应为你想要分配给每个节点的标签。如果未提供此参数，将使用默认的节点标签（通常为节点 ID）。

这段代码将会是这样的：

## **5. 将图例添加到 Matplotlib 可视化中**

最后，我们想要添加一个图例来为观众提供上下文。为此，我们需要创建三个列表（`legend labels`、`legend colors` 和 `legend markers`），然后将这些列表作为参数传递给 `plt.legend()`。我们还将使用可选参数 `loc`、`fontsize`、`frameon`、`borderpad` 和 `borderaxespad` 来美化图例。

让我们回顾一下我们选择的每个可选参数的目的。

1.  `loc`：图例应该位于屏幕的哪个位置？

1.  `fontsize`：确定图例的文字大小。

1.  `frameon`：在图例周围添加边框。

1.  `borderpad`：在边框和文本之间添加一点填充空间。

1.  `borderaxespad`: 图例边框和坐标轴之间的间距，单位为字体大小。

这些只是你可以用来美化 matplotlib 图例的一些可选参数。查看[文档](https://matplotlib.org/stable/api/legend_api.html)以回顾所有参数，以及你可以提供的可能值。

对于 `legend labels`，我们只需提供每个乐队的名称。对于 `legend_colors`，我们引用我们创建的字典 `color_palette`，并使用键值来定义每个乐队的颜色。接下来，`legend_markers` 将标记位置排列在一条直线上，并定义它们的大小和形状。

最后，我们用 `plt.legend()` 将所有内容整合在一起。这是它的样子：

## 6\. 添加最后润色

为了给我们的 NetworkX 图添加一个总结标题，我们可以通过调用 plt.title() 来添加标题，然后使用 `fontsize`、`fontweight` 和 `pad` 来美化标题。

通过使用 `off` 参数与 plt.axis()，我们可以去除网络图的边框，从而实现更均匀的间距和可读性。

调用 `plt.tight_layout()` 自动调整图形元素的位置，以获得更好的间距和可读性。

最后，我们只需调用 `plt.show()` 来显示我们的可视化！

这是最终产品：

![](img/de431df9f8965377424ef564b5086e8a.png)

使用 Matplotlib 创建的 NetworkX 图“比利·科根的音乐人网络”

# 解读 NetworkX 图

在本教程中，我们使用比利·科根的网络来演示社会网络分析的基础概念，通过构建第一部分、第二部分和第三部分中的网络。这一次，我们通过添加两支额外的乐队 Tool 和 A Perfect Circle，使网络变得更加复杂。这使我们能够观察到网络中额外个体（节点）的影响，以及这对中心性度量的影响。

我们发现詹姆斯·伊哈是比利·科根（Smashing Pumpkins 和 Zwan）关联的音乐人社区与梅纳德·詹姆斯·基南（Tool 和 A Perfect Circle）之间的重要中介。这个结果很有趣，因为它表明主唱不一定是大网络中最有影响力的参与者。如果我们考虑如何在大规模网络中使用这种技术，我们可以欣赏 NetworkX 图的实用性，以及它们如何帮助我们快速识别网络中的重要节点。然后，我们可以使用度中心性、中介中心性和接近中心性等指标来突出我们的发现中的细微差别。最后，我们结合领域知识对结果进行解释。

所以，如果你在另类音乐方面是专家，你对这些结果怎么看？如果你在添加了 Tool 和 A Perfect Circle 之后发现网络中有任何有趣的变化，请务必在**回复** 💬中提到！

***如果您想要本教程的*** [***注释笔记本***](https://github.com/christine-egan42/sna-billy-corgan/blob/main/SNA4-Graphing-Billy-Corgans-Community.ipynb) ***，请访问我的*** [***GitHub***](https://github.com/christine-egan42)***！给它一个 ⭐️ 以便于参考。*** 👩🏻‍💻 [Christine Egan](https://christine-egan42.github.io/) | [medium](https://christineegan42.medium.com/) | [github](https://github.com/christine-egan42) | [linkedin](https://www.linkedin.com/in/christineegan42/)

[](https://christineegan42.medium.com/membership?source=post_page-----724de1e431ac--------------------------------) [## 通过我的推荐链接加入 Medium - Christine Egan

### 作为 Medium 的会员，您的一部分会员费用会分配给您阅读的作者，并且您可以完全访问每个故事……

[christineegan42.medium.com](https://christineegan42.medium.com/membership?source=post_page-----724de1e431ac--------------------------------)
