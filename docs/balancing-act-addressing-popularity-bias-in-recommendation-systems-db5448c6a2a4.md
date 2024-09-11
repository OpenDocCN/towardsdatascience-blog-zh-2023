# 平衡行动：解决推荐系统中的受欢迎度偏见

> 原文：[https://towardsdatascience.com/balancing-act-addressing-popularity-bias-in-recommendation-systems-db5448c6a2a4?source=collection_archive---------3-----------------------#2023-08-18](https://towardsdatascience.com/balancing-act-addressing-popularity-bias-in-recommendation-systems-db5448c6a2a4?source=collection_archive---------3-----------------------#2023-08-18)

[](https://medium.com/@pratikaher?source=post_page-----db5448c6a2a4--------------------------------)[![Pratik Aher](../Images/5648c040ff967717c94657ebfff11e2b.png)](https://medium.com/@pratikaher?source=post_page-----db5448c6a2a4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db5448c6a2a4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----db5448c6a2a4--------------------------------) [Pratik Aher](https://medium.com/@pratikaher?source=post_page-----db5448c6a2a4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc2e5b1d7be67&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbalancing-act-addressing-popularity-bias-in-recommendation-systems-db5448c6a2a4&user=Pratik+Aher&userId=c2e5b1d7be67&source=post_page-c2e5b1d7be67----db5448c6a2a4---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----db5448c6a2a4--------------------------------) · 7 分钟阅读 · 2023年8月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdb5448c6a2a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbalancing-act-addressing-popularity-bias-in-recommendation-systems-db5448c6a2a4&user=Pratik+Aher&userId=c2e5b1d7be67&source=-----db5448c6a2a4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdb5448c6a2a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbalancing-act-addressing-popularity-bias-in-recommendation-systems-db5448c6a2a4&source=-----db5448c6a2a4---------------------bookmark_footer-----------)![](../Images/e24e03cb4bd4c3ed79aa9a0afd068cd0.png)

图片由 [Melanie Pongratz](https://unsplash.com/@melanie_sophie?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

一天早上，你决定犒劳自己，买一双新鞋。你打开了最喜欢的运动鞋网站，浏览了系统推荐的商品。特别有一双鞋引起了你的注意——你喜欢它的风格和设计。你毫不犹豫地购买了它们，迫不及待地想穿上你的新鞋。

当鞋子到达时，你迫不及待地想展示它们。你决定在即将到来的音乐会上穿着它们。不过，当你到达场地时，你注意到至少有10个人穿着完全一样的鞋子！这概率有多大？

突然间你感到失望。尽管你最初很喜欢这些鞋子，但看到这么多人穿着同样的鞋子让你觉得你的购买并不那么特别了。你本以为会让你与众不同的鞋子，最终却让你融入了人群。

那一刻，你发誓再也不从那个运动鞋网站购买东西了。尽管他们的推荐算法建议了你喜欢的物品，但最终没有带给你你所期望的满足感和独特性。所以尽管你最初欣赏那个推荐的物品，但整体体验让你感到不满。

这突显了推荐系统的局限性——推荐一个“好”的产品并不保证它会带来积极和满足的客户体验。那么，最终这真的是一个好的推荐吗？

# 为什么衡量推荐系统中的受欢迎度偏见至关重要？

受欢迎度偏见发生在推荐系统建议大量全球受欢迎的物品而非个性化推荐时。这是因为算法通常被训练来最大化用户的参与度，通过推荐许多用户喜欢的内容来实现。

虽然受欢迎的物品仍然可能相关，但过度依赖受欢迎程度会导致缺乏个性化。推荐变得泛泛而谈，无法考虑个人兴趣。许多推荐算法通过优化总体受欢迎度的指标来进行调整。这种对已有受欢迎度的系统性偏向，随着时间的推移可能会成为问题。它导致了对热门或病毒式传播物品的过度推广，而不是独特的建议。在商业方面，受欢迎度偏见还可能导致公司拥有大量小众、鲜为人知的库存，这些库存未被用户发现，导致难以销售。

考虑特定用户偏好的个性化推荐可以带来巨大的价值，特别是对于与主流不同的小众兴趣。它们帮助用户发现专门为他们量身定制的新奇物品。

理想情况下，推荐系统应在受欢迎程度和个性化之间取得平衡。目标应该是展示与每个用户产生共鸣的隐藏宝石，同时偶尔加入一些具有普遍吸引力的内容。

# 如何衡量受欢迎度偏见？

## **平均推荐受欢迎度**

平均推荐受欢迎度（ARP）是一个用来评估推荐列表中物品受欢迎程度的指标。它根据物品在训练集中收到的评分数量来计算物品的平均受欢迎度。从数学上讲，ARP 的计算方法如下：

![](../Images/5b9e9cc7ff8d8e84706afa6016492f4e.png)

其中：

+   |U_t| 是用户的数量

+   |L_u| 是用户 u 的推荐列表 L_u 中的项目数量。

+   ϕ(i) 是在训练集中“项目 i”被评分的次数。

简单来说，ARP 通过将推荐列表中所有项目的受欢迎程度（评分数量）加总，然后在测试集中的所有用户之间取平均值来衡量项目的平均受欢迎程度。

![](../Images/afe59f7e167d45d3ba7fedb7d6ae6051.png)

示例：假设我们有一个包含 100 个用户的测试集 |U_t| = 100。对于每个用户，我们提供一个 10 个项目的推荐列表 |L_u| = 10。如果项目 A 在训练集中被评分 500 次（ϕ(A) = 500），而项目 B 被评分 300 次（ϕ(B) = 300），这些推荐的 ARP 可以计算为：

在这个例子中，ARP 值为 8，表示推荐项目在所有用户中的平均受欢迎程度为 8，基于它们在训练集中获得的评分数量。

## 长尾项目的平均百分比（APLT）

长尾项目的平均百分比（APLT）指标，计算推荐列表中长尾项目的平均比例。它的表达式为：

![](../Images/a6ecb9e2ba22abfd3920829db88fa842.png)

这里：

+   |Ut| 代表用户的总数。

+   u ∈ Ut 表示每个用户。

+   Lu 代表用户 u 的推荐列表。

+   Γ 代表长尾项目集合。

简单来说，APLT 量化了推荐中较少受欢迎或利基项目的平均百分比。较高的 APLT 表示推荐中包含了更多的此类长尾项目。

示例：假设有 100 个用户（|Ut| = 100）。对于每个用户的推荐列表，平均来说，50 个项目中的 20 个（|Lu| = 50）属于长尾集合 (Γ)。使用公式，APLT 为：

APLT = Σ (20 / 50) / 100 = 0.4

因此，在这种情况下，APLT 为 0.4 或 40%，这意味着，平均而言，推荐列表中的 40% 项目来自长尾集合。

## 长尾项目的平均覆盖率（ACLT）

长尾项目的平均覆盖率（ACLT）指标评估了在整体推荐中包含的长尾项目的比例。与 APLT 不同，ACLT 考虑了所有用户中长尾项目的覆盖情况，并评估这些项目在推荐中的有效表示。它的定义为：

ACLT = Σ Σ 1(i ∈ Γ) / |Ut| / |Lu|

这里：

+   |Ut| 代表用户的总数。

+   u ∈ Ut 表示每个用户。

+   Lu 代表用户 u 的推荐列表。

+   Γ 代表长尾项目集合。

+   1(i ∈ Γ) 是一个指示函数，如果项目 i 在长尾集合 Γ 中，则等于 1，否则为 0。

简单来说，ACLT 计算了每个用户推荐中长尾项目的平均比例。

示例：假设有100个用户（|Ut| = 100）和500个长尾项目（|Γ| = 500）。在所有用户的推荐列表中，有150个长尾项目被推荐（Σ Σ 1(i ∈ Γ) = 150）。所有推荐列表中的项目总数为3000（Σ |Lu| = 3000）。使用公式，ACLT为：

ACLT = 150 / 100 / 3000 = 0.0005

因此，在这种情况下，ACLT为0.0005或0.05%，这表明平均而言，0.05%的长尾项目被包含在总体推荐中。这个指标有助于评估推荐系统中小众项目的覆盖率。

## 如何减少推荐系统中的流行度偏差

**流行度感知学习**

这个想法受到[位置感知学习](https://www.researchgate.net/publication/335771749_PAL_a_position-bias_aware_learning_framework_for_CTR_prediction_in_live_recommender_systems)（PAL）的启发，该方法是要求你的机器学习模型同时优化排序相关性和位置影响。我们可以用相同的方法结合流行度得分，这个得分可以是上面提到的任何得分，比如平均推荐流行度。

+   在训练时，你使用项目流行度作为输入特征之一

+   在预测阶段，你将其替换为一个常量值。

![](../Images/0f733eace60b4349fe82def52c50931b.png)

作者提供的图片

## xQUAD框架

解决流行度偏差的一个有趣方法是使用称为xQUAD框架的东西。它结合当前模型的推荐列表（R）和概率/可能性得分，构建一个新列表（S），这个新列表要多样化得多，其中|S| < |R|。这个新列表的多样性由超参数λ控制。

我尝试总结框架的逻辑：

![](../Images/f558fbdac473f2e7e97fc8b3be1bde60.png)

作者提供的图片

我们计算集合R中所有文档的得分。我们取得分最高的文档，将其添加到集合S中，同时将其从集合R中移除。

![](../Images/0d176c4039609b8a638cf987cd188765.png)

作者提供的图片

![](../Images/fd8c15c849f96f23af3e0c9b0573acc9.png)

作者提供的图片

要选择下一个添加到‘S’的项目，我们计算R\S（R去掉S）中每个项目的得分。对于每个选择添加到“S”的项目，P(v/u)会上升，因此一个不受欢迎项目再次被选中的机会也会增加。

如果你喜欢这个内容，可以在[linkedin](https://www.linkedin.com/in/pratikdaher/)找到我 :).

## 参考文献

[https://arxiv.org/pdf/1901.07555.pdf](https://arxiv.org/pdf/1901.07555.pdf)

[https://www.ra.ethz.ch/cdstore/www2010/www/p881.pdf](https://www.ra.ethz.ch/cdstore/www2010/www/p881.pdf)

[](https://www.analyticsvidhya.com/blog/2023/03/how-to-overcome-position-bias-in-recommendation-and-search/?source=post_page-----db5448c6a2a4--------------------------------) [## 如何克服推荐和搜索中的位置偏差？

### 在这篇文章中，我们将讨论如何通过逆倾向加权（Inverse Propensity Weighting）克服位置偏差及其缺点……

[www.analyticsvidhya.com](https://www.analyticsvidhya.com/blog/2023/03/how-to-overcome-position-bias-in-recommendation-and-search/?source=post_page-----db5448c6a2a4--------------------------------)
