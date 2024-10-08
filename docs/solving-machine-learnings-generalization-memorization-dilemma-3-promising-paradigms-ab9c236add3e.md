# 像我们一样学习的机器：解决泛化-记忆困境

> 原文：[`towardsdatascience.com/solving-machine-learnings-generalization-memorization-dilemma-3-promising-paradigms-ab9c236add3e`](https://towardsdatascience.com/solving-machine-learnings-generalization-memorization-dilemma-3-promising-paradigms-ab9c236add3e)

## 解决机器学习中最重要问题的 3 种范式

[](https://medium.com/@samuel.flender?source=post_page-----ab9c236add3e--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----ab9c236add3e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ab9c236add3e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab9c236add3e--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----ab9c236add3e--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ab9c236add3e--------------------------------) ·7 分钟阅读·2023 年 4 月 5 日

--

![](img/d07a01c4830a1e5c49f4bbf135d147d5.png)

(Midjourney)

机器学习的“圣杯”是能够构建既能记忆训练数据中的已知模式，又能泛化到未知模式的系统。

之所以称之为圣杯，是因为这也是我们人类学习的方式。你可以在一张旧照片中识别出你的奶奶，但即使你从未见过 Xoloitzcuintli，你也能将其识别为狗。如果没有记忆，我们就得不断从头再学一遍一切；如果没有泛化，我们将无法适应不断变化的世界。为了生存，我们需要两者兼备。

传统的统计学习理论告诉我们这是不可能的：模型要么能够很好地泛化，要么能够很好地记忆，但不能兼顾两者。这就是著名的偏差-方差权衡，这是我们在标准机器学习课程中首先学到的东西。

那么我们如何构建这样的通用学习系统呢？圣杯是否触手可及？

在这篇文章中，让我们深入探讨文献中的 3 种范式，

1.  先泛化，后记忆

1.  同时泛化和记忆

1.  机器泛化，人类记忆

让我们开始吧。

# 1 — 先泛化，后记忆

BERT 通过引入预训练/微调范式彻底改变了机器学习：在大量文本数据上进行无监督的预训练后，该模型可以在特定的下游任务上快速微调，所需的标签相对较少。

惊人的是，这种预训练/微调方法解决了泛化/记忆化问题。BERT 能够进行泛化和记忆，正如来自帝国理工学院的 Michael Tänzer 和他的合作者在 2022 年的 [论文](https://aclanthology.org/2022.acl-long.521/) 中展示的那样。

特别是，作者展示了在微调过程中，BERT 学习的 3 个不同的阶段：

1.  拟合（训练轮次 1）：模型学习简单的通用模式，尽可能解释训练数据。在此阶段，训练和验证性能都在提高。

1.  设置（训练轮次 2–5）：没有更多简单的模式可以学习。训练和验证性能饱和，在学习曲线上形成一个平台。

1.  记忆化（训练轮次 6+）：模型开始记忆训练集中的特定示例，包括噪声，这提高了训练性能但降低了验证性能。

他们是如何发现这一点的？通过从无噪声的训练集（CoNLL03，一个命名实体识别基准数据集）开始，然后逐渐引入更多的人工标签噪声。比较不同噪声量的学习曲线清楚地揭示了 3 个不同的阶段：更多噪声会导致第 3 阶段的下降更加陡峭。

![](img/a4f7edec8029dad338c529bd40303703.png)

微调 BERT 显示了 3 个不同的学习阶段。图来自 Tänzer 等人，“预训练语言模型中的记忆化与泛化” ([link](https://aclanthology.org/2022.acl-long.521/))

Tänzer 等人还表明，BERT 的记忆化需要重复：BERT 只有在看到特定训练示例一定次数后才会记住该示例。这可以从人为引入噪声的学习曲线中推断出来：它是一个阶跃函数，每个训练轮次都会改善。换句话说，在第 3 阶段，如果我们让 BERT 训练足够多的轮次，它最终可以记住整个训练集。

总之，BERT 似乎首先进行泛化，然后进行记忆，这从其在微调过程中观察到的 3 个不同的学习阶段可以得到证实。事实上，还可以证明这种行为是预训练的直接结果：Tänzer 等人展示了一个随机初始化的 BERT 模型不会共享相同的 3 个学习阶段。这导致了预训练/微调范式可能是解决泛化/记忆化困境的一个可能方案的结论。

# 2 — 同时进行泛化和记忆

让我们离开自然语言处理的世界，进入 [推荐系统](https://medium.com/towards-data-science/biases-in-recommender-systems-top-challenges-and-recent-breakthroughs-edcda59d30bf) 的世界。

在现代推荐系统中，同时具备记忆和泛化的能力至关重要。例如，YouTube 想向你展示与你过去观看的视频类似的内容（记忆），但也展示一些你甚至不知道自己会喜欢的新视频（泛化）。没有记忆你会感到沮丧，没有泛化你会感到无聊。

目前最好的推荐系统需要同时做到这两点。但是怎么做呢？

在 2016 年的[论文](https://arxiv.org/abs/1606.07792)中，Heng-Tze Cheng 和来自 Google 的合作者提出了他们所称的“宽广而深度学习”来解决这个问题。关键思想是构建一个包含深度组件（具有嵌入输入的深度神经网络）用于泛化以及宽广组件（具有大量稀疏输入的线性模型）用于记忆的单一神经网络。作者在 Google Play 商店的推荐中演示了这种方法的有效性，该商店向用户推荐应用程序。

深度组件的输入是密集特征以及分类特征的嵌入，例如用户语言、用户性别、印象中的应用程序、已安装的应用程序等。这些嵌入被随机初始化，然后在模型训练过程中与神经网络中的其他参数一起调整。

网络宽广组件的输入是细粒度的交叉特征，例如

```py
AND(user_installed_app=netflix, impression_app=hulu”),
```

如果用户安装了 Netflix 且印象中的应用程序是 Hulu，则其值为 1。

很容易看出为什么宽广组件能实现一种记忆：如果 99% 的安装了 Netflix 的用户也最终安装了 Hulu，宽广组件将能够学习这一信息，而它可能会在深度组件中丢失。 Cheng 等人认为，同时拥有宽广和深度组件确实是达到最佳性能的关键。

实验结果实际上证实了作者的假设。宽广&深度模型在 Google Play 商店的在线获取增益方面优于仅宽广模型（提高了 2.9%）和仅深度模型（提高了 1%）。这些实验结果表明，“宽广&深度”是解决泛化/记忆困境的另一种有前途的范式。

# 3 — 让机器泛化，让人类记忆

Tänzer 和 Cheng 提出了仅用机器解决泛化/记忆困境的方法。然而，机器很难记住单个示例：Tänzer 等人发现 BERT 需要至少 25 个实例来学习预测一个类别，且需要 100 个示例才能“有一定准确性”地预测。

退一步说，我们不必让机器完成所有的工作。与其对抗机器的记忆失败，不如接受它？为什么不构建一个结合了机器学习和人类专业知识的混合系统呢？

这正是 Chimera 背后的理念，Chimera 是沃尔玛用于大规模电子商务商品分类的生产系统，由 Chong Sun 和沃尔玛实验室的合作者在 2014 年的[论文](https://pages.cs.wisc.edu/~anhai/papers/chimera-vldb14.pdf)中提出。Chimera 的前提是，仅凭机器学习不足以处理大规模的商品分类，因为存在大量训练数据稀缺的边缘案例。

例如，沃尔玛可能同意试验性地从新供应商那里采购有限数量的新产品。由于缺乏足够的训练数据，机器学习系统可能无法准确分类这些产品。然而，人类分析师可以编写规则来准确覆盖这些情况。这些基于规则的决策随后可以用于模型训练，使模型在经过一段时间后能够适应新模式。

作者总结道：

> 我们广泛使用机器学习和手工编写的规则。我们系统中的规则不是“锦上添花”。它们对实现预期性能至关重要，并且为领域分析师提供了一种快速有效的方式将反馈输入系统。据我们所知，我们是第一个描述一个工业级系统的团队，在其中[机器学习](https://medium.com/towards-data-science/the-origin-of-intelligent-behavior-3d3f2f659dc2)和规则作为一流公民共存。

唉，这种共存也可能是解决概括/记忆问题的关键。

# 附录：像我们一样学习的系统

总结一下。构建能够同时记忆已知模式和对未知模式进行概括的系统是机器学习的**圣杯**。到目前为止，还没有人完全破解这个问题，但我们已经看到了一些有前景的方向：

+   BERT 已被证明在微调过程中先进行概括，再进行记忆，这种能力得益于其之前的预训练。

+   Wide & Deep 神经网络被设计为同时进行概括（使用深度组件）和记忆（使用宽度组件），在 Google Play 商店推荐中优于仅宽度或仅深度网络。

+   沃尔玛的混合生产系统 Chimera 利用人类专家为其机器学习模型无法记忆的边缘案例编写规则。通过将这些基于规则的决策重新加入训练数据中，随着时间的推移，机器学习模型可以赶上，但**最终**机器学习和规则作为一流公民共存。

这实际上只是对现状的一个小小的窥视。任何工业机器学习团队都需要在某种程度上解决记忆/概括困境。如果你从事机器学习工作，你很可能最终也会遇到这个问题。

解决这个问题的**最终**目标不仅是使我们能够构建更为优越的机器学习系统。它还将使我们能够构建[更像我们学习的系统](https://medium.com/towards-data-science/the-origin-of-intelligent-behavior-3d3f2f659dc2)。

[](https://medium.com/@samuel.flender/subscribe?source=post_page-----ab9c236add3e--------------------------------) [## 不想依赖 Medium 的算法？请注册。

### 不想依赖 Medium 的算法？请注册。通过注册我的电子邮件，确保你不会错过我的下一篇文章…

medium.com](https://medium.com/@samuel.flender/subscribe?source=post_page-----ab9c236add3e--------------------------------)
