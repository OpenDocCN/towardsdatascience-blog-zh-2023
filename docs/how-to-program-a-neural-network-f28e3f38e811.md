# 如何编程一个神经网络

> 原文：[`towardsdatascience.com/how-to-program-a-neural-network-f28e3f38e811`](https://towardsdatascience.com/how-to-program-a-neural-network-f28e3f38e811)

## 从头实现神经网络的逐步指南

[](https://medium.com/@callum.bruce1?source=post_page-----f28e3f38e811--------------------------------)![Callum Bruce](https://medium.com/@callum.bruce1?source=post_page-----f28e3f38e811--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f28e3f38e811--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f28e3f38e811--------------------------------) [Callum Bruce](https://medium.com/@callum.bruce1?source=post_page-----f28e3f38e811--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f28e3f38e811--------------------------------) ·阅读时间 14 分钟·2023 年 9 月 23 日

--

![](img/79509eca917d69c1d6ca7ef6178adf3d.png)

一个有三个隐藏层的神经网络

在本文中，我们将从头开始构建一个神经网络，并使用它来分类手写数字。

为什么要重新发明轮子/神经网络，我听到你说？难道我不能直接使用我最喜欢的机器学习框架来解决问题吗？是的，你可以使用许多现成的框架来构建神经网络（比如 Keras、PyTorch 和 TensorFlow）。使用这些框架的问题在于，它们让我们很容易将神经网络当作黑箱处理。

这并不总是坏事。我们通常需要这种程度的抽象，以便能够处理手头的问题，但如果我们要在工作中使用神经网络，仍然应该努力至少对其内部运作有一个基本了解。

从头开始构建神经网络在我看来是培养深刻理解其工作原理的最佳方式。

到本文结束时，你将了解前馈和反向传播算法，激活函数是什么，epoch 和 batch 之间的区别是什么，以及如何训练神经网络。我们将通过训练一个神经网络来识别手写数字来完成本例。

本文中使用的所有代码可以在 [GitHub](https://github.com/c-bruce/artificial_neural_network) [1] 上找到。

# 什么是神经网络？

神经网络，或人工神经网络，是一种机器学习算法。它们是许多深度学习和人工智能系统的核心，例如计算机视觉、预测和语音识别。

人工神经网络的结构有时与大脑中的生物神经网络的结构进行比较。我总是建议对这种比较保持谨慎。确实，人工神经网络看起来有点像生物神经网络，但将它们与像人脑这样复杂的东西进行比较是一个很大的飞跃。

神经网络由几层神经元组成。每一层神经元的激活基于前一层的激活、连接前一层与当前层的权重集合，以及施加在当前层神经元上的偏置集合。

![](img/f38cca749159b2f7552d2330aa156b09.png)

包含两个隐藏层的神经网络的一般结构。神经元根据其激活程度着色（激活量越大，神经元越暗）。正权重用红色表示，负权重用蓝色表示。线宽表示权重大小。

第一层是输入层。输入层的激活来自神经网络的输入。最后一层是输出层。输出层的激活是神经网络的输出。中间的层称为隐藏层。

神经网络是对函数的广义近似。像其他任何函数一样，当我们给它一个输入时，它会返回一个输出。

神经网络的新颖之处在于*如何*从输入到输出。这个过程由网络权重和偏置如何影响神经元激活以及这些激活如何在网络中传播，最终到达输出层来驱动。神经网络使用前馈算法将输入转换为输出。

为了使神经网络提供有用的输出，我们必须首先对其进行训练。当我们训练神经网络时，我们所做的就是通过反向传播和梯度下降迭代调整权重和偏置，以提高输出的准确性。我们计算需要将权重和偏置向哪个方向和调整多少。

# 前馈算法

前馈算法将我们的神经网络输入转化为有意义的输出。顾名思义，该算法将信息“向前传递”从一层到下一层。

为了理解它是如何实现的，让我们先放大来看信息是如何从一层传递到下一层的一个神经元的。

![](img/abb01642c9f7285b915f83047f7374c3.png)

连接层 0 中神经元与层 1 中第一个神经元的权重

第二层中第一个神经元的激活*a*₀⁽¹⁾是通过对前一层的激活进行加权求和，再加上偏置，并通过激活函数*σ*(*x*)来计算的：

计算*a*₀⁽¹⁾的方程

带圆括号的上标表示层索引，从 0 开始，表示输入层。激活（*a*）和偏置（*b*）下标表示神经元索引。权重（*w*）下标中的前两个数字表示权重连接的神经元的索引（当前层中的）和从（前一层中的）索引。

激活函数决定一个神经元是否应根据接收到的输入而被激活。常见的激活函数包括 sigmoid、tanh、修正线性单元（ReLU）和 softmax。为了简单起见，在我们的实现中，我们将始终使用 sigmoid 激活函数。

![](img/644584ee1f184d127aec522e9821bccb.png)

Sigmoid、tanh 和 ReLU 激活函数

我们用来计算 *a*₀⁽¹⁾ 的方程可以向量化，以便我们可以计算第二层中的所有激活值：

计算 ***a***⁽¹⁾ 的向量化方程

现在我们有了第二层的神经元激活值 ***a***⁽¹⁾，我们可以使用相同的计算来找到 ***a***⁽²⁾，然后是 ***a***⁽³⁾，以此类推……

让我们看看如何在 Python 中实现这一点：

```py
import numpy as np
import math

class Network:
    def __init__(self, layers):
        self.layers = layers
        self.activations = self.__init_activations_zero()
        self.weights = self.__init_weights_random()
        self.biases = self.__init_biases_zero()

    def __init_activations_zero(self):
        activations = []
        for layer in self.layers:
            activations.append(np.zeros(layer))
        return activations

    def __init_weights_random(self):
        weights = []
        for i in range(0, len(self.layers) - 1):
            weights.append(np.random.uniform(-1, 1, (self.layers[i+1], self.layers[i])))
        return weights

    def __init_biases_zero(self):
        biases = []
        for i in range(1, len(self.layers)):
            biases.append(np.zeros(self.layers[i]))
        return biases

    def sigmoid(self, x):
        return 1 / (1 + np.exp(-x))

    def feedforward(self, input_layer):
        self.activations[0] = input_layer
        for i in range(0, len(self.layers) - 1):
            self.activations[i+1] = self.sigmoid(np.dot(self.weights[i], self.activations[i]) + self.biases[i])
```

`Network` 类包含有关我们神经网络的所有信息。我们通过传递一个整数列表来初始化它，该列表与每层的神经元数量相关。例如，`network = Network([10, 3, 3, 2])` 将创建一个输入层有十个神经元、两个隐藏层每个包含三个神经元，以及一个输出层有两个神经元的网络。

`__init_*` 方法初始化激活值、权重和偏置。激活值和偏置最初都是零。权重被赋予一个在 -1 和 1 之间的随机值。

`feedforward` 方法循环遍历各层，计算每个后续层的激活值。

下面是一个示例，使用`feedforward`计算我们 `[10, 3, 3, 2]` 网络在给定随机输入时的输出。通过打印最终层的激活值来检查输出：

```py
network = Network([10, 3, 3, 2])

input_layer = np.random.uniform(-1, 1, (10))

network.feedforward(input_layer)

print(network.activations[-1])

...

[0.29059666 0.5261155 ]
```

就这样！我们已经成功实现了前向传播算法！让我们将注意力转向反向传播。

# 反向传播算法

反向传播算法是神经网络从错误中学习的过程。

在上述前向传播算法的实现中，我们将网络权重初始化为 -1 和 1 之间的随机数，并将所有偏置设置为 0。使用这种初始设置，网络对任何给定输入生成的输出本质上是随机的。

我们需要一种方法来更新权重和偏置，使网络的输出变得更有意义。为此，我们使用梯度下降：

梯度下降更新步骤

其中 ***a****ₙ* 是一个输入参数的向量。下标 *n* 表示迭代次数。*f(****a****ₙ)* 是一个多变量代价函数，∇*f(****a****)* 是该代价函数的梯度。𝛾 是学习率，它决定了每次迭代中 ***a****ₙ* 应调整的幅度。我之前写过一篇关于梯度下降的 [文章](https://medium.com/towards-data-science/pid-controller-optimization-a-gradient-descent-approach-58876e14eef2) [2]，其中详细讨论了梯度下降。

[](/pid-controller-optimization-a-gradient-descent-approach-58876e14eef2?source=post_page-----f28e3f38e811--------------------------------) ## PID 控制器优化：一种梯度下降方法

### 使用机器学习解决工程优化问题

towardsdatascience.com

在神经网络的情况下，***a****ₙ* 包含了所有网络的权重和偏置，*f(****a****ₙ)* 是网络代价（注意 *f(****a****ₙ)* ≡ *C*）。这里我们使用 L2-范数代价函数来定义网络代价，该函数是根据期望网络输出 *ŷ* 和实际网络输出 *y* 计算的。*ŷ* 和 *y* 都是包含 *n* 个值的向量，其中 *n* 是输出层中的神经元数量。

L2-范数代价函数

现在我们知道如何计算代价函数，但还不知道如何计算代价函数的梯度。

计算代价函数的梯度是反向传播的核心。代价函数的梯度告诉我们需要将网络中的权重和偏置向哪个方向以及调整多少，以提高输出的准确性。

代价函数的梯度

为了找到这些偏导数，神经网络采用了 [链式法则](https://en.wikipedia.org/wiki/Chain_rule)。这段高中微积分的内容是神经网络工作的关键。

为了演示链式法则在反向传播算法中的应用，我们将考虑一个每层包含一个神经元的网络：

![](img/da2a39beb27b374c280ef8644e3ca660.png)

每层包含一个神经元的神经网络

我引入了一个简化版本的符号表示，因为在这个例子中我们不需要为每层中的神经元编号。下面，我还引入了一个新变量 *z*，它封装了激活函数的输入。

我们从输出层 *L* 开始反向传播，并迭代地向后通过各层。

对于输出层，我们有：

单层神经元网络输出层的 C、a⁽*ᴸ*⁾ 和 z⁽*ᴸ*⁾

现在我们已经为输出层定义了 *C*、*a*⁽*ᴸ*⁾ 和 *z*⁽*ᴸ*⁾，我们可以计算它们的导数并应用链式法则来找到 ∂*C/*∂*w*⁽*ᴸ*⁾：

应用链式法则来计算 ∂*C/*∂w⁽*ᴸ*⁾

对于 ∂*C/*∂*b*⁽*ᴸ*⁾ 也是类似的：

应用链式法则来计算 ∂*C/*∂*b*⁽*ᴸ*⁾

和 ∂*C/*∂a⁽*ᴸ*⁻¹⁾：

应用链式法则计算 ∂*C/*∂a⁽*ᴸ*⁻¹⁾

现在我们有了 ∂*C/*∂a⁽*ᴸ*⁻¹⁾ 的表达式，我们可以反向迭代通过网络找到成本函数对之前的权重和偏置的敏感性：

成本函数对 w⁽*ᴸ*⁻¹⁾ 和 b⁽*ᴸ*⁻¹⁾ 的敏感性

我们为计算这种简化的每层一个神经元网络中对权重和偏置的敏感性定义的方程，在每层有多个神经元时基本保持不变。

变化的是关于 *L-1*ᵗʰ 层激活值的成本函数的导数。这是因为成本函数通过网络的多个路径受到这些激活值的影响。

我们将成本函数对 *L-1*ᵗʰ 层激活值的导数定义为：

成本函数关于 L-1ᵗʰ 层中第 kᵗʰ 激活的导数

其中下标 *j* 和 *k* 分别表示在 *L*ᵗʰ 和 *L-1*ᵗʰ 层的激活值。

反向传播算法在 `Network` 类中的 `backpropagation` 方法中实现：

```py
def backpropagation(self, expected_output):
    # Calculate dcost_dactivations for the output layer
    dcost_dactivations = 2 * (self.activations[-1] - expected_output)

    # Loop backward through the layers to calculate dcost_dweights and dcost_dbiases
    for i in range(-1, -len(self.layers), -1):
        dactivations_dz = self.dsigmoid(np.dot(self.weights[i], self.activations[i-1]) + self.biases[i]) # Sigmoid output layer

        dz_dweights = self.activations[i-1]
        dz_dbiases = 1

        self.dcost_dweights[i] += dz_dweights[np.newaxis,:] * (dactivations_dz * dcost_dactivations)[:,np.newaxis]
        self.dcost_dbiases[i] += dz_dbiases * dactivations_dz * dcost_dactivations

        # Calculate dcost_dactivations for hidden layer
        dz_dactivations = self.weights[i]
        dcost_dactivations = np.sum(dz_dactivations * (dactivations_dz * dcost_dactivations)[:,np.newaxis], axis=0)
```

注意，这个方法进行了矢量化处理，以适应每层多个神经元。dcost_dweights 和 dcost_dbiases 存储在与之前定义的权重和偏置数组相同形状的数组中。这使得使用这些偏导数进行梯度下降变得非常简单。我还认为这使得代码更具可读性。

当我们向后遍历网络时，我们对每一层应用链式法则，并使用本节中介绍的方程计算成本函数对每层权重和偏置的敏感性。

# 训练一个神经网络来分类手写数字

实现了前向传播和反向传播算法之后，是时候将所有内容整合起来，训练一个用于识别手写数字的神经网络了。

为此，我们需要一个标注了相应值的手写数字数据集。自己生成这个数据集将会非常费力。幸运的是，已经存在可以用于这个数字识别问题的数据库。我们将使用修改版国家标准与技术研究所（MNIST）数据库* [3]，这是一个大型的标注手写数字数据库，用于训练我们的神经网络。

![](img/51074ebf5befa172c202becf4f5a722c.png)

从 MNIST 数据库中随机选择的样本

MNIST 数据库包含 70000 个标注的灰度图像，大小为 28 x 28 像素（总共 784）。数据库中的每个标注图像称为 *样本*。MNIST 数据库被划分为 *训练* 和 *测试* 子集，其中包含 60000 和 10000 个样本，分别用于训练和测试。

正如它们的名字所示，训练子集用于训练网络，测试子集用于测试网络的准确性。这样我们可以使用网络从未见过的样本来测试其准确性。

接下来，我们将训练子集分割成*批次*。在这个例子中，我决定每个批次包含 100 个样本。总共有 600 个批次。我们将训练子集分成批次的原因是我们不会在每个样本后更新网络的权重和偏置。相反，我们是在每个批次后更新的。这样，当我们应用梯度下降时，我们使用的是基于一个批次所有样本计算的平均梯度，而不是基于单个样本的梯度来调整权重和偏置。

一个*周期*包含训练子集中的所有批次。一个周期会遍历所有这些批次。选择用一个周期训练我们的网络意味着网络只会“看到”训练子集中的每个样本一次。增加周期数意味着网络将对每个样本进行多次训练，从而“看到”每个样本多次。

![](img/d39aaaa868b459bc69eef9ab96fec460.png)

训练工作流程图

下方显示了`Network`类的完整定义，包括在训练工作流程中使用的所有方法。`train_network`方法负责协调训练工作流程。

```py
import numpy as np

class Network:
    def __init__(self, layers, learning_rate):
        self.layers = layers
        self.learning_rate = learning_rate
        self.activations = self.__init_activations_zero()
        self.weights = self.__init_weights_random()
        self.biases = self.__init_biases_zero()
        self.G_weights = self.__init_G_weights()
        self.G_biases = self.__init_G_biases()
        self.dcost_dweights = self.__init_weights_zero()
        self.dcost_dbiases = self.__init_biases_zero()
        self.cost = 0
        self.costs = []

    def __init_activations_zero(self):
        activations = []
        for layer in self.layers:
            activations.append(np.zeros(layer))
        return activations

    def __init_weights_random(self):
        weights = []
        for i in range(0, len(self.layers) - 1):
            weights.append(np.random.uniform(-1, 1, (self.layers[i+1], self.layers[i])))
        return weights

    def __init_weights_zero(self):
        weights = []
        for i in range(0, len(self.layers) - 1):
            weights.append(np.zeros((self.layers[i+1], self.layers[i])))
        return weights

    def __init_biases_zero(self):
        biases = []
        for i in range(1, len(self.layers)):
            biases.append(np.zeros(self.layers[i]))
        return biases

    def __init_G_weights(self):
        G_weights = []
        for i in range(0, len(self.layers) - 1):
            G_weights.append(np.zeros([len(self.weights[i]), len(self.weights[i][0]), len(self.weights[i][0])]))
        return G_weights

    def __init_G_biases(self):
        G_biases = []
        for i in range(0, len(self.layers) - 1):
            G_biases.append(np.zeros([len(self.biases[i]), len(self.biases[i])]))
        return G_biases

    def sigmoid(self, x):
        return 1 / (1 + np.exp(-x))

    def dsigmoid(self, x):
        sig = self.sigmoid(x)
        return sig * (1 - sig)

    def calculate_cost(self, expected_output):
        cost = np.sum((self.activations[-1] - expected_output)**2) # L2 cost function
        return cost

    def feedforward(self, input_layer):
        self.activations[0] = input_layer
        for i in range(0, len(self.layers) - 1):
            self.activations[i+1] = self.sigmoid(np.dot(self.weights[i], self.activations[i]) + self.biases[i])

    def backpropagation(self, expected_output):
        # Calculate dcost_dactivations for the output layer
        dcost_dactivations = 2 * (self.activations[-1] - expected_output)

        # Loop backward through the layers to calculate dcost_dweights and dcost_dbiases
        for i in range(-1, -len(self.layers), -1):
            dactivations_dz = self.dsigmoid(np.dot(self.weights[i], self.activations[i-1]) + self.biases[i]) # Sigmoid output layer

            dz_dweights = self.activations[i-1]
            dz_dbiases = 1

            self.dcost_dweights[i] += dz_dweights[np.newaxis,:] * (dactivations_dz * dcost_dactivations)[:,np.newaxis]
            self.dcost_dbiases[i] += dz_dbiases * dactivations_dz * dcost_dactivations

            # Calculate dcost_dactivations for hidden layer
            dz_dactivations = self.weights[i]
            dcost_dactivations = np.sum(dz_dactivations * (dactivations_dz * dcost_dactivations)[:,np.newaxis], axis=0)

    def average_gradients(self, n):
        # Calculate the average gradients for a batch containing n samples
        for i in range(0, len(self.layers) - 1):
            self.dcost_dweights[i] = self.dcost_dweights[i] / n
            self.dcost_dbiases[i] = self.dcost_dbiases[i] / n

    def reset_gradients(self):
        # Reset gradients before starting a new batch
        self.dcost_dweights = self.__init_weights_zero()
        self.dcost_dbiases = self.__init_biases_zero()

    def reset_cost(self):
        self.cost = 0

    def update_G(self):
        for i in range(0, len(self.layers) - 1):
            self.G_biases[i] += np.outer(self.dcost_dbiases[i], self.dcost_dbiases[i].T)
            for j in range(0, len(self.weights[i])):
                self.G_weights[i][j] += np.outer(self.dcost_dweights[i][j], self.dcost_dweights[i][j].T)

    def update_weights_and_biases(self):
        # Perform gradient descent step to update weights and biases
        # Vanilla Gradient Descent
        # for i in range(0, len(self.layers) - 1):
        #     self.weights[i] -= (self.learning_rate * self.dcost_dweights[i])
        #     self.biases[i] -= (self.learning_rate * self.dcost_dbiases[i])

        # AdaGrad Gradient Desecent
        self.update_G()
        for i in range(0, len(self.layers) - 1):
            self.biases[i] -= (self.learning_rate * (np.diag(self.G_biases[i]) + 0.00000001)**(-0.5)) * self.dcost_dbiases[i]
            for j in range(0, len(self.weights[i])):
                self.weights[i][j] -= (self.learning_rate * (np.diag(self.G_weights[i][j]) + 0.00000001)**(-0.5)) * self.dcost_dweights[i][j]

    def process_batch(self, batch):
        for sample in batch:
            self.feedforward(sample['input_layer'])
            self.backpropagation(sample['expected_output'])
            self.cost += self.calculate_cost(sample['expected_output'])

    def train_network(self, n_epochs, batches):
        for epoch in range(0, n_epochs):
            print(f"Epoch: {epoch}\n")
            for batch in batches:
                self.process_batch(batch)
                self.costs.append(self.cost / len(batch))
                self.reset_cost()
                self.average_gradients(len(batch))
                self.update_weights_and_biases()
                self.reset_gradients()
                print(f"Cost: {self.costs[-1]}")
```

注意，我们使用 AdaGrad 梯度下降算法来更新网络的权重和偏置。Adagrad 比传统梯度下降算法复杂一些，但在这个应用中表现更好。

接下来，我们需要定义网络的形状。每个样本中的 784 个像素值构成输入层的激活，因此输入层的大小设置为 784。由于我们正在对 0 到 9 的数字进行分类，我们也知道输出层必须包含 10 个神经元。对于隐藏层，我发现两个每层 32 个神经元的隐藏层对这个问题效果很好。

总的来说，这个网络中有 26432 个权重和 74 个偏置。这意味着在训练网络时，我们是在一个 26506 维的参数空间中进行优化！

不要被这项优化任务的规模吓倒。我们已经通过实现前馈和反向传播算法以及定义训练工作流程完成了艰巨的工作。

在训练网络之前，需要对训练数据进行一些准备工作，以将其分割成批次。然后，我们可以调用`train_network`来训练网络。最后，一旦网络训练完成，我们通过检查网络对测试子集的输出，来计算网络准确率，以查看网络正确分类了多少样本。

```py
import numpy as np
import matplotlib.pyplot as plt
from keras.datasets import mnist
from network import Network

def calculate_accuracy(network, x, y):
    # Calculate network accuracy
    correct = 0
    for i in range(0, len(x)):
        network.feedforward(x[i].flatten() / 255.0)
        if np.where(network.activations[-1] == max(network.activations[-1]))[0][0] == y[i]:
            correct += 1
    print(f"Accuracy: {correct / len(x)}")

# Prepare training data
(train_X, train_y), (test_X, test_y) = mnist.load_data()

# Define n_epochs and set up batches
n_epochs = 5
n_batches = 600
batches = []
input_layer = np.array_split(train_X, n_batches)
expected_output = np.array_split(np.eye(10)[train_y], n_batches)
for i in range(0, n_batches):
    batch = []
    for j in range(0, len(input_layer[i])):
        batch.append({'input_layer' : input_layer[i][j].flatten() / 255.0, 'expected_output' : expected_output[i][j]})
    batches.append(batch)

# Setup and train network
network = Network([784,32,32,10], 0.1)
network.train_network(n_epochs, batches)

# Calculate accuracy of the network
calculate_accuracy(network, test_X, test_y)

...

Accuracy: 0.942
```

训练完成后，网络的准确率为 94.2%。对于一个从零开始构建的神经网络来说，这并不算差！

# 总结

在这篇文章中，我展示了如何使用 Python 从零开始构建一个简单的神经网络。

我们详细讲解了前馈和反向传播算法的实现，介绍了训练工作流程，并用 26432 个权重和 74 个偏置训练了一个神经网络来识别 MNIST 数据库中的手写数字，达到了 94.2%的网络准确率。

通过改进我们的实现可以获得更好的准确性。例如，使用 ReLU 激活函数用于隐藏层，softmax 用于输出层，已被证明能提高网络的准确性 [4]。

类似地，我们可以选择不同形式的梯度下降来调整权重和偏差，这可能使我们在 26506 维参数空间中找到更优的最小值。

将每个样本展平成一维数组的过程也会丢弃大量重要信息。更先进的卷积神经网络保留了图像中邻近像素的信息，通常比这里实现的基本网络类型表现更好。

当我开始写这篇文章时，我的目标是制作一个简明的资源，让刚接触神经网络的人能够阅读并获得对其工作原理的基本理解。我希望我达到了这个目标，并在某种程度上鼓励你继续学习这个极具趣味的主题。

这篇文章是否帮助你更深入地理解了神经网络的工作原理？请在评论中告诉我。

> **喜欢这篇文章吗？**
> 
> 关注 和 订阅 获取更多类似内容 — 与你的网络分享 — 尝试开发你自己的神经网络或尝试更先进的卷积神经网络。

*除非另有说明，所有图片均由作者提供。*

*Yann LeCun 和 Corinna Cortes 拥有 MNIST 数据库的版权。MNIST 数据库根据* [*Creative Commons Attribution-Share Alike 3.0 license*](https://creativecommons.org/licenses/by-sa/3.0/)*的条款提供。*

# 参考文献

[1] GitHub (2023), [artificial_neural_network](https://github.com/c-bruce/artificial_neural_network)

[2] Bruce, C. (2023). [PID Controller Optimization: A Gradient Descent Approach](https://medium.com/towards-data-science/pid-controller-optimization-a-gradient-descent-approach-58876e14eef2). *Medium*

[3] Deng, L. (2012). [The MNIST database of handwritten digit images for machine learning research](https://ieeexplore.ieee.org/document/6296535). *IEEE Signal Processing Magazine*, *29*(6), 141–142

[4] Nwankpa, C., Ijomah, W.L., Gachagan, A., & Marshall, S. (2018). [Activation Functions: Comparison of trends in Practice and Research for Deep Learning](https://arxiv.org/abs/1811.03378). *ArXiv, abs/1811.03378*
