# 使用Python处理MRI和深度学习

> 原文：[https://towardsdatascience.com/dealing-with-mri-and-deep-learning-with-python-c88f3dae0620?source=collection_archive---------3-----------------------#2023-12-20](https://towardsdatascience.com/dealing-with-mri-and-deep-learning-with-python-c88f3dae0620?source=collection_archive---------3-----------------------#2023-12-20)

## 使用PyTorch的深度学习模型进行MRI分析的综合指南

[](https://medium.com/@carlapitarch?source=post_page-----c88f3dae0620--------------------------------)[![Carla Pitarch Abaigar](../Images/f0f7f963947f59399c8b3e6ac9d9aac9.png)](https://medium.com/@carlapitarch?source=post_page-----c88f3dae0620--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c88f3dae0620--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c88f3dae0620--------------------------------) [Carla Pitarch Abaigar](https://medium.com/@carlapitarch?source=post_page-----c88f3dae0620--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2cd70d9ae7e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-mri-and-deep-learning-with-python-c88f3dae0620&user=Carla+Pitarch+Abaigar&userId=f2cd70d9ae7e&source=post_page-f2cd70d9ae7e----c88f3dae0620---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c88f3dae0620--------------------------------) ·13分钟阅读·2023年12月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc88f3dae0620&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-mri-and-deep-learning-with-python-c88f3dae0620&user=Carla+Pitarch+Abaigar&userId=f2cd70d9ae7e&source=-----c88f3dae0620---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc88f3dae0620&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-mri-and-deep-learning-with-python-c88f3dae0620&source=-----c88f3dae0620---------------------bookmark_footer-----------)![](../Images/c7797e5acea961e4be445d2986a13b7b.png)

图片来源：[Olga Rai](https://stock.adobe.com/es/contributor/209778624/olga-rai?load_type=author&prev_url=detail) 于 [Adobe Stock](https://stock.adobe.com/)。

# 介绍

首先，我想介绍一下自己。我叫Carla Pitarch，是一名人工智能（AI）博士候选人。我的研究重点是通过使用深度学习（DL）模型，特别是卷积神经网络（CNNs），从磁共振图像（MRI）中提取信息，以开发自动化的脑肿瘤分级分类系统。

在我的博士研究之初，深入研究MRI数据和DL是一个全新的领域。在这个领域中运行模型的初步步骤并不像预期的那样简单。尽管花了一些时间在这个领域进行研究，我发现缺乏全面的资源来指导MRI和DL的入门。因此，我决定分享一些我在这一期间获得的知识，希望它能让你的旅程更加顺利。

通过DL进行计算机视觉（CV）任务通常涉及使用标准公共图像数据集，如`[ImageNe](https://image-net.org/about.php)t`，这些数据集以3通道RGB自然图像为特征。PyTorch模型为这些规格做好了准备，期望输入图像为这种格式。然而，当我们的图像数据来自不同的领域，如医疗领域，与这些自然图像数据集在格式和特征上都有所不同时，就会带来挑战。本文深入探讨了这个问题，强调了在模型实施之前的两个关键准备步骤：使数据与模型的要求对齐，并准备模型以有效处理我们的数据。

# 背景

让我们首先简要概述一下CNNs和MRI的基本方面。

## 卷积神经网络

在本节中，我们将深入探讨CNNs领域，假设读者对深度学习（DL）有基本的理解。CNNs作为计算机视觉（CV）中的黄金标准架构，专注于处理2D和3D输入图像数据。我们在这篇文章中的重点将集中在2D图像数据的处理上。

图像分类，将输出类别或标签与输入图像关联，是卷积神经网络（CNNs）的核心任务。由LeCun等人于1989年提出的开创性LeNet5架构为CNNs奠定了基础。该架构可以总结如下：

![](../Images/5559d225d2df07a468c8e1431111710a.png)

CNN架构包含两个卷积层、两个池化层，以及一个位于输出层之前的全连接层。

2D CNN架构通过接收图像像素作为输入来操作，期望图像是一个形状为`Height x Width x Channels`的张量。彩色图像通常包含3个通道：红色、绿色和蓝色（RGB），而灰度图像则包含一个通道。

CNNs中的一个基本操作是*卷积*，通过在输入数据的所有区域应用一组*滤波器*或*内核*来执行。下图展示了卷积在2D上下文中的工作原理。

![](../Images/b603b70f3daaf76704842d9beeb4c056.png)

一个对5x5图像进行3x3滤波器卷积的示例，生成一个3x3卷积特征。

这个过程涉及将滤波器滑过图像并计算加权和，以获得一个卷积特征图。输出将表示输入图像的该位置是否识别到特定的视觉模式，例如边缘。每个卷积层后，激活函数会引入非线性。常见的选择包括：ReLU（修正线性单元）、Leaky ReLU、Sigmoid、Tanh 和 Softmax。有关每个激活函数的更多详细信息，本文提供了清晰的解释 [Activation Functions in Neural Networks | by SAGAR SHARMA | Towards Data Science](/activation-functions-neural-networks-1cbd9f8d91d6)。

不同类型的层对 CNNs 的构建做出贡献，每一层在定义网络功能方面扮演着独特的角色。除了卷积层，CNNs 中还包括几种其他显著的层：

+   *池化层*，如最大池化或平均池化，有效地减少特征图维度，同时保留关键信息。

+   *Dropout 层* 通过在训练期间随机停用部分神经元来防止过拟合，从而增强网络的泛化能力。

+   *批量归一化层* 关注对每层的输入进行标准化，从而加快网络训练速度。

+   *全连接（FC）层* 在一个层中的所有神经元和前一层中的所有激活之间建立连接，整合学到的特征以促进最终分类。

CNNs 通过层次化的方式学习识别模式。初始层关注低级特征，逐渐移动到更深层次的高度抽象特征。当到达全连接（FC）层时，*Softmax* 激活函数会估计图像输入的类别概率。

除了 LeNet 的创始之外，像 AlexNet²、GoogLeNet³、VGGNet⁴、ResNet⁵ 和更新的 Transformer⁶ 等著名 CNN 架构也显著推动了深度学习领域的发展。

## 自然图像概述

探索自然 2D 图片可以提供对图像数据的基础理解。我们将从一些示例开始，深入探讨。

在我们的第一个示例中，我们将从广泛使用的`MNIST`数据集中选择一张图片。

![](../Images/c7dea9d21bec3e04cacbd81542d31621.png)

这个图像的形状是 `[28,28]`，表示一个具有单一通道的灰度图像。然后，神经网络的图像输入将是 `(28*28*1)`。

现在，让我们探索来自`ImageNet`数据集的一张图片。你可以直接从 ImageNet 的网站 [ImageNet (image-net.org)](https://www.image-net.org/download.php) 访问该数据集，或者在 Kaggle 上探索一个可用的子集 [ImageNet Object Localization Challenge | Kaggle](https://www.kaggle.com/c/imagenet-object-localization-challenge/data)。

![](../Images/df30982867a148631172830db8d3e469.png)

我们可以将这张图片分解为其 RGB 通道：

![](../Images/b1e48588369c8dab689156f44807f580.png)

由于该图像的形状是 `[500, 402, 3]`，神经网络的图像输入将表示为 `(500*402*3)`。

## 磁共振成像

MRI 扫描是神经学和神经外科中使用最广泛的检查方法，提供了一种非侵入性的方法，能提供良好的软组织对比度⁷。除了可视化结构细节外，MR 成像还深入提供了对大脑结构和功能方面的宝贵见解。

MRI 是由 3D 体积组成的，能够在三个解剖平面：轴向、冠状面和矢状面上进行可视化。这些体积由称为 *体素* 的 3D 立方体组成，与标准的 2D 图像相比，后者由称为 *像素* 的 2D 方块构成。虽然 3D 体积提供了全面的数据，但也可以被分解为 2D 切片。

各种 MRI 模态或序列，如 T1、T1 磁性增强（T1ce）、T2 和 FLAIR（液体衰减反转恢复），通常用于诊断。这些序列通过提供对应于特定区域或组织的不同信号强度，使肿瘤区分成为可能。以下插图展示了来自一个被诊断为胶质母细胞瘤的病人的这四种序列，胶质母细胞瘤是胶质瘤中最具侵袭性的类型，也是最常见的原发性脑肿瘤。

![](../Images/2c62aed363ebf89ac07d86dff9e62788.png)

# 脑肿瘤分割数据

[脑肿瘤分割（BraTS）挑战赛](https://www.med.upenn.edu/cbica/brats/) 提供了一个最广泛的多模态脑 MRI 扫描数据集，涵盖了 2012 到 2023 年的胶质瘤病人。BraTS 比赛的主要目标是评估最先进的方法在多模态 MRI 扫描中分割脑肿瘤的效果，尽管随着时间的推移还添加了额外的任务。

BraTS 数据集提供了关于肿瘤的临床信息，包括一个二进制标签，指示肿瘤级别（低级别或高级别）。BraTS 扫描以 NIfTI 文件形式提供，描述了 T1、T1ce、T2 和 Flair 模态。这些扫描在经过一些预处理步骤后提供，包括共配准到相同的解剖模板、插值到均匀的各向同性分辨率（1mm³）和去颅骨处理。

在这篇文章中，我们将使用 Kaggle 的 2020 数据集 [BraTS2020 数据集 (训练 + 验证)](https://www.kaggle.com/datasets/awsaf49/brats20-dataset-training-validation/) 来将胶质瘤 MRI 分类为低级别或高级别。

以下图片展示了低级别和高级别胶质瘤的例子：

![](../Images/fb787a68fa811c4b537df3a9baf3db00.png)

MRI 模态和 BraTS 病人 287 的分割掩膜。

![](../Images/51ada07cfa2f5f66a8fe47fd274f8eb6.png)

MRI 模态和 BraTS 病人 006 的分割掩膜。

Kaggle 存储库包含 369 个目录，每个目录代表一个患者并包含相应的图像数据。此外，它还包含两个 `.csv` 文件：*name_mapping.csv* 和 *survival_info.csv*。为了我们的目的，我们将使用 *name_mapping.csv*，它将 BraTS 患者姓名与 [TCGA-LGG](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=5309188) 和 [TCGA-GBM](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=1966258) 来自 [Cancer Imaging Archive](https://www.cancerimagingarchive.net/) 的公开数据集相关联。这个文件不仅便于姓名映射，还提供了肿瘤等级标签（LGG-HGG）。

![](../Images/2fdfda70921bc12e6049c468e389269a.png)

让我们探索每个患者文件夹的内容，以 *Patient 006* 为例。在 *BraTS20_Training_006* 文件夹中，我们可以找到 5 个文件，每个文件对应一种 MRI 模态和分割掩模：

+   *BraTS20_Training_006_flair.nii*

+   *BraTS20_Training_006_t1.nii*

+   *BraTS20_Training_006_t1ce.nii*

+   *BraTS20_Training_006_t2.nii*

+   *BraTS20_Training_006_seg.nii*

这些文件是 `.nii` 格式，代表 NIfTI 格式——神经成像中最流行的格式之一。

# MRI 数据准备

为了在 Python 中处理 NIfTI 图像，我们可以使用 [NiBabel](https://nipy.org/nibabel/) 包。以下是数据加载的示例。通过使用 `get_fdata()` 方法，我们可以将图像解释为 numpy 数组。

数组形状 `[240, 240, 155]` 表示一个 3D 体积，由 240 个 x 和 y 维度的 2D 切片和 155 个 z 维度的切片组成。这些维度对应不同的解剖视角：轴向视图（x-y 平面）、冠状视图（x-z 平面）和矢状视图（y-z 平面）。

![](../Images/c9fb55cd80b92c300735cec86ff32d34.png)

为了简化，我们将仅使用轴向平面的 2D 切片，生成的图像将具有 `[240, 240]` 的形状。

为了符合 PyTorch 模型的规格，输入张量必须具有 `[batch_size, num_channels, height, width]` 的形状。在我们的 MRI 数据中，四种模态（FLAIR、T1ce、T1、T2）各自强调图像中的不同特征，类似于图像中的通道。为了使数据格式符合 PyTorch 的要求，我们将这些序列堆叠为通道，从而实现 `[4, 240, 240]` 张量。

PyTorch 提供了两个数据工具，`torch.utils.data.Dataset` 和 `torch.utils.data.DataLoader`，旨在迭代数据集和批量加载数据。`Dataset` 包含各种标准数据集的子类，如 `MNIST`、`CIFAR` 或 `ImageNet`。导入这些数据集涉及加载相应的类并初始化 `DataLoader`。

考虑一个 `MNIST` 数据集的示例：

这使我们能够获得最终的张量，其维度为 `[batch_size=32, num_channels=1, H=28, W=28]` 张量。

由于我们有一个非平凡的数据集，在使用 `DataLoader` 之前需要创建一个自定义的 `Dataset class`。虽然本文未详细介绍创建此自定义数据集的步骤，但读者可以参考 PyTorch 关于 [Datasets & DataLoaders](https://pytorch.org/tutorials/beginner/basics/data_tutorial.html) 的教程以获取全面的指导。

# PyTorch 模型准备

[PyTorch](https://pytorch.org/) 是由 Facebook AI 研究人员在 2017 年开发的深度学习框架。`torchvision` 包含流行的数据集、图像变换和模型架构。在 `[torchvision.models](https://pytorch.org/vision/0.8/models.html)` 中，我们可以找到用于不同任务的深度学习架构实现，例如分类、分割或目标检测。

对于我们的应用程序，我们将加载 `ResNet18` 架构。

```py
ResNet(
  (conv1): Conv2d(3, 64, kernel_size=(7, 7), stride=(2, 2), padding=(3, 3), bias=False)
  (bn1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (relu): ReLU(inplace=True)
  (maxpool): MaxPool2d(kernel_size=3, stride=2, padding=1, dilation=1, ceil_mode=False)
  (layer1): Sequential(
    (0): BasicBlock(
      (conv1): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
    (1): BasicBlock(
      (conv1): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
  )
  (layer2): Sequential(
    (0): BasicBlock(
      (conv1): Conv2d(64, 128, kernel_size=(3, 3), stride=(2, 2), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (downsample): Sequential(
        (0): Conv2d(64, 128, kernel_size=(1, 1), stride=(2, 2), bias=False)
        (1): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      )
    )
    (1): BasicBlock(
      (conv1): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
  )
  (layer3): Sequential(
    (0): BasicBlock(
      (conv1): Conv2d(128, 256, kernel_size=(3, 3), stride=(2, 2), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (downsample): Sequential(
        (0): Conv2d(128, 256, kernel_size=(1, 1), stride=(2, 2), bias=False)
        (1): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      )
    )
    (1): BasicBlock(
      (conv1): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
  )
  (layer4): Sequential(
    (0): BasicBlock(
      (conv1): Conv2d(256, 512, kernel_size=(3, 3), stride=(2, 2), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (downsample): Sequential(
        (0): Conv2d(256, 512, kernel_size=(1, 1), stride=(2, 2), bias=False)
        (1): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      )
    )
    (1): BasicBlock(
      (conv1): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
  )
  (avgpool): AdaptiveAvgPool2d(output_size=(1, 1))
  (fc): Linear(in_features=512, out_features=1000, bias=True)
)
```

在神经网络中，输入层和输出层与具体问题密切相关。PyTorch 深度学习模型通常期望输入为 3 通道 RGB 图像，正如我们在初始卷积层配置中所观察到的 `Conv2d(in_channels = 3, out_channels = 64, kernel_size=(7,7), stride=(2,2), padding=(3,3), bias=False)`。此外，最终层 `Linear(in_features = 512, out_features = 1000, bias = True)` 默认输出大小为 1000，代表 ImageNet 数据集中的类别数量。

在训练分类模型之前，调整 `in_channels` 和 `out_features` 以与我们的特定数据集对齐是至关重要的。我们可以通过 `resnet.conv1` 访问第一个卷积层并更新 `in_channels`。类似地，我们可以通过 `resnet.fc` 访问最后一个全连接层并修改 `out_features`。

```py
ResNet(
  (conv1): Conv2d(4, 64, kernel_size=(7, 7), stride=(1, 1))
  (bn1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (relu): ReLU(inplace=True)
  (maxpool): MaxPool2d(kernel_size=3, stride=2, padding=1, dilation=1, ceil_mode=False)
  (layer1): Sequential(
    (0): BasicBlock(
      (conv1): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
    (1): BasicBlock(
      (conv1): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(64, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
  )
  (layer2): Sequential(
    (0): BasicBlock(
      (conv1): Conv2d(64, 128, kernel_size=(3, 3), stride=(2, 2), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (downsample): Sequential(
        (0): Conv2d(64, 128, kernel_size=(1, 1), stride=(2, 2), bias=False)
        (1): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      )
    )
    (1): BasicBlock(
      (conv1): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(128, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
  )
  (layer3): Sequential(
    (0): BasicBlock(
      (conv1): Conv2d(128, 256, kernel_size=(3, 3), stride=(2, 2), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (downsample): Sequential(
        (0): Conv2d(128, 256, kernel_size=(1, 1), stride=(2, 2), bias=False)
        (1): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      )
    )
    (1): BasicBlock(
      (conv1): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
  )
  (layer4): Sequential(
    (0): BasicBlock(
      (conv1): Conv2d(256, 512, kernel_size=(3, 3), stride=(2, 2), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (downsample): Sequential(
        (0): Conv2d(256, 512, kernel_size=(1, 1), stride=(2, 2), bias=False)
        (1): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      )
    )
    (1): BasicBlock(
      (conv1): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn1): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
      (relu): ReLU(inplace=True)
      (conv2): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1), bias=False)
      (bn2): BatchNorm2d(512, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
    )
  )
  (avgpool): AdaptiveAvgPool2d(output_size=(1, 1))
  (fc): Linear(in_features=512, out_features=2, bias=True)
)
```

# 图像分类

准备好模型和数据后，我们可以继续进行实际应用。

以下示例展示了如何有效地利用我们的 ResNet18 将图像分类为低等级或高等级。为了管理我们张量中的批量尺寸维度，我们将简单地添加一个单位维度（注意，这通常由数据加载器处理）。

```py
tensor(0.5465, device='cuda:0', grad_fn=<NllLossBackward0>)
```

这篇文章到此结束。希望这对那些进入 MRI 和深度学习交叉领域的人有所帮助。感谢你抽出时间阅读。欲了解我研究的深入见解，请随时查阅我的最新论文！ :)

Pitarch, C.; Ribas, V.; Vellido, A. 基于 AI 的胶质瘤分级以获得可信诊断：提高可靠性的分析管道。*Cancers* **2023**，*15*，3369。 [https://doi.org/10.3390/cancers15133369](https://doi.org/10.3390/cancers15133369)。

*除非另有说明，所有图片均由作者提供。*

[1] Y. LeCun 等人。“反向传播应用于手写邮政编码

识别”。刊载于《Neural Computation》1（1989 年 12 月 4 日），第 541–551 页。

ISSN: 0899–7667。DOI: [10.1162/NECO.1989.1.4.541](https://direct.mit.edu/neco/article-abstract/1/4/541/5515/Backpropagation-Applied-to-Handwritten-Zip-Code?redirectedFrom=fulltext)。

[2] Alex Krizhevsky, Ilya Sutskever 和 Geoffrey E. Hinton. “ImageNet 分类与深度卷积神经网络”。

深度卷积神经网络的分类”。出现在：进展。

在神经信息处理系统第25卷（2012）。

[3] Christian Szegedy 等. “通过卷积进一步深入”。出现在：论文集。

IEEE计算机学会计算机视觉会议论文集。

计算机视觉与模式识别 07–12-2015年6月（2014年9月），第1–9页。

ISSN: 10636919。DOI: [10.1109/CVPR.2015.7298594](https://ieeexplore.ieee.org/document/7298594/)。

[4] Karen Simonyan 和 Andrew Zisserman. “非常深的卷积网络”。

大规模图像识别的网络”。出现在：第三届国际。

学习表示大会，ICLR 2015 — 大会追踪。

论文集（2014年9月）。

[5] Kaiming He 等. “用于图像识别的深度残差学习”。

出现在：IEEE计算机学会计算机视觉会议论文集。

计算机视觉与模式识别2016年12月（2015年12月），第770–778页。

778。ISSN: 10636919。DOI: [10.1109/CVPR.2016.90](https://ieeexplore.ieee.org/document/7780459)。

[6] Ashish Vaswani 等. “注意力机制”。出现在：进展。

神经信息处理系统2017年12月（2017年6月），第5999–6009页。ISSN: 10495258。DOI: [10.48550/arxiv.1706.03762](https://arxiv.org/abs/1706.03762)。

[7] Lisa M. DeAngelis. “脑肿瘤”。出现在：新英格兰医学杂志344卷（2001年8月2日），第114–123页。ISSN: 0028–4793。DOI: [10.1056/NEJM200101113440207](https://www.nejm.org/doi/full/10.1056/NEJM200101113440207)
