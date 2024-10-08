# 推荐系统中的两塔模型崛起

> 原文：[`towardsdatascience.com/the-rise-of-two-tower-models-in-recommender-systems-be6217494831`](https://towardsdatascience.com/the-rise-of-two-tower-models-in-recommender-systems-be6217494831)

## 深入探讨用于消除排序模型偏差的最新技术

[](https://medium.com/@samuel.flender?source=post_page-----be6217494831--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----be6217494831--------------------------------)[](https://towardsdatascience.com/?source=post_page-----be6217494831--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----be6217494831--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----be6217494831--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----be6217494831--------------------------------) ·7 分钟阅读·2023 年 10 月 29 日

--

![](img/c2ea8cba8c6d13e4bf5926fa05096305.png)

照片由 [Evgeny Smirnov](https://unsplash.com/@smirik) 提供

推荐系统是当今世界上最普遍的机器学习应用之一。然而，底层的排序模型存在 许多偏差，这可能严重限制推荐的质量。构建无偏排序器的问题——也称为无偏排序学习（ULTR）——仍然是机器学习领域最重要的研究问题之一，且远未解决。

在这篇文章中，我们将深入探讨一种相对较新的建模方法，这种方法已经使行业能够非常有效地控制偏差，从而构建出极其优越的推荐系统：两塔模型，其中一个塔学习相关性，另一个（浅层）塔学习偏差。

虽然两塔模型可能在行业中使用了好几年，但第一篇正式向更广泛的机器学习社区介绍它们的论文是华为 2019 年的 PAL 论文。

## PAL（华为，2019）——首个两塔模型

华为的论文 [PAL](https://github.com/tangxyw/RecSysPapers/blob/main/Debias/%5B2019%5D%5BHuawei%5D%5BPAL%5D%20a%20position-bias%20aware%20learning%20framework%20for%20CTR%20prediction%20in%20live%20recommender%20systems.pdf) (“位置感知排序学习”) 考虑了在华为应用商店背景下的位置偏差问题。

在整个行业的排序模型中，一再观察到位置偏差。这意味着用户更有可能点击首先展示的项目。这可能是因为他们赶时间、盲目相信排序算法，或其他原因。以下是展示华为数据中位置偏差的图表：

![](img/39763b8fc1bce8500988b90981c66c3b.png)

位置偏差。来源：华为的论文 [PAL](https://github.com/tangxyw/RecSysPapers/blob/main/Debias/%5B2019%5D%5BHuawei%5D%5BPAL%5D%20a%20position-bias%20aware%20learning%20framework%20for%20CTR%20prediction%20in%20live%20recommender%20systems.pdf)

位置偏差是一个问题，因为我们无法确定用户点击第一个项目是因为它确实最相关，还是因为它首先展示——而在推荐系统中，我们旨在解决前者的学习目标，而不是后者。

PAL 论文提出的解决方案是将学习问题分解为

```py
p(click|x,position) = p(click|seen,x) x p(seen|position),
```

其中 x 是特征向量，`seen` 是一个二进制变量，指示用户是否已经看到该展示。在 PAL 中，`seen` 仅依赖于项的位置，但我们也可以添加其他变量（稍后我们会看到）。

基于这一框架，我们可以构建一个具有两个塔的模型，每个塔输出右侧的两个概率之一，然后简单地将这两个概率相乘：

![](img/26a05297dd86a7206de4b19d8c4c6a6c.png)

来源：华为的论文 [PAL](https://github.com/tangxyw/RecSysPapers/blob/main/Debias/%5B2019%5D%5BHuawei%5D%5BPAL%5D%20a%20position-bias%20aware%20learning%20framework%20for%20CTR%20prediction%20in%20live%20recommender%20systems.pdf)

图中的云层实际上是神经网络：一个用于位置塔的浅层网络（因为它只需要处理单一特征），另一个用于 CTR 塔的深层网络（因为它需要处理大量特征并在这些特征之间创建交互）。我们也将这两个塔分别称为偏差塔和参与塔。

值得注意的是，在推理时，位置不可用时，我们仅使用参与塔进行前向传递，而不使用偏差塔。类似于 Dropout，因此模型在训练和推理时的行为有所不同。

它有效吗？是的，PAL 的效果非常好。作者建立了两个不同版本的 DeepFM 排名模型（我在 这里 写过），一个版本使用 PAL，另一个版本使用一种简单的处理项位置的方法：将其作为特征传递到参与塔中。他们的在线 A/B 测试显示，PAL 将点击率和转化率分别提高了大约 25%，这是一个巨大的提升！

PAL 显示位置本身可以作为排名模型的输入，但需要通过专门的塔进行处理，而不是主模型（这一规则也被添加为 [Rule 36](https://developers.google.com/machine-learning/guides/rules-of-ml#rule_36_avoid_feedback_loops_with_positional_features) 到 Google 的“机器学习规则”中）。双塔模型正式诞生——尽管在 PAL 论文之前，这种方法可能已经在业界使用过。

## “接下来观看”（YouTube，2019）——加性双塔模型

YouTube 的论文“[Recommending What Video to Watch Next](https://daiwk.github.io/assets/youtube-multitask.pdf)”——通常被简单称为“Watch Next”论文——发布的时间与 PAL 相近，并试图解决相同的问题：使用双塔模型去偏差推荐系统。然而，与 PAL 相比，YouTube 使用的是*加性*双塔模型，而不是乘性模型。

为了理解这为何有效，再次考虑学习目标的分解：

```py
p(click|x,position) = p(click|seen,x) x p(seen|position)
```

由于概率是 logits 的 sigmoid 函数，sigmoid 函数本质上是加权指数函数，而指数函数遵循指数的乘法规则，因此我们也可以写作：

```py
logit(click|x,position) = logit(click|seen,x) + logit(seen|position),
```

这就是，遗憾的是，加性双塔模型。

![](img/9b68128643857a9aa4c3ef64089a6406.png)

双塔加性模型。来源：“[Recommending What Video to Watch Next](https://daiwk.github.io/assets/youtube-multitask.pdf)”

相比于 PAL，另一个显著的创新是使用除了位置以外的其他特征在浅层塔中。例如，用户设备类型。这是有道理的，因为不同设备预计会表现出不同形式的位置偏差：例如，位置偏差可能在屏幕较小的手机上更为严重，因为用户可以同时看到的位置更少，并且需要更多地滑动。因此，YouTube 模型中的浅层塔不仅学习位置偏差，还学习各种选择偏差，具体取决于传入浅层塔的特征。

为了确保浅层塔实际利用了所有这些特征，作者在仅位置特征上添加了一个 10%丢弃率的 Dropout 层。没有它，模型可能会过度依赖位置特征，而无法学习其他选择偏差。

通过 A/B 测试，作者展示了添加浅层塔提高了他们的“参与度指标”（不幸的是没有给出该指标的详细信息）0.24%。

## 在 ULTR 中解开相关性和偏差（谷歌，2023）

到目前为止，我们假设 ULTR 中的两个塔可以在模型训练期间独立学习。不幸的是，最近的谷歌论文“[Towards Disentangling Relevance and Bias in Unbiased Learning to Rank](https://arxiv.org/abs/2212.13937)”的作者表明，这个假设并不成立。

这里有一个[思想实验](https://www.britannica.com/science/Gedankenexperiment)来说明原因：假设存在一个完美的相关性模型，即一个能够完美解释点击的模型。在这种情况下，一旦我们用新数据重新训练模型，所有的偏差塔需要做的就是学习如何将位置映射到点击。相关性塔则无需学习任何东西——它将完全无用！

换句话说，相关性对位置有干扰效应，如下图因果图中的红色箭头所示。

![](img/7b8e236dd5723ff1974126bedc060891.png)

相关性塔对偏置塔的混淆效应。来源：[Zhang et al 2023](https://arxiv.org/abs/2212.13937)

但这不是我们想要的：我们希望相关性塔在新数据到来时继续学习。解决这个问题的一种简单方法——消除因果图中的红色边缘——是添加一个 Dropout 层到偏置对数值上，如下图所示：

![](img/bc33470348cb5b182aa65c9b8011bc5c.png)

在偏置塔上添加 Dropout。来源：[Zhang et al 2023](https://arxiv.org/abs/2212.13937)。

为什么使用 Dropout？关键思想是通过随机丢弃偏置对数值（bias logit），我们“推动”模型更多地依赖于相关性塔（relevance tower），而不是仅仅学习历史位置和相关性之间的映射。请注意，这一思想与 YouTube 的 Watch Next 论文非常相似，但这里我们丢弃的是整个偏置对数值，而不仅仅是位置特征。

为了使其有效，偏置对数值的 Dropout 概率需要很高：在 0.5 的 Dropout 下（即在 50% 的训练样本中随机将偏置对数值置零），作者在 Chrome 应用商店的生产数据上击败了 PAL 1% 的点击 NDGC——这是这种简单技术的有力证明。

## 总结

让我们回顾一下：

+   双塔模型是一种强大的方法来构建无偏排名模型：一个塔学习相关性，而另一个塔学习位置偏置（以及，选择性地，其他偏置）。

+   我们可以通过乘以两个塔的概率（如华为的 PAL 所做），或者通过加上它们的对数值（如 YouTube 的 Watch Next 所做）来组合两个塔的输出。我们将后者称为加性双塔模型（additive two-tower model）。

+   偏置塔的输入通常是项目的位置，但我们也可以添加其他引入偏置的特征，如用户设备类型。

+   Dropout（无论是在位置上还是在整个偏置对数值上）可以防止模型过度依赖历史位置，并且已被证明可以提高泛化能力。

这仅仅是冰山一角。鉴于推荐系统在今天的科技行业中的重要性，以及发现双塔建模可以显著提高预测性能，这一研究领域可能仍远未发挥其全部潜力：我们真的只是刚刚开始！

*如果你喜欢这些内容并想保持对最新 ML 技术的了解，确保* [*订阅*](https://samflender.substack.com)*。*
