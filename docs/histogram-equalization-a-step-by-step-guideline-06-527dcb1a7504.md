# 直方图均衡化：逐步指南 (CV- 06)

> 原文：[`towardsdatascience.com/histogram-equalization-a-step-by-step-guideline-06-527dcb1a7504`](https://towardsdatascience.com/histogram-equalization-a-step-by-step-guideline-06-527dcb1a7504)

## 图像直方图均衡化详细说明

[](https://zubairhossain.medium.com/?source=post_page-----527dcb1a7504--------------------------------)![Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----527dcb1a7504--------------------------------)[](https://towardsdatascience.com/?source=post_page-----527dcb1a7504--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----527dcb1a7504--------------------------------) [Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----527dcb1a7504--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----527dcb1a7504--------------------------------) ·5 分钟阅读·2023 年 7 月 27 日

--

![](img/36c2593b53eee8c7ba0611e2ee487993.png)

原图由 [Dan Fador](https://pixabay.com/users/danfador-55851/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=190055) 提供，[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=190055) （左上图是主图，左下图是图像的灰度版本。右侧的图像是直方图均衡化的结果）

## 动机

直方图是通过条形图可视化频率分布的过程。在计算机视觉中，图像直方图是表示强度值频率的过程。通过图像直方图均衡化，我们可以轻松调整图像强度值的频率分布。通常，这个过程有助于提高图像的对比度和亮度。该过程简单易行。本文将讨论直方图均衡化的完整过程以及代码示例。

## 目录

1.  `**图像直方图**`

1.  `**直方图均衡化的完整过程**`

1.  `**逐步实施**`

## 图像直方图

***图像直方图*** 是用条形图表示图像强度值的频率。在图-1 中，我展示了一幅样本图像及其在二维空间中的强度值。

![](img/8cd94539290d7482144a9135265f244a.png)

图-1: 样本图像强度值（作者提供的图像）

值的范围从 0 到 7。让我们计算这些值的频率。

![](img/f4c9ef582838a962918f0877a1c0a482.png)

图-2: 图像强度值的频率（作者提供的图像）

***图像直方图*** 是对*频率*与*强度值*的简单表示，如*图-3*所示。

![](img/c4c701e5abe8be94b026ccdb0d1315c7.png)

图-3: 图像的直方图（作者提供的图片）

## 直方图均衡化的完整过程

直方图均衡化是通过一些函数均匀分布图像强度值的频率的过程。主要的函数是概率函数 — **PDF *(概率密度函数)* 和 CDF *(累积分布函数)*。**

+   **PDF** 是通过将强度值的频率除以总频率来计算的。

+   **CDF** 表示小于或等于特定值的概率分布的概率。例如，强度值的 PDF 为`*0 → 0.12, 1 → 0.24, 2 → 0.12, 等等。所以，1 的 CDF 为 0.12+0.24 = 0.36，2 为 0.36+0.12=0.48，以此类推。*`完整结果编译在***图-4***中。

![](img/6af823cd29ec69378976a9d055d32604.png)

图-4: 直方图均衡化计算（作者提供的图片）

最后，我们需要将**CDF**与一个整数相乘。

***选择数字的过程 —***

图像的**最大**强度值为 7，如*图 — 1*所示。我们必须用以下公式选择一个整数值。

![](img/bce5fc512e8fbc18d6d5220e3fb79e67.png)

这里，x 是表示最大强度值的最小位数。对于我们的情况，最大值为 7，`*(2³ -1 = 7)*`。如果我们选择 2 作为 x 的值，就无法表示最大强度值。因此，在我们的案例中，最佳乘法值是 7。

最终，我们将对乘法结果进行四舍五入以获得均衡直方图值。将强度值替换为均衡直方图值以找到最终输出。整个过程见**图-4**。

![](img/7b95792c81a910f73b1fd83a379b9483.png)

图-5: 初始强度值和均衡直方图强度值（作者提供的图片）

为了应用直方图均衡化，我们需要用均衡直方图值替换初始强度值。

新的图像如下 —

![](img/ecdcfa04d1871af7061b7c245cdab0f1.png)

图-6: 带有均衡直方图强度值的图像（作者提供的图片）

均衡直方图强度值的频率与之前的频率不同，如***图-6***所示。

![](img/7ddbc2b8a55ec67f6911f94ac3c84ed6.png)

图-6: 新的均衡频率（作者提供的图片）

图像强度频率的比较见*图-7*。

![](img/a47e4da1c8d7b3f17973a3f1bedd7a22.png)

图-7: 比较（作者提供的图片）

如果我们观察均衡直方图频率与初始频率的比较，会发现均衡直方图的强度值比初始值高，如*图-7*所示。

*现在是时候用 Python 进行实际操作了……*

## 分步实现

为了实现直方图均衡化，我们使用**OpenCV**库。

+   *让我们导入必要的库，并以灰度模式读取图像 —*

+   *计算图像的频率并绘制直方图——*

由于大多数图像部分较暗，低强度值（~0）的频率大于高强度值。

+   ***OpenCV*** *函数* `***cv2.equalizeHist()***` *有助于实现直方图均衡化——*

如果我们仔细观察输出图像，可能会注意到图像的亮度高于原始图像。

+   *让我们设置均衡化直方图图像——*

强度值的频率比原始图像更加均匀分布。

## 处理彩色图像

RGB/BGR 图像有 3 个通道—— ***红色、绿色和蓝色。*** 要在图像中应用直方图均衡化，我们需要将 RGB/BGR 图像转换为 `**HSV** *(色调、饱和度和值)*` 图像。最后，我们对 **HSV** 图像的值参数应用直方图均衡化。

+   应用直方图均衡化，并使用 `**matplotlib**` 将其转换为 RGB 图像进行可视化。

显然，图像的亮度和对比度显著提高了。

*如果你愿意，可以绘制之前展示的直方图和均衡化直方图。*

## 结论

我们生活在生成式 AI 的世界里。这些基本的计算机视觉技术看起来非常基础和过时，但它们是创建强大模型或创新的基础。所以，我总是强调知识的基础。

*我已经开始撰写一系列计算机视觉文章。之前的文章如下嵌入——*

[](/getting-started-with-numpy-and-opencv-for-computer-vision-555f88536f68?source=post_page-----527dcb1a7504--------------------------------) ## NumPy 和 OpenCV 在计算机视觉中的入门 (CV-01)

### 用 Python 开始你的计算机视觉编码

towardsdatascience.com [](/how-color-is-represented-and-viewed-in-computer-vision-b1cc97681b68?source=post_page-----527dcb1a7504--------------------------------) ## 计算机视觉中色彩表示的综合指南 (CV-02)

### 色彩空间和颜色模型的详细解释

towardsdatascience.com [](/blend-images-and-create-watermark-with-opencv-d24381b81bd0?source=post_page-----527dcb1a7504--------------------------------) ## 图像融合的最简单指南 (CV-03)

### 最简单的图像融合和粘贴指南

towardsdatascience.com [](/thresholding-a-way-to-make-images-more-visible-b3e314b5215c?source=post_page-----527dcb1a7504--------------------------------) [## 阈值化——使图像更可见的一种方法 (CV-04)

### 使用阈值化从图像中提取更多信息

[阈值处理：使图像更清晰可见](https://towardsdatascience.com/thresholding-a-way-to-make-images-more-visible-b3e314b5215c?source=post_page-----527dcb1a7504--------------------------------) [形态学操作：去除图像失真](https://towardsdatascience.com/morphological-operations-a-way-to-remove-image-distortion-513d162e7d05?source=post_page-----527dcb1a7504--------------------------------) [## 计算机视觉中的形态学操作及其模拟 (CV-05)

### 图像处理中的形态学操作最简单解释

[形态学操作：去除图像失真](https://towardsdatascience.com/morphological-operations-a-way-to-remove-image-distortion-513d162e7d05?source=post_page-----527dcb1a7504--------------------------------)
