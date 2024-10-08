# 比率的可靠性如何？

> 原文：[`towardsdatascience.com/how-reliable-is-a-ratio-1467f50e943d`](https://towardsdatascience.com/how-reliable-is-a-ratio-1467f50e943d)

## 学习如何使用 Python 中的经验贝叶斯分析评估比率的可靠性

[](https://gustavorsantos.medium.com/?source=post_page-----1467f50e943d--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----1467f50e943d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1467f50e943d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1467f50e943d--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----1467f50e943d--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1467f50e943d--------------------------------) ·9 分钟阅读·2023 年 12 月 8 日

--

![](img/101cda4d83b516311142c34ced48fec5.png)

图片来源：[rupixen.com](https://unsplash.com/@rupixen?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 在 [Unsplash](https://unsplash.com/photos/person-using-laptop-computer-holding-card-Q59HmzK38eQ?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 介绍

在数据科学领域，我的一位参考对象是[Julia Silge](https://juliasilge.com/)。在她的*干净星期二*视频中，她总是制作代码跟随类型的视频，教授/展示给定的技术，帮助其他分析师提升技能并将其纳入自己的技能库。

上周二的话题是经验贝叶斯（她的[博客文章](https://juliasilge.com/blog/doctor-who-bayes/)），引起了我的注意。

那是什么呢？

# 经验贝叶斯

经验贝叶斯是一种统计方法，用于处理像*成功/总尝试数*这样的比率。当我们处理这些变量时，常常会遇到 1/2 的成功率，这转化为 50%的成功百分比，或者 3/4（75%），0/1（0%）。

这些极端百分比并不代表长期的现实，因为尝试次数太少，很难判断是否存在趋势，大多数情况下这些案例被忽略或删除。需要更多的尝试来确定真实的成功率，比如 30/60，500/100，或其他对业务有意义的数据。

然而，使用经验贝叶斯，我们能够利用当前的数据分布来计算其自身数据在早期或晚期阶段的估计值，正如我们将在本文接下来的部分看到的那样。

> 我们使用数据分布来估计每个观察值在早期和晚期阶段的比率。

# 分析

让我们跳到分析部分。需要遵循的步骤是：

1.  加载数据

1.  定义成功并计算成功比率

1.  确定分布的参数

1.  计算贝叶斯估计

1.  计算可信区间

让我们继续。

## **导入**

```py
# Imports
import pandas as pd
import numpy as np
import scipy.stats as scs
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
from distfit import distfit
```

## 数据集

本文中使用的数据集是客户及其电子商务订单和实体店订单的日志。它是基于真实数字生成的，但所有数据集都经过了缩减、随机化和修改，以使其匿名化以供公开使用。

变量如下：

+   *customer:* 客户 ID

+   *brick_mortar:* 实体店发生了多少次交易。

+   *ecomm*: 发生了多少次电子商务交易。

![](img/d5c3ec6ee85de8f53f51e4a23b0f2a98.png)

数据的头部。图像由作者提供。

## 成功比例

假设我们的公司愿意增加其在线渗透率。因此，在这个分析中，我们的成功定义为***当客户在线购买时。*** 客户在线购买[成功]，实体店购买[不成功]。

这样，我们的指标可以是简单的电子商务交易占总数的*[ecomm]/[ecomm + brick_mortar]*。这将给我们一个比例。我们现在的问题是：这两个客户：808 *vs.* 672 有多大不同。

808 的比例是 1/11（9%），而 672 的比例是 1/2（50%）。哪个更好？我应该把更多时间投资在哪里以促进增长？好吧，客户 672 的渗透率为 50%，但总共只有 2 次交易。客户 808 与公司有更长的历史，但刚刚开始在线购买。

![](img/d3419fa3425d2382ec0171c1231c2a3e.png)

808 还是 671？投资在哪里？图像由作者提供。

让我们计算所有的比例。在这里，我们将忽略那些从未在线购买过的客户，因为无法从中计算出估计值。

```py
# Creating success ratio
df2c = (
    df
    .query('ecomm > 0 & brick_mortar > 0')
    .assign(total_trx = df['brick_mortar'] + df['ecomm'],
            ratio = df['ecomm']/(df['brick_mortar'] + df['ecomm'])  )         
)

display(df2c)
```

我们得出了结果。

![](img/8e3b2a8620d4b4dfd280062b5f892abe.png)

计算出的比例。图像由作者提供。

接下来，让我们找出分布参数。

## 找出分布参数

这里的下一步是计算我们比例的分布参数。但在深入之前，让我们建立对为什么需要执行这一步骤的直觉。

好吧，托马斯·贝叶斯定理说**概率是条件性的**。因此，某事发生的可能性应该基于在类似情况下发生的先前结果。

> 如果某事发生了 10 次，那么它在第 11 次发生的可能性比某事在第 10 次未发生的情况下发生的可能性要大。

我们将使用贝塔分布来表示我们的*先验期望*，或到目前为止发生的情况。这就像观察我们的数据并理解模式，以便能够对任何阶段的任何数据点给出更精确的估计，考虑到它会遵循相同的分布。

为了知道哪个α和β使得贝塔分布适合我们的数据，我们将使用 Python 中的`distfit`模块。

看看分布，我们可以直接绘制直方图。

```py
# Looking at the distribution
px.histogram(df2c,
             x='ratio', nbins=20,
             template="simple_white", width = 1000,
             title='Histogram of the Ratio Brick & Mortar vs e-Comm')
```

![](img/7414e4a085c8be2fa6ee3c438f6bcd41.png)

比例分布。图像由作者提供。

现在，让我们看看哪些参数使得贝塔分布适合数据。

```py
# Our distribution
X = df2c['ratio']

# Alternatively limit the search for only a few theoretical distributions.
dfit = distfit(method='parametric', todf=True, distr=['beta'])

# Fit model on input data X.
dfit.fit_transform(X)

# Print the bet model results.
dfit.model
--------------
[OUT]
[distfit] >INFO> fit
[distfit] >INFO> transform
[distfit] >INFO> [beta] [0.11 sec] [RSS: 0.823271] [loc=0.011 scale=0.941]
[distfit] >INFO> Compute confidence intervals [parametric]
{'name': 'beta',
 'score': 0.8232713059833795,
 'loc': 0.011363636363636362,
 'scale': 0.9411483584238516,
 >>>'arg': (0.850939343634336, 1.4553354599535102),<<<
 'params': (0.850939343634336,
  1.4553354599535102,
  0.011363636363636362,
  0.9411483584238516),
 'model': <scipy.stats._distn_infrastructure.rv_continuous_frozen at 0x7838c17ad570>,
 'bootstrap_score': 0,
 'bootstrap_pass': None,
 'color': '#e41a1c',
 'CII_min_alpha': 0.030238213140192628,
 'CII_max_alpha': 0.8158034848017729}
```

我标记了这行`>>>arg: (0.850939343634336, 1.4553354599535102),<<<`，这些是我们需要的α和β。

如果我们想查看拟合情况，可以使用这段代码，其中我们创建了一个包含 3 个子图的图形，并添加了概率密度函数、累积分布函数和 QQ 图。

```py
import matplotlib.pyplot as plt
# Create subplot
fig, ax = plt.subplots(1,3, figsize=(18, 7))
# Plot PDF with histogram
dfit.plot(chart='PDF', ax=ax[0])
# Plot the CDF
dfit.plot(chart='CDF', ax=ax[1]);
dfit.qqplot(X, ax=ax[2])
```

![](img/658dfecb4f3f51ef6aeebd12a0170f94.png)

DistFit 生成的图表。图片由作者提供。

好的。现在来计算我们的估计值。

## 贝叶斯估计

为了计算估计值，我们需要设置刚刚发现的 alpha（a）和 beta（b）。

```py
# Alpha and Beta values
a,b = 0.850939343634336, 1.4553354599535102
```

使用 Beta 分布的估计参数，我们将用经验贝叶斯方法对电子商务交易比例进行建模。我们将从总体先验开始，然后根据个体证据进行更新。*计算方法很简单，就是将* ***a*** *添加到成功次数（电子商务订单）中，再将* ***a+b*** *添加到订单总数中。*

```py
# Calculating Bayes Estimates
df2c = (df2c
        .assign(emp_bayes = (df2c['ecomm'] + a)/(df2c['total_trx'] + a + b) )
        )
```

很好！现在我们已经可以绘制简单比例与估计比例的图表，看看模型的效果如何。

```py
# Scatterplot
fig = sns.scatterplot(data = df2c,
                 x= 'ratio', y= 'emp_bayes',
                 s=50)

# Slope line
plt.plot([0,1], [0,1], linestyle='--', color='red');
```

结果如图所示。

![](img/f7c66acd534e37878dd884bb19e41cd5.png)

模型估计值与简单比例的对比。图片由作者提供。

这里，点越接近红线，比例就越可靠。这意味着简单比例[成功]/[总数]非常接近贝叶斯估计，贝叶斯估计已经考虑了先验概率。

看一下表中的一些数字。

![](img/a55b074f206bf295feb33f61b936ab5e.png)

估计值。图片由作者提供。

查看表格，我们发现电子商务交易数量和总交易数量越高，贝叶斯估计值与实际比例越接近。这意味着那些 1/2 的情况将大大偏离目标，因此作为趋势的可靠性较低。

我们还可以计算估计值的置信区间。这就是我们接下来要做的。

## 置信区间

置信区间是使我们的分析更为完整的一步。我们可以给决策者提供一个信任范围。

首先，让我们计算每个观察值的 alpha 1 和 beta 1，这些是更新先验后的 Beta 分布的*后验*形状参数。

```py
# Calculate a1 and b1
df2 = (df2c
       .assign(a1 = df2c.ecomm + a,
               b1 = df2c.total_trx - df2c.ecomm + b,)
)
```

接下来，我们可以使用`scipy.stats.beta`模块，通过`interval`方法进行计算。95%置信区间的输出是一个包含两个数字的元组，其中索引`[0]`是下界，`[1]`是上界。

```py
# Calculating the CI and add to the dataset
df2 = (df2
          .assign( low = scs.beta.interval(.95, df2.a1, df2.b1)[0],
                   high = scs.beta.interval(.95, df2.a1, df2.b1)[1] )
          .sort_values('total_trx', ascending=False)
           )
```

最后，通过观察绘制区间图。我们创建一个图形，然后为中点绘制散点图和两个条形图，一个表示低范围，颜色为白色（在白色背景上看不见），另一个表示高范围。

```py
# customer ID to string for better visualization
df2['customer'] = df2.customer.astype('string')

# Number of customers to plot
n_cust = 100

# Plot
plt.figure(figsize=(25,7))
plt.scatter(df2.customer[:n_cust], df2.emp_bayes[:n_cust], color='black', s=100 )
plt.bar(df2.customer[:n_cust], df2.low[:n_cust], label='low', color='white', zorder=1 )
plt.bar(df2.customer[:n_cust], df2.high[:n_cust], label='high', zorder=0 )
plt.legend();
```

这里是结果。

![](img/7b052af61c8daca065a365f2ce9945b2.png)

按电子商务订单数量排序的置信区间。电子商务订单越多，区间越小。图片由作者提供。

这个图表按电子商务订单数量递减排序。从多（左）到少（右），我们注意到置信区间的大小只会变大，这显示出当电子商务交易数量过少时，预测可靠估计真的很困难。

回到我们的问题之一：我应该投入更多时间在客户 672 还是 808 的电子商务增长上，以下是这些区间。

![](img/4613b83f0cc414aba4a5fc1b5e0f312c.png)

客户 808 和 672 的估计。图像由作者提供。

客户 672 的置信区间要大得多，因此不够可靠。我们应该花更多时间在开发客户 808 上，这与品牌的历史关系一致。

![](img/d45844bb36d163cd8a2a95a546dc128b.png)

客户 808 和 672 的区间。图像由作者提供。

# 在你离开之前

好的，这个工具表现出强大的功能。我们看到它在我们的比较案例中带来了良好的结果。很多时候，我们会直接淘汰交易少的客户。但很多时候，这可能不太可能。此外，我们实际上可能希望分析小型客户。或者，它也可以帮助我们比较两个下单量大的客户，基于 CI 查看他们的潜力。

![](img/40027cfab5f877a40d9149d7d5a0155a.png)

客户 2360 的置信区间可能达到 78%，而客户 361 的置信区间为 75%。图像由作者提供。

经验贝叶斯对于较大的数据集是一个不错的主意，它可能是其他模型的额外变量。

好吧，我敢打赌你可能已经在考虑这在你工作中的实用性了。那么，我们到此为止。

如果你喜欢这些内容，请关注我的博客，订阅我的页面，并在发布后获取帖子。

[](https://gustavorsantos.medium.com/?source=post_page-----1467f50e943d--------------------------------) [## Gustavo Santos - Medium]

### 在 Medium 上阅读 Gustavo Santos 的文章。数据科学家。我从数据中提取洞察，帮助人们和公司…

gustavorsantos.medium.com](https://gustavorsantos.medium.com/?source=post_page-----1467f50e943d--------------------------------)

也可以在 [LinkedIn](https://www.linkedin.com/in/gurezende/) 上找到我。

这是完整的代码，供参考。

[](https://github.com/gurezende/Studying/blob/master/Python/statistics/Empirical_Bayes.ipynb?source=post_page-----1467f50e943d--------------------------------) [## Studying/Python/statistics/Empirical_Bayes.ipynb at master · gurezende/Studying]

### 这是一个包含我的测试和新包研究的库 - Studying/Python/statistics/Empirical_Bayes.ipynb 在…

github.com](https://github.com/gurezende/Studying/blob/master/Python/statistics/Empirical_Bayes.ipynb?source=post_page-----1467f50e943d--------------------------------)

# 参考

[](https://juliasilge.com/blog/doctor-who-bayes/?source=post_page-----1467f50e943d--------------------------------) [## 经验贝叶斯用于 #TidyTuesday 《神秘博士》剧集 | Julia Silge]

### 数据科学博客

[## 理解经验贝叶斯估计（使用棒球统计）](http://varianceexplained.org/r/empirical_bayes_baseball/?source=post_page-----1467f50e943d--------------------------------)

### 这两个比例哪个更高：10 中 4，还是 1000 中 300？这听起来像是个愚蠢的问题。显然……

[## 理解可信区间（使用棒球统计）](http://varianceexplained.org/r/credible_intervals_baseball/?source=post_page-----1467f50e943d--------------------------------)

### 本系列之前

[## 贝塔分布的直觉是什么？](https://stats.stackexchange.com/questions/47771/what-is-the-intuition-behind-beta-distribution?source=post_page-----1467f50e943d--------------------------------)

### 免责声明：我不是统计学家，而是软件工程师。我对统计学的大部分知识来自于……

[## scipy.stats.beta - SciPy v1.11.4 手册](https://stats.stackexchange.com/questions/47771/what-is-the-intuition-behind-beta-distribution?source=post_page-----1467f50e943d--------------------------------)

### 一个贝塔连续随机变量。作为 rv_continuous 类的一个实例，beta 对象从中继承了一个集合……

[docs.scipy.org](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.beta.html?source=post_page-----1467f50e943d--------------------------------)
