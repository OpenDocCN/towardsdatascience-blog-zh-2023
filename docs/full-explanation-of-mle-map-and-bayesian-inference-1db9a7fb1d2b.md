# MLE、MAP 和贝叶斯推断的全面解释

> 原文：[`towardsdatascience.com/full-explanation-of-mle-map-and-bayesian-inference-1db9a7fb1d2b`](https://towardsdatascience.com/full-explanation-of-mle-map-and-bayesian-inference-1db9a7fb1d2b)

## 介绍最大似然估计、最大后验估计和贝叶斯推断

[](https://medium.com/@hrmnmichaels?source=post_page-----1db9a7fb1d2b--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----1db9a7fb1d2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1db9a7fb1d2b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1db9a7fb1d2b--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----1db9a7fb1d2b--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1db9a7fb1d2b--------------------------------) ·阅读时间 13 分钟·2023 年 3 月 6 日

--

![](img/ed3fd52dd4249d2f6376d0b4e133f357.png)

图片来源于 [fabio](https://unsplash.com/@fabioha?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](https://unsplash.com/photos/oyXis2kALVg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇文章中，我们将介绍 *MLE（最大似然估计）*、*MAP（最大后验估计）* 和 *贝叶斯推断*——这些概念对统计学、数据科学和机器学习等领域至关重要。我们将使用一个不公平的硬币抛掷的例子来解释每种方法，分析并数字推导（对于贝叶斯推断），并展示它们之间的差异。

我们将了解到 MLE 最大化了似然——即选择最大化观察数据似然的参数。MAP 添加了先验，引入了对参数的先验知识——从而弥合了从纯粹的频率派概念到贝叶斯概念的差距 (link). 贝叶斯推断最终给我们提供了最完整的信息，但也是最难执行的：它涉及建模给定数据的参数的完整后验分布——而之前的方法只是给出点估计。

让我们通过正式化这些描述来设定背景：本质上，对于任何类型的学习问题，我们都希望找到一个模型/参数，以尽可能好地描述观察到的数据。我们可以通过找到从观察数据到参数的映射/分布来完全描述和解决这个问题：

![](img/c65eadcada2478c53f78c81b1eedc2ff.png)

通常，这些术语被称为：

![](img/5be7854aafd96a21566940d45d750248.png)

给定数据的参数的[条件分布](https://en.wikipedia.org/wiki/Conditional_probability_distribution)称为后验分布——这是我们关注的术语。似然函数是给定特定参数值的数据的条件分布。这——正如其名称所示——描述了在给定选择的参数值下观察到数据的可能性。先验分布描述了我们对参数的信念——即我们在观察到数据之前对它们的预期。最后，证据是一个归一化常数，使得后验分布成为一个“真实”的概率分布。它完全描述了给定的数据，并且通常很难计算或甚至不可计算——这是贝叶斯推断常常困难的主要原因之一。

利用这些见解和公式/符号，我们将介绍最大似然估计（MLE）、最大后验估计（MAP）和贝叶斯推断。但首先，让我们介绍一个我们将在整个演示中使用的问题。

# 玩具问题：估计不公平硬币的参数

为了展示这些概念，我们将使用抛掷不公平硬币的例子：假设，我们给定一枚硬币，其正面出现的概率为`θ`，反面出现的概率为`1 — θ`。`θ`对我们来说是未知的，我们希望在实验后估计它。也就是说，我们假设我们拥有或收集了一个抛币的数据集，然后希望使用 MLE、MAP 和贝叶斯推断来为`θ`找到一个估计值。

在继续之前，让我们描述一个我们在所有后续部分中需要的量：似然函数。抛掷硬币遵循[伯努利分布](https://en.wikipedia.org/wiki/Bernoulli_distribution)。假设我们抛掷硬币`N`次，则用`Nₕ`表示正面的次数，用`Nₜ`表示反面的次数。

对于此（以及接下来的内容）的一个核心假设是独立性：我们假设，所有的抛币结果是相互独立且同分布的（i.i.d）——这使我们能够将似然函数表示为各个独立似然函数的乘积。特别地，我们得到：

![](img/f64f63d61b1391ccddc71b6b95540910.png)

代入伯努利分布得出：

![](img/9ab25af6471c0716c7722bb75698937b.png)

在这里，我们用一个值为 1 的二元变量表示单次事件（抛硬币）的正面，值为 0 表示反面。

由于对数函数是单调的，并且处理求和往往更为简便，我们应用这一点并重新公式化：

![](img/d6c245ed915fb25eccac9b24f3770f56.png)

## 数据集

在继续之前，让我们定义一个我们通过抛掷这枚不公平硬币观察到的样本数据集——我们将在接下来的部分中使用它来展示不同方法的结果。

我们将假设我们抛掷了一枚不公平的硬币，真实的`θ = 0.3`进行了 100 次实验，并观察到 36 次正面和 64 次反面。

# 最大似然估计（MLE）

现在，让我们来看看我们的第一个感兴趣的概念：MLE。在这里，我们要找到使观察数据的似然最大化的参数集。为此，我们形成似然函数，对参数求导并解根。注意，即使你以前可能不知道这个名称，你很可能已经应用过这个概念——这实际上是大多数机器学习算法的核心。想象一个神经网络来解决回归或分类问题：L2 损失或交叉熵可以被解释为似然，而梯度下降则找到最优解。

因此——让我们取上述似然公式，并找到使其最大化的`θ`——即求导并将其设为 0：

![](img/e4f4e574d0166c9e5556244c01508d31.png)

解 0 和一些较小的重构给出了`θ`的 MLE 估计：

![](img/f4039511cb85dc16f027f5f8637d4f0f.png)

这也应该从直观上讲得通：参数`θ`，即硬币正面朝上的概率，在没有其他先验信息或约束的情况下，应该等于观察到的正面结果与总投掷次数的比例。

让我们将此应用于上述介绍的数据集。我们得到：

![](img/fd2467fd2930d5b91345f6abd8e8f8b3.png)

# 最大后验估计（MAP）

MLE 代表最大化似然——找到解释数据最好的参数值集合，而无需任何先验信息或进一步的约束。现在，我们转向 MAP——这涉及引入一个参数的先验——实际上等同于寻找后验分布的最大值。

MLE 和 MAP 是频率派与贝叶斯概念之间讨论的良好示例。频率派认为概率只是观察随机、可重复事件的结果——而贝叶斯观点则将一切，包括参数，建模为随机变量——这些变量可以根据新证据进行更新。虽然贝叶斯概念（在我看来）通常提供更强大的解决方案，但一个核心的频率派批评是其对先验的需求——这些先验必须来自某处，并且可能是错误的。

进一步说明，将正则化添加到机器学习模型中实际上可以被证明等同于最大后验估计（MAP）（除了上述声明，即没有正则化的模型代表最大似然估计（MLE）概念）。但这一有趣的认识在这里过于深入，我们将专门在未来的文章中讨论。

让我们重新审视引言公式：

![](img/c65eadcada2478c53f78c81b1eedc2ff.png)

由于`p(x)`是一个常数，并且独立于`θ`，在对`θ`求导并解 0 时，它会消失。因此，MAP 确实给出了最大化后验分布的`θ`点估计。我们需要的只是一个先验。我们选择一个[贝塔分布](https://en.wikipedia.org/wiki/Beta_distribution)，其给定`θ`的概率密度函数由以下特征描述：

![](img/0c62cfbf8d9215d6a627c3622a76231d.png)

我们选择 beta 分布的原因将在下一节讨论（提示：共轭先验）。

为简化起见，我们再次应用对数运算，得到以下问题：

![](img/112cde29aea31a116f7f2cbd9af1b03e.png)

因此，在形成导数并解出 0 时，我们可以分别考虑每个加数——左侧的我们已经在上面计算过：

![](img/e4f4e574d0166c9e5556244c01508d31.png)

现在让我们来看右侧的图像：

![](img/db603282f037a3cd6b3892689a1a0c69.png)

形成导数得到：

![](img/b8e3c150cb5a8372ace1cee33bd3e535.png)

我们现在可以将这两个项结合起来，并解出 0：

![](img/59c982b02c48de555279b8d869d76bf2.png)

一些重构返回 `θ` 的映射估计：

![](img/9c94ba790ec5e60d9a20acfebe93be8c.png)

要应用这个公式，我们首先必须制定我们的先验。假设我们模糊地听说（例如来自硬币制造商）真实的正面概率应该在 0.3 左右，但我们并不确定。

为了建模这个信念，让我们选择一个 `α = 4` 和 `β = 10` 的 beta 分布。我们可以通过以下方式绘制这个分布：

```py
import matplotlib.pyplot as plt
from scipy.stats import beta

x = np.linspace(0, 1, 100)
plt.plot(x, beta.pdf(x, 4, 10))
plt.show()
```

得到以下图形：

![](img/ccd6f0a79cb6d01f122d4bb454428f60.png)

作者生成的图像

使用这个先验，我们得到：

![](img/d17ce3327363c4e1f9c51b00384310f4.png)

# 贝叶斯推断

现在，让我们来进行完全的贝叶斯推断，求解完整的后验分布，而不是参数的点估计：也就是说，我们将获得一个以观察数据为条件的参数的概率分布。

首先我们来检查分子：

![](img/a6663642b39188bbad3ce268d171f23c.png)

插入之前介绍的分布和结果，我们得到：

![](img/22eaa87581641ec45f3423e9e24d9ed9.png)![](img/4f99a0fc97dc3b769be626427f6f5769.png)

这引出了之前提到的[共轭先验](https://en.wikipedia.org/wiki/Conjugate_prior)的概念：如果结果分布与先验分布属于同一家族，那么这个先验被称为对似然的共轭先验——这也意味着后验分布也属于同一家族，我们稍后将看到。

现在让我们来看看证据，即分母。这通常是棘手的部分，且往往是不可解的，因为它涉及到对所有可能的参数值进行边际化：

![](img/2998bb7cb8f6326eb19ecc79a05648c6.png)

这个积分通常很难计算，甚至不可解——这是(精确)贝叶斯推断困难或不可解的主要原因之一。然而，正如我们稍后将看到的，通常我们不会尝试解析解，而是 resort to 各种近似技术，这些技术也能产生令人满意的结果。

在我们的案例中，虽然存在一个解析的闭式解。观察积分，我们发现其内部与上述计算是相同的：

![](img/2f9db0feac1732945eff58253500e9f0.png)

根据我们之前巧妙忽略的[Beta 函数](https://en.wikipedia.org/wiki/Beta_function)的定义，我们观察到这实际上是 Beta 函数在`Nₕ+α-1`，`N-Nₕ-1`的值：

![](img/69200c955e699f32ee6b6b358453ea70.png)

现在我们可以将分子和分母放在一起，得到后验的闭式解：

![](img/1a708ca4c2ddbfb69414f9441335a891.png)

让我们看看这个分布的样子，保持我们之前的 beta 先验`α = 4`和`β = 10`：

![](img/8c03edefd7388239f692bb897225516a.png)

图像由作者生成

利用关于[Beta 分布](https://en.wikipedia.org/wiki/Beta_distribution)的现有知识，一个遵循 beta 分布的随机变量`X`的均值由参数`α`和`β`给出：

![](img/7a49ea2f1d21132c7fc811d5e8474d5c.png)

在我们的情况下，这计算结果与 MAP 得到的结果相同，即 0.348。

对于方差，我们得到：

![](img/9c986d91e585e09cfd41524105f2259e.png)

# 结果讨论

在本节中，我们将讨论并比较前几节中获得的结果。

**MLE**估计`θ`，即硬币正面朝上的概率为 0.36——这正是观察到的正面投掷的相对频率（36 / 100）。这很有道理。在没有其他信息、先验知识的情况下……最能解释数据的参数应为那个反映数据的参数——在我们的案例中为 0.36。

我们的**MAP**估计为 0.348，这更接近真实值 0.3，并且更接近于先验的模式。这里的最后一点就是造成这种情况的原因：由于我们的先验围绕 0.3 中心，且方差相对较小，这在最终结果中得到了反映。

为了观察这种效果，考虑一个方差更大的先验，例如由`α = 2`和`β = 3`给出：

![](img/b8f2f094764ce0c8ac1e713085298f86.png)

图像由作者生成

在这种情况下，我们的 MAP 估计变为 0.359——这更接近 MLE 值，因为它受到先验的影响较小。

**贝叶斯推断**返回了完整的后验分布。其模式为 0.348，即与 MAP 估计相同。这是预期之中的，因为 MAP 只是后验分布的点估计解。然而，拥有完整的后验分布能让我们对问题有更多的洞察——我们将在接下来的两节中深入探讨。

# 后验分布的数值近似

对于这个例子，我们成功找到后验的闭式解，从而分析性地解决了贝叶斯推断问题。然而，正如之前所述，这通常很困难甚至不可行。幸运的是，在实际中，我们不需要，也通常不会尝试分析性地解决这个问题——而是 resort to 一些近似方法。

这里有几种可能性，我们使用了[使用 MCMC 方法的数值近似](https://medium.com/towards-data-science/introduction-to-markov-chain-monte-carlo-mcmc-methods-b5bad18bc243)。在链接的文章中，我详细介绍了这一点，因此我在此建议参考更多细节。在这里，我们只是简要总结核心概念：MCMC 方法通过定义一个相对简单的马尔可夫链来工作，从中采样的目标分布是其平稳分布。然后我们跟随这个马尔可夫链，生成`N`个相关的数据样本——由于平稳特性，这等同于从目标分布中采样。

这里我们现在想应用这个原则，特别是 Metropolis-Hastings 算法，来近似我们之前解析得到的后验分布。

我们可以使用以下代码片段来完成：

```py
import matplotlib.pyplot as plt
import numpy as np
import scipy.stats

THETA_TRUE = 0.3  # True probability for landing heads
# Parameters defining the beta prior distribution
ALPHA_PRIOR = 4
BETA_PRIOR = 10
NUM_SAMPLES = 100000  # Number of MCMC steps

# Fake a dataset which equals the one assumed in the previous sections
D = np.asarray([1] * 36 + [0] * 64)

# Define prior distribution
prior = scipy.stats.beta(ALPHA_PRIOR, BETA_PRIOR)

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

# Plot the sampled posterior distribution
plt.hist(samples, density=True, bins=100)
# Plot the posterior distribution obtained by the analytical solution
x_values = np.linspace(0, 1, 100)
plt.plot(x_values, scipy.stats.beta.pdf(x_values, 36 + ALPHA_PRIOR, 100 - 36 + BETA_PRIOR))

plt.show()
```

最后两行绘制了采样后验分布（直方图）与计算后验（线）——我们观察到预期的重叠：

![](img/2eabc24e462943bbf41835ca2a457030.png)

由作者生成的图像

# 为什么要做贝叶斯推断？

到目前为止，你可能会问这样一个问题：所有展示的方法都给我相同（或非常相似）的后验分布模式——导致我最终总是选择相同的参数。那么为什么还要做这种复杂的贝叶斯推断呢？

在最后一节中，我们将回答这个问题。我们首先解释这带来了什么好处，然后用一个实际的例子来展示这一点。

首先：做贝叶斯推断给了我们更多的信息。我们不仅获得了后验分布的模式，还获得了完整的分布。因此，我们可以检查它，计算其他矩（例如方差），并总体上更好地理解问题。特别是，通过这点我们能够感知不确定性，还可以决定是否拒绝我们解释数据的假设。让我们通过一个例子来演示这一点。

假设我们把硬币分析变成一个“游戏”：我们有两个硬币，必须选择那个公平的，即出现 50:50 的硬币。

游戏节目主持人给你呈现了两个硬币：

+   硬币 1 被抛掷了 8 次，其中 4 次出现了正面。

+   硬币 2 被抛掷了 100 次，其中 50 次出现了正面。

初看之下，两者似乎都有大约 50%的概率出现正面。然而，直观上大多数人肯定会选择硬币 2——因为这里我们有一个更大的样本量。聪明的你迅速在脑海中应用了最大后验法。你选择了一个`α = β = 2`的贝塔分布，为你提供了一个在[0, 1]区间上对称分布，模式为 0.5。

让我们做数学计算并计算`θ`的 MAP 估计：

+   `θ₁` = (4+2-1)/(8+2+2–2)=0.5

+   `θ₂`= (50+2–1)/(100+2+2–2)=0.5

因此，根据 MAP 方法，两枚硬币的结果完全相同！

让我们绘制贝叶斯推断上得到的两个硬币的完整后验分布：

![](img/d51ac655967c1610b315ee4a5e08c33c.png)

作者生成的图片

现在，根据我们的预期，硬币 2 的后验方差要低得多——这证明我们应该选择它！

# 结论

在这篇文章中，我们介绍了 MLE（最大似然估计）、MAP（最大后验估计）和贝叶斯推断。我们用一个不公平的硬币作为例子进行演示。

MLE 寻找优化似然的参数。MAP 引入了参数的先验分布，返回最大化完整后验的参数。因此，MLE 和 MAP 都返回点估计。相比之下，贝叶斯推断对完整后验分布进行建模。这通常是一个复杂的任务——但也更强大，因为我们可以深入了解这个分布，例如方差。

本文到此结束。感谢阅读！

备注：

+   所有图片，除非另有说明，均由作者生成。

+   本文中的计算示例部分受到[这个很棒的教程](https://engineering.purdue.edu/kak/Trinity.pdf)的启发。
