# 大型语言模型：DistilBERT——更小、更快、更便宜、更轻便

> 原文：[https://towardsdatascience.com/distilbert-11c8810d29fc?source=collection_archive---------2-----------------------#2023-10-07](https://towardsdatascience.com/distilbert-11c8810d29fc?source=collection_archive---------2-----------------------#2023-10-07)

## 解锁 BERT 压缩的秘密：一个用于最大效率的学生-教师框架

[](https://medium.com/@slavahead?source=post_page-----11c8810d29fc--------------------------------)[![Vyacheslav Efimov](../Images/db4b02e75d257063e8e9d3f1f75d9d6d.png)](https://medium.com/@slavahead?source=post_page-----11c8810d29fc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----11c8810d29fc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----11c8810d29fc--------------------------------) [Vyacheslav Efimov](https://medium.com/@slavahead?source=post_page-----11c8810d29fc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc8a0ca9d85d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdistilbert-11c8810d29fc&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=post_page-c8a0ca9d85d8----11c8810d29fc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----11c8810d29fc--------------------------------) ·7 min read·2023年10月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F11c8810d29fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdistilbert-11c8810d29fc&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=-----11c8810d29fc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F11c8810d29fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdistilbert-11c8810d29fc&source=-----11c8810d29fc---------------------bookmark_footer-----------)![](../Images/96278a8b1b50077af63e7602a118a6e1.png)

# 介绍

近年来，大型语言模型的发展迅速。BERT 成为最受欢迎和高效的模型之一，能够以高精度解决各种自然语言处理任务。在 BERT 之后，出现了一系列其他模型，这些模型也展示了卓越的结果。

显而易见的趋势是**随着时间的推移，大型语言模型（LLMs）往往变得更加复杂，通过指数级增加参数和数据的数量**。深度学习研究表明，这些技术通常会带来更好的结果。不幸的是，机器学习领域已经遇到了一些关于 LLMs 的问题，可扩展性已成为有效训练、存储和使用它们的主要障碍。

针对这个问题，已经制定了专门的技术来压缩 LLMs。压缩算法的目标是减少训练时间、降低内存消耗或加快模型推断。实际中使用的三种最常见的压缩技术如下：

+   **知识蒸馏**涉及训练一个较小的模型，试图表示较大模型的行为。

+   **量化**是减少存储表示模型权重的数字所需内存的过程。

+   **剪枝**指的是丢弃最不重要的模型权重。

在本文中，我们将理解应用于 BERT 的蒸馏机制，这导致了一个新模型叫做**DistilBERT**。顺便提一下，下面讨论的技术也可以应用于其他 NLP 模型。

# 蒸馏基础

蒸馏的目标是创建一个较小的模型，可以模仿较大的模型。在实践中，这意味着如果一个大型模型预测某些内容，则期望较小的模型做出类似的预测。

为了实现这一点，需要一个已经预训练的大型模型（在我们的例子中是 BERT）。然后，需要选择一个较小模型的架构。为了增加成功模仿的可能性，通常建议较小模型的架构与大型模型相似，但参数数量减少。最后，较小模型从大型模型在某一数据集上做出的预测中学习。为了达到这一目标，选择一个适当的损失函数对于帮助较小模型更好地学习至关重要。

> 在蒸馏术语中，大型模型称为**教师**，较小模型称为**学生**。
> 
> 一般来说，蒸馏过程应用于预训练阶段，但也可以在微调阶段应用。

# DistilBERT

DistilBERT 从 BERT 学习，并通过使用包含三个组件的损失函数来更新其权重：

+   掩码语言建模（MLM）损失

+   蒸馏损失

+   相似性损失

接下来，我们将讨论这些损失组件，并了解每个组件的必要性。不过，在深入之前，有必要了解一个重要概念，即**Softmax 激活函数中的温度**。温度概念在 DistilBERT 损失函数中使用。

# Softmax 温度

通常可以观察到 softmax 转换作为神经网络的最后一层。Softmax 规范化了所有模型输出，使其总和为 1，可以解释为概率。

存在一个 softmax 公式，其中模型的所有输出都除以一个 **温度** 参数 T：

![](../Images/0ef39138409a1d7dc7d5b9c54d277c03.png)

Softmax 温度公式。pᵢ 和 zᵢ 分别是模型输出和第 i 个对象的规范化概率。T 是温度参数。

温度 T 控制输出分布的平滑度：

+   如果 T > 1，那么分布变得更平滑。

+   如果 T = 1，则分布与应用正常 softmax 时相同。

+   如果 T < 1，那么分布变得更粗糙。

为了明确起见，我们来看一个例子。考虑一个具有 5 个标签的分类任务，其中神经网络生成了 5 个值，表示输入对象属于相应类别的信心。应用不同 T 值的 softmax 会产生不同的输出分布。

![](../Images/d9f8c82bfca718e35bcc0ccb3b49fa80.png)

一个神经网络根据温度 T 产生不同概率分布的示例

> 温度越高，概率分布越平滑。

![](../Images/2e15f1e6f63d8e615e2cd873a3e2118b.png)

基于不同温度 T 值的 logit（自然数从 1 到 5）的 softmax 转换。随着温度的升高，softmax 值变得更加趋同。

# 损失函数

## 掩码语言建模损失

类似于教师模型（BERT），在预训练期间，学生（DistilBERT）通过对掩码语言建模任务进行预测来学习语言。在对某个标记生成预测后，预测的概率分布与教师模型的一热编码概率分布进行比较。

> 一热编码分布表示一个概率分布，其中最可能的标记的概率设置为 1，其它所有标记的概率设置为 0。

如同大多数语言模型一样，交叉熵损失是在预测分布和真实分布之间计算的，学生模型的权重通过反向传播进行更新。

![](../Images/5fea9adb3ce64f303d23caf2e3157829.png)

掩码语言建模损失计算示例

## 蒸馏损失

实际上，可以仅使用学生损失来训练学生模型。然而，在许多情况下，这可能不够。仅使用学生损失的常见问题在于其 softmax 转换，其中温度 T 设置为 1。在实际应用中，T = 1 的结果分布会变成一种形式，其中一个可能的标签具有非常接近 1 的高概率，而所有其他标签的概率则很低，接近 0。

这种情况与对特定输入有效的两个或多个分类标签不太一致：当T = 1的softmax层将很可能排除所有有效标签，除了一种，并将概率分布接近于one-hot编码分布。这会导致可能被学生模型学习的有用信息丢失，从而使其多样性降低。

这就是为什么论文的作者引入了**蒸馏损失**，其中softmax概率使用温度T > 1进行计算，使得平滑对齐概率成为可能，从而考虑到学生的多个可能答案。

> 在蒸馏损失中，相同的温度T应用于学生和教师。移除了教师分布的one-hot编码。

![](../Images/fd5275d5b6ccd07dc4dcf4558ac3ffb4.png)

蒸馏损失计算示例

> 可以使用KL散度损失代替交叉熵损失。

## 相似度损失

研究人员还指出，在隐藏状态嵌入之间添加余弦相似度损失是有益的。

![](../Images/533d35280d9b4f50832dcfdbea177731.png)

余弦损失公式

这样，学生不仅有可能正确重建被掩盖的标记，还能构建与教师相似的嵌入。这也为在两个模型空间中保持嵌入之间的相同关系打开了大门。

![](../Images/68e9a28153ec8abac446648fb86690f2.png)

相似度损失计算示例

## 三重损失

最后，计算所有三个损失函数的线性组合和，这定义了DistilBERT中的损失函数。根据损失值，对学生模型进行反向传播以更新其权重。

![](../Images/21322eac7070863f1c9e7d0a857d026a.png)

DistilBERT损失函数

> 有趣的是，在三种损失组件中，掩盖语言建模损失对模型性能的影响最小。蒸馏损失和相似度损失的影响要大得多。

# 推断

DistilBERT中的推断过程与训练阶段完全相同。唯一的细微之处是softmax温度T设置为1。这是为了获得接近BERT计算的概率。

# 架构

总体而言，DistilBERT使用与BERT相同的架构，只是进行了以下更改：

+   DistilBERT仅有BERT层的一半。模型中的每一层都通过从两个BERT层中选择一个来初始化。

+   移除了标记类型嵌入。

+   应用于[CLS]标记的隐藏状态的分类任务的密集层被移除。

+   为了更强的性能，作者使用了RoBERTa中提出的最佳方法：

    - 使用动态掩蔽

    - 移除下一个句子预测目标

    - 在更大的批次上训练

    - 应用梯度累积技术以优化梯度计算

> DistilBERT 的最后一层隐藏层大小（768）与 BERT 相同。作者报告称，其减少不会在计算效率方面带来显著改进。他们认为，减少总层数有更高的影响。

# 数据

DistilBERT 在与 BERT 相同的数据语料上训练，这些数据包含 BooksCorpus（8 亿字）和英语维基百科（25 亿字）。

# BERT 与 DistilBERT 的比较

BERT 和 DistilBERT 的关键性能参数在几个最流行的基准测试上进行了比较。以下是需要记住的重要事实：

+   在推理过程中，DistilBERT 比 BERT 快 60%。

+   DistilBERT 的参数比 BERT 少 4400 万，总体比 BERT 小 40%。

+   DistilBERT 保留了 BERT 97% 的性能。

![](../Images/8a13681d73d760daa68e782d9a1c69e4.png)

BERT 与 DistilBERT 的比较（在 GLUE 数据集上）

# 结论

DistilBERT 在 BERT 的进化中迈出了巨大的一步，它通过显著压缩模型的体积，同时在各种 NLP 任务上实现了相当的性能。除此之外，DistilBERT 的体积仅为 207 MB，使得在容量有限的设备上集成更加容易。知识蒸馏并不是唯一可应用的技术：DistilBERT 可以通过量化或剪枝算法进一步压缩。

# 资源

+   [DistilBERT，BERT 的蒸馏版本：更小、更快、更便宜、更轻](https://arxiv.org/pdf/1910.01108.pdf)

*除非另有说明，所有图像均由作者提供*
