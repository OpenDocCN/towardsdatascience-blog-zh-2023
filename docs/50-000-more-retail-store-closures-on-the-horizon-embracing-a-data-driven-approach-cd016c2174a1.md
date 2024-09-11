# 50,000家零售店关店在即：拥抱数据驱动和以客户为中心的方法

> 原文：[https://towardsdatascience.com/50-000-more-retail-store-closures-on-the-horizon-embracing-a-data-driven-approach-cd016c2174a1?source=collection_archive---------20-----------------------#2023-01-12](https://towardsdatascience.com/50-000-more-retail-store-closures-on-the-horizon-embracing-a-data-driven-approach-cd016c2174a1?source=collection_archive---------20-----------------------#2023-01-12)

## 盈利店铺队伍优化指南

## 如何分析服务区、店铺网络、销售转移和收购影响

[](https://medium.com/@martinleitner_33020?source=post_page-----cd016c2174a1--------------------------------)[![马丁·莱特纳](../Images/f069e0e14888c52f4689518a23cc20f3.png)](https://medium.com/@martinleitner_33020?source=post_page-----cd016c2174a1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd016c2174a1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cd016c2174a1--------------------------------) [马丁·莱特纳](https://medium.com/@martinleitner_33020?source=post_page-----cd016c2174a1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb910204cd9bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F50-000-more-retail-store-closures-on-the-horizon-embracing-a-data-driven-approach-cd016c2174a1&user=Martin+Leitner&userId=b910204cd9bf&source=post_page-b910204cd9bf----cd016c2174a1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd016c2174a1--------------------------------) · 7分钟阅读 · 2023年1月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcd016c2174a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F50-000-more-retail-store-closures-on-the-horizon-embracing-a-data-driven-approach-cd016c2174a1&user=Martin+Leitner&userId=b910204cd9bf&source=-----cd016c2174a1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcd016c2174a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F50-000-more-retail-store-closures-on-the-horizon-embracing-a-data-driven-approach-cd016c2174a1&source=-----cd016c2174a1---------------------bookmark_footer-----------)![](../Images/0ebd9544f464c00a09cb71e98767bce2.png)

图片由 [Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在过去的一年里，许多零售商因客户偏好变化、竞争加剧和运营费用上升而不得不关闭大量门店。一些宣布重大店铺关闭的零售商例子包括：

1.  J.C. Penney — 这家百货公司连锁店已宣布计划在未来几年关闭200多家门店，因为它在与在线零售商竞争中挣扎。

1.  梅西百货 — 这家百货公司连锁店已宣布计划在未来几年内关闭100多家门店，以适应消费者行为的变化。

1.  西尔斯 — 这家陷入困境的零售商已宣布计划关闭200多家门店，努力扭转业务局面。

1.  古驰 — 这家服装零售商已宣布计划关闭约200家门店，以精简运营。

1.  维多利亚的秘密 — 这家内衣零售商已宣布计划关闭约250家门店，因为面临来自在线零售商的激烈竞争。

这些只是过去一年中宣布重大店铺关闭计划的一些零售商例子。在此期间还有许多其他零售商也关闭了门店。分析师估计，仅在美国，短期内我们将看到超过50,000家门店的关闭。与其依赖直觉和 Excel 技能，不如采用以数据驱动和客户为中心的方法来提高门店连锁的盈利能力。

## 如何决定关闭哪些门店？

我通常看到的情况是，财务团队根据盈利能力对各个门店进行排名。然后，他们开始关闭那些盈利能力最差的门店，假设他们可以迅速谈判退出租约，因为这些门店在资产负债表上负担很重。

这很有道理。或者说有没有其他方法？一种**包括客户视角的方式**，而不仅仅是关于汇总销售和利润的方式。

让我解释一下。当你关闭一家门店时，客户有几种选择。他们可能会去距离较近的邻近门店；他们可能会去竞争对手那里；他们可能会去你的在线商店；他们可能会完全停止购买这一类别的商品而节省开支，或者上述几种情况的组合。这些选择主要集中在现有客户基础上，涉及客户留存。你跟上了吗？**大多数门店销售来自现有客户基础**，特别是你的忠实购物者，这毫无疑问。然而，现实是，门店对于零售商或百货商店的客户获取策略也很重要，因为门店能创造品牌认知并作为营销工具。那么，如何将这一点考虑到门店关闭的方程中呢？

> 回顾一下，我们讨论了两个要素 — 门店关闭对客户留存和客户获取的影响。

突然间，这听起来更像是一个**数据科学问题**而非财务排名练习。

![](../Images/9e286f435139ecb7fab31c604ceac99f.png)

摄影作品由 [Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 数据驱动与以客户为中心的方法

在您采取这种方法为您的CFO/COO制定全面建议时，有几个事项需要考虑。

+   服务区：如何定义围绕我的商店的重要区域

+   商店网络：商店之间的关系是什么

+   预测客户销售转移

+   预测获取新客户的影响

+   综合分析（1年、3年影响，排名）

+   测量与模型改进

## 服务区/贸易区

服务区是指某个特定业务接收客户的地理区域。服务区的定义基于各种因素，如距离、交通可达性和市场需求。它们对于房地产开发、分析和业务规划至关重要。

服务区的计算方式：1.) 在传统方式中，人们会在商店周围画一个一定英里半径的圆，比如三英里，圆内的区域都会被视为服务区。请不要这样做。2.) 第一种方法的进阶方式是使用驾车时间来定义服务区。例如，以您的商店为中心的30分钟驾车时间就是服务区。

**以客户为中心的建议：** 使用过去12或24个月在商店购物的客户 *(根据您的业务模型，您需要自行判断时间范围)*，并将他们的居住地点进行映射，例如，按人口普查区块组级别。当您完成后，您将发现，区域可能会非常大，通常覆盖美国的大部分区域。缩小范围的一种方法是应用优化函数，使用多边形并努力最小化区域的大小，同时保持累计销售百分比尽可能大。我发现，在一个最优解中，累计销售捕捉率通常在70%到80%之间。好了，您现在有了所有商店的定制服务区。

## 商店网络

不要再独立对待每个商店，以决定如何以及在哪里扩展或缩减商店网络。了解商店之间的连接超越服务区是做出更聪明优化决策的关键。

**以客户为中心的建议：** 从您的目标商店的客户基础开始。然后查看这些客户还在其他商店（包括电子商务）购物的情况。根据您商店在特定地理区域的分布密度，您可能会对有多少客户在多个地点购物感到惊讶。通常，这些客户是您更忠诚且高价值的客户。

您会发现有三组客户：

1.  专门在这家商店购物的客户

1.  花大部分钱在这家店而在其他店（包括电子商务）花较少的钱的客户

1.  在这家店花少量钱而在其他店（包括电子商务）花最多钱的客户

因此，你现在有了基于客户消费行为的忠诚度店铺，并且你也了解他们还在哪里购买你的产品。这是网络的基础。**不过要注意：** 很可能需要修剪你的网络以保持其规模，否则它们可能会迅速膨胀，覆盖到所有店铺。这是因为客户为了业务或休闲旅行到其他州，并在他们不会在此行之外出现的地点购买商品。你可以通过建立最低$贡献规则来轻松做到这一点。

现在，你应该拥有覆盖整个店铺网络的各种规模，从孤立区域的单店网络到包含十几家店铺的更大网络。

![](../Images/08de88845019a30d1a976e3179a3030b.png)

[Omar Flores](https://unsplash.com/@designedbyflores?utm_source=medium&utm_medium=referral) 的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 预测店铺关闭对销售转移和获取新客户的影响

在店铺关闭后，客户保留依赖于其他渠道（如在线或实体店）的便利性和质量、客户对公司及其产品的忠诚度以及市场竞争。

**你现在如何预测关闭一家店后会发生什么？** 我过去最成功的方法是使用决策树或回归分析。零售的一个优点是，当你查看历史时，你会发现由于各种原因，已经有了相当数量的店铺关闭。首先，你需要**解决客户保留问题**。从高层次来看，以下是我发现最有效的两种方法。

1.  决策树：该模型使用树状结构根据一系列二元分裂做出选择，每个分裂由特定预测变量的值决定。美妙之处在于，你已经在完成服务区和店铺网络计算时建立了关键特征。现在，你可以通过增加额外的特征来增强它，例如特定服务区的竞争情况。

1.  回归分析：该模型通过一个或多个预测变量来预测连续结果（在我们的例子中是客户保留）。如上所述，你已经创建了基本特征，因此你的工作有了良好的起点。

**关于收购的工作**可能会显得更为晦涩。我想，你可以以门店的服务范围为起点。由于该区域可能与其他门店的服务范围重叠，你需要将这些重叠部分扣除，以获得那些不会从实体店角度得到支持的区域来帮助你的收购工作。当你查看趋势时，你可以了解它对整体连锁的相对重要性，以及你是否应该建立一个数字化支持的战略，以抵消在零售位置运营成本的一小部分下获取新客户的损失。

## 结束语

通过综合上述信息，包括如租赁条款等房地产数据以及关于竞争对手和主力店的市场情报，我们可以通过合理化我们的门店网络来战略性地优化短期和中期的盈利能力。当然，当你将这些信息整理成一个吸引人的前端，并将其打包成一个产品，供你的财务和房地产团队使用时，附加分数会更高。将强大的[**产品感知**](https://medium.com/@martinleitner_33020/what-skill-is-a-significant-differentiator-for-a-data-scientist-d0a4af725a86)作为你方法的一部分，将帮助获得利益相关者的支持，并对你的业务产生实际影响。

一如既往，**期待听到你的想法、经验和评论**。

*想要联系？*

+   📖 在 [Medium](https://medium.com/@martinleitner_33020) 上关注我

+   👀 订阅我的 [内容](https://medium.com/@martinleitner_33020/subscribe)

+   ✍ 在 [Linkedin](https://www.linkedin.com/in/leitnerm/) 上联系我

![](../Images/6443a8fb2e6b1f00e705faa56d6ade82.png)

图片由 [Heidi Fin](https://unsplash.com/@nofunfin?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)
