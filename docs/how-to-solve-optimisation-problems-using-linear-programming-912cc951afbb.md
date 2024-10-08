# 如何使用线性规划解决优化问题

> 原文：[`towardsdatascience.com/how-to-solve-optimisation-problems-using-linear-programming-912cc951afbb`](https://towardsdatascience.com/how-to-solve-optimisation-problems-using-linear-programming-912cc951afbb)

## 线性规划的介绍以及如何使用图形法和单纯形算法解决线性规划问题

[](https://medium.com/@egorhowell?source=post_page-----912cc951afbb--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----912cc951afbb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----912cc951afbb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----912cc951afbb--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----912cc951afbb--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----912cc951afbb--------------------------------) ·7 分钟阅读·2023 年 7 月 24 日

--

![](img/8f8bc85a775d8dd0a1009a32df6eeacd.png)

图片由 [Isaac Smith](https://unsplash.com/es/@isaacmsmith?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

[**线性规划 (LP)**](https://en.wikipedia.org/wiki/Linear_programming) 是一种优化技术，用于从指定的*目标函数*中找到最佳解决方案，受某些*约束*的限制。它广泛应用于从金融到电子商务等各个行业，因此，如果你是数据科学家，了解它是非常值得的。LP 的关键特性是[**线性**](https://en.wikipedia.org/wiki/Linear_equation)部分，这意味着约束和目标函数都被表述为线性关系。在这篇文章中，我们将通过一个示例 LP 问题，讲解如何使用[**单纯形算法**](https://en.wikipedia.org/wiki/Simplex_algorithm)和[**图形法**](https://www.toppr.com/guides/maths/linear-programming/graphical-method-of-solving-a-linear-programming-problem/)解决它。

# 图形法

## 基本问题

我们将从图形法开始，因为它最简单易懂，并能给我们真实的线性规划直觉。

假设我们经营一家小型商店，出售**£3**的果昔和**£2**的咖啡，这就是我们的两个*决策变量*。由于原料限制，我们每天最多只能生产**75**杯果昔和**100**杯咖啡。此外，我们每天只有**140**个杯子来供应我们的果昔和咖啡。现在，让我们将这个问题形式化为一个线性规划（LP）问题！

## 公式化

如果我们让***x***代表果昔，***y***代表咖啡，那么我们想要最大化以下目标函数***c***，作为决策变量的函数：

![](img/86d5d4b66aeee55d3319ef64580abf0f.png)

方程由作者用 LaTeX 编写。

受以下约束条件限制：

![](img/9afab927c71e80569159206a366cf00e.png)

方程由作者用 LaTeX 编写。

> 决策变量需要是非负的。

现在是绘制这些约束条件的时候了！

## 绘制

由于这是一个二维问题，我们可以在笛卡尔图上将约束绘制为直线（线性部分！）：

![](img/87afd72c7fc610e99064a21bf7f4096b.png)

图表由作者用 Python 生成。

灰色区域被称为 *可行区域*，在这里任何解都是有效的，因为它满足约束条件。

从图中观察，最优解似乎位于约束条件的直线交汇的角落。这实际上被称为[**线性规划基本定理**](https://en.wikipedia.org/wiki/Fundamental_theorem_of_linear_programming)**。** 该定理指出，线性函数的最优解（最大值）位于[**凸多边形**](https://en.wikipedia.org/wiki/Convex_polygon)区域的角落（可行区域）。

这里的最优解是 **(75, 65)** 角落，值为 **£355**。这可以通过测试可行区域的每个角落简单得出。

## 限制

上述例子很容易理解，但如果我们有另外两种产品我们公司销售会怎样？那么问题就变成了四维的。然而，我们人类无法在纸上可视化这一点。那么我们该怎么办呢？正如每个数据科学家所知，我们使用算法！

# 单纯形算法

## 直觉

单纯形算法是由[**乔治·丹茨格**](https://en.wikipedia.org/wiki/George_Dantzig)在第二次世界大战期间发明的，用于解决 LP 问题。算法的一般流程是访问凸多边形的每个顶点并评估目标函数。一旦我们到达最优解（我们将展示如何知道它是最优的），就终止算法。

在我看来，学习单纯形算法的最佳方法是将其应用于一个问题，并解释每一步的作用。因此，事不宜迟，让我们开始吧！

> 算法背后的数学证明相当复杂，因此我在此帖中没有详细讨论所有细节。然而，感兴趣的读者可以在[此处](https://pi.math.cornell.edu/~web401/matt.simplex.pdf)找到相关链接。

## 步骤

***第 1 步：***

通过使用 *松弛变量* 将不等式转换为[**线性方程组**](https://en.wikipedia.org/wiki/System_of_linear_equations)：

![](img/eb7cd68f5fe20f76067cfce5c1c38942.png)

方程由作者用 LaTeX 编写。

这里 ***s_1***、***s_2*** 和 ***s_3*** 是松弛变量，它们字面上“弥补”了不等式，使其转变为方程。松弛变量被称为 *基本变量*，而决策变量 ***x*** 和 ***y*** 被称为 *非基本变量*。

***第 2 步：***

将方程构建成一个*表格*，其中每个方程有自己的一行，目标函数在底部行：

![](img/9168f49baf1e01f943dce03e72f33826.png)

约束条件和目标函数的初始表格。图示由作者提供。

这被称为*初始基本解*，对应于我们之前展示的图中的**(0,0)**顶点。

***步骤 3：***

接下来，我们需要通过找到最后一行中的最负值条目来识别枢轴列。然后，我们通过将*值*列除以枢轴列条目来识别枢轴行：

![](img/10d23e628d94a4a4032584e6998ba20f.png)

最后一行中的最负值条目。图示由作者提供。

在这种情况下，我们的枢轴列是***x***，称为*进入变量*，对应的枢轴行是第一行，***R1***，因为它具有最小的商。

我们选择底部行中的最低值，因为它代表了目标函数中的最低系数。因此，优化这个列将使目标函数增加最多。

此外，我们选择商最小的行，以不违反约束条件。有关为什么这样做的更多信息可以在[这里](https://math.stackexchange.com/questions/262207/steps-in-the-simplex-method)找到。

***步骤 4：***

使用枢轴列，***x***，和行，***R1***，我们希望使枢轴列中的所有条目除***R1***外都为***0***。我们通过将其他行加上或减去***R1***的倍数来实现：

![](img/891b7e196f155efebd5f9ce28e055031.png)

使 x 列中的每一行为 0，除了第一行。图示由作者提供。

我们看到新的目标是**225**，这对应于我们上面展示的图中的**(75,0)**顶点。因此，从图形上看，我们已经从顶点**(0,0)**移动到顶点**(75,0)**。

> 这一步骤本质上是[**高斯消元法**](https://en.wikipedia.org/wiki/Gaussian_elimination)。

***步骤 5：***

重复步骤 3 和 4，直到最后一行（目标函数）中的所有条目都是非零的：

![](img/217056592d30479ab13218d73c0f9d35.png)

最后一行中的最负值条目。图示由作者提供。

![](img/08c8be54292d9a83f5074a4d73b7b5d5.png)

使* y *列中的每一行为 0，除了第三行。图示由作者提供。

现在，我们已经达到了最佳解，因为底部行没有负值。我们问题的最佳解是**£355**，当***x = 75***和***y = 65***时，也就是说，**(75,65)**顶点。这正是我们使用图形技术上面找到的准确解！

算法步骤本身并不复杂，但理解每一步的含义可能有些棘手。如果你想更深入地探索单纯形算法并深入理解每一步的目的，我在参考文献部分提供了有用的资源链接。

# 其他算法

还有更多的算法可以解决线性规划问题。以下是一个列表，其中包含一些提供的链接供感兴趣的读者查看：

+   [*交叉算法*](https://en.wikipedia.org/wiki/Criss-cross_algorithm)

+   [*内点法*](https://en.wikipedia.org/wiki/Interior-point_method#:~:text=Interior%2Dpoint%20methods%20(also%20referred,red%20points%20show%20iterated%20solutions.)

+   [*椭圆体算法*](https://www.cs.princeton.edu/courses/archive/fall05/cos521/ellipsoid.pdf)

+   [*Vaidya 的算法*](https://link.springer.com/article/10.1007/bf02592216)

# Python 中的线性规划

幸运的是，我们在解决线性规划问题时不需要手动进行单纯形算法！在 Python 中，有两个主要的包为我们完成繁重的计算工作：

+   [*PuLP*](https://coin-or.github.io/pulp/)

+   [*SciPy*](https://scipy.org/)

我不会在本文中详细介绍如何使用它们，但 [这里链接](https://realpython.com/linear-programming-python/) 提供了一个关于如何将这些包应用于你的线性规划问题的优秀教程。

# 摘要与进一步思考

线性规划（LP）是一个非常有用的工具，可以应用于解决广泛的问题，因此对数据科学家来说非常重要。线性规划的基本概念是将问题全部用线性方程和不等式来表述，从而实现更快的计算时间。解决线性规划问题最常见的方法是单纯形算法，幸运的是，计算包已经为我们完成了这项工作。

本博客中使用的完整代码可以在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/tree/main/Optimisation/linear-programming?source=post_page-----912cc951afbb--------------------------------) [## Medium-Articles/Optimisation/linear-programming 在主分支 · egorhowell/Medium-Articles

### 我在中等博客/文章中使用的代码。通过创建账户来为 egorhowell/Medium-Articles 的开发做出贡献...

github.com](https://github.com/egorhowell/Medium-Articles/tree/main/Optimisation/linear-programming?source=post_page-----912cc951afbb--------------------------------)

# 参考文献与进一步阅读

+   [*一些单纯形和图形方法的更多实例*](https://www.cuemath.com/algebra/linear-programming/)

+   [*更深入的线性规划数学探讨*](https://math.mit.edu/~goemans/18310S15/lpnotes310.pdf)

+   [*单纯形算法的更深入实例*](https://math.libretexts.org/Bookshelves/Applied_Mathematics/Applied_Finite_Mathematics_(Sekhon_and_Bloom)/04%3A_Linear_Programming_The_Simplex_Method/4.02%3A_Maximization_By_The_Simplex_Method)

# 另一个事项！

我有一个免费的通讯，[**Dishing the Data**](https://dishingthedata.substack.com/)，在这里我每周分享成为更好的数据科学家的技巧。没有“废话”或“点击诱饵”，只有来自实践数据科学家的纯粹可操作的见解。

[](https://newsletter.egorhowell.com/?source=post_page-----912cc951afbb--------------------------------) [## Dishing The Data | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读《Dishing The Data》，作者**Egor Howell**，这是一个 Substack 出版物。

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----912cc951afbb--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)
