# 使用多项式回归建模脑电图信号

> 原文：[`towardsdatascience.com/modeling-eeg-signals-using-polynomial-regression-in-r-bd1b0da08251`](https://towardsdatascience.com/modeling-eeg-signals-using-polynomial-regression-in-r-bd1b0da08251)

## 从零开始选择最佳的多项式回归模型

[](https://medium.com/@maladeep.upadhaya?source=post_page-----bd1b0da08251--------------------------------)![Mala Deep](https://medium.com/@maladeep.upadhaya?source=post_page-----bd1b0da08251--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd1b0da08251--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd1b0da08251--------------------------------) [Mala Deep](https://medium.com/@maladeep.upadhaya?source=post_page-----bd1b0da08251--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd1b0da08251--------------------------------) ·阅读时间 12 分钟·2023 年 6 月 15 日

--

![](img/97592fae65e69dc98967db098aaa0bd2.png)

R 中多项式回归的输入脑电图信号时间序列图。图片由作者提供。

## 脑电图信号介绍

EEG 代表脑电图，它是一种测量大脑电活动的电信号[1]。为了获得 EEG 结果，将由细小金属圆盘和细电线组成的电极贴在头皮上。这些电极探测到来自脑细胞活动的微小电荷，然后这些电荷被放大并显示为计算机屏幕上的图形或可能打印在纸上的记录。EEG 的主要目的是通过无痛的方法检测脑细胞通信的潜在问题（如脑炎、出血、癫痫、帕金森病等）[2]。

## 目标

我们的目标如下：

1.  评估多项式回归模型对脑电图信号的拟合程度，并估计模型的参数。

1.  使用基于模拟的方法估计模型参数的后验分布，以考虑模型和数据中的不确定性。

在[GitHub](https://github.com/maladeep/Modeling-EEG-signals-using-polynomial-regression-in-R)上获取完整的代码，或在[RPubs](https://rpubs.com/mala101/eegmodeling)上查看该书。

[](https://github.com/maladeep/Modeling-EEG-signals-using-polynomial-regression-in-R?source=post_page-----bd1b0da08251--------------------------------) [## GitHub - maladeep/Modeling-EEG-signals-using-polynomial-regression-in-R: 使用多项式回归建模脑电图信号…

### 该存储库提供了使用多项式回归建模脑电图信号的实现。代码和分析…

[github.com](https://github.com/maladeep/Modeling-EEG-signals-using-polynomial-regression-in-R?source=post_page-----bd1b0da08251--------------------------------)

## 数据集

我们将使用 [数据集](https://sccn.ucsd.edu/~arno/fam2data/publicly_available_EEG_data.html)，该数据集由 [Swartz 计算神经科学中心](https://sccn.ucsd.edu/) 提供，包含时间、四个输入信号（x1、x2、x3 和 x4）以及一个输出信号（y）。

以下是数据集的一个样本：

![](img/e3e73ef7920b4903997a2eaad5464cd7.png)

由 [Swartz 计算神经科学中心](https://sccn.ucsd.edu/) 提供的数据集快照。作者提供的图像。

## 挑战

在这里，我们将基于假设关系可以表示为多项式回归模型来解释输入 EEG 信号与输出 EEG 信号之间的关系。

我们给出了五种不同的非线性多项式回归模型，我们需要找到最合适的一个。

![](img/4aece3c5c09e8bede206c86dec9bbf48.png)

我们给出了五种不同的非线性多项式回归模型。作者提供的图像

为了解决问题，将采取以下步骤：

1.  使用最小二乘法估计模型参数

1.  计算模型残差误差（RSS）

1.  计算对数似然函数

1.  计算赤池信息量准则（AIC）和贝叶斯信息量准则（BIC）

1.  检查模型预测误差的分布

1.  选择最佳回归模型

## 第一步：使用最小二乘法估计模型参数

当我们对分布的真实值（未知）没有任何了解时，我们使用估计量（随机变量）的概念 [3]。换句话说，我们使用估计量变量来估计与输入和输出变量相关的 EEG 数据分布的真实值。

在这里，估计量变量由“θ”表示，并可以取多个值，如 θ1、θ2、…、θbias。现在，使用最小二乘法（LSM）来计算 EEG 数据不同候选模型的估计量模型参数，LSM（θ̂）用于通过最小化预测值和实际值之间的平方残差之和来估计分布的真实值 [4]，公式如下：

![](img/46117ddc87ebbca70f87f61dad6132b5.png)

最小二乘估计量的公式。作者提供的图像。

现在，为了计算最小二乘法，我们首先需要通过绑定 EEG 数据集中适当的列或值来格式化输入数据。使用函数 cbind()。一旦输入数据正确格式化，我们可以使用上面提到的最小二乘法公式，并使用名为 solve() 的内置线性方程求解函数，我们可以找到 θ̂。我们使用 solve() 是因为它更高效且更不易出错 [5]。

同样，由于我们有 5 个模型，对于每个模型，我们将遵循相同的过程：创建 cbind，计算模型参数的估计值 θ̂ 并打印 theta_hat 的值。

![](img/1faf44a062e19de51759fede320f05be.png)

每个 5 个模型的 θ̂。作者提供的图像。

## 第 2 步。计算模型残差误差（RSS）

残差平方和（RSS），也称为预测误差平方和（SSE）或残差平方和（SSR），是数据与估计模型之间差异的度量。它是通过从基于模型参数的因变量的估计值中减去实际值的平均平方来计算的[6]。

我们通常希望最小化误差，因此误差越小，回归的估计能力越强，模型的拟合度越好。另一方面，较大的 RSS 表示模型对数据的拟合较差[7]。值得一提的是，RSS 永远不会是负值，因为残差的平方始终是非负的[8]。

为了计算 RSS，我们首先必须利用上一步计算的 (θ̂) 来计算每个模型 1–5 的误差，RSS 的数学表示为：

![](img/d111072fa864f3b384322e9fca9d5a21.png)

模型残差误差（RSS）公式。图像由作者提供。

对于模型 1 的 RSS 计算，我们有以下代码：

同样，我们将计算每个模型的 RSS 值。

因此，我们有每个模型的 RSS 值。

![](img/75e6f0d52492c286292cc18038fec62b.png)

显示每个模型的 RSS 值的表格。图像由作者提供。

在这里，表中最低的 RSS 值是 2.1355，与模型 5 相关，其次是与模型 2 相关的第二低 RSS 值 2.1398。

## 第 3 步。计算对数似然函数

现在，我们的目标是确定测量值在参数未知的情况下如何适应提供模型的样本数据。为了实现这一目标，我们将使用第 2 步获得的 RSS 计算线性回归模型的对数似然函数。

对数似然是一种衡量模型拟合优度的方法[9]，并用于简化优化问题，避免数值下溢或上溢。数学表示为：

![](img/099e19f5dc8ddc3210223189590bda15.png)

对数似然公式。图像由作者提供。

在这个任务中，我们的目标是找到一组参数，使观察值的概率最大化。由于对数似然函数的性质是单调增加（非递减）且没有局部极大值，它适合用来识别测量值的拟合度[10]。通俗来说，单调增加意味着当自变量（例如 x）的值增加时，函数值（例如 y）也随之增加，即 x 增加时，y 只能增加，不能减少。

因此，在这里，随着对数似然值的增加，给定模型参数的数据的似然性也会增加。因此，寻找对数似然函数的最大值就等同于寻找似然函数的最大值，但如果仅使用似然函数，则对数的凹性特征会丢失，我们无法获得全局最大值[11]。

因此，使用上述公式，我们将首先计算模型的方差，利用 RSS 以及 Y 信号的长度，然后计算对数似然函数。

使用相同的公式，我们计算其余模型的 BIC。

## 4\. 计算赤池信息量准则（AIC）和贝叶斯信息量准则（BIC）

现在，既然我们有了 RSS 和对数似然值，我们需要使用模型选择标准方法，这里我们将使用赤池信息量准则（AIC）和贝叶斯信息量准则（BIC）。根据[12]，模型选择涉及估计各种候选模型的性能，目标是选择最佳模型。

它们都可以用于比较不同模型并选择最佳模型。它们都基于给定数据的模型的似然性和模型中的参数数量。然而，这两种模型选择方法之间的主要区别在于，AIC 对参数更多的模型的惩罚较小，相比于 BIC[7]。

**4.1 计算每个模型的 AIC**

赤池信息量准则（AIC）是一种统计方法，旨在用尽可能少的自变量（参数）确定真正解释因变量方差的模型[13]。通过这种方法，它帮助选择一个参数较少的简单模型，而不是复杂模型。

使用最大似然估计（步骤 3），确定模型的相对信息值和参数数量。AIC 的公式如下：

![](img/67dea92e4837dbe9f0d58127812a8d2f.png)

赤池信息量准则（AIC）的公式。图像来源于作者。

在这种情况下应用 AIC 的主要目标是消除过拟合问题，因为 AIC 对参数更多的模型给予惩罚，并平衡模型拟合效果与复杂性之间的权衡[14][7]。

根据[15]，值得一提的是，当 AIC 值较低时，模型对数据的拟合效果更好，AIC 的绝对值可能是有利的也可能是有害的。

在进入 AIC 的代码之前，让我们先了解一下 BIC。

**4.2 计算每个模型的 BIC**

如前所述，BIC 与 AIC 类似，但 BIC 对参数更多的模型会给予更大的惩罚[16]。与 AIC 类似，较低的 BIC 值表示模型拟合效果更好。BIC 的公式如下：

![](img/c983cfafe3f7d861fa8f154b1bbe0a33.png)

贝叶斯信息量准则（BIC）的公式。图像来源于作者。

使用上述公式，我们计算了每个模型的 BIC，其计算方式类似于 AIC。

使用上述公式，各模型的 AIC 和 BIC 为：

![](img/d8d83c2614855c5053c47328f324fd79.png)

各模型的 AIC 和 BIC 值。图片由作者提供。

正如我们所记得的，AIC 和 BIC 值越低越好，因此模型 2 的 AIC 为 -334.6489 和 BIC 为 -321.4357 是所有列出的模型中最适合的。

## 第 5 步：检查模型预测误差的分布

现在我们已经获得了 AIC 和 BIC 值，我们有兴趣查看误差分布。毕竟，我们的目标是选择误差最小的模型。在决定绘制分布图之前，我们需要计算每个模型的误差。然后我们将使用 Q-Q 图（分位数-分位数图）通过 qqnorm() 函数可视化和比较两个概率分布，因为我们的假设是数据是独立同分布的。

根据[17]，Q-Q 图是通过将两个分位数集绘制在一起形成的。如果两个分位数集来自相同的分布，则这两个分位数集将形成一条相对直线；然而，在实际中，这并不是一个严格的规则。在同一图中，我们将添加一条参考线，称为 Q-Q 线，这是正态分布的完美拟合线。

qqline() 函数接受两个参数：第一个是数据模型的预测误差，第二个是颜色（col）、线宽（lw）和虚线（lty）。

模型 5 的 Q-Q 图显示大多数数据跟随 Q-Q 线（红色），因此我们可以说这些数据符合正态分布。

在这里，利用获得的 Q-Q 图，我们可以简单地视觉检查数据集是否符合理论分布。要正式测试数据集是否符合某种特定分布，我们需要进一步探讨。

## 第 6 步：选择最佳回归模型

完成步骤 1–5 后，我们已经收集了选择最佳候选模型所需的所有信息。通过计算 RSS、对数似然函数、绘制正态分布图以及比较 AIC 和 BIC 值，我们掌握了识别数据最佳模型的所有信息。基于 AIC 和 BIC，最佳模型是模型 2，因为它具有最低值。为了验证所选的模型 2 是否是一个良好的候选模型，我们将查看 Q-Q 图。

看着 Q-Q 图，除了模型 3，所有模型似乎都有相同的特性；然而，看着 Q-Q 线的位置，模型 2 似乎是最合适的。

为了更倾向于选择模型 2，我们希望绘制直方图以展示残差的分布。为了便于查看，我们将所有直方图绘制成 3 行 2 列的形式，使用 par(mfrow = c(3, 2))。

![](img/55aa9a52f595f429a4a245f9056779ee.png)

`*使用直方图的预测误差分布。图片由作者提供。*`

看每个模型的分布，模型 2、5 和 6 都似乎符合正态分布。

此外，我们将进一步探讨模型 2 是否比其他模型更合适。考虑额外因素将帮助我们确定在这种情况下的最佳模型。我们将根据模型的可解释性比较每个模型的参数数量，即，具有较少参数的简单模型更易于解释和理解 [18]。

查看每个参数的长度，最少的是模型 3，有 3 个参数，但它不符合正态分布且有偏，接下来是模型 4，有 4 个参数，但其 AIC 和 BIC 大于模型 2。

因此，基于 AIC、BIC、Q-Q 图以及额外的可解释性，我们选择了模型 2 作为最佳拟合模型。

![](img/191b24dbaeb85b53e7662da6b656dfcb.png)

揭示冠军：模型 2 成为在 R 中使用多项式回归进行 EEG 信号建模的*最佳选择*。图片由作者提供。

因此，通过使用最小二乘法估计模型参数、模型残差误差（RSS）、对数似然函数、赤池信息准则（AIC）和贝叶斯信息准则（BIC），并利用 Q-Q 图可视化预测误差，我们揭示了冠军：模型 2 成为在 R 中使用多项式回归进行 EEG 信号建模的*最佳选择*。

这完成了我们在 R 中的多项式回归。

*我的* [*GitHub 仓库*](https://github.com/maladeep/Modeling-EEG-signals-using-polynomial-regression-in-R) *中有完整的工作代码，或者你可以访问* [*RPubs*](https://rpubs.com/mala101/eegmodeling) *在线查看。*

> ***喜欢这篇文章？*** *通过成为 Medium 会员来解锁无限学习*。 *通过使用* [*以下链接*](https://medium.com/@maladeep.upadhaya/subscribe)*，你可以在不增加额外费用的情况下支持我。*
> 
> *如果你对文章有任何问题或有兴趣合作，请通过* [*LinkedIn*](https://www.linkedin.com/in/maladeep/) *与我联系。*

## 参考文献

[1] St, E.K., Frey, L.C., Britton, J.W., Frey, L.C., Hopp, J.L., Pearce Korb, Koubeissi, M.Z., Lievens, W.E., Pestana-Knight, E.M. 和 St, E.K. (2016). 介绍脑电图（EEG）：成人、儿童和婴儿的正常与异常发现的入门文本和图谱 [互联网]。 [在线] Nih.gov。可在此处查看：[`www.ncbi.nlm.nih.gov/books/NBK390346/`](https://www.ncbi.nlm.nih.gov/books/NBK390346/)

[2] Healthline (2012). EEG（脑电图）：目的、程序和风险。 [在线] Healthline。可在此处查看： [`www.healthline.com/health/eeg`](https://www.healthline.com/health/eeg)

[3] Peterka, V. (1981). 第八章——贝叶斯系统识别方法。 [在线] ScienceDirect。可在此处查看：[`www.sciencedirect.com/science/article/pii/B9780080256832500132`](https://www.sciencedirect.com/science/article/pii/B9780080256832500132) [访问日期：2023 年 1 月 31 日]。

[4] Björck, Å. (1990). 最小二乘法。 [在线] ScienceDirect。 可在以下网址获取：[`www.sciencedirect.com/science/article/abs/pii/S1570865905800365`](https://www.sciencedirect.com/science/article/abs/pii/S1570865905800365](https://www.sciencedirect.com/science/article/abs/pii/S1570865905800365))[访问日期：2022 年 10 月 4 日]。

[5] Schork, J. (无日期). 在 R 中解决方程组（3 个示例）| 使用 solve() 函数。 [在线] 统计学世界。 可在以下网址获取：[`statisticsglobe.com/solve-system-of-equations-in-r/`](https://statisticsglobe.com/solve-system-of-equations-in-r/](https://statisticsglobe.com/solve-system-of-equations-in-r/))

[6] Allen, D.M. (1971). 预测的均方误差作为选择变量的标准。 Technometrics, 13(3), 页 469–475。 doi:10.1080/00401706.1971.10488811

[7] Brownlee, J. (2019). 使用 AIC、BIC 和 MDL 的概率模型选择。 [在线] 机器学习大师。 可在以下网址获取：[`machinelearningmastery.com/probabilistic-model-selection-measures/`](https://machinelearningmastery.com/probabilistic-model-selection-measures/](https://machinelearningmastery.com/probabilistic-model-selection-measures/))

[8] Valchanov, I. (2018). 平方和：SST、SSR、SSE。 [在线] 365 数据科学。 可在以下网址获取：[`365datascience.com/tutorials/statistics-tutorials/sum-squares/`](https://365datascience.com/tutorials/statistics-tutorials/sum-squares/](https://365datascience.com/tutorials/statistics-tutorials/sum-squares/))

[9] Zach (2021a). 如何解读对数似然值（附示例）。 [在线] Statology。 可在以下网址获取

[10] Stephanie (2021). 对数似然函数。 [在线] 统计学怎么做。 可在以下网址获取：[`www.statisticshowto.com/log-likelihood-function/`](https://www.statisticshowto.com/log-likelihood-function/](https://www.statisticshowto.com/log-likelihood-function/))

[11] Music, A. (2020). 高斯分布与最大似然估计方法（逐步）。 [在线] 创业公司。 可在以下网址获取：[`medium.com/swlh/gaussian-distribution-and-maximum-likelihood-estimate-method-step-by-step-e4f6014fa83e`](https://medium.com/swlh/gaussian-distribution-and-maximum-likelihood-estimate-method-step-by-step-e4f6014fa83e](https://medium.com/swlh/gaussian-distribution-and-maximum-likelihood-estimate-method-step-by-step-e4f6014fa83e)) [访问日期：2023 年 1 月 31 日]。

[12] Trevor Hastie, Tibshirani, R. 和 Friedman, J. (2009). 统计学习的要素。 编辑：纽约，纽约斯普林格纽约。

[13] Manikantan, A. (2021). Akaike 信息准则：模型选择。 [在线] 极客文化。 可在以下网址获取：[`medium.com/geekculture/akaike-information-criterion-model-selection-c47df96ee9a8`](https://medium.com/geekculture/akaike-information-criterion-model-selection-c47df96ee9a8](https://medium.com/geekculture/akaike-information-criterion-model-selection-c47df96ee9a8))

[14] Bevans, R. (2020). Akaike 信息准则 | 何时及如何使用（示例）。[在线] Scribbr. 可用网址： [`www.scribbr.com/statistics/akaike-information-criterion/`](https://www.scribbr.com/statistics/akaike-information-criterion/)

[15] Zach (2021b). 如何解释负 AIC 值。[在线] Statology. 可用网址： [`www.statology.org/negative-aic`](https://www.statology.org/negative-aic/)

[16] Datacadamia (2014). 统计 — 贝叶斯信息准则（BIC）。[在线] Datacadamia — 数据与合作。可用网址： [`datacadamia.com/data_mining/bic`](https://datacadamia.com/data_mining/bic)

[17] Clay Ford (2015). 理解 Q-Q 图 | 弗吉尼亚大学图书馆研究数据服务 + 科学。[在线] Virginia.edu. 可用网址： [`data.library.virginia.edu/understanding-q-q-plots/`](https://data.library.virginia.edu/understanding-q-q-plots/)

[18] De’ath, G. 和 Fabricius, K.E. (2000). 分类与回归树：一种强大而简单的生态数据分析技术。《生态学》，81(11)，第 3178–3192 页。doi:10.1890/0012–9658(2000)081%5B3178:cartap%5D2.0.co;2
