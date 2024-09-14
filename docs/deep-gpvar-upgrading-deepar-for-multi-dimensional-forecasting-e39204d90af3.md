# Deep GPVAR：升级 DeepAR 实现多维度预测

> 原文：[`towardsdatascience.com/deep-gpvar-upgrading-deepar-for-multi-dimensional-forecasting-e39204d90af3`](https://towardsdatascience.com/deep-gpvar-upgrading-deepar-for-multi-dimensional-forecasting-e39204d90af3)

## Amazon 的新时间序列预测模型

[](https://medium.com/@nikoskafritsas?source=post_page-----e39204d90af3--------------------------------)![Nikos Kafritsas](https://medium.com/@nikoskafritsas?source=post_page-----e39204d90af3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e39204d90af3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e39204d90af3--------------------------------) [Nikos Kafritsas](https://medium.com/@nikoskafritsas?source=post_page-----e39204d90af3--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e39204d90af3--------------------------------) ·19 分钟阅读·2023 年 3 月 24 日

--

![](img/c5191da2e60488dbedbc2c1d45651487.png)

使用 DALLE 创建 [1]

> **警告：** 关于该模型的信息，包括下面的教程，可能不是最新的。阅读更新版本 [这里](https://aihorizonforecast.substack.com/p/deep-gpvar-upgrading-deepar-for-multi)

阅读新论文时最令人愉快的事情是什么？对我而言，就是以下内容：

想象一下，一个流行的模型突然升级——只需几个优雅的调整。

三年后，Amazon 工程师发布了其改版版本，称为 ***Deep GPVAR* [2] *(D****eep* ***G****aussian-****P****rocess* ***V****ector* ***A****uto-****r****egressive)*。

这是对原始版本的显著改进版。此外，它是开源的。本文中，我们讨论：

+   **深入了解 *Deep GPVAR* 的工作原理。**

+   **DeepAR 和 *Deep GPVAR* 的不同之处。**

+   ** *Deep GPVAR* 解决了哪些问题，以及为何它比 DeepAR 更好？**

+   **有关能源需求预测的实践教程。**

> 我推出了 [**AI Horizon Forecast**](https://aihorizonforecast.substack.com/)**，** 这是一个专注于时间序列和创新 AI 研究的新闻通讯。订阅 [这里](https://aihorizonforecast.substack.com/) 扩展你的视野！

# *什么是 Deep GPVAR？*

> Deep GPVAR 是一种自回归深度学习模型，利用低秩高斯过程共同建模数千个时间序列，考虑它们之间的相互依赖性。

简要回顾 *Deep GPVAR* 的优点：

+   **多时间序列支持：** 该模型使用多个时间序列数据来学习全局特征，从而提高其准确预测能力。

+   **额外协变量：** *Deep GPVAR* 允许额外特征（协变量）。

+   **可扩展性：** 该模型利用*低秩高斯分布*将训练扩展到多个时间序列**同时进行**。

+   **多维建模：** 与其他全球预测模型相比，*Deep GPVAR*模型将时间序列一起建模，而不是单独建模。这通过考虑它们的相互依赖性来提高预测准确性。

最后一部分是*Deep GPVAR*与 DeepAR 的区别所在。我们将在下一节中深入探讨这一点。

# 全球预测机制

在多个时间序列上训练的全球模型并不是一个新概念。但是为什么需要全球模型？

在我之前的公司，客户对时间序列预测项目感兴趣，主要需求如下：

> “我们有 10,000 个时间序列，我们希望创建一个单一模型，而不是 10,000 个单独的模型。”

时间序列可以代表产品销售、期权价格、气候污染等——这并不重要。重要的是，公司需要大量资源来训练、评估、部署和监控（**概念漂移**）*10,000*个生产中的时间序列。

所以，这是一个很好的理由。此外，那时没有[*N-BEATS*](https://medium.com/towards-data-science/n-beats-time-series-forecasting-with-neural-basis-expansion-af09ea39f538)或[Temporal Fusion Transformer](https://medium.com/towards-data-science/temporal-fusion-transformer-time-series-forecasting-with-deep-learning-complete-tutorial-d32c1e51cd91)。

然而，如果我们要创建一个全球模型，它应该学习什么？模型是否仅仅学习一个聪明的映射，根据输入条件处理每个时间序列？但这假设时间序列是**独立的**。

**模型是否应该学习适用于数据集中所有时间序列的全球时间模式？**

# 时间序列的相互依赖性

*Deep GPVAR*在 DeepAR 的基础上，寻求一种更先进的方法来利用多个时间序列之间的依赖关系，从而提高预测效果。

对于许多任务，这种做法是有意义的。

一个将全球数据集的时间序列视为独立的模型会失去在金融和零售等应用中有效利用它们关系的能力。例如，风险最小化的投资组合需要预测资产的协方差，而不同卖家的概率预测必须考虑竞争和侵蚀效应。

**因此，一个强大的全球预测模型不能假设潜在的时间序列是独立的。**

*Deep GPVAR 与 DeepAR 的区别*在于两个方面：

+   **高维估计：** *Deep GPVAR*将时间序列一起建模，考虑它们的关系。为此，模型使用**低秩高斯近似**来估计它们的**协方差矩阵**。

+   **扩展性：** *Deep GPVAR*并不像其前身那样仅仅对每个时间序列进行标准化。相反，模型学习如何通过使用**高斯 Copulas**首先转换每个时间序列来缩放它们。

以下部分详细描述了这两个概念的工作原理。

# 低秩高斯近似——简介

正如我们之前所说，研究多个时间序列之间关系的最佳方法之一是估计协方差矩阵。

然而，由于内存和数值稳定性限制，将此任务扩展到数千个时间序列并不容易实现。此外，协方差矩阵估计是一个耗时的过程——在训练期间，需要为每个时间窗口估计协方差。

为了解决这个问题，作者使用**低秩近似**来简化协方差估计。

让我们从基础开始。下面是多变量正态分布`N**∼**(μ,Σ)`的矩阵形式，其中均值`**μ** **∈** (k,1)`，协方差`**Σ** **∈** (k,k)`

![](img/c316b88c85d984a5ce6a287c1b09c5d1.png)

**方程 1：** 矩阵形式的多变量高斯分布

这里的问题是协方差矩阵`**Σ**`的大小是与数据集中时间序列的数量`N`平方相关的。

我们可以使用一种近似形式来解决这个挑战，称为**低秩高斯近似**。这种方法源于因子分析，并与[SVD（奇异值分解）](https://medium.com/towards-data-science/how-to-use-singular-value-decomposition-svd-for-image-classification-in-python-20b1b2ac4990)密切相关。

我们可以通过计算以下内容来近似，而不是计算大小为`**(**N,N**)**`**的完整协方差矩阵：

![](img/33f2c0aeb9ff6e8ae5d9334c88aefe18.png)

**方程 2：** 低秩协方差矩阵公式

其中`**D** **∈** R(N,N)`是对角矩阵，`**V** **∈** R(N,r)`。

但为什么我们使用低秩格式来表示协方差矩阵呢？因为`r<<N`，证明了高斯似然可以使用`O(Nr² + r³)`操作来计算，而不是`O(N³)`（证明可以在论文的附录中找到）。

低秩正态分布是 PyTorch 的**分布**模块的一部分。可以随意尝试，看看它是如何工作的：

```py
# multivariate low-rank normal of mean=[0,0], cov_diag=[1,1], cov_factor=[[1],[0]]

# covariance_matrix = cov_diag + cov_factor @ cov_factor.T

m = LowRankMultivariateNormal(torch.zeros(2), torch.tensor([[1.], [0.]]), 
torch.ones(2))
m.sample()  
#tensor([-0.2102, -0.5429])
```

# 深度 GPVAR 架构

**符号：** 从现在开始，所有粗体变量都被视为向量或矩阵。

> **注意：** 在[AI 项目文件夹](https://aihorizonforecast.substack.com/p/ai-projects)中找到一个关于 Deep GPVAR 的动手项目，该文件夹会不断更新最新时间序列模型的教程！

现在我们已经了解了低秩正态近似的工作原理，接下来我们将深入探讨*Deep GPVAR*的架构。

首先，*Deep GPVAR*类似于 DeepAR——该模型也使用 LSTM 网络。假设我们的数据集中包含`N`个时间序列，索引从`i= [1…N]`

在每个时间步`t`，我们有：

1.  一个 LSTM 单元将前一时间步`t-1`的目标变量`z_t-1` **用于部分时间序列**作为输入。此外，LSTM 接收前一时间步的隐藏状态`**h_t-1**`。

1.  模型使用 LSTM 来计算其隐藏向量`**h_t**`。

1.  隐藏向量`**h_t**`将被用来计算多变量高斯分布`N∼(μ,Σ)`的`**μ**`和`**Σ**`参数。这是一种特别的正态分布，称为**高斯 copula（**稍后会详细介绍**）**。

这个过程如**图 1**所示：

![](img/81fdb9b2b6e49070894141d5b932ef8b.png)![](img/8668ca3908cd12d4d4738e286e9ee156.png)

**图 1：**Deep GPVAR 的两个训练步骤。协方差矩阵 Σ 表示为 Σ=D+V*V^T ([来源](https://arxiv.org/abs/1910.03002))

在每个时间步`t`，*Deep GPVAR* 随机选择`B << N`个时间序列来实现低秩参数化：左侧，模型从（1、2 和 4）时间序列中选择，右侧，模型从（1、3 和 4）时间序列中选择。

作者在实验中将`B`配置为 20。对于可能包含超过`N=1000`个时间序列的数据集，这种方法的好处变得明显。

我们的模型估计了 3 个参数：

+   正态分布的**均值**`**μ**`。

+   协方差矩阵参数是`d`和`**v**`，根据**方程 2**。

它们都从`**h_t**` LSTM 隐藏向量中计算得出。**图 2**展示了低秩协方差参数：

![](img/a2d204f15dea0ffd4de84b0df258bbb8.png)

**图 2：**低秩协方差矩阵的参数化，如**方程 2**所示

> 注意符号：`μ_i`、`d_i`、`**v_i**`指的是我们数据集中第`i-th`个时间序列，其中`i ∈ [1..N]`。

对于每个时间序列`i`，我们创建`**y_i**`向量，它将`**h_i**`与`**e_i**`连接在一起——`**e_i**`向量包含第`i-th`时间序列的特征/协变量。因此，我们有：

![](img/58073208219d430b9f51f8f59c8e433a.png)

**图 3**展示了时间步`t`的训练快照：

![](img/f553782e64272b650bb415d98b3e6562.png)

**图 3：**低秩参数化的详细架构

注意：

+   `μ`和`d`是标量，而`**v**`是向量。例如，`μ = **w_μ**^T * **y**`，维度为`(1,p)`*`(p,1)`，因此`μ`是标量。

+   所有时间序列都使用相同的 LSTM，包括密集层投影`**w_μ**`、`**w_d**`和`**w_u**`。

因此，神经网络参数`**w_μ**`、`**w_d**`、`**w_u**`和`**y**`被用来计算`**μ**`和`**Σ**`参数，这些参数在**图 3**中展示。

# 高斯 Copula 函数

> 问题：`**μ**`和`**Σ**`参数化什么？

它们参数化了一种特殊的多变量高斯分布，称为**高斯 Copula**。

> 但为什么 Deep GPVAR 需要高斯 Copula？

记住，*Deep GPVAR* 进行多维预测，因此我们不能像在 DeepAR 中那样使用简单的单变量高斯分布。

> 好吧。那么为什么不使用我们熟悉的多变量高斯分布，而是使用像**方程 1**中那样的 Copula 函数呢？

2 个原因：

**1)** **因为多元高斯分布需要高斯随机变量作为边际分布。** 我们也可以使用混合分布，但它们过于复杂，不适用于每种情况。相反，高斯 Copulas 更易于使用，可以处理任何输入分布——这里的输入分布指的是我们数据集中单个时间序列。

因此，copula 学会在不对数据做出假设的情况下估计潜在的数据分布。

**2) 高斯 Copula 可以通过控制协方差矩阵的参数化来建模这些不同分布之间的依赖结构** `***Σ***`***。** 这就是 *Deep GPVAR* 学会考虑输入时间序列之间的相互依赖性，而其他全球预测模型则不做此类处理。

> **记住：时间序列可能具有不可预测的相互依赖性。** 例如，在零售中，我们有产品的自相残杀：一个成功的产品会从其类别中的类似商品中吸引需求。因此，我们在预测产品销售时也应该考虑这种现象。通过 copulas，Deep GPVAR 自动学习这些相互依赖性。

## 什么是 copulas？

Copula 是一个描述多个随机变量之间相关性的数学函数。

> **注意：** 如果你对 copulas 完全陌生，[这篇文章](https://medium.com/towards-data-science/copulas-an-essential-guide-applications-in-time-series-forecasting-f5c93dcd6e99) 深入解释了 copulas 是什么以及我们如何构造它们。

Copulas 在定量金融中被广泛用于投资组合风险评估。[它们的误用在 2008 年金融危机中也起到了重要作用。](http://samueldwatts.com/wp-content/uploads/2016/08/Watts-Gaussian-Copula_Financial_Crisis.pdf)

更正式地说，copula 函数 `**C**` 是 `N` 个随机变量的累积分布函数（CDF），其中每个随机变量（边际）均匀分布：

![](img/6b6dbbc8c39b1b59ab8718f0a16d712e.png)

**图 4** 显示了一个由 2 个边际分布组成的双变量 copula 的图像。copula 定义在 [0–1]² 域（x、y 轴）中，输出值在 [0–1]（z 轴）：

![](img/30a5e70fb487c96888bb47d5d6c3904a.png)

**图 4：** 由 2 个 beta 分布作为边际分布组成的高斯 copula CDF 函数

对于 `**C**` 的一个流行选择是高斯 Copula——**图 4** 中的 copula 也是高斯的。

## 我们如何构造 copulas

我们不会在这里深入讨论，但可以简要概述一下。

起初，我们有一个随机向量——一组随机变量。在我们的例子中，每个随机变量 `z_i` 代表时间序列 `i` 在时间步 `t` 的观测值：

![](img/664fdec17126ab7f2f266ca7fcf9a9f6.png)

然后，我们通过使用[*概率积分变换*](https://medium.com/towards-data-science/copulas-an-essential-guide-applications-in-time-series-forecasting-f5c93dcd6e99)来使我们的变量**均匀分布**：任何连续随机变量的累积分布函数（CDF）输出是均匀分布的，`F(z)=U`：

![](img/db326ee56efd02d5ce456390b83aa496.png)

最后，我们应用我们的高斯 copula：

![](img/7321552dd36eea752dadccde0a581061.png)

其中**Φ**^-1 是标准高斯 CDF `N∼(0,1)`的逆函数，**φ**（小写字母）是一个用`*μ*`和`*Σ*`参数化的高斯分布*。注意`Φ^-1[F(z)] = x`，其中`x~(-Inf, Inf)`，因为我们使用的是标准逆 CDF。

**那么，这里发生了什么？**

我们可以取任何连续随机变量，将其边际化为均匀分布，然后将其转化为高斯分布。操作链如下：

![](img/8188b43a463a65d9f50c0fb1093e1f8c.png)

这里有 2 种变换：

+   `F(z) = U`，被称为*概率积分变换*。简单来说，这种变换将任何连续随机变量转换为均匀分布。

+   `Φ^-1(U)=x`，被称为[*逆采样*](https://en.wikipedia.org/wiki/Inverse_transform_sampling)。简单来说，这种变换将任何均匀随机变量转换为我们选择的分布——在这里，`Φ` 是高斯分布，因此 `x` 也成为高斯随机变量。

在我们的案例中，`**z**`是我们数据集中时间序列的过去观察值。由于我们的模型对过去观察值的分布没有假设，我们使用**经验 CDF**——一种非参数（经验）计算任何分布 CDF 的特殊函数。

> **注意：** 如果你不熟悉**经验 CDF**，请参考我的[文章](https://medium.com/towards-data-science/copulas-an-essential-guide-applications-in-time-series-forecasting-f5c93dcd6e99)以获取详细解释。

换句话说，在`F(z) = U`变换中，`F`是经验 CDF，而不是变量`z`的实际高斯 CDF。作者在实验中使用了`m=100`个过去的观察值来计算经验 CDF。

## **回顾 copulas**

> 总结来说，高斯 copula 是一个多变量函数，使用`μ`和`Σ`直接参数化两个或更多随机变量的相关性。

但高斯 copula 如何与高斯多变量概率分布（PDF）不同？此外，高斯 copula 只是一个多变量 CDF。

1.  高斯 copula 可以使用任何随机变量作为边际分布，而不仅仅是高斯分布。

1.  数据的原始分布无关紧要——通过使用概率积分变换和经验 CDF，我们可以将原始数据转化为高斯分布，**无论它们如何分布**。

# 深度 GPVAR 中的高斯 copula

现在我们已经了解了 copula 的工作原理，是时候看看*Deep GPVAR*如何使用它们了。

回到**图 3**。使用 LSTM，我们已经计算了`*μ*`和`*Σ*`参数。我们所做的是以下操作：

**步骤 1：将我们的观察值转换为高斯分布**

使用 copula 函数，我们将观察到的时间序列数据点 `**z**` 转换为高斯 `**x**`。转换公式为：

![](img/da801c95a5e88dd990a01bb6453e9a13.png)

其中 `f(z_i,t)` 实际上是时间序列 `i` 的边际转换 `Φ^-1(F(z_i))`。

在实际应用中，我们的模型对过去观察值 `z` 的分布没有假设。因此，无论原始观察值的分布如何，我们的模型都可以有效地学习它们的行为，包括它们的相关性——这一切都归功于高斯 copula 的强大功能。

**步骤 2：使用计算出的高斯参数。**

我提到我们应该将观察值转换为高斯分布，但高斯分布的参数是什么？换句话说，当我说 `f(z_i) = Φ^-1(F(z_i))` 时，`Φ` 的参数是什么？

答案是 `*μ*` 和 `*Σ*` 参数——这些参数是从**图 3**中所示的密集层和 LSTM 计算得到的。

**步骤 3：计算损失并更新我们的网络参数**

总结一下，我们将观察值转换为高斯分布，并假设这些观察值由低秩正态高斯分布参数化。因此，我们有：

![](img/453c8a175f78d70f9cd5fa72ec5c354a.png)

其中 `f1(z1)` 是第一个时间序列的转换后的预测，`f2(z2)` 指的是第二个时间序列，而 `f_n(z_n)` 指的是我们数据集中的第 N 个时间序列。

最后，我们通过最大化**多元** **高斯对数似然函数**来训练我们的模型。论文采用了最小化损失函数**—**即前面带有负号的高斯对数似然。

![](img/a125c011c6b9b865364db9162d3bf096.png)

使用高斯对数似然损失，*Deep GPVAR* 更新其 LSTM 和**图 3**中显示的共享密集层权重。

另外，请注意：

+   `**z**` 不是单个观察值，而是所有 `N` 个时间序列在时间 `t` 的观察向量。求和会循环到 `T`，即最大查找窗口——在此之后计算高斯对数似然。

+   由于 **z** 是观察向量，高斯对数似然实际上是**多元**的。相比之下，DeepAR 使用的是单变量高斯似然。

# **Deep GPVAR: 大致概况**

我建议多次阅读文章，以掌握模型的工作原理。

> 一般来说，你可以将 Deep GPVAR 视为一个高斯随机过程。

对于那些不熟悉**随机过程**的人，你可以将随机过程视为一组随机变量——每个变量代表某一时刻随机事件的结果。

随机变量由某个集合（通常是时间）索引，这些随机变量的值彼此依赖。因此，一个事件的结果可以影响未来事件的结果。这种相互依赖性也解释了随机过程的时间性。

在我们的案例中，时间序列`y_i`的每个数据点可以视为一个随机变量。因此，*Deep GPVAR*可以被视为一个高斯过程`GP`，在每个数据点`y`上评估如下：

![](img/7c8d35297e83da00fabfd26cde061e2c.png)

现在，高斯过程的参数是函数，而不是变量。在上述方程中，`**μ**`、`**d**`、`**v**` **（带有波浪线）** 是时间函数，由`**y**`索引——其中`**y**`是在某个时间步的向量观察值。

在每个时间步，函数`**μ**`、`**d**`、`**v**` **（带有波浪线）** 本质上是 LSTM 和 Dense 层，有一个实现 `**μ**`、`**d**`、`**v**` **（不带波浪线）**。这个实现参数化了时间步`*t*`下 copula 函数的`*μ*`和`*Σ*`。因此，在训练过程中，在每个时间步，`*μ*`和`*Σ*`会变化，以最佳地解释我们的观察结果。

因此，*Deep GPVAR*作为高斯随机过程的概念是完全合理的。

# Deep GPVAR 变体

根据当前论文，亚马逊创建了 2 个模型，**Deep GPVAR**（我们在本文中描述）和**DeepVAR**。

DeepVAR 类似于*Deep GPVAR*。不同之处在于 DeepVAR 使用一个全球多变量 LSTM，一次接收和预测所有时间序列。另一方面，*Deep GPVAR*在每个时间序列上分别展开 LSTM。

在他们的实验中，作者将 DeepVAR 称为**Vec-LSTM**，而*Deep GPVAR*称为**GP***。

+   *Vec-LSTM*和*GP*项在原始论文的**表 1**中提到。

+   *Deep GPVAR*和*DeepVAR*术语在亚马逊的预测库[Gloun TS](https://ts.gluon.ai/stable/getting_started/models.html)中提到。

本文描述了***Deep GPVAR*变体**，该变体在平均情况下表现更好，并且比*DeepVAR*具有更少的参数。可以阅读原始论文以了解更多实验过程。

# 项目教程 — 需求能源预测

本教程使用了来自 UCI 的**ElectricityLoadDiagrams20112014** [4]数据集。该示例的笔记本可以从[这里](https://aihorizonforecast.substack.com/p/ai-projects)下载：

> **注意：** 目前，PyTorch Forecasting 库提供了 DeepVAR 版本。这就是我们在本教程中展示的变体。你还可以尝试所有 DeepAR 变体，包括亚马逊开源的[Gluon TS 库](https://github.com/awslabs/gluonts/tree/dev/src/gluonts/mx/model)中的 GPVAR。

## 下载数据

```py
!wget https://archive.ics.uci.edu/ml/machine-learning-databases/00321/LD2011_2014.txt.zip
!unzip LD2011_2014.txt.zip
```

## 数据预处理

```py
data = pd.read_csv('LD2011_2014.txt', index_col=0, sep=';', decimal=',')
data.index = pd.to_datetime(data.index)
data.sort_index(inplace=True)
data.head(5)
```

![](img/8d4ffbf8701559ff5f1bd0f8ce6eb654.png)

每一列代表一个消费者。每个单元格中的值是每 15 分钟的电力使用量。

此外，我们将数据汇总为每小时数据。由于模型的大小和复杂性，我们仅使用 5 个消费者进行模型训练（仅考虑非零值）。

```py
data = data.resample('1h').mean().replace(0., np.nan)
earliest_time = data.index.min()
df=data[['MT_002', 'MT_004', 'MT_005', 'MT_006', 'MT_008' ]]
```

接下来，我们将数据集转换为 PyTorch Forecasting 理解的特殊格式——称为***TimeSeriesDataset***。这种格式非常方便，因为它允许我们指定特征的性质（例如，它们是时间变化的还是静态的）。

> **注意：** 如果你想了解更多关于***TimeSeriesDataset***格式的信息，请查看我的[Temporal Fusion Transformer 文章](https://medium.com/towards-data-science/temporal-fusion-transformer-time-series-forecasting-with-deep-learning-complete-tutorial-d32c1e51cd91)，该文章详细解释了该格式的工作原理。

为了将数据集修改为 TimeSeriesDataset 格式，我们将所有时间序列垂直堆叠，而不是水平堆叠。换句话说，我们将数据框从“宽”格式转换为“长”格式。

此外，我们从日期列创建新的特征，如`day`和`month`—这些特征将帮助我们的模型更好地捕捉季节性动态。

```py
df_list = []

for label in df:

    ts = df[label]

    start_date = min(ts.fillna(method='ffill').dropna().index)
    end_date = max(ts.fillna(method='bfill').dropna().index)

    active_range = (ts.index >= start_date) & (ts.index <= end_date)
    ts = ts[active_range].fillna(0.)

    tmp = pd.DataFrame({'power_usage': ts})
    date = tmp.index

    tmp['hours_from_start'] = (date - earliest_time).seconds / 60 / 60 + (date - earliest_time).days * 24
    tmp['hours_from_start'] = tmp['hours_from_start'].astype('int')

    tmp['days_from_start'] = (date - earliest_time).days
    tmp['date'] = date
    tmp['consumer_id'] = label
    tmp['hour'] = date.hour
    tmp['day'] = date.day
    tmp['day_of_week'] = date.dayofweek
    tmp['month'] = date.month

    #stack all time series vertically
    df_list.append(tmp)

time_df = pd.concat(df_list).reset_index(drop=True)

# match results in the original paper
time_df = time_df[(time_df['days_from_start'] >= 1096)
                & (time_df['days_from_start'] < 1346)].copy()
```

最终预处理的数据框称为`time_df`。让我们打印其内容：

![](img/ce0186cef0e4fa8d3a621ab395d58320.png)

`time_df`现在已转换为*TimeSeriesDataset*的正确格式。此外，*TimeSeriesDataset*格式要求我们的数据具有**时间索引**。在这种情况下，我们使用`hours_from_start`变量。此外，`power_usage`是我们模型将尝试预测的**目标变量**。

## 创建数据加载器

在这一步，我们将`time_df`转换为*TimeSeriesDataSet*格式。我们这样做的原因是：

+   这使我们免去了编写自己的数据加载器的麻烦。

+   我们可以指定模型如何处理特征。

+   我们可以轻松地对数据集进行归一化。在我们的例子中，归一化是强制性的，因为所有时间序列在幅度上有所不同。因此，我们使用**GroupNormalizer**来单独归一化每个时间序列。

我们的模型使用一周（7*24）的回溯窗口来预测接下来的 24 小时的功率使用。

另外，请注意，`hours_from_start`既是时间索引，又是时间变化特征。为了演示，我们的验证集是最后一天：

```py
max_prediction_length = 24
max_encoder_length = 7*24
training_cutoff = time_df["hours_from_start"].max() - max_prediction_length

training = TimeSeriesDataSet(
    time_df[lambda x: x.hours_from_start <= training_cutoff],
    time_idx="hours_from_start",
    target="power_usage",
    group_ids=["consumer_id"],
    max_encoder_length=max_encoder_length,
    max_prediction_length=max_prediction_length,
    static_categoricals=["consumer_id"],
    time_varying_known_reals=["hours_from_start","day","day_of_week", "month", 'hour'],
    time_varying_unknown_reals=['power_usage'],
        target_normalizer=GroupNormalizer(
        groups=["consumer_id"]
    ),  # we normalize by group
    add_relative_time_idx=True,
    add_target_scales=True,
    add_encoder_length=True,
)

validation = TimeSeriesDataSet.from_dataset(training, time_df, predict=True, stop_randomization=True, min_prediction_idx=training_cutoff + 1)

# create dataloaders for  our model
batch_size = 64 
# if you have a strong GPU, feel free to increase the number of workers  
train_dataloader = training.to_dataloader(train=True, batch_size=batch_size, num_workers=2, batch_sampler="synchronized")
val_dataloader = validation.to_dataloader(train=False, batch_size=batch_size * 10, num_workers=2, batch_sampler="synchronized")
```

## 基线模型

同样，记得创建一个基线模型。

我们创建了一个朴素的基线模型，用于预测前一天的功率使用曲线：

```py
actuals = torch.cat([y for x, (y, weight) in iter(val_dataloader)])
baseline_predictions = Baseline().predict(val_dataloader)
(actuals - baseline_predictions).abs().mean().item()

# ➢25.139617919921875
```

## 构建和训练我们的模型

我们现在可以开始训练我们的模型了。我们将使用来自 PyTorch Lightning 库的*Trainer*接口。

首先，我们需要配置超参数和回调函数：

+   使用**EarlyStopping**回调函数来监控验证损失。

+   **Tensorboard**用于记录我们的训练和验证指标。

+   `hidden_size`和`rnn_layers`分别指代 LSTN 单元的数量和 LSTM 层的数量。

+   我们还使用`gradient_clip_val`（梯度裁剪）来防止过拟合。此外，模型的默认 dropout 为`0.1`——我们保留默认值。

我们的损失函数是`**MultivariateNormalDistributionLoss**(rank : int)`。该函数将**rank**参数作为一个参数——这是我们在开始时解释的`R`值。

记住，值越低，训练期间的加速效果越大。作者在他们的实验中使用**rank**=10——我们在这里也做了相同的处理：

```py
pl.seed_everything(42)

early_stop_callback = EarlyStopping(monitor="val_loss", min_delta=1e-4, patience=4, verbose=True, mode="min")
lr_logger = LearningRateMonitor(logging_interval='step', log_momentum=True)  # log the learning rate
logger = TensorBoardLogger("lightning_logs")  # logging results to a tensorboard

trainer = pl.Trainer(
    max_epochs=50,
    accelerator='auto', 
    devices=1,
    enable_model_summary=True,
    gradient_clip_val=0.1,
    callbacks=[lr_logger, early_stop_callback],
    logger=logger,
)

net = DeepAR.from_dataset(
    training, 
    learning_rate=0.001, 
    hidden_size=40, 
    rnn_layers=3, 
    loss=MultivariateNormalDistributionLoss(rank=10)
)

trainer.fit(
    net,
    train_dataloaders=train_dataloader,
    val_dataloaders=val_dataloader
)
```

在 6 个周期后，EarlyStopping 触发，训练结束。

> **注意：** 如果你想运行 DeepAR 而不是 DeepVAR，请使用[NormalDistributionLoss](https://pytorch-forecasting.readthedocs.io/en/stable/api/pytorch_forecasting.metrics.distributions.NormalDistributionLoss.html)。

## 加载和保存最佳模型

```py
best_model_path = trainer.checkpoint_callback.best_model_path
print(best_model_path)
best_deepvar = DeepAR.load_from_checkpoint(best_model_path)

#path:
#lightning_logs/lightning_logs/version_0/checkpoints/epoch=6-step=40495.ckpt
```

不要忘记下载你的模型：

```py
#zip and download the model
!zip  -r model.zip lightning_logs/lightning_logs/version_0/*
```

这就是如何重新加载模型——你只需要记住最佳模型路径：

```py
!unzip model.zip
best_model_path='lightning_logs/lightning_logs/version_0/checkpoints/epoch=6-step=40495.ckpt'
best_deepvar = DeepAR.load_from_checkpoint(best_model_path)
```

## Tensorboard 日志

使用 Tensorboard 仔细查看训练和验证曲线。你可以通过执行以下命令启动它：

```py
# Start tensorboard
%load_ext tensorboard
%tensorboard --logdir lightning_logs
```

## 模型评估

获取验证集上的预测并计算平均**P50**（分位数中位数）**损失**：

```py
actuals = torch.cat([y[0] for x, y in iter(val_dataloader)])
predictions = best_deepvar.predict(val_dataloader)

#average p50 loss overall
print((actuals - predictions).abs().mean().item())
#average p50 loss per time series
print((actuals - predictions).abs().mean(axis=1))

# 6.78986
# tensor([ 1.1948,  6.9811,  2.7990,  8.3856, 14.5888])
```

注意，我们获得的分数比[TFT 实现](https://medium.com/towards-data-science/temporal-fusion-transformer-time-series-forecasting-with-deep-learning-complete-tutorial-d32c1e51cd91)略差。我们使用的损失函数是概率性的——因此，每次得到的分数会略有不同。

最后两个时间序列的损失略高，因为它们的相对幅度较大。

## 绘制验证数据上的预测

```py
raw_predictions, x = best_deepvar.predict(val_dataloader, mode="raw", return_x=True)

for idx in range(5):  # plot all 5 consumers
    fig, ax = plt.subplots(figsize=(10, 4))
    best_deepvar.plot_prediction(x, raw_predictions, idx=idx, add_loss_to_title=True,ax=ax);
```

![](img/06810a5dfe9056de790441ad79a59212.png)

**图 5：** MT_002 的验证数据预测

![](img/8221abe2eee378543f06a92b5646d8a0.png)

**图 6：** MT_004 的验证数据预测

![](img/296881231e8ac8fec22168bf2b83e217.png)

**图 7：** MT_005 的验证数据预测

![](img/0613e83a4a18e6e829ee64dff68c5f59.png)

**图 8：** MT_006 的验证数据预测

![](img/c8b86c21ff8afddd84a8e0f09c9bf96b.png)

**图 9：** MT_008 的验证数据预测

验证集上的预测相当令人印象深刻。

同时，请注意我们没有进行任何超参数调整，这意味着我们可以获得更好的结果。

## 绘制特定时间序列的预测

之前，我们使用`idx`参数绘制了验证数据上的预测，该参数遍历了数据集中的所有时间序列。我们可以更具体地输出特定时间序列的预测：

```py
fig, ax = plt.subplots(figsize=(10, 5))

raw_prediction, x = best_deepvar.predict(
    training.filter(lambda x: (x.consumer_id == "MT_004") & (x.time_idx_first_prediction == 26512)),
    mode="raw",
    return_x=True,
)
best_deepvar.plot_prediction(x, raw_prediction, idx=0, ax=ax);
```

![](img/267d7a183f429edd937291ab15e065d4.png)

**图 10：** MT_004 在训练集上的第二天预测

在**图 10**中，我们绘制了时间索引=26512 的**MT_004**消费者的第二天预测。

记住，我们的时间索引列`hours_from_start`从 26304 开始，我们可以从 26388 开始获取预测（因为我们之前设置了`min_encoder_length=max_encoder_length // 2`，即`26304 + 168//2=26388`）。

## 绘制协方差矩阵

*Deep (GP)VAR*最大的优势在于协方差矩阵的估计——这提供了对数据集中时间序列相互依赖关系的一些洞察。

在我们的案例中，进行此计算没有意义，因为我们的数据集中只有 5 个时间序列。然而，以下是展示如何在有多个时间序列的数据集上进行计算的代码：

```py
cov_matrix = best_deepvar.loss.map_x_to_distribution(
    best_deepvar.predict(val_dataloader, mode=("raw", "prediction"), n_samples=None)
).base_dist.covariance_matrix.mean(0)

# normalize the covariance matrix diagnoal to 1.0
correlation_matrix = cov_matrix / torch.sqrt(torch.diag(cov_matrix)[None] * torch.diag(cov_matrix)[None].T)

fig, ax = plt.subplots(1, 1, figsize=(10, 10))
ax.imshow(correlation_matrix, cmap="bwr");
```

最终，PyTorch Forecasting 库提供了额外的功能，如**样本外预测和 Optuna 的超参数调优**。你可以在 TFT 教程中深入了解这些内容。

# 结束语

*Deep GPVAR*及其变体是一种强大的时间序列预测家族，具备了亚马逊研究的专业知识。

该模型通过考虑数千个时间序列之间的相互依赖性来处理多变量预测。

此外，如果你想了解更多关于[DeepAR](https://medium.com/p/bc717771ce85)初始架构的信息，欢迎查阅这篇相关文章。

# 感谢阅读！

+   在[Linkedin](https://www.linkedin.com/in/nikos-kafritsas-b3699180/)上关注我吧！

+   订阅我的[新闻通讯](https://aihorizonforecast.substack.com/welcome)，AI Horizon Forecast！

[## AutoGluon-TimeSeries : 创建强大的集成预测 - 完整教程](https://aihorizonforecast.substack.com/p/autogluon-timeseries-creating-powerful?source=post_page-----e39204d90af3--------------------------------)

### 亚马逊的时间序列预测框架应有尽有。

[aihorizonforecast.substack.com](https://aihorizonforecast.substack.com/p/autogluon-timeseries-creating-powerful?source=post_page-----e39204d90af3--------------------------------)

# 参考文献

[1] 使用 Stable Diffusion 创建，CreativeML Open RAIL-M 许可证。文本提示：“在太空中旅行的星云，数字艺术，插图”，到 rg

[2] D. Salinas 等人，[*DeepAR: Probabilistic forecasting with autoregressive recurrent networks*](https://arxiv.org/pdf/1704.04110.pdf)，《国际预测期刊》（2019）

[3] D. Salinas 等人，[高维多变量预测与低秩高斯 Copula 过程](https://arxiv.org/abs/1910.03002)

[4] [ElectricityLoadDiagrams20112014](https://archive.ics.uci.edu/ml/datasets/ElectricityLoadDiagrams20112014) 数据集，由 UCI 提供，CC BY 4.0。

*所有图片均由作者创建，除非另有说明*
