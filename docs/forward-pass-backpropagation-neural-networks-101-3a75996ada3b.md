# 神经网络中的前向传播与反向传播

> 原文：[`towardsdatascience.com/forward-pass-backpropagation-neural-networks-101-3a75996ada3b`](https://towardsdatascience.com/forward-pass-backpropagation-neural-networks-101-3a75996ada3b)

## 解释神经网络如何通过手动和代码中的 PyTorch 进行“训练”和“学习”数据模式

[](https://medium.com/@egorhowell?source=post_page-----3a75996ada3b--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----3a75996ada3b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3a75996ada3b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3a75996ada3b--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----3a75996ada3b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3a75996ada3b--------------------------------) ·10 分钟阅读·2023 年 11 月 4 日

--

![](img/a894bd9e34501e8413c8bf96a918f1e5.png)

神经网络图标由 juicy_fish 创建 — Flaticon. [`www.flaticon.com/free-icons/neural-network`](https://www.flaticon.com/free-icons/neural-network).

# 背景

在我过去的两篇文章中，我们探讨了神经网络从单一的 [***感知机***](https://en.wikipedia.org/wiki/Perceptron)到大型互联的 ([***多层感知机***](https://en.wikipedia.org/wiki/Multilayer_perceptron) ***(MLP)*** ) 非线性优化引擎的起源。如果你对感知机、MLP 和激活函数不熟悉，我强烈建议你查看我之前的帖子，因为我们在这篇文章中将讨论很多：

[](https://levelup.gitconnected.com/intro-perceptron-architecture-neural-networks-101-2a487062810c?source=post_page-----3a75996ada3b--------------------------------) [## 介绍，感知机与架构：神经网络 101

### 神经网络及其构建块的介绍

levelup.gitconnected.com](https://levelup.gitconnected.com/intro-perceptron-architecture-neural-networks-101-2a487062810c?source=post_page-----3a75996ada3b--------------------------------) [](/activation-functions-non-linearity-neural-networks-101-ab0036a2e701?source=post_page-----3a75996ada3b--------------------------------) ## 激活函数与非线性：神经网络 101

### 解释为什么神经网络可以学习（几乎）任何事物

towardsdatascience.com

现在是时候理解这些神经网络如何“训练”和“学习”你传递给它的数据中的模式了。有两个关键组件：[***前向传播***](https://theneuralblog.com/forward-pass-backpropagation-example/) 和 [***反向传播***](https://en.wikipedia.org/wiki/Backpropagation)。让我们深入了解吧！

# 架构

让我们快速回顾一下神经网络的一般结构：

![](img/94cdac9897a5e852d9cd6edd425fcba0.png)

一个基本的两层隐藏的多层感知器。图示由作者提供。

每个隐藏神经元执行以下过程：

![](img/57f8921c8f7e374d7f075b2f4ae32e6b.png)

每个神经元内部执行的过程。图示由作者提供。

+   ***输入:*** *这些是我们数据的特征。*

+   ***权重:*** *我们乘以输入的一些系数。算法的目标是找到最优的权重。*

+   ***线性加权和:*** *将输入和权重的乘积加起来，并添加一个偏置/偏移项，* ***b****。

+   ***隐藏层:*** *这是存储多个神经元以学习数据模式的地方。上标指的是层，下标指的是该层中的神经元/感知器。*

+   ***边缘/箭头:*** *这些是网络的权重，从相应的输入中获取，无论是特征还是隐藏层输出。我已经省略它们以使图表更简洁。*

+   [***ReLU 激活函数***](https://en.wikipedia.org/wiki/Heaviside_step_function)***:*** *最流行的* [***激活函数***](https://en.wikipedia.org/wiki/Activation_function) *因为它计算效率高且直观。*

# 前向传播

## 概述

训练神经网络的第一部分是让它生成预测。这被称为 **前向传播**，即数据从第一层经过所有神经元到最后一层（也称为输出层）。

对于本文，我们将手动进行前向传播。实际上，我们会使用像 [***PyTorch***](https://pytorch.org/) 或 [***TensorFlow***](https://www.tensorflow.org/)*** 的包。但这将帮助我们更好地理解过程。

## 示例神经网络

我们将执行前向传播的神经网络如下所示：

![](img/07e85418b0df7415133c2c8500b7e803.png)

简单的神经网络。图示由作者提供。

如你所见，使用两个输入、隐藏层中的两个神经元（具有 *ReLU* 激活函数）和一个预测（输出层）非常简单。

## 权重和偏置

现在我们为这个简单的网络创建一些任意的权重、偏置和输入：

**输入值:** *[0.9, 1.0]*

**目标/真实值:** *[2.0]*

**输入到隐藏层的权重，*W_1:***

+   *神经元 1: W_{1,1} = [0.2, 0.3]*

+   *神经元 2: W_{1,2} = [0.4, 0.5]*

**隐藏层偏置，*b_1*:** *[0.1, 0.2]*

**隐藏到输出层的权重，** ***W_2***: *[0.5, 0.6]*

**输出层偏置，*b_2***: *[0.4]*

需要注意的是，在这种情况下我随机生成了初始权重和偏置。这并不是坏事，你可以通过完全随机的初始化获得不错的结果。然而，还有更复杂的方法：

+   [**Xavier**](https://365datascience.com/tutorials/machine-learning-tutorials/what-is-xavier-initialization/#h_75113286374001686829810816): *适用于* [***sigmoid***](https://en.wikipedia.org/wiki/Sigmoid_function) *和* [***tanh***](https://en.wikipedia.org/wiki/Hyperbolic_functions) *激活函数。它从均匀分布中生成随机权重，利用该节点的输入数量来设置分布范围。*

+   [**He**](https://machinelearningmastery.com/weight-initialization-for-deep-learning-neural-networks/): *适用于 ReLU 激活函数。它从正态分布中生成随机权重，利用该节点的输入数量来设置标准差。*

如果你想了解更多关于这些初始化方法的信息，请查看上面列表中的链接。

关于权重初始化还有一个重要的注意事项是确保它们不同，以便我们‘**打破对称性。**’ 如果同一层的神经元以相同的权重开始，它们很可能会被完全相同地更新。因此，网络将不会收敛，无法学习任何东西。

## 第一次前向传播

使用上面列出的数据，我们现在可以进行第一次前向传播！结果如下：

**从输入到隐藏层的线性加权和，*z¹_{1}* 和 *z¹_{2}*，是：**

![](img/adbe828addffb5cfa63bd36204d1787f.png)

由作者在 LaTeX 中生成的方程。

> 记住，上标表示层，下标表示该层中的神经元。第一层被认为是紧接着输入层的那一层，因为输入层不进行任何计算。因此，在我们的情况下，我们有一个 2 层的网络。

在这里，我们利用了 [***点积***](https://en.wikipedia.org/wiki/Dot_product) 来简化和压缩计算。 **现在，我们可以在隐藏层中执行 ReLU 激活函数，*a¹_{1,1}* 和 *a¹_{1,2}*：**

![](img/90e70a103b4f3eba781e350a935ae01f.png)

由作者在 LaTeX 中生成的方程。

**我们需要做的最后一步是生成输出，** *z²_{1*}**，这没有关联的激活函数：**

![](img/e78c0ef07eced4faaa31e59a9eb6501a.png)

由作者在 LaTeX 中生成的方程。

> 哇，我们刚刚完成了第一次前向传播！
> 
> 注意：我通过将权重写为向量/矩阵来简化表达。这是文献中常见的做法，以使工作流程更加整洁。

前向传播也如下图所示：

![](img/72afd49009b5268e207f6ab02e502288.png)

简单神经网络及其权重、偏置和输出。图由作者提供。

# 反向传播算法

## 概述

完成正向传播后，我们现在可以开始更新权重和偏置，以最小化网络预测的误差。权重和偏置的更新是通过 *反向传播* 算法实现的。

## 计算图、链式法则和梯度下降

现在，让我们理解一下这个算法的直观意义。反向传播旨在获得每个权重和偏置对误差（损失）的偏导数。然后使用 ***梯度下降*** 更新每个参数，以最小化由每个参数引起的误差（损失）。

对，这在纸面上可能有意义，但我理解它仍然可能显得有些任意。我想通过一个真正的“简单”示例来深入了解 [***计算图***](https://www.tutorialspoint.com/python_deep_learning/python_deep_learning_computational_graphs.htm)。

考虑以下函数：

![](img/f8cb287c88da22f545f8c2a619bdff87.png)

由作者在 LaTeX 中生成的方程。

我们可以将其绘制为计算图，这只是另一种可视化计算的方法：

![](img/4047cf68f641963e1b2e1eeb0d893a1e.png)

计算图示例。图示由作者提供。

基本上这是一个计算 ***f(x,y,z)*** 的流程图。我也表达了 ***p=x-y***。现在我们来插入一些数字：

![](img/deb6bcaa4c35868c02d2abd5978aa53e.png)

带有数字的计算图示例。图示由作者提供。

*这些看起来到目前为止都很好且直观！*

我们计算 ***f(x,y,z)*** 最小值的方法是使用微积分。特别是，我们需要知道 ***f(x,y,z)*** 对其所有三个变量 ***x, y,*** 和 ***z*** 的偏导数。

我们可以开始计算 ***p=x-y*** 和 ***f=pz*** 的偏导数：

![](img/faa5838ed3085ddda4d76e6f939804bb.png)

由作者在 LaTeX 中生成的方程。

*但是，我们该如何进行呢？*

![](img/52687bd4c8162d292dd4b26310773380.png)

由作者在 LaTeX 中生成的方程。

好吧，我们使用 [***链式法则！***](https://en.wikipedia.org/wiki/Chain_rule) 这是一个 ***x*** 的示例：

![](img/95137bfc8b2891a2d09180e6b82b98d2.png)

由作者在 LaTeX 中生成的方程。

通过组合不同的偏导数，我们可以得到我们期望的表达式。因此，对于上述示例：

![](img/7568e02dcb2b420c7e448156b669dda2.png)

由作者在 LaTeX 中生成的方程。

输出的梯度 ***f*** 对 ***x*** 的梯度是 ***z***。这很有意义，因为 ***z*** 是我们唯一乘以 ***x*** 的值。

对 ***y*** 和 ***z*** 进行重复：

![](img/f56f6e7e20f83bf96d3cd44d9cbb159b.png)

由作者在 LaTeX 中生成的方程。

现在，我们可以在计算图上写出这些梯度及其对应的值：

![](img/578b82e9ed62175c510b6eada2f180ae.png)

带有数字和梯度的计算图示例。图示由作者提供。

梯度下降通过在梯度的相反方向上小幅度更新值（***x, y, z***）来工作。梯度下降的目标是尽量减少输出函数。例如，对于 ***x:***

![](img/5866f8f21dd278964bc6c1743a9eeed7.png)

作者生成的 LaTeX 方程式。

其中 ***h*** 称为 [***学习率***](https://deepchecks.com/glossary/learning-rate-in-machine-learning/#:~:text=The%20learning%20rate%2C%20denoted%20by,network%20concerning%20the%20loss%20gradient%3E.)，决定了参数更新的幅度。在这个例子中，让我们定义 ***h=0.1***，所以 ***x=3.7***。现在的输出是什么？

![](img/95a57e60032255539a1cbb03d242367b.png)

执行梯度下降后的计算图示例，包含数字和梯度。图示由作者提供。

输出变小了，换句话说，它正在被最小化！

> 这个例子灵感来源于 [Andrej Karpathy](https://www.youtube.com/@andrejkarpathy4906) 在 [YouTube](https://www.youtube.com/watch?v=i94OvYb6noo&t=358s) 上的视频。我强烈推荐你查看，以深入了解这一过程。

## 应用反向传播

*以上所有内容如何与神经网络的训练相关？*

好了，神经网络其实就是一个计算图！输入类似于上面的 ***x, y*** 和 ***z***，而操作门类似于神经元中的激活函数。

现在，让我们将这一过程应用于我们之前的简单示例神经网络。记住，我们的预测是 ***1.326***，假设目标是 ***2.0***。这个预测中的损失函数（误差）可以是 [***均方误差***](https://en.wikipedia.org/wiki/Mean_squared_error)：

![](img/5ad5582ee7f5a12428e564dfcdc0e90b.png)

作者生成的 LaTeX 方程式。

> 记住附加在 z 上的 2 是层数，而不是幂次项！

下一步是计算相对于预测/输出的损失梯度：

![](img/ae7e2e12dc0717c6c9301f931e3ed3b7.png)

作者生成的 LaTeX 方程式。

现在，我们需要计算相对于输出层偏置和权重的损失梯度：***W_2*** 和 ***b_2:***

![](img/02746a72459aeb1efc366008137b9b49.png)

作者生成的 LaTeX 方程式。

如果上述表达式看起来让你感到害怕，不必担心。我们所做的只是进行了偏导数计算，并多次应用了链式法则。

![](img/ab1122a7115e3527d82642c02ee69d5a.png)

作者生成的 LaTeX 方程式。

最后一步是使用梯度下降更新参数，其中我设置了学习率 ***h=0.1***：

![](img/99bebba4589568a8cea2d113f6dbea10.png)

作者生成的 LaTeX 方程式。

> Voilà，我们已经更新了输出层的权重和偏置！

接下来是对隐藏层的权重和偏置重复这一过程。首先，我们需要找到相对于激活函数输出的损失，***a¹_{1,1}* 和 *a¹_{1,2}***：

![](img/1e00c4f35f203e86ec6f6eee5487b668.png)

由作者在 LaTeX 中生成的方程。

现在，我们找到 **z*¹_{1}* 和 z*¹_{2}* 的导数：**

![](img/cb7bdd6f67b656c6e9762ea7dda02a5a.png)

由作者在 LaTeX 中生成的方程。

一些背景信息：

![](img/8f36e173b6bc3c4622c411947510c85e.png)

由作者在 LaTeX 中生成的方程。

下一步是找到相对于 ***W_{1,1}*** 和 ***W_{1,2}*** 的损失。在此之前，我想重新定义一些符号，以便清楚我们在处理哪个权重：

![](img/6289f16900a84c4fed7ebcbe97d64af5.png)

由作者在 LaTeX 中生成的方程。

因此：

![](img/80cbe028c9930838a828747967d7f811.png)

由作者在 LaTeX 中生成的方程。

因此，相对于权重和偏置的损失的偏导数是：

![](img/9b9cfe0f15de03a04cd2da7ce0ed431d.png)

由作者在 LaTeX 中生成的方程。

最后一步是使用梯度下降来更新这些值：

![](img/dd22b10d72a82642a99a80a0cfce5973.png)

由作者在 LaTeX 中生成的方程。

> 哇，我们刚刚完成了整个反向传播的迭代！

一个前向传播和反向传播（反向传播）的周期被称为 [**epoch**](https://www.baeldung.com/cs/epoch-neural-networks)。

下一步是使用更新后的权重和偏置进行另一次前向传播。这个前向传播的结果将是 **1.618296**。这接近目标值 **2**，因此网络已经“学习”到了更好的权重和偏置。这就是“机器学习”在发挥作用。真的很惊人！

实际上，这个算法会运行 100 或 1000 次 epoch。幸运的是，已经存在处理此过程的包，因为手动完成会非常繁琐！

## 额外细节

可以看出，为什么这个过程被称为反向传播，因为我们在网络的每一层向后传播误差（导数）。一旦你掌握了它，理解起来就很简单。因此，我强烈建议你慢慢地进行这个过程，最好自己动手，我保证你会很快理解！

# PyTorch 示例

手动完成，如上所述，确实很耗费精力，而且这只是一个小网络。然而，我们可以利用 PyTorch（一个深度学习库）来为我们完成所有这些繁重的工作：

GitHub Gist 由作者提供。

因此，我们手动编写的整个前向传播和反向传播过程可以在 ~50 行代码中完成！Python 的威力！

然而，由于随机化、浮点精度和其他计算机因素，权重、偏置和预测可能与手动计算的结果不完全匹配。这不是问题，但重要的是要意识到这一点。

> 如果你想了解更多关于 PyTorch 的内容，可以查看他们网站上的 [教程](https://pytorch.org/tutorials/)。

# 总结与进一步思考

在本文中，我们探讨了神经网络如何生成预测并从错误中学习。这个过程围绕使用部分微分更新网络的参数，针对损失误差。算法是反向传播，因为它通过链式法则将误差反向传播通过每一层。一旦你掌握了它，反向传播相当直观，但对于大型神经网络来说非常繁琐。这就是我们使用像 PyTorch 这样的深度学习库，它为我们做了大部分繁重的工作。

本文中使用的完整代码可以在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/blob/main/Neural%20Networks/basic_foward_backward_pass.py?source=post_page-----3a75996ada3b--------------------------------) [## Medium-Articles/Neural Networks/basic_foward_backward_pass.py 在主分支 · egorhowell/Medium-Articles

### 我在我的中等博客/文章中使用的代码。通过在…上创建帐户来贡献 egorhowell/Medium-Articles 的开发。

github.com](https://github.com/egorhowell/Medium-Articles/blob/main/Neural%20Networks/basic_foward_backward_pass.py?source=post_page-----3a75996ada3b--------------------------------)

# 参考资料与进一步阅读

+   [*Andrej Karpathy 神经网络课程*](https://www.youtube.com/watch?v=i94OvYb6noo)

+   [*PyTorch 网站*](https://pytorch.org/)

+   *另一个手动训练神经网络的例子*

# 另一件事！

我有一个免费的新闻通讯，[**数据解析**](https://dishingthedata.substack.com/)，在其中我分享成为更优秀数据科学家的每周技巧。没有“虚华”或“点击诱饵”，只有来自实践数据科学家的纯粹可操作的见解。

[](https://newsletter.egorhowell.com/?source=post_page-----3a75996ada3b--------------------------------) [## 数据解析 | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读《数据解析》，由 Egor Howell 撰写，这是一个 Substack 出版物，内容包括…

newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----3a75996ada3b--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)
