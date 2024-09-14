# 如何使用 Seaborn 和 Matplotlib 创建美丽的年龄分布图（包括动画）

> 原文：[`towardsdatascience.com/how-to-create-beautiful-age-distribution-graphs-with-seaborn-and-matplotlib-including-animation-68ebabff41bd`](https://towardsdatascience.com/how-to-create-beautiful-age-distribution-graphs-with-seaborn-and-matplotlib-including-animation-68ebabff41bd)

## 图形教程

## 可视化国家和地区的人口统计数据

[](https://medium.com/@oscarleo?source=post_page-----68ebabff41bd--------------------------------)![Oscar Leo](https://medium.com/@oscarleo?source=post_page-----68ebabff41bd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----68ebabff41bd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----68ebabff41bd--------------------------------) [Oscar Leo](https://medium.com/@oscarleo?source=post_page-----68ebabff41bd--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----68ebabff41bd--------------------------------) ·9 分钟阅读·2023 年 6 月 22 日

--

![](img/cfe243e016a2441148f9c3f665be353e.png)

作者创建的图形

今天，我想展示如何使用 `matplotlib` 和 `seaborn` 创建像上面那样美丽的年龄分布图。

年龄分布图对于可视化国家或地区的人口统计信息非常优秀。它们非常吸引人，但默认的 Seaborn + Matplotlib 图形对于我们来说效果不够好。

在本教程中你将学到：

+   如何创建 Seaborn 风格

+   改进坐标轴使其易读且信息丰富

+   添加标题和美丽的图例

+   将 Matplotlib 图形转换为 PIL 图像并添加外部填充

+   创建多个图像的网格（如上例所示）

+   创建时间推移动画以展示人口随时间的变化

如果你想跟着做，你可以在这个 [GitHub 仓库](https://github.com/oscarleoo/age-distribution-tutorial/tree/main)中找到数据和我的代码。

让我们开始吧。

## 数据的快速浏览

原始数据来自 [人口估计与预测](https://datacatalog.worldbank.org/search/dataset/0037655/Population-Estimates-and-Projections) 数据集，该数据集由世界银行提供，并遵循 Creative Commons Attribution 4.0 许可。它包含 1960 年至 2021 年的实际值和直到 2050 年的官方预测。

在 GitHub 仓库中，我处理了数据并创建了四个单独的 CSV 文件，以便你可以专注于制作图形。

两个文件，一个用于女性，一个用于男性，包含绝对人口数。

![](img/47a65976a7db990afd1c45924bef28e3.png)

作者提供的截图

其他两个值描述了总人口的比例。例如，在下面的截图中，你可以看到 1960 年巴林只有 0.336%的人在 70-74 岁之间。

![](img/1a9b78caed3a1085aee2269bde7453bd.png)

作者截图

数据集有 17 个年龄组，`00-04`、`05-09`、`10-14`、`15-19`、`20-24`、`25-29`、`30-34`、`35-39`、`40-44`、`45-49`、`50-54`、`55-59`、`60-64`、`65-69`、`70-74`、`75-79`和`80+`。

在 250 多个国家和地区中，你可以自由创建你感兴趣的年龄分布图。

## 创建第一个年龄分布图

现在我们理解了数据，我们可以使用 Seaborn 的默认设置创建一个简单的图表。我用红色表示女性，蓝色表示男性。

这也许有点刻板，但让图表易于理解是至关重要的，而颜色对于初步解释来说很重要。

唯一的“窍门”是我将男性的值乘以负数，这样蓝色条形图就会朝相反的方向。

这是创建图表的函数。

```py
def create_age_distribution(female_df, male_df, country, year):
    df_f = female_df[female_df.country_name == country].loc[::-1]
    df_m = male_df[male_df.country_name == country].loc[::-1]

    ax = sns.barplot(y=df_m["indicator_name"], x=df_m[year] * -1, orient="h", color=MALE_COLOR)
    ax = sns.barplot(y=df_f["indicator_name"], x=df_f[year], orient="h", color=FEMALE_COLOR)

    return ax
```

这是我如何使用它的。

```py
fig = plt.figure(figsize=(10, 7))

ax = create_age_distribution(
    female_df=population_female,
    male_df=population_male,
    country="World",
    year="2021"
)

plt.show()
```

这是 2021 年世界年龄分布图的结果。它显示了所有数据，但看起来不够好，且难以理解。

![](img/098ed96c7f43363c74dc3da54159ce09.png)

作者创建的图表

让我们改进一下。

## 创建 Seaborn 风格

`seaborn`最棒的地方在于你可以使用`sns.set_style()`轻松创建你独特的风格。它接受一个可以有多种不同值的字典。

在这个教程中，我创建了以下函数以便快速尝试不同的样式。

```py
def set_seaborn_style(font_family, background_color, grid_color, text_color):
    sns.set_style({
        "axes.facecolor": background_color,
        "figure.facecolor": background_color,

        "axes.labelcolor": text_color,

        "axes.edgecolor": grid_color,
        "axes.grid": True,
        "axes.axisbelow": True,

        "grid.color": grid_color,

        "font.family": font_family,
        "text.color": text_color,
        "xtick.color": text_color,
        "ytick.color": text_color,

        "xtick.bottom": False,
        "xtick.top": False,
        "ytick.left": False,
        "ytick.right": False,

        "axes.spines.left": False,
        "axes.spines.bottom": True,
        "axes.spines.right": False,
        "axes.spines.top": False,
    }
)
```

你可能希望有更多的控制。我在这里省略了一些不关心的选项，并在多个地方重用相同的颜色。

我们必须选择背景、网格和文本颜色来运行函数。我更喜欢有背景色的图表，因为它们在页面上更为突出。白色背景看起来不错，但不是我的风格。

创建新的颜色方案时，我通常从找到一个喜欢的颜色开始。一个好的起点是[Canva Color Palettes](https://www.canva.com/colors/color-palettes/)或[ColorHunt](https://colorhunt.co/)。

在我找到一些喜欢的颜色后，我通过[Coolors](https://coolors.co/)生成额外的颜色。

这是我在本教程中使用的主要颜色调色板。

![](img/7aeefa7380fca4671c5897b795d44137.png)

作者截图

现在我可以用新的颜色运行`set_seaborn_style()`，并且我选择了`PT Mono`作为字体。

```py
FEMALE_COLOR = "#F64740"
MALE_COLOR = "#05B2DC"

set_seaborn_style(
    font_family="PT Mono",
    background_color="#253D5B",
    grid_color="#355882",
    text_color="#EEEEEE"
)
```

这是目前图表的样子。

![](img/cbe7e0f257a0a9715c1985740b0ec732.png)

作者创建的图表

这比之前的图表有了明显的改进，但仍然缺乏信息，并且难以理解。

让我们继续修正坐标轴。

## 改进坐标轴

现在颜色看起来不错，是时候让图表更具信息性了。

这是我想做的三件事。

+   删除坐标轴标签，因为它们没有提供信息

+   格式化 x 轴上的值，使其更具信息性

+   让文本更大，以便在较小的屏幕上图表看起来更好

解决方案包括两个函数。

首先，`create_x_labels()` 函数处理第二个要点，并允许我根据国家的人口快速调整 x 轴，或者如果我想使用比例而不是绝对数字的话。

```py
def create_x_labels(ax, xformat):
    if xformat == "billions":
        return ["{}B".format(round(abs(x / 1e9))) for x in ax.get_xticks()[1:-1]]
    elif xformat == "millions":
        return ["{}M".format(round(abs(x / 1e6))) for x in ax.get_xticks()[1:-1]]
    elif xformat == "thousands":
        return ["{}K".format(round(abs(x / 1e3))) for x in ax.get_xticks()[1:-1]]
    elif xformat == "percentage":
        return ["{}%".format(round(abs(x), 1)) for x in ax.get_xticks()[1:-1]] 
```

其次，`format_ticks()` 函数负责处理第一和第三个要点，并调用 `create_x_labels()`。

```py
def format_ticks(ax, xformat, xlim=(None, None)):
    ax.tick_params(axis="x", labelsize=12, pad=8)
    ax.tick_params(axis="y", labelsize=12)
    ax.set(ylabel=None, xlabel=None, xlim=xlim)

    plt.xticks(
        ticks=ax.get_xticks()[1:-1],
        labels=create_x_labels(ax, xformat)
    )
```

如果我们想比较两个不同的年龄分布，`xlim` 参数是必需的。如果我们不设置它，坐标轴会根据数据中的值进行调整，条形图会扩展到整个坐标轴。

当我创建图表时，我添加了这些函数。它和之前完全一样，只是最后加了 `format_tricks()`。

```py
fig = plt.figure(figsize=(10, 7))

ax = create_age_distribution(
    female_df=population_female,
    male_df=population_male,
    country="World",
    year="2021"
)

# New functions
format_ticks(ax, xformat="millions")

plt.show()
```

这就是新图表的样子。

![](img/b81948f25feb244718f9fa468d8b70c5.png)

作者创建的图表

我们还可以通过设置 `xformat="percentage"` 并使用 `population_ratio_male` 和 `population_ratio_female` 来测试百分比格式。我还设置了 `xlim=(-10, 10)`。

![](img/1e93603e077dae5531ef57dae7e7f844.png)

作者创建的图表

看起来很好，但我们还可以做得更好。

## 添加标题和图例

我现在想解决的两个明显改进是：

+   添加一个描述图表的标题

+   添加一个解释条形图表示内容的图例

为了创建图例，我编写了以下函数，接受 x 和 y 参数来定义位置。

```py
def add_legend(x, y): 
    patches = [
        Patch(color=MALE_COLOR, label="Male"),
        Patch(color=FEMALE_COLOR, label="Female")
    ]

    leg = plt.legend(
        handles=patches,
        bbox_to_anchor=(x, y), loc='center',
        ncol=2, fontsize=15,
        handlelength=1, handleheight=0.4,
        edgecolor=background_color
    )
```

然后，我添加了这个函数，就像在之前的步骤中添加 `format_tricks()` 一样。

```py
fig = plt.figure(figsize=(10, 8))

ax = create_age_distribution(
    female_df=population_female,
    male_df=population_male,
    country="World",
    year="2021"
)

# New functions
format_ticks(ax, xformat="millions")
add_legend(x=0.5, y=1.09)
plt.title("Age Distribution for the World in 2021", y=1.14, fontsize=20)

plt.tight_layout()
plt.show()
```

我还添加了 `plt.title()` 来添加标题。

当我运行所有内容时，年龄分布图看起来是这样的。

![](img/deba227d634ef641766dc8d0d978e069.png)

作者创建的图表

看起来很棒。我们继续。

## 创建 PIL 图像并添加填充

在某些时候，我想将我的图形转换为可以保存到磁盘并以其他方式自定义的图像。

一种这样的定制是给图表添加一些填充，使其看起来不那么拥挤。

首先，我创建了 `create_image_from_figure()` 函数，将 Matplotlib 图形转换为 PIL 图像。

```py
def create_image_from_figure(fig):
    plt.tight_layout()

    fig.canvas.draw()
    data = np.frombuffer(fig.canvas.tostring_rgb(), dtype=np.uint8)
    data = data.reshape((fig.canvas.get_width_height()[::-1]) + (3,))
    plt.close() 

    return Image.fromarray(data)
```

这是一个添加填充的函数。

```py
def add_padding_to_chart(chart, left, top, right, bottom, background):
    size = chart.size
    image = Image.new("RGB", (size[0] + left + right, size[1] + top + bottom), background)
    image.paste(chart, (left, top))
    return image
```

再次，我将这些函数添加到创建图表的原始代码中。现在它看起来是这样的。

```py
fig = plt.figure(figsize=(10, 8))

ax = create_age_distribution(
    female_df=population_female,
    male_df=population_male,
    country="World",
    year="2021"
)

# New functions
format_ticks(ax, xformat="millions")
add_legend(x=0.5, y=1.09)
plt.title("Age Distribution for the World in 2021", y=1.14, fontsize=20)

image = create_image_from_figure(fig)
image = add_padding_to_chart(image, 20, 20, 20, 5, background_color)
```

这是生成的图表。

![](img/c78feac4cf3501dd48b072c0e5061f11.png)

作者创建的图表

在我看来，这已经接近完美了。我还有两件事要展示，如何创建网格和时间推移可视化。

让我们从第一个开始。

## 创建具有多个国家的网格

你可以使用 `plt.subplots()` 来创建网格，但在这个教程中，我想创建一个图像网格，因为我认为这样看起来更好。

以下函数接受一个图像列表，并创建一个具有 `ncols` 的网格。它通过创建一个足够大的单一背景色的空图像来工作，以适应所有图形。

```py
def create_grid(figures, pad, ncols):
    nrows = int(len(figures) / ncols)
    size = figures[0].size

    image = Image.new(
        "RGBA",
        (ncols * size[0] + (ncols - 1) * pad, nrows * size[1] + (nrows - 1) * pad),
        "#ffffff00"
    )

    for i, figure in enumerate(figures):
        col, row = i % ncols, i // ncols
        image.paste(figure, (col * (size[0] + pad), row * (size[1] + pad)))

    return image
```

在下面的代码中，我遍历了一个国家列表，将生成的图表添加到`figures`中，并在最后通过运行`create_grid()`来创建网格。

```py
figures = []

for country in [
    "United States", "China", "Japan", "Brazil", "Canada",
    "Germany", "Pakistan", "Russian Federation", "Nigeria", 
    "Sweden", "Cambodia", "Saudi Arabia", "Iceland",
    "Spain", "South Africa", "Morocco"
]:

    fig = plt.figure(figsize=(10, 8))

    ax = create_age_distribution(
        female_df=population_ratio_female,
        male_df=population_ratio_male,
        country=country,
        year="2021"
    )

    ax.set(xlim=(-10, 10))

    # New functions
    format_ticks(ax, xformat="percentage")
    add_legend(x=0.5, y=1.09)
    plt.title("Age Distribution for {} in 2021".format(country), y=1.14, fontsize=20)

    image = create_image_from_figure(fig)
    image = add_padding_to_chart(image, 20, 20, 20, 5, background_color)

    figures.append(image)

grid = create_grid(figures, pad=20, ncols=4)
```

注意，我使用了比例而不是绝对数字，并设置了`xlim=(-10, 10)`。否则，我将无法在视觉上比较各国之间的差异。

![](img/cfe243e016a2441148f9c3f665be353e.png)

作者创建的图表

让我们继续教程的最后部分——如何创建时间推移可视化。

## 创建时间推移可视化

静态的年龄分布图看起来很棒，但看到它们随时间变化的过程非常有趣。

由于我们有 1960 年至 2021 年的实际值以及预测到 2050 年的数据，我们可以创建一个相对较长时间段的时间推移动画。

在开始之前，我需要告诉你，我使用的字体`PT Mono`并没有所有字符的高度相同。为了让可视化效果良好，我需要使用`plt.text()`来标注年份，而不是`plt.title()`。如果你使用其他字体，就不必这样做。

这是代码：

```py
images = []
years = list(population_male.columns[4:])

for year in years:
    fig = plt.figure(figsize=(10, 8))

    ax = create_age_distribution(
        female_df=population_female,
        male_df=population_male,
        country="World",
        year=year
    )

    # New functions
    format_ticks(ax, xformat="millions", xlim=(-400000000, 400000000))
    add_legend(x=0.5, y=1.09)
    plt.title("Age Distribution for the World in      ", y=1.14, fontsize=21)
    plt.text(x=0.77, y=1.15, s=str(year), fontsize=21, transform=ax.transAxes)

    image = create_image_from_figure(fig)
    image = add_padding_to_chart(image, 20, 20, 20, 5, background_color)
    images.append(image)
```

我使用`imageio`从图片列表中创建 GIF。

```py
# Duplicating the last frames to add a delay 
# before the animation restarts
images = images + [images[-1] for _ in range(20)]
imageio.mimwrite('./time-lapse.gif', images, duration=0.2)
```

让我们来看一下结果。

![](img/9614204ea543bcc2b1d9ae3708d6df26.png)

作者创建的可视化图

太棒了！这就是本教程的全部内容；告诉我你是否喜欢它并学到了一些有用的东西。

## 结论

这是一个有趣的教程，希望你喜欢。

年龄分布是展示一个国家人口结构的绝佳可视化方式，现在你已经见过几种使其突出的方法。

我们已经学会了创建样式、网格和动画。像我这里所做的编写函数也很棒，如果你想快速测试不同的想法和样式。

我希望你学到了一些将来会用到的东西。

感谢你抽时间阅读我的教程。如果你喜欢这种类型的内容，请告诉我。

如果有人需要，我可以创建更多教程！ :)

下次见。
