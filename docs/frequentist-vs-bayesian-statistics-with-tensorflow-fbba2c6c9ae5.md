# 频率学派与贝叶斯统计学的比较

> 原文：[`towardsdatascience.com/frequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5`](https://towardsdatascience.com/frequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5)

## 概率深度学习

[](https://medium.com/@luisroque?source=post_page-----fbba2c6c9ae5--------------------------------)![路易斯·罗克](https://medium.com/@luisroque?source=post_page-----fbba2c6c9ae5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fbba2c6c9ae5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fbba2c6c9ae5--------------------------------) [路易斯·罗克](https://medium.com/@luisroque?source=post_page-----fbba2c6c9ae5--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fbba2c6c9ae5--------------------------------) ·阅读时长 10 分钟·2023 年 1 月 5 日

--

# 介绍

本文属于“**概率深度学习**”系列。该系列每周讨论概率方法在深度学习中的应用。主要目标是扩展深度学习模型以量化不确定性，即了解它们不知道什么。

频率学派统计方法基于重复采样和长期相对频率的理念。它涉及构建关于总体的假设，并使用样本数据对其进行检验。另一方面，贝叶斯方法基于主观概率，并涉及使用收集到的数据更新对总体的初步信念。这两种方法各有优缺点，选择使用哪一种取决于问题和分析目标。在本文中，我们将深入探讨频率学派与贝叶斯方法之间的差异，并讨论如何利用 TensorFlow 和 TensorFlow Probability 实现这两种方法。

迄今为止发布的文章：

1.  [TensorFlow 概率论的温和介绍：分布对象](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-distribution-objects-1bb6165abee1)

1.  [TensorFlow 概率论的温和介绍：可训练参数](https://medium.com/towards-data-science/gentle-introduction-to-tensorflow-probability-trainable-parameters-5098ea4fed15)

1.  从头开始在 TensorFlow 概率论中实现最大似然估计

1.  从头开始在 TensorFlow 中实现概率线性回归

1.  [TensorFlow 中的概率回归与确定性回归](https://medium.com/towards-data-science/probabilistic-vs-deterministic-regression-with-tensorflow-85ef791beeef)

1.  [使用 TensorFlow 进行频率学与贝叶斯统计比较](https://medium.com/towards-data-science/frequentist-vs-bayesian-statistics-with-tensorflow-fbba2c6c9ae5)

![](img/a4570c7be657d4edef2f441ce79f6f04.png)

图 1：今天的座右铭：看待概率的方式没有唯一答案（[来源](https://unsplash.com/photos/V3qzwMY2ak0)）。

我们使用 TensorFlow 和 TensorFlow Probability 来开发我们的模型。TensorFlow Probability 是一个基于 TensorFlow 的 Python 库。我们将从 TensorFlow Probability 中可以找到的基本对象开始，并理解如何操作它们。接下来的几周我们将逐步增加复杂性，并将我们的概率模型与现代硬件（例如 GPU）上的深度学习结合起来。

如常，代码可以在我的[GitHub](https://github.com/luisroque/probabilistic_deep_learning_with_TFP)上找到。

# 频率学与贝叶斯方法的比较

频率学统计和贝叶斯统计是统计推断的两种主要方法，统计推断是利用数据对总体得出结论的过程。这两种方法都用于估计未知量、进行预测和检验假设，但在概率的解释及如何融入先验知识和证据方面有所不同。

在频率学统计中，概率被解释为事件在无限次试验中的长期相对频率。这种方法基于一个观点，即总体参数的真实值是固定的，但未知且必须通过数据进行估计。在这个框架下，统计推断是通过对观察数据进行假设来从中得出的，使用点估计、置信区间和假设检验等技术。

另一方面，贝叶斯统计将概率解释为对事件的信念或确定性的程度。这种方法允许通过使用贝叶斯定理将先验知识和证据融入统计分析。在这个框架下，总体参数的真实值被视为随机变量，并随着新数据的收集而更新。这导致了一个关于参数空间的完整分布，称为后验分布，这可以用来进行概率预测并量化不确定性。

两种方法之间的一个关键区别是它们处理不确定性的方式。在频率学统计中，不确定性通过使用置信区间来量化，这些区间提供了基于观察数据的真实总体参数的可能范围的估计。在贝叶斯统计中，不确定性通过完整的后验分布来表示，这使得对参数真实值的不确定性进行更全面的描述成为可能。

另一个区别是贝叶斯统计允许结合先验知识，这在数据有限或数据生成过程复杂的情况下尤其有用。然而，先验分布的选择可以显著影响贝叶斯分析的结果，因此选择一个适合当前问题的先验非常重要。

# 问题

作为应用频率主义方法解决问题的一个例子，考虑基于样本数据估计总体收入均值的任务。在这种情况下，目标是使用样本数据推断总体的真实均值（我们暂时假设总体收入的标准差已知）。

让我们生成一些数据：

```py
import tensorflow as tf
import tensorflow_probability as tfp
import matplotlib.pyplot as plt
from scipy.stats import t
import numpy as np

tfd = tfp.distributions

# Sample data
sample_size = 30.
sample_mean = 50000.
sample_stddev = 10000.
sample_data = tfd.Normal(loc=sample_mean, scale=sample_stddev).sample(sample_size)

plt.hist(sample_data, density=True, alpha=0.5);
```

![](img/bb1676894082a75eb3e8d331a522c244.png)

图 2: 样本均值的直方图 — 合成生成的数据。

## 频率主义方法

使用频率主义方法解决此问题的一种方式是通过点估计。点估计涉及使用一个单一的点估计，例如样本均值，来代表未知的总体参数。

样本均值 *𝜇*̂ 是一个常用的总体均值 *𝜇* 的点估计。它计算为样本值的总和 *𝑥*1,*𝑥*2,…,*𝑥𝑛* 除以样本大小 *𝑛*：

![](img/e8683864dfca212802019de01e5c0e3d.png)

```py
sample_mean = tf.reduce_mean(sample_data)
sample_mean

<tf.Tensor: shape=(), dtype=float32, numpy=50212.75>
```

然而，仅凭点估计无法完整描述围绕估计值的不确定性。为了量化这种不确定性，我们可以使用置信区间。置信区间是基于观察数据对真实总体参数的可能范围的估计，通过从点估计中加减误差边际来构建。误差边际由所需的置信水平和样本大小决定，反映了样本数据的变异性。例如，95% 的置信区间表示我们有 95% 的信心真实总体参数落在该区间内。

总体均值的置信区间通过向点估计添加和减去误差边际 *𝑚* 来构建：

![](img/1660d16b740375d1eba7e5e97c1aa79f.png)

误差边际由所需的置信水平 1−*𝛼* 和样本大小 *𝑛* 决定。它反映了样本数据中的变异性，通常通过点估计的标准误差 *𝑆𝐸* 计算：

![](img/69cc8f472cbbd1706ff712d9cd3bf4b7.png)

其中 *𝑡*1−*𝛼*2,*𝑛*−1 是具有 *𝑛*−1 自由度的 *𝑡* 分布的临界值，用于所需的置信水平。

样本均值的标准误差计算为样本标准差 *𝜎* 除以样本大小的平方根：

![](img/aa7358e49e38f104231074c38e6eda34.png)

```py
# Standard error of the sample mean
sample_stddev = tf.math.reduce_std(sample_data)
standard_error = sample_stddev / tf.sqrt(sample_size)

# Margin of error
confidence_level = 0.95
degrees_of_freedom = sample_size - 1
t_distribution = tfp.distributions.StudentT(df=degrees_of_freedom, loc=0., scale=1.)

# t_distribution.quantile() seems to have a bug
t_value = t.ppf(confidence_level+(1-confidence_level)/2, df=sample_size-1)
margin_of_error = t_value * standard_error

confidence_interval_lower = sample_mean - margin_of_error
confidence_interval_upper = sample_mean + margin_of_error
confidence_interval = (confidence_interval_lower.numpy(), confidence_interval_upper.numpy())
confidence_interval

(46550.523, 53645.047)
```

我们可以定义一个辅助函数，用于绘制我们预测的均值和置信区间，并将其叠加在采样数据上。

```py
def visualize_output(sample_data, sample_mean, interval, type_interval):
    plt.hist(sample_data, density=True, alpha=0.5)
    plt.axvline(x=sample_mean, color='r', linestyle='dashed', linewidth=2)
    plt.axvline(x=interval[0], color='g', linewidth=2)
    plt.axvline(x=interval[1], color='g', linewidth=2)
    plt.legend(['Sample Mean', f'{type_interval} interval'])
    plt.show()

visualize_output(sample_data, sample_mean, confidence_interval, 'confidence')
```

![](img/8018f59799dbb1fe66945a8ded152fc7.png)

图 3：使用频率学派方法估计的参数的均值和置信区间。

## 贝叶斯方法

作为应用贝叶斯方法的一个示例，考虑基于数据样本估计总体平均收入的任务。在这种情况下，目标是利用样本数据和任何可用的先验知识，对总体的真实平均收入做出推断。

使用贝叶斯方法解决这个问题的一种方式是通过使用贝叶斯定理，该定理允许我们根据观察到的数据更新对总体平均收入值的信念。贝叶斯定理指出，给定某些数据的假设的后验概率等于假设的先验概率乘以给定假设的数据的似然度，再除以数据的边际概率。数学表达式为：

![](img/5104070b345c851739bee6f11763fb64.png)

其中*𝜃*是总体平均收入，*𝑥*是观察到的数据（即收入值的样本），*𝑃*(*𝜃*|*𝑥*), *𝑃*(*𝑥*|*𝜃*), *𝑃*(*𝜃*)和*𝑃*(*𝑥*)分别是后验概率、似然度、先验概率和边际概率。

要将这种方法应用于收入示例，我们必须首先指定一个关于总体平均收入的先验分布，*𝑃*(*𝜃*)。这个先验分布表示我们在观察到数据之前对总体平均收入值的信念。先验分布的选择将取决于任何可用的先验知识以及数据生成过程的特征。例如，如果我们有强有力的先验知识表明总体平均收入服从均值为 45,000 和标准差为 5,000 的正态分布，我们可以使用具有这些参数的正态先验分布。

```py
mu_prior = 40000.
sigma_prior = 5000.
prior = tfd.Normal(loc=mu_prior, scale=sigma_prior)
```

似然函数，*𝑃*(*𝑥*|*𝜃*)，表示在给定总体平均收入值*𝜃*的情况下观察到样本数据*𝑥*的概率。由于样本数据被假设为独立同分布（i.i.d.），似然函数仅为样本值的单个概率密度函数的乘积。例如，如果样本数据服从已知标准差为*𝜎*的正态分布，则似然函数是均值等于总体平均收入*𝜃*，标准差等于样本标准差的正态分布：

![](img/cdf307247325c7851dde09709c019f0d.png)

```py
mu_likelihood = np.mean(sample_data)
sigma_likelihood = np.std(sample_data)
likelihood = tfd.Normal(loc=np.mean(sample_data), scale=np.std(sample_data))
```

接下来，我们使用贝叶斯定理计算后验分布：

![](img/5a7e464737f73240911ed517243fcaa1.png)

后验分布可以用来进行概率预测，并量化围绕总体均值收入估计的不确定性。例如，我们可以利用后验分布来计算后验均值和后验标准差，分别作为对总体均值收入的估计以及对估计不确定性的量化。

如上所述，后验分布是通过两个高斯分布的乘积得出的。为了实现这一点，我们需要引入一个额外的概念——共轭分布。在我们的例子中，我们拥有正态-正态共轭族，这是一个参数化的分布族，其中先验分布和似然函数都是正态分布。这个模型类具有几个吸引人的特性，包括能够解析地计算后验分布以及最大后验（MAP）估计的封闭形式解。

后验分布被定义为给定观测数据*𝑦*的模型参数（在本例中为正态分布的均值*𝜇*和标准差*𝜎*）的分布。可以使用以下方程计算后验分布的均值和标准差：

![](img/ffab0505eda21264e5334da4daa4c4b1.png)

其中*𝜇*0 和*𝜎*2_0 分别是先验分布的均值和方差，*𝜇_𝑙*和*𝜎2_l*是似然函数的均值和方差，*𝑛*是样本大小。

这些方程假设先验分布和似然函数都是正态分布，正如在正态-正态共轭模型中所见。如果先验和似然不是正态分布，或者后验分布没有封闭形式的解，这些参数可以通过马尔可夫链蒙特卡洛（MCMC）采样等技术进行近似。

我们准备实施我们的新方程。

```py
# Compute the posterior distribution using Bayes' theorem
var_posterior = 1 / ((sample_size/(sigma_likelihood**2)) + (1/(sigma_prior**2)))
mu_posterior = var_posterior * (mu_prior/(sigma_prior**2) + (sample_size*mu_likelihood)/(sigma_likelihood**2))
posterior = tfd.Normal(mu_posterior, tf.sqrt(var_posterior))
credible_interval = (posterior.quantile(0.025).numpy(), posterior.quantile(0.975).numpy())
credible_interval

(45801.615148780984, 52224.89311258316)
```

我们可以看到，更新我们的先验信念后，得到了后验分布。

```py
x = np.linspace(30000, 70000, 1000)

y_prior = prior.prob(x)
y_likelihood = likelihood.prob(x)
y_posterior = posterior.prob(x)

plt.plot(x,y_prior, label="Prior")
plt.plot(x,y_likelihood, label="Likelihood")
plt.plot(x,y_posterior, label="Posterior")

plt.legend();
```

![](img/54e2b201408727822e8af4ab347490a6.png)

图 4：先验、似然和后验概率函数。

最后，与频率主义方法相同，我们可以绘制总体均值的估计值。请注意，我们实际上是在用贝叶斯方法计算置信区间，而不是置信区间。

```py
visualize_output(sample_data, posterior.mean(), (posterior.quantile(0.025), posterior.quantile(0.975)), 'credible')
```

![](img/0e3221655e11c5b959a0c870598ba212.png)

图 5：使用贝叶斯方法估计参数的均值和置信区间

# 结论

总之，频率主义和贝叶斯方法是分析数据和做出预测的两种不同方式。频率主义方法基于统计显著性的理念，涉及构建关于总体的假设并使用样本数据进行检验。贝叶斯方法则基于主观概率，通过使用收集的数据来更新对总体的初步信念。

使用 TensorFlow 和 TensorFlow Probability，我们展示了如何计算频率学派方法的均值和置信区间，以及贝叶斯方法的均值和可信区间。这两种方法在不同类型的问题中都可能有用，因此了解每种方法的优缺点对于选择最适合的方案至关重要。

保持联系：[LinkedIn](https://www.linkedin.com/in/luisbrasroque/)
