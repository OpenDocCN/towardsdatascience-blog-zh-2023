# 可解释机器学习的 6 个好处

> 原文：[`towardsdatascience.com/the-6-benefits-of-interpretable-machine-learning-e32fb8b60e9`](https://towardsdatascience.com/the-6-benefits-of-interpretable-machine-learning-e32fb8b60e9)

## 理解你的模型如何带来信任、知识和更好的生产性能

[](https://conorosullyds.medium.com/?source=post_page-----e32fb8b60e9--------------------------------)![Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----e32fb8b60e9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e32fb8b60e9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e32fb8b60e9--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----e32fb8b60e9--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e32fb8b60e9--------------------------------) ·9 分钟阅读·2023 年 1 月 30 日

--

![](img/283acb44320ff7d7c7b10fa2de9113ee.png)

(来源: [DALL.E 2](https://labs.openai.com/))

我们似乎正处于人工智能的黄金时代。每周都有一种新服务，可以从创作短篇故事到生成原创图像。这些创新得益于机器学习。我们使用强大的计算机和大量的数据来训练这些模型。问题是，这一过程让我们对它们的实际工作原理理解不够。

能力不断提升？却不知道它们如何运作？听起来像是我们想要机器人起义！别担心，还有一项平行的努力正在进行，旨在揭开这些怪兽的面纱。这来自于可解释机器学习（IML）领域。该研究受到更好理解我们模型所带来的诸多好处的推动。

不，IML（可解释机器学习）不会阻止人工智能末日。但它可以帮助**增加信任**于机器学习，并在其他领域**更广泛地应用**。你还可以**获得对数据集的了解**，并**讲述更好的故事**关于你的结果。你甚至可以提高**准确性**和**性能**。我们将深入讨论这 6 个好处。最后，我们将触及 IML 的局限性。

你可能还会喜欢这个话题的视频。如果你想了解更多，查看我的课程——[XAI with Python](https://adataodyssey.com/courses/xai-with-python/)。如果你订阅我的[新闻通讯](https://mailchi.mp/aa82a5ce1dc0/signup)，你可以**免费**访问。

# 什么是 IML？

在上一篇文章，我们深入讨论了 IML。总的来说，它是一个旨在构建可以被人类理解的机器学习模型的研究领域。这也涉及到开发能够帮助我们理解复杂模型的工具。实现这一目标的两种主要方法是：

+   **本质上可解释的模型** — 建立易于解释的模型的方法论

+   **模型无关方法** — 应用于训练后的任何黑箱模型

具体的好处将取决于你采取的方法。我们将重点关注后者。模型无关方法 可以应用于任何训练后的模型。这为我们的模型选择提供了灵活性。即我们可以使用复杂的模型，同时仍然获得对其工作原理的洞察。

# IML 的好处

显而易见的好处是 IML 的目标 — 理解模型。即它如何做出个别预测或其在一组预测中的总体行为。从中衍生出许多其他好处。

## 准确性提升

首先，IML 可以提高机器学习的准确性。在没有模型无关方法的情况下，我们面临的是权衡问题：

+   选项 1 — 使用一个我们不理解的准确黑箱模型。

+   选项 2 — 构建一个本质上可解释但准确性较低的模型。

现在我们可以建模我们的蛋糕并进行预测。通过在模型训练后应用诸如[SHAP](https://medium.com/towards-data-science/introduction-to-shap-with-python-d27edc23c454)、[LIME](https://medium.com/towards-data-science/squeezing-more-out-of-lime-with-python-28f46f74ca8e)或 PDPs 的方法，我们可以解读我们的黑箱模型。我们不再需要为了解释性而牺牲准确性。换句话说，通过增加模型选择的灵活性，IML 可以提高准确性。

更直接地说，模型无关的方法也可以提高黑箱模型的准确性。通过理解模型如何做出预测，我们也可以理解它为什么会做出*不正确*的预测。利用这些知识，我们可以改进数据收集过程或构建更好的特征。

## 提升生产环境中的表现

我们可以进一步扩展这一思想。即训练数据集上的准确性与生产环境中的新数据上的准确性不同。偏差和代理变量可能会导致不可预见的问题。IML 方法可以帮助我们识别这些问题。换句话说，它们可以用于调试和构建更健壮的模型。

一个例子来自一个用于驱动自动化汽车的模型。它根据赛道的图像来预测左转或右转。它在训练集和验证集上表现良好。然而，当我们转到一个新房间时，自动化汽车的表现却非常糟糕。**图 1**中的 SHAP 图可以帮助我们理解原因。注意背景中的像素具有较高的 SHAP 值。

![](img/996afc6e687106197c27ae1f796ff6d3.png)

图 1：示例的 SHAP 值在左转和右转时（来源：作者）

这意味着模型在进行预测时使用了背景信息。它只在一个房间的数据上进行了训练，并且所有图像中都有相同的物体和背景。因此，模型将这些与左转和右转相关联。当我们搬到新位置时，背景发生了变化，预测结果变得不可靠。

解决方案是收集更多的数据。我们可以继续使用 SHAP 来理解这是否导致了一个更强大的模型。实际上，我们在下面的文章中这样做了。如果你想了解更多关于这个应用的信息，可以查看一下。如果你想了解基础知识，可以参加我的[Python SHAP 课程](https://adataodyssey.com/courses/shap-with-python/)。如果你注册我的[通讯](https://mailchi.mp/aa82a5ce1dc0/signup)，可以免费获得访问权限。

[](/using-shap-to-debug-a-pytorch-image-regression-model-4b562ddef30d?source=post_page-----e32fb8b60e9--------------------------------) ## 使用 SHAP 调试 PyTorch 图像回归模型

### 使用 DeepShap 理解和改进驱动自动驾驶汽车的模型

[towardsdatascience.com

## 减少伤害并增加信任

调试不仅仅是正确预测。它还意味着确保预测是道德的。[Scott Lundberg](https://medium.com/u/3a739af9ef3a?source=post_page-----e32fb8b60e9--------------------------------)（SHAP 的创建者）在这个[演讲](https://www.youtube.com/watch?v=ngOBhhINWb8&ab_channel=H2O.ai)中讨论了一个例子。截图显示在**图 2**中。使用 SHAP，他展示了模型是如何使用**信用历史的月份**来预测违约的。这是一个年龄的代理变量——一个受保护的变量。

![](img/18288a9578c4a331d498f3b1d96abae7.png)

图 2：SHAP 演讲的快照（来源：[H20.ai](https://www.youtube.com/watch?v=ngOBhhINWb8&ab_channel=H2O.ai)，拍摄时间：14:02）

这表明退休客户更可能被拒绝贷款。这是因为他们的年龄而非真实的风险因素（例如现有债务）。换句话说，这个模型是基于年龄对客户进行歧视。

如果我们盲目信任黑箱模型，这些问题将会被忽视。IML 可以用于你的公平性分析中，以确保它们不会被用于做出有害于用户的决策。这有助于建立对我们 AI 系统的信任。

IML 另一种建立信任的方式是提供对人友好的解释。我们可以解释为什么你被拒绝了贷款或为什么做出了某个产品推荐。如果用户得到一个理由，他们更有可能接受这些决定。对于使用机器学习工具的专业人士也是如此。

## 扩展 ML 的影响力

机器学习无处不在。它正在改进或取代金融、法律甚至农业中的流程。一个有趣的应用是立即评估用于喂养奶牛的草的质量，这一过程过去既具有侵入性又漫长。

![](img/b7032d5bb66442667db8f2a2099de96f.png)

图 3：草质模型架构（来源： [M. Saadeldin, et. al.](https://www.sciencedirect.com/science/article/pii/S2352938522000490?via%3Dihub=)）

你不会期望普通农民对神经网络有了解。黑箱性质会使他们难以接受预测结果。即使在更技术性的领域，也可能对深度学习方法存在不信任。

> 许多从事水文学遥感、大气遥感和海洋遥感等领域的科学家甚至不相信深度学习的预测结果，因为这些领域的专家更倾向于相信具有明确物理意义的模型。 *—* [*Prof. Dr. Lizhe Wang*](https://www.mdpi.com/journal/remotesensing/special_issues/XAI_big_data)

IML 可以被视为计算机科学与其他行业/科学领域之间的桥梁。提供对黑箱的透视将使他们更容易接受结果。这将提高机器学习方法的采用率。

## 提升你讲故事的能力

前面两个好处都涉及建立信任。客户和专业人士的信任。即使在机器学习已经被广泛接受的环境中，你可能仍然需要建立信任。这是为了说服你的同事一个模型能够完成其工作。

数据科学家通过数据讲故事来实现这一点。即将数据中发现的结果与技术水平较低的同事的经验联系起来。通过在数据探索和建模结果之间建立联系，IML 可以帮助实现这一点。

看下面的散点图。当员工拥有一个 **学位**（学位 = 1）时，他们的年 **奖金** 倾向于随着经验年限的增加而增加。然而，当他们没有学位时，他们的奖金保持稳定 **。** 换句话说，学位和经验之间存在交互作用。

![](img/9932c639384df9ffb143464382740041.png)

图 4：经验与学位交互的散点图（来源：作者）

现在看下面的 ICE 图。它来源于一个用于预测奖金的模型，该模型使用了一组包括经验和学位在内的特征。我们可以看到模型捕捉到了交互作用。它利用我们在数据中观察到的关系来进行预测。

![](img/9872379daf2d2b5857054626f97f748b.png)

图 5：经验与学位交互的 ICE 图（来源：作者）

通过 IML，我们从“我们认为模型使用了我们在数据中观察到的这种关系”变成了“看！看到！！模型正在使用这种关系。”我们还可以将模型结果与同事的经验进行比较。这使他们可以利用他们的领域知识来验证模型捕捉到的趋势。有时我们甚至可以学到全新的东西。

## 获得知识

黑箱模型可以自动建模数据中的交互作用和非线性关系。利用 IML，我们可以分析模型，以揭示数据集中这些关系。这些知识可以用于：

+   为非线性模型提供**特征工程**的帮助。

+   在超出模型范围的**决策**时提供帮助。

最终，IML 帮助机器学习成为数据探索和知识生成的工具。如果别无其他，深入了解一个模型以理解其工作原理也会非常吸引人。

# IML 的局限性

尽管有这些好处，IML 仍然有其局限性。我们在使用这些方法得出结论时需要考虑这些局限性。最重要的是所做的假设。例如，SHAP 和 PDPs 都假设特征之间没有依赖（即模型特征是无关的）。如果这一假设不成立，这些方法可能会不可靠。

另一个限制是这些方法可能会被滥用。我们需要对结果进行解释，而我们可以将故事强加于分析之上。这可能由于确认偏差而无意识地发生。也可以恶意地进行，以支持对某人有利的结论。这类似于 p-hacking——我们扭曲数据，直到它给出我们想要的结果。

最后要考虑的是，这些方法仅提供技术解释。它们对数据科学家理解和调试模型很有用。然而，我们不能用它们来向非专业客户或同事解释模型。要做到这一点，需要一套新的技能和方法。我们将在这篇文章中讨论这一点：

[](/the-art-of-explaining-predictions-22e3584ed7d8?source=post_page-----e32fb8b60e9--------------------------------) ## 解释预测的艺术

### 如何以对人友好的方式解释你的模型

towardsdatascience.com

你还可以找到一些介绍 IML 方法的入门文章：

[](/the-ultimate-guide-to-pdps-and-ice-plots-4182885662aa?source=post_page-----e32fb8b60e9--------------------------------) ## PDPs 和 ICE 图的终极指南

### 部分依赖图和个体条件期望背后的直觉、数学和代码（R 和 Python）……

towardsdatascience.com [](/introduction-to-shap-with-python-d27edc23c454?source=post_page-----e32fb8b60e9--------------------------------) ## SHAP 与 Python 入门

### 如何创建和解释 SHAP 图：瀑布图、力图、决策图、平均 SHAP 图和蜂群图

towardsdatascience.com

我希望你喜欢这篇文章！你可以通过成为我的[**推荐会员**](https://conorosullyds.medium.com/membership)来支持我 **:)**

[](https://conorosullyds.medium.com/membership?source=post_page-----e32fb8b60e9--------------------------------) [## 使用我的推荐链接加入 Medium — Conor O’Sullivan

### 作为 Medium 会员，你的会员费的一部分会分配给你阅读的作者，你可以完全访问所有故事……

conorosullyds.medium.com](https://conorosullyds.medium.com/membership?source=post_page-----e32fb8b60e9--------------------------------)

| [Twitter](https://twitter.com/conorosullyDS) | [YouTube](https://www.youtube.com/channel/UChsoWqJbEjBwrn00Zvghi4w) | [Newsletter](https://mailchi.mp/aa82a5ce1dc0/signup) — 免费注册以获得[Python SHAP 课程](https://adataodyssey.com/courses/shap-with-python/)的访问权限
