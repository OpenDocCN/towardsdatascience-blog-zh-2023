# 揭示真实数据离散度的两个指标超越标准差

> 原文：[`towardsdatascience.com/dispersion-cv-qcd-32849f828434`](https://towardsdatascience.com/dispersion-cv-qcd-32849f828434)

## 数据科学

## 变异系数和分位数离散度系数计算与解释指南

[](https://medium.ealizadeh.com/?source=post_page-----32849f828434--------------------------------)![Essi Alizadeh](https://medium.ealizadeh.com/?source=post_page-----32849f828434--------------------------------)[](https://towardsdatascience.com/?source=post_page-----32849f828434--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----32849f828434--------------------------------) [Essi Alizadeh](https://medium.ealizadeh.com/?source=post_page-----32849f828434--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----32849f828434--------------------------------) ·8 分钟阅读·2023 年 7 月 30 日

--

![](img/01bc45bb1b6fc65f93b1357ec3318b97.png)

图像由作者使用 StockImg.AI 生成

# 介绍

我们都听过这样一句话：“变化是生活的调味品”，在数据中，这种变化或多样性通常表现为离散度。

数据的离散度使数据变得引人入胜，因为它突出了我们否则无法发现的模式和见解。通常，我们使用以下作为离散度的度量：方差、标准差、范围和四分位距 (IQR)。然而，在某些情况下，我们可能需要超越这些典型的度量来检查数据集的离散度。

在比较数据集时，变异系数 (CV) 和四分位离散度系数 (QCD) 提供了洞察力。

在本教程中，我们将深入探讨 CV 和 QCD 两个概念，并回答它们各自的以下问题：

+   它们是什么，它们是如何定义的？

+   如何计算它们？

+   如何解释结果？

上述所有问题将通过两个示例进行详细解答。

# 理解变异性和离散度

无论我们是在测量人的身高还是房价，我们很少发现所有数据点都是相同的。我们不会期望每个人都一样。有些人高，有些人中等，有些人矮。数据通常会有所变化。为了研究这种数据变异性或离散度，我们通常使用范围、方差、标准差等度量来量化它。离散度的度量量化了我们数据点的分布情况。

但是，如果我们希望评估不同数据集之间的变异性呢？例如，我们如果要比较一家珠宝店和一家书店的销售价格怎么办？标准差在这里无效，因为这两个数据集的尺度可能非常不同。

在这种情况下，CV 和 QCD 是有用的离散度指标。

# 深入探讨：变异系数

[变异系数 (CV)](https://en.wikipedia.org/wiki/Coefficient_of_variation)，也被称为*相对标准差*，是一个标准化的离散度量。它以百分比形式表示，没有单位。因此，CV 是比较不同尺度数据的出色离散度量。

从数学角度来看，CV 计算为标准差与均值的比率，有时乘以 100 得到百分比。公式如下：

![](img/65f7bad007b67957c4fff3625779622f.png)

让我们使用 Numpy 的 `mean` 和 `std` 函数在 Python 中计算 CV。

```py
def calc_cv(data_array) -> float: 
  """Calculate coefficient of variation."""
  return np.std(data_array) / np.mean(data_array)
```

现在，让我们看看如何在一个例子中使用这个统计数据。

## 示例 1

考虑以下两个数据集，展示了珠宝店和书店的月销售额：

+   珠宝店：月平均销售额为 $10,000，标准差为 $2,000。

+   书店：月平均销售额为 $1,000，标准差为 $200。

让我们使用 Numpy 生成两个例子的样本数据。

```py
Jewelry Shop: 
    - Mean = $10119.616
    - Standard Deviation = $2015.764
    - CV = 0.199 (dimensionless)
Bookstore: 
    - Mean = $1016.403
    - Standard Deviation = $206.933
    - CV = 0.204 (dimensionless)
```

![](img/218a910a8504cc061cac565411c7a4e6.png)

通过标准差研究珠宝店和书店的月平均销售额的离散度。生成这个图表的代码可以在[笔记本](https://github.com/e-alizadeh/data-science-blog/blob/master/notebooks/DataDispersion-CV-and-QCD.ipynb)中找到。（图像作者）

珠宝店的平均销售额和标准差显著高于书店（平均 $10,119 和标准差 $2,015 对比书店的平均 $1,016 和标准差 $206），然而它们的 CV 相同（20%）。

这意味着，相对于各自的平均销售额，尽管销售量（及其标准差）有很大差异，珠宝店和书店的相对变异性是相同的。

这展示了 CV 作为相对变异度量的概念，并展示了它如何用于比较不同尺度的数据集。

接下来，让我们考虑另一个无量纲的离散度量，即 QCD。

# 深入了解：四分位离散系数

[四分位离散系数 (QCD)](https://en.wikipedia.org/wiki/Quartile_coefficient_of_dispersion) 是另一种*相对*离散度量，特别适用于处理偏态数据或具有离群值的数据。QCD 关注数据集中间 50% 部分的分布，即四分位距 (IQR)。这就是为什么 QCD 是一个稳健的度量。

QCD 的计算方法如下：

![](img/0551155ac46b1ca31757ea771012501d.png)

其中 Q1 是第一个四分位数（第 25 百分位数），Q3 是第三个四分位数（第 75 百分位数）。

```py
def calc_qcd(data_array) -> float: 
  """Calculates Quartile Coefficient Difference""" 
  q1, q3 = np.percentile(data_array, [25, 75]) 
  return (q3 - q1) / (q3 + q1)
```

类似于 CV，QCD 是一个无单位的度量，对比较偏态数据集的离散度可能非常有用。

以下示例将更好地展示 CV 和 QCD 背后的概念。

## 示例 2

考虑两个公司员工年龄的数据集。

+   公司 A（一个初创公司）：年轻员工，有些年长员工。

+   公司 B（一个成熟的公司）：年长的员工，一些较年轻的员工。

让我们使用 Numpy 为两个示例生成样本数据。

```py
Company A: 
    - Q1 = 22.840 years
    - Q3 = 26.490 years
    - IQR = 3.650 years
    - QCD = 0.074 (dimensionless)
Company B: 
    - Q1 = 42.351 years
    - Q3 = 47.566 years
    - IQR = 5.215 years
    - QCD = 0.058 (dimensionless)
```

现在，让我们绘制数据分布及其箱线图和 QCD，以可视化上述信息。

![](img/cbc94dfb78b8791f30cfef20f99faf4a.png)

基于 IQR 的稳健度量，研究公司 A 和公司 B 员工年龄的离散度。生成此图的代码可在 [notebook](https://github.com/e-alizadeh/data-science-blog/blob/master/notebooks/DataDispersion-CV-and-QCD.ipynb) 中找到。（图片由作者提供）

公司 B 的 IQR（5.215 年对比 3.65 年）表明年龄离散度更广。然而，公司 B 的老年员工影响了这一点（查看箱线图）。

另一方面，公司 A 的 QCD（0.074 对比 0.058）比公司 B 更大，显示出相对于其中位数，年龄分布的变异性更大。IQR 并未揭示这一点。

在接下来的部分，我们将学习如何使用 CV 和 QCD 量化这种差异。

# 讨论

让我们回答一些你可能会有的问题。

## 为什么不关注标准差或 IQR 这样的度量？

我们使用标准差和 IQR 来量化数据集的离散度。标准差显示数据点与均值之间的平均距离。IQR 显示我们数据中间 50% 的分布情况。

然而，在比较具有不同单位或规模、偏态分布或存在离群值的两个或多个数据集的离散度时，这些度量可能会产生误导。

虽然标准差和 IQR 是有用的统计工具，但我们偶尔需要 CV 和 QCD 来进行公平比较。

CV 和 QCD 都衡量和比较变异性，尽管它们的方式有所不同。你的数据和期望的变异性决定了使用哪一个。

## 何时使用 CV

CV 是比较具有不同大小、单位或平均值的数据集的变异量的好方法。由于 CV 是一个相对的分散度量，它显示了事物与均值的不同程度。

平均值和标准差这两个受“离群值”影响较大的度量被用来创建 CV。因此，CV 可能会对那些不符合正态分布或具有离群值的数据集提供扭曲的分散视图。因此，CV 最适合用于均匀分布且没有极端值的数据。

在销售案例中，两个组的价格范围差异很大，因此用于测量它们销售的尺度也非常不同。珠宝店的平均销售额可能要高得多，并且变异性也大得多。如果我们使用标准差来衡量这两个组的变异性，可能会得出错误的结论，认为珠宝店的销售变异性更大。

CV 允许我们比较两个数据集之间的销售变异性，而不管它们的不同规模。如果某一类别的 CV 较高，这意味着该类别的销售相对于平均销售而言变异性更大。

## 何时使用 QCD

QCD 使用数据集四分位数，这些四分位数对异常值的敏感度较低。QCD 是对偏斜分布或包含异常值的数据集的鲁棒离散度测量。QCD 关注数据中心 50%，这可能更好地捕捉这些数据集的离散度。

在我们的例子中，我们检查了两家公司之间的年龄差异：一家以年轻员工为主的初创公司（A）和一家以年长员工为主的成熟公司（B）。鉴于它们的年龄范围不同，年长公司的中位年龄和变异性会更高。使用四分位数范围（IQR）来比较离散度可能不准确地表示成熟公司的年龄变异，因为 IQR 测量的是绝对变异，并且对于较大值更高。

QCD 更有效，因为它以中位数为基准标准化变异性，使我们能够在不同尺度下比较公司之间的年龄变异性。较高的 QCD 表示相对于中位数的年龄变异性更大。因此，选择 QCD 进行比较是因为它考虑了不同的尺度以及潜在的数据偏斜或异常值。

## 收获

选择 CV 还是 QCD 取决于数据集的性质和分析目标。以下是关于这两种度量的关键点：

**变异系数（CV）：**

+   CV 是通过标准差与均值的比率计算得出的。

+   CV 是无量纲的。

+   较高的 CV 表示相对于均值的变异性更大。

+   如果均值接近零，CV 可能会产生误导性结果（除以零！）。

**四分位数离散系数（QCD）：**

+   QCD 基于四分位数。

+   QCD 是一个鲁棒的测量指标（对极端值不敏感）。

+   QCD 是无量纲的。

+   较高的 QCD 表示相对于中位数的值变异性更高。

+   如果分布的尾部很重要，QCD 无法完全捕捉散布情况。

# 结论

总结一下，变异系数（CV）和四分位数离散系数（QCD）是检查数值数据离散度的重要统计指标。CV 在比较标准化数据方面表现出色，而 QCD 在偏斜或异常值数据集的情况下有帮助。我们通过两个案例（使用 Python 程序和分析）来观察这些指标在实践中的作用。通过明智地使用它们，我们可能会获得有用的决策信息。

> *📓 你可以在* [*GitHub*](https://github.com/e-alizadeh/data-science-blog/blob/master/notebooks/DataDispersion-CV-and-QCD.ipynb) *上找到本文的笔记本。*

感谢阅读！📚

> **更新（2023 年 8 月 13 日）：** 本文的早期版本中在示例 2（QCD）中包含了两个相同直方图的错误图像。图像已更新以反映正确信息。

我是一个高级数据科学家 📊 和工程师，撰写关于统计学、机器学习、Python 等方面的内容。

+   [*在 Medium 上关注我*](https://medium.com/@ealizadeh) *👋 以获取我的最新文章*

+   *让我们在* [*LinkedIn*](https://www.linkedin.com/in/alizadehesmaeil/) *上联系* 和 [*Twitter*](https://twitter.com/es_alizadeh) *🤝*

有用的链接

[](https://www.investopedia.com/terms/c/coefficientofvariation.asp?source=post_page-----32849f828434--------------------------------) [## 变异系数的意义及其使用方法

### 变异系数（CV）是衡量数据点围绕均值离散程度的一种指标。

[www.investopedia.com](https://www.investopedia.com/terms/c/coefficientofvariation.asp?source=post_page-----32849f828434--------------------------------) [## 四分位数离散系数 - 维基百科

### 来源于维基百科，自由百科全书。在统计学中，四分位数离散系数是一种描述性统计量…

[维基百科](https://en.wikipedia.org/wiki/Quartile_coefficient_of_dispersion?source=post_page-----32849f828434--------------------------------) [## 变异系数 - 维基百科

### 在概率论和统计学中，变异系数（COV），也称为标准化均方根…

[维基百科](https://en.wikipedia.org/wiki/Coefficient_of_variation?source=post_page-----32849f828434--------------------------------)

*最初发布于* [*https://ealizadeh.com*](https://ealizadeh.com/blog/dispersion-cv-qcd/)*。*
