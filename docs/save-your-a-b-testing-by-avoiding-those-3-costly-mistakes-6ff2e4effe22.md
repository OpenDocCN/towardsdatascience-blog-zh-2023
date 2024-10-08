# 通过避免这 3 个代价高昂的错误来拯救你的 A/B 测试

> 原文：[`towardsdatascience.com/save-your-a-b-testing-by-avoiding-those-3-costly-mistakes-6ff2e4effe22`](https://towardsdatascience.com/save-your-a-b-testing-by-avoiding-those-3-costly-mistakes-6ff2e4effe22)

## 细节将决定成败

[](https://medium.com/@quentin.gallea?source=post_page-----6ff2e4effe22--------------------------------)![Quentin Gallea, PhD](https://medium.com/@quentin.gallea?source=post_page-----6ff2e4effe22--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ff2e4effe22--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ff2e4effe22--------------------------------) [Quentin Gallea, PhD](https://medium.com/@quentin.gallea?source=post_page-----6ff2e4effe22--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ff2e4effe22--------------------------------) ·阅读时间 5 分钟·2023 年 7 月 7 日

--

![](img/328f6861b0e50d534a46abdf88915ce6.png)

图片由 [Christian Stahl](https://unsplash.com/@woodpecker65?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/fr/photos/8S96OpxSlvg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随机对照试验曾专门用于学术界，特别是医学研究，但现在已经成为企业进行数据驱动决策的一种流行方法。特别是，在线 A/B 测试易于实施，并且在优化数字过程方面潜力巨大。通过比较两个或多个变体，组织可以评估不同选项的有效性，并确定最有利的结果。然而，认识并解决某些限制是至关重要的，以确保偏差不会影响结果的可靠性和有效性。在本文中，我们探讨了在进行在线 A/B 测试之前需要考虑的三个关键限制，以避免代价高昂的偏差。在列出我个人认为的前三个问题之前，让我简要定义一下 A/B 测试和一些重要概念。

## 什么是 A/B 测试？

A/B 测试涉及将不同的版本/变体 A 和 B 展示给不同的研究对象（例如客户）。在线 A/B 测试可以探索网页、电子邮件活动、用户界面或任何其他数字资产的变体，并将其展示给用户的一个子集。这些变体通常在一个或多个特定元素上有所不同，如设计、布局、颜色方案、行动号召或内容。通过精心控制的实验，组织可以测量这些变体对用户行为、参与度和转化率的影响。

## **随机化**

该过程首先是将受众随机分成两个或更多组，每组接触不同的变体。对照组接收原始版本（称为基线或对照，只要存在原始版本），而其他组接收修改版。通过跟踪用户互动，如点击次数、转换率、在页面上花费的时间或任何其他预定义指标，组织可以比较不同变体的表现，并确定哪个变体能产生期望的结果。

![](img/11b28d4da2a9a24d9c9099a756c8cf57.png)

作者提供的图像。这里是 A/B 测试过程的表示。首先，我们随机将样本分为对照组和处理组（A 和 B）。其次，我们观察结果（例如转换率），在此用黑色/绿色表示。

## 因果关系

A/B 测试的主要目标是正确识别变化的效果。如果不仔细遵循这种策略，其他因素可能会影响受试者的行为。想象一下 Netflix 决定将其主页改为显示当前观看最多的内容，而不是最新发布的内容（这是一个假设的例子）。然后，假设公司没有使用 A/B 测试，而是将平台在四月时对所有人进行更改，然后比较三月和四月之间在平台上花费的时间和订阅人数。这些差异可能是由于主页更改造成的，但也可能是天气差异、其他在线流媒体平台等因素造成的。由于同时存在多个混杂因素，识别原因将变得不可能。A/B 测试旨在通过随机分配并同时测试两个或多个组来解决这一问题。要深入了解因果关系，我邀请你阅读我关于因果关系的两部分文章（[`medium.com/towards-data-science/the-science-and-art-of-causality-part-1-5d6fb55b7a7c`](https://medium.com/towards-data-science/the-science-and-art-of-causality-part-1-5d6fb55b7a7c)）。

现在，让我们深入探讨组织在进行在线 A/B 测试之前应该考虑的三个关键限制，以避免代价高昂的偏差。通过了解和减轻这些限制，企业可以最大化 A/B 测试的价值，做出更明智的决策，并推动其数字体验的有意义改进。

## 1\. 通道：揭示用户的视角

在线 A/B 测试的主要限制之一是理解用户对某一选项偏好而非另一选项的原因。通常，选项 A 和 B 之间的选择没有明确的理由，使实验者不得不对用户行为进行推测。在科学研究中，我们称之为“通道”，即解释因果效应的理由。

假设你的选项 B 在结账页面上加入了额外功能（例如，类似产品或一起购买的产品推荐）。你观察到选项 B 的购买量下降，因此得出它是一个不好的想法。然而，更仔细的分析显示实际上选项 B 的页面加载时间更长。现在你基本上有两个差异：内容和等待时间。因此，回到因果关系的概念，你不知道是什么驱动了选择；这两者相互混淆。如果你认为加载时间无关紧要，那就再想想吧：*“ […] 亚马逊的实验显示额外的 100 毫秒加载时间导致销售减少 1%，谷歌的一项特定实验将搜索结果显示时间增加 500 毫秒，收入减少 20%”* [*(*Kohavi et al. (2007))](https://ai.stanford.edu/~ronnyk/2007GuideControlledExperiments.pdf)

**解决方案：** 首先，为了减轻这一限制，加入额外的调查问题可以提供有关用户动机的宝贵见解，从而减少偏见解释的风险。其次，尽量避免有多个差异有助于确定原因（例如，保持相同的加载时间）。

## 2\. 短期与长期影响：超越即时结果

在进行在线 A/B 测试时，考虑所选择指标的潜在长期影响至关重要。虽然短期目标（如点击率或即时转化）最初可能看起来有利，但它们可能在长期内产生不利后果。例如，使用诱饵策略可能会带来快速的观看和印象，但随着时间的推移，它们可能会对受众的感知和你的信誉产生负面影响。

**解决方案：** 关键在于测量评估短期和长期影响的多个指标。通过评估全面的指标范围，组织可以做出更明智的决策，避免短视的优化策略。长期影响指标可能包括满意度评估和受众留存（例如，视频观看时间或文章阅读时间）。也就是说，这些指标的评估并非易事。

## 3\. 首因效应与新颖性效应：新颖性的影响

在线 A/B 测试中，来自新颖性影响的两个相关限制是首因效应和新颖性效应。首因效应指的是经验丰富的用户在遇到变化时可能会感到困惑或迷失，例如按钮的位置或颜色变化。相反，新颖性效应发生在用户因新功能的独特性而被诱使去互动，但这种效应可能会迅速消退。这些效应在用户有定期互动的平台上尤为突出，例如社交媒体。

**解决方案：** 建议在几周内进行实验，观察效果如何随时间变化。通过监测波动的用户行为，实验者可以更全面地了解其更改的长期影响。

## 结论：

虽然在线 A/B 测试提供了一个有价值的数据驱动决策工具，但考虑至少这三个潜在问题至关重要。通过考虑用户参与的渠道、测量短期和长期影响以及考虑首因效应和新颖效应，组织可以提高 A/B 测试结果的可靠性和有效性。这仅仅是冰山一角，我邀请你进一步阅读：[Kohavi, R., Henne, R. M., & Sommerfield, D. (2007 年 8 月)。网络上受控实验的实用指南：听取客户的声音而非大象的意见。在*第 13 届 ACM SIGKDD 国际知识发现与数据挖掘会议论文集*（第 959–967 页）。](https://ai.stanford.edu/~ronnyk/2007GuideControlledExperiments.pdf)
