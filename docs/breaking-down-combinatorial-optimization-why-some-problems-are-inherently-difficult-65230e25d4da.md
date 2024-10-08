# 什么是组合优化？

> 原文：[`towardsdatascience.com/breaking-down-combinatorial-optimization-why-some-problems-are-inherently-difficult-65230e25d4da`](https://towardsdatascience.com/breaking-down-combinatorial-optimization-why-some-problems-are-inherently-difficult-65230e25d4da)

## 展示组合爆炸的真正威力以及它们如何影响简单问题。

[](https://medium.com/@egorhowell?source=post_page-----65230e25d4da--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----65230e25d4da--------------------------------)[](https://towardsdatascience.com/?source=post_page-----65230e25d4da--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----65230e25d4da--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----65230e25d4da--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----65230e25d4da--------------------------------) ·阅读时间 5 分钟·2023 年 4 月 10 日

--

![](img/e25a11d4888e8f7a8808ec5eff99b3b2.png)

图片来源 [Shubham Dhage](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 什么是组合优化？

想象一下你是一名数据科学家，为一家航空公司工作，你被要求找到一周内的最佳航班安排，以在燃料和空域等限制条件下最大化航班数量。

***你会怎么做？***

好吧，你可以尝试每一个可能的解决方案，这称为 [***蛮力搜索***](https://en.wikipedia.org/wiki/Brute-force_search)，但如果我告诉你我们要进行 ***500*** 次航班呢？你需要尝试的不同 [***组合***](https://medium.com/towards-data-science/a-primer-on-combinations-and-permutations-d654aacea292) 数量将达到 ***~500!*** 的规模，这大约等于 ***1.22 x 10¹¹³⁴.*** 这个数字非常巨大，使得蛮力搜索变得 [***不可处理***](https://www.umsl.edu/~siegelj/information_theory/classassignments/Lombardo/04_intractableproblems.html)。

***那么，你如何解决这个问题？***

> 组合优化！

[***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization) 处理从有限对象中寻找最优解的问题，而该问题通常是难以处理的。这听起来很像我们上面的问题！

实际上，组合优化在许多领域中都有应用：

+   *物流和供应链*

+   *制造业*

+   *金融*

+   *医疗保健*

这使得理解和开发处理这些类型问题的技能成为数据科学家必备的能力。

要了解更多关于组合优化方法的知识，这些方法主要是[***元启发式***](https://en.wikipedia.org/wiki/Metaheuristic)，你可以查看我以前的一些帖子：

[](/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd?source=post_page-----65230e25d4da--------------------------------) ## 如何通过模拟退火解决旅行商问题

### 使用模拟退火优化算法获得旅行商问题的最佳解决方案

towardsdatascience.com [](/tabu-search-simply-explained-ee2852339d78?source=post_page-----65230e25d4da--------------------------------) ## Tabu 搜索简单解释

### 对 Tabu 搜索优化算法的直观解释及其如何应用于旅行商问题…

towardsdatascience.com [](/hill-climbing-optimization-algorithm-simply-explained-dbf1e1e3cf6c?source=post_page-----65230e25d4da--------------------------------) ## 爬山算法简单解释

### 最受欢迎的优化算法之一的直观解释

towardsdatascience.com

然而，在这篇文章中，我想真正探讨这些问题为何如此困难，并演示一个简单的问题如何在复杂性上‘爆炸’。

# 旧的最爱：旅行商问题

组合优化问题的经典例子是[***旅行商问题 (TSP)***](https://en.wikipedia.org/wiki/Travelling_salesman_problem)。这是一个非常简单的问题，提出了以下问题：

*‘找到一组城市中最短的路线，访问每个城市一次并回到起点城市’*

听起来很简单，对吧？在现实场景中，情况远非如此。

这是因为随着城市数量的增加，问题的可能解决方案数量会导致[***组合爆炸***](https://en.wikipedia.org/wiki/Combinatorial_explosion)。TSP 的解决方案数量是：

![](img/6b1137add2feee97910f5e7d9812cf4b.png)

作者在 LaTeX 中的方程。

其中 ***n*** 是城市的数量。

让我们用一些实际数字来真正展示这种‘*爆炸*’：

+   ***n = 4: 解决方案 = 3***

+   ***n = 8: 解决方案 = 2,520***

+   ***n = 16: 解决方案 = 1.0461395 x 10¹³***

更值得注意的是，对于***20***个城市，通过暴力破解解决 TSP 需要[大约 ***1,900*** 年](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/traveling-salesman-problem)！

# 展示组合爆炸

让我们真正深入探讨一些代码，以全面理解暴力算法的内部机制，并描绘组合爆炸。

以下是使用暴力搜索解决 TSP 的一个模板类：

GitHub Gist 作者。

现在让我们使用算法对一些虚拟‘假’城市数据进行测试，以确认它确实返回了最佳解决方案：

GitHub Gist 作者。

![](img/895e587726b7b0f9eb489c2be8827d9f.png)

作者在 Python 中生成的图表。

![](img/cb94aad684be9bd2e61e29c611352a16.png)

作者在 Python 中生成的图表。

看起来不错！初始解决方案显然不是最优的，而最终的最佳解决方案在视觉上也显得是最好的。

我们现在将测量暴力算法的持续时间与城市数量的关系，以图形方式描绘组合爆炸：

GitHub Gist 作者。

![](img/9d480df89d869cfce59133cea59ee295.png)

作者在 Python 中生成的图表。

从上面的图表中，我们看到暴力搜索的持续时间在大约***12***个城市时急剧增加。用实际数字来讲，***11***个城市时花费了***~0.5***分钟，但***12***个城市时花费了***~36***分钟！所以，仅仅增加一个城市，算法计算时间增加了***72***倍！

这真正展示了这些组合优化问题的强大和难以解决性。

# 总结与进一步思考

在这篇文章中，我们看到简单问题如何在复杂性上“爆炸”。在处理大规模系统和网络的行业中，这是很常见的。优化这些大规模系统中的业务问题的过程被称为组合优化。这个领域的需求源于暴力搜索的难以处理性，它可能需要数千年才能找到相对搜索空间的最佳解决方案。我们通过描绘旅行商问题中发生的组合爆炸来展示这种现象。

本文中使用的完整代码可以在我的 GitHub 上找到：

[## Medium-Articles/Optimisation/brute-force at main · egorhowell/Medium-Articles](https://github.com/egorhowell/Medium-Articles/tree/main/Optimisation/brute-force?source=post_page-----65230e25d4da--------------------------------)

### 你当前无法执行该操作。你在另一个标签或窗口中登录了。你在另一个标签或…

[github.com](https://github.com/egorhowell/Medium-Articles/tree/main/Optimisation/brute-force?source=post_page-----65230e25d4da--------------------------------)

# 另一件事！

我有一个免费的通讯稿，[**Dishing the Data**](https://dishingthedata.substack.com/)，在其中我每周分享成为更好数据科学家的技巧。没有“废话”或“点击诱饵”，只有来自实践数据科学家的纯粹可操作的见解。

[## Dishing The Data | Egor Howell | Substack](https://newsletter.egorhowell.com/?source=post_page-----65230e25d4da--------------------------------)

### 如何成为更优秀的数据科学家。点击阅读由 Egor Howell 撰写的《Dishing The Data》，这是一个 Substack 出版物，内容包括…

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----65230e25d4da--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考文献与进一步阅读

+   [*优化算法*](https://mitpress.mit.edu/9780262039420/algorithms-for-optimization/)。 [Mykel J. Kochenderfer](https://mitpress.mit.edu/author/mykel-j-kochenderfer-18773) 和 [Tim A. Wheeler](https://mitpress.mit.edu/author/tim-a-wheeler-28144)。 ***2019***。

+   [*组合优化：理论与算法*](https://link.springer.com/book/10.1007/978-3-662-56039-6) [Bernhard Korte](https://www.amazon.co.uk/s/ref=dp_byline_sr_book_1?ie=UTF8&field-author=Bernhard+Korte&text=Bernhard+Korte&sort=relevancerank&search-alias=books-uk) 和 [Jens Vygen](https://www.amazon.co.uk/Jens-Vygen/e/B00DQ1CB98/ref=dp_byline_cont_book_2)。 ***2018***。
