# 神经网络与深度学习的激活函数

> 原文：[`towardsdatascience.com/activation-functions-non-linearity-neural-networks-101-ab0036a2e701`](https://towardsdatascience.com/activation-functions-non-linearity-neural-networks-101-ab0036a2e701)

## 解释神经网络如何学习（几乎）任何和所有事物

[](https://medium.com/@egorhowell?source=post_page-----ab0036a2e701--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----ab0036a2e701--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab0036a2e701--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab0036a2e701--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----ab0036a2e701--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab0036a2e701--------------------------------) ·8 分钟阅读·2023 年 10 月 12 日

--

![](img/f20f48d260492d8ecb7fb2bf39ec6862.png)

机器学习图标由 Becris — Flatico 创建。 [`www.flaticon.com/free-icons/machine-learning`](https://www.flaticon.com/free-icons/machine-learning)

# 背景

在我之前的文章中，我们介绍了 [***多层感知器***](https://en.wikipedia.org/wiki/Multilayer_perceptron) ***(MLP)****，它只是堆叠互连的 [***感知器***](https://en.wikipedia.org/wiki/Perceptron) 的集合。如果你不熟悉感知器和 MLP，我强烈建议你查看我之前的文章，因为这篇文章将会详细讨论：

[](https://levelup.gitconnected.com/intro-perceptron-architecture-neural-networks-101-2a487062810c?source=post_page-----ab0036a2e701--------------------------------) [## 介绍、感知器与架构：神经网络 101

### 神经网络及其构建模块的介绍

levelup.gitconnected.com](https://levelup.gitconnected.com/intro-perceptron-architecture-neural-networks-101-2a487062810c?source=post_page-----ab0036a2e701--------------------------------)

一个具有两个隐藏层的示例 MLP 如下所示：

![](img/94cdac9897a5e852d9cd6edd425fcba0.png)

一个基本的双隐藏层多层感知器。图示由作者提供。

然而，MLP 的问题在于它只能拟合一个 [***线性分类器***](https://en.wikipedia.org/wiki/Linear_separability)。这是因为各个感知器具有 [***阶跃函数***](https://en.wikipedia.org/wiki/Step_function) 作为其 [***激活函数***](https://en.wikipedia.org/wiki/Activation_function)，这是一种线性函数：

![](img/aa3d36442d4a95bbdd62ecbac912ad3a.png)

感知器，这是最简单的神经网络。图示由作者提供。

尽管堆叠我们的感知器看起来像是现代神经网络，但它仍然是一个线性分类器，与普通的线性回归没有太大区别！

> 另一个问题是，它在整个领域范围内并非完全可微分。

那么，我们该如何处理呢？

> 非线性激活函数！

# 为什么我们需要非线性？

## 什么是线性性？

让我们快速说明一下线性性的含义，以建立一些背景。数学上，如果一个函数满足以下条件，则认为它是线性的：

![](img/5ff20f8ab0a88e38e57c203934156124.png)

作者用 LaTeX 编写的方程。

还有另一个条件：

![](img/7924532d6bff8fdc08fb33c5d3f79615.png)

作者用 LaTeX 编写的方程。

但我们将使用之前的方程进行演示。

以这个非常简单的例子为例：

![](img/27e61559c5cf914c7b7fe6a28aeac415.png)

作者用 LaTeX 编写的方程。

所以函数 ***f(x) = 10x*** 是线性的！

> 如果我们在上述方程中添加一个偏置项，它将不再是线性函数，而是[**仿射函数**](https://en.wikipedia.org/wiki/Affine_transformation)。请参见[这个 statexchange 线程](https://math.stackexchange.com/questions/275310/what-is-the-difference-between-linear-and-affine-function)了解其区别。

现在，考虑以下情况：

![](img/856701d94e726496754d19133fe10b32.png)

作者用 LaTeX 编写的方程。

所以函数 ***f(x) = 10x²*** 是 *非* 线性的。如我们大多数人所知，这个函数是二次的，具有抛物线形状：

作者总结。

![](img/9100d65fd7dc2194a0e23a1f4a431761.png)

示例抛物线。图形由作者在 Python 中生成。

> 绝对不是线性的！

## 神经网络中的线性性

我们已经回答了为什么神经网络需要非线性，但让我们深入探讨一下这个概念以及它对我们模型的意义。

每个感知器的输出可以用以下数学公式表示：

![](img/60e9f3aa07b7135e6c0eadb2b8a184ca.png)

作者用 LaTeX 编写的方程。

其中 ***f*** 是步进函数（激活函数），***y*** 是感知器的输出，***b*** 是偏置项，***n*** 是特征数量，***w_i*** 和 ***x_i*** 是[***权重***](https://en.wikipedia.org/wiki/Weighting) 及其对应的输入值。这个方程只是上面感知器图示的数学版本。

现在，假设我们的激活函数是恒等函数，换句话说，我们没有任何函数。使用这一概念，现在考虑一个前馈的双隐藏层 MLP 中间层有两个神经元（忽略偏置项）：

![](img/7e743bf6079fe4879b43c69e4dc07d27.png)

两层前馈神经网络。作者用 LaTeX 编写的方程。

你可能会想知道这意味着什么。

好吧，我们在这里做的就是将 2 层 MLP 压缩成一个单层 MLP！上面推导出的最终方程仅仅是具有特征 ***x_1*** 和 ***x_2*** 及其对应系数的线性回归模型。

因此，尽管 MLP 是“两层”的，它会变成单层，从而成为传统的线性回归模型！这不好，因为神经网络无法对数据进行建模或拟合复杂函数。

通过允许 MLP 使用非线性激活函数，我们满足了所谓的 [***通用逼近定理***](https://en.wikipedia.org/wiki/Universal_approximation_theorem)。这个定理基本上表示 MLP，或者更准确地说是神经网络，可以逼近和拟合任何函数。如果你想了解更多，查看 [这里](http://mcneela.github.io/machine_learning/2017/03/21/Universal-Approximation-Theorem.html)！

线性并不一定是坏事，更重要的是许多现实世界的问题是非线性的，因此如果我们想预测这些现象，就需要非线性模型。

> 激活函数的另一个要求是需要可微分，以便启用 **梯度下降**。

# 激活函数

现在让我们快速浏览一些最常见的非线性激活函数。

## Sigmoid

[***Sigmoid 函数***](https://en.wikipedia.org/wiki/Sigmoid_function) 在业界非常知名，源于 [***二项分布***](https://medium.com/towards-artificial-intelligence/decoding-the-binomial-distribution-a-fundamental-concept-for-data-scientists-81c64c7e4580)。它将输入“挤压”到 0 和 1 之间的输出，因此可以被视为概率，并且具有“S 形”曲线。然而，最重要的是它是非线性的，因为它包含了一个除法和一个指数函数。

Sigmoid 函数在数学上和视觉上看起来如下：

![](img/29fff662bec33adab7258d011d2e9db6.png)

方程由作者在 LaTeX 中生成。

作者总结。

![](img/b71b01559c120a8967c3bf80bc04c16f.png)

Sigmoid 函数。图表由作者在 Python 中生成。

然而，Sigmoid 函数在前沿神经网络中并未使用，原因如下：

+   [**消失梯度问题**](https://en.wikipedia.org/wiki/Vanishing_gradient_problem) **—** 从上述图中可以看到，在极端正值或负值时，梯度接近零。这在训练神经网络时是一个问题，因为它会减缓学习和收敛的速度。

+   **未以零为中心** — Sigmoid 函数在 0 和 1 之间，因此它始终是正的。这在使用基于梯度下降的算法时是一个问题，因为它会朝一个方向推动或下降。

+   **计算开销大** — 计算指数运算并不高效，因此这个函数特别是在大型神经网络中被计算数千次时，开销很大。

## Tanh

[***Tanh***](https://en.wikipedia.org/wiki/Hyperbolic_functions) 是一个常见的三角超越函数，它将输入映射到 -1 和 1 之间。因此，它是以零为中心的，相比于 Sigmoid 函数有所改进。

Tanh 函数在数学上和视觉上看起来如下：

![](img/fc548876673a33f9076e8494e4315631.png)

由作者用 LaTeX 编写的方程。

作者的概要。

![](img/b43c2e3f628349da147ec521cb2ded5b.png)

Tanh 函数。由作者在 Python 中生成的图。

尽管以零为中心，tanh 函数也面临与 sigmoid 函数类似的问题：

+   **梯度消失问题 —** Tanh 的梯度在正负极值处也接近零。这在训练神经网络时是一个问题，因为它会减缓学习和收敛速度。

+   **计算开销大 —** Tanh 需要计算更多的指数函数，这使得它在计算上比 sigmoid 更低效。

## ReLU

[***修正线性单元 (ReLU)***](https://en.wikipedia.org/wiki/Rectifier_%28neural_networks%29) 是最受欢迎的激活函数，因为它计算高效并且解决了上述的梯度消失问题。

在数学上和视觉上，它看起来像：

![](img/ad1cf21ebe2a1666034bee8102b78d29.png)

由作者用 LaTeX 编写的方程。

作者的概要。

![](img/cf6409fc078754bcab71802d9bbe477e.png)

ReLU。由作者在 Python 中生成的图。

尽管 ReLU 激活函数相比于 sigmoid 和 tanh 函数有许多优点，但它也存在一些新问题：

+   [**死亡神经元**](https://datascience.stackexchange.com/questions/5706/what-is-the-dying-relu-problem-in-neural-networks) **—** 存在一种“死亡 ReLU 问题”，即由于对任何小于零的值都设定为零，导致神经元对任何输入始终输出零。

+   **无界值 —** 梯度可以达到正无穷大。这在极端情况下可能导致计算问题。

+   **不以零为中心** — ReLU 也不以零为对称，其均值为正。这可能在训练过程中引发问题。

## Leaky ReLU

我们可以调整 ReLU 函数以生成‘Leaky’ ReLU，其中负输入不为零，而是有一个浅斜率的梯度 ***α***：

![](img/d5dbcbc68a24c8ab953e5b19704fabfa.png)

由作者用 LaTeX 编写的方程。

作者的概要。

![](img/4d44ec1c36473a7f01361080256878af.png)

Leaky ReLU。由作者在 Python 中生成的图。

Leaky ReLU 通过拥有少量的梯度来避免死亡神经元问题，从而使神经元能够继续学习。更不用说它避免了梯度消失问题，并且计算上更高效。然而，与其他函数一样，它也有一些缺陷：

+   **斜率选择**：Leaky 斜率的梯度没有规定，需要手动选择。不过，它可以通过超参数调整进行优化。

+   **不以零为中心** — 与普通 ReLU 相似，它也不以零为对称。

+   **无界值 —** 与 ReLU 类似，其梯度可以达到正无穷大，实际上是无界的。

## 其他

在这里，我列出了四种最常见的激活函数，然而还有一些其他函数，例如：

+   [**Swish**](https://medium.com/@neuralnets/swish-activation-function-by-google-53e1ea86f820)**:** 这是由 Google 开发的 ReLU 函数的可微近似，***swish(x) = x * sigmoid(x)***。

+   **门控线性单元（GLU）**：主要用于[**递归神经网络**](https://en.wikipedia.org/wiki/Recurrent_neural_network)。

+   **Softmax**：主要用于多项分类问题。

一种激活函数并不一定优于另一种，最好是尝试各种激活函数，看看哪一种最适合你的模型。

# 总结与进一步思考

要使神经网络满足通用逼近定理，它需要包含一个非线性激活函数，否则你可以将其压缩到单层，基本上就是一个线性回归模型。有很多合适的激活函数，最常用的是 ReLU。然而，这并不是一个硬性规则，你应该尝试各种函数以找到最适合你模型的那一个。

本文中使用的完整代码可以在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/blob/main/Data%20Science%20Basics/activation_functions.py?source=post_page-----ab0036a2e701--------------------------------) [## Medium-Articles/Data Science Basics/activation_functions.py at main · egorhowell/Medium-Articles

### 我在 Medium 博客/文章中使用的代码。通过创建一个账户来贡献于 egorhowell/Medium-Articles 的开发…

github.com](https://github.com/egorhowell/Medium-Articles/blob/main/Data%20Science%20Basics/activation_functions.py?source=post_page-----ab0036a2e701--------------------------------)

# 参考资料与进一步阅读

+   *关于激活函数的优秀文章*

+   [*非线性神经网络*](https://medium.com/ml-cheat-sheet/understanding-non-linear-activation-functions-in-neural-networks-152f5e101eeb)

+   [*神经网络简介*](https://web.pdx.edu/~nauna/week7b-neuralnetwork.pdf)

# 另一件事！

我有一个免费的新闻简报，[**Dishing the Data**](https://dishingthedata.substack.com/)，每周分享成为更好的数据科学家的小贴士。没有“废话”或“点击诱饵”，只有来自实际数据科学家的纯粹可操作见解。

[](https://newsletter.egorhowell.com/?source=post_page-----ab0036a2e701--------------------------------) [## Dishing The Data | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读 Egor Howell 的 Dishing The Data，这是一个 Substack 出版物，内容包括…

newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----ab0036a2e701--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)
