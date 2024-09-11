# 使用 JAX 和 Haiku 从头实现 Transformer 编码器 🤖

> 原文：[https://towardsdatascience.com/implementing-a-transformer-encoder-from-scratch-with-jax-and-haiku-791d31b4f0dd?source=collection_archive---------7-----------------------#2023-11-07](https://towardsdatascience.com/implementing-a-transformer-encoder-from-scratch-with-jax-and-haiku-791d31b4f0dd?source=collection_archive---------7-----------------------#2023-11-07)

## 理解 Transformers 的基本构建模块。

[](https://medium.com/@ryanpegoud?source=post_page-----791d31b4f0dd--------------------------------)[![Ryan Pégoud](../Images/9314b76c2be56bda8b73b4badf9e3e4d.png)](https://medium.com/@ryanpegoud?source=post_page-----791d31b4f0dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----791d31b4f0dd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----791d31b4f0dd--------------------------------) [Ryan Pégoud](https://medium.com/@ryanpegoud?source=post_page-----791d31b4f0dd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F27fba63b402e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-a-transformer-encoder-from-scratch-with-jax-and-haiku-791d31b4f0dd&user=Ryan+P%C3%A9goud&userId=27fba63b402e&source=post_page-27fba63b402e----791d31b4f0dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----791d31b4f0dd--------------------------------) ·12分钟阅读·2023年11月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F791d31b4f0dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-a-transformer-encoder-from-scratch-with-jax-and-haiku-791d31b4f0dd&user=Ryan+P%C3%A9goud&userId=27fba63b402e&source=-----791d31b4f0dd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F791d31b4f0dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-a-transformer-encoder-from-scratch-with-jax-and-haiku-791d31b4f0dd&source=-----791d31b4f0dd---------------------bookmark_footer-----------)![](../Images/bdeff665e8c9cb3e1a1bdb370c5de52f.png)

Transformers，以爱德华·霍珀（由 Dall.E 3 生成）的风格

在2017年的开创性论文“[***注意力机制就是你需要的***](https://arxiv.org/pdf/1706.03762.pdf)***”***[0]中介绍的 Transformer 架构可以说是近期深度学习历史上最具影响力的突破之一，它使大型语言模型的兴起成为可能，并且在计算机视觉等领域也找到了应用。

继承了依赖于**递归**的前沿架构，如长短期记忆（**LSTM**）网络或门控循环单元（**GRU**），**Transformer**引入了**自注意力**的概念，并结合了**编码器/解码器**架构。

在本文中，我们将从零开始一步一步实现Transformer的前半部分，即**编码器**。我们将使用**JAX**作为主要框架，并结合**Haiku**，这是DeepMind的深度学习库之一。

如果你对JAX不熟悉或需要对其惊人功能有一个新的提醒，我在我的**上一篇文章**中已经涵盖了这个话题：

[## 使用JAX向量化和并行化强化学习环境：光速Q-learning⚡](/vectorize-and-parallelize-rl-environments-with-jax-q-learning-at-the-speed-of-light-49d07373adf5?source=post_page-----791d31b4f0dd--------------------------------)

### 学习如何向量化一个GridWorld环境，并在CPU上并行训练30个Q-learning代理，步数达到180万……

[towardsdatascience.com](/vectorize-and-parallelize-rl-environments-with-jax-q-learning-at-the-speed-of-light-49d07373adf5?source=post_page-----791d31b4f0dd--------------------------------)

我们将逐一讲解构成编码器的每个模块，并学习如何高效地实现它们。特别是，本文的纲要包括：

+   **嵌入层** 和 **位置编码**

+   **多头注意力**

+   **残差连接** 和 **层归一化**

+   **位置-wise前馈网络**

*免责声明：本文并非这些概念的完整介绍，我们将首先关注实现部分。如有需要，请参阅本文末尾的资源。*

***一如既往，本文的完整注释代码以及插图笔记本可在*** [***GitHub***](https://github.com/RPegoud/jab)***上获得，如果你喜欢这篇文章，欢迎给仓库加星！***

[## GitHub — RPegoud/jab: 一系列在JAX中实现的基础深度学习模型](https://github.com/RPegoud/jab?source=post_page-----791d31b4f0dd--------------------------------)

### 在JAX中实现的一系列基础深度学习模型 — GitHub — RPegoud/jab: 一系列…

[github.com](https://github.com/RPegoud/jab?source=post_page-----791d31b4f0dd--------------------------------)

## 主要参数

在我们开始之前，我们需要定义几个在编码器模块中发挥重要作用的参数：

+   **序列长度** (`seq_len`): 序列中的标记或词的数量。

+   **嵌入维度** (`embed_dim`): 嵌入的维度，换句话说，就是用来描述单个标记或词的数值数量。

+   **批量大小 (**`batch_size`**):** 输入批量的大小，即同时处理的序列数量。

我们的编码器模型的输入序列通常是**（**`batch_size`**，** `seq_len`**）**的形状。在本文中，我们将使用`batch_size=32`和`seq_len=10`，这意味着我们的编码器将同时处理32个10词的序列。

关注每一步处理中的数据形状将使我们更好地可视化和理解数据在编码器块中的流动。以下是我们编码器的高级概述，我们将从底部开始，介绍**嵌入层**和**位置编码**：

![](../Images/818dc653dc223239b2d0b4bd7739222f.png)

**Transformer编码器块**的表示（由作者制作）

# 嵌入层和位置编码

如前所述，我们的模型接受批处理的令牌序列作为输入。生成这些令牌可能像收集数据集中一组唯一词汇并为每个词汇分配一个索引一样简单。然后，我们将采样**32**个**10词**的**序列**，并用词汇中的索引替换每个词。这一过程将为我们提供一个形状为**（**`batch_size`**，** `seq_len`**）**的数组，正如预期的那样。

我们现在准备开始使用编码器。第一步是为我们的序列创建“***位置嵌入***”。位置嵌入是**词嵌入**和**位置编码**的**和**。

## 词嵌入

词嵌入使我们能够编码**词汇中**的**意义**和**语义关系**。在本文中，嵌入维度固定为**64**。这意味着每个词由一个**64维向量**表示，从而具有相似意义的词具有相似的坐标。此外，我们可以操作这些向量来**提取词之间的关系**，如下所示。

![](../Images/55b06988518a9554add4ad341f107320.png)

从词嵌入派生的类比示例（图片来自 developers.google.com）

使用 Haiku，生成可学习的嵌入就像调用一样简单：

```py
hk.Embed(vocab_size, embed_dim)
```

这些嵌入将在模型训练期间与其他可学习的参数一起更新（*稍后会详细介绍*）。

## 位置编码

与递归神经网络不同，Transformers无法根据共享的隐藏状态推断令牌的位置，因为它们**缺乏递归**或**卷积结构**。因此，引入了**位置编码**，这些向量传达了**令牌在输入序列中的位置**。

从本质上讲，每个令牌被分配一个由**交替的正弦和余弦值**组成的**位置向量**。这些向量的维度与词嵌入相匹配，以便两者可以相加。

特别是，原始的 Transformer 论文使用了以下函数：

![](../Images/683d82eb429e0126a88dbded90881ceb.png)![](../Images/c29c91a5570ead1acef50cc6df4c3e59.png)

位置编码函数（转载自“Attention is all you need”，Vaswani等，2017）

下面的图使我们能够进一步理解位置编码的功能。让我们来看一下最上面图的第一行，我们可以看到**交替的零和一序列**。实际上，行表示序列中一个 token 的位置（`pos` 变量），而列表示嵌入维度（`i` 变量）。

因此，当 `pos=0` 时，之前的方程对偶数嵌入维度返回 `sin(0)=0`，对奇数维度返回 `cos(0)=1`。

此外，我们看到相邻的行具有相似的值，而第一行和最后一行则差异很大。这一特性有助于模型评估序列中**单词之间的距离**以及它们的**顺序**。

最后，第三个图表示位置编码和嵌入的总和，这就是嵌入块的输出。

单词嵌入和位置编码的表示，seq_len=16 和 embed_dim=64（由作者制作）

使用 Haiku，我们将嵌入层定义如下。与其他深度学习框架类似，Haiku 允许我们定义**自定义模块**（此处为 `hk.Module`），以**存储可学习的参数**和**定义模型组件的行为**。

每个 Haiku 模块需要有一个 `__init__` 和 `__call__` 函数。在这里，call 函数简单地使用 `hk.Embed` 函数和位置编码计算嵌入，然后对其进行求和。

位置编码函数使用 JAX 功能，如 `vmap` 和 `lax.cond` 来提高性能。如果你对这些函数不熟悉，可以查看我的[上一篇文章](https://medium.com/towards-data-science/vectorize-and-parallelize-rl-environments-with-jax-q-learning-at-the-speed-of-light-49d07373adf5)，那里对这些函数进行了更深入的介绍。

简而言之，`vmap` 允许我们为**单个样本**定义一个函数并**将其向量化**，以便它可以应用于**数据批次**。`in_axes` 参数用于指定我们要遍历 `dim` 输入的第一个轴，即嵌入维度。另一方面，`lax.cond` 是 XLA 兼容版本的 Python if/else 语句。

# 自注意力和多头注意力

注意力旨在计算**序列中每个单词的重要性**，**相对于输入单词**。例如，在句子中：

> “那只黑猫跳上沙发，躺下并入睡，因为它累了。”

词语“**它**”对于模型来说可能会相当模糊，因为*从技术上讲*，它可以指代“**猫**”或“**沙发**”。一个经过良好训练的注意力模型能够理解“**它**”指的是“**猫**”，并因此为句子的其余部分分配相应的注意力值。

本质上，**注意力值**可以视为**权重**，描述了某个单词**在给定输入上下文中的重要性**。例如，“**跳跃**”一词的注意力向量会对“**猫**”（*跳跃了什么？*）、“**在**”和“**沙发**”（*跳跃到哪里？*）等词具有较高的值，因为这些词**与其上下文相关**。

![](../Images/31016a82455c64d4f840c4bbe4861bad.png)

**注意力向量**的可视化表示（由作者制作）

在 Transformer 论文中，注意力是使用***缩放点积注意力***计算的。其公式总结如下：

![](../Images/252c2d3a224b203a09510ff5ba3d43a9.png)

缩放点积注意力（重现自“Attention is all you need”，*Vaswani et al. 2017*）

在这里，Q、K 和 V 分别代表***查询、键***和***值***。这些矩阵是通过将学习到的权重向量 WQ、WK 和 WV 与位置嵌入相乘得到的。

这些名称主要是**抽象概念**，用于帮助理解信息在注意力块中的处理和加权方式。它们暗指**检索系统**的词汇[2]（例如，在 YouTube 上搜索视频）。

这里是一个**直观的**解释：

+   **查询**：它们可以被理解为关于序列中所有位置的“*一组问题*”。例如，询问一个单词的上下文并试图识别序列中最相关的部分。

+   **键**：它们可以被视为包含查询交互的信息，查询与键之间的兼容性决定了查询应该给予对应值多少注意力。

+   **值**：匹配的键和查询使我们能够决定哪些键是相关的，值是与键配对的实际内容。

在下图中，查询是 YouTube 搜索，键是视频描述和元数据，而值是相关联的视频。

![](../Images/483f936afe83dcd9d81810153894e7b2.png)

查询、键、值概念的直观表示（由作者制作）

在我们的情况下，查询、键和值来自于**同一来源**（因为它们是从输入序列派生的），因此被称为**自注意力**。

注意力分数的计算通常是**多次并行执行**的，每次使用**部分嵌入**。这一机制被称为“**多头注意力**”，使每个头可以并行地学习数据的几种不同表示，从而形成更**强健**的模型。

单个注意力头通常处理形状为 (`batch_size, seq_len, d_k`**)** 的数组，其中 `d_k` 可以设置为头数与嵌入维度的比率（`d_k = n_heads/embed_dim`）。这样，连接每个头的输出就能方便地得到形状为**（`batch_size, seq_len, embed_dim`**）的数组，作为输入。

注意力矩阵的计算可以分解为几个步骤：

+   首先，我们定义**可学习的权重向量** WQ、WK 和 WV。这些向量的形状为（`n_heads, embed_dim, d_k`）。

+   同时，我们将**位置嵌入**与**权重向量**相乘。我们得到形状为（`batch_size, seq_len, d_k`）的 Q、K 和 V 矩阵。

+   然后，我们对 Q 和 K（转置）的点积进行**缩放**。这个缩放操作包括将点积的结果除以 `d_k` 的平方根，并在矩阵的行上应用 softmax 函数。因此，对于输入的令牌（即一行），注意力分数总和为一，这有助于防止值变得过大而减慢计算速度。输出的形状为 (`batch_size, seq_len, seq_len`)。

+   最后，我们将上一操作的结果与 V 进行点乘，输出的形状为 (`batch_size, seq_len, d_k`)。

![](../Images/53a16f4b352d266f793268e64d9cd1e6.png)

**注意块**内部的矩阵操作的可视化表示（作者制作）

+   然后，每个注意力头的输出可以**串联**起来形成一个形状为（`batch_size, seq_len, embed_dim`）的矩阵。Transformer论文还在多头注意力模块的最后添加了一个**线性层**，用于**汇聚**和**组合**来自**所有注意力头**的学习表示。

![](../Images/2b83a1bf528562df5c6794e0dffe2819.png)

多头注意力矩阵的串联和线性层（作者制作）

在Haiku中，多头注意力模块可以如下实现。`__call__`函数遵循与上述图表相同的逻辑，而类方法利用JAX实用程序，如`vmap`（在不同注意力头和矩阵上向量化我们的操作）和`tree_map`（在权重向量上映射矩阵点积）。

# 残差连接和层归一化

正如你在Transformer图中所注意到的，多头注意力块和前馈网络之后跟着**残差连接**和**层归一化**。

## 残差连接或跳跃连接

残差连接是解决**梯度消失问题**的标准解决方案，即当梯度变得太小以至于无法有效更新模型参数时。

由于这个问题在特别深的架构中自然而然地出现，所以残差连接被用在各种复杂模型中，比如在计算机视觉中的**ResNet**（[*Kaiming et al*](https://arxiv.org/abs/1512.03385v1)，2015年），在强化学习中的**AlphaZero**（[*Silver et al*](https://arxiv.org/abs/1712.01815v1)，2017年），当然还有**Transformers**。

在实践中，残差连接简单地将特定层的输出直接转发到下一层，**跳过一个或多个层**。例如，围绕多头注意力的残差连接相当于将多头注意力的输出与位置嵌入求和。

这使得梯度在反向传播过程中更有效地流动，通常可以导致**更快的收敛**和更**稳定的训练**。

![](../Images/abba4a948e3ceaf4187f0f3469983f17.png)

Transformer 中**残差连接**的表示（由作者制作）

## 层归一化

层归一化有助于确保通过模型传播的值不会“***爆炸***”（趋向无穷大），这种情况在注意力模块中很容易发生，因为在每次前向传递中会有多个矩阵相乘。

与在批次维度上进行归一化并假设均匀分布的批量归一化不同，**层归一化在特征**上进行归一化。此方法适用于句子批次，因为每个句子可能由于**不同的意义**和**词汇**而具有**独特的分布**。

通过在**特征**上进行归一化，例如**嵌入**或**注意力值**，层归一化**将数据标准化**为一致的尺度，**而不会混淆不同句子的特征**，保持每个句子的独特分布。

![](../Images/ac49f30b74e842229d2043091b9ef894.png)

Transformer 中**层归一化**的表示（由作者制作）

层归一化的实现非常简单，我们初始化可学习的参数 alpha 和 beta，并在所需的特征轴上进行归一化。

# **位置-wise 前馈网络**

编码器中我们需要覆盖的最后一个组件是**位置-wise 前馈网络**。这个全连接网络以注意力块的归一化输出作为输入，用于引入**非线性**并增加**模型的容量**以学习复杂的函数。

它由两个密集层组成，中间隔着一个 [gelu 激活函数](https://paperswithcode.com/method/gelu)。

在这个模块之后，我们有另一个残差连接和层归一化，以完成编码器。

# 总结

就这样！到现在你应该对 Transformer 编码器的主要概念很熟悉了。这是完整的编码器类，请注意，在 Haiku 中，我们为每一层分配了一个名称，以便学习参数分开且易于访问。`__call__`函数很好地总结了我们编码器的不同步骤：

要在实际数据上使用此模块，我们必须将 `hk.transform` 应用于封装编码器类的函数。确实，你可能会记得 JAX 采用了**函数式编程**范式，因此，Haiku 遵循相同的原则。

我们定义了一个包含编码器类实例的函数，并返回前向传递的输出。应用 `hk.transform` 返回一个转换对象，具有两个函数：`init` 和 `apply`。

前者使我们能够用一个随机键和一些虚拟数据初始化模块（请注意这里我们传递的是一个形状为`batch_size, seq_len`的零数组），而后者则允许我们处理真实数据。

```py
# Note: the two following syntaxes are equivalent
# 1: Using transform as a class decorator
@hk.transform
def encoder(x):
  ...
  return model(x) 

encoder.init(...)
encoder.apply(...)

# 2: Applying transfom separately
def encoder(x):
  ...
  return model(x)

encoder_fn = hk.transform(encoder)
encoder_fn.init(...)
encoder_fn.apply(...)
```

在下一篇文章中，我们将**完成 Transformer** 架构，通过添加一个 **解码器**，它重用了我们迄今为止介绍的大部分模块，并学习如何使用 Optax **训练模型** 以完成特定任务！

# **结论**

**感谢你读到这里**，如果你对代码感兴趣，可以在 GitHub 上找到完整的注释以及额外的细节和使用玩具数据集的示例。

[](https://github.com/RPegoud/jab?source=post_page-----791d31b4f0dd--------------------------------) [## GitHub - RPegoud/jab: 一组用 JAX 实现的基础深度学习模型

### 一组用 JAX 实现的基础深度学习模型 - GitHub - RPegoud/jab: 一组…

github.com](https://github.com/RPegoud/jab?source=post_page-----791d31b4f0dd--------------------------------)

如果你想更深入了解 Transformers，以下部分包含了一些帮助我撰写本文的文章。

下次见 👋

# 参考文献和资源：

[1] [***Attention is all you need***](https://arxiv.org/pdf/1706.03762.pdf) (2017), Vaswani 等，谷歌

[2] [***注意机制中的键、查询和值到底是什么？***](https://stats.stackexchange.com/questions/421935/what-exactly-are-keys-queries-and-values-in-attention-mechanisms)*(2019)*Stack Exchange

[3] [***图解 Transformer***](http://jalammar.github.io/illustrated-transformer/)*(2018)，* [Jay Alammar](http://jalammar.github.io/)

[4] [***Transformer 模型中位置编码的温和介绍***](https://machinelearningmastery.com/a-gentle-introduction-to-positional-encoding-in-transformer-models-part-1/)*(2023)，* [**Mehreen Saeed**](https://machinelearningmastery.com/author/msaeed/)，Machine Learning Mastery

+   [***JAX 文档***](https://jax.readthedocs.io/en/latest/index.html)

+   [***Haiku 文档***](https://dm-haiku.readthedocs.io/en/latest/notebooks/basics.html)

# 图片来源

+   [词嵌入](https://developers.google.com/machine-learning/crash-course/embeddings/translating-to-a-lower-dimensional-space?hl=fr)，developers.google.com

+   猫的照片，[Karsten Winegeart](https://unsplash.com/fr/photos/bulldog-francese-marrone-che-indossa-una-camicia-gialla-5PVXkqt2s9k)，Unsplash

+   挪威风景，[Pascal Debrunner](https://unsplash.com/fr/photos/corpo-de-agua-perto-da-montanha-LKOuYT5_dyw)，Unsplash

+   狗的照片，[Loan](https://unsplash.com/fr/photos/chaton-tabby-argente-sur-marbre-7AIDE8PrvA0)，Unsplash
