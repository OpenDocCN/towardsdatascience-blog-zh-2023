# LoRA — 直观且详尽的解释

> 原文：[`towardsdatascience.com/lora-intuitively-and-exhaustively-explained-e944a6bff46b`](https://towardsdatascience.com/lora-intuitively-and-exhaustively-explained-e944a6bff46b)

## 自然语言处理 | 机器学习

## 探索现代机器学习的前沿，通过前沿微调

[](https://medium.com/@danielwarfield1?source=post_page-----e944a6bff46b--------------------------------)![Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----e944a6bff46b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e944a6bff46b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e944a6bff46b--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----e944a6bff46b--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e944a6bff46b--------------------------------) ·18 分钟阅读·2023 年 11 月 7 日

--

![](img/ea13e6af7448529f375da8c5e914e006.png)

“Lora The Tuner” 作者 Daniel Warfield 使用 MidJourney 制作。所有图片均为作者所用，除非另有说明。

微调是将机器学习模型调整到特定应用的过程，这在实现一致和高质量的性能中可能至关重要。在本文中，我们将讨论“低秩适应”（LoRA），这是最受欢迎的微调策略之一。首先我们将介绍理论，然后使用 LoRA 微调语言模型，提高其问答能力。

![](img/88f897196d79cc9be45b43f42a722cb2.png)

微调的结果。在微调之前，输出是乱码，模型不断重复问题和虚假的答案。微调后，输出清晰、简洁且准确。

**这对谁有用？** 任何对学习最新机器学习方法感兴趣的人。本文将重点关注语言建模，但 LoRA 在许多机器学习应用中是一个受欢迎的选择。

**这篇文章有多先进？** 本文应该对新手数据科学家和爱好者可读，但包含在高级应用中至关重要的主题。

**前提条件：** 虽然不是必需的，但对大型语言模型（LLMs）有一个扎实的工作理解可能会有帮助。欢迎参考我的关于变换器的文章，变换器是语言模型的一种常见形式，了解更多信息：

[](/transformers-intuitively-and-exhaustively-explained-58a5c5df8dbb?source=post_page-----e944a6bff46b--------------------------------) ## 变换器 — 直观且详尽的解释

### 探索现代机器学习的前沿：逐步拆解变换器

[towardsdatascience.com

你也可能想了解什么是梯度。我还有一篇相关的文章：

## 什么是梯度？它们为什么会爆炸？

### 通过阅读这篇文章，你将对深度学习中最重要的概念有一个坚实的理解。

towardsdatascience.com

如果你对这两个主题都不太自信，你仍然可以从这篇文章中获益，但如果感到困惑，它们会存在。

# 什么是微调？为什么要微调？

随着机器学习技术的进步，对模型性能的期望也有所提高；这要求更复杂的机器学习方法来满足对更高性能的需求。在早期的机器学习中，构建一个模型并在一次训练中完成是可行的。

![](img/f38a87df35c70af383c252be5c3a7263.png)

训练，最简单的定义就是：你拿一个未经训练的模型，给它数据，然后得到一个性能优越的模型。

这种策略仍然是解决简单问题的流行方法，但对于更复杂的问题，将训练分为“预训练”和“微调”两个部分可能更有用。一般思路是在大数据集上进行初始训练，然后在量身定制的数据集上对模型进行优化。

![](img/eae0e38b25446fe393550adca71bc765.png)

预训练和微调，是对典型单次训练策略的改进。

这种“预训练”然后“微调”的策略可以让数据科学家利用多种数据形式，并将大型预训练模型用于特定任务。因此，预训练然后微调是一种常见且极其强大的范式。然而，它也有一些困难，我们将在下一节讨论。

# 微调中的困难

微调的最基本形式是使用你用于预训练模型的完全相同的过程，然后在新数据上微调该模型。例如，你可能会在一个巨大的通用文本数据集上训练一个模型，然后使用相同的训练策略在一个更具体的数据集上进行微调。

![](img/eae0e38b25446fe393550adca71bc765.png)

在最简单的形式下，预训练和微调在程序上是相同的。你在一组数据上预训练模型，然后在另一组数据上微调。

这种策略可能很昂贵。LLM（大规模语言模型）绝对庞大，要使用这种策略进行微调，你需要足够的内存来存储不仅是整个模型，还要存储整个模型中每个参数的梯度（梯度是让模型知道如何调整其参数方向的东西）。参数和梯度都需要存储在 GPU 上，这就是为什么训练 LLM 需要如此多的 GPU 内存。

![](img/a448ee196ed4fdcbceb751d6aabc73e1.png)

反向传播是训练机器学习模型时使用的策略。机器学习模型是“可微分的”，这意味着你可以计算“梯度”，梯度可以告诉你对某个参数的小变化会如何影响模型输出。我们生成预测，计算梯度，计算预测的错误程度，然后使用梯度来改善模型的参数。预训练和微调都采用反向传播，这需要计算模型中每个可学习参数的梯度。这意味着，如果你有一个 1000 亿参数的模型，你还需要存储 1000 亿个梯度。这一过程会重复进行，可能达到数十亿次，以训练模型。

除了存储梯度的问题，通常还会保存“检查点”，即在训练过程中模型在特定状态下的副本。这是一种很好的策略，可以在微调过程的不同阶段实验模型，但这意味着我们需要存储大量完整尺寸的模型副本。流行的现代 LLM Falcon 180B 需要大约 360GB 的存储空间。如果我们希望在微调过程中保存模型的十个检查点，这将消耗 3.6TB 的存储，这是一笔巨大的开支。也许更重要的是，保存如此大量的数据需要时间。数据通常需要从 GPU 转移到 RAM，然后再到存储器，这可能会显著延长微调过程的时间。

LoRA 可以帮助我们解决这些问题以及更多。减少 GPU 内存使用，缩小文件大小，加快微调时间，等等。从实际意义上讲，LoRA 通常被认为是传统微调方法的直接升级。我们将在接下来的部分详细介绍 LoRA 如何工作以及它如何实现如此显著的改进。

# LoRA 简介

“低秩适配”（LoRA）是一种“参数高效微调”（PEFT）的形式，它允许使用少量可学习的参数来微调大型模型。LoRA 使用了一些概念，这些概念结合在一起，可以大幅度提高微调效果。

1.  我们可以将微调视为学习对参数的更改，而不是直接调整参数本身。

1.  我们可以尝试通过删除重复信息将这些更改压缩为更小的表示形式。

1.  我们可以通过简单地将更改添加到预训练的参数中来“加载”我们的更改。

如果这让你感到困惑，不必担心；在接下来的部分中，我们将一步步讲解这些概念。

# 1) 微调作为参数变化

正如我们之前讨论的，最基本的微调方法包括迭代更新参数。就像正常的模型训练一样，你让模型进行推断，然后根据该推断的错误程度来更新模型的参数。

![](img/a448ee196ed4fdcbceb751d6aabc73e1.png)

回顾之前讨论的反向传播图。这是微调的基本形式。

LoRA 对此有不同的看法。与其把微调看作是学习更好的参数，不如把微调看作是学习参数的变化。你可以将模型参数固定不变，学习使模型在微调任务上表现更好的参数变化。

这与训练的方式非常相似；你让模型进行推断，然后根据推断的错误程度进行更新。然而，不是更新模型参数，而是更新模型参数的变化。

![](img/d1c4548035490a428d0a9549a5f9254b.png)

在 LoRA 中，我们冻结模型参数，并创建一组新的值来描述这些参数的变化。然后，我们学习使模型在微调任务上表现更好的参数变化。

你可能会觉得这有点儿抽象。LoRA 的核心目的是使微调变得更小更快，如何通过增加更多的数据和额外的步骤来实现呢？在下一节中我们将详细讨论这一点。

# 2) 参数变化压缩

为了说明问题，许多人将密集网络表示为一系列加权连接。每个输入乘以某个权重，然后加在一起以产生输出。

![](img/abb14b4b1f2a4074d288a20a84c0aeb2.png)

密集网络的概念图，展示了一系列由权重连接的神经元。某个特定神经元的值将是所有输入乘以相应权重后的总和。

从概念上讲，这是一种完全准确的可视化，但实际上，这是通过矩阵乘法来实现的。一个值矩阵（称为权重矩阵）与输入向量相乘，生成输出向量。

![](img/ec97bb13a3ee76c3aec028567378d9bd.png)

矩阵乘法的概念图。[来源](https://en.wikipedia.org/wiki/Matrix_multiplication#/media/File:Matrix_multiplication_diagram_2.svg)

为了让你了解矩阵乘法的工作原理。在上面的例子中，红点等于 a₁₁•b₁₂ + a₁₂•b₂₂。正如你所见，这种乘法和加法的组合与神经元示例中的非常相似。如果我们创建正确形状的矩阵，矩阵乘法最终会与加权连接的概念完全一致。

![](img/6e209e6a8f4ccf37c5172622475043c8.png)

将密集网络视为左侧的加权连接，右侧则视为矩阵乘法。在右侧的图示中，左侧的向量是输入，中心的矩阵是权重矩阵，右侧的向量是输出。为了可读性，只包含了一部分值。

从 LoRA 的角度来看，理解权重实际上是一个矩阵非常重要，因为矩阵具有某些属性，可以用来压缩信息。

## 矩阵属性 1) 线性独立性

你可以将矩阵（即二维值数组）视为向量的行或列。现在我们只需将矩阵视为向量的行。假设我们有一个由两个向量组成的矩阵，其大致如下所示：

![](img/1ef49f62e375e874e64ba7bbcd69c7a6.png)

一个由两个向量组成的矩阵，这些向量在矩阵中被表示为行。

每个向量指向不同的方向。你不能通过压缩和扩展一个向量来使其与另一个向量相等。

![](img/6a9d31bad32542015e594d707e794946.png)

每一行作为一个矩阵，绘制为一个向量。无论蓝色向量如何被压缩或扩展，它都不会指向与红色向量相同的方向，反之亦然。

让我们加入第三个向量。

![](img/67cd87845c2c930313d0319d04fd0e5e.png)

向量`A`和`B`指向完全相同的方向，而向量`C`指向不同的方向。因此，无论你如何压缩或扩展`A`或`B`，它们都无法用来描述`C`。因此，`C`与`A`和`B`是线性独立的。然而，你可以将`A`扩展为等于`B`，反之亦然，因此`A`和`B`是线性相关的。

假设`A`和`B`指向稍有不同的方向。

![](img/a9531adc7177a666feaa82cea38828a5.png)

现在`A`和`B`可以一起使用（经过一些压缩和扩展）来描述`C`，同样`A`和`B`也可以被其他向量描述。在这种情况下，我们会说这些向量没有线性独立性，因为所有向量都可以用矩阵中的其他向量来描述。

![](img/eb9a3b70424742ff0a0dfe754902fc88.png)

使用`A`和`B`来描述`C`。`B`的大小可以通过乘以一个负数来翻转其大小，然后加到`A`上。

从概念上讲，线性独立的向量可以被视为包含不同的信息，而线性相关的向量则包含一些重复的信息。

## 矩阵属性 2) 秩

秩的概念是量化矩阵中的线性独立性。我将跳过详细的内容，直接进入要点：我们可以将矩阵分解为一些线性独立的向量；这种形式的矩阵称为“简化行最简形”。

![](img/5c4ba81672157aced67c5768d471cbbc.png)

一个矩阵（左）和同一个矩阵的简化行最简形式（右）。在 RREF 矩阵中，你可以看到有四个线性独立的向量（行）。这些向量可以组合使用来描述输入矩阵中的所有向量。

通过将矩阵分解成这种形式（我不会描述如何做，因为这只是对我们概念上有用），你可以计算出多少个线性独立的向量可以用来描述原始矩阵。线性独立向量的数量就是矩阵的“秩”。上述 RREF 矩阵的秩为四，因为有四个线性独立的向量。

我在这里插一句：无论你是否将矩阵视为向量的行还是列，秩始终是相同的。这是一个数学上的小细节，虽然不是特别重要，但对下一节有概念上的影响。

## 矩阵属性 3) 矩阵因子

因此，矩阵可以包含某种程度的“重复信息”，这种信息以线性依赖的形式存在。我们可以利用这一点，通过因式分解来表示一个大矩阵为两个较小矩阵的乘积。类似于如何将一个大数字表示为两个小数字的乘积，矩阵也可以被视为两个较小矩阵的乘积。

![](img/5221bbcd2d03d7997c44e075a2b36960.png)

右侧的两个向量相乘等于左侧的矩阵。尽管它们的值相同，但左侧的向量占据了右侧矩阵的 40% 的大小。矩阵越大，因子就越能节省空间。

如果你有一个大矩阵，且具有显著的线性依赖（因此秩较低），你可以将该矩阵表示为两个相对较小矩阵的因子。这种因式分解的想法使得 LoRA 能够占用如此小的内存空间。

## LoRA 背后的核心思想

LoRA 认为调整不是改变参数，而是学习参数的变化。使用 LoRA 时，我们不会直接学习参数变化；相反，我们学习参数变化矩阵的因子。

![](img/9c1c8d573808ae5aeb587611e83083cd.png)

LoRA 的示意图，来自于 [LoRA 论文](https://arxiv.org/pdf/2106.09685.pdf)。矩阵 A 和 B 经过训练以找到对预训练权重的最佳调整。我们将在未来的章节中讨论“r”。

学习变化矩阵因子的这一想法依赖于核心假设，即大型语言模型中的权重矩阵具有大量的线性依赖，因为参数数量远远超出理论所需的数量。过度参数化已被证明在预训练中是有益的（这就是现代机器学习模型如此庞大的原因）。LoRA 的理念是，一旦你通过预训练学习了通用任务，你可以用显著更少的信息进行微调。

> 过度参数化的模型实际上位于较低的内在维度。我们假设模型适应过程中的权重变化也具有低的“内在秩”，这导致我们提出了低秩适应（LoRA）方法。LoRA 允许我们通过优化密集层变化的秩分解矩阵来间接训练神经网络中的一些密集层，同时保持预训练权重冻结 — [LoRA 论文](https://arxiv.org/pdf/2106.09685.pdf)

这导致训练的参数数量显著减少，从而使微调过程整体上更快且在存储和内存上更高效。

# LoRA 的微调流程

现在我们了解了 LoRA 的各个部分如何工作，让我们将它们结合起来。

首先，我们冻结模型参数。我们将使用这些参数进行推理，但不会更新它们。

![](img/805a248157862c4a47b8eb05be022483.png)

我们创建两个矩阵。这些矩阵的大小设置为，当它们相乘时，它们将与我们微调的模型的权重矩阵大小相同。在大型模型中，具有多个权重矩阵时，你将为每个权重矩阵创建一对这些矩阵。

![](img/360179d813e340fbe00f3c7a84d80582.png)

LoRA 论文将这些矩阵称为矩阵“A”和“B”。这两个矩阵代表了 LoRA 微调过程中的可学习参数。

我们计算变化矩阵

![](img/d33e12aa69588e985f2589cbaa8e00d1.png)

然后我们将输入通过冻结的权重和变化矩阵。

![](img/7cf1aea3b66b42543777fd4945aa481a.png)

我们根据两个输出的组合计算损失，然后根据损失更新矩阵 A 和 B

![](img/f5fa3f569c4a109c504078a33dd5e305.png)

注意，虽然这里显示了变化矩阵以便于说明，但实际上它是即时计算的，从未存储，这就是为什么 LoRA 具有如此小的内存占用。实际上，仅在训练期间存储模型参数、矩阵 A 和 B，以及 A 和 B 的梯度。

我们执行这个操作，直到我们优化了用于微调任务的变化矩阵的因子。由于 A 和 B 显著较小，因此更新矩阵 A 和 B 的反向传播步骤比更新整个模型参数集的过程要快。这就是为什么尽管训练过程中的操作更多，LoRA 仍然通常比传统微调更快的原因。

当我们最终想要使用这个微调模型进行推理时，我们可以简单地计算变化矩阵，并将这些变化添加到权重中。这意味着 LoRA 不会改变模型的推理时间。

![](img/1ba5cbdd78ac0b0fd793703503f74f80.png)

一个有趣的小提示，我们甚至可以将变化矩阵乘以一个缩放因子，从而控制变化矩阵对模型的影响程度。理论上，我们可以同时使用一点这种 LoRA 和一点那种 LoRA，这种方法在图像生成中很常见。

# 关于变换器的 LoRA 的说明

在研究这篇文章时，我发现了一个许多人没有讨论的概念性脱节。把机器学习模型当作一个大权重箱子来处理是可以的，但实际上许多模型具有复杂的结构，这种结构并不是非常“箱子型”的。我不太明白这个变化矩阵的概念如何确切地应用于像变换器这样的参数。

![](img/3356829911ae5e55d3884270aca500b5.png)

变换器的图示，我在[另一篇文章](https://medium.com/towards-data-science/transformers-intuitively-and-exhaustively-explained-58a5c5df8dbb)中进行了讲解。符号“Nx”表示左侧和右侧的重复次数。这不是一个干净的权重方阵，因此不明显如何应用 LoRA。图像[来源](https://arxiv.org/pdf/1706.03762.pdf)

根据我目前的理解，针对变换器，需牢记两点：

1.  通常，变换器的多头自注意层（构建查询、键和值的层）中的密集网络只有一层深度。也就是说，只有一个输入层和一个通过权重连接的输出层。

1.  这些浅层密集网络构成了变换器中大部分可学习参数，非常非常庞大。可能有超过 100,000 个输入神经元连接到 100,000 个输出神经元，这意味着描述这些网络的单个权重矩阵可能有 10B 个参数。所以，尽管这些网络可能只有一层深度，但它们却极其宽广，因此描述它们的权重矩阵也非常庞大。

从 LoRA 在变换器模型中的角度来看，这些是主要的优化参数；你在学习这些在模型内部存在的巨大但浅层的密集层的因子化变化。正如之前讨论的，每个浅层密集层都有可以表示为矩阵的权重。

# 关于 LoRA 秩的说明

LoRA 有一个名为`r`的超参数，它描述了用于构建前面讨论的变化矩阵的`A`和`B`矩阵的深度。较高的`r`值意味着`A`和`B`矩阵更大，这意味着它们可以在变化矩阵中编码更多线性独立的信息。

![](img/9c1c8d573808ae5aeb587611e83083cd.png)

LoRA 的图示，来自[LoRA 论文](https://arxiv.org/pdf/2106.09685.pdf)。参数“r”可以被视为一个“信息瓶颈”。较低的 r 值意味着 A 和 B 可以用较小的内存占用编码较少的信息。较大的 r 值意味着 A 和 B 可以编码更多的信息，但需要较大的内存占用。

![](img/02ca9251cdea02f6b2418273158f8181.png)

一个 r 值等于 1 和 2 的 LoRA 概念图。在这两个示例中，分解后的 A 和 B 矩阵会导致相同大小的变化矩阵，但 r=2 能够将更多线性独立的信息编码到变化矩阵中，因为 A 和 B 矩阵中包含更多信息。

事实证明，LoRA 论文中提出的核心假设，即模型参数的变化具有较低的隐含秩，是一个相当强的假设。微软的团队（LoRA 的出版者）尝试了一些 `r` 值，发现即使 `A` 和 `B` 矩阵的秩为 1，也表现得相当好。

![](img/b84ee54032aada28f9506edb24441c28.png)

来源于 [LoRA 论文](https://arxiv.org/pdf/2106.09685.pdf)

通常，在选择 `r` 时，我听到的建议是：当数据与预训练中使用的数据类似时，低 `r` 值可能足够。当在非常新的任务上进行微调，这可能需要模型内部的 substantial logical changes 时，可能需要高 `r` 值。

# Python 中的 LoRA

考虑到我们讨论了很多理论，你可能期望一个相当长的教程，但我有好消息！HuggingFace 有一个模块可以让 LoRA 变得非常简单。

在这个示例中，我们将对一个预训练的模型进行问答微调。让我们直接开始。完整代码可以在这里找到：

[](https://github.com/DanielWarfield1/MLWritingAndResearch/blob/main/LoRA.ipynb?source=post_page-----e944a6bff46b--------------------------------) [## MLWritingAndResearch/LoRA.ipynb 在主分支 · DanielWarfield1/MLWritingAndResearch]

### 机器学习写作和研究中使用的笔记本示例 - MLWritingAndResearch/LoRA.ipynb 在主分支 ·…

github.com](https://github.com/DanielWarfield1/MLWritingAndResearch/blob/main/LoRA.ipynb?source=post_page-----e944a6bff46b--------------------------------)

## 1) 下载依赖项

我们将使用一些超出简单 PyTorch 项目的模块。这些模块的作用如下：

+   **bitsandbytes：** 用于使用较小的数据类型表示模型，从而节省内存。

+   **datasets：** 用于下载数据集

+   **accelerate：** 一些模块所需的机器学习互操作性依赖项。

+   **loralib：** LoRA 实现

+   **peft：** 一般的“参数高效微调”模块，我们的 LoRA 接口。

+   **transformers：** 用于从 huggingface 下载和使用预训练的 transformers。

```py
!pip install -q bitsandbytes datasets accelerate loralib
!pip install -q git+https://github.com/huggingface/peft.git git+https://github.com/huggingface/transformers.git
```

## 2) 加载预训练模型

我们将使用 BLOOM，这是一个开源且许可宽松的语言模型。我们将使用 5.6 亿参数版本以节省内存，但你也可以将此策略应用于 BLOOM 的更大版本。

```py
"""Importing dependencies and downloading pre-trained bloom model
"""

import torch
import torch.nn as nn
import bitsandbytes as bnb
from transformers import AutoTokenizer, AutoConfig, AutoModelForCausalLM

#loading model
model = AutoModelForCausalLM.from_pretrained(
    # "bigscience/bloom-3b",
    # "bigscience/bloom-1b1",
    "bigscience/bloom-560m",
    torch_dtype=torch.float16,
    device_map='auto',
)

#loading tokenizer for this model (which turns text into an input for the model)
tokenizer = AutoTokenizer.from_pretrained("bigscience/tokenizer")
```

## 3) 设置 LoRA

使用以下参数配置 LoRA：

+   **r：** A 和 B 矩阵的秩

+   **lora_alpha：** 这是一个相当有争议的参数。很多人对它有很多想法。你可以将它视为一个缩放因子，根据我的理解，它的默认值应该等于 `r`。

+   **目标模块（target_modules）：** 我们希望用 LoRA 优化的模型部分。BLOOM 模块有名为 `query_key_value` 的参数，我们希望优化这些参数。

+   **lora_dropout：** dropout 是一种技术，通过隐藏输入来抑制模型的过拟合（称为正则化）。这是一个被隐藏的概率。

+   **偏置（bias）：** 神经网络通常每个连接有两个参数，一个是“权重”（weight），另一个是“偏置”（bias）。在这个示例中，我们只训练权重。

+   **任务类型（task_type）：** 并不是非常必要，主要用于超类 `PeftConfig` 中。设置为 `CAUSAL_LM` 是因为我们使用的特定语言模型是“因果型”。

```py
"""Setting up LoRA using parameter efficient fine tuning
"""

from peft import LoraConfig, get_peft_model

#defining how LoRA will work in this particular example
config = LoraConfig(
    r=8,
    lora_alpha=8,
    target_modules=["query_key_value"],
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM"
)

#this actually overwrites the model in memory, so
#the rename is only for ledgibility.
peft_model = get_peft_model(model, config)
```

## 4) 检查内存节省

LoRA 的一个重要概念是训练包含的训练参数显著减少，这意味着在内存消耗方面有很大的节省。让我们看看在这个特定示例中我们节省了多少。

```py
"""Comparing parameters before and after LoRA
"""

trainable_params = 0
all_param = 0

#iterating over all parameters
for _, param in peft_model.named_parameters():
    #adding parameters to total
    all_param += param.numel()
    #adding parameters to trainable if they require a graident
    if param.requires_grad:
        trainable_params += param.numel()

#printing results
print(f"trainable params: {trainable_params}")
print(f"all params: {all_param}")
print(f"trainable: {100 * trainable_params / all_param:.2f}%")
```

![](img/f77890c5ac04c09519ce9e0e51c95bdc.png)

比较 LoRA 中可训练参数与原始模型参数的结果。在这个示例中，我们只训练了不到千分之一的数据。

## 5) 加载微调数据集

我们将使用 SQUAD 数据集来提高我们语言模型在问答任务中的表现。斯坦福问答数据集（SQUAD）是一个高质量、常用且许可宽松的数据集。

```py
"""Loading SQUAD dataset
"""

from datasets import load_dataset
qa_dataset = load_dataset("squad_v2")
```

## 6) 重新结构化数据

我们将对特定结构的数据进行语言模型的微调。模型将期望文本呈现这种一般形式：

```py
**CONTEXT:**
{context}

**QUESTION:**
{question}

**ANSWER:**
{answer}</s>
```

我们将向模型提供上下文和问题，模型将被期望为我们提供答案。因此，我们将重新格式化 SQUAD 中的数据以符合这种格式。

```py
"""Reformatting SQUAD to respect our defined structure
"""

#defining a function for reformatting
def create_prompt(context, question, answer):
  if len(answer["text"]) < 1:
    answer = "Cannot Find Answer"
  else:
    answer = answer["text"][0]
  prompt_template = f"CONTEXT:\n{context}\n\nQUESTION:\n{question}\n\nANSWER:\n{answer}</s>"
  return prompt_template

#applying the reformatting function to the entire dataset
mapped_qa_dataset = qa_dataset.map(lambda samples: tokenizer(create_prompt(samples['context'], samples['question'], samples['answers'])))
```

## 7) 使用 LoRA 在 SQUAD 上进行微调

这段代码大多被借用。在缺乏严格验证程序的情况下，最佳实践是直接复制成功的教程，或者更好的是，直接从文档中获取。如果你要为实际用例训练一个实际模型，你可能需要研究并优化这些参数。

```py
"""Fine Tuning
This code is largly co-opted. In the absence of a rigid validation
procedure, the best practice is to just copy a successful tutorial or,
better yet, directly from the documentation.
"""

import transformers

trainer = transformers.Trainer(
    model=peft_model,
    train_dataset=mapped_qa_dataset["train"],
    args=transformers.TrainingArguments(
        per_device_train_batch_size=4,
        gradient_accumulation_steps=4,
        warmup_steps=100,
        max_steps=100,
        learning_rate=1e-3,
        fp16=True,
        logging_steps=1,
        output_dir='outputs',
    ),
    data_collator=transformers.DataCollatorForLanguageModeling(tokenizer, mlm=False)
)
peft_model.config.use_cache = False  # silence the warnings. Please re-enable for inference!
trainer.train()
```

![](img/9438d812f70c7c66b9b131c20d87a0b7.png)

损失（模型中的错误量）。在这个示例中，我们不需要非常仔细地查看损失，但它作为一个良好的度量指标。在这个示例中，我们训练了 100 步，虽然损失在步骤之间有一些随机变化，但损失通常在训练过程中下降，这是好的。

## 8) 检查 LoRA 大小

让我们保存我们的 LoRA 优化成果。

```py
"""Saving the LoRA fine tuning locally
"""
model_id = "BLOOM-560m-LoRA"
peft_model.save_pretrained(model_id)
```

然后检查文件系统中文件的大小。

```py
!ls -lh {model_id}
```

![](img/670a2dc01836e53a9128f0062a7c8b3c.png)

BLOOM 560m 模型，在其 float 16 数据类型下，总大小超过 1 GB。使用 LoRA，我们只需要保存分解后的矩阵，我们的检查点大小仅为 3 MB。这就像把整个游戏“植物大战僵尸”压缩成一张 iPhone 拍摄的图片一样。

## 9) 测试

好的，我们有一个 LoRA 精细调整后的模型，让我们问它几个问题。首先，我们定义一个辅助函数，该函数将接收上下文和问题，运行预测，并生成响应。

```py
"""Helper Function for Comparing Results
"""

from IPython.display import display, Markdown

def make_inference(context, question):

    #turn the input into tokens
    batch = tokenizer(f"**CONTEXT:**\n{context}\n\n**QUESTION:**\n{question}\n\n**ANSWER:**\n", return_tensors='pt', return_token_type_ids=False)
    #move the tokens onto the GPU, for inference
    batch = batch.to(device='cuda')

    #make an inference with both the fine tuned model and the raw model
    with torch.cuda.amp.autocast():
        #I think inference time would be faster if these were applied,
        #but the fact that LoRA is not applied allows me to experiment
        #with before and after fine tuning simultaniously

        #raw model
        peft_model.disable_adapter_layers()
        output_tokens_raw = model.generate(**batch, max_new_tokens=200)

        #LoRA model
        peft_model.enable_adapter_layers()
        output_tokens_qa = peft_model.generate(**batch, max_new_tokens=200)

    #display results
    display(Markdown("# Raw Model\n"))
    display(Markdown((tokenizer.decode(output_tokens_raw[0], skip_special_tokens=True))))
    display(Markdown("\n# QA Model\n"))
    display(Markdown((tokenizer.decode(output_tokens_qa[0], skip_special_tokens=True))))
```

让我们看看一些示例，了解我们精细调整后的模型在回答问题方面表现如何：

**示例 1)**

```py
context = "You are a monster, and you eat yellow legos."
question = "What is the best food?"

make_inference(context, question)
```

![](img/88f897196d79cc9be45b43f42a722cb2.png)

**示例 2)**

```py
context = "you are a math wizard"
question = "what is 1+1 equal to?"

make_inference(context, question)
```

![](img/a6c199e9674737d64bc786ff66865eb6.png)

我们仅使用了一个 560M 参数的模型，因此它在基本推理方面表现不佳也不奇怪。问它 1+1 是什么可能有些勉强，但至少它失败得更优雅。

**示例 3)**

```py
context = "Answer the riddle"
question = "What gets bigger the more you take away?"

make_inference(context, question)
```

![](img/f1cbcfc3c285bc7cab772fbaa8e81ed4.png)

再次强调，我们仅使用了一个 560M 参数的模型。尽管如此，经过精细调整的模型在回答问题时显得更加优雅。

# 结论

就这些！我们讨论了精细调整的概念，以及 LoRA 如何将精细调整视为学习参数的变化，而不是迭代地学习新参数。我们了解了线性独立性和秩，以及由于权重矩阵的低秩，变化矩阵如何通过小因子来表示。我们将这些知识结合在一起，逐步了解了 LoRA，然后使用 HuggingFace PEFT 模块在问答任务中实现了 LoRA。

# 关注以获取更多更新！

我描述了机器学习领域的论文和概念，重点是实用和直观的解释。

[](https://medium.com/@danielwarfield1/subscribe?source=post_page-----e944a6bff46b--------------------------------) [## 每当 Daniel Warfield 发布新内容时获取邮件提醒

### 高质量的数据科学文章直达你的邮箱。每当 Daniel Warfield 发布新内容时获取邮件提醒。通过注册，你可以…

medium.com](https://medium.com/@danielwarfield1/subscribe?source=post_page-----e944a6bff46b--------------------------------) ![](img/1f6f4c8a07d69cf53e055e0130a85b03.png)

从未预料到，总是受到赞赏。通过捐赠，你可以让我分配更多的时间和资源来制作更频繁、更高质量的文章。 [链接](https://www.buymeacoffee.com/danielwarfield)

**归属：** 除非另有来源提供，否则本文档中的所有资源均由 Daniel Warfield 创建。你可以将此帖子中的任何资源用于非商业目的，只要你引用本文或 [`danielwarfield.dev`](https://danielwarfield.dev/)，或者两者均引用。可根据请求提供明确的商业许可。
