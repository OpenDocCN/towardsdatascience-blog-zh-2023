# 使用 Python 的图像滤镜

> 原文：[`towardsdatascience.com/image-filters-with-python-3dc223a12624`](https://towardsdatascience.com/image-filters-with-python-3dc223a12624)

## 一个简洁的计算机视觉项目，用于使用 Python 构建图像滤镜

[](https://bharath-k1297.medium.com/?source=post_page-----3dc223a12624--------------------------------)![Bharath K](https://bharath-k1297.medium.com/?source=post_page-----3dc223a12624--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3dc223a12624--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3dc223a12624--------------------------------) [Bharath K](https://bharath-k1297.medium.com/?source=post_page-----3dc223a12624--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3dc223a12624--------------------------------) ·8 分钟阅读·2023 年 2 月 10 日

--

![](img/8358092c7a80daf900a213c1dcdeddeb.png)

照片由[Pineapple Supply Co.](https://unsplash.com/@pineapple?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

图像存在于不同的尺度、对比度、位深和质量中。我们被各种独特和美丽的图像包围，这些图像遍布我们的周围和互联网。操作这些图像可以产生一些有趣的结果，这些结果被用于各种有趣和有用的应用。

在图像处理和计算机视觉中，操作图像是解决不同任务和获得各种项目期望结果的关键组成部分。通过正确处理图像任务，我们可以重新创建一个修改后的图像版本，这对多种计算机视觉和深度学习应用（如数据增强）非常有用。

在本文中，我们将重点开发一个简单的图像滤镜应用程序，主要用于修改特定图像的亮度和对比度。还可以实现并添加到项目中的其他一些显著修改，包括着色器样式、剪贴画、表情符号和其他类似的附加内容。

如果读者不熟悉计算机视觉和 OpenCV，我建议查看我之前的一篇文章，内容是关于 OpenCV 和计算机视觉的全面初学者指南。相关链接如下。我建议在继续阅读本文其余内容之前先查看它。

[](/opencv-complete-beginners-guide-to-master-the-basics-of-computer-vision-with-code-4a1cd0c687f9?source=post_page-----3dc223a12624--------------------------------) ## OpenCV: 完整的初学者指南掌握计算机视觉基础及代码！

### 一个教程，包含代码，旨在掌握计算机视觉的所有重要概念及其使用 OpenCV 实现的方法

[towardsdatascience.com

# 使用 Python 的亮度和对比度调整器的起始代码：

![](img/9f846b0b9849d618ea85a912e2e512e8.png)

照片由 [Jacopo Maia](https://unsplash.com/@ja_ma?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本节中，我们将查看一个简单的起始代码，这将帮助我们开始使用 OpenCV 计算机视觉库来修改原始图像的亮度和对比度的基本图像过滤器。为此任务，我们将使用随机图像来测试示例代码并理解其基本工作原理。

为了理解这个测试案例，我使用上面的图像作为测试样本来分析和理解亮度和对比度调整器的工作过程。为了跟进这个项目，我强烈建议下载上述图像并将其存储为 test_image.jpg 在工作目录中。

请注意，你可以用任何其他功能名称存储图像，并使用任何其他格式的图像。唯一的重要步骤是读取图像时提及适当的名称及其格式。第一步是导入 OpenCV 库，如下面的代码块所示，并确保该库完全正常运行。

```py
# Importing the computer vision library
import cv2
```

一旦导入库并验证其工作，我们可以继续读取我们最近在工作目录中保存的原始图像。只需提及图像名称，cv2.imread 函数就能读取图像。

如果图像未存储在工作目录中，请确保提及特定文件及其图像名称。由于上述图像的原始尺寸为 4608 x 3072，将图像缩小一点并减少尺寸是最佳的。我已将图像调整为 (512, 512) 规模，以便更容易跟踪进度并以稍高的速度执行所需任务。

在下面代码示例的最后一步，我们将定义 alpha 和 gamma 参数，这些参数将分别作为对比度和亮度的调整器。使用这两个参数，我们可以相应地控制这些值。请注意，对比度参数的范围是 0 到 127，而亮度参数的范围是 0 到 100。

```py
# read the input image
image = cv2.imread('test_image.jpg')
image = cv2.resize(image, (512, 512))

# Define the alpha and gamma parameters for brightness and contrast accordingly
alpha = 1.2
gamma = 0.5
```

一旦我们完成了所有之前的步骤，我们将继续使用 Open CV 库中的“add weighted”函数，这有助于我们计算 alpha 和 gamma（亮度和对比度值）。该函数主要用于通过使用 alpha、beta 和 gamma 值来混合图像。请注意，对于单个图像，我们可以将 beta 设为零，并获得适当的结果。

最后，我们将在计算机视觉窗口中显示修改后的图像，如下面的示例代码块所示。一旦图像显示出来，当用户点击关闭按钮时，我们可以继续关闭窗口。为了进一步理解 Open CV 的一些基本函数，我强烈建议查看我之前提到的文章以获得更多信息。

```py
# Used the added weighting function in OpenCV to compute the assigned values together
final_image = cv2.addWeighted(image, alpha, image, 0, gamma)

# Display the final image
cv2.imshow('Modified Image', final_image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

在上述起始代码示例中，参数 alpha 作为对比度，gamma 作为亮度调整器。然而，修改这些参数以适应多种不同的变化可能会稍显不切实际。因此，最好的方法是创建一个 GUI 界面，并为所需的图像滤镜找到最佳值。这个话题将在接下来的部分中进一步讨论。

# 带有控制器的项目进一步开发：

![](img/f1ab8a7b56c6c240298dbf284792483b.png)

作者修改后的图像截图

我们可以使用 Open CV 创建一个自定义 GUI，利用控制器优化亮度和对比度参数，并相应调整我们的结果。我们可以为每个属性设置一个滑块，当滑动时，可以通过对每个亮度和对比度变化应用独特的滤镜来创建图像的不同效果。

前几步与上一部分类似，我们将导入 Open CV 库，读取相应的图像并按需调整其大小。读者可以选择适合自己的尺寸。以下是相关的代码片段。

```py
# Import the Open CV library
import cv2

# read the input image
image = cv2.imread('test_image.jpg')
image = cv2.resize(image, (512, 512))
```

在下一步中，我们将定义亮度和对比度函数，通过这些函数我们可以定义获取 trackbar 位置的函数，以获得当前亮度和对比度元素的位置。加权函数将用于计算这两个参数，以计算这两个组合的综合输出，如下面的代码块所示。

```py
# Creating the control function for the brightness and contrast
def BrightnessContrast(brightness=0):
    brightness = cv2.getTrackbarPos('Brightness',
                                    'Image')

    contrast = cv2.getTrackbarPos('Contrast',
                                  'Image')

    effect = cv2.addWeighted(image, brightness, image, 0, contrast)

    cv2.imshow('Effect', effect)
```

一旦定义了 trackbar 函数，我们将创建一个命名窗口和一个用于获取所需参数的 trackbar。我们将为亮度和对比度特性分别创建两个独立的 trackbars，如下面的代码块所示。当原始图像窗口和带有图像的 trackbar 窗口都关闭时，wait key 函数将激活，以帮助终止程序。

```py
# Defining the parameters for the Open CV window
cv2.namedWindow('Image')

cv2.imshow('Image', image)

cv2.createTrackbar('Brightness',
                    'Image', 0, 10,
                    BrightnessContrast) 

cv2.createTrackbar('Contrast', 'Image',
                    0, 20,
                    BrightnessContrast)  

BrightnessContrast(0)

cv2.waitKey(0)
```

我们可以调整亮度和对比度滑块的轨迹条位置，将数字分别从 0 到 10 和 0 到 20 进行更改。通过调整相应的位置可以观察到变化。然而，这个项目仍然有很大的改进空间，以及更高的参数调整尺度。我们可以在 255 的尺度上调整亮度值，在 127 的尺度上调整对比度，以获得更细致的图像。

为了解决这些问题并进一步提升这个项目，我建议访问 Geeks for Geeks 网站，我在本节中使用了一部分代码。强烈推荐读者查看以下[链接](https://www.geeksforgeeks.org/changing-the-contrast-and-brightness-of-an-image-using-python-opencv/)以进一步阅读和理解这个项目。

# 结论：

![](img/c744b0445c315f81cdae7f9f28dcdd51.png)

照片由 [Mailchimp](https://unsplash.com/@mailchimp?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上提供

> 我结合魔法和科学来创造幻觉。我从事新媒体和互动技术的工作，如人工智能或计算机视觉，并将它们融入我的魔法中。
> 
> **— *Marco Tempest***

图像在计算机视觉和图像处理中扮演着重要角色。通过修改图像以创建类似或高度过滤的变体，我们可以通过积累更多的数据来解决各种项目，比如数据增强或其他类似任务。

我们还可以通过对特定图像进行改进来实现更理想的效果，通过这种方式，可以执行其他有用的深度学习任务。在本文中，我们探索了通过添加亮度和对比度等特性来修改原始图像的使用。在第一部分中，我们学习了如何利用 alpha 和 gamma 参数作为亮度和对比度参数。

在接下来的部分中，我们开发了一个 GUI 界面，通过它可以操作原始图像以获取其修改版本的副本。所有任务都是在一些基本的计算机视觉和图像处理知识及库的基础上完成的。

如果你想在我的文章发布时第一时间得到通知，可以查看以下[链接](https://bharath-k1297.medium.com/subscribe)以订阅电子邮件推荐。如果你希望支持其他作者和我，请订阅以下链接。

[](https://bharath-k1297.medium.com/membership?source=post_page-----3dc223a12624--------------------------------) [## 使用我的推荐链接加入 Medium - Bharath K

### 阅读 Bharath K（以及 Medium 上成千上万的其他作者）的每一个故事。您的会员费用将直接支持…

[bharath-k1297.medium.com](https://bharath-k1297.medium.com/membership?source=post_page-----3dc223a12624--------------------------------)

如果你对本文中的各个点有任何疑问，请在下方评论中告诉我。我会尽快回复你。

查看我与本文主题相关的其他文章，你可能也会喜欢阅读！

[前往数据科学](https://towardsdatascience.com/the-ultimate-replacements-to-jupyter-notebooks-51da534b559f?source=post_page-----3dc223a12624--------------------------------) [## Jupyter Notebooks 的终极替代方案

### 讨论 Jupyter Notebooks 在数据科学项目中的一个优秀替代选项

[前往数据科学](https://towardsdatascience.com/the-ultimate-replacements-to-jupyter-notebooks-51da534b559f?source=post_page-----3dc223a12624--------------------------------) [](/7-best-research-papers-to-read-to-get-started-with-deep-learning-projects-59e11f7b9c32?source=post_page-----3dc223a12624--------------------------------) [## 阅读七篇最佳研究论文，启动深度学习项目

### 七篇经得起时间考验的最佳研究论文，将帮助你创建出色的项目

[前往数据科学](https://towardsdatascience.com/7-best-research-papers-to-read-to-get-started-with-deep-learning-projects-59e11f7b9c32?source=post_page-----3dc223a12624--------------------------------) [](/develop-your-own-spelling-check-toolkit-with-python-740bf84a865d?source=post_page-----3dc223a12624--------------------------------) [## 使用 Python 开发自己的拼写检查工具包

### 使用 Python 创建一个有效的拼写检查应用程序

[前往数据科学](https://towardsdatascience.com/develop-your-own-spelling-check-toolkit-with-python-740bf84a865d?source=post_page-----3dc223a12624--------------------------------)

感谢大家一直看到最后。希望你们喜欢阅读这篇文章。祝大家有美好的一天！
