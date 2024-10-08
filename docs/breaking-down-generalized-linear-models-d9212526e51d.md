# 广义线性模型介绍

> 原文：[`towardsdatascience.com/breaking-down-generalized-linear-models-d9212526e51d`](https://towardsdatascience.com/breaking-down-generalized-linear-models-d9212526e51d)

## 扩展你的建模技能，超越线性回归

[](https://medium.com/@egorhowell?source=post_page-----d9212526e51d--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----d9212526e51d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d9212526e51d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d9212526e51d--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----d9212526e51d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d9212526e51d--------------------------------) ·阅读时间 6 分钟·2023 年 7 月 10 日

--

![](img/d2ac6a93faf461a938eb43bc346402ef.png)

图片来源：[Roman Mager](https://unsplash.com/@roman_lazygeek?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

[**线性回归**](https://en.wikipedia.org/wiki/Linear_regression)是我们在数据科学中学习的最常见算法。每个从业者都听说过并使用过它。然而，对于某些问题，它并不适用，我们需要对其进行‘推广’。这就是[**广义线性模型** (**GLMs**)](https://en.wikipedia.org/wiki/Generalized_linear_model)的用武之地，它们为回归建模提供了更大的灵活性，并且是数据科学家必须了解的宝贵工具。

# 什么是 GLMs？

正如我们上面所说，GLMs‘推广’了普通线性回归，但我们*真正*是什么意思呢？

让我们考虑一个更简单的线性回归模型：

![](img/9bc2565c51fe65fb1fc459e89800f399.png)

作者通过 LaTeX 生成的方程。

其中 ***β*** 是系数***， x*** 是解释变量，***ε*** 是[**正态分布**](https://en.wikipedia.org/wiki/Normal_distribution)的误差。

假设我们想要模拟保险公司在一小时内接到的索赔电话数量。线性回归是否适合这个问题？

> 不！

原因如下：

+   线性回归假设误差服从正态分布，而正态分布可以取负值。然而，我们不能有负的索赔电话。

+   第二点是正态分布，因此线性回归是连续的。而索赔电话都是整数和离散的，我们不能有 1.1 个电话。

因此，线性回归模型不能正确处理这个确切的问题。然而，我们可以将回归模型推广到符合上述要求的概率分布。在这种情况下，就是 [**泊松分布**](https://medium.com/towards-data-science/predicting-the-unpredictable-an-introduction-to-the-poisson-distribution-5afd4d70b1d7)（稍后将详细介绍）。

GLM 简单地提供了一个框架，说明我们如何将输入与目标分布的期望输出链接起来。它们帮助将许多回归模型统一在一个“数学伞”下。

关于泊松分布的补充视频。

# 理论框架

## 概述

GLM 的基础依赖于三个关键因素：

+   [*线性预测器*](https://en.wikipedia.org/wiki/Linear_combination) *(系统成分)*

+   [*链接函数*](https://www.statisticshowto.com/link-function/#:~:text=A%20link%20function%20in%20a,variable%20in%20a%20linear%20way.) *(随机成分)*

+   [*指数族*](https://en.wikipedia.org/wiki/Exponential_family)

现在我们来逐一解释这些内容的含义。

## 线性预测器

这是最简单理解的一个。线性预测器，***η***，仅仅意味着我们有一个输入（解释变量/协变量）***x*** 的线性和，乘以其对应的系数 ***β***：

![](img/332750f4d7a3f759e6d1a682c9cd9370.png)

由作者在 LaTeX 中生成的方程。

## 链接函数

链接函数，***g***，*实际上*负责将线性预测器与目标分布的均值响应 ***μ*** 进行“链接”：

![](img/e645722a09a95f38179100bdfaf15533.png)

由作者在 LaTeX 中生成的方程。

## 指数族

**概述**

GLM 的一个要求是输出的目标分布需要是 [**指数族**](https://en.wikipedia.org/wiki/Exponential_family)**。** 这个分布族包含了许多你可能听说过的著名分布，如 [**泊松分布**](https://medium.com/towards-data-science/predicting-the-unpredictable-an-introduction-to-the-poisson-distribution-5afd4d70b1d7)**、** [**二项分布**](https://medium.com/towards-artificial-intelligence/decoding-the-binomial-distribution-a-fundamental-concept-for-data-scientists-81c64c7e4580)**、** [**伽马分布**](https://medium.com/towards-data-science/gamma-distribution-simply-explained-d95a9de16278)**、** 和 [**指数分布**](https://medium.com/towards-data-science/exponential-distribution-simply-explained-349d05b1bdb8)**。**

> 在 GLM 框架中，我们实际上使用 [**指数离散模型**](https://en.wikipedia.org/wiki/Exponential_dispersion_model)，这是指数族的进一步推广。

为了属于指数族，概率密度（[**PDF**](https://en.wikipedia.org/wiki/Probability_density_function)）或质量函数（[**PMF**](https://en.wikipedia.org/wiki/Probability_mass_function)）需要重新因式分解并参数化成以下形式：

![](img/39f1219ecd33e41c53794321a54a3b2a.png)

由作者用 LaTeX 生成的公式。

这种形式是为了统计上的方便选择的，但在本文中我们不需要过多关注为什么会这样。

注意有两个参数 ***θ***，这是自然或 [**canonical**](https://en.wikipedia.org/wiki/Canonical_form) 参数，将输入与输出相关联，以及 ***ϕ***，这是离散参数。

另一个有趣的事实是，指数族中的分布都有 [**共轭先验**](https://en.wikipedia.org/wiki/Conjugate_prior)。这使得它们对于 [**贝叶斯问题**](https://medium.com/towards-artificial-intelligence/conditional-probability-and-bayes-theorem-simply-explained-788a6361f333) 非常有用。如果你想了解更多关于共轭先验的信息，请查看我的相关文章：

[](/bayesian-conjugate-priors-simply-explained-747218be0f70?source=post_page-----d9212526e51d--------------------------------) ## 贝叶斯共轭先验简单解释

### 一种计算上有效的贝叶斯统计方法

towardsdatascience.com

**规范链接函数**

有一种叫做规范链接函数的东西，定义为：

![](img/97dbfd48a2b56caa6be58858268a51ae.png)

由作者用 LaTeX 生成的公式。

因此，如果我们能够用 ***μ*** 描述 ***θ***，那么我们已经推导出了目标分布的自然链接函数！

**均值和方差**

数学上可以证明，指数族的均值，***E(Y)***，由以下公式给出：

![](img/0e23ca680df0ee55dfdf08ebb5fc28e2.png)

由作者用 LaTeX 生成的公式。

同样，方差，***Var(Y)***，由以下公式给出：

![](img/2eb689a0955d876cd1254e6ea3ca0ac5.png)

由作者用 LaTeX 生成的公式。

> 如果你想查看这一推导的证明，请参考以下 [linked](https://www.utstat.toronto.edu/~brunner/oldclass/2201s11/readings/glmbook.pdf) 书的第 29 页。一般来说，解决方法是对 **θ** 的对数似然函数取导数。

# 泊松回归示例

## 泊松分布

泊松分布是一个著名的离散概率分布，用于建模在已知均值发生率下，事件发生特定次数的概率。如果你想了解更多，请查看我之前的帖子：

[](/predicting-the-unpredictable-an-introduction-to-the-poisson-distribution-5afd4d70b1d7?source=post_page-----d9212526e51d--------------------------------) ## 预测不可预测：泊松分布简介

### 对最著名概率分布之一的概述

towardsdatascience.com

其 PMF 由以下公式给出：

![](img/24a6b2df29e5fa5a80d813368a4d0573.png)

由作者在 LaTeX 中生成的方程。

其中：

+   **e**: *欧拉数（约 2.73）*

+   **x**: *出现次数（≥ 0）*

+   **λ**: 期望出现次数（≥ 0），这也是 GLM 表示法中的均值***μ***

## 指数形式

我们可以通过对两边取自然对数，将上述泊松 PMF 写成指数形式：

![](img/4e0758d39db9712db82bbf69f6d6a30d.png)

由作者在 LaTeX 中生成的方程。

然后，我们对两边进行欧拉数的指数运算：

![](img/cd120ee938e29d609d8bfc9e6940c71f.png)

由作者在 LaTeX 中生成的方程。

现在，泊松 PMF 已经是指数形式了！

通过将系数与上述方程和指数家族 PDF 对应，我们得出以下结果：

![](img/ad034afa0c60f07f4bf1c1e8fbac1126.png)

由作者在 LaTeX 中生成的方程。

因此，泊松分布的均值和方差为：

![](img/8b2a596dd72c5e30c4bcb909bc3f664f.png)

由作者在 LaTeX 中生成的方程。

这是泊松分布的一个已知结果，我们只是推导出了一种不同的方法！

## 泊松 GLM

泊松分布的标准链接函数为：

![](img/b0623289839126c042cddc41d13145ef.png)

由作者在 LaTeX 中生成的方程。

因此，泊松回归方程为：

![](img/0802b1107133c48668af0efe8aa86a8e.png)

由作者在 LaTeX 中生成的方程。

我们可以验证这个方程的输出只能为正，因此符合预测保险公司接到的索赔电话数量的问题的要求。

然后你可以通过 [**最大似然估计**](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation) 或 [**迭代加权最小二乘法**](https://en.wikipedia.org/wiki/Iteratively_reweighted_least_squares) 来求解估计量。

# 重点是什么？

你可能会想，为什么我刚刚带你经历了这么繁琐的数学。好吧，让我快速总结一下关键要点：

+   *检查问题的要求和目标分布以避免不合理的结果至关重要。*

+   *GLM 提供了一种从数学原理出发的方法，帮助你将输入与特定问题的期望输出联系起来。*

# 总结与进一步思考

标准的线性回归模型很强大，但不适用于所有类型的问题，比如输出是非负的情况。对于这些特定的问题，我们必须使用其他分布，如泊松分布，广义线性模型（GLMs）提供了一个框架来执行这个过程。它们通过从基本原理推导一个链接函数，使你能够将输入转换为期望的目标输出分布。GLMs 是一个强大的建模工具，大多数数据科学家应该至少了解它们，因为它们的多功能性。

# 还有其他内容！

我有一个免费的新闻通讯，[**数据分析分享**](https://dishingthedata.substack.com/)，每周分享成为更好的数据科学家的实用技巧。没有“虚 fluff”或“点击诱饵”，只有来自实践数据科学家的纯粹可操作见解。

[](https://newsletter.egorhowell.com/?source=post_page-----d9212526e51d--------------------------------) [## 数据分析分享 | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读由 Egor Howell 编写的《数据分析分享》，这是一个 Substack 出版物，内容涵盖…

newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----d9212526e51d--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考资料与进一步阅读

+   一些进一步有趣的理论和推导：[`www.cs.princeton.edu/courses/archive/fall11/cos597C/lectures/exponential-families.pdf`](https://www.cs.princeton.edu/courses/archive/fall11/cos597C/lectures/exponential-families.pdf)

+   更多理论：[`www.stat.purdue.edu/~dasgupta/expfamily.pdf`](https://www.stat.purdue.edu/~dasgupta/expfamily.pdf)

+   GLMs 的经典文献：[`www.utstat.toronto.edu/~brunner/oldclass/2201s11/readings/glmbook.pdf`](https://www.utstat.toronto.edu/~brunner/oldclass/2201s11/readings/glmbook.pdf)
