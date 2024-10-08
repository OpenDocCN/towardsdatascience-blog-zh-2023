# 机器学习中的随机数

> 原文：[`towardsdatascience.com/random-numbers-in-machine-learning-9cb5d83d078e`](https://towardsdatascience.com/random-numbers-in-machine-learning-9cb5d83d078e)

## 关于伪随机数、种子设置和可重复性的一切

[](https://medium.com/@caroline.arnold_63207?source=post_page-----9cb5d83d078e--------------------------------)![Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----9cb5d83d078e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9cb5d83d078e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9cb5d83d078e--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----9cb5d83d078e--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9cb5d83d078e--------------------------------) ·8 分钟阅读·2023 年 10 月 20 日

--

![](img/cede17b2b0c90191d192b48d7de7c688.png)

图片来源：[Riho Kroll](https://unsplash.com/@rihok?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

机器学习依赖于统计学，随机数对数据处理和模型训练管道中许多步骤的性能至关重要。现代机器学习框架提供了在后台实现随机性的抽象和函数，而对于我们这些数据科学家和机器学习工程师来说，随机数生成的细节往往仍然模糊不清。

在本文中，我想探讨机器学习中的随机数。你将阅读到：

+   机器学习中随机数使用的 3 个示例

+   生成（伪）随机数

+   通过设定随机种子来固定随机数

+   可重复的机器学习：scikit-learn、tensorflow 和 pytorch 的必要代码行

在本文结束时，你将了解在机器学习管道中使用随机数时发生了什么，并学习确保机器学习算法可重复性的必要代码行。

## 机器学习中随机数的 3 个使用示例

为了说明随机数的重要性，我们讨论了机器学习管道中与随机数相关的三个示例。

1.  创建数据集的训练/测试划分

1.  神经网络中的权重初始化

1.  训练过程中的小批量选择

**训练/测试划分** 将数据集分为训练数据和测试数据是评估机器学习算法性能的最重要步骤之一。我们关注的是创建能很好泛化到训练过程中未使用的数据的模型。为此，一组数据样本被划分为至少两个不相交的集合。

训练数据用于训练算法，即迭代地调整模型参数。测试数据用于通过将训练好的模型应用于测试数据并报告适当的指标来验证算法。

![](img/9a5a0010d6e5ee72993ca18b135788a1.png)

数据集的随机划分为训练数据和测试数据。图片：作者。

流行的 scikit-learn 函数`sklearn.model_selection.train_test_split`使用随机数。在底层，它创建一个与数据集长度对应的索引数组。这些索引随后被随机分配到训练数据和测试数据中。

**权重初始化**

神经网络的层包含在训练过程中调整的参数。对于线性层，这些参数是权重和偏置。它们最初被赋予随机值。考虑一个 4 个神经元连接到具有 3 个潜在类别的输出层的线性层的示例：该层对应于一个 4 x 3 权重矩阵*W*。

![](img/59fffcc5ca648fe4d7673e49fed816f5.png)

4 x 3 矩阵 W 的三种不同随机初始化。图片：作者。

权重的初始化对模型训练的收敛至关重要。如果所有权重都具有相同的值，反向传播算法在更新权重时将没有理由对每个神经元进行不同的处理。

因此，当模型首次实例化时，权重是随机选择的。通常，这些随机数背后有合理的统计分布。Xavier Glorot 在 2010 年发现，当网络权重从均匀分布中初始化时，训练一个层级维度为 n x m 的前馈神经网络会得到改进[1]。

**选择训练批次** 通常，使用整个训练数据集一次性更新模型参数既不可行也不建议。因此，训练数据集被分成固定大小的小批量。数据加载器创建这些小批量并可以随机打乱数据。这是为了防止数据以有偏的方式进入训练，例如由于收集方式而按照时间顺序。

![](img/9245d04a92db4b56d02c9df60e9965fd.png)

从包含 12 个样本的数据集中形成的大小为 4 的小批量。顶部：数据集没有打乱。底部：数据集被随机打乱。图片：作者。

随机梯度下降依赖于这些小批量的随机性。通过向算法呈现训练数据的随机子集，每一步反向传播都会强调训练数据的一个略微不同的方面。这避免了在训练过程中陷入局部最小值。

## 伪随机数

现代编程语言和机器学习框架提供了生成随机数的工具，使开发者无需担心底层算法。要在 Python 中生成 100 个 0 到 1 之间的均匀随机数，只需输入

```py
import numpy as np
np.random.rand(100)
```

> 当你执行这一行代码时，实际上会发生什么？这时就引入了随机数生成器（RNGs）。

标准计算机算法产生可预测的结果，这恰恰与我们想要的相反——一个随机的数字序列。我们所寻找的是一个能生成*难以预测*的数字序列的算法。需要注意的是，*难*并不意味着*不可能*！因此，我们需要一个熵源和一个加密算法。对于真正的随机数生成器，熵源可以由环境提供，例如通过测量传感器温度或放射性粒子的衰变来获取输入 [3]。

对于机器学习，我们不需要像加密应用那样高端的随机性。Python — 特别是 numpy 库 — 实现了一个 RNG 来生成伪随机数。

在日常语言中，伪随机数序列足够接近真正的随机序列，以至于不完全的随机性不会影响算法的目的。序列的相关长度必须足够满足应用的需求。

从初始种子开始，应用一个函数 *f* 生成一个状态。另一个函数 *g* 被用来生成一个随机数。这一过程会重复进行多次。函数 *f, g* 应该是不可逆的。此外，不同的状态必须提供足够的变异性，以真正生成大量不同的随机数，这被称为序列长度。

![](img/58d59cfb5bde0f3915d838c17a967198.png)

遵循[4]的伪随机数生成器。图像：作者。

Python 中使用的底层算法叫做 Mersenne Twister [4]。它为每个状态提供随机数序列，并且比上面展示的简单实现计算效率更高。虽然它在加密上不安全，但它快速且足够强大。

## 随机数分布

我们通常感兴趣的是生成符合某种统计分布的随机数。例如，均匀分布在给定区间内为所有数字分配相等的概率。正态分布遵循具有固定均值和方差的高斯曲线。

![](img/7055ffbb5bf73a5f41c0b228d5808f2a.png)

1000 个随机数的正态分布（左）和均匀分布（右）。图像：作者。

随机数的分布可以通过其累积分布函数（CDF）相互转换。因此，只需要能够生成均匀分布的数字，其他任何分布都可以从中生成。具体细节超出了本文的范围，初步了解我们推荐[5]。

## 通过设定种子来固定随机数

如上所述，种子是一个整数，用作伪随机数生成器的输入。

> 对于固定的种子，由该算法生成的伪随机数序列总是相同的。

我们可以利用这个洞察来实现两个不同的目标。

**选择新的随机种子** 当你选择新的随机种子时，所有依赖于随机数的机器学习流程方面都将使用不同的值进行初始化。虽然底层分布和数据集大小保持不变，但训练过程可能会受到影响。

理想情况下，你的训练过程不应过于依赖随机种子的选择。你希望确保训练能够收敛，而不是依赖于那些神奇地产生良好结果的黄金运行，并且这些结果无法重复。

**修复随机种子** 修复随机种子在教学中尤为流行。当我为小数据集创建一个简单模型时，结果依赖于随机种子的概率相当高。每当我为学生准备教学材料，如 Jupyter 笔记本时，我都会修复随机种子，以便他们获得相同的结果。这减少了混淆，让学生能够专注于新概念，而不是度量标准中不那么重要的数字。

![](img/6c12caaf7bcc716e1731fce5a3656db6.png)

克隆看起来完全一样。照片由[Phil Shaw](https://unsplash.com/@phillshaw?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 机器学习中的可重复性

我们现在已经了解到，随机数对机器学习过程的性能至关重要。然而，如果我们希望算法的精确可重复性，就需要修复随机种子。

重要的是要意识到，随机种子必须为每个使用随机数的库单独修复。例如，即使我们通过`np.random.seed(789123)`修复了`numpy`的随机种子，`torch`的随机种子也不会受到影响，因此训练将无法重复。

在下面，我们总结了修复不同流行机器学习框架随机种子的必要调用。

**Scikit-Learn**在整个过程中使用 numpy 的随机数生成器。要修复该框架的随机种子，只需设置

```py
import numpy as np
np.random.seed(789456123)
```

流行的框架提供了另一种访问随机数生成器的函数，[*check_random_state*](https://scikit-learn.org/stable/modules/generated/sklearn.utils.check_random_state.html#sklearn.utils.check_random_state)。要快速生成 1000 个随机数：

```py
from sklearn.utils.validation import check_random_state
rs = check_random_state(12345)
rs.rand(1000)
```

**Tensorflow 2**提供了其自己的随机数生成模块`tf.random`。根据[文档](https://www.tensorflow.org/guide/random_numbers)，在那里修复随机种子可以确保可重复性。然而，这在不同版本的 tensorflow 中并不能保证。为了生成相同的伪随机数，可以使用`tf.random`模块中的`stateless_XXX`函数，例如，对于遵循正态分布的 1000 个随机数序列：

```py
from tf.random import stateless_normal

shape = 1000
seed = 12344
stateless_normal(shape, seed)
```

**Pytorch** [以类似于 numpy 语句的方式控制随机数生成](https://pytorch.org/docs/stable/notes/randomness.html)。以下将设置所有依赖于该随机数生成器的过程的种子：

```py
import torch
torch.manual_seed(9870)
```

然而，他们指出一些函数可能依赖于 numpy 随机数生成器，因此建议也固定该种子。

Pytorch 文档指出了一个关于完全可重复性的问题。不仅在不同版本之间不可能精确重现随机数，而且这些数字可能还会依赖于硬件。使用不同的 GPU 配合不同的 CUDA 工具包或 CPU 可能会导致不同的随机数序列。

## 总结

随机数对机器学习算法的性能至关重要。它们确保数据集以无偏差的方式进行划分，提高算法的泛化能力，并增加训练过程的收敛性。

在幕后，随机数序列由所谓的 RNGs 生成，这些算法能够提供遵循不同分布的随机数。

随机种子可以用来固定随机数序列。这使得机器学习算法具有可重复性，这对于教学目的和学术论文特别有用。最后，请记住，如果你在项目中使用了多种机器学习框架，你可能需要为所有框架设置随机种子！

## 参考资料

1.  Xavier Glorot 和 Yoshua Bengio, “Understanding the difficulty of training deep feedforward neural networks”, *Proceedings of the Thirteenth International Conference on Artificial Intelligence and Statistics*, PMLR 9:249–256, 2010\. [[paper](http://proceedings.mlr.press/v9/glorot10a.html)]

1.  [`numpy.org/doc/stable/reference/random/index.html`](https://numpy.org/doc/stable/reference/random/index.html)

1.  [`www.redhat.com/en/blog/understanding-random-number-generators-and-their-limitations-linux`](https://www.redhat.com/en/blog/understanding-random-number-generators-and-their-limitations-linux)

1.  详细介绍梅森旋转算法: [`www.cryptologie.net/article/331/how-does-the-mersennes-twister-work/`](https://www.cryptologie.net/article/331/how-does-the-mersennes-twister-work/)

1.  [`www.wikiwand.com/en/Inverse_transform_sampling`](https://www.wikiwand.com/en/Inverse_transform_sampling)

1.  Tensorflow: [`www.tensorflow.org/guide/random_numbers`](https://www.tensorflow.org/guide/random_numbers)

1.  Pytorch: [`pytorch.org/docs/stable/notes/randomness.html`](https://pytorch.org/docs/stable/notes/randomness.html)
