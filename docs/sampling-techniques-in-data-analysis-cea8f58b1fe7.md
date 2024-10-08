# 数据分析中的抽样技术

> 原文：[`towardsdatascience.com/sampling-techniques-in-data-analysis-cea8f58b1fe7`](https://towardsdatascience.com/sampling-techniques-in-data-analysis-cea8f58b1fe7)

## 如何为你的数据选择合适的数据抽样方法

[](https://medium.com/@john_lenehan?source=post_page-----cea8f58b1fe7--------------------------------)![John Lenehan](https://medium.com/@john_lenehan?source=post_page-----cea8f58b1fe7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cea8f58b1fe7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cea8f58b1fe7--------------------------------) [John Lenehan](https://medium.com/@john_lenehan?source=post_page-----cea8f58b1fe7--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cea8f58b1fe7--------------------------------) ·阅读时间 6 分钟·2023 年 9 月 6 日

--

![](img/811dd1f1c983cbc25248680f36c27c4c.png)

图片由 [Ryoji Iwata](https://unsplash.com/@ryoji__iwata?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在数据科学项目中，虽然对分析方法和算法的重视程度很高，从数据中提取有意义的见解和发现宝贵信息，但同样重要（甚至可以说更重要）的，是在开始项目之前的数据准备；数据的质量是任何数据分析或机器学习项目的基础。期望从低质量的数据输入中获得高质量的输出是不切实际的——正如谚语所说，垃圾进垃圾出。因此，确保收集到的数据样本具有足够的质量至关重要。那么，如何为你的数据选择合适的抽样技术呢？

![](img/76a29f22283eb29e95128206f9e849b5.png)

图片由 [Ian Parker](https://unsplash.com/@evanescentlight?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我打算概述一些用于数据收集的抽样技术，并提供如何为你的数据选择最优方法的建议。我将描述的抽样方法如下：

1.  简单随机抽样

1.  分层抽样

1.  聚类抽样

1.  系统抽样

每种方法都有其优缺点，某些方法根据数据需求比其他方法更为合适。本文将详细描述这些抽样技术，并举例说明推荐使用这些方法的场景。

## 简单随机抽样

简单随机抽样（SRS）正如其名称所示——样本是从总体中随机选择的，而不考虑其他因素如总体特征。当总体被认为相对同质时，即总体中的每个元素预计都与其他元素相似时，这种方法通常是有效的。

这种方法的优势在于，由于其随机性，数据中很难引入偏差——足够大的样本量理论上会代表总体人口，如果最终目标是建模一般人口行为，这是理想的。不过，这种方法也有一些缺点——即整体中的小子组可能在数据中被低估。在这种情况下，简单随机样本可能不适合目的。

![](img/6da325ec8672447b75a1fb6d36aedf06.png)

图片来源：[罗布·卡伦](https://unsplash.com/@curranrob?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

一个例子是随机挑选城镇居民以进行公共卫生调查——统计学家可能会首先获取所有城镇居民的名单，为每个人分配一个编号，然后使用随机数生成器选择调查样本。然而，如果该调查特别关注城镇老年人口的健康（即超过 90 岁），那么这种方法可能会完全排除这一小部分人群——这意味着在这种调查需求下，简单随机抽样应被舍弃。

## 分层抽样

相比之下，分层抽样直接解决了简单随机抽样的潜在低代表性问题，通过首先根据特征将总体划分为不同的子组（或层次）——回到城镇健康调查的例子，这些层次可以按年龄组进行分组，或进一步按性别或收入进行细分。然后，从每个子组（层次）中随机抽取样本，以构建分析所需的样本群体。

这是一种在确保每个子组有足够代表性的情况下的实际方法。根据调查的需求，统计学家可以从每个层次中选取相等数量的个体，或根据个体在总体人口中的比例选择一定数量的个体——这样调查者可以在调查中保持比例代表性。考虑到这一点，将人口划分为明确的层次可能会很困难——这使得创建分层样本的任务比简单的随机样本更复杂。

![](img/1bb0c972e1fc84c1c5843c78c339bbea.png)

图片来源：[亚历山大·施梅克](https://unsplash.com/@alschim?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## **聚类抽样**

群体抽样方法中，最初将总体分组为不同的群体，然后从中随机选择群体作为样本。在这种情况下，群体抽样与分层抽样有相似之处，因为总体在选择子群体之前会先进行分段。然而，与从每个子群体中随机选择个体不同的是，群体抽样是随机选择子群体。

群体分组通常基于诸如邻近性等因素，中央指导原则是每个群体必须与其他群体区分开。回到城镇健康调查的类比，群体可能基于邻里甚至家庭，其中一些或所有家庭成员被加入到样本中。另一个例子是在生产环境中，随机选择整个批次的产品进行抽样，而不是从每个批次中选择单个单位。这种方法的好处是比逐个检查装配线上的所有单位更为方便。需要注意的一点是确保所有群体彼此独立，以便每个元素只属于一个群体——否则，这可能导致潜在的抽样误差。

![](img/ce56c5d45a5fbf07e2c0ea605323f3d1.png)

照片由 [Marjan Blan](https://unsplash.com/@marjan_blan?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

此外，群体抽样可能由于聚类效应引入偏差——每个群体内的元素是相关的，这可能导致标准误差较大，精度降低，相较于简单随机抽样（SRS）。虽然有方法可以调整这些误差，但这会增加抽样过程的复杂性。

## **系统抽样**

最后，系统抽样涉及在总体中选择一个起始点，然后定期选择每第 n 个项目来增加样本量——这在有可用列表的大型总体中尤其方便。一个例子是在生产线上的后处理测量中，每通过工具的第 10 个产品都会被检查是否有缺陷。在这个例子中，总体的 10%被加入到样本中，以确保机器处理的质量控制。

![](img/b64f1a671fc31c31cf38237572c1d0ee.png)

照片由 [Remy Gieling](https://unsplash.com/@gieling?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这种方法的好处包括数据收集的简单性和效率，同时保持对总体的均匀覆盖。不幸的是，这种方法对元素的排序敏感——如果总体中存在周期性重复的模式，这也可能引入样本偏差。

## 选择合适的抽样方法

如何确定最适合您数据的抽样技术？在选择抽样技术时，需要考虑许多因素，这些因素通常与所进行的分析类型相关。虽然没有一种特定的方法适用于所有场景，但以下陈述是选择抽样方法的良好经验法则：

+   总体中的所有元素同等重要。必须最小化样本偏差。样本需要能代表一般总体。数据收集时不关注总体中的子群体 → **使用简单随机抽样**

+   数据收集中需要代表所有子群体。将总体划分为层次以解决可能的偏差问题 → **使用分层抽样**

+   总体自然地组织成簇。簇内的相似性很小或不存在，这可能导致偏差。簇彼此独立 → **使用簇抽样**

+   总体结构良好且有序。总体中的所有元素同等重要。数据中不存在可能导致偏差的重复模式 → **使用系统抽样**

这并不是选择抽样方法的详尽过程——可能还有其他需要考虑的因素——但通常这种方法适用于绝大多数情况。最终的问题是数据收集过程中哪些数据是重要的，是否解决了潜在的偏差，以及数据收集的潜在限制。最佳的抽样技术将充分解决这些问题——只要在选择抽样方法时牢记这一点，您可以确信获得高质量的数据以满足您的目的。
