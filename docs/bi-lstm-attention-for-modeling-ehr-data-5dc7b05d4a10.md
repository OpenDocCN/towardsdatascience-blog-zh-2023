# Bi-LSTM+Attention 用于建模 EHR 数据

> 原文：[`towardsdatascience.com/bi-lstm-attention-for-modeling-ehr-data-5dc7b05d4a10`](https://towardsdatascience.com/bi-lstm-attention-for-modeling-ehr-data-5dc7b05d4a10)

## 基于注意力的 Bi-LSTM 网络在医疗保健诊断预测中的基本指南

[](https://satyam-kumar.medium.com/?source=post_page-----5dc7b05d4a10--------------------------------)![Satyam Kumar](https://satyam-kumar.medium.com/?source=post_page-----5dc7b05d4a10--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5dc7b05d4a10--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5dc7b05d4a10--------------------------------) [Satyam Kumar](https://satyam-kumar.medium.com/?source=post_page-----5dc7b05d4a10--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5dc7b05d4a10--------------------------------) ·阅读时间 7 分钟·2023 年 1 月 31 日

--

![](img/1afe40d7a671e4ddf480d8041a9b0ffc.png)

图片由 [Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=6939537) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=6939537)

预测未来健康信息或疾病使用电子健康记录（EHR）是医疗领域研究的一个关键用例。EHR 数据包括诊断代码、药房代码和程序代码。由于数据的高维度，对 EHR 数据进行建模和解释是一项繁琐的任务。

在本文中，我们将讨论一篇流行的研究论文，[DIPOLE](https://arxiv.org/abs/1706.05764)，该论文发表于 2019 年 6 月，使用了 Bi-LSTM+Attention 网络。

```py
**Contents:**
1) Limitations of Linear/Tree-based for Modeling
2) Why RNN models?
3) Essential guide to LSTM & Bi-LSTM network
4) Essential guide to Attention
5) Implementation
```

# 1) 线性/树基模型的局限性：

之前的实现是一个**随机森林模型**，使用一组固定的超参数来建模**聚合的成员级**索赔/药房/人口统计特征。

1.  在疾病预测的情况下，输出依赖于**时间序列中的事件顺序**。在 RF 模型中，这种时间序列信息会被**丢失**。因此，思想是尝试基于时间序列模型的事件预测。候选模型可以是统计时间序列模型，如**ARIMA**、**Holt-Winters**，或基于神经网络的模型，如**RNN/LSTMs**，甚至是**transformer-based 架构**。

1.  然而，事件的**长期依赖性**和事件间（不规则）时间间隔的信息在 RF 模型甚至经典时间序列模型中很难捕捉。

1.  此外，随机森林未能捕捉**非线性关联**和时间排序事件之间的复杂关系。这也是经典 TS 模型的情况。我们可以通过包括交互项（如二次项、乘法项等）或使用核函数（如 SVM 中）来引入非线性，但这取决于我们是否知道实际的非线性依赖关系，而在当前数据中很难找到。

因此，我们首先探索基于神经网络的时间序列模型，如**RNN/LSTM**，然后是**transformer 架构**。关于 RF 和经典 TS 模型的局限性的假设也会通过与 RNN/LSTM 模型的评估指标比较来验证。

# 2) 为什么选择 RNN 模型？

索赔数据包括与每个成员在索赔级别上的诊断、程序和使用情况相关的信息。索赔信息是基于时间的，而现有的 RF 模型没有利用访问的时间信息。

其思想是用更适合时间序列事件预测的东西更新 RF 模型，如 RNN。

![](img/e3f508bf43f4025cc53d7c957477c4c5.png)

([来源](https://en.wikipedia.org/wiki/File:LSTM_Cell.svg))，LSTM 单元

每个 RNN 单元的输入依赖于前一个单元的输出以及时间‘t’的输入序列。一个 RNN 单元会在序列中的每个事件上重复自身。

## RNN 模型的局限性：

RNN 在实践中被发现对数据中的**短期依赖**表现良好。例如，模型使用已有的词预测不完整句子的下一个词。如果我们尝试预测“the clouds are in the sky”中的最后一个词，我们不需要进一步的上下文——下一个词很明显是“the sky”。在这种情况下，当相关信息与所需位置之间的间隔较小时，RNN 可以学会利用过去的信息。

但对于长序列，RNN 模型**遭遇梯度消失/爆炸问题**，这妨碍了事件的长期学习。在反向传播过程中，梯度变得越来越小，参数更新对于早期事件变得微不足道，这意味着没有真正的学习。

RNN 在处理如此长的序列数据时也会变得训练缓慢。

## 替代方案：

**LSTM**（长短期记忆）和**GRU**（门控循环单元）是 RNN 网络的替代品或更新版本，能够捕捉序列事件的长期依赖，而大多数情况下不会遇到梯度消失/爆炸问题。它们通过多个权重和偏置的选择性保留机制来克服 RNN 的问题，而不是使用一个。

# 3) LSTM 和 Bi-LSTM 网络的基本指南：

一个 LSTM 单元有 3 个门（输入门、输出门和遗忘门）来保护和控制单元状态，并将必要的信息添加到当前状态中。LSTM 单元有 3 个输入，即**前一个单元状态**（C_(t-1)）、**前一个单元输出**（h_(t-1)）和**时间‘t’的输入事件**（x_t）。而它有两个输出，即**当前单元状态**（C_t）和**当前输出**（h_t）。

> 请访问 colah 的博客以了解 LSTM 网络如何在后台工作：

[## 理解 LSTM 网络

### 发布于 2015 年 8 月 27 日，人类不会每秒都从头开始思考。当你阅读这篇文章时，你…

[colah.github.io](https://colah.github.io/posts/2015-08-Understanding-LSTMs/?source=post_page-----5dc7b05d4a10--------------------------------)

## Bi-LSTM 网络：

Bi-LSTM 是 LSTM 的一种变体，它在两个方向上流动输入，以保留未来和过去的信息。

![](img/febc40c40cef2572207506d386221d57.png)

([来源](https://en.wikipedia.org/wiki/Bidirectional_recurrent_neural_networks#/media/File:Structural_diagrams_of_unidirectional_and_bidirectional_recurrent_neural_networks.png))，Bi-LSTM 网络

正向 LSTM 从 x_1 到 x_t 读取输入访问序列，并计算一系列正向隐藏状态。反向 LSTM 以相反的顺序，即从 x_t 到 x_1，读取访问序列，从而生成一系列反向隐藏状态。通过连接正向隐藏状态和反向隐藏状态，我们可以得到最终的潜在向量表示为 h_i。

**使用 Bi-LSTM 层而不是 LSTM 层的原因：**

+   以捕捉序列的完整上下文

+   很多时候，只有在回顾未来时，事情才会变得清晰。

**Bi-LSTM 的缺点：**

+   它们分别学习从左到右和从右到左的上下文，并将它们连接在一起。因此，真实的上下文在某种程度上丢失了。

+   假设每个事件的时间间隔相等

+   由于其序列性质，可能在大数据训练时变得较慢。

Transformers 可以克服上述缺点。

# 4) 注意力机制的基本指南：

注意力机制由[Bahdanau 等（2014）](https://arxiv.org/abs/1409.0473)的论文引入，以解决解码器在获取输入提供的信息时所遇到的瓶颈问题。

注意力模型使网络能够**一次关注几个特定方面/事件**并忽略其余部分。

在我们的实现中，我们添加了一个注意力层来捕捉访问向量的重要性，以进行任何预测。对应于访问向量的注意力得分越大，它在预测时的重要性就越高。

# 5) 实现：

我们当前的 Bi-LSTM 实现灵感来自于[Fenglong Ma 及团队的[DIPOLE 论文（2017 年 6 月）](https://arxiv.org/pdf/1706.05764.pdf)。该论文使用 Bi-LSTM 网络对 EHR 数据进行建模，并利用简单的注意力机制来解释结果。

我在下面讨论了我们当前实现的逐步方法：

## a) 数据：

我们使用数据（电子健康数据 — EHR）进行 Bi-LSTM+Attention 建模。

> 注意：这与我们用于线性/树模型的数据相同。

目前，我们的模型仅限于诊断医疗代码。

![](img/9c53e36be38330797f76800aae747855.png)

（作者提供的图片），原始 EHR 数据的快照

## b) 特征工程（EHR 数据）：

+   索赔数据是按行级别的，我们选择每个索赔的第一条记录，以将其转化为访问级别。

+   为训练数据中的所有唯一诊断代码准备一个诊断标签编码器。

![](img/48a435c418da78559d8b2e2484819677.png)

+   对每次访问的诊断代码进行独热编码。

+   选择每个成员的最后 x 次访问/索赔 *(超参数)*。如果一个成员的访问次数未达到阈值，我们用零向量填充剩余的访问记录。

![](img/44a9520fe157fd1a25bcd106dd987a55.png)

+   将数据格式化为适合 LSTM 网络输入的形式 `**(成员, 访问次数, 唯一医疗代码)**`

> 对于一个包含 1000 名成员和 5000 个唯一医疗代码的数据集，并将访问记录填充至 10，我们将得到最终的训练数据形状为：（1000, 10, 5000）

![](img/ec4274cc96711f24af76b70e6fe8ba78.png)

LSTM 数据输入格式

## c) Keras 中的 Bi-LSTM + Attention

我们将使用 `**tf.Keras**` 框架来实现 Bi-LSTM + Attention 网络，以进行疾病预测建模。

（作者提供的代码）

## d) 解释

在医疗保健中，学习到的医疗代码和访问的表示的可解释性很重要。我们需要理解医疗代码表示的每一维度的临床意义，并分析哪些访问对预测至关重要。

由于提出的模型基于注意力机制，我们可以通过分析注意力分数来找到每次访问在预测中的重要性。

## f) 模型摘要

![](img/768c3117696cdeb6cbd9780f4fa72569.png)

（作者提供的图片），模型摘要

# 摘要：

DIPOLE 实现使用 Bi-LSTM 架构网络，以捕捉历史 EHR 数据的长期和短期依赖关系。注意力机制可以用来解释预测结果、学习到的医疗代码和访问级别信息。

根据论文作者的说法，DIPOLE 可以显著提高与传统最先进的诊断预测方法相比的性能。

# 参考文献：

[1] 理解 LSTM (*2015 年 8 月 27 日*): [`colah.github.io/posts/2015-08-Understanding-LSTMs/`](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)

[2] DIPOLE (*2017 年 6 月 19 日*): [`arxiv.org/pdf/1706.05764.pdf`](https://arxiv.org/pdf/1706.05764.pdf)

[3] 带有注意力机制的 Bi-LSTM 实现 (*2021 年 8 月 22 日*): [`analyticsindiamag.com/hands-on-guide-to-bi-lstm-with-attention/`](https://analyticsindiamag.com/hands-on-guide-to-bi-lstm-with-attention/)

> 感谢阅读
