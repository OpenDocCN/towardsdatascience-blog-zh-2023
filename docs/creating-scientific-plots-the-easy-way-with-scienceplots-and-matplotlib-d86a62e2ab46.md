# 使用 scienceplots 和 matplotlib 轻松创建科学图表

> 原文：[`towardsdatascience.com/creating-scientific-plots-the-easy-way-with-scienceplots-and-matplotlib-d86a62e2ab46`](https://towardsdatascience.com/creating-scientific-plots-the-easy-way-with-scienceplots-and-matplotlib-d86a62e2ab46)

## 使用几行 Python 代码立即转换你的 Matplotlib 图形

[](https://andymcdonaldgeo.medium.com/?source=post_page-----d86a62e2ab46--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----d86a62e2ab46--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d86a62e2ab46--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d86a62e2ab46--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----d86a62e2ab46--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d86a62e2ab46--------------------------------) ·阅读时间 9 分钟·2023 年 7 月 17 日

--

![](img/99d7369c3c290a5d58a483b7d0b9b0da.png)

照片由[Braňo](https://unsplash.com/fr/@3dparadise?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在为学术期刊撰写文章时，图表的布局和样式应符合预定格式。这确保了该出版物所有文章的一致性，并且所包含的图表在打印时质量高。

Python 在科学界广泛使用，并提供了创建科学图表的绝佳方式。然而，当我们使用 matplotlib 这一 Python 中最流行的绘图库时，默认的图表质量较差，需要调整以满足要求。

更改 matplotlib 图形的样式可能很耗时，这就是[scienceplots](https://github.com/garrettj403/SciencePlots)库的用武之地。只需几行代码，我们就能立即改变图形的外观，而不必花费太多时间去调整图形的各个部分。

[scienceplots](https://github.com/garrettj403/SciencePlots)库允许用户创建类似于学术期刊和研究论文中常见的简单而信息丰富的图表。不仅如此，它还将某些样式的 DPI 设置为 600，这通常是出版物要求的，以确保高质量的打印图像。

scienceplots 库包含众多样式，包括对多种语言的支持，如中文和日文。你可以通过下面的链接探索[scienceplots](https://github.com/garrettj403/SciencePlots)库中的所有样式。

[](https://github.com/garrettj403/SciencePlots/wiki/Gallery?source=post_page-----d86a62e2ab46--------------------------------) [## 画廊

### 科学绘图的 matplotlib 样式。通过创建一个帐户来贡献于 garrettj403/SciencePlots 的开发…

[github.com](https://github.com/garrettj403/SciencePlots/wiki/Gallery?source=post_page-----d86a62e2ab46--------------------------------)

在本文中，我们将探讨如何将一些基本的常见数据可视化转换为可以包含在科学出版物中的内容。

# 设置 scienceplots

在使用 scienceplots 库创建图表之前，你需要确保你的计算机上已安装了 [LaTeX](https://www.latex-project.org/)。LaTeX 是一个排版系统，专为技术和科学文档的创建而设计。

如果你还没有在机器上安装 LaTeX，可以在 [这里](https://www.latex-project.org/get/) 和 [这里](https://github.com/garrettj403/SciencePlots/wiki/FAQ) 找到有关 LaTeX 及其安装的更多详细信息。

如果你在 Google Colab 上运行，你可以在单元格中运行以下代码以安装 LaTeX。

```py
!sudo apt-get install dvipng texlive-latex-extra texlive-fonts-recommended texlive-latex-recommended cm-super
```

在设置 LaTeX 之后，我们可以使用 pip 安装 scienceplots 库：

```py
pip install SciencePlots
```

一旦在你选择的平台上安装了库和 LaTeX，你就可以导入 scienceplots 库以及 [matplotlib](https://matplotlib.org/)。

```py
import scienceplots
import matplotlib.pyplot as plt
```

# 为绘图创建虚拟数据

在生成一些图表之前，我们首先需要创建一些样本数据。稍后我们将看到 scienceplots 库如何处理实际数据。

在本文的这一部分，我们将使用 `np.linspace` 创建一些线性间隔的值，然后对这些数据进行一些随机的数学计算。

```py
# Generate x values
x = np.linspace(0, 10, 20)

# Generate y values with random noise
y = np.sin(x)
y2 = np.cos(x)
y3 = y2 * 1.5
```

一旦我们创建了数据（或从 csv 文件中加载到 pandas 中），我们就可以开始创建我们的图表。

# 使用 Matplotlib 创建带标记的折线图

我们将使用的第一个图是折线图。这可以通过使用 matplotlib 的 `.plot()` 函数并传入 `x` 和 `y` 参数所需的数据来轻松创建。

由于我们处理的是从方程中派生的变量，因此有时在图例中包含这些变量对于读者理解它们可能很有帮助。

matplotlib 的一个优点是我们可以使用 LaTeX 方程作为标签。我们只需将方程用美元符号（`$`）括起来。

```py
plt.figure(figsize = (6,6))
plt.plot(x, y, marker='o', label='$y=sin(x)$')
plt.plot(x, y2, marker='o', label='$y=cos(x)$')
plt.plot(x, y3, marker='o', label='$y=y2*1.5$')

plt.xlabel('X')
plt.ylabel('Y')
plt.legend()
plt.show()
```

当我们运行上述代码时，我们将得到以下非常基本的 matplotlib 图形，具有标准颜色。

![](img/fc6784deee83168c8a847a9a45f5d259.png)

应用 scienceplots 之前的基本 matplotlib 折线图。图像由作者提供。

尽管上面的图形看起来可用，但它的质量（dpi 和大小）以及样式可能不完全适合在期刊中发表。

# 将 scienceplots 样式应用于折线图

为了即时转换我们的图形，我们可以添加一行代码：一个 `with` 语句，它调用 matplotlib 的 `style.context` 函数，并允许我们传入 [scienceplots 提供的多种风格](https://github.com/garrettj403/SciencePlots/wiki/Gallery)之一。

```py
with plt.style.context(['science', 'high-vis']):
    plt.figure(figsize = (6,6))
    plt.plot(x, y, marker='o', label='$y=sin(x)$')
    plt.plot(x, y2, marker='o', label='$y=cos(x)$')
    plt.plot(x, y3, marker='o', label='$y=y2*1.5$')
    plt.xlabel('X Variable (mm)')
    plt.ylabel('Y Variable')
    plt.legend()
    plt.show()
```

当我们运行上述代码时，我们会得到如下图，这图更适合用于期刊出版。

![](img/3009d64243a27e701cbb8c59587508d3.png)

应用 scienceplots 风格后的 Matplotlib 线图。图片由作者提供。

该图形简单（即没有图表杂质），不同的线条很容易区分。此外，当在 Jupyter Notebook 中查看此图形时，即使我们设置了相对较小的图形尺寸，它可能看起来仍然非常大。这是因为图形的 DPI 设置为 600，这通常是许多出版物的要求，并确保图形尽可能清晰。

让我们尝试应用另一种风格。这次我们将使用 [电气和电子工程师协会（IEEE）](https://www.ieee.org/) 的风格。

要做到这一点，我们只需将 `high-vis` 替换为 `ieee` 即可改变风格。

```py
with plt.style.context(['science', 'ieee']):
    plt.figure(figsize = (6,6))
    plt.plot(x, y, marker='o', label='$y=sin(x)$')
    plt.plot(x, y2, marker='o', label='$y=cos(x)$')
    plt.plot(x, y3, marker='o', label='$y=y2*1.5$')
    plt.xlabel('X')
    plt.ylabel('Y')
    plt.legend()
    plt.show()
```

当我们运行上述代码时，我们将获得符合 IEEE 推荐风格的下图。

![](img/6ddea36204928db1a61e68e3811882a6.png)

应用 scienceplots IEEE 风格后的 Matplotlib 线图。图片由作者提供。

# 带有 Science Plots 的直方图

在之前的示例中，我们探讨了如何将风格应用于线图。

但是我们能将相同的风格应用于其他类型的图吗？

当然可以！

让我们看看如何将这种风格应用于直方图。

首先，让我们使用以下代码创建一个使用伽马射线（地质构造自然放射性测量）数据的 matplotlib 图形。为了展示第二个数据集，我将相同的数据调整了 20 个 API 单位。

```py
plt.figure(figsize = (6,6))
plt.hist(df['GR'], bins=100, label='GR1', alpha =0.5)
plt.hist(df['GR']+20, bins=100, label='GR2', alpha=0.5)
plt.xlim(0, 150)
plt.xlabel('Gamma Ray')
plt.ylabel('Frequency')
plt.legend()
plt.show()
```

当我们运行上述代码时，我们会得到以下图形。

![](img/8f4124ae065c69dff98f92bcfa351422.png)

简单的 Matplotlib 伽马射线测量直方图。图片由作者提供。

我们会注意到它使用了 matplotlib 的标准风格，看起来非常基础，两组数据重叠在一起。这导致一些信息被遮盖。

让我们看看 IEEE 风格如何改变了这些内容。

```py
with plt.style.context(['science', 'ieee']):
    plt.figure(figsize = (6,6))
    plt.hist(df['GR'], bins=100, label='GR1')
    plt.hist(df['GR']+20, bins=100, label='GR2')
    plt.xlim(0, 150)
    plt.xlabel('Gamma Ray')
    plt.ylabel('Frequency')
    plt.legend()
    plt.show()
```

当我们运行上述代码时，我们得到应用 IEEE 风格后的下图。然而，第二个 GR 数据集仍然遮挡了第一个。

![](img/7846b7406e3b9b819e0a3275ba6c138c.png)

应用 scienceplots IEEE 风格后的 Matplotlib 伽马射线测量直方图。图片由作者提供。

也许我对 scienceplots 库能处理任何重叠并自动应用透明度有较高的期望。

然而，这并不是太费力。我们只需为每个数据集添加 alpha 参数。

```py
with plt.style.context(['science', 'ieee']):
    plt.figure(figsize = (6,6))
    plt.hist(df['GR'], bins=100, label='GR1', alpha=0.5)
    plt.hist(df['GR']+20, bins=100, label='GR2', alpha=0.5)
    plt.xlim(0, 150)
    plt.xlabel('Gamma Ray')
    plt.ylabel('Frequency')
    plt.legend()
    plt.show()
```

当我们运行上述代码并进行 alpha 更改时，我们得到如下图形。

![](img/66db3b0826299636032fe424dc04e2f1.png)

在应用 scienceplots 并向数据集添加透明度后得到的伽马射线测量直方图。图片由作者提供。

现在我们可以看到两个数据集的条形图变化。

建议您检查预期出版物的样式指南，以确保使用透明度是可接受的。在大多数情况下，应该是可以的，但值得检查一下。

# 将 Science Plots 应用于 Seaborn 图形

我们不仅限于将 scienceplots 库的样式应用于 matplotlib 图形。我们还可以将其应用于[Seaborn](https://seaborn.pydata.org/)图形。这是因为 Seaborn 基于 matplotlib 代码。

有时候，在创建图形时，[Seaborn](https://seaborn.pydata.org/)提供了比 matplotlib 更简便的方法来创建某些图形。例如，当我们有一个基于文本的分类变量时，我们希望能够绘制它，而不需要为每个类别添加一个单独的散点图。

在这个例子中，我们有一些中子孔隙度和体积密度数据——常见的测井测量。对于每个测量，我们还拥有一个岩性类别。

这个数据集源自 Force 2020 Xeek 机器学习竞赛数据集。详细信息可以在文章末尾找到。

首先，我们需要将 seaborn 导入到我们的笔记本中。

```py
import seaborn as sns
```

导入 seaborn 库后，我们可以使用以下代码创建散点图。

```py
plt.figure(figsize=(6, 6))
sns.scatterplot(data=df, x='NPHI', 
                y='RHOB', hue='LITH', s=10)

plt.ylabel('Bulk Density (RHOB) - g/cc')
plt.xlabel('Neutron Porosity (NPHI) - dec')
plt.ylim(2.8, 1.4)
plt.show()
```

当我们运行上述代码时，我们会得到以下散点图，其中数据通过不同的岩性进行了着色。

![](img/47c89f4c2a4d6d9c2b18964a47d94436.png)

使用 Seaborn 生成的基本中子密度交叉图。图片由作者提供。

看起来还不错。然而，我们需要确保样式适合预期的期刊，并且颜色对所有读者都是可访问的。

为了应用我们的 scienceplots 样式，我们可以使用与之前相同的语法：

```py
with plt.style.context(['science', 'ieee']):
    plt.figure(figsize=(10, 8))
    sns.scatterplot(data=df, x='NPHI', y='RHOB', hue='LITH', s=10)
    plt.ylim(2.8, 1.4)
    plt.title('RHOB vs NPHI Crossplot')
    plt.show()
```

当我们运行上述代码时，我们会得到以下具有改进样式的图形，包括新的色彩调色板。

![](img/b51e957d4ca3ab217abfb9161ce8c506.png)

Seaborn 散点图，应用 scienceplots 样式，展示了体积密度与中子孔隙度的关系，并根据岩性变化着色。图片由作者提供。

选择图形的色彩调色板可能会很棘手且耗时；然而，经过一些考虑，它可以使您的图形对视觉障碍的读者更为友好。

如果您想找到一些工具来帮助您选择有效且可访问的色彩调色板，请查看下面的链接。

+   **4 种帮助您选择数据可视化色彩调色板的必备工具**

此外，当在黑白打印时，一些颜色可能难以区分。因此，考虑为不同类别分配不同的形状可能是值得的。这一点在我们处理来自实验室过程的小数据集时尤为重要。

# 摘要

在本文中，我们探讨了如何迅速将基本的 matplotlib 图形转换为可以轻松添加到科学出版物中的内容。这些图形可能仍需进一步调整，但通过使用 scienceplots 库，我们可以基本实现目标。此外，建议检查你所选期刊的作者工具包，以确保你创建的图表符合要求的标准。

# 本教程使用的数据集

训练数据集是 Xeek 和 FORCE 2020 机器学习比赛的一部分 *(Bormann et al., 2020)*。该数据集在 Creative Commons Attribution 4.0 International 许可下发布。

完整的数据集可以通过以下链接访问：[`doi.org/10.5281/zenodo.4351155`](https://doi.org/10.5281/zenodo.4351155)。

*感谢阅读。在离开之前，你绝对应该订阅我的内容，获取我的文章到你的邮箱中。* [***你可以在这里做到这一点！***](https://andymcdonaldgeo.medium.com/subscribe)

*其次，你可以通过注册会员来获得完整的 Medium 体验，并支持成千上万的其他作者和我。这只需每月 $5，你就能全面访问所有精彩的 Medium 文章，并且有机会通过你的写作赚取收入。*

*如果你使用* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***注册***，*你将直接通过一部分费用支持我，而且不会增加你的开支。如果你这样做，非常感谢你的支持。*
