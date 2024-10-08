# 从乏味到惊艳：Matplotx 如何改善你的 Matplotlib 图表

> 原文：[`towardsdatascience.com/from-dull-to-stunning-how-matplotx-can-improve-your-matplotlib-charts-f370cbf7acc`](https://towardsdatascience.com/from-dull-to-stunning-how-matplotx-can-improve-your-matplotlib-charts-f370cbf7acc)

## 简化使用 Matplotx 创建惊艳图表的过程

[](https://andymcdonaldgeo.medium.com/?source=post_page-----f370cbf7acc--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----f370cbf7acc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f370cbf7acc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f370cbf7acc--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----f370cbf7acc--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f370cbf7acc--------------------------------) ·6 分钟阅读·2023 年 4 月 3 日

--

![](img/09bb6a98db16792a11f8976944d23011.png)

使用 matplotx 主题应用的 matplotlib 散点图示例。图片由作者提供。

Matplotib 是 Python 世界中最流行的数据可视化库之一。然而，多年来，它因创建乏味的图形和使用不便而声名狼藉。

正如我最新的文章所见，将基本的 matplotlib 图表转换成视觉上吸引人的图表确实需要几行代码。别误解我；我喜欢使用 matplotlib，因为它高度可定制，但有时我只想要一个时尚的图表，而不想过多麻烦。

这就是[**matplox 库**](https://github.com/nschloe/matplotx)的作用。通过两行代码——一个 `import` 语句和一个 `with` 语句——我们可以即时将基本的 matplotlib 图形转变为更具视觉吸引力的图形。

matplotx 库提供了一种简单的方法来即时美化你的 matplotlib 图形。这个库已经存在了一段时间。它的下载次数接近 60,000 次，并且[**平均每月 3,000 次下载**](https://pepy.tech/project/matplotx)（撰写本文时的数据）。

在这篇文章中，我们将看到如何使用这个库来为我们的 matplotlib 图形增添一些趣味。

# 安装 Matplotx

可以通过打开终端/命令提示符并运行以下命令，将 Matplotx 安装到你的 Python 环境中。

```py
pip install matplotx
```

# 导入库和设置数据

在本文中，我们将通过导入两个常见的 Python 库开始：[**pandas**](https://pandas.pydata.org/) 和 [**matplotlib**](https://matplotlib.org/)。然后我们将导入 [**matplotx**](https://github.com/nschloe/matplotx)。

```py
import pandas as pd
import matplotlib.pyplot as plt
import matplotx
```

一旦库被导入，我们可以从 CSV 文件中读取数据集。

在这个例子中，我使用了来自 Volve 数据集的一口井。有关此数据集的详细信息可以在文章底部找到。该数据集包含来自位于挪威大陆架上的 Volve Field 的一口井的系列井日志和岩石物理测量数据。

除了指定文件位置外，我们还需要将缺失数据值-999 转换为 NaN 值。这是通过使用`na_values`参数完成的。

```py
df = pd.read_csv('/well_log_data_volve.csv', na_values=-999)
df.head()
```

数据加载后，我们可以使用`df.head()`查看 Dataframe 的头部。

![](img/4c773b92821814726c239613d3f90271.png)

显示 Volve Field 井日志测量的 Dataframe 头部。图片由作者提供。

# 使用 Matplotx 增强 Matplotlib 散点图

现在数据已经加载，我们将创建的第一个图表是散点图。这可以使用以下代码轻松创建。

```py
plt.scatter(df['NPHI'], df['RHOB'], c=df['GR'])
plt.ylim(3,2)
plt.xlim(-0.15, 0.8)
plt.clim(0, 150)
plt.colorbar(label='GR')
plt.xlabel('NPHI (dec)')
plt.ylabel('RHOB (g/cc)')
plt.show()
```

当我们运行上述代码时，我们得到以下图表。

![](img/ad6045c7b4a7b49e7c9e6c5828cfa9f2.png)

显示密度与中子孔隙度的基本 matplotlib 散点图。图片由作者提供。

尽管我们有一个可用的散点图，但它并没有从其他 matplotlib 散点图中脱颖而出。

## 使用 Matplotx 创建 Dracula 样式的 Matplotlib 散点图

要应用 matplotx 的样式，我们需要在 matplotlib 散点图代码之前添加一行代码。

使用 with 语句，我们调用`plt.style.context`并传入`matplotx.styles`，从这里我们可以选择许多可用的主题之一。

在这个例子中，我选择了 Dracula 主题——一个非常流行的主题，似乎出现在几乎每个应用中。

```py
with plt.style.context(matplotx.styles.dracula):
  plt.scatter(df['NPHI'], df['RHOB'], c=df['GR'])
  plt.ylim(3,2)
  plt.xlim(-0.15, 0.8)
  plt.clim(0, 150)
  plt.colorbar(label='GR')
  plt.xlabel('NPHI (dec)')
  plt.ylabel('RHOB (g/cc)')
  plt.show()
```

在不更改之前的 matplotlib 代码的其他部分的情况下，我们可以运行它并得到以下图表。

![](img/b9b3811c2ef9cc7a5ef3895f8e59c2d9.png)

应用流行的 Dracula 主题与 matplotx 之后的 Matplotlib 散点图。图片由作者提供。

## 使用 Matplotx 和 duftify 函数去除图表垃圾

在为读者创建图表时，通常需要去除图表垃圾，如网格线、边框等。这使读者能够专注于数据，而不是被其他元素分散注意力。

为了查看去除图表垃圾的影响，以下动画是由[**Darkhorse Analytics**](https://www.darkhorseanalytics.com/blog/data-looks-better-naked)创建的。

![](img/8b5a3f53d7d38799674b496b3bf1d690.png)

通过提高数据-墨水比来使图表更有效的示例。[`www.darkhorseanalytics.com/blog/data-looks-better-naked`](https://www.darkhorseanalytics.com/blog/data-looks-better-naked)

为了帮助我们去除像上面动画那样的图表垃圾，Matplotx 提供了一个很好的函数来帮助实现这一点。

要使用它，我们需要调用`matplotx.styles.duftify`，然后传入我们想要使用的 matplotx 样式。

```py
with plt.style.context(matplotx.styles.duftify(matplotx.styles.dracula)):
  plt.scatter(df['NPHI'], df['RHOB'], c=df['GR'])
  plt.ylim(3,2)
  plt.xlim(-0.15, 0.8)
  plt.clim(0, 150)
  plt.colorbar(label='GR')
  plt.xlabel('NPHI (dec)')
  plt.ylabel('RHOB (g/cc)')
  plt.show()
```

运行上述代码后，我们得到以下图表。

![](img/6c45d0811063c142b8c20effa30e2d59.png)

应用 matplotx 的 duftify 函数后的 Matplotlib 散点图。图片由作者提供。

我们可以看到图表和色条的边框已被移除。坐标轴的标签也已被淡化，这样你的眼睛立刻会专注于数据而不是额外的内容。

## 使用 Matplotx 的其他样式

matplotx 提供了许多其他样式，可以在[项目的 GitHub 仓库](https://github.com/nschloe/matplotx)上查看。

这里仅是一些可用样式的样本：

![](img/3e9741c27e6285c1b68edd85d1545ef5.png)

下面是一些来自 matplotx 的样式。图片来源于 matplotx GitHub 仓库。

使用这些样式时需要记住的一点是，如果图形上名称后面有额外的方括号中的文本，那么你需要确保在调用`matplotx.styles`时添加这些文本。

例如，如果我们想使用东京之夜主题，我们将有三种样式可供选择：白天、夜晚和风暴。

![](img/e4f67274b66e8ff60cbe02584efa5d71.png)

东京之夜主题的三种样式。请注意方括号中的名称，可以用来访问该主题。图片来源于 matplotx GitHub 仓库。

从上面的描述来看，如果我们选择东京之夜（白天），那么我们需要在调用末尾添加方括号：`matplotx.styles.tokyo_night['day']`

```py
with plt.style.context(matplotx.styles.tokyo_night['day']):
  plt.scatter(df['NPHI'], df['RHOB'], c=df['GR'])
  plt.ylim(3,2)
  plt.xlim(-0.15, 0.8)
  plt.clim(0, 150)
  plt.colorbar(label='GR')
  plt.xlabel('NPHI (dec)')
  plt.ylabel('RHOB (g/cc)')
  plt.show()
```

运行上述代码将生成以下所需样式的图表。

![](img/a510a850ca06bb063dd33d7fb57d6aaf.png)

应用 matplotx 的东京之夜白天主题后的 Matplotlib 散点图。图片由作者提供。

# 将 Matplotx 应用于其他 Matplotlib 图表

我们还可以将 matplotx 样式应用到其他图表上，例如直方图。我们需要做的就是修改 matplotlib 绘图代码，以获得直方图。

```py
with plt.style.context(matplotx.styles.pitaya_smoothie['dark']):
  plt.hist(df['RHOB'], bins=50, alpha=0.8)
  plt.xlim(2, 3)
  plt.xlabel('RHOB (g/cc)')
  plt.show()
```

当我们运行上述代码时，我们得到的直方图比我们常见的基本蓝色条形图要美观得多。

![](img/51152118c2f20c42d79bce84d554f009.png)

应用 matplotx 主题的直方图示例。图片由作者提供。

# 总结

尽管 Matplotlib 是一个强大的数据可视化库，但使用起来可能会比较麻烦，尤其是当你想创建令人惊叹的图形时。使用 matplotx，我们只需添加两行代码，即可瞬间将图表转变为更加出色的样子。它绝对是你在创建下一个图形时，想要快速添加一些风格时应该考虑的库。

*感谢阅读。在你离开之前，你一定要订阅我的内容，将我的文章直接送入你的收件箱。* [***你可以在这里订阅！***](https://andymcdonaldgeo.medium.com/subscribe)*另外，你也可以* [***注册我的新闻通讯***](https://fabulous-founder-2965.ck.page/2ca286e572) *，以免费将额外内容直接送到你的收件箱。*

*其次，您可以通过注册会员，获得完整的 Medium 体验，并支持我和其他成千上万的作者。这只需每月$5，您就可以完全访问所有精彩的 Medium 文章，并有机会通过您的写作赚取收入。如果您使用* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***,*** *您将直接通过您的费用的一部分支持我，而不会增加额外的费用。如果您这样做，非常感谢您的支持！*

# 本教程中使用的数据

本教程使用的数据是 Equinor 在 2018 年发布的 Volve 数据集的一个子集。数据集的完整详细信息，包括许可证，可以在下面的链接中找到。

[## Volve 数据集

### Equinor 官方已经将北海油田的完整数据集提供用于研究、学习等…

[www.equinor.com](https://www.equinor.com/energy/volve-data-sharing?source=post_page-----f370cbf7acc--------------------------------)

Volve 数据许可证基于 CC BY 4.0 许可证。许可证协议的完整详细信息可以在此处找到：

[`cdn.sanity.io/files/h61q9gi9/global/de6532f6134b9a953f6c41bac47a0c055a3712d3.pdf?equinor-hrs-terms-and-conditions-for-licence-to-data-volve.pdf`](https://cdn.sanity.io/files/h61q9gi9/global/de6532f6134b9a953f6c41bac47a0c055a3712d3.pdf?equinor-hrs-terms-and-conditions-for-licence-to-data-volve.pdf=)
