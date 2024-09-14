# 理解 Naive Bayes 算法

> 原文：[`towardsdatascience.com/understanding-naive-bayes-algorithm-d753d3b76727`](https://towardsdatascience.com/understanding-naive-bayes-algorithm-d753d3b76727)

## 它是什么以及如何将其应用于实际场景

[](https://meaganburkhart.medium.com/?source=post_page-----d753d3b76727--------------------------------)![Meagan Burkhart](https://meaganburkhart.medium.com/?source=post_page-----d753d3b76727--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d753d3b76727--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d753d3b76727--------------------------------) [Meagan Burkhart](https://meaganburkhart.medium.com/?source=post_page-----d753d3b76727--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d753d3b76727--------------------------------) ·阅读时间 6 分钟·2023 年 12 月 29 日

--

![](img/e991a2baa8392c3db178a9a83844d8bd.png)

照片由[Google DeepMind](https://unsplash.com/@googledeepmind?utm_source=medium&utm_medium=referral)提供，刊登在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

今年，我的决心是回到数据科学的基础。我每天都在处理数据，但如果你在完成重复的任务，很容易忘记一些核心算法的运作。我计划每周在 Towards Data Science 上深入探讨一个数据算法。这一周，我将介绍 Naive Bayes。

# 如何发音 Naive Bayes

只是为了澄清，你可以在[这里](https://pronouncebee.com/naive-bayes/)学习如何发音 Naive Bayes。

既然我们知道怎么说它，我们来看看它的含义……

# 什么是 Naive Bayes 分类器？

这个概率分类器基于[贝叶斯定理](https://www.geeksforgeeks.org/bayes-theorem/)，可以总结如下：

当第二个事件已经发生时，第一个事件的条件概率是“事件 B 发生的情况下 A 的概率和 A 的概率除以事件 B 的概率”的乘积。

***P(A|B) = P(B|A)P(A) / P(B)***

*一个常见的误解是贝叶斯定理和条件概率是同义词。*

然而，有一个区别——贝叶斯定理使用条件概率的定义来找出所谓的“反向概率”或“逆概率”。

换句话说，条件概率是 A 发生的情况下 B 的概率。贝叶斯定理则找出 B 发生的情况下 A 的概率。

朴素贝叶斯算法的一个显著特征是其使用序列事件。简单来说，通过获取后续的附加信息，初始概率会进行调整。我们将这些称为先验概率/边际概率和后验概率。主要的结论是，通过了解另一个条件的结果，初始概率会发生变化。

# 朴素贝叶斯的应用

一个好的例子是医疗检测。例如，如果患者正在处理胃肠问题，医生可能会怀疑[炎症性肠病（IBD）。这种情况的初始概率约为 1.3%](https://www.cdc.gov/ibd/data-and-statistics/prevalence.html)。

然而，根据患者的症状，医生会要求进行[C-反应蛋白（CRP）血液检查，这是一种炎症标志物。如果该标志物高于某个阈值（>50mg/l）](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1856093/)，则患有炎症性肠病的概率增加。

随后，医生进行结肠镜检查，结果显示有明显的炎症。现在，IBD 诊断的概率再次增加。

我们将通过本文的例子来理解不同的朴素贝叶斯分类器如何在医疗保健中应用。

# 朴素贝叶斯分类器的假设

正如其名字所示，朴素贝叶斯分类器与算法的特征是“朴素”或条件独立的相关。

另一个假设是模型中的特征对最终结果的贡献是相等的。然而，这些假设在现实世界中经常被违反。其目的是简化计算单一概率的数学模型。

**朴素贝叶斯分类器的优缺点**

+   朴素贝叶斯分类器的一个优点是它在[小样本上表现良好](https://www.ibm.com/topics/naive-bayes)，即使在违反一些上述严格假设的情况下。

根据具体问题的不同，朴素贝叶斯分类器有一些缺点。

+   例如，与回归模型不同，朴素贝叶斯并不打算衡量特征的重要性——主要是因为假设所有特征对结果的贡献是相等的。

+   此外，还存在一个被称为“[零频率问题](https://www.linkedin.com/pulse/naive-bayes-algorithm-explained-simranjeet-singh/)”的情况。当某个特征组合在训练数据中出现的概率为零时，所有概率的乘积也将等于零。为了克服这一问题，分析师通常会在值中加上 1，以防其等于零的可能性。

朴素贝叶斯分类器通常用于文本分类任务，包括识别“垃圾邮件”和标记情感。

# 朴素贝叶斯模型的不同类型

以下三种类型的朴素贝叶斯模型主要在关于条件概率分布的假设上有所不同。

## 1\. 伯努利朴素贝叶斯

当变量有二进制分布时，使用伯努利朴素贝叶斯。这意味着所有特征都是分类的，可以取值 1（存在）或 0（不存在）。

在使用 Python 进行朴素贝叶斯时，我们可以使用 `[sklearn.naive_bayes](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.naive_bayes)` 的 BernoulliNB 包将尚未是分类变量的变量二值化。如果所有特征的截断点相同，这会很有用。例如，可以使用 0。然而，也有其他方法可以对连续数据进行二值化，包括 Pandas 的 cut 函数，如下所示：

`df['CRGlevel_high'] **=**` `pd.cut(x**=**df['CRG'], bins**=**[0, 50, np.inf], labels**=**['No', 'Yes'])`

## **2\. 高斯朴素贝叶斯分类器**

关于高斯朴素贝叶斯，首先要注意的是模型中的特征是连续且正态分布的。基于这一假设，在对模型中的数据点进行分类时，算法将最终值分配给具有最大后验概率的类别。

高斯朴素贝叶斯可以应用于常见的 Iris 数据集。运行此算法的基本 Python 代码如下：

```py
from sklearn.naive_bayes import GaussianNB
gnb = GaussianNB()

# Train the classifier on the training data
gnb.fit(X_train, y_train)
```

高斯 NB 模块的文档可以在 [这里](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.GaussianNB.html) 找到。

关于高斯朴素贝叶斯，值得了解的一点是，如果算法的假设得到满足，则模型的功能与逻辑回归相同。

假设从 [Bhowmik](https://www.redalyc.org/pdf/925/92542543003.pdf)（2015）的一篇文章中总结如下：

+   Y 是布尔值，受伯努利分布控制

+   Xi ∼ N(µij , σi)，对所有的 i 适用

+   对于所有 i 和 k 6= i，Xi 和 Xk 在给定 Y 的条件下是条件独立的。

“如果朴素贝叶斯假设成立，NB 和 LR 产生的模型在渐近意义上是相同的。” [来源](https://www.cs.cornell.edu/courses/cs4780/2018fa/lectures/lecturenote05.html)

在我们使用的示例中，可能需要考虑来自血液检查的多个值来确定一个人是否被诊断为 IBD。因此，我们可以输入原始的 CRG 值，以及 [其他 IBD 标志物](https://www.crohnscolitisfoundation.org/sites/default/files/legacy/assets/pdfs/diagnosingibd.pdf)，如 Calprotectin 和 SedRate，而不是二值化连续变量。

## **3\. 多项式朴素贝叶斯**

当特征值表示相对频率而非二进制字段时，我们可以使用多项式朴素贝叶斯。

这对于文本分类问题很有用，这些问题通常依赖于 token 的频率。在我们开始讨论的 IBD 示例中，我们可以考虑使用电子健康记录（EHR）笔记来应用多项式朴素贝叶斯。

根据与患者症状相关的笔记，模型可能会使用“炎症”、“腹泻”、“便秘”等关键字的频率来判断患者是否可能被诊断为溃疡性结肠炎（UC）或克罗恩病。

当然，仅凭文本并不能确保做出诊断，但一个潜在的使用案例是使用 NB 分类器根据患者的症状识别出患有炎症性肠病（IBD）风险的患者，并鼓励医疗从业者订购额外的检查，以排除差异诊断或确认诊断。

# 我在医疗保健中的建议应用

总之，朴素贝叶斯和贝叶斯定理做出了一些假设，这些假设在现实世界中可能并不适用。然而，当你处理一个小数据集并寻找一个好的基线模型时，NB 可以很好地工作，因为它计算简单且易于理解。

基于我的研究，医疗保健中的潜在应用似乎很有前景。例如，在排除差异诊断时，可以使用朴素贝叶斯分类器来确定需要进行多少测试才能得出最终诊断。由于每个特征被期望是独立的并且贡献相等，这可能有助于指导从业者确定哪种测试组合是“足够的”以达成结论性诊断。

我预期这可以减少不必要的测试，并缩短最终诊断的时间，这样可以为医生节省时间，为健康保险计划减少费用，并为患者减少接受治疗和进行侵入性检查所需的时间。
