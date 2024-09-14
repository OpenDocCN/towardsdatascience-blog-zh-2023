# **倾向评分匹配（PSM）用于 A/B 测试：减少观察研究中的偏差**

> 原文：[`towardsdatascience.com/propensity-score-matching-psm-for-a-b-testing-reducing-bias-in-observational-studies-958f24ac8884`](https://towardsdatascience.com/propensity-score-matching-psm-for-a-b-testing-reducing-bias-in-observational-studies-958f24ac8884)

## 关于如何在你的实验数据中实施 PSM 的综合指南，包括 Python 代码

[](https://frankphopkins.medium.com/?source=post_page-----958f24ac8884--------------------------------)![Frank Hopkins](https://frankphopkins.medium.com/?source=post_page-----958f24ac8884--------------------------------)[](https://towardsdatascience.com/?source=post_page-----958f24ac8884--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----958f24ac8884--------------------------------) [Frank Hopkins](https://frankphopkins.medium.com/?source=post_page-----958f24ac8884--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----958f24ac8884--------------------------------) ·阅读时间 12 分钟·2023 年 4 月 26 日

--

![](img/01bc4249555c702c1e0dd3e4dae0787e.png)

AI 生成的图像“以瓦西里·康定斯基风格的 PSM”使用 DALL:E 2 — 作者：**Frank Hopkins**

A/B 测试是一种广泛使用的实验设计，其中比较了两个或多个干预措施对感兴趣结果的影响。A/B 测试的目标是估计干预措施对结果的因果效应，同时控制潜在的混杂变量。通常使用随机化来实现处理组和对照组之间的平衡，但这可能并不总是可行或足够平衡所有相关协变量。因此，由于处理组和对照组特征的差异，估计的治疗效果可能会有偏差。

倾向评分匹配（PSM）是一种统计方法，旨在通过基于倾向评分创建可比的处理组和对照组，从而减少估计治疗效果的偏差。倾向评分是在给定一组观察到的协变量下接受治疗的条件概率，它总结了与估计治疗效果相关的协变量信息。PSM 将处理组和对照组中具有相似倾向评分的个体进行匹配，这可以平衡潜在协变量的分布，减少未观察变量的影响。

在随机对照试验（RCTs）的 A/B 测试背景下，PSM 可以在随机化之后帮助减少偏倚。虽然随机化确保了治疗组和对照组在平均水平上的平衡，但由于随机变异，它可能无法在所有相关协变量上实现平衡。PSM 可以用来基于观察到的协变量创建可比的治疗组和对照组，这可以减少偏倚，提高估计治疗效应的准确性和稳健性。

在本文中，我们将提供一个关于在 RCTs 的 A/B 测试背景下使用 PSM 的全面指南。我们将首先讨论 A/B 测试中平衡和混杂变量的重要性，以及随机化在实现平衡方面的局限性。然后，我们将介绍倾向得分的概念，并解释如何使用它来估计治疗效应。我们还将提供如何使用 PSM 来改善 A/B 测试中平衡和减少偏倚的示例，并讨论不同匹配算法的优缺点。最后，我们将提供一个逐步指南，讲解如何在 Python 中实施 PSM，以便使用一个简单的示例数据集进行 RCTs 的 A/B 测试。

# 2\. 背景

随机对照试验（RCTs）通常被认为是估计因果效应的金标准，因为它们旨在通过随机分配消除混杂变量的影响。然而，即使是 RCTs 也可能会受到未测量或未知混杂变量的偏倚。此外，RCTs 中的随机化可能无法在所有相关协变量上实现平衡，特别是在小样本中。因此，需要采用替代方法来减少偏倚并提高估计治疗效应的准确性。

PSM 是一种可以解决随机化在实现治疗组和对照组之间平衡时的局限性的方法。PSM 通过基于个体的倾向得分进行匹配，创建可比的治疗组和对照组。倾向得分是给定一组观察到的协变量下接受治疗的条件概率，它总结了有关协变量的信息，这些信息对于估计治疗效应是相关的。

基于个体的倾向得分进行匹配可以平衡潜在协变量的分布，并减少未观察到变量的影响。此外，通过控制混杂变量，倾向得分匹配（PSM）可以用来减少估计治疗效应的偏倚。通过创建可比的治疗组和对照组，PSM 可以提高估计治疗效应的准确性和稳健性，即使在随机对照试验（RCTs）中进行了随机化之后。

在下一部分，我们将介绍倾向得分的概念，并解释如何在随机对照试验（RCTs）中使用它来估计治疗效应。我们还将讨论 PSM 在 RCTs 的 A/B 测试中的优点和局限性，并提供如何使用 PSM 来减少偏倚和提高估计治疗效应准确性的示例。

# 3\. 倾向得分匹配与 RCTs

尽管随机对照试验（RCTs）被认为是评估干预效果的**黄金标准**，但它们也有其局限性。其中一个主要局限性是潜在的混杂偏倚，这种偏倚可能出现在处理组和对照组之间存在系统性差异，但这些差异在分析中未被考虑。

例如，如果处理组包含比对照组更高比例的高消费用户，则两组之间的任何结果差异可能是由于消费差异而非处理本身。这种混杂效应可能导致对处理效应的估计不准确，并降低因果推断的有效性。

倾向得分匹配可以通过创建一个在观察到的协变量上匹配的更平衡的对照组来解决这个限制。这一匹配过程有助于确保处理组和对照组在协变量分布上更加相似，从而减少混杂偏倚，提高因果推断的有效性。

需要注意的是，倾向得分匹配不会从处理组或对照组中移除用户。相反，它根据用户的倾向得分创建两组用户的匹配对。任何无法根据倾向得分匹配的用户将被简单地排除在分析之外。这个过程有助于创建一个更平衡的对照组，而不移除原始处理组或对照组中的用户。

此外，倾向得分匹配在存在大量协变量且混杂偏倚可能性较高的 RCTs 中特别有用。通过使用倾向得分匹配来平衡处理组和对照组之间的协变量分布，我们可以确保两组之间观察到的结果差异不是由于协变量分布的差异所致。

需要注意的是，倾向得分匹配并非万灵药，仍可能存在未观察到的变量影响处理效应。然而，通过将倾向得分匹配作为我们 RCT 工具包中的一个工具，我们可以提高 RCT 结果的严谨性和可靠性。

# 4\. 在 A/B 测试中使用 Python 代码实现倾向得分匹配

在本节中，我们将详细介绍如何在 A/B 测试中实施倾向得分匹配（PSM），以更多的协变量和净游戏收入（NGR）作为结果指标。PSM 的目标是减少潜在的混杂偏倚，提高因果推断的有效性。

## 第一步：准备数据

第一步是准备数据。这包括识别结果变量（即 NGR）和处理分配变量。我们还需要识别所有可能影响结果变量并可能导致混杂偏差的协变量。这些协变量应包括在用于计算倾向得分的逻辑回归模型中。

```py
# Identify outcome variable and treatment assignment variable

outcome_var = 'ngr'
treatment_var = 'group'

# Identify covariates

covariates = ['age', 'gender', 'income', 'education', 'location', 'device_type', 'browser']
```

```py
+--------+-----+------+--------+---------+------------+------------+------------+------------+
| UserID | Age | Gender | Income | Education |   Device   |  Location  |  Browser   | NGR (USD)  |
+--------+-----+------+--------+-----------+------------+------------+------------+------------+
|   1    |  23 |   M  |  45000 |   College |  iPhone X  |  New York  |  Safari    |    120     |
|   2    |  45 |   F  |  78000 |   College |  Galaxy S9 |  San Fran  |  Chrome    |    80      |
|   3    |  31 |   M  |  65000 |  Graduate |  iPhone 8  |   Boston   |  Firefox   |    50      |
|   4    |  28 |   F  |  38000 |  Graduate |  iPhone 7  |   Austin   |  Chrome    |    200     |
|   5    |  52 |   M  |  95000 |  Graduate |  Galaxy S8 |   Seattle  |  Firefox   |    150     |
|   6    |  39 |   F  |  58000 |  Graduate |  iPhone X  |  New York  |  Safari    |    90      |
|   7    |  33 |   M  |  51000 |   College |  iPhone 8  |  San Fran  |  Chrome    |    70      |
|   8    |  26 |   F  |  32000 |   College |  Galaxy S9 |   Boston   |  Firefox   |    100     |
|   9    |  41 |   M  |  73000 |  Graduate |  iPhone 7  |   Austin   |  Safari    |    80      |
|   10   |  36 |   F  |  68000 |   College |  Galaxy S8 |   Seattle  |  Chrome    |    120     |
+--------+-----+------+--------+-----------+------------+------------+------------+------------+
```

在这个例子中，我们将 NGR 识别为结果变量，将‘group’识别为处理分配变量。我们还识别了几个可能影响 NGR 的协变量，包括年龄、性别、收入、教育、地点、设备类型和浏览器。

## 步骤 2：计算倾向得分

一旦我们准备好数据，我们可以使用逻辑回归模型计算每个用户的倾向得分。逻辑回归模型应包括处理分配变量作为结果变量以及所有协变量作为预测变量。

```py
import statsmodels.api as sm

# Fit logistic regression model to calculate propensity scores

X = sm.add_constant(ab_test_df[covariates])
y = ab_test_df[treatment_var]
model = sm.Logit(y, X)
result = model.fit()
propensity_scores = result.predict(X)
```

```py
+--------+-----+------+--------+-----------+------------+------------+------------+------------+----------------+
| UserID | Age | Gender | Income | Education |   Device   |  Location  |  Browser   | NGR (USD)  | Propensity_Score|
+--------+-----+------+--------+-----------+------------+------------+------------+------------+----------------+
|   1    |  23 |   M  |  45000 |   College |  iPhone X  |  New York  |  Safari    |    120     |      0.35      |
|   2    |  45 |   F  |  78000 |   College |  Galaxy S9 |  San Fran  |  Chrome    |    80      |      0.75      |
|   3    |  31 |   M  |  65000 |  Graduate |  iPhone 8  |   Boston   |  Firefox   |    50      |      0.55      |
|   4    |  28 |   F  |  38000 |  Graduate |  iPhone 7  |   Austin   |  Chrome    |    200     |      0.30      |
|   5    |  52 |   M  |  95000 |  Graduate |  Galaxy S8 |   Seattle  |  Firefox   |    150     |      0.85      |
|   6    |  39 |   F  |  58000 |  Graduate |  iPhone X  |  New York  |  Safari    |    90      |      0.65      |
|   7    |  33 |   M  |  51000 |   College |  iPhone 8  |  San Fran  |  Chrome    |    70      |      0.60      |
|   8    |  26 |   F  |  32000 |   College |  Galaxy S9 |   Boston   |  Firefox   |    100     |      0.45      |
|   9    |  41 |   M  |  73000 |  Graduate |  iPhone 7  |   Austin   |  Safari    |    80      |      0.70      |
|   10   |  36 |   F  |  68000 |   College |  Galaxy S8 |   Seattle  |  Chrome    |    120     |      0.80      |
+--------+-----+------+--------+-----------+------------+------------+------------+------------+----------------+
```

在这个例子中，我们拟合了一个逻辑回归模型来计算每个用户的倾向得分。该逻辑回归模型包括处理分配变量作为结果变量，以及所有协变量作为预测变量。

## 步骤 3：根据倾向得分匹配用户

一旦我们计算出每个用户的倾向得分，就可以使用这些得分根据倾向得分匹配处理组和对照组的用户。这个匹配过程有助于确保处理组和对照组在协变量分布上更加相似，从而减少混杂偏差并提高因果推断的有效性。

```py
from sklearn.neighbors import NearestNeighbors
from sklearn.preprocessing import StandardScaler

# Standardize covariates for matching

X_scaled = StandardScaler().fit_transform(ab_test_df[covariates])

# Match users based on propensity scores

nbrs = NearestNeighbors(n_neighbors=1, algorithm='ball_tree').fit(propensity_scores.values.reshape(-1, 1))
distances, indices = nbrs.kneighbors(propensity_scores.values.reshape(-1, 1))
matched_df = ab_test_df.loc[indices.flatten()]
```

在这个例子中，我们使用`scikit-learn`库中的`StandardScaler`函数对协变量进行标准化以进行匹配。然后，我们使用倾向得分基于最近邻匹配处理组和对照组的用户。最后，我们创建一个包含匹配用户的新数据框。

## 步骤 4：检查处理组和对照组之间的协变量平衡

在我们根据倾向得分匹配用户后，应该检查处理组和对照组之间协变量的平衡，以确保没有剩余的混杂偏差。这可以通过比较匹配数据集中处理组和对照组之间协变量的均值来完成。

```py
matched_df.groupby(treatment_var).mean()[covariates + [outcome_var]]
```

在这个例子中，我们使用 Pandas 中的`groupby`函数将匹配的数据集按处理分配分组，并计算协变量和结果变量的均值。

## 步骤 5：比较平均处理效果

最后，我们可以比较匹配数据集中处理组和对照组之间的平均处理效果。这可以通过计算处理组和对照组之间的 NGR 均值差异来完成。

```py
matched_df.groupby(treatment_var).mean()[outcome_var].diff()[1]
```

在这个例子中，我们使用`diff`函数计算处理组和对照组之间 NGR 均值的差异。结果给出了在使用 PSM 控制潜在混杂偏差后 A/B 测试的平均处理效果。

## 步骤 6：将 PSM 与传统 A/B 测试进行比较

要查看 PSM 对 A/B 测试结果的影响，我们可以将使用 PSM 计算的平均处理效应与传统 A/B 测试的结果进行比较，在传统测试中，用户被随机分配到处理组和对照组。

```py
ab_test_df.groupby(treatment_var).mean()[outcome_var].diff()[1]
```

在这个例子中，我们使用 `diff` 函数计算原始数据集中处理组和对照组之间的平均 NGR 差异。结果给出的是 A/B 测试的平均处理效应，但没有控制潜在的混杂偏差。

通过将使用 PSM 计算的平均处理效应（ATE）与传统 A/B 测试的结果进行比较，我们可以看到混杂偏差对 A/B 测试结果的影响，以及使用 PSM 控制潜在混杂变量的重要性。

这是未匹配数据的 A/B 测试比较的 ATE：

```py
ATE (USD): 1.8
```

在这个例子中，`ab_test_df` 是包含对照组和处理组的原始数据集，`treatment_var` 是一个二元变量，表示处理组（对照组为 0，处理组为 1），`outcome_var` 是感兴趣的结果变量（在这个例子中是 NGR 或净游戏收入）。

代码 `ab_test_df.groupby(treatment_var).mean()[outcome_var].diff()[1]` 计算控制组和处理组之间的平均 NGR 值的差异（`Treatment = 1` 减去 `Treatment = 0`），并返回以美元为单位的 ATE。在这个例子中，ATE 为 1.8 美元，表明处理组的 NGR 值平均高于对照组。

请注意，此计算仅基于原始数据集，并未考虑可能影响结果变量的潜在混杂变量。使用倾向评分匹配计算的 ATE 考虑了潜在的混杂变量，从而提供了对处理效应的更准确估计。

现在，这是 PSM 匹配数据的 ATE：

```py
 ATE (USD): 2.2
```

在这个例子中，经过倾向评分匹配后的处理组 ATE 为 2.2 美元，表明在控制潜在混杂变量后，处理组的 NGR 值平均高于对照组。这个 ATE 高于使用传统 A/B 测试从原始数据集中计算得到的 ATE（1.8 美元），这表明倾向评分匹配使我们能够减少潜在混杂变量的影响，并提供了对处理效应的更准确估计。

请注意，ATE 的大小及其统计显著性将取决于具体的研究问题、研究设计以及用于评估处理效应的统计测试的选择。

在我们的例子中，PSM 使我们能够创建一个处理组和一个匹配的对照组，这些组在年龄、性别、收入、教育和其他协变量上具有相似的分布。这减少了这些协变量对结果变量的影响，从而使我们能够更准确地估计真实的处理效应。

经过 PSM 处理后的处理组 ATE 高于使用传统 AB 测试从原始数据集中计算的 ATE。这可能是由于 PSM 减少了可能使传统 AB 测试中的处理效果估计产生偏差的混杂变量的影响。通过减少这些混杂变量的影响，PSM 使我们能够更准确地估计真实的处理效果。

# 结论

倾向评分匹配（PSM）是一种减少实验研究偏差的有用技术，特别是在 A/B 测试的背景下。通过匹配具有相似倾向评分的观察值，PSM 创建了两个具有类似协变量分布的组，从而减少了混杂变量对处理效果估计的影响。

在这个例子中，我们将 PSM 应用于模拟的 A/B 测试数据集，以估计促销活动对净游戏收入（NGR）的处理效果。我们包括了多个协变量，如年龄、性别、收入、教育和婚姻状况，以控制潜在的混杂效应。我们发现经过 PSM 处理后的处理组 ATE 高于使用传统 AB 测试从原始数据集中计算的 ATE。这表明 PSM 使我们能够减少混杂变量的影响，并更准确地估计真实的处理效果。

然而，需要注意的是，PSM 不是万能的，并且存在局限性。PSM 假设所有混杂变量都是可观察的并且准确测量，这可能并非总是如此。此外，如果样本量过小或协变量过多，PSM 可能会导致统计功效和精度的损失。

因此，在将 PSM 应用于 A/B 测试时，重要的是要仔细考虑研究问题、研究设计和用于评估处理效果的统计测试的选择。PSM 可以作为减少偏差和估计真实处理效果的有价值工具，但应与其他技术和实验设计及数据分析的最佳实践结合使用。
