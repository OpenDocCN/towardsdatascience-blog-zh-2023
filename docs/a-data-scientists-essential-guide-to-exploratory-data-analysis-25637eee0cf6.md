# 数据科学家探索性数据分析的必备指南

> 原文：[https://towardsdatascience.com/a-data-scientists-essential-guide-to-exploratory-data-analysis-25637eee0cf6?source=collection_archive---------0-----------------------#2023-05-30](https://towardsdatascience.com/a-data-scientists-essential-guide-to-exploratory-data-analysis-25637eee0cf6?source=collection_archive---------0-----------------------#2023-05-30)

## 实操教程

## 理解数据的最佳实践、技术和工具

[](https://medium.com/@miriam.santos?source=post_page-----25637eee0cf6--------------------------------)[![Miriam Santos](../Images/decbc6528a641e7b02934a03e136284a.png)](https://medium.com/@miriam.santos?source=post_page-----25637eee0cf6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----25637eee0cf6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----25637eee0cf6--------------------------------) [Miriam Santos](https://medium.com/@miriam.santos?source=post_page-----25637eee0cf6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F243289394aaa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-essential-guide-to-exploratory-data-analysis-25637eee0cf6&user=Miriam+Santos&userId=243289394aaa&source=post_page-243289394aaa----25637eee0cf6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----25637eee0cf6--------------------------------) ·11分钟阅读·2023年5月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F25637eee0cf6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-essential-guide-to-exploratory-data-analysis-25637eee0cf6&user=Miriam+Santos&userId=243289394aaa&source=-----25637eee0cf6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F25637eee0cf6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-scientists-essential-guide-to-exploratory-data-analysis-25637eee0cf6&source=-----25637eee0cf6---------------------bookmark_footer-----------)![](../Images/1e89567ebcb4e9284b6da452874aa38c.png)

**如果没有正确的方法和工具，EDA 可能会感觉像是无尽而压倒性的任务。** 图片来源：[Devon Divine](https://unsplash.com/@lightrisephoto?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

探索性数据分析（EDA）是在每个数据科学项目开始时必须进行的最重要任务。

从本质上讲，它涉及彻底检查和描述你的数据，以发现其潜在的**特征**、可能的**异常**以及隐藏的**模式**和**关系**。

对数据的理解将**最终指导你完成**机器学习流程中的后续步骤，从数据预处理到模型构建和结果分析。

## EDA 过程基本上包含三个主要任务：

+   **第一步：** *数据集概览和描述统计*

+   **第二步：** *特征评估和可视化*，以及

+   **第三步：** *数据质量评估*

正如你可能猜到的，每个任务都可能涉及相当全面的分析，这很容易让你*像疯子一样切片、打印和绘制 pandas 数据框*。

## 除非你选择了正确的工具。

在这篇文章中，**我们将深入探讨** **有效 EDA 过程的每一步**，并讨论为什么你应该将 `[ydata-profiling](https://github.com/ydataai/ydata-profiling)` 变成你掌握它的一站式商店。

为了**展示最佳实践和调查见解**，我们将使用 [成人收入数据集](https://www.kaggle.com/datasets/uciml/adult-census-income)，该数据集在 Kaggle 或 UCI 仓库中免费提供（许可证：[CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)）。

# 第一步：数据概览和描述统计

当我们第一次接触一个未知的数据集时，脑海中会立刻冒出一个自动的想法：*我在处理什么？*

## 我们需要对数据有深入的理解，以便在未来的机器学习任务中高效处理它。

作为经验法则，我们通常开始时会相对数据的*观察数*、特征的数量和*类型*、总体*缺失率*以及*重复*观察的百分比来描述数据。

通过一些 pandas 操作和合适的备忘单，我们最终可以用一些简短的代码片段打印出上述信息：

数据集概览：成人普查数据集。观察数、特征、特征类型、重复行和缺失值。作者提供的片段。

总的来说，输出格式不是理想的……如果你对 pandas 熟悉，你也会知道标准的*操作模式*是从 `df.describe()` 开始 EDA 过程。

![](../Images/c41f13a2e5b76da51b3402c42934cae4.png)

成人数据集：通过 **df.describe()** 展示的主要统计数据。作者提供的图像。

然而，这仅仅考虑了**数值特征**。我们可以使用 `df.describe(include='object')` 打印出一些关于**分类特征**（计数、唯一、模式、频率）的额外信息，但简单检查现有类别需要一些更详细的操作：

数据集概览：成人普查数据集。打印每个分类特征的现有类别及其相应的频率。作者提供的片段。

然而，我们可以这样做 —— *并且猜猜看，所有后续的EDA任务！* —— **仅用一行代码**，使用 `[ydata-profiling](https://github.com/ydataai/ydata-profiling)`：

成人普查数据集的剖析报告，使用ydata-profiling。摘录来源：作者。

**上面的代码生成了一个完整的数据剖析报告**，我们可以利用这个报告进一步推进我们的EDA过程，而无需编写更多的代码！

我们将在接下来的部分中详细介绍报告的各个部分。关于**数据的整体特征**，我们寻找的所有信息都包含在***概览*部分**：

![](../Images/38aa137faf400cecd465eb5a75bfcb21.png)

ydata-profiling: 数据概况报告 — 数据集概览。图片来源：作者。

我们可以看到我们的数据集包含了**15个特征和32561个观测值**，其中**有23条重复记录，整体缺失率为0.9%**。

此外，数据集已被正确识别为**表格数据集**，且相当异质，呈现了**数值特征和分类特征**。对于**时间序列数据**，它具有时间依赖性并呈现不同类型的模式，`ydata-profiling`会在报告中加入[其他统计信息和分析](/how-to-do-an-eda-for-time-series-cbb92b3b1913)。

我们可以进一步检查**原始数据和现有的重复记录**，以对特征有一个总体的了解，然后再进行更复杂的分析：

![](../Images/b84cfb37df65f0c7df1c127be3b4fb3e.png)

ydata-profiling: 数据概况报告 — 样本预览。图片来源：作者。

**从数据样本的简要预览**中，我们可以立即看出，尽管数据集整体缺失数据的百分比较低，**某些特征可能受到影响**比其他特征更多。我们还可以识别出一些特征有**相当数量的类别**以及零值特征（或至少有大量的零）。

![](../Images/05242b38963361ebde730c02ef9ee77f.png)

ydata-profiling: 数据概况报告 — 重复行预览。图片来源：作者。

**关于重复的行**，考虑到大多数特征表示的是多个可能“同时适用”的类别，发现“重复”的观测值并不奇怪。

但也许**“数据异味”**是这些观测值共享相同的`age`值（这是可能的），以及完全相同的`fnlwgt`，考虑到呈现的值，这似乎更难以置信。因此需要进一步的分析，但我们**很可能在之后需要去除这些重复数据**。

总体而言，数据概览可能是一个简单的分析，但却**极具影响力**，因为它将帮助我们定义接下来的任务。

# 步骤2：特征评估与可视化

在查看总体数据描述符后，我们需要**深入分析数据集的特征**，以获取有关其个别属性的见解——**单变量分析**——以及它们的交互和关系——**多变量分析**。

这两个任务都极度依赖于**调查适当的统计数据和可视化**，这些统计数据和可视化需要**针对特征类型**（例如，数值型、类别型）**和行为**（例如，交互、相关性）进行**量身定制**。

## 让我们来看看每个任务的最佳实践。

## 单变量分析

分析每个特征的个别特征至关重要，因为这将帮助我们决定它们**对分析的相关性**以及**可能需要的准备数据类型**以实现最佳结果。

例如，我们可能会发现极端的值，这可能指示**不一致性**或**异常值**。我们可能需要**标准化** **数值** **数据**或根据现有类别的数量执行**类别特征的独热编码**。或者我们可能需要进行额外的数据准备，以处理**偏移或偏斜的**数值特征，如果我们打算使用的机器学习算法期望特定的分布（通常是高斯分布）。

因此，最佳实践要求对单个属性进行彻底调查，例如描述性统计和数据分布。

**这些将突显出后续任务的必要性，如异常值移除、标准化、标签编码、数据填充、数据增强和其他类型的预处理。**

让我们更详细地调查`种族`和`资本收益`。*我们能立即发现什么？*

![](../Images/c9faa51c65e9da09ad333bb5fac52b33.png)

ydata-profiling: Profiling Report（种族和资本收益）。作者提供的图像。

**`**资本收益**`**的评估很简单：**鉴于数据分布，我们可能会质疑该特征是否对我们的分析有任何价值，因为91.7%的值为“0”。

**分析** `**种族**` **稍微复杂一些：**

其他种族（除了`白人`）明显被低估。这带来了两个主要问题：

+   一个是机器学习算法通常**忽略较少表示的概念**，这被称为[小样本问题](/data-quality-issues-that-kill-your-machine-learning-models-961591340b40)，这会导致学习性能降低；

+   另一个问题有些衍生于此：由于我们处理的是一个敏感特征，这种“忽略倾向”可能会带来直接与***偏见*和*公平性*问题**相关的后果。这是我们绝对不希望渗入到模型中的。

鉴于此，也许我们应该**考虑基于不足代表的类别进行数据增强**，以及**考虑模型评估中的公平性指标**，以检查与`race`值相关的性能差异。

**我们将在讨论数据质量最佳实践（第3步）时进一步详细说明需要解决的其他数据特征。** 这个例子仅仅展示了通过评估每个特征的*属性*我们可以获得多少见解。

**最后，请注意，如前所述，不同的特征类型需要不同的统计和可视化策略：**

+   **数值特征**通常包含有关均值、标准差、偏度、峰度以及其他分位数统计的信息，并且最适合使用直方图绘制；

+   **分类特征**通常使用众数和频率表来描述，并通过条形图进行分类分析。

![](../Images/87f7df92a8524a3d1c6e8979dcc5fb8b.png)

ydata-profiling: Profiling Report. 所呈现的统计数据和可视化结果已根据每个特征类型进行了调整。屏幕录像：作者。

这样的详细分析使用通用的pandas操作会显得繁琐，但幸运的是`ydata-profiling`**将所有这些功能集成在** `ProfileReport`中，方便我们使用：代码片段中没有添加额外的代码！

## 多变量分析

对于多变量分析，最佳实践主要集中在两种策略：分析特征之间的**互动**以及分析它们的**相关性**。

## 分析互动

互动让我们**直观地探索每对特征的行为**，即一个特征的值如何与另一个特征的值相关。

例如，它们可能会展现出*正向*或*负向*关系，这取决于一个值的增加是否与另一个值的增加或减少相关。

![](../Images/1eb472444e5f86d2cb70db8df9269da6.png)

ydata-profiling: Profiling Report — Interactions. 图片来源：作者。

以`age`和`hours.per.week`之间的互动为例，我们可以看到大多数工作者的工作时间为标准的40小时。然而，也有一些“忙碌的蜜蜂”在30至45岁之间工作时间超过40小时（甚至达到60或65小时）。20多岁的人则不太可能过度工作，有时可能会有较轻的工作安排。

## 分析相关性

与互动类似，**相关性让我们** **分析特征之间的关系**。然而，相关性“给出了一个值”，使我们更容易确定这种关系的“强度”。

这种“强度”是**通过相关系数来衡量的**，可以通过数值分析（例如，检查**相关矩阵**）或使用**热图**来分析，热图使用颜色和阴影来直观突出有趣的模式：

![](../Images/6496b34ec25ecff69e835553c2d7d7af.png)

ydata-profiling: Profiling Report — Heatmap and Correlation Matrix. 屏幕录制由作者提供。

关于我们的数据集，注意`education`与`education.num`之间的相关性。这实际上是**它们包含相同的信息**，`education.num`只是对`education`值的分箱。

另一个引人注目的模式是`性别`与`关系`之间的相关性，尽管再次说明并不非常有信息量：查看这两个特征的值，我们会发现这些特征很可能相关，因为`男性`和`女性`通常分别对应`丈夫`和`妻子`。

**这些冗余类型的特征可以检查是否需要从分析中移除**（例如，`marital.status`也与`relationship`和`sex`相关；`native.country`和`race`等）。

![](../Images/2abc5c9290c2574bbacca4f137a768a6.png)

ydata-profiling: Profiling Report — Correlations. 图像由作者提供。

**然而，还有其他显著的相关性，可能对我们的分析有趣。**

例如，`性别`与`职业`之间的相关性，或者`性别`与`每周工作小时数`之间的相关性。

**最终，`收入`与其余特征之间的相关性确实很有信息量**，**特别是当我们试图绘制分类问题时**。了解与目标类别*最相关*的特征有助于我们识别出*最具区分性*的特征，并发现可能影响模型的潜在数据泄漏者。

从热图中看，`marital.status`或`relationship`似乎是最重要的预测变量，而`fnlwgt`例如，似乎对结果没有太大影响。

**类似于数据描述符和可视化，交互和相关性也需要关注当前特征的类型。**

换句话说，不同的组合将使用不同的相关系数进行测量。默认情况下，`ydata-profiling`在`auto`模式下运行相关性分析，这意味着：

+   **数值与数值**的相关性使用[Spearman秩相关系数](https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient)进行测量；

+   **分类与分类**的相关性使用[Cramer’s V](https://en.wikipedia.org/wiki/Cram%C3%A9r%27s_V#:~:text=In%20statistics%2C%20Cram%C3%A9r%27s%20V%20(sometimes,by%20Harald%20Cram%C3%A9r%20in%201946.)进行测量；

+   **数值与分类**的相关性也使用Cramer’s V，其中数值特征首先被离散化；

如果你想查看**其他相关系数**（例如，Pearson、Kendall、Phi），你可以轻松地[配置报告参数](https://ydata-profiling.ydata.ai/docs/master/pages/advanced_usage/available_settings.html#correlations%0A)。

# 第3步：数据质量评估

随着我们向*数据中心化的AI开发模式*迈进，掌握**可能的复杂因素**对于我们数据中的出现问题至关重要。

所谓“复杂因素”是指*在数据收集或处理过程中可能出现的错误*，或[*数据固有特征*](/data-quality-issues-that-kill-your-machine-learning-models-961591340b40)，它们只是数据*性质*的反映。

这些包括*缺失*数据、*不平衡*数据、*常量*值、*重复*数据、高度*相关*或*冗余*特征、*噪声*数据等。

![](../Images/a87042ac12e7e953845df3268a3a0dbf.png)

数据质量问题：错误和数据固有特征。图片由作者提供。

在项目开始时发现这些数据质量问题（并在开发过程中持续监控）至关重要。

**如果在模型构建阶段之前没有识别和解决这些问题，它们可能会危及整个机器学习流程以及由此得出的后续分析和结论。**

如果没有自动化的过程，识别和解决这些问题的能力将完全依赖于进行探索性数据分析（EDA）的人员的个人经验和专业知识，这显然并不理想。*再加上，尤其是考虑到高维数据集，这真是一个巨大的负担。即将迎来噩梦警报！*

这是`ydata-profiling`最受欢迎的功能之一，即**自动生成数据质量警报**。

![](../Images/67186badbc10e8eccf7f69e756ae6457.png)

ydata-profiling: 数据概况报告 — 数据质量警报。图片由作者提供。

**该分析报告输出至少5种不同类型的数据质量问题**，即`duplicates`、`high correlation`、`imbalance`、`missing`和`zeros`。

事实上，我们已经在进行第2步时识别出了一些这些问题：`race`是一个高度不平衡的特征，而`capital.gain`主要被0填充。我们还看到`education`和`education.num`，以及`relationship`和`sex`之间存在紧密的相关性。

## 分析缺失数据模式

在考虑的全面警报范围中，`ydata-profiling`在**分析缺失数据模式**方面尤其有用。

由于缺失数据是现实世界领域中非常常见的问题，可能会完全影响某些分类器的应用或严重偏倚其预测，**另一个最佳实践是仔细分析缺失数据**的百分比和我们特征可能显示的行为：

![](../Images/c6ce96ce2f8ffe5fa1e3c562b94c3d9d.png)

ydata-profiling: 数据概况报告 — 分析缺失值。作者录屏。

从数据警报部分，我们已经知道`workclass`、`occupation`和`native.country`有缺失的观测值。**热图进一步告诉我们`occupation`和`workclass`的缺失模式之间存在直接关系**：当一个特征缺失时，另一个特征也会缺失。

## 关键洞察：数据分析超越了EDA！

到目前为止，我们讨论了构成全面EDA过程的任务以及**数据质量问题和特征的评估** — **一个我们可以称之为数据分析的过程** — 确实是最佳实践。

然而，重要的是要澄清，[**数据分析**](/awesome-data-science-tools-to-master-in-2023-data-profiling-edition-29d29310f779) **超越了EDA。** 我们通常将EDA定义为在开发任何类型的数据管道之前的探索性、互动性步骤，而**数据分析是一个迭代过程**，[**应在每个步骤中进行**](https://medium.com/towards-artificial-intelligence/how-to-compare-2-dataset-with-pandas-profiling-2ae3a9d7695e) **的数据预处理和模型构建过程中。**

# 结论

**高效的EDA奠定了成功机器学习管道的基础。**

这就像对您的数据进行诊断，了解它包含的所有信息 — 其*属性*、*关系*、*问题* — 以便您能够在后续阶段以最佳方式解决这些问题。

**这也是我们灵感阶段的开始：从EDA中问题和假设开始产生，并计划进行分析以验证或排除这些假设。**

在整篇文章中，我们涵盖了**引导您进行有效EDA的3个主要基本步骤**，并讨论了拥有一款顶尖工具 — `ydata-profiling` — 的影响，这不仅能够指引我们正确的方向，还**节省了大量时间和脑力负担**。

**我希望这份指南能帮助您掌握“数据侦探”的艺术**，并且一如既往地，非常感谢您的反馈、问题和建议。告诉我您希望我写些什么其他主题，或者更好的是，来[数据中心AI社区](https://tiny.ydata.ai/dcai-medium)与我见面，让我们合作吧！

# 关于我

博士，机器学习研究员，教育者，数据倡导者，以及全能型人才。在Medium上，我撰写关于**数据中心AI和数据质量**的文章，教育数据科学与机器学习社区如何将不完善的数据转化为智能数据。

[数据中心AI社区](https://tiny.ydata.ai/dcai-medium) | [GitHub](https://github.com/Data-Centric-AI-Community) | [Google Scholar](https://scholar.google.com/citations?user=isaI6u8AAAAJ&hl=en) | [LinkedIn](https://www.linkedin.com/in/miriamseoanesantos/)
