# 案例研究：使用彩虹方法进行实际标签编码

> 原文：[https://towardsdatascience.com/case-study-practical-label-encoding-with-rainbow-method-d167c386e78?source=collection_archive---------13-----------------------#2023-02-24](https://towardsdatascience.com/case-study-practical-label-encoding-with-rainbow-method-d167c386e78?source=collection_archive---------13-----------------------#2023-02-24)

## 在MassMutual生产模型上的真实世界测试

[](https://medium.com/@anna_arakelyan?source=post_page-----d167c386e78--------------------------------)[![安娜·阿拉凯良](../Images/fd742f57a1443a275f44786dfe78c020.png)](https://medium.com/@anna_arakelyan?source=post_page-----d167c386e78--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d167c386e78--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d167c386e78--------------------------------) [安娜·阿拉凯良](https://medium.com/@anna_arakelyan?source=post_page-----d167c386e78--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5058c6266b23&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcase-study-practical-label-encoding-with-rainbow-method-d167c386e78&user=Anna+Arakelyan&userId=5058c6266b23&source=post_page-5058c6266b23----d167c386e78---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d167c386e78--------------------------------) ·7分钟阅读·2023年2月24日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd167c386e78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcase-study-practical-label-encoding-with-rainbow-method-d167c386e78&user=Anna+Arakelyan&userId=5058c6266b23&source=-----d167c386e78---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd167c386e78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcase-study-practical-label-encoding-with-rainbow-method-d167c386e78&source=-----d167c386e78---------------------bookmark_footer-----------)

*与* [*德米特罗·卡拉巴什*](https://medium.com/u/79cc5dc1f7e1) *共同编著*

![](../Images/c35a74496e5749414655443e3f3bb5a1.png)

照片由 [杰森·波加尼克](https://unsplash.com/photos/BY8FZFwLMK0) 提供，来源于 [Unsplash](https://unsplash.com/)

在我们之前的文章“[隐藏的数据科学瑰宝：用于标签编码的彩虹方法](/hidden-data-science-gem-rainbow-method-for-label-encoding-dfd69f4711e1)”中，我们讨论了在开发基于树的模型时，使用标签编码而非独热编码的优势。我们介绍了彩虹方法，这有助于确定不同类型的分类变量的最合适的有序编码。

在本文中，我们将继续探讨Rainbow方法——这一次，从实际的角度，展示其在[MassMutual](https://www.massmutual.com/)的[数据科学团队](https://careers.massmutual.com/explore-careers-data-science/)开发的真实项目中的有效性，MassMutual是一家知名的寿险公司，致力于推动数据科学家、工程师和技术专家来帮助做出明智的商业决策。

# 商业用例

目标是预测每个潜在客户的五个思维模式细分中的一个。实质上，这是一个多类别分类问题。

细分框架包括五个类别，反映了一个人的年龄、财务稳定性以及对金融决策的态度。MassMutual营销团队随后在各种活动中使用预测的细分进行目标定位和定制。

![](../Images/26e20118fd5b0d74406539bf37f925c2.png)

图1（由Anna Arakelyan绘制）

例如，展现出思维模式A的客户倾向于在决定是否购买人寿保险时优先考虑独立性和自主性，而思维模式B的客户则通常更愿意从专门的顾问那里获得指导和详细的金融产品解释。

数据代表了一小部分标记个体（17.5K人），标签由设计了细分分配规则的MassMutual供应商提供。我们首先将主潜在客户数据库中的列添加到这些数据中。目标是使用这些目标标签和可用特征学习最佳模型，并预测所有其他（未标记）潜在客户的细分。

我们使用的消费者数据库涵盖了大约300列，代表了多种人口统计特征，如家庭组成、收入和净资产倾向、金融行为以及数字敏锐度。

在本文中，我们通过消费数据库和思维模式细分项目，将传统的一热编码与Rainbow编码进行比较。我们展示了一些标准指标——如宏观平均F1得分、宏观平均AUC ROC、Cohen’s Kappa和准确率——用于解释和比较这个5类分类问题。

# 分类变量

我们选择了消费数据库中的所有分类变量——包括区间变量、序数变量和名义变量——但排除了定量变量和二元变量。目的是展示相同分类因素下，一热编码和Rainbow编码在模型性能上的差异。

我们进行了目标分层的4折交叉验证拆分，并且从这一点开始的所有数据处理都在交叉验证循环内完成。这包括从每个折的训练集创建一热特征和Rainbow特征，然后将它们应用于每个折的验证集。

总共111个变量被转换为121个Rainbow特征，另外转换为2260个一热特征。

![](../Images/dfcc0d1da586df8d7d27b38a0469fcfb.png)

表 1\. 编码前后的变量列表

对区间和序数变量的 Rainbow 转换非常简单，结果是从 64 个区间特征得到了 64 个 Rainbows，从 14 个序数特征得到了 14 个 Rainbows。

名义变量的转换更为复杂，我们为剩下的 10 个变量创建了 23 个自然属性 Rainbows 和 20 个人工 Rainbow 特征。由于我们处理了五个类别，我们对随机类别应用了相关排序和目标百分比排序（见原文的[自动化 Rainbow 选择](/hidden-data-science-gem-rainbow-method-for-label-encoding-dfd69f4711e1#ec50)部分）。例如，名义变量“Financial_Cluster”被转换为特征“*Financial_Cluster_Mindset_B_correlation_rank*”和“*Financial_Cluster_Mindset_D_target_percent*”。总体而言，33 个名义变量被转换为 43 个 Rainbows。

对于实际排序的选择——无论是自然属性 Rainbow 还是人工 Rainbow——高度依赖于项目和上下文，更多的是艺术而非科学。这需要在模型简洁性、性能和可解释性之间取得平衡。

与序数编码不同，One-hot 转换生成了超过两千个特征。

> 为什么我们在这里为区间和序数变量制作 One-hot 特征？因为我们希望在从完美顺序到模糊顺序，再到无顺序（或错误顺序）的完整连续体上，将 Rainbow 与 One-hot 进行比较。
> 
> 此外，将变量分类为序数或名义有时是一种主观决定。一个明显的例子是颜色。正如我们在[第一篇文章](/hidden-data-science-gem-rainbow-method-for-label-encoding-dfd69f4711e1)中讨论的，颜色被一些模型者认为是名义的，而另一些则认为是序数的。

起初，我们将所有类别变量进行汇总，但在文章后面，我们分别分析了区间、序数和名义变量。

我们训练了所有的*XGBoost*模型，涵盖了下面显示的超参数空间：

```py
params = {
    'objective': 'multi:softprob',
    'eval_metric': 'mlogloss',
    'num_class': 5, 
    'subsample': 0.8, 
    'max_depth': [2, 3, 5], 
    'eta': [0.1, 0.3, 0.5],
    'n_estimators': [50, 100, 200],
}
```

我们避免将 max_depth 设置得高于 5，因为数据量相对较小，并且每个分支的末端需要至少 100 个样本。我们倾向于使用简单模型，这也有助于防止过拟合。

下面的所有结果表示交叉验证的平均指标。

# 综合结果

让我们从所有运行的总体平均值开始。显然，对于 Rainbow 编码，所有模型的平均指标更高。总体差异为几个百分点。

![](../Images/18e6bbfc8456185d80786dfac29570e1.png)

图 2（作者生成）

# 超参数

下面的图示展示了在保持所有其他超参数不变的情况下，每个超参数的指标变化。这些图示也清楚地表明，Rainbow 的结果在每个超参数和指标上都超过了 One-hot 的结果。

![](../Images/d4f3c9d9544b78ce70ab0e0759d2f202.png)

图 3a（由作者生成）

![](../Images/991691aaa512e61abb581dfff7703f30.png)

图 3b（由作者生成）

![](../Images/488ffd3527caa93c2eb62c8f0fb9ee27.png)

图 3c（由作者生成）

# 运行时间

接下来，让我们比较每种方法的运行时间。

```py
One-hot: 65.059 s
Rainbow:  5.491 s
```

运行一个“Rainbow”模型的平均时间几乎是运行一个“One-hot”模型的 12 倍！因此，除了显著提高模型性能指标外，我们还可以看到，“Rainbow”方法可以为数据科学家节省大量时间。

# 间隔型、序数型和名义型

接下来，我们分别运行了包含间隔型、序数型和名义型特征的模型。结果列在下面。

![](../Images/714d73a19b1db682614be3c22057948b.png)

图 4（由作者生成）

这些结果再次强调了“Rainbow”相比于“One-hot”的优势。正如预期的那样，“Rainbow”编码对间隔型和序数型特征的提升最大，而对名义型变量的提升则较小。

显然，类别顺序越明确，选择“Rainbow”而非“One-hot”的好处就越大。虽然“Rainbow”对名义型变量的表现与“One-hot”相似或略低，但它仍然能以显著较少的时间和空间达到相同的性能水平，生成的模型也显著更简单。

# 特征选择

最后，为了确保在维度方面的公平比较，我们从每个特征集（Rainbow 和 One-hot）中选择了前 10、50 和 100 个特征。我们利用了*XGBoost*模型的特征重要性属性，并聚合了四次交叉验证折中的特征重要性分数，以获得每种编码类型的最佳超参数集。结果如下所示。

![](../Images/8e3402d9ea4aae243269ed5a8ce06405.png)

图 5（由作者生成）

“Rainbow”编码轻松超越了“One-hot”编码，特别是在特征数量较少的情况下。“Rainbow”编码比“One-hot”编码更快地达到性能峰值，并且使用的特征更少。实际上，只有 10 个特征时，“Rainbow”编码已经接近其峰值，而“One-hot”编码则需要 50–100 个特征才能达到类似水平！

此外，“Rainbow”编码在 50 个特征上的结果甚至优于“One-hot”编码在 100 个特征上的结果。值得注意的是，当特征数量从 50 降至 10 时，“One-hot”编码的 Macro-F1 降低幅度是“Rainbow”方法的六倍（Kappa 和 Accuracy 降低幅度为三倍，Macro-AUC 降低幅度为两倍）。

# 结论

MassMutual 的心态分段模型的例子清楚地说明了 Rainbow 标签编码优于 One-hot 编码。**不仅**为建模人员节省了大量时间，还显著降低了维度，并提供了一个有机的特征选择框架。*此外*，如果所选的 Rainbow 顺序与数据生成过程一致，那么这种编码还可以显著提升模型性能指标。
