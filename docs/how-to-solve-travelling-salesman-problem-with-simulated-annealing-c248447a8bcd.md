# 如何用模拟退火解决旅行推销员问题

> 原文：[`towardsdatascience.com/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd`](https://towardsdatascience.com/how-to-solve-travelling-salesman-problem-with-simulated-annealing-c248447a8bcd)

## 使用模拟退火优化算法获取旅行推销员问题的**最佳解决方案**。

[](https://medium.com/@egorhowell?source=post_page-----c248447a8bcd--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----c248447a8bcd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c248447a8bcd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c248447a8bcd--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----c248447a8bcd--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c248447a8bcd--------------------------------) ·6 分钟阅读·2023 年 2 月 10 日

--

![](img/32255d463d6594ee2b3194c9ece853c7.png)

照片由 [Annie Spratt](https://unsplash.com/de/@anniespratt?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

# 背景

[***旅行推销员问题 (TSP)***](https://en.wikipedia.org/wiki/Travelling_salesman_problem) 是 [***组合优化***](https://en.wikipedia.org/wiki/Combinatorial_optimization) 和数学中一个普遍存在的问题。这个问题提出了这样一个问题：‘给定一个城市及其距离的列表，什么是访问每个城市一次并返回原始城市的最短路径？’

> 旅行推销员问题的一个更正式的数学定义是 [**哈密顿回路**](https://en.wikipedia.org/wiki/Hamiltonian_path)。

这个问题之所以著名，是因为它是 [***NP-hard***](https://en.wikipedia.org/wiki/NP-hardness)***。*** 这本质上意味着你不能在合理的时间内找到**最佳解决方案**，因为新城市的增加会导致 [***组合爆炸***](https://en.wikipedia.org/wiki/Combinatorial_explosion) 可能路径的数量。例如，***4*** 个城市的可能路径数为 ***3***，***6*** 个城市的路径数为 ***60***，然而 ***20*** 个城市的路径数达到了巨大的 ***60,822,550,200,000,000!***

> TSP 的解决方案数量为 **(n-1)!/2**，其中 **n** 是城市的数量。对于较大的 **n**，解决方案的数量 [**不可处理**](https://en.wiktionary.org/wiki/intractable) 现代计算标准。

那么，这就引出了一个问题：如果我们有一些难以想象的路线要尝试，我们如何找到最短距离？当然，你可以使用[***暴力破解***](https://blog.routific.com/blog/travelling-salesman-problem)（简单解法）的方法尝试每一种可能的路线，但正如我们上面提到的，对于一定数量的城市，使用当前的计算标准可能需要几年时间。事实上，对于***20***个城市，尝试每一条路线大约需要[***~1000*** 年](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/traveling-salesman-problem)！

这里我们提到了一些特殊的[***启发式***](https://en.wikipedia.org/wiki/Heuristic_(computer_science))或[***元启发式***](https://en.wikipedia.org/wiki/Metaheuristic)算法，这些算法并不保证每次都能找到最佳解，但会尝试在合理的时间内获得一个*足够好的*解决方案。在本文中，我们将使用一种名为[***模拟退火***](https://en.wikipedia.org/wiki/Simulated_annealing)的算法***(SA)***来解决旅行商问题。

# 模拟退火

## 概述

模拟退火是一种随机[***全局搜索***](https://en.wikipedia.org/wiki/Global_optimization)算法，这意味着它在寻找最佳解决方案的过程中使用了随机性。它的名字和灵感来自于[***退火***](https://en.wikipedia.org/wiki/Annealing_(materials_science))过程，这一过程在[***冶金***](https://en.wikipedia.org/wiki/Metallurgy)中，通过加热或冷却金属来影响其物理特性。

SA 利用温度的概念来确定过渡到更差解决方案的概率（搜索的随机性），以更广泛地探索搜索空间，并更有可能找到[***全局最优解***](https://en.wikipedia.org/wiki/Maxima_and_minima)。这有助于避免陷入[***局部最优解***](https://en.wikipedia.org/wiki/Local_optimum)，这类情况常常发生在[***爬山算法***](https://en.wikipedia.org/wiki/Hill_climbing)等算法中。

## 理论

SA 的一般数学框架是：

![](img/9da4d08b77e9b035591c1bf918a26073.png)

作者生成的方程。

其中：

![](img/85547289e68b773bdc0e161dd3c1a438.png)

作者生成的方程。

在这些方程中，***x*** 是当前状态，***x’*** 是新状态，***Δy*** 是两个状态之间成本或得分的差异，***P(x → x’)*** 是从当前状态过渡到新状态的概率，***T*** 是温度。

可以看出，当新状态的得分优于当前状态时，我们总是会过渡到新状态。此外，当温度***T***很高时，我们也很可能过渡到得分较差的新状态。这使我们能够更容易地在搜索空间中移动，**探索**全局最优解。

随着温度的降低，我们转移到更差解的可能性减小。因此，过程开始趋向于最优并开始 **利用** 搜索空间。

温度通常以指数形式冷却：

![](img/5792d44c41d7e19438a14cf4bb2e9d58.png)

方程由作者生成。

其中 ***0 ≤ γ ≤ 1*** 和 ***t*** 是迭代次数。

另一个常见问题是如何确定初始温度？这实际上是一个相当复杂的话题，我建议读者查看 [***这里***](https://www.researchgate.net/publication/227061666_Computing_the_Initial_Temperature_of_Simulated_Annealing) 的研究论文。一般来说，你可以从一个你知道足够高的温度开始，以便在开始时接受更差的解。这只是一个经典的试错过程！

## TSP 算法

使用 SA 进行 TSP 的方法如下：

+   获取一个初始解，这可以是销售员的任何有效路线。

+   随机选择两个城市，并在路线中交换它们。

+   使用 SA 计算我们是否接受这个新解的概率。

+   对一组设定的迭代次数继续此过程，并在每次迭代中降低温度（减少）。

+   始终记录你找到的最佳总体解。

# Python 实现

让我们在 Python 中实现这一点，展示一个工作示例。

首先我们将生成一些数据点，这些将是我们的城市：

GitHub Gist 作者提供。

![](img/a0993029f03520a91079adecb7566a70.png)

图由作者在 Python 中生成。

上图显示了我们的城市以及一个初始解。初始解是通过按字母顺序遍历城市生成的，例如 ***a, b, c ... r, s, a***。我们有 ***20*** 个城市，这意味着我们总共有 ***60,822,550,200,000,000*** 种不同的可能路线可供尝试！

现在我们将构建一个 Python 类来执行 TSP 的模拟退火过程！

> 我不是最好的 Python 程序员，所以我知道以下代码片段可能不是最佳实现……

GitHub Gist 作者提供。

我们现在将运行 SA 优化算法并绘制解：

GitHub Gist 作者提供。

![](img/3f8d29554b07054dcea1badd265ea11d.png)

图由作者在 Python 中生成。

![](img/5d955185b51b0ef02ecee46a6a6fd90b.png)

图由作者在 Python 中生成。

上述图显示了我们迭代过的解，与温度和当前最佳解一起绘制。底部图显示了我们通过 SA 算法找到的最佳解的路线。从简单的“眼睛测试”来看，这个新解比初始解要好得多，并且似乎相当接近最优。

# 总结

在这篇文章中，我们讨论了旅行商问题，该问题要求我们找到一组城市（点）之间的最短路径。随着城市数量的增加，可能的不同路径数量呈指数增长，无法通过穷举法解决。然而，我们可以利用像模拟退火（SA）这样的元启发式算法，在大多数情况下找到足够的解决方案。

文章中使用的完整代码可以在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/blob/main/Optimisation/simulated-annealing/simulated_annealing_tsp.py?source=post_page-----c248447a8bcd--------------------------------) [## Medium-Articles/simulated_annealing_tsp.py at main · egorhowell/Medium-Articles

### 我在中等博客/文章中使用的代码。通过创建一个账户来为 egorhowell/Medium-Articles 的发展做贡献…

github.com](https://github.com/egorhowell/Medium-Articles/blob/main/Optimisation/simulated-annealing/simulated_annealing_tsp.py?source=post_page-----c248447a8bcd--------------------------------)

# 另一件事！

我有一个免费的新闻通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，每周分享成为更好数据科学家的技巧。没有“虚假”或“点击诱饵”，只有来自实践数据科学家的纯粹可操作的见解。

[](https://newsletter.egorhowell.com/?source=post_page-----c248447a8bcd--------------------------------) [## Dishing The Data | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读《Dishing The Data》，由 Egor Howell 编写，Substack 出版物…

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----c248447a8bcd--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell?sub_confirmation=1)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考文献与进一步阅读

+   [*优化算法*](https://mitpress.mit.edu/9780262039420/algorithms-for-optimization/)*.* [Mykel J. Kochenderfer](https://mitpress.mit.edu/author/mykel-j-kochenderfer-18773) 和 [Tim A. Wheeler](https://mitpress.mit.edu/author/tim-a-wheeler-28144)。 ***2019***.
