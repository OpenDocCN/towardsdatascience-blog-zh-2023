# 马尔科夫链蒙特卡罗 (MCMC) 方法介绍

> 原文：[`towardsdatascience.com/introduction-to-markov-chain-monte-carlo-mcmc-methods-b5bad18bc243`](https://towardsdatascience.com/introduction-to-markov-chain-monte-carlo-mcmc-methods-b5bad18bc243)

## 马尔科夫链、Metropolis-Hastings 算法、Gibbs 采样，以及它们与贝叶斯推断的关系

[](https://medium.com/@hrmnmichaels?source=post_page-----b5bad18bc243--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----b5bad18bc243--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b5bad18bc243--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5bad18bc243--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----b5bad18bc243--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5bad18bc243--------------------------------) ·阅读时间 14 分钟·2023 年 2 月 21 日

--

![](img/f1fc4e2428f11d573f2bd8434e5b64c7.png)

照片由 [Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/uKlneQRwaxY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上提供

本文是对马尔科夫链蒙特卡罗 (MCMC) 采样方法的介绍。我们将特别考虑两种方法，即 Metropolis-Hastings 算法和 Gibbs 采样。我们将介绍这些方法并证明它们的有效性，提供 Python 中的实际示例，最后解释采样如何应用于贝叶斯推断以及其重要性。

# 介绍

MCMC 方法是一类采样方法，利用马尔科夫链生成依赖的数据样本。其基本思想是构建这样一种马尔科夫链，使得采样简单，而其平稳分布即为我们的目标分布——这样当跟随这些链时，最终我们能从目标分布中获得样本。

为什么我们需要这个？在之前的一篇文章中，我介绍了基本的采样方法，其中包括了复杂分布的拒绝采样和重要性采样。这些方法生成独立的数据样本，而这里我们生成的是依赖的数据样本，如前所述——这并没有回答之前的问题，但这是一个重要的区别。然而，在之前的文章中，我们看到所提出的方法存在严重的限制：很难找到合适的建议分布，特别是在高维空间中，这会导致高方差和浪费计算。

在这些情况下，MCMC 方法，即遵循（简单的）马尔可夫链表现更好，特别是由于对我们要采样的分布所需的信息较少，以及我们只需要能够评估到一个固定因子的事实。也就是说：我们不需要能够评估给定`x`的完整 pdf，`p(x)`，但能够计算`zp(x)`就足够了。在本文的最后，我们将通过应用它来解决一个贝叶斯推断问题，来看到它为何如此强大。在许多教程和解释中，这最后一点通常只是作为附带说明简要提及——但我认为这值得——特别是对于贝叶斯推断的初学者——更多关注。

当然，MCMC 方法也有其缺点：由于样本之间的相关性，*有效*样本大小会减少，并且方法有时可能不收敛或非常慢。

# 马尔可夫链

由于正如名字所示（而且我们到目前为止已经多次提到过），MCMC 方法基于 [马尔可夫链](https://en.wikipedia.org/wiki/Markov_chain)，因此我们首先介绍这些方法。

这些是将随机过程建模为事件序列的一种方法。在这其中，*马尔可夫性质*指出下一个状态仅依赖于当前状态，而不依赖于任何历史信息。

（小插曲：许多实际的机器学习方法要求这一属性，例如强化学习。要求这种一步依赖可能看起来非常限制和不切实际——但请注意，我们可以简单地将状态空间扩展到任意维度，特别是将过去的事件包含在当前状态中——从而完全绕过这种“限制”。）

形式上，设我们考虑一个随机变量`X`，并用`X₀`、`X₁`等表示其每个时间步的实现。`X`随时间的发展由一个转移函数`P`给出，其中

![](img/02d81ab19151acffac1d93168c539508.png)

表示`X`从状态`i`转移到状态`j`的概率是`p`。

要完全指定一个马尔可夫链，此外我们还需要定义一个初始分布`π₀`。有了这个，我们可以从`π₀`开始，迭代应用`P`，得到每个时间戳的分布`π₁`、`π₂`等。

让我们通过一个例子来可视化这一点。我们选择了以下转移矩阵：

![](img/5082dd6763eec64021c6c7c472d6fb4b.png)

请注意，在我们的符号中，索引`ij`表示从状态`j`到`i`的转移概率，出于方便考虑。

我们现在取一个随机初始分布，并跟踪马尔可夫链的 30 步。可以通过以下 Python 代码实现：

```py
import numpy as np

P = np.asarray([[0.3, 0.5, 0.75], [0.1, 0.1, 0.1], [0.6, 0.4, 0.15]])

print(f"Transition matrix P: {P}")

# Generate random initial distribution (normalize to obtain valid distribution).
pi = np.random.rand(3)
pi /= np.sum(pi)

print(f"Initial distribution pi_0: {pi}")

for i in range(30):
    pi = np.matmul(P, pi)
    if i % 5 == 0:
        print(f"Distribution after i steps: {pi}")
```

执行此程序时，我们将得到类似于以下的输出：

```py
Distribution after i steps: [0.51555326 0.1 0.38444674]
Distribution after i steps: [0.499713 0.1 0.400287]
Distribution after i steps: [0.5000053 0.1 0.3999947]
Distribution after i steps: [0.4999999 0.1 0.4000001]
Distribution after i steps: [0.5 0.1 0.4]
Distribution after i steps: [0.5 0.1 0.4]
```

如我们所见，这个马尔可夫链会收敛——对于任何初始分布——到分布 `[0.5, 0.1, 0.4]`——我们称之为该马尔可夫链的平稳分布。

在继续之前，我们将介绍一个标准，在后续章节中需要用来判断马尔可夫链是否收敛：*详细平衡*。我们说一个马尔可夫链满足详细平衡标准，如果存在一个分布 `π` 满足：

![](img/b39d9e9063701e593522c35345a382d7.png)

即，从状态 `j` 转移到状态 `i` 的概率与考虑分布 `π` 的反向转移概率相同。直观上这也应该是合理的，因为这解释了为什么会产生一个平稳分布。请随意确认上述定义的马尔可夫链是否满足该标准，并且确实 `[0.5, 0.1, 0.4]` 是满足它的分布。

# Metropolis-Hastings

凭借这些知识，我们现在描述并介绍一种最常见和最常用的 MCMC 算法，即 [Metropolis-Hastings 算法](https://en.wikipedia.org/wiki/Metropolis%E2%80%93Hastings_algorithm)。总而言之，我们要做的是从一个难以处理的概率分布 `f(x)`（目标分布）中采样值。

让我们从算法概述开始。实际上，它由以下步骤组成：

1.  在目标分布的支持范围内选择一个任意初始值 `x₀`

1.  使用提议分布 `q` 抽取 `y₁`

1.  计算 `p₁`（见下文）

1.  从均匀分布 `[0, 1]` 中抽取 `u₁`

1.  如果 `u₁ ≤ p₁`，则设置 `x₁ = y₁`，否则设置 `x₁ = x₀`

1.  重复步骤 2-5

`p₁` 由以下给出：

![](img/990d62f08043f5efbfd2c2f5953c1271.png)

## 示例

让我们用一个具体的例子来演示这个过程，使用 Python 实现。设置是：我们想要采样的目标分布是一个高斯分布。我们的提议分布是另一个高斯分布。这自然并不是一个现实世界的实际例子。然而，我相信并希望，这种简化的设置有助于理解，而不是使读者感到困惑。请注意，在这个例子中，所有感兴趣的值都是一维的。

对应的 Python 代码如下：

```py
import matplotlib.pyplot as plt
import numpy as np
import scipy.stats

NUM_SAMPLES = 10000

# Target distribution
f = scipy.stats.norm(5, 2)

# Plot target distribution
x = np.linspace(-5, 15, 5000)
plt.plot(x, f.pdf(x))

# Step 1
x = np.random.uniform(-2, 2)

# Proposal distribution
q = scipy.stats.norm(0, 1)

samples = []

for i in range(NUM_SAMPLES):
    # Step 2
    y = x + q.rvs()
    # Step 3
    p = min(f.pdf(y) / f.pdf(x) * q.pdf(x - y) / q.pdf(y - x), 1)
    # Step 4
    u = np.random.uniform(0, 1)
    # Step 5
    x = y if u <= p else x
    samples.append(x)

plt.hist(samples, density=True, bins=30)
plt.show()
```

让我们详细了解一下。在开始时，我们使用 scipy 的 stats 模块来表示目标分布 `f` —— 然后绘制其概率密度函数（pdf）。接着我们定义一个初始值 `x` 来开始采样 —— 仅仅是从均匀分布中生成一个值。然后我们进入采样循环，根据上述介绍的算法迭代生成 `NUM_SAMPLES` 个值。作为提议分布，我们使用另一个高斯分布 `q` —— 这会生成一个新值 `y`，通过在 `x` 周围按照此高斯分布“跳跃”获得。值得注意的是，`q` 的条件评估等于给定跳跃范围的 `q` 的 pdf —— 直观地说，跳得越远，可能性就越小。

执行此程序应产生类似以下的结果：

![](img/911cce25f9c50a0899befa04e3d88454.png)

我们看到我们正确地从“未知”分布 `f` 中采样。

## 正确性证明

要证明 Metropolis-Hastings 算法的正确性，我们需要展示所使用的马尔可夫链的平稳分布确实是目标分布。为此，我们使用了上述介绍的详细平衡符号。记住，这涉及到展示以下内容：

![](img/6c2fbdbe6c83267d45a41aa48d2ec5ec.png)

即无论我们是首先访问状态`(t-1)`然后转移到`(t)`，还是反之，都没有关系。

因此，让我们评估这个方程的左侧，并简单地代入我们的提议分布和接受标准：

![](img/3300d56bc2a80df286db4113a6c0e2bf.png)

一个快速的重新表述为：

![](img/c41db989f62547d6b9fe7670594bc248.png)

当对上述目标方程的右侧进行类似操作时，我们得到相同的结果，从而完成证明。

## 讨论

我们在引言中提到 MCMC 方法，如 Metropolis-Hastings，比例如拒绝抽样更优越和高效。这是正确的，但我们仍应努力选择`q`，因为这一选择将影响转换的速度。再考虑一下接受标准：

![](img/e04627702d98f215d75281295238f8ff.png)

如果我们将`q`定义为等于`f`，我们将得到 1——即接受所有样本，这是理想情况。自然，这是不可能的，因为我们无法从`f`中抽样（这也是我们要这样做并从`q`中抽样的原因）。不过，这提供了如何选择`q`的一些直觉。相反，如果`q`选择不当，我们将拒绝许多样本，得到多个高度相关的样本，这是一个问题（链条在某个区域“卡住”了）。

这些讨论与*有效样本*大小和*烧入*的术语相关。由于 MCMC 方法生成的是相关样本，而不是独立样本，在研究这些时我们必须考虑这一点。特别地，这产生了有效样本大小的术语——它可以被视为“清除”了相关效应的实际样本大小。此外，通常会丢弃由 MCMC 算法获得的前`N`个元素（烧入）：这主要是为了平衡“坏”的初始化，这些初始化位于低概率区域，并且提议分布难以摆脱这些区域。

# Gibbs 抽样

作为 MCMC 抽样方法的第二个例子，我们将看看[吉布斯抽样](https://en.wikipedia.org/wiki/Gibbs_sampling)。由于我们已经介绍了基本概念并证明了一种 MCMC 方法的正确性，这次我们会快得多——但我还是想把它放在这里，以达到教程的足够深度，并建议读者参考其他资源以获取更多细节，或者自己进行计算。

## 概述

Gibbs 采样用于从具有多个变量的分布中进行采样，当从联合分布 `p(X, Y)` 采样困难时，我们可以采样条件分布 `p(X | Y)`、`p(Y | X)`。利用这一点，所使用的马尔科夫链在 `X` 和 `Y` 的采样值之间迭代，并利用更新后的条件分布。因此，整体上 — 引入和实现相当迅速 — 我们将在下一节中进行详细探讨。

为了引入，我们将考虑一个二维的 [多元正态分布](https://en.wikipedia.org/wiki/Multivariate_normal_distribution)。这种多维正态分布由均值向量 `μ` 和协方差矩阵 `Σ` 特征化。方便的是，所需的条件分布也是正态分布，且（以 `x₁` 为例）定义为：

![](img/000ef162d4dcfc55bee38a27770e9ebd.png)![](img/5a4217177bf288e1fb4b8ae58dab6651.png)

## 实现

首先，让我们使用 `scipy.stats` 来定义并绘制我们的目标分布：

```py
from typing import Any

import matplotlib.pyplot as plt
import numpy as np
import numpy.typing as npt
import scipy.stats

MEAN = np.asarray([0, 0])
VARIANCE = np.asarray([[0.25, 0.3], [0.3, 1]])

def plot_multivariate(
    mean: npt.NDArray[np.float32], variance: npt.NDArray[np.float32]
) -> None:
    multivariate_normal = scipy.stats.multivariate_normal(mean, variance)

    num_ticks = 100
    min_axis_value = -5
    max_axis_value = 5

    x = np.linspace(min_axis_value, max_axis_value, num_ticks)
    y = np.linspace(min_axis_value, max_axis_value, num_ticks)

    X, Y = np.meshgrid(x, y)
    pos = np.array([X.flatten(), Y.flatten()]).T

    fig = plt.figure()
    ax = fig.add_subplot(projection="3d")
    ax.plot_surface(
        X,
        Y,
        multivariate_normal.pdf(pos).reshape((100, 100)),
        cmap="viridis",
        linewidth=0,
    )

    plt.show()

plot_multivariate(MEAN, VARIANCE)
```

我们应该得到类似这样的结果：

![](img/c1f306082382d092fdce728e2d1332dc.png)

接下来，我们按照上述描述执行 Gibbs 采样过程：

```py
def get_cond_distr_x(
    mean: npt.NDArray[np.float32], variance: npt.NDArray[np.float32], y: float
) -> Any:
    mean = mean[0] + variance[0, 1] * 1 / variance[1, 1] * (y - mean[1])
    var = variance[0, 0] - variance[0, 1] * 1 / variance[1, 1] * variance[1, 0]
    return scipy.stats.norm(mean, var)

def get_cond_distr_y(
    mean: npt.NDArray[np.float32], variance: npt.NDArray[np.float32], x: float
) -> Any:
    mean = mean[1] + variance[1, 0] * 1 / variance[0, 0] * (x - mean[0])
    var = variance[1, 1] - variance[1, 0] * 1 / variance[0, 0] * variance[0, 1]
    return scipy.stats.norm(mean, var)

def gibbs_sampling(
    mean: npt.NDArray[np.float32],
    variance: npt.NDArray[np.float32],
    num_samples: int = 50000,
) -> npt.NDArray[np.float32]:
    xs = []
    ys = []

    x = 0
    for i in range(num_samples):
        y = get_cond_distr_y(mean, variance, x).rvs()
        x = get_cond_distr_x(mean, variance, y).rvs()
        xs.append(x)
        ys.append(y)

    return np.stack((xs, ys)).transpose(1, 0)

sampled_points = gibbs_sampling(MEAN, VARIANCE)
```

最终，我们通过绘制 3D 直方图来验证获得的分布是正确的：

```py
def draw_3d_hist(points: npt.NDArray[np.float32]) -> None:
    # Taken from https://matplotlib.org/stable/gallery/mplot3d/hist3d.html.
    fig = plt.figure()
    ax = fig.add_subplot(projection="3d")
    hist, xedges, yedges = np.histogram2d(
        points[:, 0], points[:, 1], bins=50, range=[[-5, 5], [-5, 5]]
    )

    # Construct arrays for the anchor positions of the 16 bars.
    xpos, ypos = np.meshgrid(
        xedges[:-1] + 0.25, yedges[:-1] + 0.25, indexing="ij"
    )
    xpos = xpos.ravel()
    ypos = ypos.ravel()
    zpos = 0

    # Construct arrays with the dimensions for the bars.
    dx = dy = 0.5 * np.ones_like(zpos)
    dz = hist.ravel()

    ax.bar3d(xpos, ypos, zpos, dx, dy, dz, zsort="average")

    plt.show()

draw_3d_hist(sampled_points)
```

![](img/17a334b9bbfbc20b80a0cefc39d11814.png)

# 贝叶斯推断中的应用

为了总结这篇文章，让我们举一个实际的使用案例，在这个案例中，这种采样变得非常方便，并帮助解决重要问题：[贝叶斯推断](https://en.wikipedia.org/wiki/Bayesian_inference)。我们将在未来的帖子中详细介绍这一点，目前：这是解决给定问题的“完整”概率分布，特别是计算给定数据的参数的概率分布：

![](img/c65eadcada2478c53f78c81b1eedc2ff.png)

这些术语通常被称为：

![](img/5be7854aafd96a21566940d45d750248.png)

使解决这个方程特别具有挑战性的是证据，因为这需要对所有可能的参数值进行边际化，即：

![](img/9785de884a936b7618763b1b596592cd.png)

这个积分通常很难计算，甚至不可解。

因此，以 MCMC 方法的形式进行数值近似是一个很好的起点，并且是解决此类问题的常见选择。像 Metropolis-Hastings 这样的方法特别有用，因为它们只要求能够评估分布直到某个归一化常数 — 证据恰好就是这样的常数！这意味着我们可以制定并处理后验分布，而不需要处理复杂的分母部分。

让我们用一个例子来说明：我们将抛掷一个（不公平的）硬币 `N` 次，并且有兴趣找出硬币正面朝上的概率 `θ`。特别地，我们不仅仅希望得到一个点估计，而是采用贝叶斯方法，建模完整的后验分布。

让我们更详细地分析一下单个术语：抛硬币的结果遵循 [伯努利分布](https://en.wikipedia.org/wiki/Bernoulli_distribution)，并且，记 `Nₕ` 为观察到的正面次数，`Nₜ` 为相应的反面次数，对于给定的参数值 `θ`，这产生以下似然函数：

![](img/c9df7b66ce44d61e935b375c2ccd922e.png)

接下来，我们需要找到一个合适的先验——即对估计参数值引入某种信念。由于我们不必担心解析地解决问题（见：[共轭先验](https://en.wikipedia.org/wiki/Conjugate_prior)），我们可以自由选择任何先验。因此，我们简单地选择均值为 0.5、标准差为 0.2 的正态分布——表示我们期望硬币大约是 50:50，但也覆盖 [0, 1] 的所有情况。

这两个术语足以运行 Metropolis-Hastings 算法。在其中，我们需要计算某些参数值 `y` 和 `x` 的 `f(y)/f(x)`，以及我们感兴趣的密度函数 `f`。在我们的例子中，正如前面提到的，这是 `p(θ|x)`。我们也已经提到，证据部分会被抵消，因为这是一个常数。剩下的就是以下内容：

![](img/8bf2a74917bfe90792a8e5bfec9d5487.png)

通过这些评估和上述对 Metropolis-Hastings 算法的介绍，将其移植到 Python 中应该不会太困难：

```py
import matplotlib.pyplot as plt
import numpy as np
import scipy.stats

NUM_THROWS = 100  # Number of coin tosses
THETA_TRUE = 0.3  # True probability for landing heads
THETA_PRIOR = 0.5  # Prior estimate for heads probability
NUM_SAMPLES = 100000  # Number of MCMC steps

# Define the unfair coin and generate data from it
unfair_coin = scipy.stats.bernoulli(THETA_TRUE)
D = np.asarray([unfair_coin.rvs() for _ in range(NUM_THROWS)])

# Define prior distribution
prior = scipy.stats.norm(THETA_PRIOR)

def likelihood_ratio(theta_1, theta_2):
    return (theta_1 / theta_2) ** np.sum(D == 1) * (
        (1 - theta_1) / (1 - theta_2)
    ) ** np.sum(D == 0)

def norm_ratio(theta_1, theta_2):
    return prior.pdf(theta_1) / prior.pdf(theta_2)

# Step 1
x = np.random.uniform(0, 1)

# Proposal distribution
q = scipy.stats.norm(0, 0.1)

samples = []

for i in range(NUM_SAMPLES):
    # Step 2
    y = x + q.rvs()
    # Step 3
    ratio = likelihood_ratio(y, x) * norm_ratio(y, x)
    p = min(ratio * q.pdf(x - y) / q.pdf(y - x), 1)
    # Step 4
    u = np.random.uniform(0, 1)
    # Step 5
    x = y if u <= p and 0 <= y <= 1 else x
    samples.append(x)

plt.hist(samples, density=True, bins=100)
plt.show()
```

运行这段代码应该会输出类似的内容：

![](img/ced0a88512ca7f0f3385df82ec4131ed.png)

正如我们所见，该算法正确地找到了约为 0.3 的真实`θ`，这从后验分布的模式中可以看出。我们还可以观察到一些方差，这很好，实际上也是预期中的——这就是我们进行全面贝叶斯推断的原因之一。抛硬币“仅”100 次可以给我们一个对其真实翻转概率的良好初步估计，但为了更加确定，我们希望看到更多的例子。

所以让我们将数据集增加到 5000 次抛掷，并检查这种情况下的输出：

![](img/17592cf071b9d65e04de731537d93f7b.png)

现在，后验分布的方差确实比预期的要低得多。

# 结论

在这篇文章中，我们介绍了马尔科夫链蒙特卡罗（MCMC）方法，这是一种用于数值采样的强大方法。这些方法使我们能够高效地从复杂分布中进行采样，而对可处理性的要求并不严格：特别是，我们只需要能够评估感兴趣的分布到固定因子的程度。MCMC 方法通过生成马尔科夫链来工作，其平稳分布就是目标分布——我们可以跟踪这个分布，并有效地采样我们所需要的分布。

作为第一个算法，我们介绍了 Metropolis-Hastings，并证明了它的正确性。它通过引入一个提议分布来工作，该分布用于从当前点“跳”到一个新点。这个新点以一定的概率被接受，这个概率与这些点周围的概率密度比成正比。

接下来，我们讨论了吉布斯抽样，这是一种用于抽样多维分布的方法。核心思想是使用条件分布，并通过为每个维度抽样新值，同时保持其他维度固定来迭代。

最终，我们给出了一个贝叶斯推断的实际例子。分析性地解决这个问题需要解一个复杂的积分，这使得它成为一个典型的（而且非常常见的）数值逼近示例。我们展示了如何通过模拟不公平抛硬币来估计伯努利变量的后验分布。

我希望这篇文章对你有帮助，并对这个令人兴奋的领域提供了一些见解。感谢阅读！

*所有图像，除非另有说明，均由作者生成。*

本文是关于抽样的系列文章的第三部分。你可以在这里找到其他部分：

+   第一部分：[抽样方法简介](https://medium.com/towards-data-science/introduction-to-sampling-methods-c934b64b6b08)

+   第二部分：通过重要性抽样减少方差
