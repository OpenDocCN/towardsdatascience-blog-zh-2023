# 每个数据科学家都应该了解的前 10 大预训练模型

> 原文：[`towardsdatascience.com/top-10-pre-trained-models-for-image-embedding-every-data-scientist-should-know-88da0ef541cd`](https://towardsdatascience.com/top-10-pre-trained-models-for-image-embedding-every-data-scientist-should-know-88da0ef541cd)

## 转移学习的基础指南

[](https://satyam-kumar.medium.com/?source=post_page-----88da0ef541cd--------------------------------)![Satyam Kumar](https://satyam-kumar.medium.com/?source=post_page-----88da0ef541cd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----88da0ef541cd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----88da0ef541cd--------------------------------) [Satyam Kumar](https://satyam-kumar.medium.com/?source=post_page-----88da0ef541cd--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----88da0ef541cd--------------------------------) ·9 分钟阅读·2023 年 4 月 19 日

--

![](img/bcf5c3734418b63acaacc768035b72e8.png)

图像由 [Chen](https://pixabay.com/users/chenspec-7784448/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=5692896) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=5692896)

计算机视觉的快速发展——图像分类用例已因转移学习的出现而进一步加速。在大规模图像数据集上训练计算机视觉神经网络模型需要大量计算资源和时间。

幸运的是，通过使用预训练模型，可以缩短时间和资源。利用预训练模型的特征表示的技术被称为转移学习。预训练模型通常使用高端计算资源和大规模数据集进行训练。

预训练模型可以以多种方式使用：

+   使用预训练权重并直接对测试数据进行预测

+   使用预训练的权重进行初始化，并使用自定义数据集训练模型

+   仅使用预训练网络的架构，并在自定义数据集上从头开始训练

本文介绍了获取图像嵌入的十大最先进的预训练模型。所有这些预训练模型都可以通过 [keras.application](https://keras.io/api/applications/) API 以 Keras 模型的形式加载。

```py
**CNN Architecture discussed in this article:**
1) VGG
2) Xception
3) ResNet
4) InceptionV3
5) InceptionResNet
6) MobileNet
7) DenseNet
8) NasNet
9) EfficientNet
10) ConvNEXT
```

> **许可协议：** 本文中使用的所有图像均来自 [paperwithcode.com](https://paperswithcode.com/)，其许可协议为 CC BY-SA 4.0。

# 1) VGG:

VGG-16/19 网络在 ILSVRC 2014 大会上推出，因为它是最受欢迎的预训练模型之一。它由牛津大学的视觉图形组开发。

VGG 模型有两种变体：16 层和 19 层网络，VGG-19（19 层网络）是 VGG-16（16 层网络）模型的改进。

**架构：**

![](img/bf14e8e28334a24a8ae22c55a4e1de6d.png)

([来源](https://paperswithcode.com/method/vgg)，使用[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)的免费许可证)，VGG-16 网络架构

VGG 网络结构简单且顺序，并且使用了大量的滤波器。在每个阶段，使用小的（3*3）滤波器来减少参数的数量。

VGG-16 网络具有以下特点：

+   卷积层 = 13

+   池化层 = 5

+   全连接密集层 = 3

**输入：** 维度为（224, 224, 3）的图像

**输出：** 1000 维的图像嵌入

**VGG-16/19 的其他细节：**

+   论文链接：[`arxiv.org/pdf/1409.1556.pdf`](https://arxiv.org/pdf/1409.1556.pdf)

+   GitHub: [VGG](https://paperswithcode.com/paper/very-deep-convolutional-networks-for-large#code)

+   发布时间：2015 年 4 月

+   在 ImageNet 数据集上的性能：71%（Top 1 准确率），90%（Top 5 准确率）

+   参数数量：约 140M

+   层数：16/19

+   磁盘大小：约 530MB

**实现：**

+   在您的输入数据上调用 `[tf.keras.applications.vgg16.preprocess_input](https://www.tensorflow.org/api_docs/python/tf/keras/applications/vgg16/preprocess_input)` 将输入图像转换为 BGR，并对每个颜色通道进行零中心化。

+   使用以下代码实例化 VGG16 模型：

```py
tf.keras.applications.VGG16(
    include_top=True,
    weights="imagenet",
    input_tensor=None,
    input_shape=None,
    pooling=None,
    classes=1000,
    classifier_activation="softmax",
)
```

上述代码用于 VGG-16 的实现，keras 提供了类似的 API 用于 VGG-19 的实现，更多细节请参阅[此文档](https://keras.io/api/applications/vgg/#vgg16-function)。

# 2) Xception：

Xception 是一个深度 CNN 架构，涉及深度可分离卷积。深度可分离卷积可以理解为具有最大数量塔的 Inception 模型。

**架构：**

![](img/cca16ef8eae292510d58bc3c94f14bb5.png)

([来源](https://paperswithcode.com/paper/xception-deep-learning-with-depthwise)，使用[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)的免费许可证)，Xception 架构

**输入：** 维度为（299, 299, 3）的图像

**输出：** 1000 维的图像嵌入

**Xception 的其他细节：**

+   论文链接：[`arxiv.org/pdf/1409.1556.pdf`](https://arxiv.org/pdf/1610.02357.pdf)

+   GitHub: [Xception](https://paperswithcode.com/paper/xception-deep-learning-with-depthwise#code)

+   发布时间：2017 年 4 月

+   在 ImageNet 数据集上的性能：79%（Top 1 准确率），94.5%（Top 5 准确率）

+   参数数量：约 30M

+   深度：81

+   磁盘大小：88MB

**实现：**

+   使用以下代码实例化 Xception 模型：

```py
tf.keras.applications.Xception(
    include_top=True,
    weights="imagenet",
    input_tensor=None,
    input_shape=None,
    pooling=None,
    classes=1000,
    classifier_activation="softmax",
)
```

上述代码用于 Xception 实现，更多详细信息请参考[该文档](https://keras.io/api/applications/xception/)。

# 3) ResNet:

之前的 CNN 架构未设计为扩展到许多卷积层。这导致了梯度消失问题，并且在向现有架构添加新层时性能受限。

ResNet 架构提供跳跃连接以解决梯度消失问题。

**架构：**

![](img/ffc35dcf643069854d00c4f511e8ab1a.png)

([来源](https://production-media.paperswithcode.com/methods/Screen_Shot_2020-09-25_at_10.26.40_AM_SAB79fQ.png)，使用[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)的自由使用许可)，ResNet 架构

这个 ResNet 模型使用了一个 34 层的网络架构，灵感来源于 VGG-19 模型，并在其上添加了捷径连接。这些捷径连接将架构转换为残差网络。

ResNet 架构有多个版本：

+   ResNet50

+   ResNet50V2

+   ResNet101

+   ResNet101V2

+   ResNet152

+   ResNet152V2

**输入：** 尺寸为(224, 224, 3)的图像

**输出：** 1000 维的图像嵌入

**ResNet 模型的其他详细信息：**

+   论文链接: [`arxiv.org/pdf/1512.03385.pdf`](https://arxiv.org/pdf/1512.03385.pdf)

+   GitHub: [ResNet](https://paperswithcode.com/paper/deep-residual-learning-for-image-recognition#code)

+   发布日期: 2015 年 12 月

+   在 ImageNet 数据集上的性能: 75–78%（Top 1 准确率），92–93%（Top 5 准确率）

+   参数数量: 25–60M

+   深度: 107–307

+   磁盘大小: ~100–230MB

**实现：**

+   使用下面的代码实例化 ResNet50 模型：

```py
tf.keras.applications.ResNet50(
    include_top=True,
    weights="imagenet",
    input_tensor=None,
    input_shape=None,
    pooling=None,
    classes=1000,
    **kwargs
)
```

上述代码用于 ResNet50 实现，keras 提供了类似的 API 用于其他 ResNet 架构实现，更多详细信息请参考[该文档](https://keras.io/api/applications/resnet/#resnet101-function)。

# 4) Inception:

多层深度卷积导致了数据的过拟合。为避免过拟合，Inception 模型使用了并行层或在同一层级上使用不同大小的多个滤波器，从而使模型变宽，而不是变深。Inception V1 模型由 4 个并行层组成，分别为：(1*1)、(3*3)、(5*5)卷积和(3*3)最大池化。

Inception (V1/V2/V3) 是由谷歌团队开发的基于深度学习的 CNN 网络。InceptionV3 是 InceptionV1 和 V2 模型的高级和优化版本。

**架构：**

InceptionV3 模型由 42 层组成。InceptionV3 的架构逐步构建如下：

+   因式分解卷积

+   更小的卷积

+   非对称卷积

+   辅助卷积

+   网格尺寸缩减

所有这些概念都整合到了下面提到的最终架构中：

![](img/6c6221bb9bc94860e18662fde362d19d.png)

([来源](https://paperswithcode.com/method/inception-v3)，使用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 许可证的免费使用)，InceptionV3 架构

**输入：** 尺寸为 (299, 299, 3) 的图像

**输出：** 1000 维图像嵌入

**InceptionV3 模型的其他细节：**

+   论文链接： [`arxiv.org/pdf/1512.00567.pdf`](https://arxiv.org/pdf/1512.00567.pdf)

+   GitHub: [InceptionV3](https://paperswithcode.com/paper/what-do-deep-networks-like-to-see#code)

+   发布日期：2015 年 12 月

+   在 ImageNet 数据集上的表现：78%（Top 1 精度），94%（Top 5 精度）

+   参数数量：24M

+   深度：189

+   磁盘大小：92MB

**实现：**

+   使用以下代码实例化 InceptionV3 模型：

```py
tf.keras.applications.InceptionV3(
    include_top=True,
    weights="imagenet",
    input_tensor=None,
    input_shape=None,
    pooling=None,
    classes=1000,
    classifier_activation="softmax",
)
```

上述代码是用于 InceptionV3 实现的，更多细节请参考 [这份文档](https://keras.io/api/applications/inceptionv3/)。

# 5) InceptionResNet:

InceptionResNet-v2 是由 Google 的研究人员开发的 CNN 模型。该模型的目标是减少 InceptionV3 的复杂性，并探索在 Inception 模型中使用残差网络的可能性。

**架构：**

![](img/a49ccb18b6aae5e423a69f62a3f40e91.png)

([来源](https://paperswithcode.com/method/inception-resnet-v2)，使用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 许可证的免费使用)，Inception-ResNet-V2 架构

**输入：** 尺寸为 (299, 299, 3) 的图像

**输出：** 1000 维图像嵌入

**Inception-ResNet-V2 模型的其他细节：**

+   论文链接： [`arxiv.org/pdf/1602.07261.pdf`](https://arxiv.org/pdf/1602.07261.pdf)

+   GitHub: [Inception-ResNet-V](https://paperswithcode.com/paper/inception-v4-inception-resnet-and-the-impact#code)2

+   发布日期：2016 年 8 月

+   在 ImageNet 数据集上的表现：80%（Top 1 精度），95%（Top 5 精度）

+   参数数量：56M

+   深度：189

+   磁盘大小：215MB

**实现：**

+   使用以下代码实例化 Inception-ResNet-V2 模型：

```py
tf.keras.applications.InceptionResNetV2(
    include_top=True,
    weights="imagenet",
    input_tensor=None,
    input_shape=None,
    pooling=None,
    classes=1000,
    classifier_activation="softmax",
    **kwargs
)
```

上述代码是用于 Inception-ResNet-V2 实现的，更多细节请参考 [这份文档](https://keras.io/api/applications/inceptionresnetv2/)。

# 6) MobileNet:

MobileNet 是一种简化的架构，使用深度可分离卷积来构建深度卷积神经网络，并为移动和嵌入式视觉应用提供高效的模型。

**架构：**

![](img/72ad57d5161cf1660e5501abfde689bc.png)

([来源](https://www.hindawi.com/journals/misy/2020/7602384/)，使用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 许可证的免费使用)，Mobile-Net 架构

**输入：** 尺寸为 (224, 224, 3) 的图像

**输出：** 1000 维图像嵌入

**MobileNet 模型的其他细节：**

+   论文链接： [`arxiv.org/pdf/1602.07261.pdf`](https://arxiv.org/pdf/1704.04861.pdf)

+   GitHub：[MobileNet-V3](https://paperswithcode.com/paper/searching-for-mobilenetv3#code)，[MobileNet-V2](https://paperswithcode.com/paper/mobilenetv2-inverted-residuals-and-linear#code)

+   发布日期：2017 年 4 月

+   在 ImageNet 数据集上的表现：71%（Top 1 准确率），90%（Top 5 准确率）

+   参数数量：3.5–4.3M

+   深度：55–105

+   磁盘上的大小：14–16MB

**实现：**

+   使用下面提到的代码实例化 MobileNet 模型：

```py
tf.keras.applications.MobileNet(
    input_shape=None,
    alpha=1.0,
    depth_multiplier=1,
    dropout=0.001,
    include_top=True,
    weights="imagenet",
    input_tensor=None,
    pooling=None,
    classes=1000,
    classifier_activation="softmax",
    **kwargs
)
```

上述代码用于 MobileNet 实现，keras 提供了与其他 MobileNet 架构（MobileNet-V2，MobileNet-V3）实现类似的 API，更多详细信息请参见[此文档](https://keras.io/api/applications/mobilenet/)。

# 7) DenseNet：

DenseNet 是一个 CNN 模型，旨在改善由于输入层和输出层之间的长距离导致的梯度消失问题，从而提高高层神经网络的准确性。

**架构：**

DenseNet 架构有 3 个密集块。两个相邻块之间的层称为过渡层，通过卷积和池化改变特征图的尺寸。

![](img/6ad573e101eebd6f3ecb20c10bcda321.png)

([来源](https://paperswithcode.com/method/densenet)，在[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)下的免费使用许可)，DenseNet 架构

**输入：** 尺寸为（224, 224, 3）的图像

**输出：** 1000 维的图像嵌入

**DenseNet 模型的其他细节：**

+   论文链接：[`arxiv.org/pdf/1608.06993.pdf`](https://arxiv.org/pdf/1608.06993.pdf)

+   GitHub：[DenseNet-169](https://paperswithcode.com/paper/densely-connected-convolutional-networks#code)，[DenseNet-201](https://paperswithcode.com/paper/densely-connected-convolutional-networks)，[DenseNet-264](https://paperswithcode.com/paper/densely-connected-convolutional-networks#code)

+   发布日期：2018 年 1 月

+   在 ImageNet 数据集上的表现：75–77%（Top 1 准确率），92–94%（Top 5 准确率）

+   参数数量：8–20M

+   深度：240–400

+   磁盘上的大小：33–80MB

**实现：**

+   使用下面提到的代码实例化 DenseNet121 模型：

```py
tf.keras.applications.DenseNet121(
    include_top=True,
    weights="imagenet",
    input_tensor=None,
    input_shape=None,
    pooling=None,
    classes=1000,
    classifier_activation="softmax",
)
```

上述代码用于 DenseNet 实现，keras 提供了与其他 DenseNet 架构（DenseNet-169，DenseNet-201）实现类似的 API，更多详细信息请参见[此文档](https://keras.io/api/applications/mobilenet/)。

# 8) NasNet：

Google 研究人员设计了一个 NasNet 模型，将寻找最佳 CNN 架构的问题框架化为一种强化学习方法。其思想是搜索给定搜索空间的最佳参数组合，包括层数、滤波器大小、步幅、输出通道等。

**输入：** 尺寸为（331, 331, 3）的图像

**NasNet 模型的其他细节：**

+   论文链接：[`arxiv.org/pdf/1608.06993.pdf`](https://arxiv.org/pdf/1707.07012.pdf)

+   发布日期：2018 年 4 月

+   在 ImageNet 数据集上的表现：75–83%（Top 1 准确率），92–96%（Top 5 准确率）

+   参数数量：5–90M

+   深度：389–533

+   磁盘大小：23–343MB

**实现：**

+   使用以下代码实例化 NesNetLarge 模型：

```py
tf.keras.applications.NASNetLarge(
    input_shape=None,
    include_top=True,
    weights="imagenet",
    input_tensor=None,
    pooling=None,
    classes=1000,
    classifier_activation="softmax",
)
```

上述代码用于 NesNet 实现，keras 提供了类似的 API 用于其他 NasNet 架构（NasNetLarge，NasNetMobile）的实现，更多细节请参考[此文档](https://keras.io/api/applications/nasnet/#nasnetmobile-function)。

# 9) EfficientNet：

EfficientNet 是谷歌研究人员提出的一种 CNN 架构，通过一种称为复合缩放的缩放方法实现更好的性能。该缩放方法通过固定的复合系数均匀缩放所有深度/宽度/分辨率维度。

**架构：**

![](img/c66322e34001b6b1958742fb0b98839d.png)

([来源](https://paperswithcode.com/method/efficientnet)，在[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)下免费使用许可证)，Efficient-B0 架构

**EfficientNet 模型的其他细节：**

+   论文链接：[`arxiv.org/pdf/1905.11946v5.pdf`](https://arxiv.org/pdf/1905.11946v5.pdf)

+   GitHub：[EfficientNet](https://paperswithcode.com/paper/efficientnet-rethinking-model-scaling-for#code)

+   发布日期：2020 年 9 月

+   在 ImageNet 数据集上的表现：77–84%（Top 1 准确率），93–97%（Top 5 准确率）

+   参数数量：5–67M

+   深度：132–438

+   磁盘大小：29–256MB

**实现：**

+   使用以下代码实例化 EfficientNet-B0 模型：

```py
tf.keras.applications.EfficientNetB0(
    include_top=True,
    weights="imagenet",
    input_tensor=None,
    input_shape=None,
    pooling=None,
    classes=1000,
    classifier_activation="softmax",
    **kwargs
)
```

上述代码用于 EfficientNet-B0 实现，keras 提供了类似的 API 用于其他 EfficientNet 架构（EfficientNet-B0 到 B7，EfficientNet-V2-B0 到 B3）的实现，更多细节请参考[此文档](https://keras.io/api/applications/efficientnet/#efficientnetb0-function)，以及[此文档](https://keras.io/api/applications/efficientnet_v2/#efficientnetv2b0-function)。

# 10) ConvNeXt：

ConvNeXt CNN 模型被提出作为一种纯卷积模型（ConvNet），受 Vision Transformers 设计的启发，声称能够超越它们。

**架构：**

![](img/ec14809f89d123eb8425466d17393e20.png)

([来源](https://arxiv.org/pdf/2201.03545.pdf)，在[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)下免费使用许可证)，ConvNeXt 架构

**ConvNeXt 模型的其他细节：**

+   论文链接：[`arxiv.org/pdf/1905.11946v5.pdf`](https://arxiv.org/pdf/1905.11946v5.pdf)

+   GitHub：[ConvNeXt](https://paperswithcode.com/paper/a-convnet-for-the-2020s#code)

+   发布日期：2022 年 3 月

+   在 ImageNet 数据集上的表现：81–87%（Top 1 准确率）

+   参数数量：29–350M

+   磁盘大小：110–1310MB

**实现：**

+   使用以下代码实例化 ConvNeXt-Tiny 模型：

```py
tf.keras.applications.ConvNeXtTiny(
    model_name="convnext_tiny",
    include_top=True,
    include_preprocessing=True,
    weights="imagenet",
    input_tensor=None,
    input_shape=None,
    pooling=None,
    classes=1000,
    classifier_activation="softmax",
)
```

上述代码是用于 ConvNeXt-Tiny 实现的，keras 提供了类似的 API 用于其他 EfficientNet 架构（ConvNeXt-Small、ConvNeXt-Base、ConvNeXt-Large、ConvNeXt-XLarge）的实现，更多细节请参见[此文档](https://keras.io/api/applications/convnext/#convnexttiny-function)。

# 总结：

我讨论了 10 种流行的 CNN 架构，这些架构可以使用迁移学习生成嵌入。这些预训练的 CNN 模型在 ImageNet 数据集上表现优异，并且证明了它们是最好的。Keras 库提供了 API 来加载这些讨论过的预训练模型的架构和权重。从这些模型生成的图像嵌入可以用于各种用例。

然而，这是一个不断发展的领域，总是有新的 CNN 架构值得期待。

# 参考文献：

[1] Papers with code: [`paperswithcode.com/sota/image-classification-on-imagenet`](https://paperswithcode.com/sota/image-classification-on-imagenet)

[2] Keras 文档: [`keras.io/api/applications/`](https://keras.io/api/applications/)

> 感谢阅读
