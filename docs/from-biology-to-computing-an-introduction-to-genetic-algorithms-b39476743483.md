# 遗传算法简介

> 原文：[`towardsdatascience.com/from-biology-to-computing-an-introduction-to-genetic-algorithms-b39476743483`](https://towardsdatascience.com/from-biology-to-computing-an-introduction-to-genetic-algorithms-b39476743483)

## 探索进化计算的力量及其在日常问题中的应用

[](https://medium.com/@egorhowell?source=post_page-----b39476743483--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----b39476743483--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b39476743483--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----b39476743483--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----b39476743483--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----b39476743483--------------------------------) ·阅读时间 6 分钟·2023 年 4 月 17 日

--

![](img/e410603becb8f4c414e88ace60f06e32.png)

图片由[沃伦·乌莫](https://unsplash.com/@warrenumoh?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

查尔斯·达尔文在 19 世纪发展他的[***进化论***](https://en.wikipedia.org/wiki/Evolution)时，几乎未曾想到这一理论会对计算算法产生深远的影响***。***

自然选择、适者生存、突变和交叉等概念都导致了‘最佳’变体的存活，并将‘最佳’特征传递下去。约翰·亨利·霍兰将这些概念发展成为[***元启发式***](https://en.wikipedia.org/wiki/Metaheuristic)[***遗传算法***](https://en.wikipedia.org/wiki/Genetic_algorithm)，于 1975 年提出以解决数学优化问题***。*** 生物过程对机器学习的影响并非首次出现。最著名的例子或许是 1958 年弗兰克·罗森布拉特受到大脑中真实神经元启发开发的[***神经网络感知机***](https://en.wikipedia.org/wiki/Perceptron)。

大多数遗传算法用于解决[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization)问题，在这些问题中，由于排列组合的庞大规模，[***暴力搜索***](https://en.wikipedia.org/wiki/Brute-force_search)是不可行的。示例包括[***背包问题***](https://en.wikipedia.org/wiki/Knapsack_problem)和[***旅行推销员问题***](https://en.wikipedia.org/wiki/Travelling_salesman_problem)。

如果你是一名数据科学家，你可能会在职业生涯中遇到类似的问题，因此了解如何处理这些问题是很有价值的，本文将介绍其中一种最佳且最常见的方法。我们将深入探讨遗传算法的理论、方法论和一般用途，以展示如何实现它们来解决几乎所有的优化问题。

# 符号与术语

遗传算法的术语大多源自其相应的生物过程：

+   **编码与解码：** *将问题映射到算法可以利用的方便的数值格式。常见的技术包括* [***二进制编码和排列编码。***](https://www.obitko.com/tutorials/genetic-algorithms/encoding.php)

+   **种群：** *一组可能的解决方案。*

+   **染色体：** *一个单一的可能解决方案。*

+   **基因：** *染色体中一个元素的位置表示。*

+   **等位基因：** *基因的值。*

+   **适应度函数：** *为每个解决方案赋予一个分数，以确定其对优化问题的‘适应度’。*

这些不同的术语可以通过下面的图示进行说明：

![](img/78340c76994f7e32fa61b616488c0565.png)

图表由作者提供。

# 算法

## 初始化

算法开始时生成问题的初始种群。这需要在可能的搜索空间中具有足够的多样性，理想情况下总数在***100***或***1000***的范围内。

## 选择

在确定了我们的种群之后，我们需要***选择***解决方案（父代）以便进行下一代的繁殖。父代的选择是基于它们的适应度，以确保‘最佳’基因得以传递。

一些[技术](https://en.wikipedia.org/wiki/Selection_%28genetic_algorithm%29)用于选择：

+   **精英主义：** *选择当前种群中适应度函数最佳的若干个解决方案。*

+   [**轮盘赌选择：**](https://en.wikipedia.org/wiki/Fitness_proportionate_selection)*从当前种群中的解决方案（染色体）的概率分布中进行抽样，每个解决方案的概率* ***p_s*** *与其适应度得分* ***f_s*** *成正比：*

![](img/b3a6659302b278f49252377a9c8b3ca4.png)

方程由作者在 LaTeX 中生成。

+   [**锦标赛选择：**](https://en.wikipedia.org/wiki/Tournament_selection)**：** *这涉及随机选择解决方案并进行锦标赛，赢家是适应度得分最好的那个。*

选择过程还有许多其他方法，以及上述技术的不同变体。你也可以结合选择程序来生成混合方法。没有*‘一刀切’*的方法，最好尝试各种类型。

## 交叉

现在我们有了父代，我们需要结合它们的遗传材料以产生后代，从而生成一个新的、更适应的种群。与选择阶段类似，有各种[***操作符***](https://en.wikipedia.org/wiki/Operator_%28computer_programming%29)来执行交叉，这很依赖于问题的编码类型。

让我们了解一些[最常见的](https://en.wikipedia.org/wiki/Crossover_(genetic_algorithm))：

+   **单点交叉：** *在父代染色体的选定点之间交换基因：*

![](img/5882b7305d624132f182c802fe6011bb.png)

图示由作者提供。

+   **双点交叉：** *在父代染色体的两个选定点之间交换基因：*

![](img/45439e13dd16bd5d945c2234dc5b86a0.png)

图示由作者提供。

+   **均匀交叉：** *每个基因从两个父代的相应基因中随机选择：*

![](img/13ff770c59bc0718c31664741443bfc2.png)

图示由作者提供。

这些技术适用于二进制编码的染色体，对于其他编码类型，可能需要不同的操作符。链接的[***这里***](https://medium.com/geekculture/crossover-operators-in-ga-cffa77cdd0c8)有一篇很好的文章，给出了更多复杂的交叉操作符的综合列表。

## 变异

算法的最后一步是[***变异***](https://en.wikipedia.org/wiki/Mutation_%28genetic_algorithm%29)。这类似于[***生物变异***](https://en.wikipedia.org/wiki/Mutation)，其中后代的 DNA 可能会发生随机改变。变异是对解中的基因进行小概率的轻微修改。如果概率很高，那么遗传算法就变成了[***随机搜索***](https://en.wikipedia.org/wiki/Random_search)。变异操作符的主要动机是使种群多样化，减少[***局部最优***](https://en.wikipedia.org/wiki/Maximum_and_minimum)的可能性。

![](img/97a25926114e725beff85aa319789ff3.png)

图示由作者提供。

让我们了解一些[简单易用的变异](https://www.tutorialspoint.com/genetic_algorithms/genetic_algorithms_mutation.htm)：

**位翻转：** *对染色体中的基因进行迭代，每次通过时以小概率随机翻转一个位：*

![](img/270f1a62905be6ee6f5a9b2931482ab5.png)

图示由作者提供。

+   **交换：** *在染色体中选择两个基因，以低概率交换它们：*

![](img/7ad22b1132e46480aff4f14b85ec323c.png)

图示由作者提供。

> 链接的[***这里***](https://www.geeksforgeeks.org/mutation-algorithms-for-string-manipulation-ga/)是一个进一步的变异操作符列表。

与交叉类似，上述变异操作符适用于二进制编码问题，因此可能不适合其他类型的问题和编码。

## 终止

算法可以因多种原因结束，这些原因由用户自行决定：

+   *达到一定代数*

+   *达到了期望的适应度*

+   *计算资源耗尽*

+   *适应度评分已经达到平台期*

# 应用

遗传算法用于多种领域，包括但不限于：

+   *生产计划中的调度*

+   *车辆路线问题*

+   *金融市场与投资组合优化*

+   *训练神经网络*

+   *图像处理*

# 弱点

遗传算法的一些漏洞包括：

+   *可扩展性不如其他优化算法，因此可能有较长的计算时间*

+   *难以充分调整超参数，如变异和选择，可能导致不收敛或无效结果*

+   *并不能保证找到全局最优解，不过这是所有元启发式方法的权衡*

+   *一个昂贵且复杂的适应度函数也可能导致长时间的计算*

# 总结与思考

遗传算法的名称来源于进化生物学中的类似过程。它是一种元启发式优化算法，从初始种群开始，通过选择、交叉和变异操作迭代使用种群中的最佳解来创建新的、更好的解（后代）。它可以用于许多领域，如医学和供应链，但根据评估指标和选择的超参数，它可能会面临较长的计算时间。

# 另一个事项！

我有一个免费的通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，在其中我分享成为更优秀数据科学家的每周技巧。没有“浮夸”或“点击诱饵”，只有来自实践中的数据科学家的纯粹可操作的见解。

[](https://newsletter.egorhowell.com/?source=post_page-----b39476743483--------------------------------) [## Dishing The Data | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读《Dishing The Data》，由 Egor Howell 编写的 Substack 出版物……

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----b39476743483--------------------------------)

# 联系我！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考文献与进一步阅读

+   [*优化算法*](https://mitpress.mit.edu/9780262039420/algorithms-for-optimization/)*。 [Mykel J. Kochenderfer](https://mitpress.mit.edu/author/mykel-j-kochenderfer-18773) 和 [Tim A. Wheeler](https://mitpress.mit.edu/author/tim-a-wheeler-28144)。***2019***。

+   [*组合优化：理论与算法。*](https://link.springer.com/book/10.1007/978-3-662-56039-6) [Bernhard Korte](https://www.amazon.co.uk/s/ref=dp_byline_sr_book_1?ie=UTF8&field-author=Bernhard+Korte&text=Bernhard+Korte&sort=relevancerank&search-alias=books-uk) 和 [Jens Vygen](https://www.amazon.co.uk/Jens-Vygen/e/B00DQ1CB98/ref=dp_byline_cont_book_2)。***2018***。

+   Holland, J.H. (1975) *《自然与人工系统中的适应》*。密歇根大学出版社，安娜堡。（第二版，麻省理工学院出版社，1992 年。）

+   Rosenblatt, F. (1958). 识别机：一种用于信息存储和组织的概率模型。*《心理学评论，65*(6)，386–408\. [`doi.org/10.1037/h0042519`](https://psycnet.apa.org/doi/10.1037/h0042519)
