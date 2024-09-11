# 使用倾向评分匹配来构建领先指标

> 原文：[https://towardsdatascience.com/using-propensity-score-matching-to-build-leading-indicators-3e656dccbaf9?source=collection_archive---------1-----------------------#2023-03-04](https://towardsdatascience.com/using-propensity-score-matching-to-build-leading-indicators-3e656dccbaf9?source=collection_archive---------1-----------------------#2023-03-04)

## **关于构建产品激活指标的简短指南**

[](https://medium.com/@jordangom?source=post_page-----3e656dccbaf9--------------------------------)[![Jordan Gomes](../Images/d08bb9fd8b084687599a67a2221ec68c.png)](https://medium.com/@jordangom?source=post_page-----3e656dccbaf9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3e656dccbaf9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3e656dccbaf9--------------------------------) [Jordan Gomes](https://medium.com/@jordangom?source=post_page-----3e656dccbaf9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbd72dcfe2a5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-propensity-score-matching-to-build-leading-indicators-3e656dccbaf9&user=Jordan+Gomes&userId=bd72dcfe2a5a&source=post_page-bd72dcfe2a5a----3e656dccbaf9---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e656dccbaf9--------------------------------) · 6 分钟阅读 · 2023年3月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3e656dccbaf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-propensity-score-matching-to-build-leading-indicators-3e656dccbaf9&user=Jordan+Gomes&userId=bd72dcfe2a5a&source=-----3e656dccbaf9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3e656dccbaf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-propensity-score-matching-to-build-leading-indicators-3e656dccbaf9&source=-----3e656dccbaf9---------------------bookmark_footer-----------)

[在上一篇文章中](https://medium.com/towards-data-science/driving-operational-successes-through-careful-metric-design-ca55e3f84dad)，我讨论了“输入 > 输出 > 结果”框架，以及“输出”是核心部分，但并不一定容易定义——仅仅因为你希望它受到输入的影响，但同时，你需要与结果有因果联系。

用户激活指标属于这种类型的指标。“激活”是由Dave McClure（著名的AAARRR框架——意识、获取、激活、保留、推荐、收入）设计的海盗指标框架的第三个阶段，它通常定义为用户克服了第一组障碍，开始使用你的产品，从中获得了一些价值，并且现在更有可能在长期内被保留。

```py
Some examples of product activation metric:
Loom: Sharing a loom¹ 
Zappier: Setting a zap¹
Zoom: Completing a zoom meeting within 7d of signup¹
Slack: Sending 2,000+ team messages in the first 30 days²
Dropbox: Uploading 1 file in 1 folder on 1 device within 1 hour²
HubSpot: Using 5 features within 60 days²

¹2022 product benchmark from Open View 
https://openviewpartners.com/2022-product-benchmarks/

²Stage 2 Capital: the science of scaling:
https://www.stage2.capital/science-of-scaling
```

测量激活是重要的，因为它帮助你了解你的产品对新用户的吸引力如何，以及你是否有效地让他们成为“活跃”用户。这是通向用户忠诚的第一步——在这个阶段你可以知道用户是否可能长期留在你的产品中。如果激活率较低，可能表示产品或入驻流程存在问题，可能需要做出调整以改善用户体验并提高激活率。

![](../Images/058b83b830bbe7e29f5fe7100985be2c.png)

图片来自[邓肯·迈耶](https://unsplash.com/@dunc_in?utm_source=medium&utm_medium=referral)于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **定义产品激活为何困难**

+   你希望激活能成为保留的良好预测指标，但同时，你希望它足够简单，因为这应该是用户遵循的简单第一步。

+   基本上，你要寻找用户能够采取的最小动作，这个动作可以展示产品的价值，但你希望这个小动作与保留有因果关系（无论你如何定义它）。

+   与任何“领先”指标一样，因果关系部分（“做某个动作导致长期保留”）是困难的。你通常从观察数据开始，传统的数据分析可能无法提供完整的图景，因为它可能忽略了影响激活/保留的混杂因素。

# **使用队列分析和倾向得分匹配来调查因果关系**

## 通过队列分析，你可以开始建立对哪些用户行为可能是激活指标的良好候选的直觉。

其目的是：

+   根据用户注册你的产品的位置对他们进行分组。

+   根据用户是否达到了保留阶段将他们分开。

+   寻找那些在达成保留阶段的用户中做得非常多的动作，而在未达成的用户中做得较少的动作。

假设你运营一个健身应用。你开始创建每月的队列，并且注意到，70%在注册后的第一周内上传至少一次锻炼的用户在一年后仍然活跃于应用中，而如果他们没有上传，则只有40%。这可以成为激活指标的初步想法。

这里的前提条件是你要弄清楚要研究哪个行动。在上述示例中，你必须想到查看谁跟踪了他们的锻炼。这是定量与定性相结合的地方，也是你的‘用户洞察’/常识发挥作用的地方。或者，如果你想寻求其他领域专家的帮助，那就需要你的网络技能。

一些建议：

+   你可能只想提出几个潜在的行动想法，不必过多地研究它们，因为俗话说：“如果你折磨数据足够久，它会承认任何事”（罗纳德·H·科斯）。你选择的行动越多，你发现一些东西的可能性就越大，但你也面临较高的假阳性风险。因此，坚持有意义且不过于牵强的东西可以作为一个好的经验法则。

+   你可能想要采用一种原则性的方法，只寻找你认为能够移动的事物。如果你提出的方案过于复杂或小众，你可能无法实施，这将使整个过程失去意义。

## 倾向得分匹配可以验证或否定你之前的直觉

一旦你确定了潜在的激活信号，下一步是确保它们的准确性。这时倾向得分匹配就能派上用场——以了解你之前找到的相关性是否实际上是因果关系。虽然这不是唯一的解决方案，并且确实需要对你的用户有一些了解（这可能并非总是可能的），但它相对容易实施，并且能让你对结果更有信心（直到进一步通过更稳健的方法如A/B测试进行三角验证）。

倾向得分匹配的理念如下：

+   为了找到采取行动与留存之间的因果联系，理想情况下，你可以复制采取行动的用户，并让克隆体不采取行动——以比较结果。

+   由于目前还无法做到（或许？），下一步最佳的方法是查看你的数据，找到与你采取行动的用户非常相似（几乎相同）的用户——**但他们没有采取行动。**

倾向得分匹配是一种方法学，可以帮助你找到这些非常相似的用户并进行配对。具体来说，它包括：

+   训练一个模型来预测你的用户采取你定义的行动的可能性（他们的倾向）。

+   基于之前发现的可能性来匹配用户（匹配部分）

（注意：你有多种方法可以完成这两个步骤，关于如何选择模型、如何选择合适的变量、选择什么匹配算法等方面，网上有一些很好的指南——有关更多信息，请参见 “[倾向得分匹配的实施实践指南](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1467-6419.2007.00527.x)”）

再次以我们的健身应用程序为例：

+   你已经发现，在注册后的第一周上传至少一个锻炼计划的用户，仍然在一年后继续使用应用的比例为70%，而如果他们没有上传，则为40%。

+   你训练一个模型来预测用户在注册后一周内上传锻炼计划的可能性——你发现，通过大型健身网站的推荐链接下载应用的用户，其可能性非常高。

+   你根据可能性对用户进行排序，然后开始进行简单的1:1匹配（按可能性排序的第一个采取行动的用户与按可能性排序的第一个未采取行动的用户匹配，依此类推）。

+   匹配后，你会发现差异大大缩小，但这仍然对你考虑它作为潜在的激活指标很重要！

# 定义产品激活的万灵药？

阶段分析 + 倾向评分匹配可以帮助你隔离特定行动对用户行为的影响，这对于定义准确的激活指标至关重要。

但这种方法并不是万灵药——这套方法论有一堆假设，你需要对其进行微调/验证，以确保它适用于你的使用场景。

特别是，PSM的有效性将高度依赖于你能预测自我选择的准确程度。如果你缺少关键特征，而未观察到的特征带来的偏差很大——那么PSM的估计可能会非常偏颇，并且不够有用。

综上所述——即使以不完美的方式使用这种方法，也能帮助你采用更加数据驱动的方法来选择指标，让你开始‘关注的内容’，直到你进入A/B测试阶段，并更好地理解推动长期成功的因素。

## 希望你喜欢阅读这篇文章！你有任何想分享的技巧吗？请在评论区告诉大家！

**如果你想了解更多关于我的内容，以下是你可能喜欢的几篇文章**：

[](/7-tips-to-avoid-public-embarrassment-as-a-data-analyst-caec8f701e42?source=post_page-----3e656dccbaf9--------------------------------) [## 7个使你的数据分析更稳健的技巧]

### 增加你结果的信心，建立更强的个人品牌

towardsdatascience.com](/7-tips-to-avoid-public-embarrassment-as-a-data-analyst-caec8f701e42?source=post_page-----3e656dccbaf9--------------------------------) [](/how-to-build-a-successful-dashboard-359c8cb0f610?source=post_page-----3e656dccbaf9--------------------------------) [## 如何构建成功的仪表盘]

### 来自某个曾经构建过几个不成功的案例的清单

towardsdatascience.com](/how-to-build-a-successful-dashboard-359c8cb0f610?source=post_page-----3e656dccbaf9--------------------------------) [](https://medium.com/@jolecoco/how-to-choose-which-data-projects-to-work-on-c6b8310ac04e?source=post_page-----3e656dccbaf9--------------------------------) [## 如何选择数据项目进行工作]

### 如果你对如何使用时间有一个合理的方法，你可以优化你所创造的价值。

[medium.com](https://medium.com/@jolecoco/how-to-choose-which-data-projects-to-work-on-c6b8310ac04e?source=post_page-----3e656dccbaf9--------------------------------)
