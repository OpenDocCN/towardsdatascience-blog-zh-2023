# 使用PyTorch进行高效的图像分割：第3部分

> 原文：[https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-3-3534cf04fb89?source=collection_archive---------7-----------------------#2023-06-27](https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-3-3534cf04fb89?source=collection_archive---------7-----------------------#2023-06-27)

## 深度可分离卷积

[](https://medium.com/@dhruvbird?source=post_page-----3534cf04fb89--------------------------------)[![Dhruv Matani](../Images/d63bf7776c28a29c02b985b1f64abdd3.png)](https://medium.com/@dhruvbird?source=post_page-----3534cf04fb89--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3534cf04fb89--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3534cf04fb89--------------------------------) [Dhruv Matani](https://medium.com/@dhruvbird?source=post_page-----3534cf04fb89--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F63f5d5495279&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-image-segmentation-using-pytorch-part-3-3534cf04fb89&user=Dhruv+Matani&userId=63f5d5495279&source=post_page-63f5d5495279----3534cf04fb89---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3534cf04fb89--------------------------------) · 12分钟阅读 · 2023年6月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3534cf04fb89&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-image-segmentation-using-pytorch-part-3-3534cf04fb89&user=Dhruv+Matani&userId=63f5d5495279&source=-----3534cf04fb89---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3534cf04fb89&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-image-segmentation-using-pytorch-part-3-3534cf04fb89&source=-----3534cf04fb89---------------------bookmark_footer-----------)

在这个四部分系列中，我们将逐步实现图像分割，使用PyTorch中的深度学习技术从零开始。本部分将重点优化我们的CNN基线模型，通过使用深度可分离卷积来减少可训练参数的数量，使模型可以在移动设备和其他边缘设备上部署。

与[Naresh Singh](https://medium.com/@brocolishbroxoli)共同撰写

![](../Images/f29d9d6903f7abb12a21cd463b98a2b2.png)

图1：使用深度可分离卷积而非普通卷积进行图像分割的结果。从上到下依次为输入图像、真实分割掩码和预测分割掩码。来源：作者

# 文章大纲

在本文中，我们将增强我们[早期构建的卷积神经网络（CNN）](https://medium.com/p/bed68cadd7c7/)，以减少网络中可学习参数的数量。识别输入图像中的宠物像素（属于猫、狗、仓鼠等的像素）这一任务保持不变。我们选择的网络将继续是[SegNet](https://arxiv.org/abs/1511.00561)，我们唯一的改变是用深度可分卷积（DSC）替换卷积层。在此之前，我们将深入探讨深度可分卷积的理论与实践，并欣赏这一技术背后的理念。

在本文中，我们将引用来自这个[笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/pets_segmentation/oxford-iiit-pets-segmentation-using-pytorch-segnet-and-depth-wise-separable-convs.ipynb)的代码和结果用于模型训练，以及这个[笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/pets_segmentation/types%20of%20convolutions.ipynb)作为DSC的入门。如果你希望复现结果，你需要一个GPU以确保第一个笔记本在合理的时间内完成运行。第二个笔记本可以在普通的CPU上运行。

# 本系列文章

本系列适合所有深度学习经验水平的读者。如果你想学习深度学习和视觉AI的实践，了解一些扎实的理论和实践经验，你来对地方了！预计将会有4篇文章的系列，内容如下：

1.  [概念与想法](https://medium.com/p/89e8297a0923/)

1.  [基于CNN的模型](https://medium.com/p/bed68cadd7c7/)

1.  **深度可分卷积（本文）**

1.  [基于视觉变换器的模型](https://medium.com/p/6c86da083432/)

# 介绍

让我们从模型大小和计算成本的角度深入探讨卷积。可训练参数的数量是模型大小的一个很好的指标，而张量操作的数量反映了模型的复杂性或计算成本。考虑一个具有n个dₖ x dₖ大小滤波器的卷积层。进一步假设此层处理形状为*m x h x w*的输入，其中*m*是输入通道的数量，而*h*和*w*分别是高度和宽度维度。在这种情况下，卷积层将产生形状为*n x h x w*的输出，如图2所示。我们假设卷积使用*stride=1*。让我们继续评估这一设置的可训练参数和计算成本。

![](../Images/f05ac4a1e2764b6daf7b1c0384ff9c5f.png)

图2：常规卷积滤波器应用于输入以产生输出。假设stride=1和padding=dₖ-2。来源：[高效深度学习书](https://efficientdlbook.com/)

**可训练参数的评估：** 我们有 *n* 个滤波器，每个滤波器都有 *m x dₖ x dₖ* 个可训练参数。这总共会有 *n x m x dₖ x dₖ* 个可训练参数。为了简化讨论，忽略了偏置项。我们来看下面的 PyTorch 代码以验证我们的理解。

```py
import torch
from torch import nn
def num_parameters(m):
return sum([p.numel() for p in m.parameters()])
dk, m, n = 3, 16, 32
print(f"Expected number of parameters: {m * dk * dk * n}")
conv1 = nn.Conv2d(in_channels=m, out_channels=n, kernel_size=dk, bias=False)
print(f"Actual number of parameters: {num_parameters(conv1)}")
```

打印如下内容。

```py
Expected number of parameters: 4608
Actual number of parameters: 4608
```

现在，让我们评估卷积的计算成本。

**计算成本的评估：** 一个形状为 *m x dₖ x dₖ* 的单个卷积滤波器在对尺寸为 *h x w* 的输入进行 *stride=1* 和 *padding=dₖ-2* 操作时，会对每个尺寸为 *dₖ x dₖ* 的图像区域应用卷积滤波器 *h x w* 次，总共 *h x w* 个区域。这导致每个滤波器或输出通道的成本为 *m x dₖ x dₖ x h x w*。由于我们希望计算 n 个输出通道，因此总成本为 *m x dₖ x dₖ x h x n*。让我们使用 torchinfo PyTorch 包来验证这一点。

```py
from torchinfo import summary
h, w = 128, 128
print(f"Expected total multiplies: {m * dk * dk * h * w * n}")
summary(conv1, input_size=(1, m, h, w))
```

将打印如下内容。

```py
Expected total multiplies: 75497472

==========================================================================================
Layer (type:depth-idx)                   Output Shape              Param #
==========================================================================================
Conv2d                                   [1, 32, 128, 128]         4,608
==========================================================================================
Total params: 4,608
Trainable params: 4,608
Non-trainable params: 0
Total mult-adds (M): 75.50
==========================================================================================
Input size (MB): 1.05
Forward/backward pass size (MB): 4.19
Params size (MB): 0.02
Estimated Total Size (MB): 5.26
==========================================================================================
```

如果我们暂时忽略卷积层的实现细节，我们会发现，从高层次来看，卷积层只是将 *m x h x w* 的输入转换为 *n x h x w* 的输出。这一转换是通过可训练的滤波器实现的，滤波器在“看到”输入时逐步学习特征。接下来的问题是：是否可以使用更少的可训练参数来实现这一转换，同时确保层的学习能力不会受到最小妥协？深度可分离卷积旨在回答这个确切的问题。让我们详细了解它们，并学习它们在我们评估指标上的表现。

# 深度可分离卷积

深度可分离卷积（DSC）的概念最早由 Laurent Sifre 在其博士论文《[刚性运动散射用于图像分类](https://www.di.ens.fr/data/publications/papers/phd_sifre.pdf)》中提出。从那时起，它们在各种流行的深度卷积网络中成功应用，例如 [XceptionNet](https://arxiv.org/abs/1610.02357) 和 [MobileNet](/review-mobilenetv1-depthwise-separable-convolution-light-weight-model-a382df364b69)。

普通卷积与 DSC 之间的主要区别在于 DSC 由 2 个卷积组成，如下所述：

1.  **深度分组卷积**，其中输入通道 m 的数量等于输出通道的数量，使得每个输出通道仅受单个输入通道的影响。在 PyTorch 中，这被称为“分组”卷积。你可以在 PyTorch 的 [这里](https://pytorch.org/docs/stable/generated/torch.nn.Conv2d.html) 阅读更多关于分组卷积的信息。

1.  **逐点卷积**（滤波器大小=1），其工作方式类似于普通卷积，每个 n 个滤波器作用于所有 m 个输入通道，以产生单个输出值。

![](../Images/0f229cb577579524f10f7960aaa3e0b3.png)

图 3：深度可分卷积滤波器应用于输入以生成输出。假设 *stride=1* 和 *padding=dₖ-2*。来源：[高效深度学习书](https://efficientdlbook.com/)

让我们对 DSC 进行与常规卷积相同的操作，计算可训练参数和计算量。

**可训练参数的评估：** “分组”卷积有 m 个滤波器，每个滤波器有 *dₖ x dₖ* 个可学习的参数，生成 m 个输出通道。这导致总共 *m x dₖ x dₖ* 个可学习的参数。点卷积有 n 个大小为 *m x 1 x 1* 的滤波器，合计为 *n x m x 1 x 1* 个可学习的参数。让我们查看下面的 PyTorch 代码以验证我们的理解。

```py
class DepthwiseSeparableConv(nn.Sequential):
    def __init__(self, chin, chout, dk):
        super().__init__(
            # Depthwise convolution
            nn.Conv2d(chin, chin, kernel_size=dk, stride=1, padding=dk-2, bias=False, groups=chin),
            # Pointwise convolution
            nn.Conv2d(chin, chout, kernel_size=1, bias=False),
        )

conv2 = DepthwiseSeparableConv(chin=m, chout=n, dk=dk)
print(f"Expected number of parameters: {m * dk * dk + m * 1 * 1 * n}")
print(f"Actual number of parameters: {num_parameters(conv2)}")
```

将打印。

```py
Expected number of parameters: 656
Actual number of parameters: 656
```

我们可以看到，DSC 版本的参数大约少 *7x*。接下来，让我们关注 DSC 层的计算成本。

**计算成本的评估：** 假设我们的输入空间维度为 *m x h x w*。在 DSC 的分组卷积部分，我们有 **m** 个大小为 *dₖ x dₖ* 的滤波器。一个滤波器应用于其对应的输入通道，结果是 *m x dₖ x dₖ x h x w* 的段成本。对于点卷积，我们应用 **n** 个大小为 *m x 1 x 1* 的滤波器以生成 **n** 个输出通道。这导致 *n x m x 1 x 1 x h x w* 的段成本。我们需要将分组和点卷积操作的成本加起来计算总成本。让我们使用 [torchinfo](https://pypi.org/project/torchinfo/) PyTorch 包验证这一点。

```py
print(f"Expected total multiplies: {m * dk * dk * h * w + m * 1 * 1 * h * w * n}")
s2 = summary(conv2, input_size=(1, m, h, w))
print(f"Actual multiplies: {s2.total_mult_adds}")
print(s2)
```

将打印。

```py
Expected total multiplies: 10747904
Actual multiplies: 10747904
==========================================================================================
Layer (type:depth-idx)                   Output Shape              Param #
==========================================================================================
DepthwiseSeparableConv                   [1, 32, 128, 128]         --
├─Conv2d: 1-1                            [1, 16, 128, 128]         144
├─Conv2d: 1-2                            [1, 32, 128, 128]         512
==========================================================================================
Total params: 656
Trainable params: 656
Non-trainable params: 0
Total mult-adds (M): 10.75
==========================================================================================
Input size (MB): 1.05
Forward/backward pass size (MB): 6.29
Params size (MB): 0.00
Estimated Total Size (MB): 7.34
==========================================================================================
```

让我们通过一些示例比较两种卷积的大小和成本，以获得一些直观的理解。

## 常规卷积与深度可分卷积的大小和成本比较

为了比较常规卷积和深度可分卷积的大小和成本，我们将假设输入大小为 *128 x 128*，卷积核大小为 *3 x 3*，网络逐步将空间维度减半并将通道维度加倍。我们假设每一步有一个 2d-conv 层，但实际上可能会有更多。

![](../Images/4646694c069f37f3ed9be0287e6d4d40.png)

图 4：比较常规卷积和深度可分卷积的可训练参数（大小）和多次加法（成本）。我们还展示了两种卷积的大小和成本的比例。来源：作者。

你可以看到，平均而言，DSC 的大小和计算成本大约是上述配置常规卷积成本的 11% 到 12%。

![](../Images/12addf86c5b96f25ef62b5277e3360ea.png)

图 5：常规卷积与深度可分卷积的相对大小和成本。来源：作者。

现在我们已经对各种卷积类型及其相对成本有了很好的理解，你一定在想使用DSC是否有任何缺点。到目前为止，我们看到的一切似乎都表明它们在各方面都更好！不过，我们还没有考虑一个重要方面，即它们对模型准确性的影响。让我们通过下面的实验来探讨这个问题。

# 使用深度可分离卷积的SegNet

[这个笔记本](https://github.com/dhruvbird/ml-notebooks/blob/main/pets_segmentation/oxford-iiit-pets-segmentation-using-pytorch-segnet-and-depth-wise-separable-convs.ipynb)包含了本节的所有代码。

我们将从[之前的帖子](https://medium.com/p/bed68cadd7c7/)中调整我们的SegNet模型，并将所有常规卷积层替换为DSC层。这样做后，我们发现笔记本中的参数数量从15.27M减少到1.75M，减少了88.5%！这与我们之前估计的网络可训练参数减少11%到12%的一致。

在模型训练和验证过程中使用了与[之前](https://medium.com/p/bed68cadd7c7/)相似的配置。配置如下所示。

1.  *随机水平翻转*和*颜色抖动*数据增强方法被应用于训练集，以防止过拟合。

1.  图像在进行不保持长宽比的调整操作时被调整为128x128像素。

1.  图像没有进行输入归一化处理，而是[使用批量归一化层作为模型的第一层](/replace-manual-normalization-with-batch-normalization-in-vision-ai-models-e7782e82193c)。

1.  模型使用Adam优化器进行20个周期的训练，学习率为0.001，并且没有学习率调度器。

1.  交叉熵损失函数用于将像素分类为宠物、背景或宠物边界。

该模型在20个训练周期后达到了86.96%的验证准确率。这低于使用常规卷积在相同训练周期内达到的88.28%准确率。我们已经通过实验确定，训练更多周期会提高两个模型的准确性，因此20个周期绝对不是训练周期的结束。为了本文章的演示目的，我们在20个周期后停止训练。

我们绘制了一个gif，展示了模型如何学习预测验证集中21张图像的分割掩码。

![](../Images/aa9aec7b78d408275a886ec1f9bcafc1.png)

图6：一个gif展示了使用DSC的SegNet模型如何学习预测验证集中21张图像的分割掩码。来源：作者

现在我们已经看到模型如何通过训练周期的进展，让我们比较一下使用常规卷积和DSC的模型的训练周期。

## 准确性比较

我们发现查看使用常规卷积和DSC的模型训练周期很有用。我们注意到的主要区别是在训练的早期阶段（周期），之后两种模型大致趋于相同的预测流程。实际上，在训练了100个周期后，我们发现使用DSC的模型准确性比使用常规卷积的模型低约1%。这与我们从仅20个周期的训练中观察到的结果一致。

![](../Images/e1142979b7b9b8d83e1a5eac150d630c.png)

图7：一个动图展示了使用常规卷积与DSC的SegNet模型预测的分割掩码进展。来源：作者。

你可能会注意到，两种模型在仅经过6个训练周期后预测大致正确——即可以直观地看到模型预测了一些有用的东西。训练模型的大部分艰巨工作在于确保预测掩码的边界尽可能紧密，并尽可能接近图像中的实际物体。这意味着尽管在后续训练周期中，准确性的绝对提升可能较少，但这对预测质量的影响要大得多。我们注意到，在较高的绝对准确性值（例如从89%提升到90%）下，准确性的单个位数提升会显著改善预测质量。

## 与UNet模型的比较

我们进行了一个实验，调整了很多超参数，重点是提高整体准确性，以了解这种设置与最优设置的接近程度。以下是该实验的配置。

1.  图像大小：128 x 128——与之前的实验相同

1.  训练周期：100——当前实验训练了20个周期

1.  数据增强：增加了很多数据增强技术，例如图像旋转、通道丢弃、随机块移除。我们使用了Albumentations而不是torchvision transforms。Albumentations会自动为我们转换分割掩码。

1.  学习率调度器：使用了StepLR调度器，每25个训练周期衰减0.8倍

1.  损失函数：我们尝试了4种不同的损失函数：交叉熵、焦点损失、Dice损失、加权交叉熵。Dice损失表现最差，而其他损失函数则相差无几。实际上，经过100个周期后，其他损失函数的最佳准确性差异在小数点后第四位（假设准确性在0.0到1.0之间）。

1.  卷积类型：常规

1.  模型类型：UNet——当前实验使用了SegNet模型

我们在上述设置下取得了91.3%的最佳验证准确性。我们注意到图像大小对最佳验证准确性有显著影响。例如，当我们将图像大小更改为256 x 256时，最佳验证准确性上升到93.0%。然而，训练时间大大增加，并且使用了更多内存，这意味着我们不得不减少批量大小。

![](../Images/adf9fa994f9857ab5aaa8a0ce9b761c0.png)

图 8：使用上述超参数训练 100 个训练周期的 UNet 模型的结果。来源：作者。

你会发现，预测结果相比之前的要更平滑、更清晰。

# 结论

在本系列第 3 部分，我们了解了深度可分离卷积（DSC）作为一种在不显著降低验证准确性的情况下减少模型大小和训练/推理成本的技术。我们了解了在特定设置下，常规卷积与 DSC 之间的大小/成本权衡。

我们展示了如何在 PyTorch 中调整 SegNet 模型以使用 DSC。这项技术可以应用于任何深度 CNN。事实上，我们可以选择性地用 DSC 替换一些卷积层 —— 即我们不需要全部替换。选择替换哪些层将取决于你希望在模型大小/运行成本和预测准确性之间达成的平衡。这个决定将取决于你的具体使用案例和部署设置。

虽然本文训练了模型 20 个周期，但我们解释了这对于生产工作负载来说是不足够的，并且提供了如果训练更多周期会得到什么的初步了解。此外，我们介绍了一些在模型训练过程中可以调整的超参数。虽然这个列表并不全面，但它应能让你理解训练一个用于生产工作的图像分割模型所需的复杂性和决策过程。

在[本系列的下一部分](https://medium.com/p/6c86da083432/)，我们将探讨 Vision Transformers，以及如何利用这种模型架构来执行宠物分割任务的图像分割。

# 参考文献和进一步阅读

+   [高效深度学习书籍第04章 — 高效架构](https://github.com/EfficientDL/book/raw/main/book/%5BEDL%5D%20Chapter%204%20-%20Efficient%20Architectures.pdf)

+   [可分离卷积基础介绍](/a-basic-introduction-to-separable-convolutions-b99ec3102728)
