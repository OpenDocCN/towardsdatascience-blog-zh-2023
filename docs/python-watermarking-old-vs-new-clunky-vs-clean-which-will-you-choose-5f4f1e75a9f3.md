# Python 水印：旧 vs 新，笨重 vs 干净 — 你会选择哪个？

> 原文：[`towardsdatascience.com/python-watermarking-old-vs-new-clunky-vs-clean-which-will-you-choose-5f4f1e75a9f3`](https://towardsdatascience.com/python-watermarking-old-vs-new-clunky-vs-clean-which-will-you-choose-5f4f1e75a9f3)

![](img/7b429df4182e762658cdb26e413f5f4d.png)

图片由[Siegfried Frech](https://pixabay.com/users/stilles_wasser-19985110/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7863868)提供，来源于[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7863868)

## Python 水印制作简化：OpenCV、PIL 和 filestools 的全面比较

[](https://christophertao.medium.com/?source=post_page-----5f4f1e75a9f3--------------------------------)![Christopher Tao](https://christophertao.medium.com/?source=post_page-----5f4f1e75a9f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5f4f1e75a9f3--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----5f4f1e75a9f3--------------------------------) [Christopher Tao](https://christophertao.medium.com/?source=post_page-----5f4f1e75a9f3--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----5f4f1e75a9f3--------------------------------) ·8 分钟阅读·2023 年 3 月 28 日

--

对图像进行水印处理是摄影师、艺术家以及任何希望保护其视觉内容免受未经授权使用的人的重要任务。在 Python 世界中，有许多库可以让你为图像添加水印。在本文中，我们将比较三种流行的 Python 图像水印方法：**OpenCV**、**PIL**（Python Imaging Library）和**filestools**。对于最后一种方法，你只需一行代码！

在这篇文章中，我将演示使用我在澳大利亚维多利亚州菲利普岛拍摄的照片的水印功能。原始照片在这里。请随意下载以便使用。

![](img/eb3c9f8a925a5b880f69a9a38eb16b8b.png)

作者拍摄的照片

# 1\. OpenCV — 小任务的大工具

![](img/907a3fa4f5f07147a6edf904a232c5ae.png)

图片由[Lukas](https://pixabay.com/users/computerizer-4588466/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2301646)提供，来源于[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2301646)

OpenCV 是一个综合性的计算机视觉库，提供了广泛的图像处理功能，包括向图像添加文本水印的能力。虽然 OpenCV 并非专门为添加水印设计，但它仍然提供了实现这一目标的灵活性和控制力。然而，使用 OpenCV 添加水印可能会有些挑战，尤其是对于那些不熟悉该库的人来说。此外，使用 OpenCV 实现基于图像的水印需要一些手动处理。

无论如何，让我们看看 OpenCV 如何为我们完成这项任务。

在一切之前，请确保如果你还没有安装库，需要安装它。只需使用 `pip` 如下。

```py
pip install opencv-python
```

要在 Python 代码中使用 OpenCV，我们需要导入 `cv2` 模块。为了使这个示例更简单，我还想导入 `matplotlib`，这样我就可以即时显示图像。

```py
import cv2
import matplotlib.pyplot as plt
```

OpenCV 使从本地路径读取图像变得非常简单。你只需使用 `imread` 函数即可。

```py
img = cv2.imread("my_photo.jpeg")
```

以下函数是可选的，我创建了这个函数以方便在 Jupyter Notebook 环境中内联显示图像。如果你想查看图像对象的样子，可以随意使用它。

```py
def show_image(img, is_cv=False):
    if is_cv:
        img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    plt.figure(figsize=(16, 9))
    plt.imshow(img)
    plt.axis("off")
    plt.show()
```

在上述函数中，我添加了 `is_cv` 来指定这个图像对象是否来自 OpenCV。我们需要这样做，因为我们可能希望以后将这个函数用于 PIL 库。OpenCV 图像对象默认使用 BGR 而不是 RGB。因此，我们需要使用 `cvtColor()` 函数来转换编码方法。

之后，使用 `matplotlib` 来显示图像。在我的例子中，我指定了一个适合浏览器窗口的大小。此外，由于我们只是显示图像，可以关闭坐标轴。`imshow()` 是显示图像对象的关键函数。

因此，我们可以简单地通过调用我们刚刚创建的函数来显示图像。

```py
show_image(img, is_cv=True)
```

![](img/69bd3e991353b3400af2c9ce91e3b491.png)

现在，让我们创建一个字符串，这个字符串就是我们想要添加到图像上的水印文本。接下来，我们需要配置字体。有几种 OpenCV 内置的字体样式可以选择。`font_scale` 将决定水印文本的大小。最后，我们可以创建一个元组作为颜色。`(255, 255, 255)` 将使水印文本为白色。

```py
watermark_text = "Christopher Tao @TDS"

# Set the font, font scale, and color of the text
font = cv2.FONT_HERSHEY_TRIPLEX
font_scale = 5
color = (255, 255, 255)
```

接下来是决定水印位置。`getTextSize()` 方法将帮助我们获取文本的大小。同时，我们可以从图像的 `shape` 属性中获取图像的维度。

```py
# Get the size of the text
text_size, _ = cv2.getTextSize(watermark_text, font, font_scale, thickness=20)

# Calculate the position of the text
x = int((img.shape[1] - text_size[0]) / 2)
y = int((img.shape[0] + text_size[1]) / 2)
```

然而，需要强调的是，图像的大小是“H x W”，而文本的大小是“W x H”。因此，当我们计算坐标时，需要使用图像形状中的第二项（宽度）减去文本大小中的第一项（宽度），反之亦然。

![](img/f5d88d16ac7391063fb46e12877ae946.png)

最后，我们可以使用 `putText()` 方法将水印文本添加到图像中，使用我们定义的所有参数。

```py
# Add the text watermark to the image
cv2.putText(img, watermark_text, (x, y), font, font_scale, color, thickness=2)
```

让我们看看结果。成功！

![](img/0c44dbe5e1c7d318a97064fba6d6134e.png)

# 2. PIL — 简化水印处理

![](img/a0d396c6b5078160d9ddaaa050590029.png)

图片来源：[an_photos](https://pixabay.com/users/an_photos-3160435/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4503287)来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4503287)

PIL（Python Imaging Library）是一个流行的第三方图像处理库，它提供了比 OpenCV 更简单直接的方式来给图像添加水印。然而，它仍然需要一些步骤来实现水印。PIL 是那些需要可靠且相对简单的方式来给图像添加水印的用户的不错选择，无需复杂的计算机视觉能力。

同样，在使用 PIL 库之前，我们需要按照如下方式安装它。

```py
pip install pillow
```

对于 PIL 库，我们需要以下 3 个模块。

+   `Image` 模块：提供了一个用于表示和操作 PIL 中图像的类。

+   `ImageDraw` 模块：提供了一组用于在图像上绘制的函数，包括线条、矩形、圆形和文本。

+   `ImageFont` 模块：提供了一个用于加载和操作字体的类，包括设置字体大小、样式和颜色。

```py
from PIL import Image, ImageDraw, ImageFont
```

然后，我们可以使用`Image`模块打开图像，如下所示。我们也可以重用之前定义的`show_image()`方法来显示原始图像。

```py
img = Image.open('my_photo.jpeg')
show_image(img)
```

![](img/00ba9ffaf90bc88635687dc9dc80c157.png)

要操作图像，我们需要从图像对象创建一个`ImageDraw`实例。

```py
# Create an ImageDraw object
draw = ImageDraw.Draw(img)
```

下一步有点棘手。与 OpenCV 内置的字体样式不同，PIL 只能使用单独的“.ttf”文件。虽然所有操作系统都有一些字体样式，但我们仍需要了解现有的字体样式，以便可以使用它们。

在这种情况下，我建议最简单的方法是使用`matplotlib`来显示可用的字体，如下所示，除非你有特定的字体样式需要使用。

```py
import matplotlib

matplotlib.font_manager.findSystemFonts(fontpaths=None, fontext='ttf')
```

以下是我所使用的一些可用字体。

![](img/58cd1e4b3cf11205f4fa63f8dd1bf6fe.png)

现在，我们可以开始设置参数。

```py
# Prepare watermark text
font = ImageFont.truetype('Humor-Sans.ttf', size=150)

# Calculate the size of the watermark text
t_width, t_height = draw.textsize(watermark_text, font)

# Calculate the x and y coordinates for the text
x = int((img.size[0] - t_width) / 2)
y = int((img.size[1] - t_height) / 2)
```

我们可以使用`ImageFont.truetype`创建一个特定大小的水印字体。之后，我们可以通过`draw`对象使用`textsize()`方法获取文本大小。之后，计算坐标的方式与我们在 OpenCV 演示中做的一样。

最后，我们可以使用`draw`对象的`text()`方法添加水印。

```py
# Add the text as a watermark on the image
draw.text((x, y), watermark_text, font=font, fill=(255, 255, 255))
```

![](img/288dcc6377fd3158577a17e65243552e.png)

# 3. Filestools — 一行代码奇迹

![](img/bbd8b08d6d12800a49d81978ec7a77b8.png)

图片来源：[Pexels](https://pixabay.com/users/pexels-2286921/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1868956)来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1868956)

`filestools` 是一个第三方 Python 库，提供了一系列有用的文件和图像处理工具。它包括显示目录结构的功能，如 Linux 中的 `tree` 命令，比较文件差异的功能，如 `diff` 命令，以及使用 `marker` 命令给图像添加水印。此外，filestools 还可以用于将 curl 请求转换为 Python 请求代码。尽管该库由一位中国开发者创建，但它仍然被广泛访问和使用，尽管一些日志是中文的。

同样，要使用这个库，我们可以按如下方式安装它。

```py
pip install filestools
```

然后，我们将水印文字添加到图像中。我们可以按如下方式从库的 `watermarker` 模块中导入 `add_mark()` 函数。然后，这个函数将完成我们需要的一切。

```py
from watermarker.marker import add_mark

add_mark(file="my_photo.jpeg", 
         out="watermarked",
         mark=watermark_text, 
         size=60,
         color="#ffffff",
         opacity=0.5, 
         angle=30, 
         space=60)
```

`out` 参数是一个目录名称，因此带水印的图像将被输出到这个目录中。`opacity` 指定了水印的透明度。我们确实可以使用 OpenCV 和 PIL 实现这一点，但需要更多的步骤和复杂的逻辑。除此之外，水印还会被渲染为图像上的“图案”。因此，我们可以给文字指定一个 `angle`，以及定义文本实例之间间距的 `space`。

![](img/0755ad546074e415c748331411c76166.png)

运行这个函数后，会显示“成功保存”。现在，我们可以检查我们的工作目录。我们应该能够找到一个包含带水印图像的新子目录。

![](img/52a547e877c9d05b26169654efa1be0d.png)

这是我们打开后的带水印照片。

![](img/b10d7dc986f9916b2c9f7cf7389390f7.png)

作者拍摄的照片

# 总结

![](img/441a2dfe734d4d873d628bf3d2168160.png)

图片由 [Nikolett Emmert](https://pixabay.com/users/niki_emmert-13526667/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7666292) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7666292)

在这篇文章中，我们比较了三种流行的 Python 库用于给图像加水印：OpenCV、PIL（Python Imaging Library）和 filestools。OpenCV 是一个综合的计算机视觉库，提供广泛的图像处理功能，而 PIL 提供了一种更简单直接的方法来给图像加水印。然而，这两个库都需要多个步骤和一些手动处理才能实现水印。另一方面，filestools 提供了一行代码的解决方案来添加水印，使其成为三者中最简单和最流线型的库。总体而言，虽然 OpenCV 和 PIL 提供了更高级的图像处理功能，但在水印添加的易用性方面，filestools 是明显的赢家。

[](https://medium.com/@qiuyujx/membership?source=post_page-----5f4f1e75a9f3--------------------------------) [## 使用我的推荐链接加入 Medium - Christopher Tao

### 感谢你阅读我的文章！如果你不介意，请请我喝杯咖啡 :) 你的会员费用支持成千上万的…

[medium.com](https://medium.com/@qiuyujx/membership?source=post_page-----5f4f1e75a9f3--------------------------------)

**如果你觉得我的文章有帮助，请考虑加入 Medium 会员来支持我和其他成千上万的作者！(点击上面的链接)**

> *除非另有说明，所有图片均由作者提供*
