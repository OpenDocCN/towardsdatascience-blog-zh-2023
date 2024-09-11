# 使用营销组合建模来超级提升你的跨渠道客户获取

> 原文：[https://towardsdatascience.com/supercharge-your-cross-channel-customer-acquisition-with-marketing-mix-modeling-21ba1bc5f2e8?source=collection_archive---------8-----------------------#2023-02-22](https://towardsdatascience.com/supercharge-your-cross-channel-customer-acquisition-with-marketing-mix-modeling-21ba1bc5f2e8?source=collection_archive---------8-----------------------#2023-02-22)

## 离线营销和销售渠道正在回归，你需要适应

[](https://ivylc.medium.com/?source=post_page-----21ba1bc5f2e8--------------------------------)[![艾薇·刘](../Images/74483fd84a1b4e4a0e013474496d9925.png)](https://ivylc.medium.com/?source=post_page-----21ba1bc5f2e8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----21ba1bc5f2e8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----21ba1bc5f2e8--------------------------------) [艾薇·刘](https://ivylc.medium.com/?source=post_page-----21ba1bc5f2e8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F71fa5614d897&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharge-your-cross-channel-customer-acquisition-with-marketing-mix-modeling-21ba1bc5f2e8&user=Ivy+Liu&userId=71fa5614d897&source=post_page-71fa5614d897----21ba1bc5f2e8---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----21ba1bc5f2e8--------------------------------) ·5 min 阅读·2023年2月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F21ba1bc5f2e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharge-your-cross-channel-customer-acquisition-with-marketing-mix-modeling-21ba1bc5f2e8&user=Ivy+Liu&userId=71fa5614d897&source=-----21ba1bc5f2e8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F21ba1bc5f2e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupercharge-your-cross-channel-customer-acquisition-with-marketing-mix-modeling-21ba1bc5f2e8&source=-----21ba1bc5f2e8---------------------bookmark_footer-----------)

随着数字媒体上的客户获取成本持续上涨，越来越多的消费品牌开始寻求多样化的营销支出。品牌正在重新评估传统渠道，如电视、广播、店内营销等。然而，这些渠道的数据追踪有限。现在，品牌面临着在所有渠道中可靠衡量营销绩效的新挑战。

此外，消费者在经历了三年的COVID后正回归到线下购物。对于同时在线上和线下销售的品牌来说，全面理解营销和销售表现是困难的。

此外，通过视频或网红进行促销的品牌难以揭示这些营销努力的回报（或没有回报）。随着IDFA和Cookies的消退，使用归因方法测量视图广告表现成为不可能的任务。

![](../Images/0d32de6197780ea4b0a1349d147c76a2.png)

图片来源于作者

这些问题每年让品牌损失数百万美元。在当前经济不确定的情况下，防止这种浪费比以往任何时候都更为关键。因此，品牌高管们正在要求他们的营销和数据团队探索潜在的解决方案，并确定最有效的方案。在本文中，我将讨论如何通过营销组合建模（MMM）来解决这些难题，并在当今充满挑战的经济环境中获得优势。

# 使用MMM测量营销信号

经典的衡量营销活动是否带来回报的方法是通过回归分析。我们可以通过评估每个自变量与回报之间的相关性来发现变量是否带来了回报。

![](../Images/44bc6bf5e4f6f704477c5e6dd56b7a64.png)

图片来源于作者

MMM基于复杂的回归分析，处理包括广告支出、宏观经济和其他外部因素以及收入在内的许多输入，然后将转化信用分配给每个输入。典型的MMM输入包括以下内容。

![](../Images/ecc6c4dbc493d2bd0ac4299be937ed77.png)

图片来源于作者

MMM展示了因素贡献如下。

![](../Images/ae291fff89dd8688c11c7b9918f06119.png)

图片来源于作者

# MMM实践中的应用

让我们来看一个现实例子。一家美妆品牌在其店面、Amazon、Sephora和便利店销售产品，并在Google、Meta、Amazon、电视、播客和店内花费了大量的营销预算。每个月，它都想了解其广告支出如何影响各渠道的销售。美妆品牌还希望了解下个月最佳的营销预算分配。

根据经验，许多美妆品牌的客户在线查看广告后会线下购买，反之亦然。因此，将在线和离线渠道的表现分开是不切实际的。

![](../Images/5ddb2e6cc34637c06d0349601545e9b9.png)

图片来源于作者

MMM可以提供帮助。通过将每日广告支出、美妆市场趋势和每日销售数据输入模型，品牌可以看到每个变量对销售的贡献随时间的变化。

![](../Images/ee72ff270f77300abc71e034117d4888.png)

图片来源于作者

更好的是，回归模型易于可视化和解释。美妆品牌的业务利益相关者可以评估模型对其业务的拟合程度，并决定是否接受模型结果。

另一方面，通过将不同的广告支出情景输入到训练模型中，美妆品牌将获得这些情景下的相关收入预测。

![](../Images/25da4041bb02d84b71f42f9c0523e4c8.png)

图片由作者提供。

# 连接营销和销售渠道之间的点。

MMM处理如每日广告支出和销售数据等汇总数据，而不是像点击流这样的详细用户级数据。广告支出和销售数据通常可以从主要的营销和销售平台获取，并且每个数据属性通常遵循类似的测量方式。因此，品牌可以期望在广告平台之间进行可比的营销测量。如果没有这种可比性，营销测量将是不可靠的。

同样，MMM为视图广告提供了可靠的测量。如果品牌尝试用归因模型测量视图广告的表现，测量结果可能会被低估。由于归因模型依赖于点击流数据，如果用户看到广告但没有点击，模型将无法知道用户是否查看了广告。在这种情况下，MMM成为归因模型的绝佳补充。

# MMM有其局限性。

然而，MMM并不完美，有许多局限性。

由于MMM需要汇总数据，因此需要动态且长期的营销数据以检测足够的市场信号。因此，只有在营销上大量投资的品牌才能利用MMM。此外，如果品牌希望测量特定的营销渠道，该品牌必须在该渠道上进行长期的积极营销活动。否则，MMM因数据不足而无法生成有意义的结果。

此外，MMM通常只能在广告平台层面进行测量，无法在活动层面进行测量。这是因为大多数品牌在特定活动上不会生成足够的数据点。

最后，品牌需要定期对活动进行实验，以创建动态的营销运动，使MMM能够发挥作用。并非所有团队都有足够的带宽来有效地操作活动，从而充分利用MMM。

# 营销测量应服务于你的收入目标。

总结来说，以下是MMM可以独特地为品牌增值的使用场景：

1.  在多个营销渠道之间进行多样化投资。

1.  同时进行在线和线下销售。

1.  在线营销旨在提升线下销售的认知度，反之亦然。

1.  对于视频和某些付费社交广告等视图广告进行大量投入。

然而，在某些情况下，MMM可能对品牌的帮助有限：

1.  如果特定渠道的营销或销售历史较短，MMM将无法对这些渠道进行准确测量。

1.  测量目标设定在活动层面。

1.  品牌在进行营销实验的带宽有限，因此没有足够的营销信号供MMM使用。

重要的是要记住，没有任何一种营销测量方法是完美的。营销人员和数据团队应该寻找一种最佳契合其使用案例的技术组合，以帮助他们通过营销获得更多利润。在当前的经济环境中，运营时减少开支至关重要，而营销测量方法可以帮助我们实现这一目标。

我在我的文章中讨论了如何利用数据科学提升您的业务和优化您的营销。如果您想讨论营销测量或其他数据科学话题，请在[LinkedIn](https://www.linkedin.com/in/ivylc/)上关注我，或通过[newsletter@ivyliu.io](mailto:newsletter@ivyliu.io)与我联系。下次见。
