# 多层感知器的解释与说明

> 原文：[`towardsdatascience.com/multi-layer-perceptrons-8d76972afa2b`](https://towardsdatascience.com/multi-layer-perceptrons-8d76972afa2b)

## 理解神经网络的第一个完全功能模型

[](https://medium.com/@roiyeho?source=post_page-----8d76972afa2b--------------------------------)![Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----8d76972afa2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8d76972afa2b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d76972afa2b--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----8d76972afa2b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d76972afa2b--------------------------------) ·13 分钟阅读·2023 年 4 月 2 日

--

在 [上一篇文章](https://medium.com/@roiyeho/perceptrons-the-first-neural-network-model-8b3ee4513757) 中，我们讨论了感知器作为最早的神经网络模型之一。正如我们所见，单个感知器在计算能力上有限，因为它们只能解决线性可分的问题。

在本文中，我们将讨论多层感知器（MLP），这些网络由多个感知器层组成，比单层感知器强大得多。我们将探讨这些网络如何运作，以及如何利用它们解决复杂任务，例如图像分类。

# 定义与符号

**多层感知器**（MLP）是一个至少有三层的神经网络：输入层、隐藏层和输出层。每一层都处理其前一层的输出：

![](img/a1eb1fbb0ed55fbf175252ae54f14c61.png)

MLP 架构

我们将使用以下符号：

+   *aᵢˡ* 是层 *l* 中神经元 *i* 的激活值（输出）

+   *wᵢⱼˡ* 是从层 *l*-1 中神经元 *j* 到层 *l* 中神经元 *i* 的连接权重

+   *bᵢˡ* 是层 *l* 中神经元 *i* 的偏置项

输入层和输出层之间的中间层称为 **隐藏层**，因为它们在网络之外不可见（它们构成了网络的“内部大脑”）。

输入层通常不算作网络中的层数。例如，一个 3 层网络有一个输入层、两个隐藏层和一个输出层。

# 前向传播

前向传播是将输入数据逐层通过网络的过程，直到生成输出。

在前向传播阶段，神经元的激活值计算类似于单个感知器的激活值计算方式。

例如，让我们看一下第*l*层的神经元 *i*。该神经元的激活值是通过两个步骤计算得出的：

1.  我们首先计算神经元的净输入，作为其输入的加权和加上偏置：

![](img/c1bdf5f6f5c68e0d2e90012c8360fb73.png)

第 *l* 层的神经元 *i* 的净输入

2\. 我们现在对净输入应用激活函数以获得神经元的激活值：

![](img/cb4bcad164ee6ecf2463d814e5fd5155.png)

第 *l* 层神经元 *i* 的激活值

根据定义，输入层神经元的激活值等于当前呈现给网络的示例的特征值，即，

![](img/b5717c3d1ced500889945d444a0d6255.png)

输入神经元的激活值

其中 *m* 是数据集中的特征数量。

## 向量化形式

为了提高计算效率（特别是在使用像 NumPy 这样的数值库时），我们通常使用上述方程的向量化形式。

我们首先定义向量 **a***ˡ* 为包含第 *l* 层所有神经元激活值的向量，以及向量 **b***ˡ* 为包含第 *l* 层所有神经元偏置的向量。

我们还定义 *Wˡ* 为从第 *l* 层到第 *l* 层 - 1 的所有神经元的连接权重矩阵。例如，*W*¹₂₃ 是层 0（输入层）中神经元编号 2 与层 1（第一隐藏层）中神经元编号 3 之间连接的权重。

我们现在可以将前向传播方程写成向量形式。对于每一层 *l*，我们计算：

![](img/9ce5b15f01e65393f47a2048d9ebba24.png)

前向传播方程的向量化形式

# 解决 XOR 问题

多层感知器（MLP）相对于单层感知器的首次演示表明，它们能够解决 XOR 问题。XOR 问题是非线性可分的，因此单层感知器无法解决：

![](img/37e6380514099aaec8d2ef89be3a9642.png)

XOR 问题

然而，具有单层隐藏层的 MLP 可以轻松解决这个问题：

![](img/cf692a0df09ba84762cf196f2cc00473.png)

解决 XOR 问题的多层感知器（MLP）。偏置项写在节点内部。

让我们分析一下这个 MLP 是如何工作的。这个 MLP 除了两个输入神经元外，还有三个隐藏神经元和一个输出神经元。我们在这里假设所有的神经元都使用阶跃激活函数（即，对于所有非负输入，函数值为 1，而对于所有负输入，函数值为 0）。

顶层隐藏神经元仅与第一个输入 *x*₁ 连接，连接权重为 1，且有一个偏置 -1。因此，这个神经元仅在 *x*₁ = 1 时激活（此时其净输入为 1 × 1 + (-1) = 0，*f*(0) = 1，其中 *f* 是阶跃函数）。

中间隐藏神经元与两个输入相连，连接权重为 1，且有一个偏置 -2。因此，这个神经元仅在两个输入都为 1 时激活。

底部隐藏神经元仅与第二输入 *x*₂ 连接，连接权重为 1，且偏置为 -1。因此，这个神经元仅在 *x*₂ = 1 时被激活。

输出神经元与顶部和底部隐藏神经元的权重为 1，且与中间隐藏神经元的权重为 -2，偏置为 -1。因此，它仅在顶部或底部隐藏神经元激活时被激活，而在两者同时激活时不被激活。换句话说，它仅在 *x*₁ = 1 或 *x*₂ = 1 时被激活，而在两个输入都为 1 时不会被激活，这正是我们对 XOR 函数输出的预期。

例如，计算这个多层感知机（MLP）在输入 *x*₁ = 1 和 *x*₂ = 0 时的前向传播。此时隐藏神经元的激活情况是：

![](img/04c4f48e457481f6411dae3a8eb2ba51.png)

x1 = 1 和 x2 = 0 时隐藏神经元的激活情况

我们可以看到在这种情况下只有顶部的隐藏神经元被激活。

输出神经元的激活情况是：

![](img/414f961379e3a54e7d06cf06e737f52d.png)

MLP 在 x1 = 1 和 x2 = 0 时的输出

在这种情况下，输出神经元被激活，这正是我们对输入 *x*₁ = 1 和 *x*₂ = 0 时 XOR 输出的预期。

验证你是否理解 MLP 如何计算 XOR 函数的其他三个情况！

# MLP 构建练习

作为另一个例子，考虑以下数据集，其中包含来自三个不同类别的点：

![](img/7dfb7d4a25f24acdf00bcbaec13823d1.png)

构建一个 MLP，正确分类数据集中所有点。

*提示*：使用隐藏神经元来识别三个分类区域。

解决方案可以在本文底部找到。

# 通用逼近定理

关于 MLP 的一个显著事实是它们可以计算任何任意的函数（尽管网络中的每个神经元计算的是非常简单的函数，如阶跃函数）。

[通用逼近定理](https://en.wikipedia.org/wiki/Universal_approximation_theorem)指出，一个具有足够数量神经元的单隐层 MLP 可以任意精确地逼近任何连续的输入函数。具有两个隐层的 MLP 甚至可以逼近不连续的函数。这意味着即使是非常简单的网络架构也可以非常强大。

不幸的是，定理的证明是非构造性的，即它没有告诉我们如何构建一个网络来计算特定的函数，只是展示了这样的网络存在。

# MLP 中的学习：反向传播

尽管 MLP 已被证明在计算上非常强大，但很长一段时间内还不清楚如何在特定的数据集上训练它们。虽然单层感知器有一个简单的权重更新规则，但不清楚如何将此规则应用于隐藏层的权重，因为这些权重不会直接影响网络的输出（因此也不会直接影响训练损失）。

当 1986 年 Rumelhart 等人引入其突破性的**反向传播**算法用于训练 MLP 时，AI 社区花费了超过 30 年的时间才解决了这个问题。

反向传播的主要思想是首先计算网络误差函数相对于每个权重的梯度，然后使用梯度下降来最小化误差。之所以称之为反向传播，是因为我们利用**导数链式法则**将误差的梯度从输出层传播回输入层。

反向传播算法在 [这篇文章](https://medium.com/towards-data-science/backpropagation-step-by-step-derivation-99ac8fbdcc28) 中有详细解释。

# 激活函数

在单层感知器中，我们使用了步进函数或符号函数作为神经元的激活函数。这些函数的问题在于它们的梯度几乎为 0（因为它们在 *x* > 0 和 *x* < 0 时等于常数值）。这意味着我们无法在梯度下降中使用它们来找到网络的最小误差。

因此，在 MLP 中我们需要使用其他激活函数。这些函数应该既可微分又是非线性的（如果 MLP 中所有神经元使用线性激活函数，则 MLP 的行为类似于单层感知器）。

对于隐藏层，最常见的三种激活函数是：

1.  Sigmoid 函数

![](img/626faaeee07320671c208244f8fc9d07.png)

2\. 双曲正切函数

![](img/722972c5d254f95a20b77ace39781619.png)

3\. ReLU（修正线性单元）函数

![](img/6abf1e2e4d040e3f101274fac04c0b11.png)

输出层的激活函数取决于网络试图解决的问题：

1.  对于回归问题，我们使用恒等函数 *f*(*x*) = *x*。

1.  对于二分类问题，我们使用 Sigmoid 函数（如上所示）。

1.  对于多类分类问题，我们使用 softmax 函数，它将 *k* 个实数的向量转换为 *k* 种可能结果的概率分布：

![](img/55e040a35e031692c9b4ee9159a4cb44.png)

Softmax 函数

在这篇文章中深入解释了为什么我们在多类问题中使用 Softmax 函数：

[](/deep-dive-into-softmax-regression-62deea103cb8?source=post_page-----8d76972afa2b--------------------------------) ## 深入了解 Softmax 回归

### 理解 Softmax 回归背后的数学原理以及如何使用它来解决图像分类任务

towardsdatascience.com

# Scikit-Learn 中的 MLP

Scikit-Learn 提供了两个实现 MLP 的类，位于 sklearn.neural_network 模块中：

1.  [MLPClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html) 用于分类问题。

1.  [MLPRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPRegressor.html) 用于回归问题。

这些类中的重要超参数有：

+   *hidden_layer_sizes* — 定义每个隐藏层中神经元数量的元组。默认值是 (100,)，即一个包含 100 个神经元的隐藏层。对于许多问题，使用一两个隐藏层应该足够。对于更复杂的问题，你可以逐渐增加隐藏层的数量，直到网络开始过拟合训练集。

+   *activation* — 在隐藏层中使用的激活函数。选项有 ‘identity’，‘logistic’，‘tanh’，和 ‘relu’（默认）。

+   *solver* — 用于权重优化的求解器。默认值是 ‘adam’，它在大多数数据集上表现良好。各种优化器的行为将在未来的文章中解释。

+   *alpha* — L2 正则化系数（默认为 0.0001）

+   *batch_size* — 用于训练的迷你批次的大小（默认为 200）。

+   *learning_rate* — 权重更新的学习率调度（默认为 ‘constant’）。

+   *learning_rate_init* — 使用的初始学习率（默认为 0.001）。

+   *early_stopping* — 是否在验证得分没有改善时停止训练（默认为 False）。

+   *validation_fraction* — 从训练集中留出用于验证的比例（默认为 0.1）。

我们通常使用网格搜索和交叉验证来调整这些超参数。

# 在 MNIST 上训练 MLP

例如，让我们在 [MNIST 数据集](https://en.wikipedia.org/wiki/MNIST_database) 上训练一个 MLP，这是一个广泛用于图像分类任务的数据集。

数据集包含 60,000 张训练图像和 10,000 张测试图像，每张图像为 28 × 28 像素，通常用一个包含 784 个数字的向量表示，范围在 [0, 255] 之间。任务是将这些图像分类到十个数字（0-9）之一。

我们首先使用 [fetch_openml()](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.fetch_openml.html) 函数获取 MNIST 数据集：

```py
from sklearn.datasets import fetch_openml

X, y = fetch_openml('mnist_784', return_X_y=True, as_frame=False)
```

*as_frame* 参数指定我们希望以 NumPy 数组而不是 DataFrame 的形式获取数据和标签（这个参数的默认值在 Scikit-Learn 0.24 中从 False 改为 ‘auto’）。

让我们检查 *X* 的形状：

```py
print(X.shape)
```

```py
(70000, 784)
```

也就是说，*X* 由 70,000 个 784 像素的平面向量组成。

让我们显示数据集中的前 50 个数字：

```py
fig, axes = plt.subplots(5, 10, figsize=(10, 5))
i = 0
for ax in axes.flat:
    ax.imshow(X[i].reshape(28, 28), cmap='binary')
    ax.axis('off')    
    i += 1
```

![](img/8cf5f43a5f3f214df8bf8277c60c3530.png)

MNIST 数据集中的前 50 个数字

让我们检查每个数字的样本数量：

```py
np.unique(y, return_counts=True)
```

```py
(array(['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'], dtype=object),
 array([6903, 7877, 6990, 7141, 6824, 6313, 6876, 7293, 6825, 6958],
       dtype=int64))
```

数据集在 10 个类别之间比较均衡。

我们现在将输入缩放到 [0, 1] 范围内，而不是 [0, 255]：

```py
X = X / 255
```

特征缩放使神经网络的训练更快，并防止它们陷入局部最优解。

我们现在将数据分为训练集和测试集。请注意，MNIST 中前 60,000 张图像已被指定用于训练，因此我们可以通过简单的切片操作来进行拆分：

```py
train_size = 60000
X_train, y_train = X[:train_size], y[:train_size]
X_test, y_test = X[train_size:], y[train_size:]
```

我们现在创建一个具有 300 个神经元的单隐藏层 MLP 分类器。我们将保持所有其他超参数的默认值，除了*early_stopping*，我们将其更改为 True。我们还将设置 verbose=True，以便跟踪训练进度：

```py
from sklearn.neural_network import MLPClassifier

mlp = MLPClassifier(hidden_layer_sizes=(300,), early_stopping=True, 
                    verbose=True)
```

让我们将分类器拟合到训练集上：

```py
mlp.fit(X_train, y_train)
```

训练期间得到的输出是：

```py
Iteration 1, loss = 0.35415292
Validation score: 0.950167
Iteration 2, loss = 0.15504686
Validation score: 0.964833
Iteration 3, loss = 0.10840875
Validation score: 0.969833
Iteration 4, loss = 0.08041958
Validation score: 0.972333
Iteration 5, loss = 0.06253450
Validation score: 0.973167
...
Iteration 31, loss = 0.00285821
Validation score: 0.980500
Validation score did not improve more than tol=0.000100 for 10 consecutive epochs. Stopping.
```

训练在 31 次迭代后停止，因为在前 10 次迭代中验证分数没有改善。

让我们检查一下 MLP 在训练集和测试集上的准确性：

```py
print('Accuracy on training set:', mlp.score(X_train, y_train))
print('Accuracy on test set:', mlp.score(X_test, y_test))
```

```py
Accuracy on training set: 0.998
Accuracy on test set: 0.9795
```

这些是很好的结果，但具有更复杂架构的网络，如卷积神经网络（CNNs），可以在这个数据集上获得更好的结果（在测试集上的准确率高达 99.91%！）。你可以在[这里](https://paperswithcode.com/sota/image-classification-on-mnist)找到关于 MNIST 的最先进结果及相关论文的链接。

为了更好地理解我们模型的错误，让我们显示其混淆矩阵：

```py
from sklearn.metrics import confusion_matrix, ConfusionMatrixDisplay

y_pred = mlp.predict(X_test)
cm = confusion_matrix(y_test, y_pred)
disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=mlp.classes_)
disp.plot(cmap='Blues')
```

![](img/565ee83303ba8410af3687aa5c8e7f26.png)

测试集上的混淆矩阵

我们可以看到，模型的主要混淆发生在数字 4⇔9、7⇔9 和 2⇔8 之间。这是有道理的，因为这些数字在手写时经常彼此相似。为了帮助我们的模型区分这些数字，我们可以增加这些数字的样本（例如，通过数据增强）或从图像中提取额外的特征（例如，数字中的闭合环数）。

## 可视化 MLP 权重

尽管神经网络通常被认为是“黑箱”模型，但在由一到两个隐藏层组成的简单网络中，我们可以可视化学到的权重，并偶尔对这些网络如何在内部工作有所了解。

例如，我们可以绘制 MLP 分类器的输入层和隐藏层之间的权重。权重矩阵的形状为 (784, 300)，并存储在一个名为 mlp.coefs_[0] 的变量中：

```py
print(mlp.coefs_[0].shape)
```

```py
(784, 300)
```

这个矩阵的 *i* 列表示输入到隐藏神经元 *i* 的权重。我们可以将这一列显示为一个 28 × 28 像素的图像，以检查哪些输入神经元对该神经元的激活有较强的影响。

以下图展示了前 20 个隐藏神经元的权重：

```py
fig, axes = plt.subplots(4, 5)

for coef, ax in zip(mlp.coefs_[0].T, axes.flat):
    im = ax.imshow(coef.reshape(28, 28), cmap='gray')
    ax.axis('off')

fig.colorbar(im, ax=axes.flat)
```

![](img/ec1a1c851e805c9f1ec63ced661523f7.png)

前 20 个隐藏神经元的权重

我们可以看到每个隐藏神经元关注图像的不同部分。

# 其他库中的 MLP

尽管 Scikit-Learn 中的 MLP 分类器易于使用，但在实际应用中，你更可能使用如 TensorFlow 或 PyTorch 等深度学习库来构建 MLP。这些库可以利用更快的 GPU 处理速度，还提供许多附加选项，如额外的激活函数和优化器。你可以在[这篇文章](https://medium.com/@roiyeho/pytorch-2-0-or-tensorflow-2-10-which-one-is-better-52669cec994)中找到如何使用这些库的示例。

# MLP 构建练习的解决方案

以下 MLP 正确分类了数据集中的所有点：

![](img/78fe6af3673e411edf25fde4e044d6bc.png)

MLP 用于解决分类问题。偏置项被写在节点内部。

解释：

左侧隐藏神经元只有在*x*₁ ≤ 3 时才会激活，中间隐藏神经元只有在*x*₂ ≥ 4 时才会激活，右侧隐藏神经元只有在*x*₂ ≤ 0 时才会激活。

左侧输出神经元对左侧和中间隐藏神经元执行 OR 操作，因此只有在*x*₁ ≤ 3 OR *x*₂ ≥ 4 时才会激活，即只有当点是蓝色时。

中间输出神经元对所有隐藏神经元执行 NOR（非 OR）操作，因此只有在 NOT（*x*₁ ≤ 3 OR *x*₂ ≥ 4 OR *x*₂ ≤ 0）时才会激活。换句话说，只有当*x*₁ > 3 AND 0 < *x*₂ < 4 时，它才会激活，即只有当点是红色时。

只有当右侧隐藏神经元激活时，右侧输出神经元才会激活，即只有当*x*₂ ≤ 0 时，才会发生这种情况，这仅对紫色点有效。

# 最终说明

你可以在我的 GitHub 上找到本文的代码示例：[`github.com/roiyeho/medium/tree/main/mlp`](https://github.com/roiyeho/medium/tree/main/mlp)

除非另有说明，否则所有图片均由作者提供。

MNIST 数据集信息：

+   **引用：** 邓丽君，2012。用于机器学习研究的手写数字图像的 MNIST 数据库。*IEEE 信号处理杂志*，29(6)，第 141–142 页。

+   **许可证：** Yann LeCun 和 Corinna Cortes 拥有 MNIST 数据集的版权，该数据集根据*知识共享署名-相同方式共享 4.0 国际许可证*（[CC BY-SA](https://creativecommons.org/licenses/by-sa/4.0/)）提供。

感谢阅读！
