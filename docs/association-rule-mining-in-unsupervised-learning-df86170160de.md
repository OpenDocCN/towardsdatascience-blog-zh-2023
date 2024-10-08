# 无监督学习中的关联规则挖掘

> 原文：[`towardsdatascience.com/association-rule-mining-in-unsupervised-learning-df86170160de`](https://towardsdatascience.com/association-rule-mining-in-unsupervised-learning-df86170160de)

## 数据挖掘中的模式发现术语和概念

[](https://kayjanwong.medium.com/?source=post_page-----df86170160de--------------------------------)![Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----df86170160de--------------------------------)[](https://towardsdatascience.com/?source=post_page-----df86170160de--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----df86170160de--------------------------------) [Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----df86170160de--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----df86170160de--------------------------------) ·5 分钟阅读·2023 年 1 月 25 日

--

![](img/a6785c84b324d9b94cdc3e0fc0f84db0.png)

[Kier... in Sight](https://unsplash.com/@kierinsight?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 的照片

模式发现试图揭示大数据集中的模式，这为许多数据挖掘任务奠定了基础，如关联分析、相关性分析和因果分析、聚类分析等。

本文介绍了关联规则挖掘中的常见术语，并随后讲解了频繁模式和序列模式的关联规则挖掘技术。

# 目录

+   [术语：支持度、置信度、提升度、杠杆度、确信度](https://medium.com/p/df86170160de/#2842)

## **频繁模式**

+   [Apriori 算法](https://medium.com/p/df86170160de/#035c)

+   [等价类转换（ECLAT）](https://medium.com/p/df86170160de/#5f42)

+   [频繁模式增长（FP-Growth）](https://medium.com/p/df86170160de/#a37a)

## 序列模式

+   [广义序列模式（GSP）](https://medium.com/p/df86170160de/#e8ad)

+   [前缀投影序列模式挖掘（PrefixSpan）](https://medium.com/p/df86170160de/#4226)

# 术语

+   **支持度**：项`X`出现的概率，表示为`P(X)`，衡量项的受欢迎程度

+   **置信度**：在获取项`Y`后获取项`X`的条件概率，表示为`P(X|Y)`

+   **提升度**：置信度除以支持度，表示为`P(X|Y)/P(X)`，衡量项的独立性以及在添加项`Y`到购物车后，项`X`被购买的可能性***更高***

+   **杠杆度**：两项的支持度之差与两项独立时的预期支持度之差，表示为`P(XUY)-P(X)P(Y)`，衡量项的独立性

+   **置信度**：支持度除以置信度，记作 `(1-P(X))/(1-P(X|Y))`，衡量项的独立性，高置信度是 `Y->X` 强置信度和 `X` 低支持度的组合。

# Apriori 算法

> 水平广度优先搜索算法

Apriori 算法通过指定**最低置信度阈值**来识别关联。关联的直觉在于对一个后继项在一个前驱项被选择后被选择的*信心*，记作 `P(consequent|antecedent)`。

![](img/0a962143017ac934fd470d8abb84c798.png)

图 1：事务数据示例 — 作者提供的图片

例如，在图 1 中，`Confidence(A->C) = P(C|A) = 0.75`，因为项 `C` 在 `A` 后被购买的次数为 4 次中的 3 次。如果此置信度高于最低置信度阈值（例如 0.5），则可以得出 `A->C` 的关联。

通过应用向下封闭原则来加速频繁项集的搜索，而不是计算每个项集之间的置信度。**向下封闭原则**指出，任何频繁项集的子集必须也是频繁的，例如，如果项集 `A, B, C` 是频繁的，则它的子集项集 `A, B` 也必须是频繁的。

Apriori 算法的缺点是必须反复扫描数据以计算每个项或项集的支持度和置信度。

# 等价类转换（ECLAT）

> 使用集合交集的垂直深度优先搜索算法

在 Apriori 算法中，数据被视为**水平事务级数据**，其中每个事务包含一个或多个项。在 ECLAT 中，数据被转换为**垂直项级数据**，每个项具有一个包含其出现的事务 ID 的集合（称为 `TIDset`）。

![](img/d677c16990e0f52f5f618476fe4f6404.png)

图 2：垂直项级数据示例 — 作者提供的图片

根据图 1 中的事务数据示例，可以将其重新排列为垂直项级数据，如图 2 所示。例如，项 `B` 出现在事务 1、2 和 3 中，这将导致 `TIDset {1, 2, 3}`。

项的支持度和置信度计算可以通过 `TIDset` 之间的集合交集完成。ECLAT 算法比 Apriori 算法更节省内存和计算，因为它使用深度优先搜索，不需要多次扫描数据以获取每个项的支持度。

# 频繁模式增长（FP-Growth）

> 使用 Trie 数据结构的垂直深度优先搜索算法

FP-Growth 的直觉是找到频繁单项，并根据每个这样的项对数据库进行分区，然后递归地为每个分区数据库增长频繁模式。这些操作通过 Trie 数据结构（FP-树）有效完成。

数据被扫描两次 — 第一次是查找单项频繁模式（满足最低支持度），第二次是构建 FP-树。

![](img/76f8d9b7df9f3d72ebcf4667b41f0063.png)

图 3：频繁模式的样本 Trie 数据结构 — 图片来源于作者

根据图 1 中的交易数据示例，可以创建如图 3 所示的 FP 树。交易 1 和 2 路径为 `A-B-C-D`，交易 3 路径为 `A-B-D`，交易 4 路径为 `A-C`。

![](img/f59802a204b2fbef57e5d31da39ab4b1.png)

图 4：条件模式基 — 图片来源于作者

从这个 FP 树中，我们可以轻松计算条件模式基（图 4），例如项 `C` 与 `{A, B}` 一起出现两次，与 `{A}` 一次 — 无需重新扫描数据。

# 广义序列模式（GSP）

> 基于 Apriori 的序列模式挖掘，广度优先搜索

GSP 与 Apriori 算法类似，其中数据首先扫描单项序列并筛选频繁序列。然后，数据会不断扫描和筛选以检索更长的子序列。需要注意的是，项在子序列中不必连续，模式可以包含重复的项。

其他基于 Apriori 的序列模式挖掘包括 [使用等效类的序列模式发现（SPADE）](http://www.philippe-fournier-viger.com/spmf/SPADE.pdf)算法，该算法类似于 ECLAT 算法。

# 前缀投影序列模式挖掘（PrefixSpan）

> 基于模式增长的序列模式挖掘，深度优先搜索

PrefixSpan 将条目分为前缀和后缀。捕获频繁的前缀，并将前缀的投影作为后缀。

PrefixSpan 通过一次扫描数据来查找长度为 1 的序列模式，并递归扩展频繁的长度为 1 的序列模式。扩展是通过将长度为 1 的模式作为前缀，并将以长度为 1 的模式开头的其余项作为后缀（称为投影数据库）来完成的，并递归地增加前缀的长度。每次迭代后，投影数据库会缩小，因为后缀的长度减少。

该算法速度较慢，但可以通过伪投影优化，使用指针代替物理复制后缀。

其他基于模式增长的序列模式挖掘包括 [频繁模式投影序列模式挖掘（FreeSpan）](https://www.researchgate.net/publication/221654035_FreeSpan_Frequent_pattern-projected_sequential_pattern_mining)，其效率不如 PrefixSpan。

与聚类相比，聚类是另一种无监督学习的主题，我觉得关联规则挖掘在统计学上更有基础，使其更具挑战性。不过，希望这篇文章对一些流行的关联规则挖掘技术提供了一个概述！

# 相关链接

+   术语定义： [`rasbt.github.io/mlxtend/user_guide/frequent_patterns/association_rules/`](http://rasbt.github.io/mlxtend/user_guide/frequent_patterns/association_rules/)

## 论文

+   频繁模式: [ECLAT](https://www.researchgate.net/publication/303523871_ECLAT_Algorithm_for_Frequent_Item_sets_Generation)、[FP-Growth](https://borgelt.net/papers/fpgrowth.pdf)

+   基于 Apriori 的序列模式: [SPADE](http://www.philippe-fournier-viger.com/spmf/SPADE.pdf)

+   基于模式增长的序列模式: [PrefixSpan](http://hanj.cs.illinois.edu/pdf/span01.pdf)、[FreeSpan](https://www.researchgate.net/publication/221654035_FreeSpan_Frequent_pattern-projected_sequential_pattern_mining)
