# 开始使用 NumPy 和 OpenCV 进行计算机视觉 (CV-01)

> 原文：[`towardsdatascience.com/getting-started-with-numpy-and-opencv-for-computer-vision-555f88536f68`](https://towardsdatascience.com/getting-started-with-numpy-and-opencv-for-computer-vision-555f88536f68)

## 开始使用 Python 进行计算机视觉编程

[](https://zubairhossain.medium.com/?source=post_page-----555f88536f68--------------------------------)![Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----555f88536f68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----555f88536f68--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----555f88536f68--------------------------------) [Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----555f88536f68--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----555f88536f68--------------------------------) ·8 分钟阅读·2023 年 3 月 15 日

--

![](img/a1f29b1c045ba48aeb7b58ca7d9aece1.png)

[Dan Smedley](https://unsplash.com/@nadyeldems?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

## 动机

我们人类通过视觉系统感知环境和周围事物。人眼、大脑和四肢协同工作以感知环境并做出相应的动作。一个智能系统能够执行那些如果由人类完成则需要某种程度智力的任务。因此，为了执行智能任务，人工视觉系统是计算机的重要组成部分。通常，使用相机和图像来收集完成工作的所需信息。计算机视觉和图像处理技术帮助我们完成类似人类的任务，如图像识别、对象跟踪等。

在计算机视觉中，相机像人眼一样捕捉图像，处理器像大脑一样处理捕捉到的图像并生成重要结果。但人类和计算机之间有一个基本的区别。人脑自动工作，智力是与生俱来的。而计算机没有智力，除非有人的指令（程序）。计算机视觉就是提供适当的指令，使计算机能够与人类视觉系统兼容。然而，其能力是有限的。

`*在接下来的部分中，我们将讨论图像如何形成及如何使用 Python 进行操作的基本概念。*`

## 目录

`*点击目录直接跳转到相应章节*`

1.  `**图像如何形成和显示**`

1.  `**计算机如何在内存中存储图像？**`

1.  `**灰度图像和彩色图像**`

1.  `**NumPy 基础以处理图像**`

1.  `**OpenCV 基础**`

1.  `**玩转 NumPy**`

## 图像的形成与显示

图像只不过是具有不同颜色强度的像素组合。‘像素’和‘颜色强度’这两个术语可能对你来说是陌生的。不要担心。阅读完文章后，一切都会变得清晰明了。

**像素**是数字图像的最小单位/元素。详细信息见下图。

![](img/760bb7c8723d0bd48ae408ef43565950.png)

作者提供的图像

显示屏由像素构成。在上图中，有 25 列和 25 行。每个小方块被视为一个像素。这个设置可以容纳 625 个像素。它表示一个具有 625 个像素的显示屏。如果我们用不同的**颜色强度**（亮度）来照亮这些像素，它将形成一个数字图像。

## **计算机如何将图像存储在内存中？**

如果我们仔细观察图像，可以将其与 2D 矩阵进行比较。矩阵有行和列，其元素可以通过索引访问。矩阵结构类似于数组。计算机将图像存储在计算机内存的数组中。

每个数组元素保存**强度**值。通常，强度值范围从`**0 到 255**`。为了演示目的，我提供了图像的数组表示。

![](img/854daf2155323f2f5cdde6fad18b0a80.png)

灰度图像的示例数组表示（作者提供的图像）

## 灰度图像与彩色图像

**灰度图像**是一种黑白图像。它由一种颜色形成。接近 0 的像素值代表黑暗，强度值越高，图像就越亮。最高值是 255，代表白色。一个 2D 数组足以存储灰度图像，正如最后的图示所示。

**彩色图像**不能仅由一种颜色形成；可能有成千上万种颜色组合。主要有三种基本颜色通道`**红色 (R)、绿色 (G) 和蓝色 (B)**`。每个颜色通道存储在一个*2D 数组*中，并保存其强度值，最终图像是这三个颜色通道的组合。

![](img/c59a3df10731dbae40ebf5e3ea60d76b.png)

RGB 颜色通道（作者提供的图像）

这种颜色模型具有 (256 x 256 x 256) = 16,777,216 种可能的颜色组合。 `[***你可以在这里可视化这些组合。***](https://csunplugged.jp/csfg/data/interactives/rgb-mixer/index.html)`

但在计算机内存中，图像的存储方式有所不同。

![](img/357d8039806f954a8e72102e575d233f.png)

存储在计算机内存中的图像（作者提供的图像）

计算机不知道 RGB 通道。它知道的是强度值。红色通道以高强度存储，绿色和蓝色通道分别以中等和低强度值存储。

## NumPy 基础知识以便与 Python 一起使用

NumPy 是一个用于科学计算的基础 Python 包。它主要作为数组对象工作，但它的操作不限于数组。然而，该库可以处理各种数字和逻辑操作[1]。`你可以在[***这里***](https://numpy.org/doc/stable/user/absolute_beginners.html)找到 NumPy 的官方文档。`

*让我们开始我们的旅程。首先是第一步。*

+   ***导入 NumPy 库。***

现在是使用 NumPy 的时候了。正如我们所知，NumPy 与数组一起工作。所以，让我们尝试创建第一个全零的 2D 数组。

+   ***创建 NumPy 数组***

就这么简单。我们也可以创建一个全为 1 的 NumPy 数组，方法如下。

有趣的是，NumPy 还提供了一种方法来用任意值填充数组。简单的语法`array.fill(value)`可以完成这项工作。

数组`‘b’`中所有的元素现在都被填充为`3`。

+   ***随机数生成中的种子功能***

只需看看以下的代码示例。

在第一个代码单元格中，我们使用了`**np.random.seed(seed_value)**`，但其他两个代码单元格中没有使用任何种子。在有种子和无种子的随机数生成之间有一个主要区别。在随机种子的情况下，生成的随机数对于特定的种子值保持不变。另一方面，没有种子值的情况下，每次执行时随机数都会变化。

+   **使用 NumPy 的基本操作（最大值、最小值、均值、重塑等）**

NumPy 通过提供大量函数来执行数学运算，使我们的生活更轻松。`array_name.min(), array_name.max(), array_name.mean()`语法帮助我们找到数组的最小值、最大值和均值。*编码示例 —*

最小值和最大值的索引可以通过语法`array_name.argmax(), array_name.argmin()`提取。*示例 —*

数组重塑是`NumPy.array_name.reshape(row_no, column_no)`的一个重要操作，用于重塑数组。在重塑数组时，我们必须注意重塑前后数组元素的数量。在两种情况下，总元素数必须相同。

+   **数组索引和切片**

每个数组元素可以通过其`列和行`编号进行访问。我们来生成另一个具有 10 行和列的数组。

假设我们想找出数组的第一个值。可以通过传递行和列索引（0 , 0）来提取。

可以使用语法`array_name[row_no,:], array_name[:,column_no]`来切片特定的行和列值。

让我们尝试切片数组的中心元素。

## OpenCV 基础

**OpenCV**是由英特尔开发的开源 Python 计算机视觉库[2]。我将讨论 OpenCV 的一些用法，尽管它的范围很广。`你可以在[**这里**](https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html)找到官方文档。`

`我使用了以下图像进行演示。`

![](img/65956af1e9aa9a01bfa553246358120a.png)

图像由[jackouille21](https://pixabay.com/users/jackouille21-34177786/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7834580)提供，来源于[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7834580)

+   **导入 OpenCV 和 Matplotlib 库**

Matplotlib 是一个可视化库。它有助于可视化图像。

+   **使用 OpenCV 加载图像并用 matplotlib 可视化**

我们已经用**OpenCV**读取了图像，并用**matplotlib**库对其进行了可视化。颜色发生了变化，因为**OpenCV**以**BGR**格式读取图像，而**matplotlib**期望图像为**RGB**格式。因此，我们需要将图像从**BGR 转换为 RGB**。

+   **将图像从 BGR 格式转换为 RGB 格式**

现在，图像看起来还不错。

+   **将图像转换为灰度**

我们可以轻松地使用`cv2.COLOR_BGR2GRAY`将图像从 BGR 转换为灰度图像，操作如下。

尽管图像已经转换为灰度图像，但上述图像并不完全灰色。它已通过 matplotlib 进行了可视化。默认情况下，matplotlib 使用的是不同于灰度的颜色映射。为了正确可视化它，我们需要在 matplotlib 中指定灰度颜色映射。让我们来做一下。

+   **旋转图像**

旋转也是一个简单的任务，使用`OpenCV`。`cv2.rotate()`函数可以帮助我们完成这项工作。`顺时针和逆时针 90 度以及 180 度`旋转如下所示。

+   **调整图像大小**

我们可以通过将宽度和高度的像素值传递给`cv2.resize()`函数来调整图像大小。

+   **在图像上绘制**

有时我们需要在现有图像上绘制。例如，我们需要在图像对象上绘制一个边界框以进行识别。让我们在花朵上绘制一个矩形。`cv2.rectangle()`函数帮助我们在其上绘制。*它需要一些参数，比如我们绘制矩形的图像、左上角坐标点* `*(pt1)*` *和右下角坐标点* `*(pt2)*`*，以及边界线的厚度。下面是一个编码示例。*

还有其他绘图函数`[***cv.line()***](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#ga7078a9fae8c7e7d13d24dac2520ae4a2)*,* [***cv.circle()***](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#gaf10604b069374903dbd0f0488cb43670) *,* [***cv.ellipse()***](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#ga28b2267d35786f5f890ca167236cbc69)*,* [***cv.putText()***](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html#ga5126f47f883d730f633d74f07456c576)*,* 等。完整的官方文档可以在`[**这里**](https://docs.opencv.org/4.x/dc/da5/tutorial_py_drawing_functions.html)`**[3]**中找到。

## 玩转 NumPy

我们将改变图像的强度值。我会尽量保持简单。因此，考虑之前显示的灰度图像。*找出图像的形状。*

它显示这是一个`2D 数组，大小为**1200 x 1920**`。在基本的 NumPy 操作中，我们学习了如何切片数组。

使用该概念，我们提取了灰度图像数组片段`**[400:800, 750:1350]**`并将强度值替换为`**255**`。最后，我们对其进行可视化，得到了上述图像。

## 结论

计算机视觉是现代计算机科学技术中一个有前途的领域。我总是强调任何领域的基础知识。我仅讨论了计算机视觉的基本知识，并展示了一些实践编码。这些概念非常简单，但对于计算机视觉的初学者可能起到重要作用。

`***完整*** [***代码***](https://github.com/Zubair063/ML_articles/blob/main/Getting%20Started%20with%20NumPy%20and%20OpenCV/Numpy_OpenCV.ipynb) ***可在此处*** [***获取***](https://github.com/Zubair063/ML_articles/tree/main/Getting%20Started%20with%20NumPy%20and%20OpenCV) ***。***`

`这是计算机视觉系列的第一篇文章。请保持关注，以阅读即将发布的文章。`

*[注：讲师* [*Jose Portilla*](https://pieriantraining.com/) *的课程帮助我获取知识。]*

## 参考文献

1.  [NumPy 文档 — NumPy v1.25.dev0 手册](https://numpy.org/devdocs/)

1.  [OpenCV — 维基百科](https://en.wikipedia.org/wiki/OpenCV)

1.  [OpenCV：OpenCV 中的绘图函数](https://docs.opencv.org/4.x/dc/da5/tutorial_py_drawing_functions.html)

*您可以阅读下面提供的其他系列文章。*

[](/ultimate-guide-to-statistics-for-data-science-a3d8f1fd69a7?source=post_page-----555f88536f68--------------------------------) ## 数据科学的终极统计指南

### 数据科学概述：标准指南

[towardsdatascience.com [](https://medium.datadriveninvestor.com/ultimate-guide-to-data-visualization-for-data-science-90b0b13e72ab?source=post_page-----555f88536f68--------------------------------) [## 数据科学的终极数据可视化指南

### 数据科学概述：标准指南

[medium.datadriveninvestor.com](https://medium.datadriveninvestor.com/ultimate-guide-to-data-visualization-for-data-science-90b0b13e72ab?source=post_page-----555f88536f68--------------------------------)
