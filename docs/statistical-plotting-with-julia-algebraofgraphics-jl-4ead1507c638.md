# 使用 Julia 进行统计绘图：AlgebraOfGraphics.jl

> 原文：[https://towardsdatascience.com/statistical-plotting-with-julia-algebraofgraphics-jl-4ead1507c638?source=collection_archive---------8-----------------------#2023-04-19](https://towardsdatascience.com/statistical-plotting-with-julia-algebraofgraphics-jl-4ead1507c638?source=collection_archive---------8-----------------------#2023-04-19)

![](../Images/26ea2729d5703d04b6d7676e1fa51b46.png)

Phyto by [Antoine Dautry](https://unsplash.com/@antoine1003?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/de/fotos/05A-kdOH6Hw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 如何使用 AlgebraOfGraphics.jl（以及 Makie.jl）包创建统计图表

[](https://medium.com/@schaetzle.ka?source=post_page-----4ead1507c638--------------------------------)[![Roland Schätzle](../Images/5d03aad32cda174f2fee595a3fc34a17.png)](https://medium.com/@schaetzle.ka?source=post_page-----4ead1507c638--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4ead1507c638--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4ead1507c638--------------------------------) [Roland Schätzle](https://medium.com/@schaetzle.ka?source=post_page-----4ead1507c638--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8ada39358e9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstatistical-plotting-with-julia-algebraofgraphics-jl-4ead1507c638&user=Roland+Sch%C3%A4tzle&userId=8ada39358e9d&source=post_page-8ada39358e9d----4ead1507c638---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4ead1507c638--------------------------------) · 9分钟阅读 · 2023年4月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4ead1507c638&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstatistical-plotting-with-julia-algebraofgraphics-jl-4ead1507c638&user=Roland+Sch%C3%A4tzle&userId=8ada39358e9d&source=-----4ead1507c638---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4ead1507c638&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstatistical-plotting-with-julia-algebraofgraphics-jl-4ead1507c638&source=-----4ead1507c638---------------------bookmark_footer-----------)

图形语法（GoG）是一个理论概念，是许多流行图形包（如 R 中的 ggplot2 或 Python 中的 ggplot）的基础。在 Julia 生态系统中，甚至有几个基于 GoG 的图形包。因此，用户有选择的余地。因此，我创建了这一系列文章，以比较这些包，从而使选择变得更容易。

我以 [GoG 介绍](/the-grammar-of-graphics-or-how-to-do-ggplot-style-plotting-in-julia-1b0ac2162c82) 开始了这一系列，并已介绍了图形包 `[Gadfly.jl](http://gadflyjl.org/stable/)` ([*使用 Julia 进行统计绘图：Gadfly.jl*](/statistical-plotting-with-julia-gadfly-jl-39582f91d7cc)) 和 `[VegaLite.jl](https://www.queryverse.org/VegaLite.jl/stable/)` ([*使用 Julia 进行统计绘图：VegaLite.jl*](/statistical-plotting-with-julia-vegalite-jl-ad6fda253215))。

`[AlgebraOfGraphics.jl](https://aog.makie.org/stable/)`[-package (*AoG*)](https://aog.makie.org/stable/) 现在是我在这一系列中介绍的基于图形语法（GoG）的 *第三* 个图形包。

对于本文章中演示 *AoG* 的示例，我将使用与之前文章中完全相同的数据（数据的详细说明可以在 [这里](/the-grammar-of-graphics-or-how-to-do-ggplot-style-plotting-in-julia-1b0ac2162c82) 找到），并尝试创建与之前完全相同的可视化（条形图、散点图、直方图、箱形图和小提琴图），以便对所有包进行 1:1 的比较。我假设用于示例的数据已经准备好，分别为 DataFrames `countries`、`subregions_cum` 和 `regions_cum`（如前所述）。

# AlgebraOfGraphics

*AoG* 包可能是到目前为止对 GoG 的最纯粹的实现，正如我们在接下来的示例中将看到的。它建立在坚实的数学概念之上，作者将其描述为“*一种用于数据可视化的声明式、问题驱动的语言*”。其主要开发者是 [Pietro Vertechi](https://piever.github.io/index.html)。

在技术层面上，它采用了与我们到目前为止看到的包完全不同的方法：`Gadfly.jl` 是一个完全用 Julia 编写的独立图形包，而 `VegaLite.jl` 是一个用于 Vega-Lite 图形引擎的 Julia 接口，*AoG* 则是 `[Makie.jl](https://docs.makie.org/stable/)` 的附加包。[*Makie*](https://docs.makie.org/stable/) 本身是 Julia 生态系统中最年轻的图形包（也是完全用 Julia 编写的）。

*AoG* 和 *Makie* 之间的界限是流动的。*AoG* 的几个元素使用了 *Makie* 的属性，并且如果某些方面无法使用 *AoG* 本身的概念来表达，*Makie* 总是备用解决方案。

还应注意的是，*AoG* 仍然在进行中。0.1 版本仅于 2020 年发布。因此，它不如其他更成熟的包完整，某些方面尚未完全实现。

# 条形图

所以让我们进入第一个可视化图，它分别使用条形图描绘地区（即大陆）和子区域的人口规模。

## 按地区划分的人口

首先，我们想展示每个地区（即大陆）在 2019 年的人口规模，作为条形图中的一条条形。除此之外，每个“地区条形”应具有不同的颜色。

使用这个简单的例子，我们可以深入了解 *AoG* 的基本概念：用 GoG 术语来说，这个可视化基于 `regions_cum` 数据框的数据显示，并包含：

+   将数据属性 `Region` 映射到 x 轴

+   将数据属性 `Pop2019` 映射到 y 轴

+   将数据属性 `Region` 映射到颜色

+   使用“条形”几何体

正如我在 GoG 介绍中所解释的，其理念之一是可以从不同的构建块创建可视化规范，这些块可以根据特定需求组合。*AoG* 完全实现了这一理念。因此，我们可以直接将 GoG 描述转换为 *AoG* 元素：

+   `regionPop2xy = mapping(:Region, :Pop2019)` 是将 `Region` 映射到 x 轴，`Pop2019` 映射到 y 轴

+   `region2color = mapping(color = :Region)` 是将 `Region` 映射到颜色的映射

+   `barplot = visual(BarPlot)` 是“条形”几何体

现在我们可以组合这些构建块（使用操作符 `*`），从 `regions_cum` 中获取数据，并通过调用 `draw` 创建图表：

```py
draw(data(regions_cum) * regionPop2xy * region2color * barplot)
```

这会生成以下条形图：

![](../Images/54685434d0d2f19a3bb6aa7e0dff3fd9.png)

按地区划分的人口（1） [图片由作者提供]

与之前的文章一样，我们还为每个可视化创建了美化版本，添加了标签、标题和漂亮的背景色等。这可以在 *AoG* 中使用 *Makie* 参数 `axis` 和 `figure` 来 `draw` 实现：

这将生成以下图表：

![](../Images/1f431ce22bec088942a09196a40a946d.png)

按地区划分的人口（2） [图片由作者提供]

## 按子区域划分的人口

现在，让我们继续可视化按 *子区域* 划分的人口。这基本上与上述图表相同，但我们使用 `subregions_cum` 中的数据，而不是 `regions_cum`。

所以我们现在的轴映射是 `subregionPop2xy = mapping(:Subregion, :Pop2019)`。由于我们希望子区域的条形图按地区着色，我们可以重复使用上面的映射，基本图表可以通过以下方式绘制：

```py
draw(data(subregions_cum) * subregionPop2xy * region2color * barplot)
```

这将生成以下图表：

![](../Images/9edbde8b361d151832db3e7a464a4334.png)

按人口划分的子区域（1） [图片由作者提供]

显然，如果我们选择水平条形图，子区域标签会更易读。这可以通过交换映射到轴的数据属性实现：`subregionPop2xy_hor = mapping(:Pop2019, :Subregion)` 并在 `visual` 中添加 `orientation = :x`。绘制水平版本的条形图的代码是：

```py
draw(data(subregions_cum) * subregionPop2xy_hor * region2color * 
     visual(BarPlot; direction = :x))
```

不幸的是，这是一个说明*AoG*仍在进展中的规格。在渲染过程中一定有一些 bug，因为这个`draw`命令的结果如下所示：

![](../Images/464e849885db8713005d1a24478dfd17.png)

按人口划分的子区域（2）[图片来源：作者]

y 轴上的刻度以及条形图的位置不正确，x 轴上的刻度也不是我们想要的。

## 使用Makie.jl按子区域划分的人口

因此，我们把这个问题作为一个机会来切换到`Makie.jl`。*Makie*是一个相当低级的图形包。在我们之前看到的包中自动获得的许多功能，在*Makie*中需要明确指定。这给程序员提供了很多控制权，但使得规格描述非常冗长。

另一个缺点是，*Makie*无法处理名义数据。所有名义数据在可视化之前必须转换为数字形式。在我们的例子中，这意味着我们需要将`Region`和`Subregion`属性的名义数据转换为数字：

+   对于`Subregion`来说，这相对简单，因为这个属性包含唯一值。因此，我们只需使用该数据帧列的索引值，并将其存储在新列`subregion_num`中。

+   `Region`值不是唯一的。因此，我们首先将其转换为`CategoricalArray`，这会隐式地映射到数值。然后，我们可以使用函数`levelcode`获得相应的数字，并将其存储在另一个新列`region_num`中。

除此之外，我们从`ColorSchemes.jl`中选择了一个合适的颜色方案（`Set2_8`），以获得美观且易于区分的区域颜色。这个方案如下所示：

![](../Images/878d4a4f8710656d5f8cf22b75df6970.png)

颜色方案Set2_8 [图片来源：作者]

为了进行这些准备，我们需要以下代码：

然后我们将直接创建一个“美化”版的条形图，带有标签等。在*Makie*中，我们需要一个`Figure`作为基础元素，`barplot`可以放置在其中。由于*Makie*无法处理名义数据，我们还需要使用`yticks`属性手动指定 y 轴的刻度，如下代码所示，这样可以创建我们的水平条形图：

这段代码比较长，但结果看起来相当令人满意：

![](../Images/744c720764f3ec749a47da81629c1dd4.png)

按子区域划分的人口（3）[图片来源：作者]

为了获得一个按人口规模排序的条形图版本，我们需要使用`sort!(subregions_cum, :Pop2019)`相应地对`subregions_cum`中的数据进行排序，然后重新执行上面的代码（包括映射到数字数据）。这将生成如下图表：

![](../Images/aed644a3be9d2dd460fe9528626a53d8.png)

按子区域划分的人口（4）[图片来源：作者]

# 散点图

在这次*Makie*的插曲之后，我们回到*AoG*，尝试可视化人口变化如何依赖于人口规模。我们可以使用如下的散点图来实现：

```py
popChangeVsPop = data(countries) * 
      mapping(:Pop2019, :PopChangePct) * 
      mapping(color = :Region)
draw(popChangeVsPop)
```

规格包括将`Pop2019`映射到x轴，将`PopChangePct`映射到y轴，以及将`Region`映射到颜色（我们本可以在此时重用`region2color`，但也可以直接指定映射）。此处可以省略`visual`，因为在这种情况下，*AoG*默认使用点几何图形（`Scatter`）。这给我们提供了以下图表：

![](../Images/039ec366b41f0dfbf2b831ca03ab9250.png)

人口相关的增长率（1） [作者提供的图片]

与之前的文章一样，我们现在通过在x轴上使用对数刻度来改进可视化，因为数据相当偏斜。此外，我们通过添加标签、标题等来进行“美化”。所有这些可以通过重用图表规格`popChangeVsPop`并通过传递适当的参数给`draw`来实现：

这导致了以下图表：

![](../Images/be2f2c105dd2ee9690d37839d95f19d4.png)

人口相关的增长率（2） [作者提供的图片]

# 直方图

现在我们切换到直方图，用于描述不同国家之间的人均GDP分布。由于*AoG*提供了所谓的`histogram`-*analysis*，规格非常简单：

```py
draw(data(countries) * mapping(:GDPperCapita) * histogram())
```

*分析*在*AoG*中是可视化之前处理数据的一种方式。通常几何图形（`visual`）直接依赖于*分析*，例如在这个例子中，直方图将自动使用条形几何图形进行显示。

![](../Images/dd260534468adcf29abcc9dc8ae05c9e.png)

人均GDP的分布（1） [作者提供的图片]

直方图的创建可以通过改变箱数（通过参数`bins`）和使用不同的`normalization`算法来影响。因此，我们通过以下规格得到了改进版本：

这段代码再次展示了*AoG*如何将可视化的规格（`histGDPperCapita`）与其“美化”（在调用`draw`时）分离开来，从而生成以下图表：

![](../Images/b90e35c50278487bc5678ebd49b3b139.png)

人均GDP的分布（2） [作者提供的图片]

# 箱线图和小提琴图

最后，我们使用箱线图和小提琴图可视化每个地区的人均GDP分布。这可以像之前一样简单实现，因为*AoG*为这两种图表变体提供了特定的几何图形。

为了最大化元素的重用，我们首先定义数据和分布的映射（`distGDPperCapita`），然后添加几何图形（使用`visual`）。如所有示例中所示，可以通过在调用`draw`时传入适当的参数来添加额外的“美化”。

这段代码创建了以下两个图表：

![](../Images/3fb330d6698d9f4894836d607b0fa7ef.png)

按地区划分的人均GDP分布（1） [作者提供的图片]

![](../Images/cf62a6ab1a3a75af617283f66d50d3de.png)

按地区划分的人均GDP分布（2） [作者提供的图片]

## 放大

由于两个图中“最有趣”的部分都位于0到100,000的范围内（在y轴上），我们希望将图表限制在该范围内（进行某种缩放）。

在*AoG*中，这可以通过`datalimits`参数应用于`visual`来实现。但似乎*AoG*中存在另一个bug，因为这个参数只有在用于小提琴图时才有期望的效果，而在应用于箱线图时则没有任何变化。

因此，使用以下规格 …

```py
violinRestricted = distGDPperCapita * 
                   visual(Violin; show_notch = true, datalimits = (0, 100000))
drawDist(violinRestricted)
```

… 我们得到这个图表：

![](../Images/69905fb63cb801c8b501ec92e423a4d8.png)

各地区人均GDP分布 (3) [图片由作者提供]

# 结论

如上所述，*AoG*包显然是我们在本系列中看到的最纯粹的图形语法实现。它确实将映射、几何等分离成不同的构建模块，然后可以使用`*`运算符组合起来。它还清晰地将更多“装饰性”元素（所有我们上面称之为“美化”的东西）与实际可视化分开，从而使规格更加模块化，并提供了更多可以重用的构建块。

我认为，对于这样一个年轻的包来说，仍有一些粗糙之处是很正常的，但它确实有坚实的基础，看起来相当有前景。当然，在这篇文章中无法展示*AoG*的所有功能。因此，如果你想了解更多，请查看[文档](https://aog.makie.org/stable/)。最后但同样重要的是，了解这种方法的基本理念也是值得的，可以在[这里](https://aog.makie.org/stable/philosophy/)找到。

对于那些希望深入研究代码的人，还有一个[Pluto notebook](https://github.com/roland-KA/StatisticalPlotsWithJulia/blob/main/notebooks/DV-Basics-AlgebraOfGraphics.jl)，其中包含我在GitHub库中展示的所有示例。
