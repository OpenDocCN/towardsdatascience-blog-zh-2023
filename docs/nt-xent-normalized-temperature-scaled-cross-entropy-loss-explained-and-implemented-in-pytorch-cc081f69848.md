# NT-Xent（归一化温度调节交叉熵）损失函数的解释及在 PyTorch 中的实现

> 原文：[https://towardsdatascience.com/nt-xent-normalized-temperature-scaled-cross-entropy-loss-explained-and-implemented-in-pytorch-cc081f69848?source=collection_archive---------1-----------------------#2023-06-13](https://towardsdatascience.com/nt-xent-normalized-temperature-scaled-cross-entropy-loss-explained-and-implemented-in-pytorch-cc081f69848?source=collection_archive---------1-----------------------#2023-06-13)

## 一种直观解释 NT-Xent 损失函数的方法，详细解释其操作，并在 PyTorch 中进行了实现

[](https://medium.com/@dhruvbird?source=post_page-----cc081f69848--------------------------------)[![Dhruv Matani](../Images/d63bf7776c28a29c02b985b1f64abdd3.png)](https://medium.com/@dhruvbird?source=post_page-----cc081f69848--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cc081f69848--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cc081f69848--------------------------------) [Dhruv Matani](https://medium.com/@dhruvbird?source=post_page-----cc081f69848--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F63f5d5495279&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnt-xent-normalized-temperature-scaled-cross-entropy-loss-explained-and-implemented-in-pytorch-cc081f69848&user=Dhruv+Matani&userId=63f5d5495279&source=post_page-63f5d5495279----cc081f69848---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----cc081f69848--------------------------------) ·14 min read·Jun 13, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcc081f69848&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnt-xent-normalized-temperature-scaled-cross-entropy-loss-explained-and-implemented-in-pytorch-cc081f69848&user=Dhruv+Matani&userId=63f5d5495279&source=-----cc081f69848---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcc081f69848&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnt-xent-normalized-temperature-scaled-cross-entropy-loss-explained-and-implemented-in-pytorch-cc081f69848&source=-----cc081f69848---------------------bookmark_footer-----------)

与[Naresh Singh](https://medium.com/u/1e659a80cffd)合作撰写。

![](../Images/f1a271acfecfd43c4d250941c7754440.png)

NT-Xent 损失函数公式。来源：[Papers with code](https://paperswithcode.com/method/nt-xent) (CC-BY-SA)

# 介绍

最近在 [自监督学习](https://neptune.ai/blog/self-supervised-learning) 和 [对比学习](/understanding-contrastive-learning-d5b19fd96607) 方面的进展激发了机器学习（ML）领域的研究人员和从业者重新关注这一领域。

尤其是，[SimCLR](https://arxiv.org/pdf/2002.05709.pdf) 论文提出了一个简单的对比学习视觉表示框架，在自监督和对比学习领域获得了大量关注。

论文的核心思想非常简单——允许模型学习一对图像是否来自相同或不同的初始图像。

![](../Images/04e9c2852b2200c323cdbabca0b3e988.png)

图1：SimCLR的高层次思路。来源：[SimCLR 论文](https://arxiv.org/pdf/2002.05709.pdf)

SimCLR 方法将每个输入图像 ***i*** 编码为特征向量 ***zi***。需要考虑两种情况：

1.  **正对**：相同图像使用不同的增强集合进行增强，结果特征向量 ***zi*** 和 ***zj*** 进行比较。这些特征向量通过损失函数被强制保持相似。

1.  **负对**：不同的图像使用不同的增强集合进行增强，结果特征向量 ***zi*** 和 ***zk*** 进行比较。这些特征向量通过损失函数被强制保持不相似。

本文其余部分将集中于解释和理解该损失函数及其使用PyTorch的高效实现。

# NT-Xent损失

从高层次看，对比学习模型接收2N张图像，来源于N个基础图像。每个N个基础图像都使用随机的图像增强集合进行增强，生成2张增强图像。这就是我们在单个训练批次中获得2N张图像的方式。

![](../Images/0e24735fcc69c1116d2734e8f11df06e.png)

图2：对比学习中的单个训练批次中的6张图像。每张图像下方的数字是该图像在输入批次中的索引，输入到对比学习模型中。图像来源：[牛津视觉几何组](https://www.robots.ox.ac.uk/~vgg/data/pets/)（CC-SA）。

在接下来的章节中，我们将深入探讨NT-Xent损失的以下方面。

1.  [温度对SoftMax和Sigmoid的影响](#8cb8)

1.  [NT-Xent损失的简单直观解释](#40d2)

1.  [PyTorch中NT-Xent的逐步实现](#29d4)

1.  [激发多标签损失函数需求（NT-BXent）](#f004)

1.  [PyTorch中NT-BXent的逐步实现](#242d)

步骤2-5的所有代码可以在 [这个笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/nt-xent-loss/NT-Xent%20Loss.ipynb) 中找到。步骤1的代码可以在 [这个笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/nt-xent-loss/SoftMax%20and%20Sigmoid%20with%20temperature.ipynb) 中找到。

## 温度对SoftMax和Sigmoid的影响

为了理解本文中要研究的对比损失函数的所有活动部分，我们需要首先了解温度对SoftMax和Sigmoid激活函数的影响。

通常，温度缩放应用于SoftMax或Sigmoid的输入，以平滑或突出这些激活函数的输出。在传递到激活函数之前，输入logits被温度除以。你可以在[这个笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/nt-xent-loss/SoftMax%20and%20Sigmoid%20with%20temperature.ipynb)中找到所有相关代码。

**SoftMax**：对于SoftMax，高温度会降低输出分布的方差，从而使标签变得更加柔和。低温度则会增加输出分布的方差，使最大值相对于其他值更加突出。请参见下面的图表，了解输入张量[0.1081, 0.4376, 0.7697, 0.1929, 0.3626, 2.8451]的温度对SoftMax的影响。

![](../Images/2bd5edcd98118bea0c576c1e7b3ada7f.png)

图3：温度对SoftMax的影响。来源：作者

**Sigmoid**：对于Sigmoid，高温度会导致输出分布向0.0拉伸，而低温度则将输入扩展到更高的值，使输出更接近0.0或1.0，具体取决于输入的未签名幅度。

![](../Images/0d7036a7fb298c646e1652f03d546b79.png)

图4：温度对Sigmoid的影响。来源：作者

现在我们理解了不同温度值对SoftMax和Sigmoid函数的影响，让我们看看这些知识如何应用于理解NT-Xent损失。

## 解读NT-Xent损失

NT-Xent损失通过理解损失名称中的各个术语来进行理解。

1.  标准化：余弦相似度产生范围在[-1.0到+1.0]之间的标准化分数

1.  温度缩放：所有对的余弦相似度在计算交叉熵损失之前被温度缩放

1.  交叉熵损失：底层损失是一个多类别（单标签）交叉熵损失

如上所述，我们假设对于大小为2N的批次，以下索引处的特征向量代表正对（0, 1）、（2, 3）、（4, 5）、（6, 7）等，其余组合代表负对。在解释NT-Xent损失时，这一点是与SimCLR相关的重要因素。

现在我们了解了NT-Xent损失的术语在上下文中的含义，让我们来看看计算特征向量批次上NT-Xent损失所需的机械步骤。

1.  所有对的余弦相似度分数是针对SimCLR模型生成的每个2N向量计算的。这导致了(2N)²的相似度分数，表示为一个2N x 2N矩阵

1.  相同值 (i, i) 之间的比较结果会被丢弃（因为一个分布与自身完全相似，不能让模型学到任何有用的东西）

1.  每个值（余弦相似度）都由温度参数 𝜏（这是一个超参数）进行缩放

1.  交叉熵损失应用于上述结果矩阵的每一行。以下段落将详细解释

1.  通常，这些损失的均值（每批次一个损失）用于反向传播

这里使用交叉熵损失的方式在语义上与标准分类任务中的使用方式略有不同。在分类任务中，训练一个最终的“分类头”来为每个输入产生一个独热概率向量，我们在这个独热概率向量上计算交叉熵损失，因为我们实际上是在计算两个分布之间的差异。[这个视频](https://www.youtube.com/watch?v=Pwgpl9mKars) 美丽地解释了交叉熵损失的概念。在 NT-Xent 损失中，训练层和输出分布之间没有一一对应的关系。相反，每个输入都计算一个特征向量，然后计算每对特征向量之间的余弦相似度。这里的诀窍是，由于每张图片与输入批次中的恰好 1 张其他图片相似（正样本对）（如果我们忽略特征向量与自身的相似度），我们可以将其视为一种类似分类的设置，其中图像之间相似度概率的概率分布表示了一个分类任务，其中一个值接近 1.0，其余值接近 0.0。

既然我们对 NT-Xent 损失有了充分的理解，我们应该能很好地将这些思想实现到 PyTorch 中。我们开始吧！

## NT-Xent 损失在 PyTorch 中的实现

本节中的所有代码可以在[这个笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/nt-xent-loss/NT-Xent%20Loss.ipynb)中找到。

**代码重用**：许多[NT-Xent 损失的实现](https://github.com/KevinMusgrave/pytorch-metric-learning/blob/master/src/pytorch_metric_learning/losses/ntxent_loss.py)从头开始实现所有操作。此外，其中一些实现损失函数的方式效率不高，更喜欢使用[for 循环而非 GPU 并行](https://stackoverflow.com/questions/62793043/tensorflow-implementation-of-nt-xent-contrastive-loss-function)。相反，我们将使用不同的方法。我们将通过 PyTorch 已经提供的标准交叉熵损失来实现这种损失。为此，我们需要将预测和真实标签转换为交叉熵可以接受的格式。下面我们来看一下如何实现。

**预测张量**：首先，我们需要创建一个 PyTorch 张量，它将表示我们对比学习模型的输出。假设我们的批量大小是 8（2N=8），并且我们的特征向量有 2 个维度（2 个值）。我们将输入变量称为 *“x”*。

```py
x = torch.randn(8, 2)
```

**余弦相似度**：接下来，我们将计算此批次中每个特征向量之间的所有对的余弦相似度，并将结果存储在名为 *“xcs”* 的变量中。如果下面的代码看起来令人困惑，请阅读[这个页面](https://medium.com/@dhruvbird/all-pairs-cosine-similarity-in-pytorch-867e722c8572)上的详细信息。这是“标准化”步骤。

```py
xcs = F.cosine_similarity(x[None,:,:], x[:,None,:], dim=-1)
```

如上所述，我们需要忽略每个特征向量的自相似度分数，因为它不对模型的学习做出贡献，并且在我们想要计算交叉熵损失时会成为不必要的麻烦。为此，我们将定义一个变量 *“eye”*，这是一个矩阵，其中主对角线上的元素值为 1.0，其余元素值为 0.0。我们可以使用以下命令创建这样的矩阵。

```py
eye = torch.eye(8)
```

现在让我们将其转换为布尔矩阵，以便可以使用这个掩码矩阵在 *“xcs”* 变量中进行索引。

```py
eye = eye.bool()
```

让我们将张量 *“xcs”* 克隆到一个名为 *“y”* 的张量中，以便以后可以引用“xcs”张量。

```py
y = xcs.clone()
```

现在，我们将所有对的余弦相似度矩阵的主对角线上的值设置为 *-inf*，这样当我们对每一行计算 softmax 时，这个值将不会产生任何贡献。

```py
y[eye] = float("-inf")
```

张量 *“y”* 通过温度参数缩放后，将成为 [PyTorch 中的交叉熵损失 API](https://pytorch.org/docs/stable/generated/torch.nn.functional.cross_entropy.html#torch.nn.functional.cross_entropy) 的输入之一。接下来，我们需要计算要传递给交叉熵损失 API 的真实标签（目标）。

**真实标签（目标张量）**：对于我们使用的示例（2N=8），这就是真实标签张量的样子。

> tensor([1, 0, 3, 2, 5, 4, 7, 6])

这是因为张量 *“y”* 中的以下索引对包含正对。

> (0, 1), (1, 0)
> 
> (2, 3), (3, 2)
> 
> (4, 5), (5, 4)
> 
> (6, 7), (7, 6)

要解释上述索引对，我们来看一个单一的例子。对 (4, 5) 来说，这意味着第 4 行第 5 列应该设置为 1.0（正对），这也是上述张量所表示的。太好了！

要创建上述张量，我们可以使用以下 PyTorch 代码，该代码将真实标签存储在变量 *“target”* 中。

```py
target = torch.arange(8)
target[0::2] += 1
target[1::2] -= 1
```

**交叉熵损失**：我们已经具备了计算损失所需的所有成分！剩下的唯一任务就是调用 PyTorch 中的 cross_entropy API。

```py
loss = F.cross_entropy(y / temperature, target, reduction="mean")
```

变量 “loss” 现在包含了计算出的 NT-Xent 损失。让我们把所有的代码封装到一个 Python 函数中。

```py
def nt_xent_loss(x, temperature):
  assert len(x.size()) == 2

  # Cosine similarity
  xcs = F.cosine_similarity(x[None,:,:], x[:,None,:], dim=-1)
  xcs[torch.eye(x.size(0)).bool()] = float("-inf")

  # Ground truth labels
  target = torch.arange(8)
  target[0::2] += 1
  target[1::2] -= 1

  # Standard cross-entropy loss
  return F.cross_entropy(xcs / temperature, target, reduction="mean")
```

上述代码有效，只要每个特征向量在训练对比学习模型时批次中恰好有一个正对。让我们来看一下如何在对比学习任务中处理多个正对。

## 用于对比学习的多标签损失：NT-BXent

在SimCLR论文中，每个图像***i***在索引***j***处有恰好1个相似对。这使得交叉熵损失成为任务的完美选择，因为它类似于多类别问题。相反，如果我们将M > 2个相同图像的增强输入到对比学习模型的单个训练批次中，那么每个批次将包含图像***i***的M-1个相似对。这将使任务类似于多标签问题。

显而易见的选择是将*交叉熵损失*替换为*二元交叉熵损失*。因此命名为NT-BXent损失，代表归一化温度缩放的二元交叉熵损失。

下面的公式展示了元素***i***的损失***Li***。公式中的σ表示[S型函数](https://en.wikipedia.org/wiki/Sigmoid_function)。

![](../Images/923d835482d9a31dafa43fe719a94d34.png)

图5：NT-BXent损失的公式。图像来源：本文作者

为了避免类别不平衡问题，我们通过我们小批量中正负对的数量的倒数来加权正负对。在用于反向传播的小批量中的最终损失将是小批量中每个样本损失的平均值。

接下来，让我们将注意力集中在我们在PyTorch中对NT-BXent损失的实现上。

## 在PyTorch中实现NT-BXent损失

本节中的所有代码可以在[这个笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/nt-xent-loss/NT-Xent%20Loss.ipynb)中找到。

**代码重用**：类似于我们对NT-Xent损失的实现，我们将重用PyTorch提供的二元交叉熵（BCE）损失方法。我们的真实标签设置将类似于使用BCE损失的多标签分类问题。

**预测张量**：我们将使用与NT-Xent损失实现中相同的(8, 2)预测张量。

```py
x = torch.randn(8, 2)
```

**余弦相似度**：由于输入张量***x***相同，所有对的余弦相似度张量***xcs***也将相同。有关下面这行代码的详细解释，请参见[这一页](https://medium.com/@dhruvbird/all-pairs-cosine-similarity-in-pytorch-867e722c8572)。

```py
xcs = F.cosine_similarity(x[None,:,:], x[:,None,:], dim=-1)
```

为了确保位置***(i, i)***处的损失为***0***，我们需要进行一些操作，使得在对***xcs***张量应用Sigmoid后，它在每个索引***(i, i)***处的值为***1***。由于我们将使用BCE损失，我们会将每个特征向量的自相似性分数标记为张量***xcs***中的值为***无穷大***。这是因为在***xcs***张量上应用Sigmoid函数将无穷大转换为值***1***，我们将设置我们的真实标签，使得真实标签中的每个位置***(i, i)***的值为***1***。

创建一个掩码张量，该张量在主对角线上具有值***True***（***xcs***在主对角线上具有自相似性分数），而其他地方为***False***。

```py
eye = torch.eye(8).bool()
```

将张量*“xcs”*克隆到一个名为*“y”*的张量中，以便我们可以稍后引用*“xcs”*张量。

```py
y = xcs.clone()
```

现在，我们将所有对的余弦相似度矩阵的主对角线上的值设置为*无穷大*，以便在对每一行计算Sigmoid时，这些位置的值为1。

```py
y[eye] = float("inf")
```

张量*“y”*由温度参数缩放后，将作为PyTorch中[BCE损失API](https://pytorch.org/docs/stable/generated/torch.nn.functional.binary_cross_entropy.html)的输入（预测）之一。接下来，我们需要计算要提供给BCE损失API的真实标签（目标）。

**真实标签（目标张量）**：我们期望用户传递给我们包含正例的所有（x, y）索引对。这与我们对NT-Xent损失所做的有所不同，因为正对是隐式的，而这里正对是显式的。

除了用户提供的位置外，我们还将所有对角线元素设置为正对，如上所述。我们将使用PyTorch张量索引API提取这些位置的所有元素并将其设置为1，而其他元素初始化为0。

```py
target = torch.zeros(8, 8)
pos_indices = torch.tensor([
  (0, 0), (0, 2), (0, 4),
  (1, 4), (1, 6), (1, 1),
  (2, 3),
  (3, 7),
  (4, 3),
  (7, 6),
])
# Add indexes of the principal diagonal as positive indexes.
# This will be useful since we will use the BCELoss in PyTorch,
# which will expect a value for the elements on the principal
# diagonal as well.
pos_indices = torch.cat([pos_indices, torch.arange(8).reshape(8, 1).expand(-1, 2)], dim=0)
# Set the values in the target vector to 1.
target[pos_indices[:,0], pos_indices[:,1]] = 1
```

**二元交叉熵（BCE）损失**：与NT-Xent损失不同，我们不能简单调用`torch.nn.functional.binary_cross_entropy_function`，因为我们需要根据当前小批量中索引i处的正负对数目来加权正负损失。

不过第一步是计算逐元素的BCE损失。

```py
temperature = 0.1
loss = F.binary_cross_entropy((y / temperature).sigmoid(), target, reduction="none")
```

我们将创建一个正负对的二进制掩码，然后创建两个张量，loss_pos和loss_neg，只包含计算损失中对应于正对和负对的元素。

```py
target_pos = target.bool()
target_neg = ~target_pos
# loss_pos and loss_neg below contain non-zero values only for those elements
# that are positive pairs and negative pairs respectively.
loss_pos = torch.zeros(x.size(0), x.size(0)).masked_scatter(target_pos, loss[target_pos])
loss_neg = torch.zeros(x.size(0), x.size(0)).masked_scatter(target_neg, loss[target_neg])
```

接下来，我们将分别对每个小批量中的元素i的正负对损失进行求和。

```py
# loss_pos and loss_neg now contain the sum of positive and negative pair losses
# as computed relative to the i'th input.
loss_pos = loss_pos.sum(dim=1)
loss_neg = loss_neg.sum(dim=1)
```

为了进行加权，我们需要跟踪每个小批量中每个元素i对应的正负对的数量。张量*“num_pos”*和*“num_neg”*将存储这些值。

```py
# num_pos and num_neg below contain the number of positive and negative pairs
# computed relative to the i'th input. In an actual setting, this number should
# be the same for every input element, but we let it vary here for maximum
# flexibility.
num_pos = target.sum(dim=1)
num_neg = target.size(0) - num_pos
```

我们已经具备了计算损失所需的所有要素！我们唯一需要做的就是按正负对的数量对正负损失进行加权，然后在小批量中计算损失的平均值。

```py
def nt_bxent_loss(x, pos_indices, temperature):
    assert len(x.size()) == 2

    # Add indexes of the principal diagonal elements to pos_indices
    pos_indices = torch.cat([
        pos_indices,
        torch.arange(x.size(0)).reshape(x.size(0), 1).expand(-1, 2),
    ], dim=0)

    # Ground truth labels
    target = torch.zeros(x.size(0), x.size(0))
    target[pos_indices[:,0], pos_indices[:,1]] = 1.0

    # Cosine similarity
    xcs = F.cosine_similarity(x[None,:,:], x[:,None,:], dim=-1)
    # Set logit of diagonal element to "inf" signifying complete
    # correlation. sigmoid(inf) = 1.0 so this will work out nicely
    # when computing the Binary cross-entropy Loss.
    xcs[torch.eye(x.size(0)).bool()] = float("inf")

    # Standard binary cross-entropy loss. We use binary_cross_entropy() here and not
    # binary_cross_entropy_with_logits() because of
    # https://github.com/pytorch/pytorch/issues/102894
    # The method *_with_logits() uses the log-sum-exp-trick, which causes inf and -inf values
    # to result in a NaN result.
    loss = F.binary_cross_entropy((xcs / temperature).sigmoid(), target, reduction="none")

    target_pos = target.bool()
    target_neg = ~target_pos

    loss_pos = torch.zeros(x.size(0), x.size(0)).masked_scatter(target_pos, loss[target_pos])
    loss_neg = torch.zeros(x.size(0), x.size(0)).masked_scatter(target_neg, loss[target_neg])
    loss_pos = loss_pos.sum(dim=1)
    loss_neg = loss_neg.sum(dim=1)
    num_pos = target.sum(dim=1)
    num_neg = x.size(0) - num_pos

    return ((loss_pos / num_pos) + (loss_neg / num_neg)).mean()

pos_indices = torch.tensor([
    (0, 0), (0, 2), (0, 4),
    (1, 4), (1, 6), (1, 1),
    (2, 3),
    (3, 7),
    (4, 3),
    (7, 6),
])
for t in (0.01, 0.1, 1.0, 10.0, 20.0):
    print(f"Temperature: {t:5.2f}, Loss: {nt_bxent_loss(x, pos_indices, temperature=t)}")
```

打印。

> 温度：0.01，损失：62.898780822753906
> 
> 温度：0.10，损失：4.851151943206787
> 
> 温度：1.00，损失：1.0727109909057617
> 
> 温度：10.00，损失：0.9827173948287964
> 
> 温度：20.00，损失：0.982099175453186

# 结论

自监督学习是深度学习中的一个新兴领域，它允许在未标记的数据上训练模型。这项技术让我们绕过了大规模标记数据的需求。

在这篇文章中，我们了解了对比学习的损失函数。第一个，称为NT-Xent损失，用于对每个输入在小批量中学习单个正对。我们介绍了NT-BXent损失，该损失用于在小批量中对每个输入学习多个（> 1）正对。我们学会了直观地解释这些损失，基于我们对交叉熵损失和二元交叉熵损失的理解。最后，我们在PyTorch中高效地实现了这两种损失函数。
