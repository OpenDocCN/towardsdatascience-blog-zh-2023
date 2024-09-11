# 在 PyTorch 中实现可解释的神经模型！

> 原文：[https://towardsdatascience.com/implement-interpretable-neural-models-in-pytorch-6a5932bdb078?source=collection_archive---------4-----------------------#2023-05-29](https://towardsdatascience.com/implement-interpretable-neural-models-in-pytorch-6a5932bdb078?source=collection_archive---------4-----------------------#2023-05-29)

[](https://medium.com/@pb737?source=post_page-----6a5932bdb078--------------------------------)[![Pietro Barbiero](../Images/28a6cfc89e32de0401ec3d732c085410.png)](https://medium.com/@pb737?source=post_page-----6a5932bdb078--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6a5932bdb078--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6a5932bdb078--------------------------------) [Pietro Barbiero](https://medium.com/@pb737?source=post_page-----6a5932bdb078--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc3b867b869ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-interpretable-neural-models-in-pytorch-6a5932bdb078&user=Pietro+Barbiero&userId=c3b867b869ca&source=post_page-c3b867b869ca----6a5932bdb078---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6a5932bdb078--------------------------------) · 11 分钟阅读 · 2023年5月29日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6a5932bdb078&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-interpretable-neural-models-in-pytorch-6a5932bdb078&user=Pietro+Barbiero&userId=c3b867b869ca&source=-----6a5932bdb078---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6a5932bdb078&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplement-interpretable-neural-models-in-pytorch-6a5932bdb078&source=-----6a5932bdb078---------------------bookmark_footer-----------)

**总结 —** 体验可解释性的强大力量，通过 “[PyTorch, Explain!](https://github.com/pietrobarbiero/pytorch_explain)” —— 一个能够帮助你实现最先进且可解释的基于概念的模型的 Python 库！ [[GitHub](https://github.com/pietrobarbiero/pytorch_explain)]

![](../Images/424bbb13c1b438e116c9cc6b35e31bd1.png)

可解释的 AI 模型会给出人类能够理解的预测理由。图片由作者提供。

灵感来源于以下方法的教程：

+   ***ICML*** 2020 论文 “[概念瓶颈模型](https://arxiv.org/abs/2007.04612)”；

+   ***NeurIPS*** 2022 论文 “[概念嵌入模型：超越准确性与可解释性的权衡](https://arxiv.org/abs/2209.09056)”；

+   ***ICML*** 2023 论文 “[可解释的神经符号概念推理](https://arxiv.org/abs/2304.14068)”。

# 动机

深度学习系统缺乏可解释性对建立人类信任构成了重大挑战。这些模型的复杂性使得人类几乎无法理解其决策背后的原因。

> 深度学习系统缺乏可解释性阻碍了人类信任。

为了解决这个问题，研究人员一直在积极探索新颖的解决方案，导致了诸如基于概念的模型这样的重大创新。这些模型不仅提高了模型的透明性，还通过在训练过程中融入高层次的人类可解释概念（如“颜色”或“形状”）来培养对系统决策的新信任感。因此，这些模型可以提供简单直观的预测解释，基于学习到的概念，允许人类**检查决策背后的推理**。而且不仅如此！它们甚至允许人类与学习到的概念互动，赋予我们**对最终决策的控制**。

> 基于概念的模型允许人们**检查深度学习预测背后的推理**，并重新获得**对最终决策的控制**。

在这篇博客文章中，我们将深入探讨这些技术，并为你提供使用简单 PyTorch 接口实现最先进的基于概念的模型的工具。通过实践经验，你将学会如何利用这些强大的模型来增强可解释性，并最终校准人类对深度学习系统的信任。

# 教程 #1：实现你的第一个概念瓶颈模型

为了展示 PyTorch Explain 的强大功能，让我们深入我们的第一个教程！

## 概念瓶颈模型简介

在这个介绍性会议中，我们将深入探讨概念瓶颈模型。这些模型在2020年国际机器学习会议上发表的论文 [1] 中首次提出，旨在首先学习和预测一组概念，例如“颜色”或“形状”，然后利用这些概念来解决下游分类任务：

![](../Images/3266d868c1f5d009d12463ed3c44186d.png)

概念瓶颈模型将任务（**Y**）学习为概念（**C**）的函数。图像由作者提供。

通过遵循这种方法，我们可以将预测追溯到概念，从而提供类似于“输入对象是一个{苹果}，因为它是{球形}和{红色}”的解释。

> 概念瓶颈模型首先学习一组概念，例如“颜色”或“形状”，然后利用这些概念来解决下游分类任务。

## 实践概念瓶颈

为了说明概念瓶颈模型，我们将重新审视众所周知的异或（XOR）问题，但有了一点变化。我们的输入将由两个连续的特征组成。为了捕捉这些特征的本质，我们将使用一个概念编码器将它们映射到两个有意义的概念，标记为“A”和“B”。我们的任务目标是预测“A”和“B”的异或（XOR）。通过解决这个例子，你将更好地理解概念瓶颈如何在实践中应用，并见证它们在解决具体问题时的有效性。

我们可以先导入必要的库并加载这个简单的数据集：

```py
import torch
import torch_explain as te
from torch_explain import datasets
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split

x, c, y = datasets.xor(500)
x_train, x_test, c_train, c_test, y_train, y_test = train_test_split(x, c, y, test_size=0.33, random_state=42)
```

接下来，我们实例化一个概念编码器，将输入特征映射到概念空间，并一个任务预测器，将概念映射到任务预测中：

```py
concept_encoder = torch.nn.Sequential(
    torch.nn.Linear(x.shape[1], 10),
    torch.nn.LeakyReLU(),
    torch.nn.Linear(10, 8),
    torch.nn.LeakyReLU(),
    torch.nn.Linear(8, c.shape[1]),
    torch.nn.Sigmoid(),
)
task_predictor = torch.nn.Sequential(
    torch.nn.Linear(c.shape[1], 8),
    torch.nn.LeakyReLU(),
    torch.nn.Linear(8, 1),
)
model = torch.nn.Sequential(concept_encoder, task_predictor)
```

然后，我们通过优化交叉熵损失来训练网络，既包括概念又包括任务：

```py
optimizer = torch.optim.AdamW(model.parameters(), lr=0.01)
loss_form_c = torch.nn.BCELoss()
loss_form_y = torch.nn.BCEWithLogitsLoss()
model.train()
for epoch in range(2001):
    optimizer.zero_grad()

    # generate concept and task predictions
    c_pred = concept_encoder(x_train)
    y_pred = task_predictor(c_pred)

    # update loss
    concept_loss = loss_form_c(c_pred, c_train)
    task_loss = loss_form_y(y_pred, y_train)
    loss = concept_loss + 0.2*task_loss

    loss.backward()
    optimizer.step()
```

在对模型进行训练后，我们在测试集上评估其性能：

```py
c_pred = concept_encoder(x_test)
y_pred = task_predictor(c_pred)

concept_accuracy = accuracy_score(c_test, c_pred > 0.5)
task_accuracy = accuracy_score(y_test, y_pred > 0)
```

现在，经过仅仅几个epochs之后，我们可以观察到概念和任务的准确率在测试集上相当不错（~98% 准确率）！

多亏了这种架构，我们可以通过观察任务预测器对输入概念的响应来为模型的预测提供解释，方法如下：

```py
c_different = torch.FloatTensor([0, 1])
print(f"f({c_different}) = {int(task_predictor(c_different).item() > 0)}")

c_equal = torch.FloatTensor([1, 1])
print(f"f({c_different}) = {int(task_predictor(c_different).item() > 0)}")
```

这导致例如，`f([0,1])=1`和`f([1,1])=0`，如预期的那样。这使我们能够更多地理解模型的行为，并检查它是否对任何相关的概念集合（例如，对于互斥的输入概念`[0,1]`或`[1,0]`）都返回`y=1`的预测。

> 概念瓶颈模型通过将预测追溯到概念来提供**直观**的解释。

## 在准确性-可解释性的权衡中陷入困境

概念瓶颈模型的一个关键优势之一是它们能够通过揭示概念-预测模式来为他们的预测提供解释，从而使人类能够评估模型的推理是否符合他们的期望。

但是，标准概念瓶颈模型的主要问题在于它们在解决复杂问题时很困难！更一般地说，它们**在可解释人工智能中遇到了一个众所周知的问题，被称为准确性-可解释性的权衡**。实际上，我们希望模型不仅在任务表现上取得高准确性，而且能够提供高质量的解释。然而，很不幸的是，在许多情况下，随着我们追求更高的准确性，模型提供的解释往往会质量下降，且反之亦然。

在视觉上，这种权衡可以表示为下图所示：

![](../Images/ba3b17e1ca59b6c3cabc0f81395f1077.png)

准确性-可解释性权衡的视觉表示。图片

展示了可解释和“黑盒”（不可解释）模型之间的差异

从两个角度来看：任务表现和解释质量。图片作者提供。

可解释模型擅长提供高质量解释，但在解决具有挑战性的任务时表现不佳，而黑盒模型则通过提供脆弱和贫乏的解释来实现高任务准确性。

为了在一个具体的情景中说明这种折衷，让我们考虑一个应用于略微更具挑战性基准数据集“三角函数”的概念瓶颈模型：

```py
x, c, y = datasets.trigonometry(500)
x_train, x_test, c_train, c_test, y_train, y_test = train_test_split(x, c, y, test_size=0.33, random_state=42)
```

在这个数据集上训练相同的网络架构后，我们观察到明显降低的任务准确性，仅达到约80%。

> 概念瓶颈模型未能在任务准确性和解释质量之间取得平衡。

这引出了一个问题：我们是否被永远迫使在准确性和解释质量之间进行选择，还是有更好的平衡方式？

# 教程 #2：超越准确性和可解释性的折衷之道：概念嵌入模型

答案是“是的！”，方案确实存在！

## 概念嵌入模型简介

最近提出的解决这一挑战的方法是在 *神经信息处理系统的进展* 会议上介绍的，一篇名为“概念嵌入模型：超越准确性和可解释性之间的折衷”的论文 [2]（如果你想了解更多，我在[这篇](/concept-embedding-models-beyond-the-accuracy-explainability-trade-off-f7ba02f28fad)博客文章中更详细地讨论了这种方法！）。 这篇论文的关键创新是设计了受监督的高维概念表示。与用单个神经元激活表示每个概念的标准概念瓶颈模型不同：

![](../Images/15b64307f135319265cae5970401a4c4.png)

概念瓶颈模型将任务（**Y**）作为概念（**C**）的函数进行学习。图片由作者提供。

… 一个概念嵌入模型用一组神经元表示每个概念，有效地克服了与概念层相关的信息瓶颈：

![](../Images/eec3e417a7fcbc25c6982010803deb6a.png)

概念嵌入模型将每个概念表示为一个受监督的向量。图片由作者提供。

因此，概念嵌入模型使我们能够同时实现高准确性和高质量解释：

![](../Images/8cc20b7417da10e603aa681a9e8d3cbe.png)

概念嵌入模型在概念瓶颈模型中超越了准确性和可解释性的折衷，几乎实现了最佳的任务准确性和概念对齐。 最佳折衷由红色星星（右上方）表示。 任务是学习两个向量之间点积的符号（+/-）。图片由作者提供。

> 概念嵌入模型成功地在任务准确性和解释质量之间取得平衡。

## 亲身体验概念嵌入模型

在 pytorch 中实现这些模型就像实现标准概念瓶颈模型那样简单！

我们从加载数据开始：

```py
x, c, y = datasets.trigonometry(500)
x_train, x_test, c_train, c_test, y_train, y_test = train_test_split(x, c, y, test_size=0.33, random_state=42)
```

接下来，我们实例化一个概念编码器，将输入特征映射到概念空间，并实例化一个任务预测器，将概念映射到任务预测：

```py
embedding_size = 8
concept_encoder = torch.nn.Sequential(
    torch.nn.Linear(x.shape[1], 10),
    torch.nn.LeakyReLU(),
    te.nn.ConceptEmbedding(10, c.shape[1], embedding_size),
)
task_predictor = torch.nn.Sequential(
    torch.nn.Linear(c.shape[1]*embedding_size, 8),
    torch.nn.LeakyReLU(),
    torch.nn.Linear(8, 1),
)
model = torch.nn.Sequential(concept_encoder, task_predictor)
```

然后，我们通过优化概念和任务上的交叉熵损失来训练网络：

```py
optimizer = torch.optim.AdamW(model.parameters(), lr=0.01)
loss_form_c = torch.nn.BCELoss()
loss_form_y = torch.nn.BCEWithLogitsLoss()
model.train()
for epoch in range(2001):
    optimizer.zero_grad()

    # generate concept and task predictions
    c_emb, c_pred = concept_encoder(x_train)
    y_pred = task_predictor(c_emb.reshape(len(c_emb), -1))

    # compute loss
    concept_loss = loss_form_c(c_pred, c_train)
    task_loss = loss_form_y(y_pred, y_train)
    loss = concept_loss + 0.2*task_loss

    loss.backward()
    optimizer.step()
```

在训练模型后，我们在测试集上评估其性能：

```py
c_emb, c_pred = concept_encoder.forward(x_test)
y_pred = task_predictor(c_emb.reshape(len(c_emb), -1))

concept_accuracy = accuracy_score(c_test, c_pred > 0.5)
task_accuracy = accuracy_score(y_test, y_pred > 0)
```

现在，仅经过几轮训练，我们可以观察到概念和任务的准确性在测试集上都相当好（约96%的准确率），比标准概念瓶颈模型高出近15%！

## 为什么可解释性>可解释性？

尽管到目前为止所讨论的技术提供了简单直观的解释，但仍存在一个固有的限制：模型预测背后的精确逻辑推理仍然不清楚。

实际上，即使我们使用像决策树或逻辑回归这样的透明机器学习模型，它也不一定能缓解使用概念嵌入时的问题。这是因为概念向量的个别维度对人类缺乏明确的语义解释。例如，决策树中的逻辑句子*“如果 {yellow[2]>0.3} 和 {yellow[3]<-1.9} 和 {round[1]>4.2} 那么 {banana}”* 的术语如*“{yellow[2]>0.3}”*（指概念向量“yellow”的第二维大于“0.3”）对我们来说并没有太大语义意义。

![](../Images/c634d0f0dd1554ef247f98676266e0e7.png)

标准的可解释分类器无法使用概念嵌入提供可解释的预测，因为个别嵌入维度缺乏明确的语义意义。图片由作者提供。

> 即使是透明模型在应用于概念嵌入时也无法提供可解释的预测。

我们这次如何克服这个挑战？！

# 第3步：无妥协的可解释性

再次，解决方案确实存在！

## 深度概念推理简介

深度概念推理器 [3]（一篇被2023年*国际机器学习大会*接受的最新论文）通过实现完全的可解释性来解决概念嵌入模型的局限性。该方法的关键创新在于设计了一个任务预测器，分别处理概念嵌入和概念真实性度。标准的机器学习模型则会同时处理概念嵌入和概念真实性度：

![](../Images/c634d0f0dd1554ef247f98676266e0e7.png)

标准的可解释分类器无法使用概念嵌入提供可解释的预测，因为个别嵌入维度缺乏明确的语义意义。图片由作者提供。

深度概念推理器生成（***可解释的***！）逻辑规则，使用概念嵌入，然后以符号化方式执行规则，将相应的真实性值分配给概念符号：

![](../Images/32b9923c40fcb6be424da5d743252162.png)

深度概念推理器使用神经模型在概念嵌入上生成模糊逻辑规则，然后

使用概念真实性度执行规则，以符号化方式评估规则。图片由作者提供。

> 深度概念推理器在应用于概念嵌入时提供可解释的预测，因为每个预测都是基于逻辑规则的概念真值生成的。

这种独特的技术使我们能够实现**完全可解释的模型，因为它们基于逻辑规则进行预测，如决策树一样！** 使它们与众不同的是在挑战性任务中的卓越表现，超越了传统的可解释模型，如决策树或逻辑回归：

![](../Images/3ea093fae88bfa1d3f630952ae078f92.png)

深度概念推理器超越了可解释的基于概念的模型，并且与黑箱模型的准确性相匹配。CE代表概念嵌入，CT代表概念真值。图片由作者提供。

通过利用深度概念推理，我们可以释放出具有高解释性的模型的潜力，这些模型在复杂任务上表现出色。

> 深度概念推理器提供可解释的预测，同时在任务准确性方面超越了可解释模型。

## 实践深度概念推理

使用`pytorch_explain`库实现深度概念推理同样非常简单！

如前面的示例所示，我们实例化一个概念编码器，将输入特征映射到概念空间，并实例化一个深度概念推理器，将概念映射到任务预测：

```py
from torch_explain.nn.concepts import ConceptReasoningLayer
import torch.nn.functional as F

x, c, y = datasets.xor(500)
x_train, x_test, c_train, c_test, y_train, y_test = train_test_split(x, c, y, test_size=0.33, random_state=42)
y_train = F.one_hot(y_train.long().ravel()).float()
y_test = F.one_hot(y_test.long().ravel()).float()

embedding_size = 8
concept_encoder = torch.nn.Sequential(
    torch.nn.Linear(x.shape[1], 10),
    torch.nn.LeakyReLU(),
    te.nn.ConceptEmbedding(10, c.shape[1], embedding_size),
)
task_predictor = ConceptReasoningLayer(embedding_size, y_train.shape[1])
model = torch.nn.Sequential(concept_encoder, task_predictor)
```

然后，我们通过优化概念和任务的交叉熵损失来训练网络：

```py
optimizer = torch.optim.AdamW(model.parameters(), lr=0.01)
loss_form = torch.nn.BCELoss()
model.train()
for epoch in range(2001):
    optimizer.zero_grad()

    # generate concept and task predictions
    c_emb, c_pred = concept_encoder(x_train)
    y_pred = task_predictor(c_emb, c_pred)

    # compute loss
    concept_loss = loss_form(c_pred, c_train)
    task_loss = loss_form(y_pred, y_train)
    loss = concept_loss + 0.2*task_loss

    loss.backward()
    optimizer.step()
```

训练模型后，我们可以在测试集上评估其性能，并检查其是否与概念嵌入模型的准确性相匹配（约99%）：

```py
c_emb, c_pred = concept_encoder.forward(x_test)
y_pred = task_predictor(c_emb, c_pred)

concept_accuracy = accuracy_score(c_test, c_pred > 0.5)
task_accuracy = accuracy_score(y_test, y_pred > 0.5)
```

但是，这次我们可以通过阅读相应的逻辑规则，精确获取每个预测背后的推理：

```py
local_explanations = task_predictor.explain(c_emb, c_pred, 'local')
```

每个`local_explanations`中的元素具有以下结构：

```py
{'sample-id': 0,
 'class': 'y_1',
 'explanation': '~c_0 & c_1',
 'attention': [-1.0, 1.0]}
```

同样，我们可以使用命令提取全局解释：

```py
global_explanations = task_predictor.explain(c_emb, c_pred, 'global')
```

该方法返回模型找到的完整规则集，使人类能够再次检查深度学习系统的推理是否与预期行为一致：

```py
[{'class': 'y_0', 'explanation': 'c_0 & c_1', 'count': 39},
 {'class': 'y_0', 'explanation': '~c_0 & ~c_1', 'count': 46},
 {'class': 'y_1', 'explanation': '~c_0 & c_1', 'count': 45},
 {'class': 'y_1', 'explanation': 'c_0 & ~c_1', 'count': 35}]
```

# 主要收获

在这篇文章中，我们探索了`pytorch_explain`库的关键功能，突出了最先进的基于概念的架构，并用几行代码演示了它们的实现。

这里是我们涵盖内容的总结：

+   概念瓶颈模型：这些模型通过追溯预测到一组人类可解释的概念，提供直观的解释；

+   概念嵌入模型：通过克服与概念相关的信息瓶颈，这些模型在不影响解释质量的情况下实现了高预测准确性；

+   深度概念推理器：这些模型的预测完全可解释，因为深度概念推理器使用逻辑规则来组合概念的真值进行预测。

通过利用“*Pytorch, Explain!*”库的强大功能并实施讨论中的技术，你有机会显著提升模型的可解释性，同时保持高预测准确性。这不仅使你能够深入了解模型预测背后的推理，还培养和校准用户对系统的信任。

# 参考文献

[1] Koh, Pang Wei, 等人. “概念瓶颈模型。” *国际机器学习大会*。PMLR，2020年。

[2] Zarlenga, Mateo Espinosa, 等人. “概念嵌入模型：超越准确性与可解释性权衡。” *神经信息处理系统进展*。第35卷。Curran Associates, Inc.，2022年。21400–21413。

[3] Barbiero, Pietro, 等人. “可解释的神经符号概念推理。” *arXiv预印本 arXiv:2304.14068*（2023年）。
