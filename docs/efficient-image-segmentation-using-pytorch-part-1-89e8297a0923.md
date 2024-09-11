# 使用 PyTorch 的高效图像分割：第 1 部分

> 原文：[https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-1-89e8297a0923?source=collection_archive---------1-----------------------#2023-06-27](https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-1-89e8297a0923?source=collection_archive---------1-----------------------#2023-06-27)

## 概念与想法

[](https://medium.com/@dhruvbird?source=post_page-----89e8297a0923--------------------------------)[![Dhruv Matani](../Images/d63bf7776c28a29c02b985b1f64abdd3.png)](https://medium.com/@dhruvbird?source=post_page-----89e8297a0923--------------------------------)[](https://towardsdatascience.com/?source=post_page-----89e8297a0923--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----89e8297a0923--------------------------------) [Dhruv Matani](https://medium.com/@dhruvbird?source=post_page-----89e8297a0923--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F63f5d5495279&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-image-segmentation-using-pytorch-part-1-89e8297a0923&user=Dhruv+Matani&userId=63f5d5495279&source=post_page-63f5d5495279----89e8297a0923---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----89e8297a0923--------------------------------) ·18分钟阅读·2023年6月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F89e8297a0923&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-image-segmentation-using-pytorch-part-1-89e8297a0923&user=Dhruv+Matani&userId=63f5d5495279&source=-----89e8297a0923---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F89e8297a0923&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-image-segmentation-using-pytorch-part-1-89e8297a0923&source=-----89e8297a0923---------------------bookmark_footer-----------)

在这个四部分系列中，我们将一步步地使用 PyTorch 的深度学习技术从零开始实现图像分割。我们将从本文开始介绍图像分割所需的基本概念与想法。

![](../Images/bbca4847fd8e7d2bf8725818e1f76773.png)

图1：宠物图像及其分割掩膜（来源：[The Oxford-IIIT Pet Dataset](https://www.robots.ox.ac.uk/~vgg/data/pets/))

**与** [**Naresh Singh**](https://medium.com/@brocolishbroxoli) **合作撰写**

[图像分割](https://en.wikipedia.org/wiki/Image_segmentation)是一种将图像中属于特定对象的像素隔离的技术。隔离对象像素开辟了有趣的应用。例如，在图 1 中，右侧的图像是对应左侧宠物图像的掩码，其中黄色像素属于宠物。一旦像素被识别，我们可以轻松地放大宠物或更改图像背景。这种技术在几个社交媒体应用中的面部滤镜功能中被广泛使用。

我们在本系列文章结束时的目标是让读者了解构建视觉 AI 模型并使用 PyTorch 进行不同设置实验的所有步骤。

# 本系列文章

本系列适用于所有深度学习经验水平的读者。如果你想了解深度学习和视觉 AI 的实践，以及一些扎实的理论和实践经验，你来对地方了！预计这是一个四部分的系列，包括以下文章：

1.  **概念和思路（本文）**

1.  [基于 CNN 的模型](https://medium.com/p/bed68cadd7c7/)

1.  [深度可分离卷积](https://medium.com/p/3534cf04fb89/)

1.  [基于视觉变换器的模型](https://medium.com/p/6c86da083432/)

# 图像分割简介

[图像分割](https://en.wikipedia.org/wiki/Image_segmentation)将图像划分或分割成对应于对象、背景和边界的区域。请查看图 2，它展示了一个城市场景。它用不同的颜色掩码标记了对应于汽车、摩托车、树木、建筑物、人行道和其他有趣对象的区域。这些区域是通过图像分割技术识别的。

历史上，我们使用了[专用图像处理工具和流程](https://www.v7labs.com/blog/image-segmentation-guide#h4)来将图像分解为不同区域。然而，由于过去二十年来视觉数据的惊人增长，深度学习已成为图像分割任务的首选解决方案。它显著减少了对专家的依赖，以构建特定领域的图像分割策略，这在过去是必需的。只要有足够的训练数据，深度学习从业者可以训练图像分割模型。

![](../Images/3a397ef2a4ba2c4df9565de6196fe194.png)

图 2：来自[a2d2 数据集 (CC BY-ND 4.0)](https://aev-autonomous-driving-dataset.s3.eu-central-1.amazonaws.com/LICENSE.txt)的分割场景

## 图像分割的应用有哪些？

图像分割在通信、农业、交通、医疗保健等多个领域都有应用。此外，随着视觉数据的增长，它的应用也在不断增长。以下是一些例子：

+   在**自动驾驶汽车**中，深度学习模型不断处理来自汽车摄像头的视频流，将场景分割成汽车、行人和交通信号灯等对象，这对于汽车安全操作至关重要。

+   在**医学成像**中，图像分割帮助医生识别医学扫描中的肿瘤、病变和其他异常区域。

+   在**Zoom视频通话**中，利用虚拟场景替换背景以保护个人隐私。

+   在**农业**中，通过图像分割识别的杂草和作物区域的信息被用来[保持健康的作物产量](https://www.sciencedirect.com/science/article/pii/S2214317323000112)。

你可以在[v7labs的这一页面](https://www.v7labs.com/blog/image-segmentation-guide#h6)上阅读有关图像分割实际应用的更多细节。

## 图像分割任务的不同类型有哪些？

图像分割任务有很多不同类型，每种类型都有其优缺点。最常见的两种图像分割任务是：

+   **类别或语义分割**：类别分割为每个图像像素分配一个语义类别，如*背景*、*道路*、*汽车*或*行人*。如果图像中有2辆车，那么与两辆车对应的像素将标记为汽车像素。这通常用于自主驾驶和[场景理解](https://ps.is.mpg.de/research_fields/semantic-scene-understanding)等任务。

+   **对象或实例分割**：对象分割识别图像中的对象，并为每个独特对象分配一个掩膜。如果图像中有2辆车，那么与每辆车对应的像素将被识别为属于不同的对象。对象分割通常用于跟踪单个对象，例如编程为跟随前方特定汽车的自动驾驶汽车。

![](../Images/9cd15779082c7c510647f283d7f03af8.png)

图3：对象和类别分割（来源：[MS Coco — 创作共享署名许可](https://cocodataset.org/#home)）

在这一系列中，我们将重点关注类别分割。

## 实施高效图像分割所需的决策

高效训练模型以提高速度和准确性涉及在项目生命周期内做出许多重要决策。这包括（但不限于）：

1.  选择你的深度学习框架

1.  选择一个好的模型架构

1.  选择一个有效的损失函数来优化你关心的方面

1.  避免过拟合和欠拟合

1.  评估模型的准确性

在本文的其余部分，我们将深入探讨上述每一个方面，并提供大量链接，以便进一步了解每个主题。

# 高效图像分割的PyTorch

## 什么是PyTorch？

*“*[*PyTorch*](https://pytorch.org/) *是一个开源深度学习框架，旨在灵活和模块化以便于研究，同时具备生产部署所需的稳定性和支持。PyTorch 提供了一个 Python 包，用于高层次的特性，如张量计算（类似于 NumPy），并具有强大的 GPU 加速和 TorchScript，实现了在急切模式和图模式之间的轻松过渡。最新版本的 PyTorch 框架提供了基于图的执行、分布式训练、移动部署和量化。”*（来源：[Meta AI 页面的 PyTorch](https://ai.facebook.com/tools/pytorch/)）

PyTorch 是用 Python 和 C++ 编写的，这使得它既易于使用和学习，又高效运行。它支持多种硬件平台，包括（服务器和移动设备）CPU、GPU 和 TPU。

## 为什么 PyTorch 是图像分割的好选择？

PyTorch 是深度学习研究和开发的热门选择，因为它提供了一个灵活且强大的环境来创建和训练神经网络。它是实现基于深度学习的图像分割的绝佳框架，具有以下特点：

+   **灵活性**：PyTorch 是一个灵活的框架，允许你以多种方式创建和训练神经网络。你可以使用预训练模型，也可以非常轻松地从头开始创建自己的模型。

+   **后端支持**：PyTorch 支持多种后端，如 GPU/TPU 硬件。

+   **领域库**：PyTorch 具有丰富的领域库，使得处理特定数据垂直领域变得非常容易。例如，对于与视觉（图像/视频）相关的 AI，PyTorch 提供了一个库 [torchvision](https://pytorch.org/vision/main/index.html)，我们将在本系列中广泛使用。

+   **易用性和社区接受度**：PyTorch 是一个易于使用的框架，文档齐全，并拥有一个大型的 [社区](https://dev-discuss.pytorch.org/) 用户和开发者。许多研究人员在他们的实验中使用 PyTorch，他们发表的论文中有一个 PyTorch 实现的模型可以自由获取。

# 数据集的选择

我们将使用 [Oxford IIIT Pet 数据集](https://www.robots.ox.ac.uk/~vgg/data/pets/)（许可协议：CC BY-SA 4.0）进行类别分割。这个数据集的训练集中有 3680 张图像，每张图像都有一个分割三值图。三值图分为 3 类像素：

1.  宠物

1.  背景

1.  边界

我们选择这个数据集是因为它足够多样化，能够提供一个非平凡的类别分割任务。此外，它又不至于复杂到我们需要花时间处理类别不平衡等问题，从而失去对我们想要学习和解决的主要问题的关注；即类别分割。

其他常用的图像分割任务数据集包括：

1.  [Pascal VOC（视觉对象类别）](http://host.robots.ox.ac.uk/pascal/VOC/)

1.  [MS Coco](https://cocodataset.org/#home)

1.  [Cityscapes](https://www.cityscapes-dataset.com/examples/)

# 使用 PyTorch 进行高效图像分割

在本系列中，我们将从头开始训练多个分类分割模型。构建和训练从头开始的模型时需要考虑许多因素。以下，我们将探讨在进行此操作时需要做出的一些关键决策。

## 选择适合你任务的模型

选择适合图像分割的深度学习模型时，有许多因素需要考虑。最重要的一些因素包括：

+   **图像分割任务的类型**：图像分割任务主要有两种类型：分类（语义）分割和对象（实例）分割。由于我们专注于较简单的分类分割问题，我们将根据这个问题来建模。

+   **数据集的大小和复杂性**：数据集的大小和复杂性将影响我们需要使用的模型的复杂性。例如，如果我们处理的是空间维度较小的图像，我们可以使用较简单（或较浅）的模型，如全卷积网络（FCN）。如果我们处理的是大而复杂的数据集，我们可以使用更复杂（或更深）的模型，如 U-Net。

+   **预训练模型的可用性**：有许多预训练模型可用于图像分割。这些模型可以作为我们自己模型的起点，也可以直接使用。然而，如果我们使用预训练模型，可能会受到模型输入图像空间维度的限制。在本系列中，我们将重点关注从头开始训练模型。

+   **可用的计算资源**：深度学习模型的训练可能计算开销较大。如果我们的计算资源有限，可能需要选择较简单的模型或更高效的模型架构。

在这一系列中，我们将使用 Oxford IIIT Pet 数据集，因为它足够大，可以训练中等规模的模型，并且需要使用 GPU。我们强烈建议在 [kaggle.com](https://www.kaggle.com/) 上创建一个帐户，或使用 [Google Colab](https://colab.research.google.com/) 的免费 GPU 来运行本系列中提到的笔记本和代码。

## 模型架构

以下是一些最受欢迎的深度学习模型架构，用于图像分割：

+   [**U-Net**](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/): U-Net 是一种常用于图像分割任务的卷积神经网络。它使用跳跃连接，这有助于加快网络训练速度并提高整体准确率。如果必须选择，U-Net 始终是一个**极佳的默认选择**！

+   [**FCN**](/review-fcn-semantic-segmentation-eb8c9b50d2d1)：全卷积网络（FCN）是一个完全卷积的网络，但它[不如 U-Net 深](https://stackoverflow.com/questions/50239795/intuition-behind-u-net-vs-fcn-for-semantic-segmentation)。缺乏深度主要是因为在较高的网络深度下，准确率会下降。这使得它训练更快，但可能不如 U-Net 准确。

+   [**SegNet**](https://arxiv.org/abs/1511.00561)：SegNet 是一种类似于 U-Net 的流行模型架构，并且比 U-Net 使用更少的激活内存。我们在这一系列中也将使用 SegNet。

+   [**视觉 Transformer (ViT)**](https://arxiv.org/abs/2010.11929)：视觉 Transformer 最近因其简单结构和将注意力机制应用于文本、视觉等领域的能力而受到欢迎。与 CNN 相比，视觉 Transformer 在训练和推理时可能更高效，但历史上需要更多数据来训练。我们在这一系列中也将使用 ViT。

![](../Images/616bcf8bc926426b47371e10a626122f.png)

图 4：U-Net 模型架构。来源：[弗莱堡大学，U-Net 的原作者](https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/)。

这些只是众多可以用于图像分割的深度学习模型中的一部分。适合你特定任务的最佳模型将取决于之前提到的因素、具体任务和你自己的实验。

## 选择正确的损失函数

图像分割任务的损失函数选择非常重要，因为它对模型性能有显著影响。可用的损失函数有很多，每种都有其优缺点。图像分割中最流行的损失函数包括：

+   [**交叉熵损失**](https://pytorch.org/docs/stable/generated/torch.nn.CrossEntropyLoss.html)：交叉熵损失是预测概率分布与真实概率分布之间差异的度量。

+   **IoU 损失**：IoU 损失衡量每个类别中预测掩膜与真实掩膜之间的重叠量。IoU 损失惩罚预测或召回性能差的情况。由于定义的 IoU 不是可微分的，因此我们需要稍微调整它以用作损失函数。

+   [**Dice 损失**](https://torchmetrics.readthedocs.io/en/stable/classification/dice.html)：Dice 损失也是衡量预测掩膜与真实掩膜之间重叠量的一种方法。

+   [**Tversky 损失**](https://arxiv.org/abs/1706.05721)：Tversky 损失被提出作为一种稳健的损失函数，可以用于处理不平衡的数据集。

+   [**Focal 损失**](https://pytorch.org/vision/main/generated/torchvision.ops.sigmoid_focal_loss.html#torchvision.ops.sigmoid_focal_loss)：Focal 损失旨在关注难以分类的样本。这对于提高模型在具有挑战性数据集上的表现可能很有帮助。

对于特定任务，最佳损失函数将取决于任务的具体要求。例如，如果准确性更重要，则 IoU 损失或 Dice 损失可能是更好的选择。如果任务存在不平衡，则 Tversky 损失或 Focal 损失可能是较好的选择。使用的具体损失函数可能会影响模型训练时的收敛速度。

损失函数是模型的一个超参数，根据我们观察到的结果使用不同的损失函数可以让我们更快地减少损失，并提高模型的准确性。

**默认**：在这一系列中，我们将使用交叉熵损失，因为在结果未知时，选择它通常是一个很好的*默认*选择。

你可以使用以下资源来了解更多关于损失函数的内容。

1.  [PyTorch 损失函数：终极指南](https://neptune.ai/blog/pytorch-loss-functions)

1.  [Torchvision — 损失](https://pytorch.org/vision/main/ops.html#losses)

1.  [Torchmetrics](https://torchmetrics.readthedocs.io/en/stable/pages/overview.html#metrics-and-differentiability)

让我们详细查看下面定义的 IoU 损失，作为分割任务中交叉熵损失的一个强健替代方案。

## 自定义 IoU 损失

[IoU](/intersection-over-union-iou-calculation-for-evaluating-an-image-segmentation-model-8b22e2e84686) 被定义为交集与并集之比。对于图像分割任务，我们可以通过计算每个类别的像素交集来计算 IoU，这些像素是由模型预测的，并且在实际分割掩码中。

例如，如果我们有 2 个类别：

1.  背景

1.  人物

然后我们可以确定哪些像素被分类为人物，并将其与实际人物像素进行比较，从而计算人物类别的 IoU。同样，我们可以计算背景类别的 IoU。

一旦我们有了这些特定类别的 IoU 度量，我们可以选择对它们进行无权平均，或在平均之前加权，以考虑之前示例中的任何类别不平衡。

按定义的 IoU 度量要求我们为每个度量计算硬标签。这需要使用 argmax() 函数，而该函数不可微分，因此我们不能将此度量用作损失函数。因此，我们用 softmax() 替代硬标签，并使用预测的概率作为软标签来计算 IoU 度量。这将得到一个可微分的度量，我们可以基于此计算损失。因此，有时，在作为损失函数时，IoU 度量也被称为软 IoU 度量。

如果我们有一个在 0.0 和 1.0 之间取值的度量（M），我们可以计算损失（L）如下：

*L = 1 — M*

不过，如果你的指标值在0.0和1.0之间，可以使用另一种技巧将指标转换为损失。计算：

*L = -log(M)*

即计算指标的负对数。这与之前的公式有意义的不同，你可以[在这里]( /why-we-care-about-the-log-loss-50c00c8e777c)和[这里]( /intuition-behind-log-loss-score-4e0c9979680a)了解更多。基本上，这将带来更好的模型学习效果。

![](../Images/02aaeb2051bb8124a8b565eccf1c584f.png)

图6：比较1-P(x)与-log(P(x))产生的损失。来源：作者。

使用IoU作为损失函数也使得损失函数更接近于捕捉我们真正关心的内容。使用评估指标作为损失函数有利有弊。如果你对深入探讨这个领域感兴趣，可以从[这个讨论](https://stats.stackexchange.com/questions/577556/why-not-use-evaluation-metrics-as-the-loss-function)开始。

## 数据增强

为了高效且有效地训练你的模型以获得良好的准确率，需要注意训练数据的数量和种类。所使用的训练数据的选择会显著影响最终模型的准确率，所以如果你希望从这篇文章系列中学到一件事，那就是这一点！

通常情况下，我们会将数据分成3部分，部分之间的大致比例如下所示。

1.  训练 (80%)

1.  验证 (10%)

1.  测试 (10%)

你会在训练集上训练你的模型，在验证集上评估准确率，然后重复这个过程，直到你对报告的指标感到满意。只有在那时你才会在测试集上评估模型，并报告数字。这样做是为了防止任何偏差渗入到模型的架构和训练及评估过程中使用的超参数中。一般来说，你越是根据测试数据的结果来调整设置，你的结果就会越不可靠。因此，我们必须将决策限制在仅基于训练和验证数据集上看到的结果。

在这一系列中，我们不会使用测试数据集。相反，我们将使用我们的测试数据集作为验证数据集，并对测试数据集应用[数据增强](https://en.wikipedia.org/wiki/Data_augmentation)，以便我们总是在稍有不同的数据上验证我们的模型。这种做法有助于防止我们在验证数据集上过拟合决策。这有点像是一个变通方法，我们这样做只是为了方便和作为一种捷径。对于生产模型开发，你应该尽量坚持上述标准方法。

在这一系列中我们将使用的数据集包含3680张图像作为训练集。虽然这可能看起来图像数量很多，但我们希望确保我们的模型不会在这些图像上过拟合，因为我们将对模型进行多个轮次的训练。

在一个训练周期中，我们会在整个训练数据集上训练模型，通常我们会在生产环境中训练模型 60 个周期或更多。在本系列中，我们将只训练模型 20 个周期以加快迭代速度。为了**防止过拟合**，我们将采用一种叫做 [数据增强](https://efficientdlbook.com/#table-of-contents) 的技术，用于从现有输入数据生成新的输入数据。数据增强的基本理念是，如果你稍微更改图像，它对模型来说就像是一张新图像，但可以推测期望的输出是否相同。以下是我们将在本系列中应用的一些数据增强示例。

1.  [随机水平翻转](https://pytorch.org/vision/main/generated/torchvision.transforms.RandomHorizontalFlip.html)

1.  [随机颜色抖动](https://pytorch.org/vision/main/generated/torchvision.transforms.ColorJitter.html)

虽然我们将使用 Torchvision 库来应用上述数据增强，但我们也鼓励你评估 Albumentations 数据增强库在视觉任务中的应用。两个库都提供了丰富的图像数据变换选项。我们个人继续使用 Torchvision，因为这是我们最初选择的库。[Albumentations](https://albumentations.ai/) 支持更丰富的数据增强原语，可以同时对输入图像及其真实标签或掩码进行更改。例如，如果你需要调整图像大小或翻转图像，你也需要对真实分割掩码进行相应的更改。Albumentations 可以直接为你完成这些操作。

广义而言，这两个库都支持对图像应用的变换，这些变换可以是像素级别的，也可以改变图像的空间维度。像素级变换被 torchvision 称为颜色变换，而空间变换被称为几何变换。

接下来，我们将看到 Torchvision 和 Albumentations 库对像素级和几何变换的应用示例。

![](../Images/61329a6abccd37e7631dcc683568d326.png)

图 7：使用 Albumentations 对图像应用的像素级数据增强示例。来源：[Albumentations](https://albumentations.ai/)

![](../Images/5c490cff06d9218c578606c1cb6e3a7e.png)

图 8：使用 Torchvision 变换对图像应用的数据增强示例。来源：作者 ([notebook](https://www.kaggle.com/code/dhruv4930/starter-for-oxford-iiit-pet-using-torchvision/notebook?scriptVersionId=129839935))

![](../Images/7e9741d85b50584037b6a25aa71e91e0.png)

图 9：使用 Albumentations 应用的空间级变换示例。来源：作者 ([notebook](https://www.kaggle.com/code/dhruv4930/starter-for-oxford-iiit-pet-using-torchvision/notebook?scriptVersionId=129839935))

## 评估模型性能

在评估模型的性能时，你会想要了解它在代表模型在真实数据上的表现质量的指标上的表现。例如，对于图像分割任务，我们想知道模型在预测像素的正确类别方面的准确度。因此，我们称[像素准确率](https://torchmetrics.readthedocs.io/en/stable/classification/accuracy.html)为该模型的验证指标。

你可以将评估指标用作损失函数（为什么不优化你真正关心的东西！），只不过这[可能并不总是可行的](https://jonathan-sands.com/metric/loss/2021/05/13/Metric-vs-Loss.html#what-is-a-loss-function)。

除了[准确率](https://torchmetrics.readthedocs.io/en/stable/classification/accuracy.html)，我们还将跟踪IoU指标（也称为[Jaccard指数](https://torchmetrics.readthedocs.io/en/stable/classification/jaccard_index.html)），以及我们上面定义的自定义IoU指标。

要了解更多适用于图像分割任务的各种准确率指标，请参见：

+   [所有分割指标 — Kaggle](https://www.kaggle.com/code/yassinealouini/all-the-segmentation-metrics)

+   [如何评估图像分割模型](/how-accurate-is-image-segmentation-dd448f896388)

+   [评估图像分割模型](/evaluating-image-segmentation-models-1e9bb89a001b)

## 使用像素准确率作为性能指标的缺点

尽管准确率指标可能是衡量图像分割任务表现的一个良好默认选择，但它确实存在自身的缺陷，这些缺陷可能在特定情况下非常重要。

例如，考虑一个图像分割任务，以识别图片中的人的眼睛，并相应地标记这些像素。因此，模型将把每个像素分类为以下之一：

1.  背景

1.  眼睛

假设每张图像中只有1个人，并且98%的像素不对应于眼睛。在这种情况下，模型可以简单地学会将每个像素预测为背景像素，从而在分割任务中实现98%的像素准确率。哇！

![](../Images/01e74d83606404a77323179cb7419753.png)

图10：一个人的面部图像及其眼睛的分割掩码。你可以看到眼睛只占整个图像的很小一部分。来源：改编自[Unsplash](https://unsplash.com/photos/iFgRcqHznqg)

在这种情况下，使用IoU或Dice指标可能是一个更好的选择，因为IoU会捕捉到预测的正确部分，并且不会受到每个类别或类别在原始图像中所占区域的偏见。你甚至可以考虑将IoU或Dice系数按类别作为指标。这可能更好地反映模型在当前任务中的表现。

当仅考虑像素准确性时，[精确度和召回率](https://developers.google.com/machine-learning/crash-course/classification/precision-and-recall)对于我们计算分割掩膜的对象（如上述示例中的眼睛）可以捕捉我们所寻找的细节。

现在我们已经涵盖了图像分割理论基础的大部分内容，让我们绕道讨论一下与实际工作负载的图像分割推理和部署相关的考虑。

## 模型大小和推理延迟

最后但同样重要的是，我们希望确保我们的模型参数数量合理而不多，因为我们需要一个小而高效的模型。我们将在未来的帖子中详细探讨这个方面，讨论如何使用[高效模型架构](https://efficientdlbook.com/#table-of-contents)来减少模型大小。

就推理延迟而言，重要的是模型执行的数学运算（加减运算）的数量。模型大小和加减运算可以通过[torchinfo](https://pypi.org/project/torchinfo/)包显示。虽然加减运算是确定模型延迟的一个很好的代理，但在不同的后端之间，延迟可能会有很大差异。唯一真正确定模型在特定后端或设备上性能的方法是对该设备进行性能分析和基准测试，并使用你预计在生产环境中看到的输入。

```py
from torchinfo import summary
model = nn.Linear(1000, 500)
summary(
  model,
  input_size=(1, 1000),
  col_names=["kernel_size", "output_size", "num_params", "mult_adds"],
  col_width=15,
)
```

输出：

```py
====================================================================================================
Layer (type:depth-idx)                   Kernel Shape    Output Shape    Param #         Mult-Adds
====================================================================================================
Linear                                   --              [1, 500]        500,500         500,500
====================================================================================================
Total params: 500,500
Trainable params: 500,500
Non-trainable params: 0
Total mult-adds (M): 0.50
====================================================================================================
Input size (MB): 0.00
Forward/backward pass size (MB): 0.00
Params size (MB): 2.00
Estimated Total Size (MB): 2.01
====================================================================================================
```

# 进一步阅读

以下文章提供了有关图像分割基础知识的额外信息。如果你喜欢阅读不同观点的内容，请考虑阅读这些文章。

1.  [计算机视觉中的图像分割指南：最佳实践](https://encord.com/blog/image-segmentation-for-computer-vision-best-practice-guide/)

1.  [图像分割简介：深度学习与传统方法 [+示例]](https://www.v7labs.com/blog/image-segmentation-guide#h5)

1.  [图像分割：基础知识和5种关键技术](https://datagen.tech/guides/image-annotation/image-segmentation/)

如果你希望亲自动手处理Oxford IIIT Pet数据集，并使用torchvision和Albumentations进行图像增强，我们提供了一个[在Kaggle上的起始笔记本](https://www.kaggle.com/code/dhruv4930/starter-for-oxford-iiit-pet-using-torchvision/notebook?scriptVersionId=129839935)供你克隆和尝试。本文中的许多图像都是由该笔记本生成的！

# 文章总结

这是我们迄今为止讨论内容的快速回顾。

+   图像分割是一种将图像划分为多个部分的技术（来源：[维基百科](https://en.wikipedia.org/wiki/Image_segmentation)）

+   图像分割任务主要有两种类型：类别（语义）分割和对象（实例）分割。类别分割将图像中的每个像素分配给一个语义类别。对象分割则识别图像中的每个独立对象，并为每个唯一对象分配一个掩膜。

+   在本系列高效图像分割中，我们将使用PyTorch作为深度学习框架，并使用Oxford IIIT Pet数据集。

+   选择合适的深度学习模型进行图像分割时需要考虑许多因素，包括（但不限于）图像分割任务的类型、数据集的大小和复杂性、预训练模型的可用性以及计算资源的情况。一些最受欢迎的图像分割深度学习模型架构包括U-Net、FCN、SegNet和Vision Transformer（ViT）。

+   图像分割任务中损失函数的选择非常重要，因为它可能对模型的性能和训练效率产生重大影响。对于图像分割任务，我们可以使用交叉熵损失、IoU损失、Dice损失或Focal损失（以及其他一些）。

+   数据增强是一种宝贵的技术，用于防止过拟合以及处理训练数据不足的问题。

+   评估模型性能对当前任务至关重要，必须仔细选择这一指标。

+   模型的大小和推理延迟是开发模型时需要考虑的重要指标，尤其是当你打算将其用于实时应用程序，如面部分割或背景噪声去除时。

在[下一篇文章](https://medium.com/p/bed68cadd7c7/)中，我们将讨论如何使用PyTorch从零开始构建一个卷积神经网络（CNN）来对Oxford IIIT Pet数据集进行图像分割。
