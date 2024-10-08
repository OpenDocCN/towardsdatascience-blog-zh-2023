# Sobel 算子在图像处理中的应用

> 原文：[`towardsdatascience.com/sobel-operator-in-image-processing-1d7cdda8cadb`](https://towardsdatascience.com/sobel-operator-in-image-processing-1d7cdda8cadb)

## Sobel 算子是什么及其用法示例

[](https://medium.com/@egorhowell?source=post_page-----1d7cdda8cadb--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----1d7cdda8cadb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d7cdda8cadb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d7cdda8cadb--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----1d7cdda8cadb--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d7cdda8cadb--------------------------------) ·7 分钟阅读·2023 年 12 月 31 日

--

![](img/1a9c4a638e4549d5bdf35076e9ba2b0e.png)

href=”[`www.flaticon.com/free-icons/image-processing`](https://www.flaticon.com/free-icons/image-processing)" title=”图像处理图标” 图像处理图标由 juicy_fish 创建 — Flaticon。

# 卷积概述

在我之前的文章中，我们深入探讨了 [***卷积神经网络 (CNNs)***](https://en.wikipedia.org/wiki/Convolutional_neural_network) 的关键构建块，即卷积数学算子。我强烈建议你查看那篇文章，因为它为这篇文章提供了背景和理解：

[](/convolution-explained-introduction-to-convolutional-neural-networks-5babc47fbcaa?source=post_page-----1d7cdda8cadb--------------------------------) ## 卷积解释 — 卷积神经网络简介

### CNNs 的基本构建块

towardsdatascience.com

简而言之，图像处理中的卷积是我们将一个小矩阵，称为核，应用于输入图像，以创建一个应用了某种效果的输出图像，如模糊或锐化。

从数学上讲，我们得到的是：

![](img/e0b04fd904fe239a320a742507aadd1a.png)

离散卷积定理。作者在 LaTeX 中的方程。

+   ***f*∗*g:*** 函数 ***f*** 和 ***g*** 之间的卷积。

+   ***f:*** 输入图像

+   ***g***: 核矩阵，也称为滤波器

+   ***t:*** 卷积正在计算的像素位置。

+   ***f(τ):*** 图像 ***f*** 在像素 ***τ*** 处的像素值。

+   ***g(t−τ):*** ***g*** 在 ***t*** 处被 ***τ*** 移动后的像素值。

以下是该过程的示例，我们对输入图像应用*盒状模糊*：

![](img/ab2eb9f458f2ed5e17b27b1411c48567.png)

这是一个对灰度图像应用模糊效果的卷积示例。图示由作者创建。

![](img/0608af17b582be4c58fc0df0fed9a0e3.png)

图像模糊示例。图示由作者在 Python 中创建。

卷积通过将输入图像的每个像素与卷积核中的相应元素相乘并求和这些乘积，然后按元素数量进行归一化来计算。

这是如上图所示的中间像素的示例：

```py
[30*1 + 30*1 + 30*1] +
[30*1 + 70*1 + 30*1] +
[30*1 + 30*1 + 30*1] 

= 30 + 30 + 30 + 30 + 70 + 30 + 30 + 30 + 30 = 310

pixel value = 310 / 9 ~ 34
```

根据卷积核的结构，我们可以对图像应用不同的效果。最有用的卷积核之一是[***Sobel 算子***](https://en.wikipedia.org/wiki/Sobel_operator#:~:text=The%20Sobel%20operator%2C%20sometimes%20called,Irwin%20Sobel%20and%20Gary%20M.)，这也是本文的主题！

# 什么是 Sobel 算子？

Sobel 算子是一个强大的边缘检测卷积核，它对机器学习特别是卷积神经网络非常有用。它通过计算图像在每个像素处的强度梯度（一级导数）的近似值来实现这一点。

Sobel 算子由两个卷积核组成，一个测量水平方向上的梯度，***x***，另一个测量垂直方向上的梯度，***y***。在数学上，它的形式如下：

![](img/90a14eaff74e59bf13f1723492807cd6.png)

Sobel 算子。公式由作者在 LaTeX 中提供。

这些卷积核与输入图像进行卷积，以计算我们所考虑的像素两侧的强度差异。

由于它们的大小为***3x3***，它们计算速度也很快，这对于处理深度学习和 CNNs 是一个巨大的好处。然而，这意味着它的分辨率较小，因此可能会检测到人眼认为不“真实”的边缘。

图像通常没有完美的水平或垂直边缘，因此我们需要计算梯度的总幅度。可以按照如下步骤进行计算：

![](img/8e3b51bedeeec27d72955d3374c11b1a.png)

梯度幅度。公式由作者在 LaTeX 中提供。

还有一个没有幂的版本，它计算速度更快但只是一个近似值：

![](img/9bc3d6ab01685577fed77a28c8776270.png)

梯度幅度的近似值。公式由作者在 LaTeX 中提供。

我们还可以通过使用*arctan*来计算边缘的角度：

![](img/b8ead798ee617d236fdcccc194473061.png)

边缘的角度。公式由作者在 LaTeX 中提供。

> 需要注意的是，Sobel 算子主要用于灰度图像，因为它测量强度的变化。

# Sobel 算子示例

现在，让我们通过一个示例来演示如何将 Sobel 算子应用于图像。

以下是我们的灰度输入图像，大小为***5x5*** 像素：

```py
Image:

  0   0   0   0   0
  0   0   0   0   0
200 200 200 200 200
200 200 200 200 200
200 200 200 200 200
```

如我们所见，前两行全是黑色，后三行全是白色，表明存在一个垂直边缘。

> 像素值越小，颜色越暗。值为 0 表示黑色，值为 255 表示白色，中间的值表示灰度级。

这些是 Sobel 算子：

```py
G_x:

-1  0  +1
-2  0  +2
-1  0  +1

G_y:

-1  -2  -1
 0   0   0
+1  +2  +1
```

现在让我们将 Sobel 算子应用于输入图像的中央***3x3***像素网格：

```py
Central Grid:

  0   0   0
255 255 255
255 255 255

Apply G_x:

(0*-1 + 0*0 + 0*1) +
(255*-2 + 255*0 + 255*2) +
(255*-1 + 255*0 + 255*1) =

0 + 0 + 0 +
-510 + 0 + 510 +
-255 + 0 + 255 = 0

Apply G_y:

(0*-1 + 0*-2 + 0*-1) +
(255*0 + 255*0 + 255*0) +
(255*1 + 255*2 + 255*1) =

0 + 0 + 0 +
0 + 0 + 0 +
255 + 510 + 255 = 1020
```

***G_x***为零，意味着在水平方向上没有梯度，这在我们查看输入图像的像素值时是有道理的。

梯度的幅值为：

```py
G = (G_x² + G_y²)⁰.5 = (0² + 1020²)⁰.5 = 1020
```

以及角度：

```py
theta = arctan(G_y/G_x) = arctan(1020/0) = undefined
```

由于除以零是未定义的，我们可以推断方向完全是垂直的，这意味着我们有一个水平边缘。

这些都可以在 Python 中完成，如下所示：

```py
# Import packages
import numpy as np
import matplotlib.pyplot as plt
from scipy import ndimage

# Input image
image = np.array([
    [0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0],
    [200, 200, 200, 200, 200],
    [200, 200, 200, 200, 200],
    [200, 200, 200, 200, 200]
])

# Sobel Operators
G_x = np.array([
    [-1, 0, +1],
    [-2, 0, +2],
    [-1, 0, +1]
])

G_y = np.array([
    [-1, -2, -1],
    [0, 0, 0],
    [+1, +2, +1]
])

# Apply the Sobel kernels to the image
output_x = ndimage.convolve(image.astype(float), G_x)
output_y = ndimage.convolve(image.astype(float), G_y)

# Define the light grey color for the background
light_grey = [0.8, 0.8, 0.8]  # RGB values for light grey

# Normalize the Sobel outputs
norm_output_x = (output_x - output_x.min()) / (output_x.max() - output_x.min())
norm_output_y = (output_y - output_y.min()) / (output_y.max() - output_y.min())

# Set the background color of the plots
plt.rcParams['figure.facecolor'] = light_grey
plt.rcParams['axes.facecolor'] = light_grey

# Plot the images with the light grey background
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

axes[0].imshow(image, cmap='gray')
axes[0].set_title('Original Image')
axes[0].axis('off')

axes[1].imshow(norm_output_x, cmap='gray')
axes[1].set_title('Sobel - Horizontal Direction')
axes[1].axis('off')

axes[2].imshow(norm_output_y, cmap='gray')
axes[2].set_title('Sobel - Vertical Direction')
axes[2].axis('off')

plt.tight_layout()
plt.show()
```

![](img/9da0879fdbbafd9d12ec73f225f0cf73.png)

Sobel 算子示例。由作者在 Python 中生成的图表。

水平 Sobel 算子未定义/为零，因为没有水平边缘。而垂直 Sobel 算子有强烈的响应，检测到黑色矩形显示的边缘区域。

代码可在我的 GitHub 上找到：

[## Medium-Articles/Neural Networks/sobel_operator.py at main · egorhowell/Medium-Articles](https://github.com/egorhowell/Medium-Articles/blob/main/Neural%20Networks/sobel_operator.py?source=post_page-----1d7cdda8cadb--------------------------------)

### 这是我在 Medium 博客/文章中使用的代码。通过创建一个帐户来参与 egorhowell/Medium-Articles 的开发…

[github.com](https://github.com/egorhowell/Medium-Articles/blob/main/Neural%20Networks/sobel_operator.py?source=post_page-----1d7cdda8cadb--------------------------------)

# 其他边缘检测器

Sobel 并不是唯一的边缘检测核/算法。以下是一些其他常见的算法的列表，供感兴趣的读者参考：

+   [**Canny 边缘检测器**](https://en.wikipedia.org/wiki/Canny_edge_detector): 可能是最受欢迎的边缘检测器，但有多个阶段，包括高斯模糊、梯度检测和抑制。

+   [**Prewitt 算子**](https://en.wikipedia.org/wiki/Prewitt_operator)**:** 非常类似于 Sobel 算子，因为它也使用两个 3x3 的卷积核来测量两个方向上的梯度强度。然而，Prewitt 算子的准确度略低于 Sobel，但对噪声的敏感度较低。

+   [**Robert Cross 算子**](https://en.wikipedia.org/wiki/Roberts_cross): 另一种差分算子，但使用 2x2 的卷积核，因此计算时间更快。

# 总结与进一步思考

在本文中，我们展示了如何使用 Sobel 算子检测图像中的边缘。该算子计算图像中单个像素的离散近似梯度。如果像素两侧的差异很大，它很可能位于边缘上。这是卷积神经网络通过找到这些类型的卷积核来工作的核心。

# 另外一件事！

我有一个免费的新闻通讯，[**数据深度解析**](https://dishingthedata.substack.com/)，我在其中分享成为更优秀的数据科学家的每周技巧。没有“花里胡哨”的内容或“点击诱饵”，只有来自实际数据科学家的纯粹可操作的见解。

[## 数据深度解析 | Egor Howell | Substack](https://newsletter.egorhowell.com/?source=post_page-----1d7cdda8cadb--------------------------------)

### 如何成为更好的数据科学家。点击阅读由 Egor Howell 发布的 Substack 文章《Dishing The Data》…

newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----1d7cdda8cadb--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell)

+   [**LinkedIn**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)

# 参考资料和进一步阅读

+   [*关于边缘检测的好读物*](https://www.cse.usf.edu/~r1k/MachineVisionBook/MachineVision.files/MachineVision_Chapter5.pdf)

+   [*关于 Sobel 算子的精彩视频*](https://www.youtube.com/watch?v=uihBwtPIBxM)

+   [*关于实施 Sobel 算子*](https://homepages.inf.ed.ac.uk/rbf/HIPR2/sobel.htm#:~:text=The%20Sobel%20operator%20performs%20a,in%20an%20input%20grayscale%20image.)
