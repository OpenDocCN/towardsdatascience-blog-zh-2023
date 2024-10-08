# 平均精确度@K（MAP@K）清晰解释

> 原文：[`towardsdatascience.com/mean-average-precision-at-k-map-k-clearly-explained-538d8e032d2`](https://towardsdatascience.com/mean-average-precision-at-k-map-k-clearly-explained-538d8e032d2)

## 对于推荐或排名问题，最受欢迎的评估指标逐步解释

[](https://konstantin-rink.medium.com/?source=post_page-----538d8e032d2--------------------------------)![Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----538d8e032d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----538d8e032d2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----538d8e032d2--------------------------------) [Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----538d8e032d2--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----538d8e032d2--------------------------------) ·阅读时长 7 分钟·2023 年 1 月 18 日

--

![](img/f8af88de2aaea97d6425f70111af801f.png)

图片由[Joshua Burdick](https://unsplash.com/de/@gadgetpastor)提供，来源于[Unsplash](https://unsplash.com/de/Fotos/0xpFNUjzdbw)。

平均精确度@K（MAP@K）是推荐系统和其他**排名**相关分类任务中最常用的评估指标之一。由于该指标是不同误差指标或层的组合，因此初次查看时可能不易理解。

本文将**逐步**解释 MAP@K 及其组成部分。文章末尾还将提供**代码片段**，说明如何计算该指标。但在深入了解每个部分之前，我们首先谈谈**为什么**。

# 为什么使用 MAP@K？

MAP@K 是一种误差度量指标，当你的推荐项的**序列**或**排名**在任务中**扮演重要角色或是目标**时，可以使用该指标。通过使用它，你可以获得以下问题的答案：

+   我生成或预测的推荐是否**相关**？

+   **最相关**的推荐是否在**前几位**？

# 使以下步骤更易于理解

既然你知道了“为什么”，让我们来谈谈**怎么做**。接下来的章节将以“洋葱”风格逐步解释，从内部（从精确度 P 开始）到外部（MAP@K），MAP@K 的结构。

为了使步骤及其组成更易理解，我们以以下示例进行操作：我们希望评估我们的推荐系统，该系统在访问产品详细信息页面时向潜在客户推荐六个项目（见图 1）。

![](img/d1113537826f88df96afb13512147e77.png)

图 1\. 推荐示例（作者提供的图片）。

# 精确度（P）

你可能已经在书籍或文章中听说过精度，当你学习分类模型的误差指标时。精度可以看作是**质量**的衡量标准。高精度意味着我们的模型**返回的结果或推荐更相关，而不是无关的**。

精度可以定义为**所有推荐项目（相关 + 无关项目）**中**相关项目的比例**。

![](img/a7b3504f0d3ba8a5edbdd8cbab670174.png)

图 2\. 精度公式（图片由作者提供）。

以下示例（图 3）显示了 6 个推荐项目。在这 6 个推荐中，有 2 个是相关的。

![](img/ce034f648acc4c58d583f677a138221d.png)

图 3\. 精度示例（图片由作者提供）。

将这些值代入我们的公式（图 1），我们得到一个**0.33**的精度（ `**2** 个相关项目 / (**2** 个相关 **+** **4** 个无关项目)` ）。 

# Precision@K（P@K）

精度指标（图 2）本身不考虑相关项目出现的排名或顺序。是时候将排名纳入我们的精度公式中。Precision**@K**可以定义为前**K**个推荐项中相关项目的比例（图 4）。

![](img/570a1c4029c23a173f0d0eb0acf13340.png)

图 4\. Precision@K 公式（图片由作者提供）。

下图（图 5）展示了我们上述示例（图 3）中的**排名**场景。

![](img/5c93503da99784d70a3e9af0a9d6a000.png)

图 5\. Precision@K 示例（图片由作者提供）。

**Precision@K** 列显示了每个排名（1 到 6）的 Precision@K 值。**K** 代表我们考虑的**排名数量**（1、2、…、6）。

## Precision@1

假设我们只考虑**第一个排名**（**K=1**），那么我们将有**0 个相关项目**除以**1**（总项目），这导致 Precision@1 为**0**。

## Precision@3

假设我们考虑**前三名排名**（K=3），那么在前 3 个推荐中我们将有 1 个**相关项目**和 2 个**无关项目**。如果我们将这些数字代入我们的公式（图 3），我们将得到**0.33**（`**1** 个相关项目 / (**1** 个相关 + **2** 个无关项目)` ）。

## Precision@5

最后但同样重要的是，让我们考虑**前五名排名**（K=5）。我们将有前 5 个推荐中 2 个**相关**和 3 个**无关**的项目。如果我们重新计算，我们将得到一个**0.4**的值。

(`**2** 个相关项目 / (**2** 个相关 + **3** 个无关项目)`).

# 平均精度@K（AP@K）

正如我们所见，精度和 Precision@K 都非常简单。下一步有一点复杂性。

平均精度@K 或 AP@K 是 Precision@K 的总和，其中第 kₜₕ 排名的项目是**相关的**（`rel(k)`），除以前 K 个推荐项中的**相关项目总数**（r）（图 6）。

![](img/5d3f306435bb84bdd4d3d919b2ad31e6.png)

图 6\. AP@K 公式（图片由作者提供）。

迷惑了？让我们看看以下关于 AP@6 的示例（图 7）。

![](img/58c566a74d9de0add78d514c4bb97753.png)

图 7\. AP@K 示例 1（图片由作者提供）。

相关项目的总数（r）在这个示例中为**2**（在排名 2 和 4）。因此，我们可以将 2 放在分数 `1/r` 中。

看第一个排名 1。Precision@1 为 0，且该项不相关（灰色）。因此，我们将 Precision@1 的值乘以 0，得到 `0 * 0`。

然而，在第 2 个排名中，我们有一个 Precision@2 值为 0.5 和一个在第 2 个排名的相关项。这导致 `0.5 * 1`。

在第 3 个排名中，我们再次有一个无关项，Precision@3 为 0.33。结果为 `0.33 * 0`。

我们会逐个排名进行处理。如果排名 k 包含相关项，我们将其 Precision@k 乘以 1。如果无关，我们将其乘以 0，这意味着它不会对我们的总和产生影响。

对于这个示例，**最终结果**将是**AP@6**为**0.5**。

在我们进入最后一步之前，你还记得我们在开始时提到 MAP@K 可以回答的问题吗：

> **最相关**的推荐是否在**前几个排名**中？

该度量的一个重要特性是它惩罚低排名中的相关项。为了让你更好地理解，让我们看看以下示例（图 8）。

![](img/af3ce9d044b5b30d17b8e300181872f6.png)

图 8\. 不同排名下相关项目的示例。最佳情况（左），最差情况（右）。排名越高越好（图片由作者提供）。

与最初的示例（图 7）相比，相关项目的数量没有变化。但变化的是它们被放置的排名。AP@K（因此 MAP@K）会惩罚那些将相关项目放在较低排名的推荐或模型。

# 平均精度均值@K（MAP@K）

之前的步骤和示例都是基于评估**单个查询**或一个访客在浏览产品 X 的产品详情页时收到的单个推荐列表。但我们有不止一个访客……

平均精度均值@K 或 MAP@K 考虑到了这一点。它平均了对***M* 用户显示的推荐的 AP@K**。

![](img/51ef07924cf2fce8e85841273309f8cc.png)

图 9\. MAP@K 公式（图片由作者提供）。

> **请注意**：为了简化起见，我在这个示例中选择了“用户”。但是，根据你的情况，M 也可以是例如搜索查询。

为了更好地理解 MAP@K 是如何计算的，下面的示例（图 10）可以提供帮助。

![](img/94dc3618c0c8ef7fa03167c883baa0bd.png)

图 10\. MAP@K 示例（图片由作者提供）。

基于 3 个不同的用户（或查询），MAP@6 为**0.59**。

# 编码

现在我们已经熟悉了理论，让我们开始编码。在接下来的示例中，我们将处理两个包含 product_ids 的列表 `actuals` 和 `predicted`。

目标是检查我们的实际值是否出现在预测列表中，以及如果出现，它们出现在什么排名/位置。这就是为什么在 `actuals` 列表中项目的顺序无关紧要，而在 `predicted` 列表中却很重要。

## AP@K

以下列表反映了我们之前使用的示例（图 11）。

![](img/ef0672a28c17ae46725ad5b7ce570428.png)

图 11\. 从开始回顾的示例（图片由作者提供）。

```py
actuals = ['p_a', 'p_b']
predicted = ['p_d', 'p_a', 'p_c', 'p_b', 'p_e', 'p_f']
```

如果我们将实际值与预测值进行比较，那么我们可以看到 `p_b` 出现在**第 2** 位，`p_d` 出现在**第 4** 位。

```py
def apk(y_true, y_pred, k_max=0):

  # Check if all elements in lists are unique
  if len(set(y_true)) != len(y_true):
    raise ValueError("Values in y_true are not unique")

  if len(set(y_pred)) != len(y_pred):
    raise ValueError("Values in y_pred are not unique")

  if k_max != 0:
    y_pred = y_pred[:k_max]

  correct_predictions = 0
  running_sum = 0

  for i, yp_item in enumerate(y_pred):

    k = i+1 # our rank starts at 1

    if yp_item in y_true:
      correct_predictions += 1
      running_sum += correct_predictions/k

  return running_sum/len(y_true)
```

如果我们将我们的两个列表放入我们的函数中

```py
apk(actuals, predicted)
```

然后我们得到像我们手动示例中那样的**0.5**（图 7）。

## MAP@K

由于 MAP@K 对多个查询进行平均，我们将我们的列表调整为以下结构：

```py
actual = ['p_a', 'p_b']

predic = [
    ['p_a', 'p_b', 'p_c', 'p_d', 'p_e', 'p_f'],
    ['p_c', 'p_d', 'p_e', 'p_f', 'p_a', 'p_b'],
    ['p_d', 'p_a', 'p_c', 'p_b', 'p_e', 'p_f'],
]
```

我们的 `actuals` 保持不变，但我们的 `predicted` 列表现在包含了多个列表（每个查询一个）。`predicted` 列表对应于图 10 中的列表。

![](img/8da4585bdf5304dae0bc165eb1599a95.png)

图 12\. 从图 10 中回顾的示例（图片由作者提供）。

下面的代码显示了 MAP@K 的计算：

```py
import numpy as np
from itertools import product

def mapk(actuals, predicted, k=0):
  return np.mean([apk(a,p,k) for a,p in product([actual], predicted)])
```

如果我们将我们的列表放入这个函数中

```py
mapk(actuals, predicted)
```

然后我们得到**0.59**。

# 结论

在评估推荐系统或排名模型时，MAP@K 是一个很好的选择。它不仅提供了推荐是否相关的洞察，还考虑了正确预测的排名。

由于其排名考虑因素，它比其他标准误差指标如 F1 分数更难以理解。但我希望这篇文章能为你提供关于这个误差指标如何计算和实施的全面解释。

如果你想测量**你的推荐是否相关**以及**它们有多相关**，可以查看归一化折扣累积增益（NDCG）。

# 来源

**信息检索导论 — 评估**，[`web.stanford.edu/class/cs276/handouts/EvaluationNew-handout-1-per.pdf`](https://web.stanford.edu/class/cs276/handouts/EvaluationNew-handout-1-per.pdf)
