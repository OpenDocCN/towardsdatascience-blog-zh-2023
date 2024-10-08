# 卷积解释——卷积神经网络简介

> 原文：[`towardsdatascience.com/convolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa`](https://towardsdatascience.com/convolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa)

## CNNs 的基本构建块

[](https://medium.com/@egorhowell?source=post_page-----5babc47fbcaa--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----5babc47fbcaa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5babc47fbcaa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5babc47fbcaa--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----5babc47fbcaa--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5babc47fbcaa--------------------------------) ·8 分钟阅读·2023 年 12 月 27 日

--

![](img/725d855cae28fe1f795da02e71253227.png)

”[`www.flaticon.com/free-icons/neural-network`](https://www.flaticon.com/free-icons/neural-network)" 标题为“神经网络图标。” 神经网络图标由 Freepik 创建 — Flaticon。

我最近的文章系列是关于神经网络的，我们从简单的 [***感知器***](https://medium.com/gitconnected/intro-perceptron-architecture-neural-networks-101-2a487062810c) 讲到复杂的架构以及如何处理 [***深度学习中的常见问题***](https://medium.com/towards-data-science/vanishing-exploding-gradient-problem-neural-networks-101-c8f48ec6a80b)。如果你感兴趣，可以在这里查看系列文章：

![Egor Howell](img/e969a9f3c3357e1c80dcd0092d9a1288.png)

[Egor Howell](https://medium.com/@egorhowell?source=post_page-----5babc47fbcaa--------------------------------)

## 神经网络

[查看列表](https://medium.com/@egorhowell/list/neural-networks-616db722dbbb?source=post_page-----5babc47fbcaa--------------------------------)9 个故事![](img/66f86f14ddf20d9da9cac53dee54bae3.png)![](img/d968b0bd4358bb0d60bd296aef08a720.png)![](img/30f3f4354836e6d3d56175fe20cf6b7c.png)

神经网络在 [***计算机视觉***](https://en.wikipedia.org/wiki/Computer_vision) 领域取得了显著进展。这是自驾车和面部识别的 AI！

然而，大多数人所了解的常规全连接神经网络并不适合许多现实中的图像识别任务。它在著名的 [***MNIST***](https://en.wikipedia.org/wiki/MNIST_database#:~:text=Article%20Talk,the%20field%20of%20machine%20learning.) 数据集上有效，但其图像尺寸为 ***28×28*** 像素。

高清 (HD) 图像有 ***1280×720*** 像素。这大约是 ***1,000,000*** 像素，这意味着输入层需要 ***1,000,000 个神经元***。更不用说隐藏层所需的数百万个权重，这使得常规神经网络因维度复杂性而不适用。

那么，我们该怎么做？

卷积神经网络！

[***卷积神经网络 (CNN)***](https://en.wikipedia.org/wiki/Convolutional_neural_network) 现在是大多数计算机视觉任务的金标准。它们不像全连接层那样，而是具有 *部分* 连接的层，并且共享其权重，从而减少模型的复杂性。

例如，对于全连接神经网络层中的每个神经元，我们需要处理一张 ***100×100*** 像素的图像的 ***10,000*** 个权重。然而，CNN 只需 ***25*** 个神经元即可处理相同的图像。

在这篇文章中，我们将深入探讨 CNN 的基本构建块，[***卷积***](https://en.wikipedia.org/wiki/Convolution)。

# 直觉

就像机器学习中的许多事物一样，CNN 也受到自然的启发。计算机科学家观察了大脑中的 [***视觉皮层***](https://en.wikipedia.org/wiki/Visual_cortex) 的工作方式，并将类似的概念应用于神经网络。

视觉皮层不会同时处理图像中的所有像素。生物神经元只对 [***感受野***](https://en.wikipedia.org/wiki/Receptive_field) 中的小区域做出反应。然而，这些小区域会重叠，使动物能够理解整个图像。

此外，一些具有相同感受野的生物神经元只能检测不同方向的线条，这是 [***滤波***](https://medium.com/@sumit-kr-sharma/image-filtering-in-computer-vision-ec60ec8a3e1) 的一个例子。一些神经元还具有更大的感受野，这意味着高层神经元是低层神经元的组合。

这些生物学发现启发了 1979 年的 [***新认知机***](https://en.wikipedia.org/wiki/Neocognitron)。随着时间的推移，新认知机演变成了卷积神经网络，这就是我们今天所拥有的！

# 什么是卷积？

CNN 的命名来源于 [***卷积***](https://en.wikipedia.org/wiki/Convolution#:~:text=In%20mathematics%20(in%20particular%2C%20functional,is%20modified%20by%20the%20other.) 这个数学操作，其定义如下：

![](img/e9e408c33cfac8e5cf1d05eaeae61c99.png)

连续卷积定理。作者用 LaTeX 表示的方程式。

![](img/e0b04fd904fe239a320a742507aadd1a.png)

离散卷积定理。作者用 LaTeX 表示的方程式。

+   ***f*∗*g:*** 函数 ***f*** 和 ***g*** 之间的卷积。

+   ***t:*** 卷积被评估的点。

+   ***f(τ):*** 在点 ***τ*** 处的函数 ***f*** 值。

+   ***g(t−τ):*** 被 ***τ*** 移动并在 ***t*** 处评估的 ***g*** 值。

这个表达式并没有直观地告诉我们卷积是什么。让我们用更通俗的术语来解释一下。

卷积的作用是混合两个函数。在上述情况下，我们有输入的 ***f***，并且我们在 ***f*** 上滑动某个函数 ***g***（称为内核）。

1.  **输入**：这是一个函数，在我们的情况下是图像，用于分析或操作。

1.  **内核**：一个小矩阵，用于对图像应用视觉效果，例如模糊、锐化、边缘检测等。

1.  **滑动内核**：内核一次滑过输入图像一个像素，计算新的像素值。

1.  **计算输出**：每个像素的输出是通过将输入和内核的重叠值相乘并求和这些乘积来计算的。然后，通过除以内核中的元素数量进行归一化。

1.  **结果**：输出是一个新图像，这个图像已经被内核转换过。

# 示例卷积

让我们通过一些视觉示例来了解图像处理的简单卷积示例。

在下面的图示中，我们有一个输入的灰度图像，其尺寸为 ***5x5*** 像素，以及一个 ***3x3*** 的内核，其中全是 ***1s***，这将产生模糊效果（特别是一个 [***箱型模糊***](https://en.wikipedia.org/wiki/Box_blur)）。

![](img/ab2eb9f458f2ed5e17b27b1411c48567.png)

示例卷积用于在灰度图像上应用模糊效果。图示由作者创建。

> 像素值越小，图像越暗。值为 0 表示黑色，255 表示白色，中间的值则是灰度值。

如果我们取中间的像素（用红色高亮显示），其卷积输出是通过将每个像素值与内核中对应的元素相乘并求和这些乘积来计算的。然后，通过内核中的元素数量进行归一化，以确保图像不会变亮或变暗。

以下是此过程的逐步讲解。

```py
[30*1 + 30*1 + 30*1] +
[30*1 + 70*1 + 30*1] +
[30*1 + 30*1 + 30*1] 

= 30 + 30 + 30 + 30 + 70 + 30 + 30 + 30 + 30 = 310

pixel value = 310 / 9 ~ 34
```

然后对输入图像中的所有其他像素（仅在内核适合的地方）重复此操作，以生成所示输出。

请注意，结果图像的像素值现在比原始图像的像素值更接近。这是因为我们的内核对邻域中的像素值进行了平均，产生了模糊效果。

这种卷积可以很容易地在 Python 中实现，并显示结果图像：

```py
# Import packages
import numpy as np
from scipy.signal import convolve2d
import matplotlib.pyplot as plt

# Input Image
image = np.array([
    [10, 10, 10, 10, 10],
    [10, 30, 30, 30, 10],
    [10, 30, 70, 30, 10],
    [10, 30, 30, 30, 10],
    [10, 10, 10, 10, 10]
])

# Kernel
kernel = np.array([
    [1, 1, 1],
    [1, 1, 1],
    [1, 1, 1]
]) / 9

# Convolution
result = convolve2d(image, kernel, mode='same', boundary='fill', fillvalue=0)

# Plot
plt.figure(figsize=(10, 5))

# Input Image
plt.subplot(1, 2, 1)
plt.imshow(image, cmap='gray')
plt.title('Input Image')
plt.axis('off')

# Output Image
plt.subplot(1, 2, 2)
plt.imshow(result, cmap='gray')
plt.title('Output Image')
plt.axis('off')

plt.show()
```

![](img/0608af17b582be4c58fc0df0fed9a0e3.png)

模糊图像的示例。图表由作者在 Python 中创建。

用于生成此图像的代码可以在我的 GitHub 上找到：

[](https://github.com/egorhowell/Medium-Articles/blob/main/Neural%20Networks/convolution_image.py?source=post_page-----5babc47fbcaa--------------------------------) [## Medium-Articles/Neural Networks/convolution_image.py at main · egorhowell/Medium-Articles

### 我在我的中等博客/文章中使用的代码。通过创建帐户来为 egorhowell/Medium-Articles 的开发做出贡献...

[github.com](https://github.com/egorhowell/Medium-Articles/blob/main/Neural%20Networks/convolution_image.py?source=post_page-----5babc47fbcaa--------------------------------)

某些卷积核结构对图像有不同的效果。下面是一些常用卷积核及其效果：

![](img/1fce459dc8be4ee5e91e85c3e6a787cc.png)

卷积核及其效果。作者的图示。

> 一份完整的卷积核及其操作列表可以在[这里](https://en.wikipedia.org/wiki/Kernel_(image_processing))找到。

# 维度、步幅与填充

## 输出维度

你可能已经注意到，与原始图像相比，应用卷积核会导致图像尺寸减小。推导输出尺寸的公式是：

![](img/49542c2ac79fe5b6c3b51ab0b4aff936.png)

输出维度。作者的 LaTeX 公式。

+   ***H_in*** 和 ***W_in*** 是输入图像的高度和宽度。

+   ***H_filter*** 和 ***W_filter*** 是卷积核的高度和宽度。

+   ***Padding_height*** 和 ***Padding_width*** 是应用于输入图像高度和宽度的填充量。

+   ***Stride_height*** 和 ***Stride_width*** 是卷积核在输入图像高度和宽度上的步长。

我们尚未讨论的两个概念出现在上述方程中，[***步幅***](https://deepai.org/machine-learning-glossary-and-terms/stride)和[***填充***](https://www.geeksforgeeks.org/cnn-introduction-to-padding/)。

## 步幅

步幅是我们在输入图像上滑动卷积核的像素数。步幅为 1 表示卷积核移动一个像素，步幅为 2 表示卷积核每次移动两个像素。滑动可以是垂直、水平，或根据设置同时进行。

增加步幅会导致：

+   *减少输出图像的尺寸大小。*

+   *由于操作较少，计算成本更低。*

+   *输入图像的视野范围更大。*

+   *跳过像素会导致信息丢失。*

![](img/e8c11ade7b1193a03d5be9b18838096e.png)

步幅为 1 的示例。作者的图示。

## 填充

卷积的一个问题是我们往往会*丢失像素*和图像边缘的信息。这取决于卷积核的使用次数。角落像素只会被使用一次，而中间像素则会使用得更多。

如果我们多次应用卷积核，这在 CNN 中会发生，输出图像会比原始输入图像小很多。这不好，因为我们会丢失大量信息，特别是关于图像边界的信息。

解决这个问题的一种方法是对输入图像的边缘进行***零填充***。下面是使用前面部分图像的示例：

![](img/b244fc04933d33006aaa84a4c73d642b.png)

零填充。作者的图示。

输入图像现在是***7x7***像素。使用上述公式，我们可以验证，应用一个***3x3***的核到这个填充图像上将得到一个***5x5***尺寸的输出。

![](img/a216e333bd85f1c7de7128f1dfb824ea.png)

应用填充时输出维度的示例计算。方程由作者用 LaTeX 表示。

这将增加周边像素的利用率，并使输出图像具有与原始输入图像相同的尺寸。

零填充是**相同填充**的一种示例。完全不填充称为**有效填充**。还有另一种类型叫做**因果填充**，它是不对称的，通常应用于时间序列和自然语言处理。

我们不一定总是要使用零填充，尽管这是一种最常见的技术。以下是一些其他填充策略的列表：

+   [**反射/镜像填充**](https://dev.to/sally20921/padding-in-neural-network-o8m)：我们通过在边界周围反射图像来进行填充。

+   [**复制填充**](https://www.educative.io/answers/types-of-padding-in-images)：填充值是最接近边界的像素值。

# 摘要与进一步思考

卷积神经网络（CNN）是目前计算机视觉任务的金标准。它们的主要特点是利用卷积数学运算，这使我们能够将两个函数“融合”在一起。这种方法在图像处理中通过在我们的图像上应用一个矩阵（即核），来实现模糊和锐化等效果。这样我们就能操控和分析图像，这是 CNN 的基础，也是它们如何“看”图像的方式。

# 另一个话题！

我有一个免费的新闻通讯，[**数据分享**](https://dishingthedata.substack.com/)，在其中我每周分享成为更好的数据科学家的小贴士。没有“虚头八脑”的内容或“点击诱饵”，只有来自一名实践数据科学家的纯粹可操作的见解。

[](https://newsletter.egorhowell.com/?source=post_page-----5babc47fbcaa--------------------------------) [## 数据分享 | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读《数据分享》，作者 Egor Howell，Substack 出版的……

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----5babc47fbcaa--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell?sub_confirmation=1)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考文献和进一步阅读

+   [*关于核的精彩计算机视频*](https://www.youtube.com/watch?v=C_zFhWdM4ic)

+   *关于 CNN 的好文章*

+   [*动手学习 Scikit-Learn、Keras 和 TensorFlow，第 2 版。奥雷利安·热龙。2019 年 9 月。出版社：O’Reilly Media, Inc. ISBN: 9781492032649*](https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/)*.*

+   [*有关填充的更多信息*](https://www.knowledgehut.com/blog/data-science/padding-in-cnn)
