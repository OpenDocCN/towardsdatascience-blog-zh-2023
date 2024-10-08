# 统计学 Bootcamp 8：两个均值的故事

> 原文：[`towardsdatascience.com/statistics-bootcamp-8-a-tale-of-two-means-ecec013ae414`](https://towardsdatascience.com/statistics-bootcamp-8-a-tale-of-two-means-ecec013ae414)

## [统计学 Bootcamp](https://towardsdatascience.com/tagged/statistics-bootcamp)

## 学习你作为数据科学家每日使用的库背后的数学和方法

[](https://medium.com/@askline1?source=post_page-----ecec013ae414--------------------------------)![Adrienne Kline](https://medium.com/@askline1?source=post_page-----ecec013ae414--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ecec013ae414--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ecec013ae414--------------------------------) [Adrienne Kline](https://medium.com/@askline1?source=post_page-----ecec013ae414--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ecec013ae414--------------------------------) ·13 分钟阅读·2023 年 1 月 4 日

--

![](img/20db195e8720ff5e031223d910ebf4c6.png)

作者提供的图片

本文是一个更大 Bootcamp 系列的一部分（请参见 kicker 以获取完整列表！）。这一部分致力于学习如何比较两个总体，基于我们对样本均值与总体均值的了解。

我们可能想调查两个总体（或两个样本）在某些方面是否存在差异。例如，在西北大学或芝加哥大学医学院就读的人在年龄上是否存在差异？在这里，我们寻求*比较*两个均值，而不是在之前的 Bootcamp 中，我们尝试查看样本均值与总体均值之间是否存在差异。

我们将关于年龄和医学生的问题概述如下：

1.  population 1 = 所有在 UIC 注册的医学生

    population 1 = 所有在西北大学注册的医学生

1.  假设（在双尾检验的情况下）：

![](img/339e5320628c93e36ef125a7b1fcb013.png)

# 比较两个总体

如果我们想比较两个*总体*，基于 我们在之前的 Bootcamp 中学到的知识，我们回到了我们钟爱的 z 检验。我们对这个检验的假设是总体彼此独立，两者的标准差已知，样本要么服从正态分布，要么样本量≥30，并且样本是随机的。

我们可以将假设比较表述如下：

对于两个独立的总体，它们各自的均值分别为 xbar_1 和 xbar_2，相关的方差为 σ²*₁* 和 σ²₂。

平均差异：

![](img/7d3ad8b72f7a67571554a371489966a5.png)

方差：

![](img/1c3e5ffc51c5aea3b651af98774da921.png)

标准差：

![](img/3e5441142d945545b1e9b112d4abc3f2.png)

## 两个总体均值差异的置信区间

扩展计算单变量的置信区间（CIs）（训练营 6），我们取均值的差异（估计值），并将每个总体的标准误差相加：

![](img/e29f0b95440200524f9a38568c8a8d3b.png)

***示例。*** *一个 USC Trojans 橄榄球队样本的平均体重为 120 公斤，总体方差为 49 公斤。100 名 Notre Dame 的 fighting Irish 的平均体重为 99 公斤，总体方差为 50 公斤。计算均值差异的 95 百分位置信区间。*

![](img/6b08cc715eac2cb036fd9acad530e626.png)

# 两样本 z 检验

我们可以使用两样本 z 检验来检验两个*总体*均值的假设。我们的假设包括：1) 总体彼此独立，2) 总体是大的或正态分布的，3) 简单随机样本（SRS），4) **每个总体的标准差已知**。为此，我们将按照以下步骤进行：

1.  写出我们的假设陈述，比较均值。H₀: μ1 = μ2 其中我们的备择假设 H₁可以是：μ1 ≠ μ2（双尾），μ1 < μ2（左尾）或μ1 > μ2（右尾）。

1.  设定显著性水平，alpha。

1.  计算 z 统计量：

![](img/8e63e4492cade0c0999efd14b20cb299.png)

为了对这个方程有一些直观的理解，可以将分子（x_1 — x_2）视为观察到的总体差异，而期望的（μ_1 — μ_2）我们用 0 来表示，即没有差异（零假设）。

*例如。假设我们正在决定是否去迪士尼乐园还是迪士尼世界度假。我们正在跟踪每个地方的机票价格以决定去哪儿。现在有促销，你可以以 88.42 美元的价格购买到迪士尼乐园的往返票，而迪士尼世界的票价为 80.61 美元。假设这些数字是从 50 个航班选项的两个样本中生成的，标准差分别为 5.62 美元和 4.83 美元。在α = 0.05 时，你能得出存在成本差异的结论吗？*

1.  设定假设：H₀: μ1 = μ2 H₁: μ1 ≠ μ2

1.  *α* = 0.05

1.  对于*α* = 0.05，我们的对应临界值是± 1.96

1.  我们计算得到的 z 分数是：

![](img/f97ee25f04b8f931b364b330bda1a40a.png)

5\. 比较计算得到的 z 值与 z 临界值：7.45 > 1.96

6\. 根据比较结果，我们会拒绝零假设

7\. 解释我们的发现。有足够的证据表明均值不相等，并且迪士尼乐园与迪士尼世界之间的比率存在差异。

# 两样本 t 检验

到目前为止，你已经了解到我们可以使用 z 检验来调查两个总体均值之间的差异，当每个总体的方差已知时。如果我们没有关于总体方差的信息，我们会返回使用 t 检验（也称为学生 t 检验）来比较两个均值。

t 检验分为两类：**非配对**和 **配对**。当你看到配对与非配对时，在处理总体时将其等同于 *独立* 和 *依赖*。配对检验用于考察同一总体在变化前后的情况，或在随机对照试验（RCTs）中配对样本。独立总体是指它们在某个方面被检验，但群体在其他方面彼此独立。非配对 t 检验（简称 t 检验）可以进一步分为两类：**合并 t 检验** 和 **非合并 t 检验**。

**合并 t 检验** 用于总体方差相等或假设相等的情况。**非合并 t 检验** 用于方差不等或假设不等的情况。我们可以通过较大（s*ₗ*）和较小（s*ₛ*）标准差的平方比来进行（非正式）检查。

如果：

s*ₗ² /* s*ₛ²* < 4 或 s*ₗ /* s*ₛ* < 2：使用合并 t 检验

否则：使用非合并 t 检验

## 合并 t 检验

在进行合并 t 检验以比较两个总体均值时，我们会有通常的假设：简单随机样本，正常/大样本量，总体/样本独立。但我们还将添加假设，即总体的标准差相等。

1.  陈述我们的备择假设和原假设：H₀（μ₁ = μ₂）和 H₁

1.  根据我们的假设确定检验是右尾（μ₁ > μ₂）、左尾（μ₁ < μ₂）还是双尾检验（μ₁ ≠ μ₂）

1.  确定显著性水平 *α*

1.  计算检验统计量：

![](img/4a478d87168c5263f7365367d4ac2dde.png)

其中 sp 为：

![](img/dff6b5a99f4a0fdf2a66687f03a02abc.png)

并且 *n1* 和 *n2* 是每个样本中的实体数量。注意 *sp* 是一个“合并”标准差，这使得该检验成为合并 t 检验。计算 *sp* 时，我们根据各自的样本量创建了 σ (*sp*) 的加权估计。我们可以使用临界值或 p 值方法继续：

5\. 临界值为：±t_{*α/2*}（双尾），-t_{*α*}（左尾）或 t_{*α*}（右尾），其中 DOF = n1 + n2–2 = (n1–1) + (n2–1)。可以通过 t 表确定这些值。同样的 t 表也可以估算 p 值。

6\. 如果检验统计量落入拒绝区域或 p 值 ≤ *α*，我们拒绝 H₀，否则我们不能拒绝 H₀。

7\. 解释和总结统计检验结果。始终注意，假设检验对于正态总体是精确的，对于非正态但大的总体是近似的。

***示例。*** *一名足球教练想确定电解质液体的消耗是否在他的足球队和田径校队之间有所不同，以升/周（L/week）为单位。随机选取了每队的 10 名运动员。*

```py
track:
1.65
3.24
1.87
1.64
2.35
3.39
0.56
1.93
2.21
```

```py
football:
2.23
2.78
1.89
3.67
1.45
1.28
3.57
2.63
2.78
```

1.  *H₀: μ₁ = μ₂ 和 H₁: μ₁ ≠ μ₂（双尾），总体 1：田径，总体 2：足球*

1.  *α = 0.05*

1.  *样本均值和标准差：*

![](img/aad39a8bfac73b054baec6fd6301c2f6.png)![](img/83a111dfcf2553c7eed8fc9dfc49d51a.png)

4\. *合并方差 sp：*

![](img/8b576cc821001f3c9d334e49a62fd05b.png)

5\. *检验统计量：*

![](img/e4176d94d60b44be25e623415c35faa0.png)

6*. 自由度（DOF）：10 + 10 -2 = 18，tcrit：-2.101。由于 -1.89 不小于 -2.101，我们无法拒绝 H0。*

7*. 由于没有足够的证据表明两个人群之间的电解质液体消费存在差异，我们无法拒绝* *H₀。*

## 当 **σ** 相等时的两个均值的置信区间（CI）

当计算两个 *独立* 样本均值的置信区间且 σ 相等时的假设包括：正态/大样本，均等的总体 σ（标准差），简单随机抽样（SRS）和独立总体。

1.  根据 *α 选择置信水平，CI = 1 - α*

1.  确定自由度 = n1 + n2 — 2

1.  使用 t 表格，利用自由度（DOF）查找 ±t_{*α/2*}

1.  置信区间的范围由公式确定，形式为（下限，上限）：

![](img/a0fea09924b65bbc04532ebfac3e9eb8.png)

5\. 现在我们可以根据置信区间（CI）进行解释和建议。

## 非合并 t 检验

非合并 t 检验与合并 t 检验几乎相同。然而，它的不同之处在于假设总体的标准差是 ***不等的***。这最终导致了检验统计量 *t* 和自由度（DOF）的具体计算差异——通常用 Δ 表示。如果返回浮点值，我们需要向下取整到最近的整数。我们按照上面合并 t 检验中的步骤 1–3，因此我们将跳到步骤 4：

4\. 确定自由度（Δ）：

![](img/61056ea23254743bfef6938cf07d4aad.png)

5\. 确定 t 统计量：

![](img/32a9da221b9271e7d4bb6a7bf66d43ab.png)

6\. 使用 t 表格将 t 统计量与临界值或 p 值进行比较，如假设检验所示：±t_{*α/2*}（双尾），-t_{*α*}（左尾）或 t_{*α*}（右尾）

7\. 如果 P ≤ *α* 或检验统计量落在拒绝区域，拒绝 H0，否则无法拒绝 H0。我们现在可以解释我们的结果，并根据数据提出建议。

## 当 σ 不相等时的两个均值的置信区间（CI）

当计算两个 *独立* 样本均值的置信区间且 σ 不相等时的假设包括：正态/大样本，不等的总体 σ（标准差），简单随机抽样（SRS）和独立总体。

1.  根据 *α 选择置信水平，CI = 1 - α*

1.  确定自由度（记得向下取整到最近的整数）：

![](img/61056ea23254743bfef6938cf07d4aad.png)

3\. 使用 t 表格，利用自由度（DOF）查找 ±t_{*α/2*}

4\. 置信区间的范围由公式确定，形式为（下限，上限）：

![](img/a0fea09924b65bbc04532ebfac3e9eb8.png)

5\. 现在我们可以根据置信区间（CI）进行解释和建议。

***示例。*** *一名高中田径教练想知道短跑运动员的鞋钉是否能改善他们的平均时间（减少）。在显著性水平为 0.01 的情况下，有足够的证据得出结论吗？使用鞋钉的运动员与不使用鞋钉的运动员相比，其 100 米平均时间是否减少了？*

```py
spikes
11.2
12.1
10.9
11.2
12.1
10.9
11.8
12.1
10.9
11.7
12.5
10.9
12.1
10.6
11.1

no spikes
13.6
14.1
12.3
11.5
15.8
13.6
14.1
12.3
10.5
15.8
13.6
16.2
12.7
10.8
15.8
```

1.  *计算每个样本的均值和标准差：*

![](img/00de15a9641886da64e0d12383b3c8a9.png)

*2\. s_l/s_s = ~3，这大于 2，因此我们可以假设方差不相等，然后进行非合并 t 检验。*

*3\. H₀：μ₁ = μ₂ 和 H₁：μ₁ < μ₂（左尾），种群 1：有鞋钉，种群 2：无鞋钉*

*4\. α = 0.01*

*5\. 计算 t 统计量：*

![](img/82823fc4e56454cf1e1b2ff44765f931.png)

6\. *自由度：*

![](img/49fe0210b38dc05b4ffdfb75646630be.png)

7\. *t_{df,α} 的临界值 = t_{18,0.01} = 2.552。我们需要将其转换为负值（-2.552），因为标准 t 表不总是提供方向性。与我们计算出的 -4.20 相比，我们可以看到它落在我们假设检验的拒绝区域。*

8\. *我们拒绝 H₀，并得出结论：样本数据中有足够的证据表明，使用鞋钉的 100 米短跑运动员的时间减少了。*

# 配对样本检验

在配对假设检验中，我们希望控制个体差异对结果测量的影响。这通常表现为对个体进行结果测量，实施干预，然后再次对个体进行结果测量。不同的是，我们将其扩展到许多人，并进行汇总，以查看是否存在总体效益。例如，学生在睡眠 5 小时与 8 小时后在数学测试中的表现是否有所不同？将其概括，我们称之为*重复测量*、*匹配*设计。随机对照试验（RCTs）尝试做同样的事情，只是他们必须找到与接受治疗（或没有治疗）的人相似的患者。这两个例子**不再代表两个独立的样本**。

## 配对 t 检验

配对 t 检验用于比较配对样本中的两个均值。假设与我们的独立 t 检验相同——样本是随机获得的，并且要么是大样本，要么是正态分布的。然而，我们不再假设样本彼此**独立**。

我们的假设如下：简单随机样本（SRS）和正态/大样本。

1.  陈述我们的备择假设和原假设：H₀（μ₁ = μ₂）和 H₁

1.  根据我们的假设，确定检验是否为右尾（μ₁ > μ₂）、左尾（μ₁ < μ₂）或双尾检验（μ₁ ≠ μ₂）

1.  确定显著性水平 *α*

1.  计算样本中每个实例的差异（例如，前后对比）

1.  计算检验统计量：

![](img/bbf4f1a2789ec09601c96a6f1f20e3dd.png)

其中 d_bar 是差异的均值，s_d 是差异的标准差，n 是样本量。

5\. 临界值为：±t_{*α/2*}（双尾），-t_{*α*}（左尾）或 t_{*α*}（右尾），自由度 DOF = n-1。使用 t 表可以确定这些值。相同的 t 表也可以估算 p 值。

6\. 如果检验统计量落在拒绝区域内或 p 值 ≤ *α*，我们拒绝 H₀，否则我们不能拒绝 H₀。

7\. 解释和总结统计检验的结果。始终注意，假设检验对于正态分布的人群是准确的，对于非正态但样本量大的群体则是近似的。

*例如。一位医生声称某种药物能帮助超重患者减轻体重。一个样本中 10 名患者在开始治疗前和 1 个月后称重。这些数据是否支持医生在 p 值为 0.05 时的声明？*

![](img/9099fbecb69e29fc888a5fc2799e23f9.png)

1.  *陈述假设：H₀: μ2 = μ1 H₁: μ2 < μ1。这构成了一个左侧检验*

1.  *alpha = 0.05*

1.  确定配对数据的平均差异（治疗后 - 治疗前）

![](img/212dac3224a1c1e7134f08caa398fd8a.png)

4\. 确定配对差异的标准差（此处 n=10）

![](img/063461feb0e415312132983571bb4cb4.png)

5\. 计算 t 统计量，我们得到：

![](img/473902b6f27ce6e429318f1f97900ee5.png)

6\. 使用我们的 t 表，我们需要根据我们的自由度（DOF = n-1）和 alpha = 0.05 确定临界值。在这里，我们的 DOF = 10–1 = 9。t_crit = -1.833

7\. 因为 -3.06 < -1.833，我们拒绝 *H₀*

8\. 我们的结果是我们有足够的证据支持该减重药物导致统计学上显著的净体重减少的声明。

# 配对 t 区间计算

如果我们希望确定配对样本均值的置信区间（CI），µ1 和 µ2。请记住，对于差异不正态分布的大样本，置信区间计算只是近似的。我们计算 t 区间的假设是样本大/差异正态分布且样本是简单随机样本（SRS）。

1.  确定置信水平 alpha

1.  确定 DOF=n-1

1.  使用 t 表查找 t_{*α/2*}

1.  µ1 — µ2 的置信区间边界计算为：

![](img/3c598786c845a534529b819af2f02e3d.png)

5\. 我们现在可以解释我们的置信区间，并从中得出结论。

# 总结

在这次训练营中，我们详细讲解了 t 检验。具体来说，我们讨论了何时使用配对 t 检验和非配对 t 检验。阅读本文后，你应该能够识别数据违反独立性假设的情景，并且对于那些保持独立的数据，如何进行必要的计算。我们还探讨了独立 t 检验中的子类别——即合并和非合并检验，因此请始终记得检查你的标准差。每当我们从数据中进行推断时，**始终**重要的是承认我们在进行分析时所做的假设。

除非另有说明，所有图片均由作者创作。

此外，如果你喜欢看到类似的文章，并且希望无限制访问我的文章以及 Medium 提供的所有文章，请考虑使用下面的推荐链接进行注册。会员费用为每月 5 美元（USD）；我会获得一小部分佣金，这反过来有助于提供更多的内容和文章！

[](https://medium.com/@askline1/membership?source=post_page-----ecec013ae414--------------------------------) [## 使用我的推荐链接加入 Medium — Adrienne Kline

### 阅读 Adrienne Kline（以及 Medium 上的成千上万其他作家的）每一篇文章。你的会员费用将直接支持…

medium.com](https://medium.com/@askline1/membership?source=post_page-----ecec013ae414--------------------------------)
