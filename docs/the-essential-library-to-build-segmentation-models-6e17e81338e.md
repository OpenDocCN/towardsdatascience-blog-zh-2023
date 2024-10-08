# 用一行代码构建一个分割模型

> 原文：[`towardsdatascience.com/the-essential-library-to-build-segmentation-models-6e17e81338e`](https://towardsdatascience.com/the-essential-library-to-build-segmentation-models-6e17e81338e)

## 以最快的方式构建和训练图像分割神经网络模型

[](https://mattiagatti.medium.com/?source=post_page-----6e17e81338e--------------------------------)![Mattia Gatti](https://mattiagatti.medium.com/?source=post_page-----6e17e81338e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6e17e81338e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6e17e81338e--------------------------------) [Mattia Gatti](https://mattiagatti.medium.com/?source=post_page-----6e17e81338e--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6e17e81338e--------------------------------) ·阅读时间 6 分钟·2023 年 3 月 6 日

--

![](img/ad811a836b54548872ee8eafd0abbba0.png)

[MartinThoma](https://commons.wikimedia.org/wiki/File:Image-segmentation-example-segmented.png)，CC0，通过 Wikimedia Commons（已编辑）

神经网络模型在解决分割问题上已被证明非常有效，达到了最先进的准确率。它们在医学图像分析、自动驾驶、机器人技术、卫星图像、视频监控等各种应用中取得了显著的改进。然而，构建这些模型通常需要较长时间，但阅读完本指南后，你将能够用仅仅几行代码来构建一个模型。

## 目录

1.  介绍

1.  构建模块

1.  构建模型

1.  训练模型

# 介绍

分割是将图像根据某些特征或属性划分为多个片段或区域的任务。一个分割模型以图像为输入，并返回一个分割掩码：

![](img/18664e6573f75bd5b860c3694ed659e2.png)

（左）输入图像 | （右）其分割掩码。两张图像均来自[PyTorch](https://pytorch.org/hub/pytorch_vision_deeplabv3_resnet101/)。

分割神经网络模型由两个部分组成：

+   一个**编码器**：接受输入图像并提取特征。编码器的例子包括 ResNet、EfficientNet 和 ViT。

+   一个**解码器**：提取特征并生成分割掩码。解码器的架构各不相同。例如的架构包括 U-Net、FPN 和 DeepLab。

因此，在为特定应用构建分割模型时，你需要选择一个架构和一个编码器。然而，在不测试多个组合的情况下，很难选择最佳组合。这通常需要很长时间，因为更改模型需要编写大量的样板代码。[Segmentation Models](https://github.com/qubvel/segmentation_models.pytorch) 库解决了这个问题。它允许你通过指定架构和编码器在一行代码中创建一个模型。然后，你只需修改该行代码即可更改其中的一个。

要从 PyPI 安装最新版本的 Segmentation Models，请使用：

```py
pip install segmentation-models-pytorch
```

# 构建模块

该库为大多数分割架构提供了一个类，每个类都可以与任何可用的编码器一起使用。在下一部分中，你将看到要构建模型，你需要实例化所选架构的类，并将所选编码器的字符串作为参数传递。下图显示了库提供的每个架构的类名称：

![](img/b49a27e0de58532dca66f45a639efa55.png)

库提供的所有架构的类别名称。

下图显示了库提供的最常见编码器的名称：

![](img/d1756893e86b8006ecc1b313fda9e6bc.png)

库提供的最常见编码器的名称。

目前有超过 400 种编码器，因此不可能一一展示，但你可以在[这里](https://github.com/qubvel/segmentation_models.pytorch#encoders)找到完整的列表。

# 构建模型

一旦从上面的图中选择了架构和编码器，构建模型就非常简单：

参数：

+   `encoder_name` 是所选编码器的名称（例如：resnet50、efficientnet-b7、mit_b5）。

+   `encoder_weights` 是预训练数据集。如果 `encoder_weights` 等于 `"imagenet"`，则编码器权重将使用 ImageNet 预训练权重初始化。所有编码器至少有一种预训练模型，完整列表可以在[这里](https://github.com/qubvel/segmentation_models.pytorch#encoders)找到。

+   `in_channels` 是输入图像的通道数（如果是 RGB，则为 3）。

    即使 `in_channels` 不是 3，也可以使用 ImageNet 预训练模型：第一层将通过重用预训练的第一个卷积层的权重来初始化（该过程描述[这里](https://github.com/qubvel/segmentation_models.pytorch#input-channels)）。

+   `out_classes` 是数据集中的类别数量。

+   `activation` 是输出层的激活函数。可选的值有 `None`（默认）、`sigmoid` 和 `softmax`。

    **注意：** 当使用期望 logits 作为输入的损失函数时，激活函数必须为 None。例如，当使用 `CrossEntropyLoss` 函数时，`activation` 必须为 `None`。

# 训练模型

本节展示了执行训练所需的所有代码。然而，这个库不会改变训练和验证模型的常规流程。为了简化过程，该库提供了许多损失函数的实现，例如*Jaccard Loss, Dice Loss, Dice Cross-Entropy Loss, Focal Loss*，以及*Accuracy, Precision, Recall, F1Score, 和 IOUScore*等指标。有关它们及其参数的完整列表，请查阅[Losses](https://smp.readthedocs.io/en/latest/losses.html)和[Metrics](https://smp.readthedocs.io/en/latest/metrics.html)部分的文档。

提议的训练示例是使用[Oxford-IIIT Pet Dataset](https://www.robots.ox.ac.uk/~vgg/data/pets/)进行二分类分割（将通过代码下载）。以下是数据集中的两个样本：

![](img/b3778e77e2af3b35453e7c23c7171992.png)

Oxford-IIIT Pet Dataset 中的一只猫的样本。

![](img/9bc5248992644cf2eca7c0358331308e.png)

Oxford-IIIT Pet Dataset 中的一只狗的样本。

最后，这些是执行此类分割任务的所有步骤：

1.  构建模型。

根据你将使用的损失函数设置最后一层的激活函数。

2. 定义参数。

请记住，在使用预训练模型时，输入应通过使用训练预训练模型时的数据的均值和标准差来进行归一化。

3. 定义训练函数。

这里没有改变，你在不使用库的情况下训练模型时所编写的训练函数。

4. 定义验证函数。

真阳性、假阳性、假阴性和真阴性从批次中一起求和，仅在批次结束时计算指标。请注意，logits 必须转换为类别后才能计算指标。调用训练函数开始训练。

5. 使用模型。

这些是一些分割示例：

![](img/4eb0f727a6a5f922ae61e421c080d9f0.png)![](img/54b4f642093a462f5e98794c8d56f3f7.png)![](img/6d7a007d8d96edcf33c0b1fc4abcf432.png)

## 结论

这个库包含了你实验分割所需的一切。构建模型和应用更改非常简单，并且大多数损失函数和指标都已提供。此外，使用这个库不会改变我们习惯的流程。有关更多信息，请参见[官方文档](https://smp.readthedocs.io/en/latest/)。我还在参考文献中包括了一些最常见的编码器和架构。

[Oxford-IIIT Pet Dataset](https://www.robots.ox.ac.uk/~vgg/data/pets/)在[创作共用署名-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-sa/4.0/)下可供下载用于商业/研究目的。版权归图像的原始所有者所有。

所有图像，除非另有说明，均由作者提供。感谢阅读，希望你觉得这些信息有用。

[1] O. Ronneberger, P. Fischer 和 T. Brox, [U-Net: 用于生物医学图像分割的卷积网络](https://arxiv.org/abs/1505.04597) (2015)

[2] Z. Zhou, Md. M. R. Siddiquee, N. Tajbakhsh 和 J. Liang, [UNet++: 一种嵌套的 U-Net 架构用于医学图像分割](https://arxiv.org/abs/1807.10165) (2018)

[3] L. Chen, G. Papandreou, F. Schroff, H. Adam, [重新思考用于语义图像分割的膨胀卷积](https://arxiv.org/abs/1706.05587) (2017)

[4] L. Chen, Y. Zhu, G. Papandreou, F. Schroff, H. Adam, [用于语义图像分割的编码器-解码器与膨胀可分离卷积](https://arxiv.org/abs/1802.02611) (2018)

[5] R. Li, S. Zheng, C. Duan, C. Zhang, J. Su, P.M. Atkinson, [用于细分辨率遥感图像语义分割的多注意力网络](https://arxiv.org/abs/2009.02130) (2020)

[6] A. Chaurasia, E. Culurciello, [LinkNet: 利用编码器表示进行高效的语义分割](https://arxiv.org/abs/1707.03718) (2017)

[7] T. Lin, P. Dollár, R. Girshick, K. He, B. Hariharan, S. Belongie, [用于目标检测的特征金字塔网络](https://arxiv.org/abs/1612.03144) (2017)

[8] H. Zhao, J. Shi, X. Qi, X. Wang, J. Jia, [金字塔场景解析网络](https://arxiv.org/abs/1612.01105) (2016)

[9] H. Li, P. Xiong, J. An, L. Wang, [用于语义分割的金字塔注意力网络](https://arxiv.org/abs/1805.10180) (2018)

[10] K. Simonyan, A. Zisserman, [用于大规模图像识别的非常深的卷积网络](https://arxiv.org/abs/1409.1556) (2014)

[11] Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun, [用于图像识别的深度残差学习](https://arxiv.org/abs/1512.03385) (2015)

[12] S. Xie, R. Girshick, P. Dollár, Z. Tu, K. He, [深度神经网络的聚合残差变换](https://arxiv.org/abs/1611.05431) (2016)

[13] J. Hu, L. Shen, S. Albanie, G. Sun, E. Wu, [压缩-激励网络](https://arxiv.org/abs/1709.01507) (2017)

[14] G. Huang, Z. Liu, L. van der Maaten, K. Q. Weinberger, [密集连接卷积网络](https://arxiv.org/abs/1608.06993) (2016)

[15] M. Tan, Q. V. Le, [EfficientNet: 重新思考卷积神经网络的模型缩放](https://arxiv.org/abs/1905.11946) (2019)

[16] E. Xie, W. Wang, Z. Yu, A. Anandkumar, J. M. Alvarez, P. Luo, [SegFormer: 基于 Transformers 的简单高效语义分割设计](https://arxiv.org/abs/2105.15203) (2021)
