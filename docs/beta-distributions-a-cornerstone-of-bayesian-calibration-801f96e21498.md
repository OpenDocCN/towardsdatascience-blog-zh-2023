# Beta 分布：贝叶斯标定的基石

> 原文：[`towardsdatascience.com/beta-distributions-a-cornerstone-of-bayesian-calibration-801f96e21498`](https://towardsdatascience.com/beta-distributions-a-cornerstone-of-bayesian-calibration-801f96e21498)

## 探索 Beta 分布在贝叶斯推断中的多样性

[](https://medium.com/@MahamsMultiverse?source=post_page-----801f96e21498--------------------------------)![Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----801f96e21498--------------------------------)[](https://towardsdatascience.com/?source=post_page-----801f96e21498--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----801f96e21498--------------------------------) [Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----801f96e21498--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----801f96e21498--------------------------------) ·阅读时间约 9 分钟·2023 年 10 月 28 日

--

![](img/fc1f75d3aabe4b9031947d45a476263e.png)

图片由 [Google DeepMind](https://unsplash.com/@googledeepmind?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/6Y4EzfSP5Tc?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

嗨，大家好！

分布乍看起来可能并不是一个复杂的概念，但它们在数据分析和统计领域中却非常强大和基础。这样想：如果你收集了 50 件不同尺码和颜色的衬衫，你将创建一个颜色分布，一个尺码分布，甚至可能还有一个“这件衬衫有多让你烦”分布（当然这是开玩笑）。重点是，只要你有一个可以测量的类别，就有一个分布等待被探索。

那么，什么是分布呢？本质上，它是一种展示某个类别如何在概率或可能性范围内分布的方式。你可以从你拥有的数据或你对特定主题的了解来搞清楚这一点。你可能听说过像正态分布、偏斜分布、长尾分布等术语——这些术语描述了数据点的形态。

今天我想谈谈 Beta 分布，特别是它在贝叶斯标定中的应用。贝叶斯标定是一种通过新数据更新贝叶斯推断的方法，以找到模型参数的最佳拟合值。它考虑了关于这些参数的先验信息和给定这些参数时观察到的数据的可能性。

在我们深入探讨贝塔分布的贝叶斯校准之前，让我们先了解一些技术细节。掌握了这些基础知识后，我们将通过一个有趣的场景来探讨贝叶斯校准与贝塔分布。

# 贝塔分布

[贝塔分布](https://ocw.mit.edu/courses/18-05-introduction-to-probability-and-statistics-spring-2014/d6d722fef1c4f36cf2525bfe0b4f905a_MIT18_05S14_Reading14a.pdf)，记作 Beta(α, β)，是一个由两个参数定义的概率分布。其概率密度函数（pdf）表达如下：

![](img/f3eba5f4da479701085dac5897fe4efc.png)

作者提供的图像

在这个方程中，α和β代表超参数，重要的是要注意它们必须始终大于 0。此外，本文将专注于整数值。

在我们开始之前，让我们添加一个视觉辅助工具，查看一系列不同的贝塔分布概率密度函数（PDF），其α和β从 0.5 到 50 不等。

![](img/b2fef7ab366e9285990c962c8f2ded3f.png)

作者提供的图像

现在我们对贝塔分布有了一个清晰的了解，让我们进入一个具体的场景。

## 我们的场景

我们虚构的公司 MM 制造业，以生产精密权重而闻名。他们的系统一流，确保大多数产品的校准几乎完美。然而，最近出现了一个不寻常的问题——客户对权重不完美的投诉激增。对此，MM 制造业引入了一个额外的人为验证层，以确保每个发货给客户的权重都完美无瑕。

但为了分析实际生产权重的趋势，他们要求数据科学团队分析遇到这些不规则权重的可能性，更重要的是监测这些情况的分布，以便获得改善性能的洞察。幸运的是，所有的权重都在传送带上记录了其准确值。

数据科学团队的方法根植于贝叶斯校准。每个月，他们都会更新一个贝塔分布概率密度函数（pdf）以评估权重的异常情况及其随时间的变化。为此，他们使用传送带上的数据，作为观察值来最终确定后验分布。他们还需要建立一个先验分布，这可以基于历史数据、领域知识，或一个非特定的、非信息性的分布。

对于可观察数据或似然估计中的贝塔分布的α（α）和β（β）值，他们采用以下策略：

α = 正确校准的权重数量 + 1（确保α > 0）

β = 校准错误的权重数量 + 1（确保β > 0）

至于他们选择的先验，他们最初选择一个无信息量的先验，表示为 Beta(1,1)（如下图所示的均匀分布），这最小化了先验对后验的影响，并主要依赖于观察数据。

![](img/2cb1b2814df4f58acb4d24d5f9ffdd80.png)

作者提供的图片

可能值得稍微偏离一下，说明先验在此背景下的作用

## 先验的作用

在贝叶斯推断的领域，先验是你可以融入你观点的地方——虽然不是你的意见，而是你在先前观察的基础上形成的知情观点。它有各种形式，从高度信息量的到完全无信息量的，它在塑造后验分布中扮演了关键角色。

在我们的贝叶斯校准过程中，后验分布受似然和先验的比例影响。

后验分布 ∝ 似然 × 先验

此外，Beta 分布作为贝叶斯推断中许多分布的 [共轭先验](https://www.statlect.com/fundamentals-of-statistics/conjugate-prior)。这意味着，如果你处理像 [伯努利](https://en.wikipedia.org/wiki/Bernoulli_distribution)、[二项分布](https://en.wikipedia.org/wiki/Binomial_distribution)、[负二项分布](https://en.wikipedia.org/wiki/Negative_binomial_distribution) 和 [几何分布](https://en.wikipedia.org/wiki/Geometric_distribution) 这样的分布，对于似然函数，结果的后验也将属于 Beta 分布家族。在我们的案例中，异常权重的似然分布基于类似于二项分布的成功失败场景。

现在，让我们探索先验的选项，考虑分布的性质：

## 无信息量先验

无信息量先验对后验的影响最小，适用于你对分布的外观几乎没有信息的情况。在 Beta 分布中，无信息量先验的例子包括：

1.  Beta(0.5, 0.5) 或 [Jeffreys 先验](https://www.statisticshowto.com/jeffreys-prior/)

1.  Beta(1, 1) 或均匀先验。

当你希望似然成为主导因素，而先验的影响较小时，这种选择是理想的。

## 轻度信息量先验

轻度信息量先验向后验传达了一些信息。在 Beta 分布中，轻度信息量先验的选项包括 Beta(2, 2) 和 Beta(1, 2)。

这些先验基于部分知识对后验提供了一定的提示。

## 信息量先验

当你拥有大量关于分布的信息，并希望基于新的观察进行细微调整时，信息量先验发挥作用。在 Beta 分布的背景下，信息量先验可以是 Beta(10, 10) 和 Beta(20, 2) 等较大值。这些先验在塑造后验方面具有更大的权重。

在更好地理解不同类型的先验及其作用后，让我们回到特定场景，将 MM Manufacturing 的异常重量映射到可观察的后验分布中。

# Python 实现

所以让我们使用 Beta 分布先验和贝叶斯校准进行一些异常检测，以便更清晰地理解这一概念。

首先，为了模拟输送带产生的重量，我们将为下面两个场景各生成 500 个数据点的合成数据。

## 场景 1：第一次贝叶斯校准

对于第一次校准，我们使用了一个非信息先验，记作 Beta(1,1)。我们定义了似然 Beta(α, β)，其中 α 和 β 为：

α = 正确校准的重量 + 1（因为 α 应该大于 0）

β = 错误校准的重量 + 1（同样，为了没有事件，β > 0）

我们还生成了合成数据，其中如果重量在 4.85 到 5.15 之间（含）则视为正确校准的 5 磅重量，如果重量超出这些值则视为校准不正确。

我们最初生成了 10% 异常值的数据。

```py
import random
import matplotlib.pyplot as plt
from scipy.stats import beta

# Simulated data: 500 observations with 90% normal weight and 10% anomalous weights
np.random.seed(42)
normal_instances =  [random.uniform(4.85, 5.15) for i in range(450)]
anomalous_instances_1 =  [random.uniform(3, 4.85) for i in range(25)]
anomalous_instances_2 =  [random.uniform(5.15, 6) for i in range(25)]

data = np.concatenate((normal_instances, anomalous_instances_1, anomalous_instances_2))

# Initial prior belief using a Beta distribution (uninformative uniform prior)
prior_alpha = 1
prior_beta = 1

# Beta Distribution as inferred by Observed data 
likelihood_alpha = len(data[(data >= 4.85) & (data <= 5.15)]) + 1
likelihood_beta = len(data) - likelihood_alpha + 1

# Calculate posterior parameters based on observed data and prior
posterior_alpha = prior_alpha + likelihood_alpha
posterior_beta = prior_beta + likelihood_beta

# Plot the prior, likelihood and posterior Beta distributions
x = np.linspace(0, 1, 1000)
prior_distribution = beta.pdf(x, prior_alpha, prior_beta)
likelihood_distribution = beta.pdf(x, likelihood_alpha, likelihood_beta)
posterior_distribution = beta.pdf(x, posterior_alpha, posterior_beta)

plt.plot(x, prior_distribution, label='Prior Distribution')
plt.plot(x, likelihood_distribution, label='Likelihood Distribution')
plt.plot(x, posterior_distribution, label='Posterior Distribution')

plt.title('Bayesian Calibration for Anomalous Weight Detection')
plt.xlabel('Anomaly Probability')
plt.ylabel('Probability Density')
plt.legend()
plt.show()
```

如预期的那样，我们的后验几乎完全像似然一样，因此这次校准并不多。这也显示了均匀先验对后验的影响。

![](img/1eea00dde9ed48d8507bf0822cca77a2.png)

图片来源：作者

下个月我们有更多数据，现在我们的先验是上个月的后验，类似地，我们可能获得了一些内部系统的信息，并相应地调整了先验。

## 场景 2：贝叶斯校准更新

假设 MM Manufacturing 关注并对系统进行了某些更改，现在只有 6% 的重量是异常的。现在我们有了更具信息量的先验，基于我们之前的数据的后验。

```py
# Simulated data: 500 observations with 94% normal weight and 6% anomalous weights
np.random.seed(42)
normal_instances =  [random.uniform(4.85, 5.15) for i in range(470)]
anomalous_instances_1 =  [random.uniform(3, 4.85) for i in range(15)]
anomalous_instances_2 =  [random.uniform(5.15, 6) for i in range(15)]

data = np.concatenate((normal_instances, anomalous_instances_1, anomalous_instances_2))

# Initial prior belief about normal behavior using a Beta distribution
prior_alpha = posterior_alpha
prior_beta = posterior_beta

# Beta Distribution as inferred by Observed data 
likelihood_alpha = len(data[(data >= 4.85) & (data <= 5.15)]) + 1
likelihood_beta = len(data) - likelihood_alpha + 1

# Calculate posterior parameters based on observed data and prior
posterior_alpha = prior_alpha + likelihood_alpha
posterior_beta = prior_beta + likelihood_beta

# Plot the prior, likelihood and posterior Beta distributions
x = np.linspace(0, 1, 1000)
prior_distribution = beta.pdf(x, prior_alpha, prior_beta)
likelihood_distribution = beta.pdf(x, likelihood_alpha, likelihood_beta)
posterior_distribution = beta.pdf(x, posterior_alpha, posterior_beta)

plt.plot(x, prior_distribution, label='Prior Distribution')
plt.plot(x, likelihood_distribution, label='Likelihood Distribution')
plt.plot(x, posterior_distribution, label='Posterior Distribution')

plt.title('Bayesian Calibration for Anomalous Weight Detection')
plt.xlabel('Anomaly Probability')
plt.ylabel('Probability Density')
plt.legend()
plt.show()
```

这次我们看到了先验对后验的影响以及分布的定义程度。先验、后验和似然之间的关系在这里更为清晰可见。

![](img/199d8cbfc32c61600ea2107f294b9c00.png)

图片来源：作者

考虑到上述两种场景，很明显，这些结果的多样性可以被用来获取系统性能的洞察，进行比较以观察改进，并在广泛的时间范围内改进数据校准。

Beta 分布的吸引力在于其在定义各种分布中的适应性和多功能性，而贝叶斯校准的优势在于其灵活地接纳和整合复杂模型参数的能力。

让我们讨论一些其他应用。

## 其他应用

关于贝塔分布的讨论如果不提及其广泛的应用将是不完整的。它不仅仅用于贝叶斯推断和校准领域，如我们在成功-失败情境中所看到的。贝塔分布在 A/B 测试中也发挥着重要作用，它帮助建模不同网页或应用版本的转换率——这一情境类似于成功和失败，只是处于不同的背景中。

此外，贝塔分布在风险分析中也可以发挥作用，其中概率方法对于估计项目成功的可能性非常有帮助。

## 总结

总之，贝塔分布，特别是当应用于贝叶斯校准的背景下，是一个极其有价值且优雅的概念。它在处理模型的复杂性方面表现出色，同时提供了一种直观的决策方法。此外，它的相关性扩展到各个行业的广泛应用，在这些应用中，它在获得对系统校准过程中的性能的宝贵洞察方面发挥了关键作用。

贝塔分布不仅是一个理论概念；它是数据科学家工具箱中的一个实用而宝贵的资产。理解它的应用和适应性为增强决策和改善系统性能开辟了新的视角。随着你在数据科学领域的深入探索，请记住贝塔分布在处理复杂模型和在各个行业中获得宝贵洞察方面的重要性。

## 一种可视化贝塔分布的酷炫方式

[](https://mathlets.org/mathlets/beta-distribution/?source=post_page-----801f96e21498--------------------------------) [## 贝塔分布 - MIT Mathlets

### 贝塔分布依赖于两个参数。

mathlets.org](https://mathlets.org/mathlets/beta-distribution/?source=post_page-----801f96e21498--------------------------------)

## 不要忘记阅读我其他一些引人入胜的文章！

[](/p-values-understanding-statistical-significance-in-plain-language-41a00ff68f23?source=post_page-----801f96e21498--------------------------------) ## P 值：用通俗语言理解统计显著性

### 选择通往显著结果的路径

towardsdatascience.com [](/exploring-counterfactual-insights-from-correlation-to-causation-in-data-analysis-c3ee44d8e777?source=post_page-----801f96e21498--------------------------------) ## 探索反事实洞察：从相关性到因果性的数据分析

### 在数据科学中使用反事实进行知情决策

towardsdatascience.com

欢迎在评论中分享你的想法。
