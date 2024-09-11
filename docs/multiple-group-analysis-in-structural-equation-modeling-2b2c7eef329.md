# 结构方程模型中的多组分析

> 原文：[https://towardsdatascience.com/multiple-group-analysis-in-structural-equation-modeling-2b2c7eef329?source=collection_archive---------7-----------------------#2023-06-16](https://towardsdatascience.com/multiple-group-analysis-in-structural-equation-modeling-2b2c7eef329?source=collection_archive---------7-----------------------#2023-06-16)

## 测试子群体之间的效应

[](https://laura-castro-schilo.medium.com/?source=post_page-----2b2c7eef329--------------------------------)[![Laura Castro-Schilo](../Images/78ab9f49dd2a84e3092d7772ddc73ef4.png)](https://laura-castro-schilo.medium.com/?source=post_page-----2b2c7eef329--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2b2c7eef329--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2b2c7eef329--------------------------------) [Laura Castro-Schilo](https://laura-castro-schilo.medium.com/?source=post_page-----2b2c7eef329--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F362adbd3ba84&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultiple-group-analysis-in-structural-equation-modeling-2b2c7eef329&user=Laura+Castro-Schilo&userId=362adbd3ba84&source=post_page-362adbd3ba84----2b2c7eef329---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2b2c7eef329--------------------------------) ·6 分钟阅读·2023年6月16日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2b2c7eef329&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultiple-group-analysis-in-structural-equation-modeling-2b2c7eef329&user=Laura+Castro-Schilo&userId=362adbd3ba84&source=-----2b2c7eef329---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2b2c7eef329&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultiple-group-analysis-in-structural-equation-modeling-2b2c7eef329&source=-----2b2c7eef329---------------------bookmark_footer-----------)

多组分析（MGA）是一种统计技术，允许研究人员通过指定具有*组特定*估计值或在各组之间具有*相等*估计值的[结构方程模型](https://community.jmp.com/t5/JMPer-Cable/Structural-Equation-Modeling-The-arrival-of-a-powerful-new/ba-p/230440)（SEMs），来研究子群体或人口统计细分中的差异。

可以使用MGA来研究均值、回归、载荷、方差和变量的协方差之间的差异，因为所有这些参数都可以在SEM中建模。因此，即使其他建模技术（例如方差分析或带有交互效应的回归）也可以调查分组变量的作用，但这些技术在SEM中的灵活性不如MGA。

![](../Images/9a93db9bde1bedf10ba3356c3c142d47.png)

*图1\. 多组分析的一般概述以及推断策略。图像由作者提供。*

# 多组分析的常见用途

每当有兴趣探索组间差异时，MGA可以成为一个有用的工具。当数据是关于个人时，组通常基于具有少量层级的因素（例如，性别、民族、职业、家庭状态、健康状态等）来定义，但也可以根据不同的领域、数据和分析背景来定义其他各种因素。以下是一些可以在几个不同领域使用MGA回答的问题示例：

消费者研究

+   **满意度**（或**质量**）在不同的人口统计群体中是否有所不同？

人力分析

+   员工**表现**（或**动机**）在公司各分支机构或部门之间是否相等？

医疗保健

+   **患者报告的结果**是否因药物制造商而异？

营销

+   新的营销活动在不同地理区域提高**品牌声誉**的效果如何？

心理学

+   **情感体验**是否存在跨文化差异？

教育

+   **学业成就**的增长在女性和男性之间是否相等？

# 在多个组中测量未观察到的变量

上述所有问题涉及的变量都是未观察到的（例如，满意度、表现等），也称为*潜在*变量。由于这些变量无法直接观察，因此测量起来比较困难。

![](../Images/9befbc6aa469ea358638e6ead255ba3e.png)

*图2\. 比较未观察（潜在）变量与观察变量的测量。图像由作者提供。*

这样一个困难是，不同的组可能对这些变量有不同的概念化。问问自己：

> *什么*是*满意度？*
> 
> *什么*是*良好的表现？*
> 
> *你的回答是否可能与那些具有不同生活经历的人的回答不同？*
> 
> ***很多时候，答案是肯定的。***

幸运的是，我们可以通过实证测试不同组是否以相似的方式概念化潜在变量。这个测试在SEM框架下通过MGA进行，称为[因子不变性](https://web.pdx.edu/~newsomj/semclass/ho_invariance.pdf)（也称为测量不变性）。因子不变性测试对于确保跨组比较的有效性至关重要；因此，如果存在潜在变量，这些测试必须在比较组间的回归或均值（即结构参数）之前进行。

![](../Images/5f6b6430f46aae7a30238ac8ed78299a.png)

*图 3\. 模型化未观察变量的挑战在于，它们可能在子群体之间测量的内容不相同。图片由作者提供。*

# 参数差异测试

为了测试组间参数的差异，研究人员通常会拟合有无组间等式约束的结构方程模型。然后，使用似然比检验（等效于[卡方差异检验](https://www.jmp.com/support/help/en/17.0/#page/jmp/model-comparison-report-4.shtml#ww518615)）和其他拟合统计量（例如比较拟合指数和均方根误差）比较这两个模型，以评估施加约束是否导致模型拟合的统计学显著恶化。如果模型的拟合没有显著恶化，则保留具有等式约束的模型，并得出结论认为考虑的总体在测试的参数上没有显著差异。相反，如果模型的拟合显著恶化，则保留没有约束的模型（即允许每个组具有自己的估计），并得出结论认为考虑的总体在测试的参数上存在显著差异。

下图展示了在进行简单线性回归的双组示例中MGA背后的策略。此图显示了对一个参数施加的等式约束。模型 1 具有零自由度（即，完全饱和），而模型 2 由于等式约束而具有一个自由度。这些模型通过它们的卡方差异进行比较，该差异也服从自由度为一的卡方分布（模型之间的自由度差异）。可以通过对多个参数同时施加等式约束来进行更不特定的测试。

![](../Images/521b3dd8038223adcc6736b38b8437c2.png)

*图 4\. 具有简单线性回归的双组示例中MGA背后的策略。图片由作者提供。*

**结构方程模型（SEMs）作为确认性模型开发而来。** 也就是说，人们提出假设，将其转化为可检验的统计模型，然后通过推断来确定数据是否支持这些假设。这种方法也适用于MGA，并且对于避免大规模的I型错误率至关重要，这种错误会导致发现数据中实际上不存在的统计效应。因此，不推荐进行所有可能的组间比较。

# MGA估计的直观理解

***免责声明：*** *以下段落适用于希望深入了解MGA的方法论者。本节假设读者了解全信息最大似然估计器。此外，这里列出的步骤仅用于解释MGA背后的逻辑。实际上，按照这些步骤进行MGA会低效，因为统计软件应利用简化此过程的算法。*

MGA 的估计与具有缺失数据的简单 SEM 没有不同。在 MGA-SEM 的标准实施中，用户提交他们想要分析的数据及一个分组变量，该变量指示每个观察值所属的组。需要一个简单的数据处理步骤——使用分组变量——来为多个组设置分析。下面的图示出了用于分析的数据和用于 MGA 的数据重组。

![](../Images/43253ff4aeae03d3a07a9a3f4aeaceeb.png)

*图 5. 用户输入的数据与进行多组分析后的数据重组。图像由作者提供。*

现在可以使用重组数据与完全信息最大似然法作为估计量，确保数据中的所有行都提交用于分析，即使存在缺失数据。重组数据的一些方便结果包括：

+   任何给定行的对数似然仅受非缺失单元格的影响，因此将所有‘Group 0’行的对数似然相加会得到该组的对数似然。同样，将所有‘Group 1’行的对数似然相加会得到组 1 的对数似然。每组的对数似然用于估计整体模型的卡方统计量，该统计量量化了每组的拟合度。

+   缺失值的模式禁止在各组变量间估计任何参数（例如，Var1_0 和 Var1_1 的协方差无法估计），这并不重要，因为 MGA 关注的是跨组效果的比较，而不是组间估计。

+   ‘Vanilla SEM’ 允许对参数设置等式约束。因此，使用重组后的数据在 SEM 中，可以指定两个相同的模型，每个模型的变量子集不同，并且可以对各组间等效参数设置等式约束。再重复一遍，所有这些都可以在标准 SEM 中完成，而无需明确要求软件进行 MGA。

幸运的是，想进行 MGA-SEM 的用户无需执行这些步骤！SEM 软件通过允许用户指定分组变量，使拟合多组模型变得非常简单。然而，进行数据处理（见图 5）并使用标准 SEM 进行 MGA-SEM 将加深你对这个主题的理解。要了解更多信息，请查看以下引用的资源。

[**JMP 中应用的多组分析的逐步示例**](https://www.jmp.com/support/help/en/17.1/index.shtml#page/jmp/example-of-multiple-group-analysis.shtml) **。**

[**有关因子（测量）不变性的多组分析的章节**](https://books.google.com/books?hl=en&lr=&id=PWObEAAAQBAJ&oi=fnd&pg=PA367&ots=8lnhhyPxo-&sig=W7dJH0fswFOqiVuEDyorm8-vqwg#v=onepage&q&f=false) **：**

Widaman, K. F., & Olivera-Aguilar, M. (2022). 使用确认性因素分析调查测量不变性。*结构方程建模手册*, 367.

[**期刊文章**](https://www.tandfonline.com/doi/full/10.1080/10705510701301834) **关于使用替代适配度指数测试不变性：**

Chen, F. F. (2007). 对适配度指数对测量不变性的敏感性。*结构方程建模：多学科期刊*，*14*(3)，464–504。

*本文最初发表于* [*JMP 用户社区*](https://community.jmp.com/t5/JMP-Blog/Multiple-Group-Analysis-in-Structural-Equation-Modeling/ba-p/604714) *，发布时间为 2023 年 2 月 27 日。*
