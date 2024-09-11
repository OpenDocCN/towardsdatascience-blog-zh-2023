# 为什么随机实验是因果推断中的黄金标准？

> 原文：[https://towardsdatascience.com/why-are-randomized-experiments-the-gold-standard-in-causal-inference-f3fa240a1d02?source=collection_archive---------14-----------------------#2023-03-07](https://towardsdatascience.com/why-are-randomized-experiments-the-gold-standard-in-causal-inference-f3fa240a1d02?source=collection_archive---------14-----------------------#2023-03-07)

## 理解实验中的识别假设

[](https://medium.com/@murat.unal?source=post_page-----f3fa240a1d02--------------------------------)[![Murat Unal](../Images/9f00db7597d7ece01213a6b0589c87d8.png)](https://medium.com/@murat.unal?source=post_page-----f3fa240a1d02--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f3fa240a1d02--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f3fa240a1d02--------------------------------) [Murat Unal](https://medium.com/@murat.unal?source=post_page-----f3fa240a1d02--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F15a64c9fc55d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-are-randomized-experiments-the-gold-standard-in-causal-inference-f3fa240a1d02&user=Murat+Unal&userId=15a64c9fc55d&source=post_page-15a64c9fc55d----f3fa240a1d02---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f3fa240a1d02--------------------------------) ·7分钟阅读·2023年3月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff3fa240a1d02&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-are-randomized-experiments-the-gold-standard-in-causal-inference-f3fa240a1d02&user=Murat+Unal&userId=15a64c9fc55d&source=-----f3fa240a1d02---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff3fa240a1d02&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-are-randomized-experiments-the-gold-standard-in-causal-inference-f3fa240a1d02&source=-----f3fa240a1d02---------------------bookmark_footer-----------)![](../Images/b84e56591feb7ece2a967855fe86a5e9.png)

图片由 [takaharu SAWA](https://unsplash.com/@haru88?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

没有假设的因果推断是不可能的。每一种可用的方法都需要不可测试的假设，以从数据中观察到的关联中建立因果关系。因此，陈述识别假设并为其辩护是因果推断中最关键的方面，但这也是最被忽视的方面。

在我之前的文章中，我们通过描述什么是识别以及为什么它在因果推断中优先于估计来开始讨论这个话题。提醒一下，识别本质上包括清晰地陈述从数据中获得的统计估计需要的假设，以便这些估计可以被赋予因果解释，并在我们的因果分析中为这些假设辩护。接下来我们讨论随机实验中的识别，即因果推断中所有识别策略中的黄金标准。

[## 识别：可信因果推断的关键](https://towardsdatascience.com/identification-the-key-to-credible-causal-inference-c3023143349e?source=post_page-----f3fa240a1d02--------------------------------)

### 提升你的因果智商，通过掌握识别建立对因果推断的信任

[towardsdatascience.com](https://towardsdatascience.com/identification-the-key-to-credible-causal-inference-c3023143349e?source=post_page-----f3fa240a1d02--------------------------------)

实验在其他识别策略中为何具有特殊地位？这有两个原因。首先，良好开展的实验只需最少的识别假设来确定因果关系。其次，这些假设比其他方法所需的假设更为合理。这两个事实的结合增加了随机实验中因果推断的可信度。

为了帮助讨论，我们再次考虑一个简单的案例，其中两个处理条件由一个二元随机变量描述，*Ti=[0,1]*，结果由*Yi*表示。我们感兴趣的是找到平均处理效应（ATE），即期望值之间的差异：

其中 *Yi1* 表示如果被试 *i* 接受治疗的潜在结果，而 *Yi0* 表示如果被试 *i* 不接受治疗的潜在结果。如果潜在结果的含义不清楚，我强烈建议你阅读上述链接的文章。你将看到一个具体的例子，这将帮助你理解本文的其余部分。

由于我们仅能访问样本而不是整个总体，我们只能观察到条件期望 *E[Yi|Ti=1]* 和 *E[Yi|Ti=0]*，即我们数据中看到的处理组和对照组的期望结果。

现在，为了声称我们通过条件期望的差异得到的是ATE，即我们的因果估计量，我们需要展示这些差异等同于无条件期望。做到这一点的方法是通过提出假设并使我们的观众相信它们在我们特定背景下的合理性。

## 识别部分 1：陈述假设

在随机实验中，识别假设如下：

**1\. 独立性：**潜在结果与治疗状态独立。

2\. **SUTVA（稳定单位处理价值假设）：**（1）没有互动效应，意味着一个单位收到的处理不会影响另一个单位的结果。（2）没有隐藏的处理变异，只有施加在特定受试者上的处理水平可能会影响该受试者的结果。

从技术上讲，除了两个识别假设之外，我们还假设实验对象之间没有系统性流失和不遵守情况。

有了这些假设，我们可以如下编写观察结果的期望值：

这使我们能够利用数据中处理组和对照组的结果来获得ATE：

另一种观察这些假设如何发挥作用的方式是查看以下分解：

![](../Images/0eaf986824d3d5bdb5274c943cfecef1.png)

独立性假设有效地消除了偏差项，我们剩下的是ATT（对处理组的平均处理效应），这相当于ATE。

## 识别部分 2：捍卫假设

这种识别策略的直接后果是它允许我们

断言处理组和对照组在所有方面（可观察和不可观察）都是相同的，除了处理分配的差异。

此外，如果我们有一个良好实施的实验且受试者之间没有系统性流失，那么说服我们的观众接受识别假设的有效性将容易得多。

为了使事情更具体，我们用一个模型来描述结果、*Yi*、处理*Ti*和协变量*Xi*之间的关系，并且为了简化起见，我们假设它是线性的：

*P*协变量可能非常大，包括对结果的其他线性影响的观察或未观察到的影响，*Bi*是个体处理效应，即两个潜在结果*Yi1 — Yi0*之间的差异。我们没有观察到这一效应，但通过从处理组的平均结果中减去对照组的平均结果，我们得到了这个：

这表明当所有其他效应对结果的均值差异相同时，我们通过均值差异获得我们想要的结果。

这两组是相同的。

这是**完美平衡**的情况，随机化通过构造保证了这一点，因为它为我们因果模型中其他*P*因果因素提供了正交性。换句话说，我们知道在确定处理分配时除了掷硬币之外没有使用其他方法。

仍然存在对**内部有效性**的威胁，这可能会削弱我们的因果推断。首先，SUTVA的违背，即两个组之间的受试者相互作用，在许多社会和经济应用中可能会发生。如果发生这种情况，我们所做的比较将不再是处理组和对照组之间的比较，而是处理组和部分处理组之间的比较。

其次，由于共同原因排除混淆是有期望保证的，这意味着处理组和对照组仅在处理分配上有所不同而在其他方面没有差异的概率，随着样本量的增加而变得任意更高。在给定样本中，特别是如果样本较小，其他原因的净效应可能不为零，这可能会偏倚 ATE。

因此，在分析实验之前，检查处理组和对照组样本中的观测值平衡总是一个好的做法。通常，这通过计算标准化差异作为分布差异的无尺度度量来完成，而不是使用 t 统计量。具体而言，对于每个协变量，我们报告按处理状态划分的平均差异，按方差和的平方根进行缩放：

其中 S1²、S0² 分别是处理组和对照组的样本方差。作为一个经验法则，大于 0.10 的差异可能表明不平衡，这将要求在分析中进行协变量调整。

然而，请注意，我们没有办法测试实验中是否排除了共同的不可观测原因。我们只是简单地假设如果在观测值中达成平衡，那么它在不可观测值中也有可能保持平衡。但这可能并非如此，尤其是在小样本中。

我们还需要注意实验结果的普遍适用性，即其**外部有效性**。重要的是认识到实验使我们能够在研究所用的人群中识别处理效应。此外，除非实验旨在测量长期效应，否则估计的 ATE 通常适用于短期。通常将实验中的估计视为真理，不仅仅在研究样本中，更广泛地应用，但这可能并不有效。

有许多案例中，企业决定根据一个市场、样本或时间段的实验结果来扩展操作，但由于这些结果未被证明是普遍适用的，因此失败了。一般来说，为确保有效的外推，我们需要随机抽样以及处理的随机化，或额外的假设。例如，如果我们有一个在线实验，为确保结果对预定位置的所有用户都是普遍适用的，我们需要通过在一天内和一周内随机抽样访问者来消除星期几和时间的影响。

## 结论

因果推断的困难在于其固有的不确定性，只要我们提高对最关键部分——识别的理解，就可以取得进展。随机实验需要的识别假设比其他任何识别策略都少。而且，这些假设在实验中更加可信。这两个特征共同使实验在因果推断中赢得了金标准的地位。然而，如果实验未能正确进行，它们仍可能缺乏内部效度，并且将实验发现推广到其他情境也存在挑战。

> 感谢阅读！希望你觉得这值得你的时间。
> 
> 我努力为从事因果推断方法和应用以及营销数据科学的实践者编写高质量和有用的文章。
> 
> 如果你对这些领域感兴趣，可以关注我，并随时分享你的评论/建议。

# 参考文献

[1] L. Keele, [因果推断统计：来自政治方法论的视角](https://www.cambridge.org/core/journals/political-analysis/article/abs/statistics-of-causal-inference-a-view-from-political-methodology/314EFF877ECB1B90A1452D10D4E24BB3)（2015），*政治分析。*

[2] A. Lewbel, [计量经济学中的识别意义：识别动物园](https://www.aeaweb.org/articles?id=10.1257%2Fjel.20181361) (2019)，*经济文献期刊。*

[3] G. W. Imbens 和 J. M. Wooldridge, [项目评估计量经济学的最新进展](https://www.aeaweb.org/articles?id=10.1257%2Fjel.47.1.5)，（2009），*经济文献期刊。*

[4] J. List, 《电压效应：如何将好想法变成伟大想法并使其扩展》，（2022）。
