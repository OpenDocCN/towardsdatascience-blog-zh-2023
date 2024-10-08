# 使用 Python 量化肥尾的 4 种方法

> 原文：[`towardsdatascience.com/4-ways-to-quantify-fat-tails-with-python-10ce62c0ada1`](https://towardsdatascience.com/4-ways-to-quantify-fat-tails-with-python-10ce62c0ada1)

## 直观和示例代码

![Shaw Talebi](https://shawhin.medium.com/?source=post_page-----10ce62c0ada1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----10ce62c0ada1--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----10ce62c0ada1--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----10ce62c0ada1--------------------------------) ·阅读时间 11 分钟·2023 年 12 月 7 日

--

![](img/b5203c61a2cee6046950ad6cec930138.png)

一条肥（猫的）尾巴。图像来自 Canva。

这是关于幂律和肥尾的系列文章中的第三篇。在[上一篇文章](https://medium.com/towards-data-science/detecting-power-laws-in-real-world-data-with-python-b464190fade6)中，我们探讨了如何从经验数据中检测幂律。虽然这种技术很有用，但肥尾超出了仅仅将数据拟合到幂律分布的范围。在这篇文章中，我将解析 4 种量化肥尾的方法，并分享分析真实数据的 Python 示例代码。

*注意：如果你不熟悉幂律分布或肥尾等术语，请查看* [*这篇文章*](https://medium.com/towards-data-science/pareto-power-laws-and-fat-tails-0355a187ee6a) *作为入门。*

在本系列的第一篇文章中，我们介绍了**肥尾**的概念，描述了**稀有事件如何推动分布的整体统计数据**。我们通过帕累托分布看到了肥尾的极端例子，例如，80%的销售额来自 20%的客户（而 50%的销售额来自仅 1%的客户）。

[## 帕累托、幂律和肥尾]

### 统计学中未教授的内容

towardsdatascience.com

尽管帕累托（更一般地说，幂律）分布为我们提供了肥尾的显著例子，但这是一种更普遍的概念，它在从薄尾（即高斯分布）到非常肥尾（即帕累托 80–20）的光谱上存在。

![](img/1165dd459098d49a27a5760128eeac49.png)

肥尾的光谱。图像由作者提供。

这种*粗尾性*的观点为我们提供了一种比仅仅标记为幂律（或不是）更灵活、更精确的数据分类方法。然而，这就提出了一个问题：*我们如何定义粗尾性？*

# **量化粗尾的 4 种方法**

虽然没有“真实”的粗尾量度，但在实践中我们可以使用一些启发式方法（即经验法则）来量化*数据的粗尾程度*。在这里，我们回顾这 4 种启发式方法。我们首先从概念上介绍每种技术，然后深入示例 Python 代码。

## **启发式方法 1：幂律尾部指数**

在幂律分布中，*最粗*的粗尾出现，其中**幂律的尾部指数越小（即*α*），尾部越粗**，如下面的图像所示。

![](img/3930ecd4789e8b354961f2b415e00124.png)

具有不同α值的幂律分布示例。图像由作者提供。

这种观察表明**较小的尾部指数意味着尾部更粗**，自然促使我们使用*α*来量化粗尾。在实践中，这归结为拟合幂律分布到给定数据集，并提取估计的*α*值。

尽管这是一种直接的方法，但它有一个明显的限制。即当处理数据与幂律拟合不良时，该方法将失效。

## **启发式方法 2：峰度（即非高斯性）**

粗尾的对立面是**细尾**（即**稀有事件非常稀少到可以忽略不计**）。细尾的典型例子是受欢迎的高斯分布，其中事件偏离均值 6 个标准差的概率大约为十亿分之一。

这启发了另一种通过量化数据“非高斯”程度来测量粗尾的方法。我们可以通过所谓的非高斯性测量来实现这一点。虽然我们可以设计许多这种测量方法，但最受欢迎的是**峰度**，由下面的公式定义。

![](img/064b94c8c1dc272c7093f90b9f05cb2a.png)

根据参考文献[1]和[2]的峰度定义。图像由作者提供。

峰度由远离中心的值（即尾部）驱动。因此，**峰度越大，尾部越粗**。

当所有矩都是有限的[3]时，这个测量方法往往效果很好。然而，一个主要的限制是峰度对某些分布没有定义，例如*α* <= 4 的帕累托分布，这使得它对许多粗尾数据无用。

## **启发式方法 3：对数正态的σ**

在本系列的过去文章中，我们讨论了对数正态分布，该分布由下面的概率密度函数定义。

![](img/baf93cd83c594d20e2c395eb9c14bdf7.png)

对数正态分布的概率密度函数 [4]。图像由作者提供。

正如我们之前看到的，这种分布有点*狡黠*，因为它在低**σ**时可能看起来像高斯分布，而在高**σ**时则类似于帕累托分布。这自然提供了另一种量化粗尾的方法，其中**σ越大，尾部越粗**。

我们可以通过类似于*启发式 1* 的方法获得这个度量值。即，我们将对数据拟合一个对数正态分布并提取拟合的 σ 值。虽然这是一个简单的过程，但当对数正态拟合未能很好地解释数据时，它（如*启发式 1*）就会失效。

## **启发式 4：Taleb 的 κ**

之前的启发式方法（H）以特定分布为起点（即 H1 — 幂律和 H3 — 对数正态）。然而，在实践中，我们的数据很少精确地遵循任何特定的分布。

此外，当评估遵循质的不同分布的两个变量时，使用这些度量进行比较可能存在问题。例如，使用幂律的尾部指数来比较类似 Pareto 和类似高斯的数据可能意义不大，因为幂律对类似高斯的数据拟合较差。

这促使我们使用不依赖于特定分布的肥尾度量。Taleb 在参考文献 [3] 中提出了一个这样的度量。所提议的**度量 (κ)** 定义为**具有有限均值的单峰数据**，**其值在 0 和 1 之间**，其中**0** 表示数据**最大程度地薄尾**，而**1** 表示数据**最大程度地肥尾**。其定义如下。

![](img/c60aa289efeb7e9afb85bba6042f03b1.png)

Taleb 的 κ 度量定义 [3]。图像来源于作者。

该度量比较两个样本（例如，Sₙ₀ 和 Sₙ），其中 Sₙ 是从特定分布中抽取的 *n* 个样本的总和。例如，如果我们评估一个高斯分布并选择 *n*=100，我们将从高斯分布中抽取 100 个样本并将它们全部加在一起以创建 S₁₀₀。

***M(n)*** 在上述表达式中表示**平均绝对偏差**，根据下面的方程定义。这个**围绕均值的离散度**度量比标准差 [3][5] 更加稳健。

![](img/c4be29d1e35847f2c5406b6b80027ea3.png)

κ 方程中平均绝对偏差的定义 [3]。图像来源于作者。

为了简化问题，我们可以选择 n₀=1，从而得到下面的表达式。

![](img/e94ec6235c9d1eb4d9f45036f749224a.png)

κ with n₀=1。图像来源于作者。

这里的关键术语是 *M(n)/M(1)*，其中***M(n)* 量化了 n 个样本总和的均值周围的离散度**（某种分布）。

对于**薄尾**分布，*M(30)* 将相对接近 *M(1)*，因为数据通常接近均值。因此，*M(30)/M(1) ~ 1*。

然而，对于**肥尾**数据，*M(30)* 将比 *M(1)* 大得多。因此，*M(30)/M(1) >> 1*。下图中，左侧图示展示了高斯分布的离散度如何随和数的总和而变化，右侧图示则展示了 Pareto 分布的变化情况。

![](img/619514d46d0fb2e071e1b9098bc217a0.png)

*高斯（左）和 Pareto 80–20（右）分布的 M(n) 和 M(1) 的缩放。注意 y 轴标签。* 注意：高斯离散度的尺度增加是由于和数的增加。*图像来源于作者。*

因此，对于肥尾数据，κ 方程中的分母将大于分子，使得右侧第二项变小，最终，κ 较大。

如果这些数学内容超出了你的预期，那么总结如下：

**大 κ = 肥尾，小 κ = 瘦尾**。

# **示例代码：量化（现实世界）社交媒体数据的肥尾特征**

了解了概念后，让我们看看这些启发式方法在实际中的应用。这里，我们将使用上述每种方法分析本系列 上一篇文章 中的相同数据。

数据来自我的社交媒体账户，包括 **Medium** 上的月度关注者增加、**YouTube** 视频的收入以及 **LinkedIn** 的每日印象数。数据和代码可以在 [GitHub 仓库](https://github.com/ShawhinT/YouTube-Blog/tree/main/power-laws/3-quantifying-fat-tails) 上免费获取。

[](https://github.com/ShawhinT/YouTube-Blog/tree/main/power-laws/3-quantifying-fat-tails?source=post_page-----10ce62c0ada1--------------------------------) [## YouTube-Blog/power-laws/3-quantifying-fat-tails 在主分支 · ShawhinT/YouTube-Blog

### 补充 YouTube 视频和 Medium 博客文章的代码。 - YouTube-Blog/power-laws/3-quantifying-fat-tails 在主分支…

[github.com](https://github.com/ShawhinT/YouTube-Blog/tree/main/power-laws/3-quantifying-fat-tails?source=post_page-----10ce62c0ada1--------------------------------)

我们首先导入一些有用的库。

```py
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import powerlaw
from scipy.stats import kurtosis
```

接下来，我们将加载每个数据集并将其存储在字典中。

```py
filename_list = ['medium-followers', 'YT-earnings', 'LI-impressions']

df_dict = {}

for filename in filename_list:
    df = pd.read_csv('data/'+filename+'.csv')
    df = df.set_index(df.columns[0]) # set index
    df_dict[filename] = df
```

在这一点上，查看数据总是一个好主意。我们可以通过绘制直方图和打印每个数据集的前 5 条记录来做到这一点。

```py
for filename in filename_list:
    df = df_dict[filename]

    # plot histograms (function bleow is defined in notebook on GitHub)
    plot_histograms(df.iloc[:,0][df.iloc[:,0]>0], filename, filename.split('-')[1])
    plt.savefig("images/"+filename+"_histograms.png")

    # print top 5 records
    print("Top 5 Records by Percentage")
    print((df.iloc[:,0]/df.iloc[:,0].sum()).sort_values(ascending=False)[:5])
    print("")
```

![](img/c6f1b93d5bce6020817c85f4614546f9.png)

Medium 月度关注者的直方图。图片由作者提供。

![](img/df31d7b781cdbfc08bff529aaeff5800.png)

YouTube 视频收入的直方图。注意：如果你发现与之前文章的差异，那是因为我在数据中发现了一个异常记录（这就是查看数据的好处 😅）。图片由作者提供。

![](img/1bbd897528f57b6f247f33ee6223bb66.png)

LinkedIn 日常印象的直方图。图片由作者提供。

根据上述直方图，每个数据集在某种程度上都表现出肥尾特征。让我们查看按百分比排名前五的记录，以更深入地了解这一点。

![](img/fa20b74cd0e5ba368dc5e5595d01d78d.png)

每个数据集按百分比排名前五的记录。图片由作者提供。

从这个角度看，Medium 的关注者似乎最具肥尾特征，60% 的关注者来自仅 2 个月。YouTube 收入也强烈表现为肥尾特征，大约 60% 的收入来自仅 4 个视频。LinkedIn 的印象数似乎是最不具肥尾特征的。

尽管我们可以通过 *查看数据* 得到肥尾特征的定性感觉，但让我们通过我们的 4 个启发式方法使其更具定量性。

## **启发式 1：幂律尾部指数**

为每个数据集获得一个*α*，我们可以使用[*powerlaw*](https://pypi.org/project/powerlaw/)库，正如我们在上一篇文章中所做的那样。这在下面的代码块中完成，我们在循环中进行拟合并打印每个数据集的参数估计值。

```py
for filename in filename_list:
    df = df_dict[filename]

    # perform Power Law fit
    results = powerlaw.Fit(df.iloc[:,0])

    # print results
    print("")
    print(filename)
    print("-"*len(filename))
    print("Power Law Fit")
    print("a = " + str(results.power_law.alpha-1))
    print("xmin = " + str(results.power_law.xmin))
    print("")
```

![](img/27ab7fb5f3ff30dd33631589952cd248.png)

幂律拟合结果。图片作者提供。

上述结果与我们的定性评估一致，即 Medium 粉丝是最胖尾的，其次是 YouTube 收入和 LinkedIn 印象（请记住，较小的*α*意味着较胖的尾部）。

## 启发式方法 2：峭度

计算峭度的一种简单方法是使用现成的实现。在这里，我使用[Scipy](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kurtosis.html)并以类似之前的方式打印结果。

```py
for filename in filename_list:
    df = df_dict[filename]

    # print results
    print(filename)
    print("-"*len(filename))
    print("kurtosis = " + str(kurtosis(df.iloc[:,0], fisher=True)))
    print("")
```

![](img/7a5c6028042943ab2c6e65e12b4bf67a.png)

每个数据集的峭度值。图片作者提供。

峭度告诉我们与*启发式方法 1*不同的故事。根据这个测量，胖尾的排名如下：LinkedIn > Medium > YouTube。

然而，这些结果应持保留态度。正如我们在上面的幂律拟合中看到的，所有 3 个数据集都符合*α* < 4 的幂律，这意味着峭度是无限的。因此，尽管计算返回了一个值，但最好对这些数字保持怀疑态度。

## 启发式方法 3：对数正态的σ

我们可以再次使用[*powerlaw*](https://pypi.org/project/powerlaw/)库来获得类似于*启发式方法 1*的σ估计值。这是它的样子。

```py
for filename in filename_list:
    df = df_dict[filename]

    # perform Power Law fit
    results = powerlaw.Fit(df.iloc[:,0])

    # print results
    print("")
    print(filename)
    print("-"*len(filename))
    print("Log Normal Fit")
    print("mu = " + str(results.lognormal.mu))
    print("sigma = " + str(results.lognormal.sigma))
    print("")
```

![](img/c04fa0e0af43c6313421f3731517e5f7.png)

对数正态拟合结果。图片作者提供。

查看上面的σ值，我们看到所有拟合都暗示数据是胖尾的，其中 Medium 粉丝和 LinkedIn 印象的σ估计值相似。另一方面，YouTube 收入的σ值显著较大，暗示了一个（更）胖的尾部。

令人怀疑的一点是，拟合估计了一个负的μ，这可能表明对数正态拟合可能无法很好地解释数据。

## 启发式方法 4：Taleb 的κ

由于我找不到现成的 Python 实现来计算κ（我没有很认真地找），这个计算需要一些额外的步骤。也就是说，我们需要定义 3 个辅助函数，如下所示。

```py
def mean_abs_deviation(S):
    """
        Computation of mean absolute deviation of an input sample S
    """
    M = np.mean(np.abs(S - np.mean(S)))

    return M

def generate_n_sample(X,n):
    """
        Function to generate n random samples of size len(X) from an array X
    """
    # initialize sample
    S_n=0

    for i in range(n):
        # ramdomly sample len(X) observations from X and add it to the sample
        S_n = S_n + X[np.random.randint(len(X), size=int(np.round(len(X))))]

    return S_n

def kappa(X,n):
    """
        Taleb's kappa metric from n0=1 as described here: https://arxiv.org/abs/1802.05495

        Note: K_1n = kappa(1,n) = 2 - ((log(n)-log(1))/log(M_n/M_1)), where M_n denotes the mean absolute deviation of the sum of n random samples
    """
    S_1 = X
    S_n = generate_n_sample(X,n)

    M_1 = mean_abs_deviation(S_1)
    M_n = mean_abs_deviation(S_n)

    K_1n = 2 - (np.log(n)/np.log(M_n/M_1))

    return K_1n
```

第一个函数*mean_abs_deviation()*计算之前定义的平均绝对偏差。

接下来，我们需要一种方法来生成并求和*n*个样本。这里，我采用了一个简单的方法，随机从输入数组（X）中采样*n*次并将样本相加。

最后，我将*mean_abs_deviation(S)*和*generate_n_sample(X,n)*结合起来，实现之前定义的κ计算，并为每个数据集进行计算。

```py
n = 100 # number of samples to include in kappa calculation

for filename in filename_list:
    df = df_dict[filename]

    # print results
    print(filename)
    print("-"*len(filename))
    print("kappa_1n = " + str(kappa(df.iloc[:,0].to_numpy(), n)))
    print("")
```

![](img/e9a0e3eec0eb8455d966fa603ee674c8.png)

κ(1,100)的每个数据集值。图片作者提供。

上述结果给了我们另一个故事。然而，考虑到这一计算的隐含随机性（回忆一下*generate_n_sample()*的定义）以及我们正在处理胖尾数据的事实，点估计（即只运行一次计算）是不可靠的。

因此，我进行了 1000 次相同的计算，并打印出每个数据集的均值*κ(1,100)*。

```py
num_runs = 1_000
kappa_dict = {}

for filename in filename_list:
    df = df_dict[filename]

    kappa_list = []
    for i in range(num_runs):
        kappa_list.append(kappa(df.iloc[:,0].to_numpy(), n))

    kappa_dict[filename] = np.array(kappa_list)

    print(filename)
    print("-"*len(filename))
    print("mean kappa_1n = " + str(np.mean(kappa_dict[filename])))
    print("")
```

![](img/3a6876630004a5b93d4e7621354d4920.png)

从每个数据集的 1000 次运行中得到的均值κ(1,100)。图片由作者提供。

这些更稳定的结果表明，Medium 的关注者是最胖尾的，其次是 LinkedIn 的印象和 YouTube 的收入。

*注意：可以将这些值与参考文献[3]中的表 III 进行比较，以更好地理解每个κ值。即，这些值与α介于 2 到 3 之间的帕累托分布相当。*

尽管每种启发式方法讲述了略有不同的故事，但所有迹象都表明 Medium 的关注者增长是这三个数据集中最胖尾的。

# 结论

虽然将数据二分类为胖尾（或非胖尾）可能很诱人，但胖尾特征存在于一个范围上。在这里，我们分解了 4 种启发式方法，用于量化*数据的胖尾程度*。

尽管每种方法都有其局限性，但它们为从业者提供了定量比较经验数据的胖尾特征的方式。

**👉 更多关于幂律和胖尾**: 介绍 | 幂律拟合

[## 免费获取我撰写的每个新故事](https://shawhin.medium.com/subscribe?source=post_page-----10ce62c0ada1--------------------------------)

### 免费获取我撰写的每个新故事 另：我不会将您的电子邮件分享给任何人 注册后，您将创建一个...

[shawhin.medium.com](https://shawhin.medium.com/subscribe?source=post_page-----10ce62c0ada1--------------------------------)

# 资源

**连接**: [我的网站](https://shawhintalebi.com/) | [预约电话](https://calendly.com/shawhintalebi) | [问我任何问题](https://shawhintalebi.com/contact/)

**社交媒体**: [YouTube 🎥](https://www.youtube.com/channel/UCa9gErQ9AE5jT2DZLjXBIdA) | [LinkedIn](https://www.linkedin.com/in/shawhintalebi/) | [Twitter](https://twitter.com/ShawhinT)

**支持**: [请我喝杯咖啡](https://www.buymeacoffee.com/shawhint) ☕️

[## 数据创业者](https://medium.com/the-data-entrepreneurs?source=post_page-----10ce62c0ada1--------------------------------)

### 一个为数据领域创业者提供的社区。 👉 加入 Discord！

[medium.com](https://medium.com/the-data-entrepreneurs?source=post_page-----10ce62c0ada1--------------------------------)

[1] [Scipy 峰度](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kurtosis.html#scipy.stats.kurtosis)

[2] [Scipy Moment](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.moment.html)

[3] [arXiv:1802.05495](https://arxiv.org/abs/1802.05495) [stat.ME]

[4] [`en.wikipedia.org/wiki/Log-normal_distribution`](https://en.wikipedia.org/wiki/Log-normal_distribution)

[5] Pham-Gia, T., & Hung, T. (2001). 平均绝对偏差和中位绝对偏差。*数学与计算建模*，*34*(7–8)，921–936\. [`doi.org/10.1016/S0895-7177(01)00109-1`](https://doi.org/10.1016/S0895-7177(01)00109-1)
