# 适合你神经网络的最佳优化算法

> 原文：[`towardsdatascience.com/the-best-optimization-algorithm-for-your-neural-network-d16d87ef15cb`](https://towardsdatascience.com/the-best-optimization-algorithm-for-your-neural-network-d16d87ef15cb)

## 如何选择它并最小化你的神经网络训练时间。

[](https://medium.com/@riccardo.andreoni?source=post_page-----d16d87ef15cb--------------------------------)![Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----d16d87ef15cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d16d87ef15cb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d16d87ef15cb--------------------------------) [Riccardo Andreoni](https://medium.com/@riccardo.andreoni?source=post_page-----d16d87ef15cb--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d16d87ef15cb--------------------------------) ·13 分钟阅读·2023 年 10 月 14 日

--

![](img/5d74e7bc343277d4b012a49a8ed2de2f.png)

图片来源：[unsplash.com](https://unsplash.com/photos/FfbVFLAVscw)。

开发任何机器学习模型都涉及一个严格的实验过程，该过程遵循**思想-实验-评估循环**。

![](img/89f8be024c84d92865f49b0077b29126.png)

图片由作者提供。

上述循环重复多次，直到达到令人满意的性能水平。“实验”阶段包括机器学习模型的编码和训练步骤。由于**模型变得更加复杂**且在更**大的数据集**上训练，训练时间不可避免地增加。因此，训练大型深度神经网络可能会非常缓慢。

对于数据科学从业者来说，幸运的是，存在几种加速训练过程的技术，包括：

+   **迁移学习**。

+   **权重初始化**，如 Glorot 或 He 初始化。

+   **批量归一化**用于训练数据。

+   选择一个**可靠的激活函数**。

+   使用**更快的优化器**。

尽管我指出的所有技术都很重要，但在这篇文章中我将深入关注最后一点。我将描述多种神经网络参数优化算法，强调它们的优点和局限性。

在这篇文章的最后一部分，我将展示一个可视化图，显示讨论过的优化算法之间的比较。

对于**实际实现**，本文中使用的所有代码可以在这个**GitHub 仓库**中访问：

[## articles/NN-optimizer at main · andreoniriccardo/articles

### 通过在 GitHub 上创建一个帐户，参与 andreoniriccardo/articles 的开发。

[github.com](https://github.com/andreoniriccardo/articles/tree/main/NN-optimizer?source=post_page-----d16d87ef15cb--------------------------------)

# 批量梯度下降

传统上，批量梯度下降被认为是神经网络中优化方法的**默认选择**。

在神经网络对整个训练集*X*生成预测之后，我们将网络的预测与每个训练点的实际标签进行比较。这是为了计算一个成本函数*J(W,b)*，它是模型生成准确预测能力的标量表示。梯度下降优化算法使用成本函数作为指导来调整每个网络参数。这个迭代过程持续进行，直到成本函数接近零，或无法进一步降低。梯度下降算法的作用是以特定方向调整网络的每个权重和偏置。选择的方向是最能降低成本函数的方向。

![](img/032bf6c09d88228ba0b850a1ae27dcc4.png)

作者提供的图片。

从视觉角度来看，可以参考上面的图像。梯度下降从最左边的蓝点开始。通过分析起始点周围的成本函数梯度，它将参数（x 轴值）调整到右侧。这个过程重复多次，直到算法得到了非常好的最优近似。

如果成本函数有 2 个输入参数，函数不是一条直线，而是创建了一个三维表面。可以考虑该表面的等高线来显示批量梯度下降步骤：

![](img/c005940ab8e986e586912d524d4b1965.png)

作者提供的图片。

神经网络由成千上万的参数组成，这些参数会影响成本函数。虽然可视化百万维度的表示是不切实际的，但数学方程对理解梯度下降过程很有帮助。

首先，我们计算成本函数*J(W,b)*，这是网络权重*W*和偏置*b*的函数。然后，反向传播算法计算成本函数相对于网络每层的每个权重和偏置的导数：

![](img/b9fac62a55efc0ded9e5e592526f0797.png)![](img/d14e35916ec9cbcb3f90950c9b0f03b8.png)

知道了调整参数的方向后，我们更新它们。每个参数更新的幅度由梯度本身和学习率 alpha 调节。Alpha 是优化算法的一个超参数，其值通常保持不变。

![](img/653c4eb05a3afa4a33499ea2f81524f3.png)

批量梯度下降提供了几个优点：

+   **简洁性**：这是一种直接且易于理解的方法。

+   **超参数管理**：只需要调整一个超参数，即学习率。

+   **最优性**：对于凸成本函数，它可靠地在合理的学习率下达到全局最优。

梯度下降的缺点是：

+   **计算强度**：它可能较慢，特别是对于大型训练集，因为在评估所有样本后更新参数。

+   **局部最优和鞍点**：它可能会陷入局部最优或鞍点，从而减慢收敛速度。

+   **梯度问题**：它容易出现梯度消失问题。

+   **内存使用**：它需要将整个训练数据存储在 CPU/GPU 内存中。

# 小批量梯度下降

在批量梯度下降算法中，所有训练样本都用于计算单个改进步骤。该步骤将是最准确的，因为它考虑了所有可用的信息。然而，这种方法在实际应用中往往过于缓慢，建议实现更快的替代方案。

最常见的解决方案是称为[**小批量梯度下降**](https://machinelearningmastery.com/gentle-introduction-mini-batch-gradient-descent-configure-batch-size/)，它只使用**训练数据的小子集**来更新权重值。

考虑整个训练集 *X* 和相关标签 *Y*：

![](img/775b6cf970b2d797aac374f7015b1acb.png)

其中 *m* 表示训练样本的数量。

与其将整个批次喂给优化算法，不如仅处理训练集的一小部分。假设我们将子集 *X^t* 和 *Y^t* 喂给算法，每个子集包含 512 个训练样本：

![](img/bc8b776451f0b2634dcf04cb13cf92aa.png)

例如，如果总训练集包含 5,120,000 个点，则可以将其分成 10,000 个小批量，每个小批量包含 512 个样本。

对于每个小批量，我们执行经典的梯度下降操作：

+   **计算相对于小批量 t 的成本**，*J^t(W,b)*。

+   **执行反向传播**以计算 *J^t(W,b)* 相对于每个权重和偏差的梯度。

+   **更新参数**。

下图中的绿色线条显示了小批量梯度下降的典型优化路径。虽然批量梯度下降会沿着更直接的路径到达最优点，但由于每次迭代中数据集的限制，小批量梯度下降似乎会采取几步不必要的路径。

![](img/c9c4d7274702fbc3b4c2dfedc82d47ce.png)

图片由作者提供。

原因在于，小批量梯度下降在任何时间 *t* 只有一小部分训练集用于计算其决策。由于可用信息的限制，显然所走的路线不是最直接的。

然而，小批量梯度下降的显著优势在于每一步计算非常快速，因为算法只需评估数据的一小部分，而不是整个训练集。在我们的例子中，每一步只需要评估 512 个数据点，而不是 500 万。这也是为什么几乎没有真实应用需要大量数据时使用批量梯度下降的原因。

如图所示，使用小批量梯度下降算法无法保证迭代 *t+1* 的成本低于迭代 t 的成本，但如果问题定义良好，优化算法可以非常快速地达到接近最优点的区域。

小批量大小的选择代表了网络训练过程中的一个附加超参数。批量大小等于总样本数量 (*m*) 对应于批量梯度下降优化。相反，如果 *m=1*，我们正在执行随机梯度下降，每个训练样本就是一个小批量。

如果训练集很小，批量梯度下降可能是一个有效的选择，否则，像 32、64、128、256 和 512 这样的常见小批量大小通常被考虑。出于某种原因，等于 2 的幂的批量大小似乎表现更好。

虽然批量大小为 1 的随机梯度下降是一种选择，但它往往非常嘈杂，并且可能远离最小值。此外，考虑到每个样本的计算时间，随机梯度下降效率很低，因为它无法利用向量化的好处。

# 动量梯度下降

请记住，在批量和小批量梯度下降中，参数更新遵循一个定义的公式：

![](img/87fefcb663e3b7faa65b4eaa7be30835.png)

因此，在这个方程中，每一步优化的大小由学习率（一个固定的量）和在成本函数特定点计算出的梯度决定。

当梯度在成本函数的近似平坦区域计算时，它将非常小，从而导致相应的小梯度下降步长。考虑下图中点 A 和 B 的梯度差异。

![](img/807b45e443cab6256ff027001474723b.png)

图像由作者提供。

**动量梯度下降** 解决了这个问题。我们可以把动量梯度下降想象成一个沿着坡道滚下的保龄球，坡道的形状由成本函数定义。如果保龄球从坡道陡峭的部分开始下降，它的运动开始时较慢，但会很快获得速度和动量。由于其动量，即使在坡道的平坦区域，保龄球也能保持很高的速度。

这就是动量梯度下降的核心概念：算法会考虑以前的梯度，而不仅仅是迭代 *t* 时计算的梯度。类似于保龄球的类比，迭代 *t* 时计算的梯度定义了保龄球的加速度，而不是速度。

在每一步中，权重和偏置的速度都是使用前一速度和当前迭代的梯度计算的。

![](img/8a438ce7233a86faeebe73227e864497.png)

参数 *beta*，即动量，调节新速度值是根据当前斜率还是过去速度值来决定的。

最后，使用计算出的速度更新参数：

![](img/48791f6392f67a809a6c5c07ccae0868.png)

相较于迷你批量梯度下降，动量梯度下降在大多数应用中表现出更优越的性能。动量梯度下降相对于标准梯度下降的主要缺点是它需要额外的参数进行调整。然而，实践表明，*beta* 等于 0.9 的值效果很好。

# RMS Prop

考虑一个成本函数，其形状类似于**长形碗**，其中最小点位于最窄的部分。该函数的轮廓由下图中的水平线条描述。

![](img/c94d07c2aca361a7d27c0a0ee2e2f998.png)

图片来源：作者。

在起始点距离最小值较远的情况下，梯度下降（即使是带有动量的变体）开始时沿着最陡的斜坡前进，在上图中，这不是通向最小值的最佳路径。[**RMS Prop**](https://keras.io/api/optimizers/rmsprop)优化算法的关键思想是早期修正方向，并更迅速地瞄准全局最小值。

与动量梯度下降类似，RMS Prop 需要通过一个额外的超参数（称为衰减率）进行调整。通过实际经验，已经证明将衰减率设置为 0.9 是大多数问题的良好选择。

# Adam

[**Adam**](https://keras.io/api/optimizers/adam)及其变体可能是神经网络中最常用的优化算法。Adam，即**自适应动量估计**，源自动量梯度下降和 RMS Prop 的组合。

作为两种优化方法的混合，Adam 需要调整两个额外的超参数（除了学习率 *alpha*）。我们称它们为 *beta_1* 和 *beta_2*，它们分别是动量和 RMS Prop 中使用的超参数。与其他讨论的算法一样，*beta_1* 和 *beta_2* 也有有效的默认值选择：*beta_1* 通常设置为 0.9，*beta_2* 设置为 0.999。Adam 还包括一个 *epsilon* 参数，它作为平滑项，并且几乎总是设置为像 e-7 或 e-8 这样的小值。

在大多数情况下，Adam 优化算法优于上述所有方法。唯一的例外是一些非常简单的问题，在这些问题中，简单的方法效果更快。Adam 的效率确实有一个权衡：需要调整两个额外的参数。然而，在我看来，这是为其效率付出的微小代价。

## Nadam 和 AdaMax

值得提及的两个算法是作为广泛使用的 Adam 优化算法的修改版：**Nadam** 和 **AdaMax**。

[Nadam](https://keras.io/api/optimizers/Nadam)，即 Nesterov 加速自适应矩估计，通过将 Nesterov 加速梯度（NAG）融入其框架来增强 Adam。这意味着 Nadam 不仅受益于 Adam 的自适应学习率和动量，还受益于 NAG 组件，这是一种帮助算法更准确预测下一步的技术。尤其在高维空间中，Nadam 的收敛速度更快，效果更佳。

[AdaMax](https://keras.io/api/optimizers/adamax)则采用了略微不同的方法。虽然 Adam 根据梯度的一阶矩和二阶矩计算自适应学习率，但 AdaMax 仅关注梯度的最大范数。AdaMax 在处理稀疏梯度方面的简单性和高效性使其成为训练深度神经网络的一个有吸引力的选择，尤其是在涉及稀疏数据的任务中。

# 优化算法比较

为了实际测试和可视化每种讨论的优化算法的性能，我使用上述每种优化器训练了一个简单的深度神经网络。网络的任务是对 28x28 像素的图像中的时尚物品进行分类。数据集名为 [Fashion MNIST](https://github.com/zalandoresearch/fashion-mnist)（MIT 许可），由 70,000 张小型灰度服装图像组成。

我用来测试不同优化器的算法在下面的代码片段中展示，更详细的信息请参见下方的 GitHub 仓库。

```py
# Load data
(X_train, y_train), (X_test, y_test) = load_my_data()
# Print an example
#print_image(X_train, 42)

# Normalize the inputs
X_train = X_train / 255.
X_test = X_test / 255.

# Define the optimizers to test
my_optimizers = {"Mini-batch GD":tf.keras.optimizers.SGD(learning_rate = 0.001, momentum = 0.0),
                 "Momentum GD":tf.keras.optimizers.SGD(learning_rate = 0.001, momentum = 0.9),
                 "RMS Prop":tf.keras.optimizers.RMSprop(learning_rate = 0.001, rho = 0.9),
                 "Adam":tf.keras.optimizers.Adam(learning_rate = 0.001, beta_1 = 0.9, beta_2 = 0.999)
    }

histories = {}
for optimizer_name, optimizer in my_optimizers.items():
    # Define a neural network

    my_network = tf.keras.models.Sequential([
                tf.keras.layers.Flatten(input_shape=(28, 28)),                    
                tf.keras.layers.Dense(128, activation='relu'),
                tf.keras.layers.Dense(64, activation='relu'),
                tf.keras.layers.Dense(10, activation='softmax')
                ])
    # Compile the model
    my_network.compile(optimizer=optimizer,
                       loss='sparse_categorical_crossentropy', # since labels are more than 2 and not one-hot-encoded
                       metrics=['accuracy'])

    # Train the model
    print('Training the model with optimizer {}'.format(optimizer_name))
    histories[optimizer_name] = my_network.fit(X_train, y_train, epochs=50, validation_split=0.1, verbose=1)

# Plot learning curves
for optimizer_name, history in histories.items():
    loss = history.history['loss']
    epochs = range(1,len(loss)+1)
    plt.plot(epochs, loss, label=optimizer_name)
    plt.legend(loc="upper right")
    plt.xlabel("Epoch")
    plt.ylabel("Loss")
```

[## articles/NN-optimizer at main · andreoniriccardo/articles

### 通过在 GitHub 上创建账户来为 andreoniriccardo/articles 的开发做贡献。

github.com](https://github.com/andreoniriccardo/articles/tree/main/NN-optimizer?source=post_page-----d16d87ef15cb--------------------------------)

结果图表在这条折线图中可视化：

![](img/dd630ab1d73df29ca08fc4dd809994a4.png)

图片由作者提供。

我们可以立即看到动量梯度下降（黄色线）比标准梯度下降（蓝色线）快得多。相反，RMS Prop（绿色线）似乎获得了与动量梯度下降相似的结果。这可能是由于几个原因，如超参数调节不完全或神经网络过于简单。最后，Adam 优化器（红色线）在所有其他方法中明显优越。

# 学习率衰减

我希望在这篇文章中包含学习率衰减，因为尽管它不严格算作优化算法，但它是一种**加速网络学习过程**的强大技术。

![](img/aef64cc8a1598dbc30577bad03f3e05b.png)

图片来源：[unsplash.com](https://unsplash.com/photos/OyCl7Y4y0Bk)。

学习率衰减指的是**学习率**超参数*alpha*在训练周期中的**减少**。这种调整是必要的，因为在优化的初始阶段，算法可以接受更大的步伐，但当它接近最小点时，我们更愿意采取较小的步伐，以便它能在更接近最小点的区域内跳跃。

有几种方法可以在迭代中减少学习率。一种方法由以下公式描述：

![](img/cc1823be5f8dbec5cdcf155298ad11ed.png)

其中，衰减率是一个额外的调节参数。

另一种可能性是：

![](img/c21313ad29ebd06c945998b69722069f.png)![](img/ec8ea285de373dc3220d44e361a23551.png)

第一个叫做指数学习率衰减，而第二个中，参数*k*是一个常量。

最后，也可以应用离散学习率衰减，例如在每*t*次迭代后将其减半，或手动减少它。

![](img/c2ad4436b2a23cca5d7f8e308e0a1c17.png)

图片由作者提供。

# 结论

时间对每个数据科学从业者来说都是宝贵而有限的资源。因此，掌握加速学习算法训练的工具可以带来差异。

在这篇文章中，我们已经看到标准的梯度下降优化器是过时的工具，并且存在一些替代方法，它们在更短的时间内提供更好的解决方案。

选择适合特定机器学习应用的优化器并不总是容易的。这取决于任务，而且尚未有明确的共识关于哪一种优化器最好。然而，正如我们上面所看到的，Adam 优化器在大多数情况下是一个有效的选择，了解最受欢迎的优化器的工作原理是一个很好的起点。

如果你喜欢这个故事，请考虑关注我，以便接收到我即将发布的项目和文章的通知！

这是我过去的一些项目：

[社交网络分析与 NetworkX：温和的介绍](https://towardsdatascience.com/social-network-analysis-with-networkx-a-gentle-introduction-6123eddced3?source=post_page-----d16d87ef15cb--------------------------------)

### 了解像 Facebook 和 LinkedIn 这样的公司如何从网络中提取洞察。

[深度学习生成幻想名字：从零构建语言模型](https://towardsdatascience.com/social-network-analysis-with-networkx-a-gentle-introduction-6123eddced3?source=post_page-----d16d87ef15cb--------------------------------) 

### 语言模型能否发明独特的幻想角色名字？让我们从零开始构建它。

[使用深度学习生成幻想角色名称：从头构建语言模型](https://towardsdatascience.com/use-deep-learning-to-generate-fantasy-character-names-build-a-language-model-from-scratch-792b13629efa?source=post_page-----d16d87ef15cb--------------------------------) [](/support-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d?source=post_page-----d16d87ef15cb--------------------------------) [## 使用 Scikit-Learn 的支持向量机：友好的介绍

### 每个数据科学家都应该在工具箱中拥有 SVM。了解如何通过实践掌握这一多用途模型…

[支持向量机与 Scikit-Learn：友好的介绍](https://towardsdatascience.com/support-vector-machine-with-scikit-learn-a-friendly-introduction-a2969f2ff00d?source=post_page-----d16d87ef15cb--------------------------------)

# 参考文献

+   [梯度下降 — Wikipedia.org](https://en.wikipedia.org/wiki/Gradient_descent)

+   [深入学习 — Aston Zhang、Zachary C. Lipton、Mu Li 和 Alexander J. Smola](https://d2l.ai/)

+   [深入了解整流器：超越 ImageNet 分类的人类水平表现 — Kaiming He、Xiangyu Zhang、Shaoqing Ren、Jian Sun](https://arxiv.org/abs/1502.01852v1)

+   [改善深度神经网络：超参数调整、正则化和优化 — Andrew Ng](https://www.coursera.org/learn/deep-neural-network)

+   [理解深度前馈神经网络训练的难度 — Xavier Glorot、Yoshua Bengio](http://proceedings.mlr.press/v9/glorot10a/glorot10a.pdf)

+   [动手机器学习：使用 Scikit-Learn、Keras 和 TensorFlow（第 2 版） — Aurélien Géron](https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/)

+   [Keras Python 库](https://keras.io/)
