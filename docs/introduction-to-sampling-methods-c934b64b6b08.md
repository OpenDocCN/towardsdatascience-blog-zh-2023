# 抽样方法介绍

> 原文：[`towardsdatascience.com/introduction-to-sampling-methods-c934b64b6b08`](https://towardsdatascience.com/introduction-to-sampling-methods-c934b64b6b08)

## 在 Python 中实现逆变换抽样、拒绝抽样和重要性抽样

[](https://medium.com/@hrmnmichaels?source=post_page-----c934b64b6b08--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----c934b64b6b08--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c934b64b6b08--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c934b64b6b08--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----c934b64b6b08--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c934b64b6b08--------------------------------) ·8 min 阅读·2023 年 1 月 10 日

--

![](img/08c4a7efaed06bcd1a42a2a74b87d3d5.png)

图片由[Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/uKlneQRwaxY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇文章中，我们将讨论如何从概率分布中抽样。这是一个常见的需求，在机器学习环境中，这种方法最常用于对概率模型进行推断。然而，由于分布非常复杂，这通常是不可行的。因此，本帖的主要重点是介绍该任务的近似方法，特别是使用数值抽样，称为[蒙特卡罗方法](https://en.wikipedia.org/wiki/Monte_Carlo_method)。

尽管如此，为了介绍目的，我们将首先介绍逆变换抽样，它允许对任意可处理的分布进行精确推断。然后，我们将把重点转向近似方法，允许对（接近）任意分布进行抽样或矩估计：我们从拒绝抽样开始，然后转向重要性抽样。

请注意，这篇文章是一个系列的第一篇，旨在帮助读者熟悉[1]，特别是本帖涵盖了第十一章：抽样方法的部分内容。

# 逆变换抽样

逆变换抽样方法允许从任何我们知道如何计算其[累积分布函数](https://en.wikipedia.org/wiki/Cumulative_distribution_function)（CDF）的分布中进行抽样。它包括从`U[0, 1]`（[均匀分布](https://en.wikipedia.org/wiki/Continuous_uniform_distribution)）中抽取`y`，然后计算所需的`x`，其计算公式如下：

![](img/847febd0b222b048f817600a7c1fcf3f.png)

即，我们正在生成一个随机变量

![](img/53302e119ead29be92d41521d43440b4.png)

然后声称

![](img/2ad2b65b0642631838162a6dd9a67d44.png)

— 即累计分布的逆（记作`F`）遵循我们期望的目标分布（更具体地说，是其[概率密度函数](https://en.wikipedia.org/wiki/Probability_density_function)，记作`f`）。

当且仅当 CDF 相同，即：

![](img/6ee56bc56eb144a9af14b132b6367fe2.png)

为了证明这一点，我们来检查左侧，并对方程两边应用 F：

![](img/f2856c3357f19bcec77a11fb75c7496e.png)

由于对[0, 1]上的均匀分布，我们有

![](img/9d1080b0df162d4a7c32904756fdf697.png)

我们得到，如所愿：

![](img/6ee56bc56eb144a9af14b132b6367fe2.png)

（证明取自[COMP 480 / 580](https://www.cs.rice.edu/~as143/COMP480_580_Spring20/scribe/lect14.pdf)）。

## Python 实现

让我们对[指数分布](https://en.wikipedia.org/wiki/Exponential_distribution)进行尝试，其定义为 pdf：

![](img/6b91eec309ea52b0c8db9def6bd14dde.png)

取 CDF 并进行反转得到：

![](img/0e982ba57732fbc9c7966afcafc4f947.png)

这就给出了我们想要的采样公式！让我们将其转换为 Python 代码。

我们首先定义一个函数`exp_distr`，用于评估给定`x`值的 pdf，然后定义一个函数`exp_distr_sampled`，用于通过逆变换方法和我们上面推导的公式从指数分布中采样。

最后，我们绘制真实的概率密度函数（pdf）和我们采样值的直方图：

```py
import numpy as np
import matplotlib.pyplot as plt

NUM_SAMPLES = 10000
RATE = 1.5

def exp_distr(x: np.ndarray, rate: float) -> np.ndarray:
    return rate * np.exp(-x * rate)

def exp_distr_sampled(num_samples: int, rate: float) -> np.ndarray:
    x = np.random.uniform(0, 1, num_samples)
    return -1 / rate * np.log(1 - x)

x = np.linspace(0, 5, NUM_SAMPLES)
y_true = exp_distr(x, RATE)
y_sampled = exp_distr_sampled(NUM_SAMPLES, RATE)

plt.plot(x, y_true, "r-")
plt.hist(y_sampled, bins=30, density=True)
plt.show()
```

运行此代码会生成类似以下的结果，显示我们的采样确实产生了正确的结果：

![](img/fddaafaffcf1e9b99387ace9f943c17e.png)

# 数值采样

如果可能，逆变换采样效果完美。然而，如前所述，它要求我们能够反转 CDF。这仅适用于某些更简单的分布——即使对于[正态分布](https://en.wikipedia.org/wiki/Normal_distribution)，这也显著复杂，尽管可能。当这太困难或不可能时——我们必须诉诸其他方法，我们将在本节中介绍这些方法。

我们首先进行拒绝采样，然后引入重要性采样，最后讨论这些方法的局限性和展望。

注意，两种方法仍然要求我们能够评估给定`x`的`p(x)`。

## 拒绝采样

拒绝采样涉及引入一个更简单的提议分布`q`，我们可以从中采样，并用它来近似`p`。它遵循的思想是`q(x) ≥ p(x)`对于所有`x`，我们从`q`中采样，但拒绝所有高于`p`的样本——从而最终得到分布`p`。

如上所述，虽然可以将逆变换采样应用于正态分布，但这很困难——因此为了演示，我们在这里尝试逼近一个正态分布（`p`），并使用均匀分布作为提议分布（`q`）。让我们在这个设置中可视化上述直觉：

![](img/e0e88ef5470cf875f12869feb5cacf84.png)

现在，通过拒绝采样，我们会从类似均匀分布的`q`中采样，并拒绝所有高于`p`的样本——因此在极限情况下，得到的点将完全填满`p`下方的区域，即从该分布中采样。

从形式上看，我们首先选择提议分布`q`，然后选择一个缩放系数`k`，使得`kq(x) ≥ p(x)`对所有`x`都成立。接下来，我们从`q`中采样一个值`x_0`。然后，我们从`[0, kq(x_0)]`上的均匀分布中生成一个值`u_0`。如果`u_0 > p(x_0)`，我们拒绝该样本，否则接受它。

让我们用 Python 实现这一点：

```py
from typing import Tuple

import matplotlib.pyplot as plt
import numpy as np

NUM_SAMPLES = 10000
MEAN = 0
STANDARD_DEVIATION = 1
MIN_X = -4
MAX_X = 4

def uniform_distr(low: float, high: float) -> float:
    return 1 / (high - low)

def normal_dist(
    x: np.ndarray, mean: float, standard_deviation: float
) -> np.ndarray:
    return (
        1
        / (standard_deviation * np.sqrt(2 * np.pi))
        * np.exp(-0.5 * ((x - mean) / standard_deviation) ** 2)
    )

x = np.linspace(MIN_X, MAX_X, NUM_SAMPLES)
y_true = normal_dist(x, MEAN, STANDARD_DEVIATION)

def rejection_sampling(
    num_samples: int, min_x: float, max_x: float
) -> Tuple[np.ndarray, float]:
    x = np.random.uniform(min_x, max_x, num_samples)
    # uniform_distr(-4, 4) = 0.125 -> we need to scale this to ~0.5, i.e.
    # select k = 4.
    k = 4
    u = np.random.uniform(np.zeros_like(x), uniform_distr(min_x, max_x) * k)
    (idx,) = np.where(u < normal_dist(x, MEAN, STANDARD_DEVIATION))
    return x[idx], len(idx) / num_samples

y_sampled, acceptance_prob = rejection_sampling(NUM_SAMPLES * 10, MIN_X, MAX_X)
print(f"Acceptance probability: {acceptance_prob}")
plt.plot(x, y_true, "r-")
plt.hist(y_sampled, bins=30, density=True)
plt.show()
```

得到以下结果：

![](img/b4ee5a140b3367d938abbde4c0d1c15d.png)

`接受概率：0.24774`

## 重要性采样

通常，我们只对期望感兴趣，这也是重要性采样发挥作用的地方：它是一种通过再次使用提议分布`q`来逼近复杂分布`p`的期望（或其他矩）的技术。

一个（连续）随机变量 X 的期望定义为：

![](img/bfa4ae4493bf6718076cac4cf4253643.png)

然后我们使用一个小数学技巧将其重新表达为：

![](img/461958fa0473f0f7ec2f1d80816db530.png)

那么这个公式说明了什么？这是

![](img/31ebb2940253e9718a0e244544be8e5a.png)

在`q`下——即为了计算`E[X]`，我们现在可以在从`q`采样时评估这个项！系数`p(x) / q(x)`被称为重要性权重。

为了演示，我们再次将`p`设为正态分布——并使用另一种正态分布作为`q`：

```py
import numpy as np

NUM_SAMPLES = 10000
MEAN_P = 3
MEAN_Q = 2

def normal_dist(
    x: np.ndarray, mean: float, standard_deviation: float
) -> np.ndarray:
    return (
        1
        / (standard_deviation * np.sqrt(2 * np.pi))
        * np.exp(-0.5 * ((x - mean) / standard_deviation) ** 2)
    )

q_sampled = np.random.normal(loc=MEAN_Q, size=NUM_SAMPLES)
p_sampled = (
    q_sampled
    * normal_dist(q_sampled, MEAN_P, 1)
    / normal_dist(q_sampled, MEAN_Q, 1)
)
print(
    f"Resulting expecation when sampling from q {np.mean(p_sampled)} vs true expecation {MEAN_P}"
)
```

在代码中，我们将`p`设为均值为 3 的正态分布。然后，我们从提议分布`q`中采样`NUM_SAMPLES`，该分布是均值为 2 的正态分布——并利用上述介绍的变换来计算在`p`下的`X`的期望——得到的结果大约是正确的（~3 对比 3）。

让我们以对方差的讨论结束这一部分：结果样本的方差将是

![](img/540e85cd55f614df34e5f95def51cca5.png)

对于我们的情况，这意味着方差会随着`p`和`q`之间的差异（即不一致）增加。为了演示，比较`MEAN_Q = 3.2`和`5`的方差，我们分别得到~0.20 和~91.71。

不过需要注意的是，重要性采样也常作为方差降低技术，通过巧妙选择`q`来实现——然而这可能是未来文章的主题。

# 限制与展望

在这篇文章中，我们介绍了三种采样方法：逆变换采样、拒绝采样和重要性采样。

逆变换抽样可以用于相对简单的分布，对这些分布我们知道如何反转累积分布函数（CDF）。

对于更复杂的分布，我们必须求助于拒绝或重要性抽样。然而，对于这两种方法，我们仍需要能够评估所讨论分布的概率密度函数（pdf）。此外，还有其他缺点，例如：当我们不能用`kq`适当地“框住”`p`时，拒绝抽样是浪费的——在高维空间中尤其棘手。重要性抽样也是如此——特别是在高维空间中——很难找到适合的提议分布`q`以及合适的重要性权重。

如果上述提到的缺点过于严重，我们必须求助于更强大的方法——例如来自[马尔可夫链蒙特卡洛方法](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo)（MCMC）家族。这些方法对我们想要近似的分布的要求要宽松得多，并且受到的限制也较少，例如在高维空间中。

这篇文章结束了对抽样方法的介绍。请注意，所有图片除非另有说明，均由作者提供。希望你喜欢这篇文章，谢谢阅读！

本文是关于抽样的系列的第一部分。你可以在这里找到其他部分：

+   第二部分: 利用重要性抽样进行方差减少

+   第三部分: [马尔可夫链蒙特卡洛（MCMC）方法简介](https://medium.com/towards-data-science/introduction-to-markov-chain-monte-carlo-mcmc-methods-b5bad18bc243)

**参考文献：**

[1] Bishop, Christopher M., “模式识别与机器学习”，2006，[`www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf`](https://www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf)
