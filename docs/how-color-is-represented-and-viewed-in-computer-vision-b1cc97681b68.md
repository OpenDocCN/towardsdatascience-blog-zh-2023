# 关于计算机视觉中颜色表示的全面指南 (CV-02)

> 原文：[`towardsdatascience.com/how-color-is-represented-and-viewed-in-computer-vision-b1cc97681b68`](https://towardsdatascience.com/how-color-is-represented-and-viewed-in-computer-vision-b1cc97681b68)

## 颜色空间和颜色模型的详细解释

[](https://zubairhossain.medium.com/?source=post_page-----b1cc97681b68--------------------------------)![Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----b1cc97681b68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b1cc97681b68--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1cc97681b68--------------------------------) [Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----b1cc97681b68--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1cc97681b68--------------------------------) ·7 min read·2023 年 4 月 3 日

--

![](img/75cba015e48aa64937b4f3ffb861747e.png)

照片由 [Lucas Kapla](https://unsplash.com/@aznbokchoy?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 动机

> *我们如何感知一个物体？人类和计算机视觉系统的工作方式相同吗？有多少种颜色？我们是否知道或想知道这些问题的所有答案？如果“是”，那么这篇文章将是最适合你的。*

眼睛是造物主创造的美丽杰作，它能以美观和和谐的方式感知物体的颜色。颜色是一种基于电磁波谱的视觉感知 [1]。动物视觉系统探测物体波长的吸收情况，不同的吸收产生不同的颜色。颜色可能是无限的，但我们大致认为存在 1000 万种颜色 [2]。光是产生颜色的必需条件。在没有光的情况下，一切都显得黑暗。想象一下你被锁在一个黑暗的房间里。感觉如何？所以，没有光，世界是没有意义的。它也为我们的生活带来了魅力。

计算机不能像人类一样感知物体。计算机通过一些标准的颜色空间（颜色的表示方式）来生成和识别颜色。

`*在这篇文章中，我将讨论不同的颜色模型，以便更好地了解颜色在计算机视觉系统中的表示和感知。*`

## 目录

1.  `**人类与计算机视觉系统**`

1.  `**什么是颜色空间？**`

1.  `**不同的颜色模型**`

## 人类与计算机视觉系统

人类视觉系统是复杂的。首先，眼睛感知环境/物体，然后不同的器官和神经将信号传递给大脑。最后，大脑进行识别并做出相应的反应。

***[****注：我只是简单地表示了这个过程，因为它超出了本文的范围。实际上，过程要复杂得多。如果你仍然对了解更多关于人类视觉系统的信息感兴趣，请阅读* [***文章***](https://en.wikipedia.org/wiki/Visual_system)*。****]***

![](img/9eb8305395b78a1c85b0171214893ae0.png)

人类与计算机视觉系统（图像来源于作者）

计算机是一台在没有人类隐性或显性指令的情况下无法执行任何任务的机器。对于计算机视觉系统，摄像头拍摄图像以感知环境，摄像头输入进一步传递给计算机。最后，不同的计算机视觉模型识别图像并显示输出。

## 什么是颜色空间？

颜色是计算机视觉系统的一个重要组成部分。颜色空间是以有组织的方式组合的颜色，主要用于在不同介质中再现颜色。例如，你有一个笔记本电脑屏幕和一个 52 英寸的大型显示器。显然，总像素和颜色组合会有所不同。不同的颜色空间帮助我们使其与屏幕分辨率兼容，以提供更好的颜色组合体验。

![](img/473a3b445f717b75d7c0f6ad00410040.png)

[CIE 1931](https://en.wikipedia.org/wiki/CIE_1931_color_space) xy [色度图](https://en.wikipedia.org/wiki/Chromaticity_diagram)（根据 Wikimedia Commons 自由许可证 [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0)）

上面的色度图表示了颜色空间。它展示了不同 x 和 y 轴值及各种波长的波段。这个空间使所有可能的颜色组合对人眼可见。像 RGB、sRGB 等有很多颜色空间。每个颜色空间下的颜色组合（用三角形标记）包含了指定的颜色。*阅读* [***文章***](https://en.wikipedia.org/wiki/List_of_color_spaces_and_their_uses)*以获得详细解释。*

## **不同的颜色模型**

颜色模型是我们可以在数学上表示不同颜色的方式。例如，RGB 颜色模型结合了红色、绿色和蓝色。不同强度值的混合产生不同的颜色。

*我们经常将颜色模型与颜色空间搞混。* 让我们来澄清一下。颜色模型在数学上表示不同的颜色，而“颜色”一词出现在我们在不同介质中再现颜色时。例如，sRGB 颜色空间是红色、绿色和蓝色不同范围的颜色强度的混合。*阅读答案如果你还想了解更多区别。*

**常见颜色模型有** `*RGB, CMYK, HSL, 和 HSV*`。

+   **RGB 颜色模型**

RGB 是基本的颜色模型组合，包括不同强度的红色、绿色和蓝色。强度值范围从 0 到 255。这个模型使用三个颜色通道（红色、绿色和蓝色）来表示颜色。

![](img/e41ce6987466ee55973febc335fb4a75.png)

RGB 加法颜色（来自[Wikimedia Commons](https://commons.wikimedia.org/wiki/File:AdditiveColor.svg) 公共领域许可证）

可能的颜色组合有（255 x 255 x 255）16,581,375 种。它也被称为加法颜色模型，因为它通过结合 RGB 颜色来产生不同的颜色。大多数显示器使用 RGB 颜色模型。

*我创建了一个立方体 RGB 颜色模型的 gif，以便更好地理解。*

![](img/857cc97703d3e8ee3f6e0478a6f69fec.png)

如何通过 RGB 值改变颜色（gif 由作者创建）

立方体上的黑色圆圈表示不同 RGB 强度值的确切颜色。

`*[注：我使用了* [***网站***](https://programmingdesignsystems.com/color/color-models-and-color-spaces/index.html) *来模拟颜色模型。]*`

+   **CMYK 颜色模型**

CMYK 颜色模型是由四种颜色青色、品红色、黄色和黑色（Key）组成的。它是 RGB 颜色模型的修改版本，因为我们通过连续减去红色、绿色和蓝色来获得青色、品红色和黄色。

```py
Cayan = 255- Red
Magenta = 255- Green
Yellow = 255- Blue
```

![](img/7278fec0287c869f20a4729277074114.png)

CMYK 颜色模型（图片由作者提供）

我们无法通过青色、品红色和黄色的组合来产生纯黑色。因此，额外的黑色（Key）用于完成模型。

这种颜色模型最常用于打印。

+   **HSL 和 HSV 颜色模型**

这两种颜色模型是基于人类对 RGB 颜色的感知创建的。尽管这些模型与人类感知不完全相同，但它们是兼容的。在讨论颜色模型之前，我想解释一下主要的颜色属性`***色相、饱和度和亮度/明度***。`

![](img/518f2ea62043524ad16e3b3fc2e10544.png)

RGB/HSL/HSV 色相，饱和度 100%和亮度 50%（值 100%）（来自 wikimedia commons 的免费许可证[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0)）

***色相 —*** 上面的颜色轮表示从 0 到 360 度的多种颜色。轮子上的任何特定位置都被认为是*色相*。下图清楚地描绘了这一情况。

![](img/d5f3b6b3a05b4ff8c977416f6812ea7b.png)

色相范围 0–360°（来自 wikimedia commons 的免费许可证[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0)）

***饱和度 —*** 它是颜色（色相）的强度。下图表示了红色的饱和度范围。红色的灰度是低饱和度，而纯红色是高饱和度。

![](img/67a5fd98e785153d0d7844714dc8c869.png)

饱和度范围（左侧为 0%）（Wikimedia commons 免费许可证[CC0](http://creativecommons.org/publicdomain/zero/1.0/deed.en)）

***明度 / 亮度 —*** 它是测量单个颜色亮度的指标***。*** 其中，暗色是低亮度，白色是高亮度。蓝色的亮度如下所示。

![](img/57b532db71cdc40b4fe3a46c9e59c4e8.png)

蓝色的亮度/明度（图片由作者提供）

👉 **HSL** 颜色模型由色调、饱和度和亮度组成。请看下面的图像。

![](img/36bb152da6b153674aef094be3a5756d.png)

HSL 颜色模型（来自 Wikimedia Commons 免费许可 [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0)）

颜色模型与实心圆柱进行比较。色调表示从 0 到 360 度的不同角度的颜色。在圆柱的顶部，所有颜色的亮度都很高。这就是为什么它看起来是白色的原因。从上到下，亮度逐渐降低；在底部，没有光线。饱和度值从圆柱的中心到外侧逐渐从低到高。*HSL 颜色模型在不同色调、饱和度和亮度下的模拟如下所示。*

![](img/d8288e52b200257e1f26349fbbe167a5.png)

HSL 颜色模型（GIF 由作者创建）

圆柱上的圆圈表示不同色调、饱和度和亮度值在左侧刻度中显示的特定颜色。

👉**HSV** 颜色模型类似于 HSL 颜色模型。在这里，“亮度”一词被“值”取代。在 HSL 中，最大亮度时，颜色为纯白色。另一方面，在 HSV 中，最大值时，颜色不是纯白色，而是类似于在特定颜色上照射光线。

![](img/b958ce154dcaf6a52248657e618bf068.png)

HSV 颜色模型（来自 Wikimedia Commons 免费许可 [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0)）

让我们通过模拟更好地理解该模型。

![](img/533ddb1f2ea8b0f707169239aec220c8.png)

HSV 颜色模型（GIF 由作者创建）

*[我用来模拟模型的* [*网站*](https://programmingdesignsystems.com/color/color-models-and-color-spaces/index.html) *]*

## 结论

除了这些颜色模型，还有一些其他颜色模型。但这些模型在计算机视觉中广泛使用。详细来说，RGB 颜色模型通过混合红色、绿色和蓝色来表示颜色。CMYK 是青色、品红色、黄色和关键（黑色）颜色的颜色模型组合。而最后两个颜色模型，HSL 和 HSV，是用于更直观地表示 RGB 颜色的颜色模型，以反映人类对 RGB 颜色的感知。

## 参考文献

1.  [颜色 — 维基百科](https://en.wikipedia.org/wiki/Color)

1.  [有多少种颜色？你想知道的一切 (wikihow.com)](https://www.wikihow.com/How-Many-Colors-Are-There)

1.  [颜色模型和颜色空间 — 编程设计系统](https://programmingdesignsystems.com/color/color-models-and-color-spaces/index.html)

*我为你选择了一些文章列表。*

[](/getting-started-with-numpy-and-opencv-for-computer-vision-555f88536f68?source=post_page-----b1cc97681b68--------------------------------) ## 开始使用 NumPy 和 OpenCV 进行计算机视觉

### 用 Python 开始你的计算机视觉编程

towardsdatascience.com ![Md. Zubair](img/0c4ffded7421acbb503e55363a4c7f3c.png)

[Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----b1cc97681b68--------------------------------)

## 机器学习基础

[查看列表](https://zubairhossain.medium.com/list/machine-learning-basics-5d41676d50d5?source=post_page-----b1cc97681b68--------------------------------)6 个故事![](img/ce0270abd6cc464f4806fe32512cc91c.png)![](img/78f543be758b6b5f7fd931120cc32a91.png)![](img/38f61b1839629f825eb3588c9a159758.png)![Md. Zubair](img/0c4ffded7421acbb503e55363a4c7f3c.png)

[Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----b1cc97681b68--------------------------------)

## 数据科学统计

[查看列表](https://zubairhossain.medium.com/list/statistics-for-data-science-86b8f8cf53df?source=post_page-----b1cc97681b68--------------------------------)13 个故事![](img/8eccb217cc037d0c7a79e9bd13790461.png)![](img/7bc3f3699cfda939e493015c191e9aa9.png)![](img/c703de5211eb56454f9bdc26657e6afd.png)![Md. Zubair](img/0c4ffded7421acbb503e55363a4c7f3c.png)

[Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----b1cc97681b68--------------------------------)

## 数据可视化完整指南

[查看列表](https://zubairhossain.medium.com/list/complete-guide-to-data-visualization-e95ce842d507?source=post_page-----b1cc97681b68--------------------------------)4 个故事![](img/8f97d23598dc9f9ab7094e525a5a10ce.png)![](img/0e87d714b148a3cce0903437390639ff.png)![](img/f2357f5669559e2daa06363760d13eca.png)
