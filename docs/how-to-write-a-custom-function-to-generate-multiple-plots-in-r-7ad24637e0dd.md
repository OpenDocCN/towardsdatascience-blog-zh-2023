# 如何编写自定义函数以在 R 中生成多个图表

> 原文：[https://towardsdatascience.com/how-to-write-a-custom-function-to-generate-multiple-plots-in-r-7ad24637e0dd?source=collection_archive---------2-----------------------#2023-04-11](https://towardsdatascience.com/how-to-write-a-custom-function-to-generate-multiple-plots-in-r-7ad24637e0dd?source=collection_archive---------2-----------------------#2023-04-11)

## 轻松介绍自定义函数的编写

[](https://medium.com/@create_self?source=post_page-----7ad24637e0dd--------------------------------)[![Vivian Peng](../Images/867b8bbfe22ae0881776fef31108fe89.png)](https://medium.com/@create_self?source=post_page-----7ad24637e0dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ad24637e0dd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7ad24637e0dd--------------------------------) [Vivian Peng](https://medium.com/@create_self?source=post_page-----7ad24637e0dd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffd5a22d4fcc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-write-a-custom-function-to-generate-multiple-plots-in-r-7ad24637e0dd&user=Vivian+Peng&userId=fd5a22d4fcc&source=post_page-fd5a22d4fcc----7ad24637e0dd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ad24637e0dd--------------------------------) · 9 分钟阅读 · 2023年4月11日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7ad24637e0dd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-write-a-custom-function-to-generate-multiple-plots-in-r-7ad24637e0dd&source=-----7ad24637e0dd---------------------bookmark_footer-----------)![](../Images/a796c3be75f979deddaa04aa1bd47593.png)

一双手在笔记本电脑上打字，屏幕上显示代码的视觉图像。图片由作者提供

我总是觉得在 R 中编写函数很有压力，因为我习惯了使用 [tidyverse](https://www.tidyverse.org/) 提供的现成解决方案。当我开始用 Python 编程时，我发现自己越来越多地编写自定义函数，以将我喜欢的 `dplyr` 函数从 R 转移到 Python 中。

学习如何在 Python 中编写函数使我在 R 编程方面也变得更好。它帮助我自动化工作并确保结果可复现。我最常用自定义函数的场景是生成多个 R 图表。

## 编写自定义函数以生成 R 中的图表的理由：

**1\. 自动化报告：** 有时你需要为不同的变量构建相同类型的视觉报告（例如柱状图）。例如，我曾在大流行期间为洛杉矶市长 Eric Garcetti 工作。我们的数据团队负责为他的每日 Covid 新闻发布会制作报告，涵盖了每日病例、死亡、住院、检测和疫苗接种率。我们对这些变量使用了相同的图表格式——柱状图。为了避免每次为不同变量重复图表代码，我们通过编写自定义函数循环处理变量并生成图表，从而自动化了报告过程。这样，我们每天只需提取数据并运行代码脚本以生成新图表。

**2\. 构建仪表盘的跳板：** 一旦你自动化了报告过程，将报告转化为仪表盘以获得互动体验是一个自然的进展。R 具有一个很棒的仪表盘库，[Shiny](https://shiny.rstudio.com/)，可以轻松地用 R 语言直接构建 Web 应用程序。当你已经有一个用于生成图表的自定义函数时，可以轻松地在仪表盘代码中使用相同的函数，以便用户可以选择感兴趣的变量。

**3\. 为** [**Plotly**](https://plotly.com/)** 创建 DIY facet wrap：** 我最喜欢 `[ggplot2](https://ggplot2.tidyverse.org/)` 的功能之一是使用 `facet_wrap`，它可以在一个视图中生成多个子图。这在 `ggplot2` 中只需一行代码。不幸的是，目前 Plotly 中还没有类似的功能，所以我不得不使用自定义函数重新创建它。

我承认我们现在可以使用 `ggplotly()` 创建一个带有 `facet_wrap` 的 `ggplot2` 图表的互动版本。但我个人发现使用 `ggplotly()` 性能较差，因此选择直接在 Plotly 中自定义构建所有内容。

## 以下是创建自定义函数的逻辑分解：

1\. 首先创建一个可视化

2\. 了解你要用来创建多个图表的变量

3\. 将图表代码转换为函数

4\. 循环遍历你的唯一值以生成多个图表

让我们使用可爱的 [Palmer Penguins 数据集](https://allisonhorst.github.io/palmerpenguins/)。该数据集包含三种独特的企鹅种类—— Chinstrap、Gentoo、Adelie：

![](../Images/ab73693b37cdb30493b10c814d498777.png)

Artwork by [@allison_horst](https://allisonhorst.github.io/palmerpenguins/articles/art.html)

## 以下是如何加载数据

```py
# Load libraries
library(palmerpenguins)
library(tidyverse)

# Load in data
data(package = 'palmerpenguins')
# Write penguins to a `df` variable.
# I'm doing this simply because it's easier to type `df` than `penguins` each time.
df <- penguins
```

## 1\. 首先创建一个可视化

让我们为 Adelie 物种创建一个柱状图，以查看它们每年的中位体重。

```py
# Create a summary table to calculate the median body mass by species and year
summary <- df %>% 
  group_by(species, year) %>%
  summarise(median_body_mass = median(body_mass_g, na.rm =T))

# Create a Plotly bar chart for the median bass of the Adelie penguins
plot_ly(
  data= {summary %>% filter(species == "Adelie")},
  x = ~year,
  y = ~median_body_mass,
  color = ~year,
  type = "bar",
  showlegend = FALSE) %>%
  layout(
    yaxis = list(title = 'Median Body Mass (g)'),
    xaxis = list(title = 'Year',tickvals = list(2007, 2008, 2009)),
    title = "Median Body Mass for Adelie Penguins") %>%
  hide_colorbar() %>%
  suppressWarnings()
```

![](../Images/be94cb0811c8e75e96430be2d33d5646.png)

一个条形图显示了Adelie企鹅在2007年、2008年和2009年的中位体重。

## 2\. 理解你想使用哪个变量来创建多个图表

*即：你的`facet_wrap`变量是什么？*

这是我们`summary`表格的视图。我们希望为每个物种创建相同的条形图。在这个例子中，我们感兴趣的变量是`species`变量。

![](../Images/2d0722fb6735d2e9cc53b58f415bed6f.png)

我们的总结表格的视图，展示了每种企鹅物种——Adelie、Chinstrap和Gentoo——在2007年、2008年和2009年的中位体重。

## 3\. 将绘图代码转换为函数

确定绘图代码中需要概括的组件。现在，我们将用一个概括的变量替换任何`Adelie`物种名称的实例：

![](../Images/6f07fe4415ff8e1689c36a3362720ef3.png)

描述我们的Plotly代码，展示了我们希望概括的变量。在这个例子中，我们希望用一个概括的变量替换任何“Adelie”物种名称的实例，以便为每个新物种创建图表。

将绘图代码转换为函数。这个函数接收一个变量`species_name`，它将以字符串形式输入。请看这里，我们用变量`species_name`代替了名称`Adelie`：

```py
plot_fx <- function(species_name){
  plot_ly(
    data= {summary %>% filter(species == species_name)},
    x = ~year,
    y = ~median_body_mass,
    color = ~year,
    type = "bar",
    showlegend = FALSE) %>%
    layout(
      yaxis = list(title = 'Median Body Mass (g)'),
      xaxis = list(title = 'Year',tickvals = list(2007, 2008, 2009)),
      title = paste("Median Body Mass for", species_name, "Penguins")) %>%
    hide_colorbar() %>%
    suppressWarnings()
  }
```

这是如何运行函数以生成新图表的示例。让我们为物种`Chinstrap`制作相同的条形图：

```py
# Run function for species name "Chinstrap"
plot_fx("Chinstrap")
```

![](../Images/a77fbd98eab37699a6d2126f4892d8f8.png)

一个条形图显示了Chinstrap企鹅在2007年、2008年和2009年的中位体重。这是通过我们在帖子中创建的自定义函数生成的。

## 4\. 循环遍历你的唯一值以生成多个图表

从这里开始，你需要一个包含所有唯一物种的列表，以便循环遍历你的函数。我们通过`unique(summary$species)`来获取这个列表。

从创建一个空列表开始，用于存储所有你的图表

```py
# Create an empty list for all your plots
plot_list = list() 
```

循环遍历唯一的物种变量，为每个物种生成一个图表。然后，将其添加到`plot_list`中。

```py
# Run the plotting function for all the species
for (i in unique(summary$species)){
    plot_list[[i]] = plot_fx(i)
}

# Now you have a list of three plots - one for each species. 
# You can see the plots by changing the value within the square brackes from 1 to 3
plot_list[[1]]
```

现在，使用Plotly中的`subplot`函数将所有图表可视化到一个网格中：

```py
# Plot all three visuals in one grid
subplot(plot_list, nrows = 3, shareX = TRUE, shareY = FALSE) 
```

![](../Images/8fb15638ec571d9b222deeb7b7724304.png)

三个条形图显示了Adelie、Chinstrap和Gentoo企鹅在2007年、2008年和2009年的中位体重。这是通过循环遍历数据集中每个唯一物种，并使用我们的自定义绘图函数生成的。

## 我们做到了！

我知道这比使用`facet_wrap`函数在`ggplot2`中要多做很多工作，但理解如何创建函数有助于自动化报告和创建更动态的仪表板和视觉效果！

## 奖励步骤！添加注释以获取每个图表的标题

要在最后的视觉效果中为每个子图添加标题，你必须使用[Plotly中的注释](https://plotly.com/r/text-and-annotations/)。

```py
# Create a list of annotations
# The x value is where it lies on the entire subplot grid
# The y value is where it lies on the entire subplot grid 

my_annotations = list(
  list(
    x = 0.1, 
    y = 0.978, 
    font = list(size = 16), 
    text = unique(summary$species)[[1]], 
    xref = "paper", 
    yref = "paper", 
    xanchor = "center", 
    yanchor = "bottom", 
    showarrow = FALSE
  ), 
  list(
    x = 0.1, 
    y = 0.615, 
    font = list(size = 16), 
    text = unique(summary$species)[[2]], 
    xref = "paper", 
    yref = "paper", 
    xanchor = "center", 
    yanchor = "bottom", 
    showarrow = FALSE
  ), 
  list(
    x = 0.1, 
    y = 0.285, 
    font = list(size = 16), 
    text = unique(summary$species)[[3]], 
    xref = "paper", 
    yref = "paper", 
    xanchor = "center", 
    yanchor = "bottom", 
    showarrow = FALSE
  ))
```

这有点像一个混乱的试错过程，因为你必须硬编码位置。以下是如何操作的详细说明：

1.  **为每个子图标题创建注释列表：** 注释将是一个列表的列表。每个元素是一个包含每个子图所有信息的列表。在我们的例子中，我希望每个子图显示物种名称，因此我将有一个包含 3 个元素的列表。每个元素包含以下内容：

![](../Images/4a63ee02469225780840568581b40823.png)

我们的注释代码的描述，展示了‘x’、‘y’和‘text’变量所对应的内容。

+   `x`：这是一个介于 0 和 1 之间的值，对应于整个图形的位置，其中 0 代表左端，1 代表右端。

+   `y`：这是一个介于 0 和 1 之间的值，对应于整个图形的位置，其中 0 代表底部，1 代表顶部。

+   `text:` 这是你希望显示的每个子图标题的文本。

+   `xref` 和 `yref`：你可以选择‘paper’，这意味着位置参考的是绘图区域左侧的标准化坐标距离，其中“0”（“1”）对应左侧（右侧）。或者，你可以选择‘domain’，这将对应于每个单独子图的域。

+   `xanchor`：设置文本框的水平位置锚点。这个锚点将 `x` 位置绑定到注释的“左边”、“中间”或“右边”。根据你的 x 和 y 坐标想象一下你的点的位置，以及你希望文本如何相对于该位置对齐。

![](../Images/6ec807a1709f8cad8959de33c0c0eff8.png)

Plotly 布局的 xanchor 对齐描述。

+   `yanchor`：设置文本框的垂直位置锚点。这个锚点将 `y` 位置绑定到注释的“顶部”、“中间”或“底部”。根据你的 x 和 y 坐标想象一下你的点的位置，以及你希望文本如何相对于该位置对齐。

![](../Images/eb8af2b52172494cf5d321c3ecd22e1c.png)

Plotly 布局的 yanchor 对齐描述。

+   `showarrow`：Plotly 可以使用 TRUE 或 FALSE 选项绘制指向注释位置的箭头。如果你想在散点图上标记特定点，这个功能非常有用。由于我们只是向每个子图添加文本标签，因此在这个例子中箭头是不必要的。

**2\. 将布局选项添加到你的子图代码中：** 你可以通过 `layout()` 函数添加布局选项。

```py
# Run the subpot line including a layout
subplot(plot_list, nrows = 3, shareX = TRUE, shareY = FALSE) %>%
  layout(annotations = my_annotations,
         title = "Median Body Mass for Palmer Penguins",
         xaxis = list(tickvals = list(2007, 2008, 2009)),
         xaxis2 = list(tickvals = list(2007, 2008, 2009)),
         xaxis3 = list(tickvals = list(2007, 2008, 2009)))
```

这里是一些你可以指定的选项：

+   `annotations`：你创建的注释列表，包括每个标签的文本和位置的所有信息。

+   `title`：这是整个网格的标题文本。

+   `xaxis`、`xaxis2`、`xaxis3`：在 Plotly 中，每个子图都有自己的 x 轴属性。`xaxis` 指代第一个子图。在这个例子中，就是为阿德利企鹅物种的那个。剩余的 x 轴可以通过编号进行引用。这里我指定了刻度值的标签，以便我们有标准化的年份。

## 结论

虽然这是一个简单的例子，但我希望这能帮助你通过使用自定义函数来开启更多改进数据科学工作流的可能性！你可以将我们在这里所采取的步骤推广到总体编写自定义函数的方法中：

+   从简化的示例开始

+   将你的变量替换为一个通用变量

+   将该函数应用到其余的数据上

一旦掌握了基础，你可以扩展这一点，通过自动报告、仪表板和交互式可视化来确保工作的可重复性。拥有这个基础也有助于你在两种语言——R 和 Python——中变得更加熟练，因为你可以将某一语言中的有效方法转化到另一种语言中。在 R 和 Python 越来越可互换的世界里，这提供了不局限于特定语言的可能性！

除非另有说明，否则所有图片均由作者提供。
