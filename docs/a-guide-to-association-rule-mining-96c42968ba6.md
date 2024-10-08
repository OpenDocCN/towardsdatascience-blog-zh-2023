# 关联规则挖掘指南

> 原文：[`towardsdatascience.com/a-guide-to-association-rule-mining-96c42968ba6`](https://towardsdatascience.com/a-guide-to-association-rule-mining-96c42968ba6)

## 使用 Python 通过市场篮子分析创建频繁模式的洞察

[](https://idilismiguzel.medium.com/?source=post_page-----96c42968ba6--------------------------------)![Idil Ismiguzel](https://idilismiguzel.medium.com/?source=post_page-----96c42968ba6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96c42968ba6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----96c42968ba6--------------------------------) [Idil Ismiguzel](https://idilismiguzel.medium.com/?source=post_page-----96c42968ba6--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----96c42968ba6--------------------------------) ·阅读时间 10 分钟·2023 年 4 月 5 日

--

![](img/c4deb918ffc351de2f6044aa03c1a954.png)

图片来源 [Matthias Schröder](https://unsplash.com/@trancepole?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

关联规则挖掘是一种基于规则的机器学习技术，用于发现数据集中频繁出现的模式。频繁模式可能包括通常一起购买的频繁项集或按顺序购买的子序列。例如，对于一家咖啡馆，饼干和咖啡可以是*频繁项集*，而对于一家电子产品商店，笔记本电脑和外接显示器可以是*子序列*。在交易数据库中寻找频繁模式和检测项之间的关联是一个极受欢迎的数据科学用例。一些应用领域包括商品推荐、交叉销售、促销设计、客户行为分析和库存管理。

在本文中，我们将涵盖**市场篮子分析**技术的关联规则挖掘，并回答包括但不限于以下问题：

+   如果顾客购买了商品 A，她还有多大可能购买商品 B？是否存在正相关或负相关，还是完全随机发生？

+   哪些商品应该放置在商店或应用的内容页面上的相邻位置？

+   哪些商品可以在促销活动中捆绑在一起，或在对某一商品表现出兴趣后推荐？

在阅读本文时，你可以查看我在 GitHub 上的 [Jupyter Notebook](https://github.com/Idilismiguzel/Machine-Learning/blob/master/Association_Rule_Mining/market_basket_analysis.ipynb) 以获取完整分析和代码。

# 什么是市场篮子分析？

购物篮是用户选择的商品集合，例如购物车中的商品或网站上消费的内容。市场篮分析帮助我们了解库存中商品之间的**关联**，因此，我们可以利用历史数据预测用户购买行为，并在不知道额外信息的情况下向用户推荐商品。

## 数据

在这篇文章中，我们将使用“[面包篮](https://www.kaggle.com/datasets/mittalvasu95/the-bread-basket)”数据集，该数据集来自位于爱丁堡的一家面包店，包括超过 9000 笔交易。你可以从 Kaggle 下载。

来看看吧！☕️

```py
basket = pd.read_csv("/content/bread_basket.csv")
basket.head()
```

![](img/91ade79624ab8a4e9fbca0608beace99.png)

数据集的前 5 行

如你所见，数据集在`Transaction`列中有交易 ID，每个购买的商品即使属于同一个交易 ID，也有单独的一行。让我们打印出交易 ID=3 的所有商品。

```py
basket.loc[basket['Transaction']==3]
```

![](img/9845e068c4b575151f28c0bd761b2dc2.png)

交易 ID=3

在分析过程中，我们将不会使用`date_time`、`period_day`和`weekday_weekend`列。此外，我们需要将商品显示为布尔值，每个交易 ID 应该在一行中表示。经过此特征工程后，我们将数据集转换为如下形式。

```py
# Groupby by the Transaction Ids and count items
basket = basket.groupby(
              by=['Transaction', 
                  'Item'])['Item'].count().reset_index(name='Item_Count')

# Pivot table by the transaction and convert item count to boolean 
basket = basket.pivot_table(
              index='Transaction', 
              columns='Item', 
              values='Item_Count', 
              aggfunc='sum').fillna(0).astype(bool)
```

![](img/23fedc78d81a907531c1e21076d67072.png)

数据集经过特征工程后的前 5 行

# 关联规则是什么？

市场篮分析基于使用**关联规则**发现商品之间的关联，这些规则采取 if-then 关系的形式。要构建一个关联规则，我们应该至少有一个**前提**和一个**结果**。

+   一个前提和一个结果：如果{🍪}则{☕️}

+   多重前提：如果{🍪, 🍰}则{☕️}

+   多重结果：如果{🍪}则{☕️, 🥛}

总规则数随唯一项的数量增长。正如你所想，并非所有规则都同样重要，我们需要**指标**来帮助我们识别哪些规则应该被淘汰，哪些应该被考虑。

## 支持度

支持度是衡量规则有趣性和重要性的主要指标。它可以应用于单个项或前提和结果的对。它通过将包含特定项的交易数除以总交易数来计算。支持度值范围从 0 到 1。

![](img/cd0aaeb3206a2245dbe2f4da013227d6.png)

例如，如果我们有 10 笔交易，其中 6 笔包括咖啡，4 笔同时包括咖啡和饼干，则{咖啡}的支持度为 60%，{咖啡, 饼干}的支持度为 40%。注意，如果{咖啡}则{饼干}和如果{饼干}则{咖啡}的支持度都是 40%。让我们计算数据集上的支持度。

```py
# Support value for single items
support = basket.mean().sort_values(ascending=False)
support.head(10)
```

![](img/fa1000546a5d0a630eb2e6c57e9d77b9.png)

前 10 的支持度值

如果我们想找到一对前提和结果的支持度值，可以按照以下方式添加它们。

```py
# Add antecedent and consequent pairs to the data set 
basket['Coffee & Bread'] = np.logical_and(basket['Coffee'], basket['Bread'])
basket['Coffee & Tea'] = np.logical_and(basket['Coffee'], basket['Tea'])
basket['Coffee & Cake'] = np.logical_and(basket['Coffee'], basket['Cake'])

# Calculate support
support = basket.mean().sort_values(ascending=False)
support.head(10)
```

![](img/f7f7e2def33cd2e46256d0ba9b1fbf69.png)

添加新对后前 10 的支持度值

## 置信度

置信度是购买项目 B 的概率，前提是他们已经购买了项目 A。它通过将项目 A&B 的支持度除以项目 A 的支持度来计算。置信度值的范围是 0 到 1。

重要的是要将支持度与置信度一起使用，因为如果仅使用支持度指标，流行的项目可能会误导结果的解释。

![](img/fb803052d2616b5fc768e686b848a57c.png)

```py
# Confidence value of if Coffee then Bread 
print(support['Coffee & Bread']/support['Coffee'])

# Confidence value of if Coffee then Cake 
print(support['Coffee & Cake']/support['Coffee'])
```

> 0.18 对于咖啡 → 面包
> 
> 0.11 对于咖啡 → 蛋糕

我们可以将这些结果解释为 18%的置信度意味着 18%的包含咖啡的交易也包含面包。

重要的是要提到，置信度指标不是对称的，Confidence(A→B) 与 Confidence(B→A) 是不同的。

我们用以下符号总结支持度和置信度，并且在修剪关联规则时，检查规则是否同时满足最小支持度和最小置信度水平是很重要的。

![](img/64f7de3d4c3c345458218fa0152b21af.png)

## 提升度

提升度指标用于检测无趣的关联规则以便于规则修剪。它假设在交易中，项目 A 的出现与项目 B 的出现是独立的，如果 *P*(*A* ∪ *B*) = *P*(*A*)*P*(*B*)，否则这两个项目是依赖的，因此**相关**。它通过将包含项目 A 和 B 的交易比例除以项目 A 和 B 独立出现的比例来计算。提升度值的范围是 0 到无穷大。

![](img/915c7bb4ff7a030961ced891244f4cac.png)

> Lift(A → B) > 1 意味着项目是**正相关**的，一个项目的出现会正面影响另一个项目的出现。
> 
> Lift(A → B) =1 意味着没有相关性
> 
> Lift(A → B) < 1 意味着项目是**负相关**的，一个项目的出现会负面影响另一个项目的出现。

```py
# Lift value of if Coffee then Bread 
print(support['Coffee & Bread']/(support['Coffee']*support['Bread']))

# Lift value of if Coffee then Cake 
print(support['Coffee & Cake']/(support['Coffee']*support['Cake']))
```

> 0.57 对于咖啡 → 面包
> 
> 1.10 对于咖啡 → 蛋糕

正如你所见，在交易中有咖啡会减少有面包的机会，同时增加有蛋糕的机会。

此外，请注意提升度指标是对称的，这意味着 Lift(A→B) 等于 Lift(B→A)。

## 杠杆度

杠杆度是衡量在交易中同时出现项目 A 和 B 与项目 A 和 B 独立出现之间差异的度量。它通过从包含项目 A 和 B 的交易比例中减去项目 A 和 B 独立出现的比例来计算。杠杆度值的范围是-1 到 1。

![](img/f13b75a6c4b566db65d8ed26a6a22440.png)

```py
# Leverage value of if Coffee then Bread 
print(support['Coffee & Bread'] - (support['Coffee']*support['Bread']))

# Leverage value of if Coffee then Cake 
print(support['Coffee & Cake'] - (support['Coffee']*support['Cake']))
```

> -0.06 对于咖啡 → 面包
> 
> 0.005 对于咖啡 → 蛋糕

由于杠杆度值的范围是-1 到 1，我们可以用它来比较不同的配对。

## 确信度

确信度指标用于衡量结果项依赖于前项的程度。它通过将包含项目 A 的交易比例与不包含项目 B 的交易比例相乘，然后除以包含项目 A 而不包含项目 B 的交易比例来计算。确信度值的范围是 0 到无穷大。

![](img/6b3e2804fdd6b4b1d8109f74788e0d4b.png)

如果置信度值很高，这意味着结果是高度依赖于前提的。

```py
# Conviction value of if Coffee then Bread 
print(support['Coffee']*(
        1-support['Bread'])/(support['Coffee'] - support['Coffee & Bread']))

# Conviction value of if Coffee then Cake 
print(support['Coffee']*(
        1-support['Cake'])/(support['Coffee'] - support['Coffee & Cake']))
```

如你所见，我们计算了*非*面包的支持度为`1-support(Bread)`，以及咖啡和*非*面包的支持度为`support(Coffee) — support(Coffee & Bread)`。

> 0.82 对于咖啡 → 面包
> 
> 1.01 对于咖啡 → 蛋糕

## 综合所有内容

之前，我们学习了支持度、置信度、提升度、杠杆效应和置信度度量，并手动计算了它们。现在让我们把它们结合起来，改用`[mlxtend](http://rasbt.github.io/mlxtend/)`库进行计算。

我们首先使用`[apriori](https://github.com/rasbt/mlxtend/blob/master/mlxtend/frequent_patterns/apriori.py)`函数从我们的事务数据中创建频繁项集。在该函数中，我们可以定义一个支持度度量的最小阈值，这对于剪枝非常有用。我决定将最小支持度定义为 0.01，但你可以尝试不同的值。

```py
from mlxtend.frequent_patterns import association_rules, apriori

# Apply Apriori algorithm
frequent_itemsets = apriori(basket, min_support=0.01, use_colnames=True)
```

让我们看看`frequent_itemsets`的前 20 项。

```py
frequent_itemsets.sort_values(by='support', ascending=False).head(20)
```

![](img/07a1cb406f122c0f5ef96535faf67d46.png)

生成的 frequent_itemsets 的前 20 行

你可以看到 apriori 算法自动计算了唯一项和前提-结果对的支持度度量。

接下来，我们将通过使用`[association_rules](https://github.com/rasbt/mlxtend/blob/master/mlxtend/frequent_patterns/association_rules.py)`函数生成规则。在 association_rules 中，我们可以选择一个评估指标（`'support', 'confidence', 'lift', 'leverage', 'conviction', or 'zhangs_metric'`）并为其设置最小阈值。这里，我选择了‘confidence’作为评估指标，并将最小阈值设置为 0.5。可以根据感兴趣的规则自由更改指标。

```py
# Compute association rules
rules = association_rules(frequent_itemsets, metric="confidence", 
  min_threshold=0.5)
```

让我们来看一下`rules`——输出的 pandas 数据框。

![](img/650685d724c3fabb1d705e66fecebbe3.png)

由 association_rules 生成的规则

太棒了！🚀 我们现在有了一组非常有趣的规则，这些规则的置信度大于 0.5，提升度大于 1（意味着正相关），具有正向杠杆效应，并且置信度高。还要注意，我们的起点是规则总数与项数成指数关系，而我们最终得到了 10 条最重要和有趣的规则。

现在我们将查看一种绘图技术来帮助我们可视化。你可能认为这不是必需的，因为我们只有 10 条规则，然而，如果你有很多规则或需要展示，好的视觉效果会大有帮助。

我认为`PyCaret`库中的`plot_model`函数非常易于使用和解释，以可视化关联规则。但请注意，在撰写本文时 pycaret 版本 3.0 已发布，但为了使用这个特定的函数，我不得不降级到 pycaret 版本 2.3。

```py
!pip install pycaret==2.3

from pycaret.arules import plot_model

# Plot rules
plot_model(rules, plot = '2d')
```

![](img/9724992165b5215b64d156d5cb14b983.png)

可视化关联规则

如你所见，我们在左侧绘制了置信度，在右侧使用色彩图绘制了提升度，并在 x 轴上绘制了支持度。从图表中可以明显看出，Toast 具有最高的提升度和置信度，这使其成为 Coffee 的一个很好的前提。如果你将鼠标悬停在 Toast 上，它会显示所有指标计算值。

![](img/1218985a5af71114b1cc9a0dec71ec21.png)

Coffee 的关联规则可视化

你可以通过查看图表来轻松地对规则进行排名，以便进一步评估。但一般来说，所有这些前提都可以得到 Coffee 作为推荐。

在筛选规则时，可以随意重新设置最小阈值或更改评估指标，以允许不同的结果和更大的项集。

# 结论

在这篇文章中，我们介绍了关联规则挖掘，并学习了如何使用市场篮子分析技术将其应用于数据集。我们学习了支持度、置信度、提升度、杠杆度和定罪度指标，并且通过手动和使用 mlxtend 库计算了这些指标。我们还看到了如何设置这些指标的最小阈值以筛选出不感兴趣的规则。最后，我们得到了 10 条有趣的规则，并使用 pycaret 库对它们进行了可视化。

希望你喜欢阅读关于关联规则的内容，并发现这篇文章有用！✨

**喜欢这篇文章吗？** [**成为会员获取更多内容！**](https://idilismiguzel.medium.com/membership)

*你可以*[***在这里阅读我的其他文章***](https://medium.com/@idilismiguzel)*并* [***在 Medium 上关注我。***](https://medium.com/@idilismiguzel/follow)如有任何问题或建议，请告诉我。✨

参考文献

1.  来自 [Kaggle](https://www.kaggle.com/datasets/mittalvasu95/the-bread-basket) 的 Bread Basket 数据集，许可证 [CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)

1.  头图由 [Matthias Schröder](https://unsplash.com/@trancepole?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄

1.  所有其他图片均由作者提供

1.  [Mlxtend 源代码](https://github.com/rasbt/mlxtend/tree/master/mlxtend/frequent_patterns)

1.  [PyCaret 源代码](https://pycaret.gitbook.io/docs/get-started/installation)
