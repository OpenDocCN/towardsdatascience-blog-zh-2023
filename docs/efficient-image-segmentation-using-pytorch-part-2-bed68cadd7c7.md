# 使用PyTorch进行高效图像分割：第二部分

> 原文：[https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-2-bed68cadd7c7?source=collection_archive---------4-----------------------#2023-06-27](https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-2-bed68cadd7c7?source=collection_archive---------4-----------------------#2023-06-27)

## 基于CNN的模型

[](https://medium.com/@dhruvbird?source=post_page-----bed68cadd7c7--------------------------------)[![Dhruv Matani](../Images/d63bf7776c28a29c02b985b1f64abdd3.png)](https://medium.com/@dhruvbird?source=post_page-----bed68cadd7c7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bed68cadd7c7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bed68cadd7c7--------------------------------) [Dhruv Matani](https://medium.com/@dhruvbird?source=post_page-----bed68cadd7c7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F63f5d5495279&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-image-segmentation-using-pytorch-part-2-bed68cadd7c7&user=Dhruv+Matani&userId=63f5d5495279&source=post_page-63f5d5495279----bed68cadd7c7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bed68cadd7c7--------------------------------) ·11分钟阅读·2023年6月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbed68cadd7c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-image-segmentation-using-pytorch-part-2-bed68cadd7c7&user=Dhruv+Matani&userId=63f5d5495279&source=-----bed68cadd7c7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbed68cadd7c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-image-segmentation-using-pytorch-part-2-bed68cadd7c7&source=-----bed68cadd7c7---------------------bookmark_footer-----------)

这是一个4部分系列中的第二部分，旨在使用PyTorch中的深度学习技术一步步实现图像分割。本部分将重点介绍实现一个基线图像分割卷积神经网络（CNN）模型。

与[Naresh Singh](https://medium.com/@brocolishbroxoli)共同撰写

![](../Images/71d6ffa1c9d4d8f16cc63e2c2f47ce90.png)

图1：使用CNN进行图像分割的结果。从上到下依次为输入图像、真实分割掩码、预测的分割掩码。来源：作者

# 文章大纲

在本文中，我们将实现一种基于[卷积神经网络（CNN）](https://en.wikipedia.org/wiki/Convolutional_neural_network)的架构，称为[SegNet](https://arxiv.org/abs/1511.00561)，它将为输入图像中的每个像素分配相应的宠物标签，例如猫或狗。那些不属于任何宠物的像素将被归类为背景像素。我们将使用[Oxford Pets 数据集](https://www.robots.ox.ac.uk/~vgg/data/pets/)和[PyTorch](https://pytorch.org/)来构建和训练此模型，以便深入了解成功完成图像分割任务所需的内容。模型构建过程将是动手操作的，我们将详细讨论模型中每一层的作用。文章中将包含大量研究论文和文章的参考资料以供进一步学习。

在本文中，我们将参考来自这个[笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/pets_segmentation/oxford-iiit-pets-segmentation-using-pytorch-segnet-and-depth-wise-separable-convs.ipynb)的代码和结果。如果你希望复现结果，你需要一个GPU，以确保笔记本在合理的时间内完成运行。

# 本系列文章

本系列适用于所有深度学习经验水平的读者。如果你想了解深度学习和视觉AI的实践，同时获得一些扎实的理论和实践经验，你来对地方了！预计这是一个四部分的系列，包含以下文章：

1.  [概念和思想](https://medium.com/p/89e8297a0923/)

1.  **基于CNN的模型（本文）**

1.  [深度可分离卷积](https://medium.com/p/3534cf04fb89/)

1.  [基于视觉变换器的模型](https://medium.com/p/6c86da083432/)

让我们从卷积层的简短介绍开始讨论，以及一些通常一起使用的其他层，作为卷积块。

# 卷积-批归一化-ReLU 和 最大池化/反池化

卷积、批归一化、ReLU块是视觉AI的圣三位一体。你会在基于CNN的视觉AI模型中频繁看到它的使用。这些术语分别代表在PyTorch中实现的不同层。卷积层负责对输入张量进行学习到的滤波器的交叉相关操作。批归一化将批中的元素中心化到零均值和单位方差，而ReLU是一个非线性激活函数，只保留输入中的正值。

一个典型的卷积神经网络（CNN）在层叠的过程中逐步减少输入的空间维度。空间维度减少的动机将在下一节讨论。这个减少是通过使用最大值或平均值等简单函数对邻近值进行池化来实现的。我们将在最大池化部分进一步讨论这个问题。在分类问题中，一系列Conv-BN-ReLU-Pool块后面跟着一个分类头，它预测输入属于目标类之一的概率。某些问题集，如语义分割，要求逐像素预测。对于这种情况，会在下采样块后添加一系列上采样块，以将其输出投影到所需的空间维度。上采样块实际上是Conv-BN-ReLU-Unpool块，它用反池化层替换了池化层。我们将在最大池化部分进一步讨论反池化。

现在，让我们进一步阐述卷积层背后的动机。

## 卷积

卷积是视觉AI模型的基本构建块。它们在计算机视觉中被广泛使用，并且历史上被用于实现视觉变换，例如：

1.  边缘检测

1.  图像模糊和锐化

1.  压花

1.  强化

卷积操作是两个矩阵的逐元素乘法和聚合。图2展示了一个卷积操作的例子。

![](../Images/be30104d93750453b246b4c05e9c4998.png)

图2：卷积操作的示意图。来源：作者

在深度学习环境中，卷积是在较大尺寸的输入上进行的，卷积滤波器或内核是一个*n维*参数矩阵。通过在输入上滑动滤波器并对相应部分应用卷积来实现这一点。滑动的范围由步幅参数配置。步幅为一表示内核滑动一步以处理下一个部分。与使用固定滤波器的传统方法不同，深度学习通过反向传播从数据中学习滤波器。

**那么，卷积如何在深度学习中提供帮助？**

在深度学习中，卷积层用于检测视觉特征。一个典型的CNN模型包含这样一系列层。栈底层检测简单特征，如线条和边缘。随着我们向上移动，这些层检测越来越复杂的特征。栈中的中间层检测线条和边缘的组合，而顶层检测复杂的形状，如汽车、面孔或飞机。图3直观地展示了训练模型的顶层和底层的输出。

![](../Images/9886c8e540b981a81ab8ae4da0ce4e96.png)

图3：卷积滤波器学习识别的内容。来源：[用于可扩展无监督层次表示学习的卷积深度置信网络](http://web.eecs.umich.edu/~honglak/icml09-ConvolutionalDeepBeliefNetworks.pdf)

卷积层具有一组可学习的滤波器，这些滤波器作用于输入中的小区域，为每个区域产生一个代表性输出值。例如，一个3x3的滤波器在一个3x3大小的区域上操作，并产生一个代表该区域的值。对输入区域重复应用滤波器会产生一个输出，该输出成为堆栈中下一层的输入。直观地说，越高层的层可以“看到”输入的更大区域。例如，第二个卷积层中的3x3滤波器在第一个卷积层的输出上操作，其中每个单元格包含输入中3x3大小区域的信息。如果我们假设卷积操作的步长为1，那么第二层中的滤波器将“看到”原始输入的5x5大小区域。这被称为卷积的[感受野](https://theaisummer.com/receptive-field/)。卷积层的重复应用逐渐减少输入图像的空间尺寸，并增加滤波器的视野，使它们能够“看到”复杂的形状。图4展示了卷积网络对1-D输入的处理过程。输出层中的一个元素是相对较大输入块的代表。

![](../Images/3302985b5a8d105588b693f168f7581a.png)

图4：卷积的1维感受野，核大小为3，应用3次。假设步长=1且无填充。经过第三次连续应用卷积核后，单个像素能够看到原始输入图像中的7个像素。来源：作者

一旦卷积层能够检测到这些对象并生成它们的表示，我们就可以将这些表示用于图像分类、图像分割以及对象检测和定位。广义上说，CNN遵循以下一般原则：

1.  卷积层要么保持输出通道数（©）不变，要么将其加倍。

1.  使用步长=1保持空间尺寸不变，或使用步长=2将其减少一半。

1.  对卷积块的输出进行池化以改变图像的空间尺寸是很常见的。

卷积层将核独立地应用于每个输入。这可能导致其输出对不同输入变化。批量归一化层通常跟在卷积层后面，以解决这个问题。让我们在下一节详细了解其作用。

## 批量归一化

批量归一化层将批输入中的通道值标准化为零均值和单位方差。这种标准化是针对批中每个通道独立进行的，以确保输入的通道值具有相同的分布。批量归一化具有以下好处：

1.  它通过防止梯度变得过小来稳定训练过程。

1.  它在我们的任务上实现了更快的收敛速度。

如果我们只有一堆卷积层，这实际上等同于一个单卷积层网络，因为线性变换的级联效应。换句话说，一系列线性变换可以用一个具有相同效果的单一线性变换来替代。直观上，如果我们用一个常数*k₁*乘以一个向量，再乘以另一个常数*k₂*，这等同于用常数*k₁k₂*进行一次乘法。因此，为了使网络实际具有深度，它们必须具有非线性以防止其*崩溃*。我们将在下一节中讨论ReLU，它通常用作非线性函数。

## ReLU

ReLU是一个简单的非线性激活函数，它将最低输入值剪裁为大于或等于0。它还帮助解决梯度消失问题，将输出限制为大于或等于0。ReLU层通常后接一个池化层，以在下采样子网络中缩小空间维度，或者一个反池化层，以在上采样子网络中扩大空间维度。详细信息将在下一节中提供。

## 池化

池化层用于缩小输入的空间维度。使用*stride=2*的池化将输入的空间维度从(H, W)转换为(H/2, W/2)。[最大池化](https://pytorch.org/docs/stable/generated/torch.nn.MaxPool2d.html)是深度CNN中最常用的池化技术。它将2x2网格中的最大值投影到输出上。然后，我们根据与卷积类似的步幅滑动2x2池化窗口到下一个区域。重复此过程，使用*stride=2*的结果是输出的高度和宽度都是输入的一半。另一种常用的池化层是[平均池化](https://pytorch.org/docs/stable/generated/torch.nn.AvgPool2d.html#torch.nn.AvgPool2d)层，它计算平均值而不是最大值。

池化层的反向操作称为反池化层。它将一个(H, W)维度的输入转换为一个(2H, 2W)维度的输出，适用于*stride=2*。这一转换的必要成分是选择在输出的2x2区域中投影输入值的位置。为此，我们需要一个[max-unpooling](https://pytorch.org/docs/stable/generated/torch.nn.MaxUnpool2d.html#torch.nn.MaxUnpool2d)索引图，它告诉我们输出区域中的目标位置。这个反池化图是由之前的最大池化操作生成的。图5展示了池化和反池化操作的示例。

![](../Images/e30783dc4a997c5fc7e750d938b02a0e.png)

图5：最大池化和反池化。来源：[DeepPainter: Painter Classification Using Deep Convolutional Autoencoders](https://www.researchgate.net/publication/306081538_DeepPainter_Painter_Classification_Using_Deep_Convolutional_Autoencoders)

我们可以将最大池化视为一种非线性激活函数。然而，*据报道*，[使用它来替代如 ReLU 这样的非线性函数会影响网络性能](https://arxiv.org/pdf/1606.02228.pdf)。相比之下，平均池化不能被视为非线性函数，因为它使用所有输入生成一个线性组合的输出。

这涵盖了深度 CNN 的所有基本构建块。现在，让我们将它们组合起来创建一个模型。我们为这次练习选择的模型称为 SegNet。接下来我们将讨论它。

# SegNet: 一种基于 CNN 的模型

[SegNet](https://arxiv.org/abs/1511.00561) 是一个基于本文讨论的基本块的深度 CNN 模型。它有两个不同的部分。底部部分，也称为编码器，进行下采样以生成代表输入的特征。顶部解码器部分上采样特征以进行逐像素分类。每个部分由一系列 Conv-BN-ReLU 块组成。这些块还在下采样和上采样路径中分别包含池化或反池化层。图 6 显示了层的更详细排列。SegNet 使用编码器中的最大池化操作的池化索引来确定在解码器中的最大反池化操作期间复制哪些值。虽然激活张量的每个元素是 4 字节（32 位），但在 2x2 的方块内可以仅用 2 位来存储偏移量。这在内存使用方面更有效，因为这些激活（或 SegNet 中的索引）在模型运行时需要被存储。

![](../Images/c2781431e13509544ca760404da8b1f5.png)

图 6: SegNet 模型架构用于图像分割。来源: SegNet: [用于图像分割的深度卷积编码器-解码器架构](https://arxiv.org/pdf/1511.00561.pdf)

[此笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/pets_segmentation/oxford-iiit-pets-segmentation-using-pytorch-segnet-and-depth-wise-separable-convs.ipynb)包含本节的所有代码。

该模型具有 15.27M 可训练参数。

在模型训练和验证期间使用了以下配置。

1.  *随机水平翻转* 和 *颜色抖动* 数据增强被应用于训练集以防止过拟合

1.  图像在不保持纵横比的调整操作中被调整为 128x128 像素

1.  对图像没有应用输入标准化；而是使用了 [作为模型第一层的批量归一化层](/replace-manual-normalization-with-batch-normalization-in-vision-ai-models-e7782e82193c)

1.  模型使用 Adam 优化器训练 20 个周期，学习率为 0.001，并且使用 StepLR 调度器，每 7 个周期将学习率衰减 0.7。

1.  交叉熵损失函数用于将像素分类为属于宠物、背景或宠物边界。

经过20个训练轮次，模型达到了88.28%的验证准确率。

我们绘制了一个 gif，展示了模型如何学习预测验证集21张图像的分割掩码。

![](../Images/f0bfdb501845cc50d9025cebf77968ba.png)

图6：一个 gif，展示了 SegNet 模型如何学习预测验证集21张图像的分割掩码。来源：作者

所有验证指标的定义在[本系列的第1部分](https://medium.com/p/89e8297a0923/)中描述。

如果你想查看一个使用 Tensorflow 实现的用于分割宠物图像的全卷积模型，请参阅[《高效深度学习书》](https://efficientdlbook.com/#table-of-contents)的第4章：高效架构。

## 模型学习的观察

根据经过训练的模型在每个轮次后的预测发展情况，我们可以观察到以下几点。

1.  模型能够在仅经过1个训练轮次时，就学会使输出在图像中的宠物位置上看起来正确。

1.  边界像素更难以分割，因为我们使用的是一个未加权的损失函数，该函数对每次成功（或失败）的处理是一样的，因此，边界像素的错误对模型的损失影响不大。我们鼓励你研究这个问题，并查看你可以尝试哪些策略来解决它。试试使用[Focal Loss](/focal-loss-a-better-alternative-for-cross-entropy-1d073d92d075)并看看效果如何。

1.  即使在20个训练轮次后，模型似乎仍在学习。这表明，如果我们训练模型更长时间，可能会提高验证准确率。

1.  有些真实标签本身也很难确定——例如，中间一行最后一列的狗的掩码在狗的身体被植物遮挡的区域有很多未知像素。这对模型来说很难确定，因此对于这样的示例，通常会有准确率损失。然而，这并不意味着模型表现不好。除了查看整体验证指标之外，还应随时检查预测，以了解模型的行为。

![](../Images/7de58141bb2a950e94576c23ceddc981.png)

图7：一个包含大量未知像素的真实分割掩码示例。这对任何机器学习模型来说都是一个非常困难的输入。来源：作者

# 结论

在本系列的第2部分中，我们学习了深度卷积神经网络（CNN）在视觉AI中的基本构建块。我们展示了如何从零开始在PyTorch中实现SegNet模型，并可视化了模型在连续训练周期上的表现，这有助于你理解模型如何迅速学习到足够的知识以使输出大致接近正确范围。在这种情况下，我们可以看到分割掩码在第一次训练周期时就大致类似于实际的分割掩码！

在[本系列的下一部分](https://medium.com/p/3534cf04fb89/)，我们将探讨如何优化我们的模型以实现设备上的推理，并减少可训练参数的数量（从而减少模型大小），同时保持验证精度大致不变。

# 进一步阅读

在这里阅读更多关于卷积的内容：

1.  由约瑟夫·雷德蒙教授在华盛顿大学讲授的课程 “[计算机视觉的古老秘密](https://pjreddie.com/courses/computer-vision/)” 提供了一套关于卷积（特别是第4、5和13章）的优秀视频，我们强烈推荐观看。

1.  [深度学习中的卷积算术指南](https://arxiv.org/abs/1603.07285)（强烈推荐）

1.  [https://towardsdatascience.com/computer-vision-convolution-basics-2d0ae3b79346](/computer-vision-convolution-basics-2d0ae3b79346)

1.  [PyTorch中的Conv2d层（文档）](https://pytorch.org/docs/stable/generated/torch.nn.Conv2d.html#torch.nn.Conv2d)

1.  [卷积学习了什么？](https://www.kdnuggets.com/2016/11/intuitive-explanation-convolutional-neural-networks.html/3)

1.  [卷积可视化工具](https://ezyang.github.io/convolution-visualizer/index.html)

在这里阅读更多关于批量归一化的内容：

1.  [批量归一化：维基百科](https://en.wikipedia.org/wiki/Batch_normalization)

1.  [批量归一化：机器学习大师](https://machinelearningmastery.com/batch-normalization-for-training-of-deep-neural-networks/)

1.  [PyTorch中的BatchNorm2d层](https://pytorch.org/docs/stable/generated/torch.nn.BatchNorm2d.html)。

在这里阅读更多关于激活函数和ReLU的内容：

1.  [ReLU：机器学习大师](https://machinelearningmastery.com/rectified-linear-activation-function-for-deep-learning-neural-networks/)

1.  [ReLU：维基百科](https://en.wikipedia.org/wiki/Rectifier_(neural_networks))

1.  [ReLU：Quora](https://www.quora.com/Why-is-ReLU-used-so-much-in-neural-networks)

1.  [PyTorch中的ReLU API](https://pytorch.org/docs/stable/generated/torch.nn.ReLU.html)
