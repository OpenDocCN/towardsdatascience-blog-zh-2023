# 破解 Matplotlib 的神秘面纱

> 原文：[`towardsdatascience.com/demystifying-matplotlib-3895ab229a63`](https://towardsdatascience.com/demystifying-matplotlib-3895ab229a63)

## 快速成功数据科学

## 你困惑是有原因的

[](https://medium.com/@lee_vaughan?source=post_page-----3895ab229a63--------------------------------)![李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----3895ab229a63--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3895ab229a63--------------------------------)![数据科学导向](https://towardsdatascience.com/?source=post_page-----3895ab229a63--------------------------------) [李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----3895ab229a63--------------------------------)

·发布于 [数据科学导向](https://towardsdatascience.com/?source=post_page-----3895ab229a63--------------------------------) ·16 分钟阅读·2023 年 11 月 2 日

--

![](img/d556e988458efce5ec0801d4c6a1fca6.png)

图片由 Cederic Vandenberghe 在 Unsplash 上提供

*你在使用 Matplotlib 时是否感到困难？* 如果你是初学者，可能是因为你没有花时间去了解它的一些特性。如果你怀疑是这样，那么请给自己一个机会，继续阅读！这不会造成伤害，也不会花费太多时间。

# Matplotlib

开源的 Matplotlib 库在 Python 中主导绘图。它允许你生成快速简单的图形以及复杂的图表，你可以控制显示的每一个方面。它的流行和成熟意味着你总能找到有用的建议和有用的代码示例。

像任何强大的软件一样，Matplotlib 也可以是“一些语法上的繁琐”。最简单的图形很容易，但难度很快上升。即使像[Matplotlib 画廊](https://matplotlib.org/stable/gallery/index.html)这样的资源提供了有用的代码示例，如果你想要稍微不同的内容，你可能会发现自己在挠头。

实际上，许多人通过复制粘贴别人的代码，然后在边缘进行调整，直到得到他们喜欢的结果来使用 Matplotlib。正如一位用户曾告诉我，“无论我使用 Matplotlib 多少次，它总是感觉像是第一次！”

幸运的是，你可以通过花时间学习这个包的一些关键方面来大大减轻这种痛苦。因此，在这篇文章中，我们将重点关注可能导致困惑的术语和绘图接口。掌握这些知识后，你可能会发现 Matplotlib 是一个值得拥抱的工具，而不是一个需要回避或勉强使用的工具。

# 问题是什么？

根据我学习 Matplotlib 的经验，这里有三个导致困惑的问题：

1.  绘图时使用的有些尴尬的术语。

1.  我将称之为*pyplot 方法*和*面向对象风格*的*两个*绘图接口的共存。

1.  两个接口中的绘图操作方法具有*相似*但*不同*的名称。

让我们逐一查看这些内容。

# 绘图的解剖结构

理解 Matplotlib 的第一步是掌握绘图术语。为此，让我们剖析一个图及其组件。

Matplotlib 中的绘图都保存在 `Figure` 对象中。这是一个空白画布，代表所有绘图元素的*顶层容器*。除了提供绘图的画布外，`Figure` 对象还控制诸如绘图的大小、宽高比、在同一画布上绘制的多个图之间的间距以及将绘图输出为图像的能力。以下图示中最左侧的方块代表一个 `Figure` 对象。

![](img/5dae4177989cfdba352257f3c56a46c2.png)

绘图的解剖结构（来源：Python 科学工具 [1]）

绘图本身——即你我认为的*图形*——由 `Axes` 类表示，如之前图示的中间所示。这个类包括大多数图形*元素*，如线条、多边形、标记（点）、文本、标题等，以及对这些元素进行操作的方法。它还设置了坐标系统。一个 `Figure` 对象可以包含*多个* `Axes` 对象，但每个 `Axes` 对象只能属于*一个* `Figure`。

`Axes` 对象不应与表示图表 x 轴或 y 轴上数值的 `Axis` 元素混淆（在之前图示的最右侧显示）。这包括刻度标记、标签和限制。这些元素都包含在 `Axes` 类中。

之前图示中的每个组件都存在于下面显示的层次结构中。最底层包括各个轴、轴的刻度标记和标签以及曲线（`Line2D`）。最高层是 `Figure` 对象，它作为下面所有内容的容器。

![](img/1056a6c5f2d8deefde74916e7c83fd60.png)

之前图示中的绘图组件层次结构（来源：Python 科学工具 [1]）

由于 `Figure` 对象可以包含*多个* `Axes` 对象，因此你可以在之前的图示中有多个 `Axes` 对象指向同一个 `Figure` 对象。一个常见的例子是*子图*，其中一个 `Figure` 画布上包含两个或多个不同的图：

![](img/52ac3c669cd55402d9336d03f0d3e8ad.png)

图示中包含两个子图的示例（由红框标出）（作者提供）

# pyplot 和面向对象方法

使用 Matplotlib 主要有两种绘图接口。第一种，称为*pyplot 方法*，你依赖于 Matplotlib 的内部 `pyplot` 模块来*自动*创建和管理 `Figure` 和 `Axes` 对象，然后使用 `pyplot` 方法进行绘图。该方法主要用于处理单个图形，它减少了你需要了解和编写的代码量。它是一个类似 MATLAB 的 API，适用于快速、交互式的工作。

这里是一个例子：

```py
import matplotlib.pyplot as plt
import numpy as np

data = np.arange(5, 10)
plt.plot(data)
```

![](img/b022f8f2e3ff11016a3d8e9ee4e43ace.png)

pyplot 方法的输出（作者提供）

整个图形只需要一行代码。`pyplot` 模块为你做出了所有决策，包括使用线条、线条的颜色和粗细、每个轴上的值范围，以及文本的字体和颜色。它还为每个 y 值提供了相应的 x 值，假设计数从 0 开始并以 1 单位递增。

使用第二种方法，称为*面向对象风格*，你*明确地*创建 `Figure` 和 `Axes` 对象，然后对生成的对象调用方法。这种方法让你对自定义图形和跟踪大型程序中的多个图形拥有最大的控制权。如果你首先创建一个 `Axes` 对象，还更容易理解与其他库的交互。

```py
import matplotlib.pyplot as plt
import numpy as np

data = np.arange(5, 10)
fig, ax = plt.subplots()
ax.plot(data)
```

![](img/b022f8f2e3ff11016a3d8e9ee4e43ace.png)

面向对象风格的输出（作者提供）

结果与使用 `pyplot` 方法获得的结果相同。

一旦你看到以下这一行，你就知道你在处理面向对象风格：

`fig, ax = plt.subplots()`

`plt.subplots()` 方法创建一个 `Figure` 实例和一组子图（一个包含 `Axes` 对象的 NumPy 数组）。如果未指定子图的数量，则默认返回一个*单一*的子图。

由于返回了*两个*对象，你需要将结果*解包*到*两个*变量中，按惯例称为 `fig` 和 `ax`。请记住，使用 `pyplot` 方法时，这两个实体是在幕后创建的。

在接下来的章节中，我们将查看这两种方法。然而，根据 [Matplotlib 文档](https://matplotlib.org/stable/index.html)，为了保持一致性，你应该*选择一种方法并坚持使用*。他们建议使用面向对象的风格，特别是对于复杂的图形以及那些打算作为更大项目的一部分重复使用的方法和脚本。

确实可以说，初学者发现 Matplotlib 令人望而生畏的原因之一是，他们在现有代码中看到了一种*混合*的这些方法，例如在问答网站如*Stack Overflow*上。由于这种情况是不可避免的，我建议你阅读这*两种*方法的描述，以便做出明智的选择。这样，当你在遗留代码或教程中遇到另一种方法时，你会对其有所了解。

# 使用 pyplot 方法

在上一节中，我们使用一行代码用`pyplot`创建了一个图表：

`plt.plot(data)`

这里有两点需要注意：我们没有*明确*在代码中引用`Figure`或`Axes`对象，因为`pyplot`在幕后处理了这些问题。我们也没有指定要在图中显示哪些*元素*，包括显示在 x 轴和 y 轴上的刻度和数值。相反，Matplotlib 查看了你的数据，并对你想要的图类型及其注释做出了智能选择。

> 在这方面，`plot()`方法绘制折线图，`scatter()`绘制散点图，`bar()`绘制条形图，`hist()`绘制直方图，`pie()`绘制饼图等等。你可以在 Matplotlib 的[图表类型索引](https://matplotlib.org/stable/plot_types/index)中找到所有这些示例。

`pyplot`的图表创建方法的自动特性在你想快速探索数据集时很有用，但生成的图表通常过于简单，不适合演示或报告。一大问题是，像`plt.plot()`这样的默认配置假设你希望每个轴的*大小*与输入数据的*范围*匹配（例如，如果数据限制在 5 到 8 之间，则 x 轴范围为 5 到 8，而不是 0 到 10）。

它还假设你不需要图例、标题或轴标签，并且希望线条和标记绘制成蓝色。这并非总是如此，因此`pyplot`提供了许多方法来装饰图表，包括标题、轴标签、背景网格等等。我们接下来会看看这些。

## 使用 pyplot 方法创建和操作图表

尽管被认为是一种比面向对象风格更简单的方法，`pyplot`仍然可以生成一些非常复杂的图表。为了演示，让我们使用一些`pyplot`方法创建比以前更复杂的图表。

*悬链线*是链条悬挂在两端时所呈现的形状。这在自然界和建筑中是常见的形状，例如在风压下的方形帆和位于密苏里州圣路易斯的著名拱门。你可以使用以下代码生成悬链线，其中`cosh(x)`表示 x 值的双曲余弦。

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(-5, 5, 0.1)
y = np.cosh(x)

plt.title('A Catenary')
plt.xlabel('Horizontal Distance')
plt.ylabel('Height')
plt.xlim(-8, 8)
plt.ylim(0, 60)
plt.grid()
plt.plot(x, y, lw=3, color='firebrick')
```

![](img/d2c67f1e0146a571bc089c0fad9c1521.png)

pyplot 悬链线程序的输出（作者提供）

尽管有些冗长，但代码逻辑清晰且可读。所有绘图步骤都调用了`plt`上的方法。

在 Matplotlib 中，渲染在`Figure`画布上的元素，如标题、图例或线条，被称为`Artist` *对象*。标准图形对象，如矩形、圆形和文本，被称为*基本* `Artists`。持有基本对象的对象，如`Figure`、`Axes`和`Axis`对象，称为*容器* `Artists`。

以下表格列出了制作图表和操作`Artists`时一些较常见的`pyplot`方法。要查看完整列表，请访问 Matplotlib `pyplot` [汇总页面](https://matplotlib.org/stable/api/pyplot_summary.html)[.](https://matplotlib.org/stable/api/pyplot_summary.html.) 点击在线列表中的方法名称，将带您到有关方法参数的详细信息以及示例应用程序。如果您想了解更多关于`Artists`的信息，请访问 Matplotlib [艺术家页面](https://matplotlib.org/stable/tutorials/intermediate/artists.html)。

![](img/c15ed472ed49f7a82923e30a3e70fd93.png)

一些用于创建图表的有用 pyplot 方法（来源：Python 科学工具[1]）

![](img/70f1be4931e91f12b0ba2ca9a5c8a18f.png)

一些用于操作图表的有用 pyplot 方法（来源：Python 科学工具[1]）

请注意，表格中的代码示例代表了*简单*的情况。大多数方法需要许多参数，让您可以根据字体样式和大小、线条宽度和颜色、旋转角度、爆炸视图等属性来微调图表。

## 使用子图

到目前为止，我们一直在处理单一图形，但有时您可能希望并排比较两个图表或将几个图表汇总到一个摘要显示中。在这些情况下，Matplotlib 提供了 `subplot()` 方法。要了解其工作原理，让我们先生成两个不同正弦波的数据：

```py
time = np.arange(-12.5, 12.5, 0.1)
amplitude = np.sin(time)
amplitude_halved = np.sin(time) / 2
```

比较这些波形的一种方法是将它们绘制在同一个`Axes`对象中，如下所示：

```py
plt.plot(time, amplitude, c='k', label='sine1')
plt.plot(time, amplitude_halved, c='firebrick', ls='--', label='sine2')
plt.legend()
```

![](img/b97e337ca338ab4fd1d18c21489bd5f3.png)

pyplot 正弦程序的输出（作者提供）

默认情况下，两条曲线将用不同的颜色（蓝色和橙色）绘制。我们用黑色（使用缩写‘k’）和“火砖”红色覆盖了这一点。我们还使用`ls`参数强制使用不同的线型。否则，两条线将是实线。（有关可用于标记和线型的字符列表，请访问[这个网站](https://matplotlib.org/stable/api/_as_gen/ matplotlib.pyplot.plot.html)）。

如果您比较的曲线数量超过几个，单个图表可能会变得混乱且难以阅读。在这种情况下，您会想要使用`subplot()`方法创建的独立堆叠图。下图描述了此方法的语法，其中四个子图（`Axes`）被放置在一个`Figure`容器中。

![](img/fe0f66b24af1ff5fedb0533b152677cd.png)

理解`subplot()`方法（来源：Python 科学工具[1]）

子图将以网格形式排列，传递给`subplot()`方法的前两个参数指定了该网格的维度。第一个参数表示网格的行数，第二个参数表示列数，第三个参数是活动子图的索引（在图中以灰色突出显示）。

活动子图是你当前正在绘制的子图，当你调用`plot()`或`scatter()`等方法时。与 Python 中的大多数事物不同，第一个索引是 1，而不是 0。

Matplotlib 使用称为“当前图形”的概念来跟踪当前正在处理哪个`Axes`。例如，当你调用`plt.plot()`时，`pyplot`会创建一个新的“当前图形”`Axes`进行绘制。在处理多个子图时，索引参数告诉`pyplot`哪个子图代表“当前图形”。

> 为了方便，你不需要在`subplot()`参数中使用逗号。例如，`plt.subplot(223)`与`plt.subplot(2, 2, 3)`效果相同，尽管前者可能阅读性较差。

现在，让我们将正弦波绘制为两个独立的堆叠图。过程是调用`subplot()`方法，并更改其活动子图参数以改变当前子图。对于每个当前子图，`plot()`方法将发布特定于该子图的数据，如下所示：

```py
plt.subplot(2, 1, 1)
plt.plot(time, amplitude, label='sine1')
plt.legend(loc='upper right')

plt.subplot(2, 1, 2)
plt.ylim(-1, 1)
plt.plot(time, amplitude_halved, label='sine2')
plt.legend(loc='best')
```

![](img/597adaeb9a40a61c77175c7d003f2c8d.png)

`pyplot`正弦子图程序的输出（由作者提供）

注意，如果你没有设置第二个图的 y 轴限制，`pyplot`会自动调整图表，使两个子图看起来相同。由于我们使用`ylim()`方法手动设置了第二个子图的比例，因此很明显第二个正弦波的幅度是第一个的一半。

这是一瞥`pyplot`方法的一些语法。现在让我们来看一下面向对象的风格。

# 使用面向对象风格

面向对象的绘图风格通常需要比之前描述的`pyplot`方法更多的代码，但它让你能够最大限度地利用 Matplotlib。通过显式创建`Figure`和`Axes`对象，你可以更轻松地控制图表，更好地理解与其他库的交互，创建具有多个 x 和 y 轴的图表等等。

## 使用面向对象风格创建和操作图表

为了熟悉面向对象的风格，让我们重新创建文章前面提到的链线图。为了演示该风格的一些增强功能，我们将强制 y 轴通过图表的中心。

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(-5, 5, 0.1)
y = np.cosh(x)

fig, ax = plt.subplots()
```

上述代码将创建一个空的图形。要自定义配置图表，接下来调用`Axes`对象的`set()`方法，并传递标题、轴标签和轴限制的关键字参数。`set()`方法是一个便利方法，它允许你一次设置*多个*属性，而不是为每个属性调用特定的方法。

```py
ax.set(title='A Catenary',
        xlabel='Horizontal Distance',
        ylabel='Height',
        xlim=(-8, 8.1),
        ylim=(0, 60))
```

接下来，我们将把 y 轴移到图表的中心，而不是沿着一侧。在 Matplotlib 中，*spines* 是连接轴刻度线并标记包含绘制数据的区域边界的线。

默认情况下，坐标轴的脊柱 *围绕* 绘图，刻度和标签沿左侧和底部边缘。然而，脊柱也可以放置在任意位置。通过面向对象风格，我们可以使用 `Spine` 子类的 `set_position()` 方法来实现这一点。

以下代码首先将左（y）轴移动到 x 轴上的 `0` 值。然后，将线宽设置为 `2`，使得该轴在我们稍后将使用的背景网格中更加突出。

```py
ax.spines.left.set_position('zero')
ax.spines.left.set_linewidth(2)
```

以下代码通过将右边界的颜色设置为无来关闭绘图的右边界：

```py
ax.spines.right.set_color('none')
```

接下来的三行分别对底部轴和顶部轴重复了这个整体过程：

```py
ax.spines.bottom.set_position('zero')
ax.spines.bottom.set_linewidth(2)
ax.spines.top.set_color('none')
```

为了完成图形，我们添加一个背景网格并调用 `plot()` 方法，传入 x 和 y 数据，将线宽设置为 `3`，颜色设置为 `firebrick`：

```py
ax.grid()
ax.plot(x, y, lw=3, color='firebrick')
```

![](img/840fd3319e5e127eeb786a8cef5435db.png)

使用面向对象风格构建的链形线图（作者提供）

如果你省略有关脊柱的代码，你可以用基本相同的代码重现 `pyplot` 版本的图形。因此，面向对象风格的冗长程度很大程度上与它的功能有关，人们通常会利用这一点。

`pyplot` 方法在面向对象风格中有对应的等价方法。不幸的是，方法名称通常不同。例如，`pyplot` 中的 `title()` 变成了 `set_title()`，`xticks()` 变成了 `set_xticks()`。这就是为什么选择一种方法并坚持使用它的原因之一。

以下表格列出了面向对象绘图的更常见方法。你可以在 [图形类型索引](https://matplotlib.org/stable/plot_types/index.html) 和 [Matplotlib 画廊](https://matplotlib.org/stable/gallery/index.html) 中找到更多方法，比如制作箱形图、小提琴图等。

![](img/26e2718d976ccab1c725594c0ca8fa5b.png)

一些用于创建图形的有用面向对象方法（来源：Python Tools for Scientists [1]）

用于处理 `Figure` 和 `Axes` 对象的常用方法列在以下表格中。在许多情况下，这些方法的工作方式与 `pyplot` 方法类似，但方法名称可能不同。

![](img/7f0e52e6442fa0ad16ec5362dc829aac.png)

一些用于操控图形的有用面向对象方法（来源：Python Tools for Scientists [1]）

![](img/66374df39a80062f0fd1945c82c58d74.png)

一些用于处理 Axes 对象的有用方法（来源：Python Tools for Scientists [1]）

如 `pyplot` 部分所述，这些表格中的代码示例表示 *简单* 情况。大多数方法需要多个参数，允许你根据字体样式和大小、线宽和颜色、旋转角度、爆炸视图等属性微调图形。要了解更多信息，请访问 Matplotlib [文档](https://matplotlib.org/)[.](https://matplotlib.org/.)

## 处理子图

与 pyplot 方法类似，对象导向风格支持使用子图。尽管有多种方式可以将子图分配给`Figure`和`Axes`对象，`plt.subplots()`方法既方便又返回一个 NumPy 数组，使你可以使用标准索引或唯一名称如`axs[0, 0]`或`ax1`来选择子图。另一个好处是你可以在绘制任何数据之前预览子图的几何布局。

> 创建子图的面向对象方法拼写为*subplots*，而 pyplot 方法使用*subplot*。你可以通过将最简单的技术（pyplot）与最短的名称联系起来记住这一点。

调用`plt.subplots()`而不带参数会生成一个空的单图。技术上，这会生成一个`1×1 AxesSubplot`对象。

```py
fig, ax = plt.subplots()
```

![](img/bb7970a9306f72d943cd6fc463c0ffbf.png)

一个空的子图（作者提供）

生成多个子图的工作方式类似于`plt.subplot()`方法，只是不需要为活动子图提供索引参数。第一个参数表示行数；第二个参数指定列数。按照惯例，多个`Axes`被赋予*复数*名称`axs`，而不是`axes`，以避免与单个`Axes`实例混淆。

传递两个参数给`plt.subplots()`方法可以控制子图的数量和几何布局。以下代码生成了下面显示的 2×2 子图网格，并将两个`AxesSubplot`对象的列表存储在`axs`变量中。

```py
fig, axs = plt.subplots(2, 2)
axs
```

![](img/9808fd51aafa446cc8c3f2391ca38bcb.png)

一个 2x2 的子图网格（作者提供）

要激活一个子图，你可以使用它的索引。在这个例子中，我们在第一行的第二个子图上绘图：

```py
fig, axs = plt.subplots(2, 2)
axs[0, 1].plot([1, 2, 3])
```

![](img/1940e92300950f087d59de97764be13a.png)

一个 2x2 的子图网格，第二个子图处于活动状态（作者提供）

另外，你可以通过使用元组解包为多个`Axes`来单独命名和存储子图。每一行的子图都需要放在自己的元组中。然后，你可以通过名称选择子图，而不是使用不太易读的索引：

```py
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2)
ax3.plot([1, 2, 3])
```

![](img/2d6bf2996a09d61330ad65aa9cd98232.png)

一个 2x2 的子图网格，第三个子图处于活动状态（作者提供）

在`pyplot`方法和面向对象风格中，你可以通过在`Figure`对象上调用`tight_layout()`方法来增加子图周围的空白。

```py
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2)
ax3.plot([1, 2, 3])
fig.tight_layout()
```

![](img/7288071bb53b562ca91552d92388ecad.png)

`tight_layout()`方法对子图间距的影响（作者提供）

现在子图看起来不那么拥挤了。对于`pyplot`方法，你会使用`plt.tight_layout()`。

# 创建子图的替代方法

无论你使用哪种技术，都有更高级的替代方法来帮助你将图形拆分为网格状的子区域。这反过来有助于你创建具有不同宽度和高度的子图。生成的多面板显示对总结演示文稿和报告中的信息非常有用。

这些面板工具包括 Matplotlib 的 `[GridSpec](https://matplotlib.org/stable/api/_as_gen/matplotlib.gridspec.GridSpec.html)` 模块及其 `subplot_mosaic()` 方法。以下是一个使用 `GridSpec` 构建的示例：

![](img/36c464e0f04344764af7323270e81045.png)

使用 GridSpec 构建的多面板显示（来源：Python 工具用于科学家 [1]）

要了解更多关于这些工具的信息，请访问 [*处理多个图形和坐标轴*](https://matplotlib.org/stable/users/explain/quick_start.html) 和 [*在一个图形中排列多个坐标轴*](https://matplotlib.org/stable/users/explain/axes/arranging_axes.html) 的 Matplotlib 文档，以及我在 *Better Programming* 上的 `GridSpec` 教程 [文章](https://betterprogramming.pub/build-an-interactive-lithium-factbook-with-gridspec-and-geopandas-e699054237a8)。

# 总结

如果您使用 Python 编程，您需要了解 Matplotlib。要了解 Matplotlib，您需要理解其主要的绘图术语和两种绘图接口。

`Figure` 对象代表您绘制的 *画布*。它控制诸如图表大小、纵横比、子图之间的间距、超级标题以及保存图表的能力等事项。

`Figure` 对象可以包含多个 `Axes` 对象，这些对象形成我们通常认为的图形或图表。这些包括线条、点、文本、标题、图表的坐标系统等。一个 `Figure` 对象中的多个 `Axes` 对象构成 *子图*。

在 `Axes` 对象中，`Axis` 元素表示 x、y 或 z 轴上的数值，包括刻度线、标签和限制。

Matplotlib 提供了两种主要的绘图方法。`pyplot` 方法旨在快速和简单地绘图，例如用于探索性数据分析。使用此方法，`Figure` 和 `Axes` 对象在幕后创建，大多数决策，如坐标轴缩放、颜色、线条样式等，都由系统为您做出（尽管您可以在一定程度上覆盖这些设置）。

对于更复杂的图形，例如报告和演示，*面向对象风格* 明确地创建 `Figure` 和 `Axes` 对象（按惯例标记为 `fig` 和 `ax`）。这为您提供了更多控制权，并使您更容易理解与其他 Python 库的交互。

如果您不了解这两种绘图范式，在使用在线找到的代码片段时很容易感到困惑，例如在 *Stack Overflow* 上。由于每种方法的使用方法相似但不同，Matplotlib 开发者建议您选择一种方法并始终如一地使用。

# 引用

1.  Lee Vaughan 的《Python 工具用于科学家：使用 Anaconda、JupyterLab 和 Python 的科学库简介》（[*No Starch Press*](https://nostarch.com/real-world-python)，2023）。

# 谢谢！

感谢阅读，未来请关注我获取更多*快速成功数据科学*文章。如果你想深入了解 Matplotlib 和 Python 的其他绘图库，可以查看我的书籍，[*科学家的 Python 工具*](https://a.co/d/67VvS3G)，该书在线和在像 Barnes and Noble 这样的书店均有售。

[](https://a.co/d/67VvS3G?source=post_page-----3895ab229a63--------------------------------) [## 科学家的 Python 工具：Anaconda、JupyterLab 和 Python 科学库入门…

### 《科学家的 Python 工具：Anaconda、JupyterLab 和 Python 科学库入门》…

a.co](https://a.co/d/67VvS3G?source=post_page-----3895ab229a63--------------------------------)
