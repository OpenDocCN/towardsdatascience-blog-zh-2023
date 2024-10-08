# 如何使用 Matplotlib 样式化图表

> 原文：[`towardsdatascience.com/how-to-style-plots-with-matplotlib-45591394ff98`](https://towardsdatascience.com/how-to-style-plots-with-matplotlib-45591394ff98)

## 快速成功的数据科学

## 不要满足于默认设置！

[](https://medium.com/@lee_vaughan?source=post_page-----45591394ff98--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----45591394ff98--------------------------------)[](https://towardsdatascience.com/?source=post_page-----45591394ff98--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----45591394ff98--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----45591394ff98--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----45591394ff98--------------------------------) ·8 分钟阅读·2023 年 11 月 8 日

--

![](img/b56ed3118211d9fd51f6edb70d57dec3.png)

设置样式（来源：Cole Keister 在 Unsplash）

几十年前，我妈妈给我送了一件栗色天鹅绒运动套装作为圣诞礼物。那真是件糟糕的东西，我回答说这不太流行。她嗤之以鼻地说，“*你*设置了样式！做个潮流引领者！”

不用说，我确实没有设置样式，但我的妻子还是拿“你设置了样式！”这句话来逗我。不过，当使用 Matplotlib 时，我*确实*会设置样式，并且这与穿着天鹅绒运动套装不同，这是一件好事。

为了方便，Python 的 Matplotlib 库允许你覆盖其默认绘图选项。你可以利用这个强大的功能，不仅可以自定义图表，还可以为报告、出版物和演示文稿应用一致、自动且可重用的样式。

在这个*快速成功的数据科学*项目中，我们将快速了解如何使用 Matplotlib 样式化图表。

# Matplotlib 中的样式选项

如果你使用过 Matplotlib，你可能已经通过向绘图方法传递*新的*值来更改图表的默认设置，例如线条的颜色。但如果你想要为*多个*图表*同时*设置这些值，以便所有曲线颜色相同，或者*循环使用*预定义的颜色顺序，该怎么办？

好吧，你可以通过以下两种方式实现：

1.  运行时配置参数

1.  样式文件

1.  样式表

让我们依次查看这些内容。

# 更改运行时配置参数

一种样式化图形的方法是*运行时*设置参数，使用`RcParams`类的实例。这个类的名称代表*运行时配置参数*，你可以通过`pyplot`方法或面向对象的风格在笔记本、脚本或控制台中运行它。（如果你不熟悉这两种方法，请参阅我的文章，[*解密 Matplotlib*](https://medium.com/towards-data-science/demystifying-matplotlib-3895ab229a63)）。

绘图参数存储在`matplotlib.rcParams`变量中，这是一个类似字典的对象。可配置参数的列表非常长，你可以在 Matplotlib [文档](https://matplotlib.org/stable/api/matplotlib_configuration_api.html#matplotlib.rcParams)中查看。

现在，让我们看一个`pyplot`的例子，其中我们标准化图形的大小，使用*红色*绘制所有线条，并循环使用两种不同的线条样式。这意味着第一条绘制的线条将始终具有一定的一致样式，而第二条绘制的线条将具有另一种一致样式。

```py
import numpy as np
import matplotlib.pyplot as plt
import matplotlib as mpl
from cycler import cycler
```

请注意，我们还将 Matplotlib 导入为`mpl`。以这种方式导入 Matplotlib 使我们可以访问比`pyplot`模块更多的功能。我们还导入了`cycler`。`Cycler`类允许我们指定在制作多数据图时要循环的颜色和其他样式属性。你可以在[这里](https://matplotlib.org/stable/users/explain/artists/color_cycle.html)了解更多信息。

要访问`rcParams`中的属性，你可以将其视为字典键。你可以通过输入`mpl.rcParams.keys()`找到有效的参数名称。

在接下来的三行中，我们设置了图形大小、线条颜色和线条样式：

```py
mpl.rcParams['figure.figsize'] = (5, 4)
mpl.rcParams['lines.color'] = 'firebrick'
mpl.rcParams['axes.prop_cycle'] = cycler('linestyle', ['-', ':'])
```

> 注意：你也可以通过`pyplot`设置参数，使用类似`plt.rcParams[‘lines.color’] = ‘black’`的语法。

要循环样式，我们使用了`axes.prop_cycle`键，然后将 cycler 工厂函数传递参数（`‘linestyle’`）和样式列表（实线和虚线）。这些默认设置现在已经被重置为当前会话中你将要制作的所有图形。

为了测试，让我们生成一些数据并绘制它：

```py
# Prepare the data:
x = np.arange(0, 15, 0.1)
y = np.sin(x)

# Plot the data:
plt.plot(x, y)
plt.plot(x + 1, y - 2);
```

![](img/c47e2ef72dd4f2bebea31ae41b9f0bb0.png)

样式化的图形（作者）

通常，这段代码会生成一个有两条实线的图，一个蓝色，一个橙色。然而，我们覆盖了默认设置，因此我们得到了两条红色的线，通过不同的线条样式加以区分。

请注意，如果你在前一个图中绘制*三条*线条，*第三*条线将循环回使用*实线*样式，你将有一条虚线和两条实线。如果你想要三种不同的样式，你需要将额外的样式添加到 cycler 中。

为了方便，Matplotlib 提供了通过*关键字参数*同时修改一组多个设置的函数。这里是一个例子，我们首先恢复默认设置，然后更改线宽：

```py
plt.rcdefaults()  # Restore plot defaults.

mpl.rc('lines', lw=5)
plt.plot(x, y)
plt.plot(x + 1, y - 2);
```

![](img/12bbadf7e7230f444f544feb20c758c9.png)

重新样式化的图形（由作者提供）

在这个图中，恢复了默认的蓝橙色调，但线条宽度被自定义为`5`。

注意，你也可以使用以下命令重置默认值：

```py
mpl.rcParams.update(mpl.rcParamsDefault)
```

# 创建和使用样式文件

你可以将对 Matplotlib 默认设置的更改保存到*文件*中。这可以让你为报告或演示文稿标准化图形，并在项目团队内共享自定义设置。它还通过让你预设某些图形参数并将其封装在外部文件中，从而减少了代码的冗余和复杂性。

让我们创建一个简单的样式文件，为图形设置一些标准，如图形大小和分辨率、背景网格的使用，以及标题、标签和刻度的字体和大小。在文本编辑器中，输入以下内容：

```py
# scientific_styles.mplstyle

figure.figsize:    4, 3  # inches
figure.dpi:        200
axes.grid:         True
font.family:       Times New Roman
axes.titlesize:    24
axes.labelsize:    16
xtick.labelsize:   12
ytick.labelsize:   12
```

为了让 Matplotlib 能够轻松找到该文件，你需要将其保存在特定位置。首先，在控制台中输入以下内容以找到*matplotlibrc*文件的位置（Matplotlib 存储其默认设置的位置）：

```py
import matplotlib as mpl
print(mpl.matplotlib_fname())
```

这是我电脑上的输出（你的会有所不同）：

```py
C:\Users\hanna\anaconda3\lib\site-packages\matplotlib\mpl-data\matplotlibrc
```

这显示了到*mpl-data*文件夹的路径，该文件夹包含*matplotlibrc*文件以及一个名为*stylelib*的文件夹等。将样式文件保存到*stylelib*文件夹中，命名为*scientific_styles.mplstyle*（替换*.txt*扩展名）。

> 注意：如果 Matplotlib 在后续查找该文件时出现问题，可能需要重新启动内核。

现在，让我们使用这个文件来创建标准化的图形。导入`pyplot`后，使用其`style.use()`方法加载样式文件而不带文件扩展名：

```py
import matplotlib.pyplot as plt
plt.style.use('scientific_styles')
```

接下来，使用面向对象的风格生成一个空图：

```py
fig, ax = plt.subplots()
ax.set_title('Standardized Title')
ax.set_xlabel('Standardized X-label')
ax.set_ylabel('Standardized Y-label');
```

![](img/1d89d4842049de6ad6497cee15cda120.png)

标准化风格（由作者提供）

当你保存样式文件时，你可能会注意到*stylelib*文件夹中充满了现有的*mplstyle*文件。这些文件创建了许多不同的图形格式，你可以查看它们以获取编写自己样式文件的线索。在下一部分，我们将使用这些文件中的一个来覆盖 Matplotlib 的一些默认值。

# 应用样式表

除了让你自定义自己的图形外，Matplotlib 还提供了*预定义*的样式表，你可以通过使用`style.use()`来导入这些样式表。它们的格式与之前创建的样式文件相同。

样式表的外观与*matplotlibrc*文件相同，但在其中，你只能设置与*实际图形样式*相关的`rcParams`。这使得样式表在不同机器之间是可移植的，因为不必担心未安装的依赖项。但不用担心，只有少数`rcParams`不能重置，你可以在[这里](https://matplotlib.org/stable/api/style_api.html#matplotlib.style.use/)查看这些列表。

要查看可用样式表的示例，请访问 Matplotlib 的 [样式表参考页面](https://matplotlib.org/stable/gallery/style_sheets/style_sheets_reference.html)。这些示例以缩略图条带的形式展示，如下所示。一些样式表模仿了流行的绘图库，如 Seaborn 和 ggplot。

![](img/cce2091300acca7d0b843d50b6450cb3.png)

灰度样式表示例（来自 matplotlib.org）

> 一个重要的样式表是 *seaborn-colorblind* 表。这个样式表使用了针对 5%到 10%有色盲的群体设计的“色盲安全”颜色。

让我们尝试使用 Matplotlib 附带的 *grayscale* 样式表来绘制散点图。首先，导入 NumPy 和 Matplotlib，并选择 `grayscale` 样式表。

```py
import numpy as np
import matplotlib.pyplot as plt

plt.style.use('grayscale')
```

接下来，为生成两个不同的点云生成一些虚拟数据：

```py
x = np.arange(0, 20, 0.1)
noise = np.random.uniform(0, 10, len(x))
y = x + (noise * x**2)
y2 = x + (noise * x**3)
```

通过设置并执行 `pyplot` 方法来完成图表的绘制。对两个轴使用对数刻度：

```py
plt.title('Grayscale Style Scatterplot')
plt.xlabel('Log X')
plt.ylabel('Log Y')
plt.loglog()
plt.scatter(x, y2, alpha=0.4, label='X Cubed') 
plt.scatter(x, y, marker='+', label='X Squared')
plt.legend(loc=(1.01, 0.7));
```

![](img/b3e63df8dc9a5322207486ed2ca007da.png)

使用灰度样式表制作的图（作者提供）

你看到的点的位置可能与该图不同，因为使用了随机生成的数据。

如果你打开 *grayscale.mplstyle* 文件，你会发现它看起来非常像我们在“创建和使用样式文件”部分中制作的 *scientific_styles.mplstyle* 文件。因此，如果现有的样式表不完全适合你的目的，你总是可以复制该文件，编辑它，并将其保存为新的样式表！

# 将样式限制在代码块中

如果你只想为 *特定代码块* 使用某种样式，而不是整个脚本或笔记本，样式包提供了一个上下文管理器来将更改限制在特定范围内。有关更多信息，请参阅 [文档](https://matplotlib.org/stable/users/explain/customizing.html)中的“临时样式”。

# 总结

Matplotlib 提供了三种主要的方法来为图表设置样式。你可以在脚本中更改 *运行时配置参数*，制作自己的 *样式文件* 并将其保存在 *stylelib* 文件夹中，或者使用 *stylelib* 文件夹中的预定义 *样式表*。你可以将这些样式更改应用于整个代码或指定的代码块。

将样式参数存储在文件中，可以让你与团队成员共享这些参数，以便每个人的图表都有一致的、统一的外观。除了帮助自定义图表，这些技术还让你通过抽象过程编写更简洁的代码。

# 谢谢！

感谢阅读，请关注我以获取未来更多 *快速成功的数据科学* 项目。有关更多 Matplotlib 的技巧和窍门，请查看我的书的第十九章， [*Python 工具*](https://a.co/d/ggipGxL)。

[](https://a.co/d/ggipGxL?source=post_page-----45591394ff98--------------------------------) [## Python 工具：使用 Anaconda、JupyterLab 和 Python 科学工具的介绍…

### 科学家的 Python 工具：使用 Anaconda、JupyterLab 和 Python 科学库的入门介绍…

[a.co](https://a.co/d/ggipGxL?source=post_page-----45591394ff98--------------------------------)
