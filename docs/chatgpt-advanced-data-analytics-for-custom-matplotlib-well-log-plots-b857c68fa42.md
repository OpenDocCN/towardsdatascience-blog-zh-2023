# ChatGPT 高级数据分析用于自定义 Matplotlib 井眼日志图表

> 原文：[`towardsdatascience.com/chatgpt-advanced-data-analytics-for-custom-matplotlib-well-log-plots-b857c68fa42`](https://towardsdatascience.com/chatgpt-advanced-data-analytics-for-custom-matplotlib-well-log-plots-b857c68fa42)

## 使用 OpenAI 的代码解释器创建用于岩石物理和地球科学解释的井眼日志图表

[](https://andymcdonaldgeo.medium.com/?source=post_page-----b857c68fa42--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----b857c68fa42--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b857c68fa42--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b857c68fa42--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----b857c68fa42--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b857c68fa42--------------------------------) ·15 分钟阅读·2023 年 9 月 18 日

--

![](img/825c2744d109da5e1fa631b5999278d3.png)

由[D koi](https://unsplash.com/@dkoi?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

ChatGPT 的代码解释器，现在更名为[**高级数据分析**](https://www.pluralsight.com/resources/blog/data/ChatGPT-Advanced-Data-Analytics)，已经推出了一段时间。它于 2023 年 7 月 6 日发布，是一个由 OpenAI 开发的插件，允许用户上传数据并进行分析。这包括数据清理、创建可视化和数据总结等。

与其依赖你编写 Python 代码来分析数据，你可以通过用简单的英语告诉 ChatGPT 你想做什么。然后，它将为你进行分析。

正如我的许多常规读者所知道的，我是[**matplotlib**](https://matplotlib.org/)的大粉丝。尽管这个库看起来笨拙且耗时，但只需一点努力，就能用它创建令人惊叹的可视化图表。

在玩弄这个新工具之后，我觉得是时候看看如何使用[**ChatGPT**](https://openai.com/chatgpt)和[**高级数据分析**](https://openai.com/blog/chatgpt-plugins)插件来创建自定义的井眼日志数据图表了。

在继续之前，鉴于针对 OpenAI 的法律案件不断增加：

> **始终对上传到 ChatGPT 的数据保持谨慎，因为这些数据和你的输入可能会被用于训练未来的模型。如果有疑问，避免上传任何数据，并始终遵循公司政策。**

# 启用 ChatGPT 中的高级数据分析

要在 ChatGPT 中使用高级数据分析插件，首先需要启用它。

这可以通过进入设置并选择 Beta 功能来完成。在这里，你将看到启用**高级数据分析**的选项，这将启用该插件。

![](img/2e10f058702cfa9fc8448ddb22ec8fe2.png)

在 ChatGPT 中启用高级数据分析插件。图片由作者提供。

插件现在将在你开始新的聊天时可用。

# 将数据上传并转换为 Pandas 数据框

首先，我们需要上传我们的文件。对于这个例子，我使用了来自[**NLOG 数据库**](https://www.nlog.nl/datacenter/)的测井数据集（文章末尾有详细信息）。这个数据集包含从一个油气勘探井获得的一系列测井数据。

首先，我们点击聊天输入框上的加号图标，然后选择包含我们数据的文件。

![](img/0cd2a70d899e6c6f0ecc01d0691df742.png)

高级数据分析插件的聊天输入框包括一个用于上传数据的加号 (+) 图标。图片由作者提供。

接下来，我们需要给 ChatGPT 提供一个提示。在这种情况下，我将告诉它扮演一个[**岩石物理学家**](https://en.wikipedia.org/wiki/Petrophysics)（地球科学中的一个冷门角色），并且它理解测井数据。这可以帮助微调 ChatGPT 返回的一些响应。

![](img/71ca98f7192c1ad7306477d388a2fc8b.png)

初始提示给 ChatGPT，包含测井数据集。图片由作者提供。

提交上述输入后，ChatGPT 将加载文件并查看内容。

以下是我们从 ChatGPT 的高级数据分析插件获得的响应，它提供了有关数据集中每个测量项的信息。

在这种情况下，它使用 pandas 将 CSV 文件读取到数据框中，然后使用常见的`df.head()`命令输出表头。

![](img/520ee0f4377fe45465fc7b64da8ad73f.png)

ChatGPT 分析的测井数据集的初步分析和总结。图片由作者提供。

关于文件中曲线的评论大多是正确的。然而，也有一些不一致之处，比如 DT 被称为 Delta-Time。这部分正确；然而，这条曲线表示的是声波压缩速度，是通过声学工具发射的声波在地层中传播的压缩到达时间的测量。

**在我使用 ChatGPT 的经验中，我发现它在处理诸如岩石物理学等冷门主题时可能会遇到挑战，并可能返回不正确的信息。这是每个人在使用大型语言模型（LLMs）时都应该注意的事情。**

始终仔细检查输出，确保其合理。

我之前在下面的文章中讨论过这个话题：

[](/openais-amazing-chatgpt-is-it-promising-for-niche-topics-fcee2328dbd8?source=post_page-----b857c68fa42--------------------------------) ## OpenAI 的惊人 ChatGPT：它对冷门话题有希望吗？

### 向 ChatGPT 询问岩石物理学

[towardsdatascience.com

点击“显示工作”下拉框，我们可以检查用于加载数据的代码。我们可以看到它进行了基本的 Python 编程，将我们的 CSV 文件读取到一个[**pandas 数据框**](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)中。

```py
import pandas as pd

# Load the data from the provided CSV file
well_data = pd.read_csv("/mnt/data/L0509WellData.csv")

# Display the first few rows of the data
well_data.head()
```

ChatGPT 还识别出一些曲线中的占位符值为-999。这些值代表缺失的数据。然而，正如我们将看到的，这可能会引起一些混淆和问题。

# 使用代码解释器进行数据探索

在处理任何数据集时，最耗时的部分是理解你拥有的数据，进行数据质量检查以及清理数据。

这是我个人认为 ChatGPT 的高级数据分析插件对岩石物理学家和数据科学家最有帮助的地方。然而，像往常一样，你必须检查这些 AI 系统产生的结果和工作，因为它们可能无意中犯了一些错误。

在数据清理步骤的实验过程中，ChatGPT 在将-999 值转换为 NaNs 时遇到了困难。在进行此操作时，它还将列的数据类型转换为字符串。

结果，我必须在请求中具体说明，以确保它保持列为数值型。

![](img/c49c3f5ae27cb7839ab916b7eb8dc9a4.png)

更具体的提示以让 ChatGPT 高级数据分析插件将-999 值转换为 NaNs。图片由作者提供。

它最终返回了以下响应：

![](img/091d33759c969e3c2d73417584107a14.png)

ChatGPT 对将-999 值替换为 NaNs 的响应。图片由作者提供。

以及以下代码

```py
# Replace -999 values with NaN
well_data.replace(-999, float("nan"), inplace=True)

# Display the first few rows of the updated data
well_data.head()
```

这生成了以下输出：

```py
RESULT
       DEPTH         GR         DT      RHOB      DRHO      NPHI
0  4609.8008  89.059479       <NA>  2.483700  0.018494  0.129119
1  4609.9008  90.721848       <NA>  2.489308  0.013656  0.108034
2  4610.0008  90.709061       <NA>  2.501088  0.011289  0.085650
3  4610.1008  89.568954  66.223099  2.515450  0.008615  0.070332
4  4610.2008  88.169571  66.705551  2.530982  0.005628  0.065343
```

这很有趣且有点奇怪，因为响应试图将字符串`nan`转换为浮点数。我本来会在数据框的替换函数中使用`np.nan`来用 NaNs 替换-999 值。然而，目前这似乎有效，但以后会导致一些问题。

## 生成描述性总结

接下来，我们来看看是否可以使用以下简单提示获取数据集中每个测量值的描述性总结表。

![](img/ab3ef5a70210cc2ea51d32c9bc1c09d5.png)

ChatGPT 提示生成一个关于测井数据集的简单描述性总结。图片由作者提供。

ChatGPT 在其响应中返回了以下表格。

![](img/01604a8d16e5c51fca94e9e9aeb35c87.png)

ChatGPT 总结表提供了测井数据的信息和统计数据。图片由作者提供。

初看起来，这可能和`df.describe()`函数的输出相同；然而，ChatGPT 还为每个测量值添加了单位。如果我们想把这些内容放入报告中，这很方便。尽管将单位放在左侧的行头而不是每个单元格中会更好。

此外，它将孔隙度单位从小数转换为百分比。这在报告或将信息传递给同事时可能会造成误导。我希望它能将数据保持在原始单位。

最终，由于存在空值，它未能为 DT 曲线创建统计数据——这与之前 ChatGPT 未能将-999 值转换为空值的部分相关。然而，如果我们使用`df.describe()`方法，并且数据是数值型的，我们仍然可以看到该曲线的统计数据。

对于不了解情况的数据分析师，这可能会被忽视而未被发现。

# 使用 Matplotlib 和 ChatGPT 高级数据分析插件创建测井图

[**当我第一次开始在 Medium 上写文章时**](https://medium.com/@andymcdonaldgeo/loading-and-displaying-well-log-data-b9568efd1d8)，我专注于如何使用 matplotlib 创建基本的测井图以及如何使用 Python 处理测井数据。由于我对 Python 相对陌生且在编写代码时遇到困难，这个过程花费了很多时间。

本质上，测井图由子图组成，这些子图通常称为轨迹。在每个轨迹中，不同的科学测量结果被绘制出来，这些测量可以结合使用以制定地下解释。每个测量结果都以深度为 y 轴进行绘图。

作为对 ChatGPT 高级数据分析插件的快速测试，我想看看它是否可以通过[我之前的文章](https://medium.com/@andymcdonaldgeo/loading-and-displaying-well-log-data-b9568efd1d8)中的几个简单提示来帮助重新创建测井图。

![](img/2cd1d9d803286dd8c7ab6305166df7f7.png)

使用 matplotlib 生成的示例测井图。[作者提供的图片。](https://medium.com/@andymcdonaldgeo/loading-and-displaying-well-log-data-b9568efd1d8)

第一步是让它创建一个基本的测井图，其中每个测量结果都显示在各自的子图中。

![](img/f05ad676a9375b63293e66a160ca1ec0.png)

生成测井图的初始提示。作者提供的图片。

在第一次处理和编写代码后，ChatGPT 再次遇到了数据集中的 NaN 值问题。因此，它不得不重新创建图表。

![](img/eb7ce2fb92755c7b083335f54c39f09f.png)

使用 ChatGPT 的高级数据分析插件生成的基本测井图。作者提供的图片。

一旦图表生成，它将返回一个相对不错的首次测井数据图。

有趣的是，它对图表还做了一些调整，包括：

+   反转 y 轴，以便从浅层深度到深层深度

+   对一些测量应用了标准刻度，例如 GR 的刻度范围为 0 到 150

ChatGPT 生成的代码如下：

```py
# Setting up the figure and axes again
fig, ax = plt.subplots(nrows=1, ncols=6, figsize=(15, 10), sharey=True)

# Plotting DEPTH on the first track
ax[0].plot(well_data['DEPTH'], well_data['DEPTH'], color='black')
ax[0].set_title('DEPTH')
ax[0].set_ylabel('Depth (m)')
ax[0].invert_yaxis()  # Invert the y-axis to have depth increasing downwards
ax[0].grid()

# Plotting GR on the second track
ax[1].plot(well_data['GR'], well_data['DEPTH'], color='green')
ax[1].set_title('GR (Gamma Ray)')
ax[1].set_xlim(0, 150)  # Typical scale for GR
ax[1].grid()

# Plotting DT on the third track (excluding NaN values)
ax[2].plot(well_data['DT'].dropna(), well_data['DEPTH'].loc[well_data['DT'].dropna().index], color='blue')
ax[2].set_title('DT (Delta Time)')
ax[2].grid()

# Plotting RHOB on the fourth track
ax[3].plot(well_data['RHOB'], well_data['DEPTH'], color='red')
ax[3].set_title('RHOB (Bulk Density)')
ax[3].set_xlim(1.5, 3.0)  # Typical scale for RHOB
ax[3].grid()

# Plotting DRHO on the fifth track
ax[4].plot(well_data['DRHO'], well_data['DEPTH'], color='cyan')
ax[4].set_title('DRHO')
ax[4].grid()

# Plotting NPHI on the sixth track
ax[5].plot(well_data['NPHI'], well_data['DEPTH'], color='magenta')
ax[5].set_title('NPHI (Neutron Porosity)')
ax[5].set_xlim(-0.15, 0.45)  # Typical scale for NPHI
ax[5].grid()

plt.tight_layout()
plt.show()
```

这段代码与 [**我早期文章中的代码**](https://medium.com/@andymcdonaldgeo/loading-and-displaying-well-log-data-b9568efd1d8) 非常相似，但不是使用 subplot2grid，而是访问单独的轴并添加图表。

我们现在可以对这个图进行修改，一次进行多个更改。

在使用 ChatGPT 时，我发现将提示分解为单独的指令有助于获得所需的输出。然而，为了观察 ChatGPT 的代码解释器如何处理多个命令，我们可以使用以下提示来进行更改：

+   移除包含深度曲线的子图

+   向每个子图添加网格线

+   为每个子图添加右侧和顶部的脊线

+   为每条曲线添加标准颜色

+   确保所有曲线都有定义的 x 轴刻度范围

![](img/fde5dfad30c53be1d9241f77db9e6564.png)

在 ChatGPT 提示中进行多个请求以改善井日志图。图像由作者提供。

在进一步处理后，ChatGPT 返回了包含我请求的更改的图。

![](img/44be3bd84bb967e33755fe46e4f58692.png)

根据建议的更改后的基本井日志图。图像由作者提供。

它还将代码从对每个轴的单独调用重构为使用循环遍历每个测量值，并使用一些公共函数（如脊线和网格颜色）来应用更改。

```py
# Setting up the figure and axes with one less subplot for depth
fig, ax = plt.subplots(nrows=1, ncols=5, figsize=(15, 10), sharey=True)

# Define a dictionary for colors and scales for each log
colors = {
    "GR": "green",
    "DT": "magenta",
    "RHOB": "red",
    "DRHO": "grey",
    "NPHI": "blue"
}
scales = {
    "GR": (0, 150),
    "DT": (140, 40),
    "RHOB": (1.95, 2.95),
    "DRHO": (-0.2, 0.2),
    "NPHI": (0.45, -0.15)
}

# Plotting each log with the specified colors and scales
logs = ['GR', 'DT', 'RHOB', 'DRHO', 'NPHI']
for i, log in enumerate(logs):
    ax[i].plot(well_data[log], well_data['DEPTH'], color=colors[log])
    ax[i].set_xlim(scales[log])
    ax[i].set_title(log)
    ax[i].grid(color="lightgrey")
    # Adding top and right spines
    ax[i].spines['top'].set_visible(True)
    ax[i].spines['right'].set_visible(True)

ax[0].set_ylabel('Depth (m)')
ax[0].invert_yaxis()  # Invert the y-axis to have depth increasing downwards

plt.tight_layout()
plt.show()
```

# 在 Y 轴和一条线之间添加变量填充

在日志图中，通常会在曲线和子图边缘之间应用阴影和额外填充。这有助于岩石物理学家通过可视化来理解数据。

一种方法是为伽马射线（GR）应用变量阴影，以帮助识别页岩和非页岩区间。为此，我们通常使用黄色 — 橙色 — 棕色色图，这可以帮助将干净的储层区间显示为更亮的颜色，将更具页岩特征的区间显示为更暗的颜色。

我在下面的早期文章中详细介绍了这个过程。

[## 使用绘图填充增强井日志可视化](https://medium.com/@andymcdonaldgeo/loading-and-displaying-well-log-data-b9568efd1d8)

### 使用 matplotlib 对井日志数据应用颜色填充

towardsdatascience.com](/enhancing-visualization-of-well-logs-with-plot-fills-72d9dcd10c1b?source=post_page-----b857c68fa42--------------------------------)

我以前尝试过使用 ChatGPT 和代码解释器，但总是失败。也许这与我如何编写提示有关。

让我们看看 ChatGPT 是否可以通过在提示中更具体地创建一个 YlOrBr 颜色地图阴影来帮助 GR 曲线。

![](img/9b76fb527c117c0c7c2a3c879592beec.png)

ChatGPT 提示以创建 GR — 伽马射线曲线的变量阴影。图像由作者提供。

我们得到以下图：

![](img/86cac7bebc4032cef993f71d563091e7.png)

ChatGPT 数据分析插件生成 GR 曲线的变量阴影。图像由作者提供。

在第一次尝试中，它成功地在子图上填充了颜色。然而，它没有从子图的左侧到线条进行填充。

我们可以看看 ChatGPT 是否会纠正这一点：

![](img/d7240ea600dca64e8f961800072f4be7.png)

提示仅从子图的左侧到曲线应用填充。图片由作者提供。

并且它确实做到了，这非常令人惊讶，因为之前的尝试都失败了。

![](img/a5aa7c5bd7e00ac44ae3310efd95e4a7.png)

ChatGPT 为变量填充修正图表后的日志图。图片由作者提供。

我们可以仔细查看代码，了解它是如何生成图表的。

```py
# Setting up the figure and axes again
fig, ax = plt.subplots(nrows=1, ncols=5, figsize=(15, 10), sharey=True)

# Plotting each log with the specified colors and scales, excluding NaN values for DT
for i, log in enumerate(logs):
    valid_indices = well_data[log].dropna().index
    ax[i].plot(well_data[log].loc[valid_indices], well_data['DEPTH'].loc[valid_indices], color=colors[log])
    ax[i].set_xlim(scales[log])
    ax[i].set_title(log)
    ax[i].grid(color="lightgrey")
    # Adding top and right spines
    ax[i].spines['top'].set_visible(True)
    ax[i].spines['right'].set_visible(True)

    # Adding the fill for the GR subplot
    if log == 'GR':
        # Add color based on the value of the GR line
        for depth, value in zip(well_data['DEPTH'].loc[valid_indices], well_data[log].loc[valid_indices]):
            ax[i].fill_betweenx([depth-0.05, depth+0.05], 
                                0, value, 
                                facecolor=plt.cm.YlOrBr(value/150), 
                                edgecolor='none', 
                                zorder=0)  # Place the fill behind the line

ax[0].set_ylabel('Depth (m)')
ax[0].invert_yaxis()  # Invert the y-axis to have depth increasing downwards

plt.tight_layout()
plt.show()
```

ChatGPT 采用的方法不同于 **我之前的尝试**，那种方法是逐一遍历每个排序值并应用填充。而 ChatGPT 是逐层处理，然后应用填充。

```py
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np 

left_col_value = 0
right_col_value = 150

#assign the column to a variable for easier reading
curve = well_data['GR']

#calculate the span of values
span = abs(left_col_value - right_col_value)

#assign a color map
cmap = plt.get_cmap('YlOrBr')

#create array of values to divide up the area under curve
color_index = np.arange(left_col_value, right_col_value, span / 100)

#setup the plot
well_data.plot(x='GR', y='DEPTH', c='black', lw=0.5, legend=False, figsize=(6,15))
plt.ylim(4800, 4600)
plt.xlim(left_col_value, right_col_value)
plt.title('Plot With a Variable Fill to Y-Axis')

#loop through each value in the color_index
for index in sorted(color_index):
    index_value = (index - left_col_value)/span
    color = cmap(index_value) #obtain colour for color index value
    plt.fill_betweenx(well_data['DEPTH'], 0 , curve, where = curve >= index,  color = color)

plt.show()
```

上述代码生成了带有阴影的 GR 图。

![](img/ffef7bc79e7e5f60c539faecd7520a6f.png)

使用 GR 曲线进行颜色填充。 图片由作者提供。

# 将两条线移动到同一子图上

在日志图上，通常将体积密度 (RHOB) 和中子孔隙度 (NPHI) 绘制在同一轨道上。由于这两条曲线具有不同的刻度，我们需要将其中一条放置在次要 x 轴上。

在 matplotlib 中准确做到这一点可能很棘手，因此我们将看看 ChatGPT 如何处理以下提示。

![](img/889e1ecb6496831eff76301912378640.png)

提示请求将 NPHI 放置在与 RHOB 相同的子图上，并设置为次要轴。图片由作者提供。

返回的图表还不错。ChatGPT 成功地将 NPHI 曲线放置在与 RHOB 相同的子图上，并将其放置在次要 x 轴上。然而，子图的标签相互重叠，尚不清楚哪个刻度属于哪些曲线。

此外，我们现在在子图上有两组网格线，这可能会造成混淆。

![](img/9aca13b783ab07307550fa470e07cf42.png)

ChatGPT 在将 NPHI 移动到与 RHOB 相同的子图后的返回日志图。图片由作者提供。

我们可以通过简单的提示修改它，以确保标签不会重叠，并从其中一个曲线中移除网格线。

![](img/8426cffa222e6f7bf0dd0c026eeb4fa5.png)

调整子图标签和网格线的提示。

ChatGPT 完成了它被要求的任务，但它还为不同的标签添加了颜色。

我没有预期结果会这样做，然而，这确实使我们能够轻松地将标签与正确的线条链接起来。

![](img/4da617edcbe08f14316dd92c1beb47a3.png)

ChatGPT 数据分析插件在调整 NPHI 和 RHOB 曲线的标签后生成了一个井日志图。图片由作者提供。

因此，基于 ChatGPT 更改标签颜色的情况，我们可以以此为灵感，将其余标签更改为与线条颜色匹配。

如果我们在同一个子图上有多条线，这将对我们有所帮助，这在日志图表中非常常见。

![](img/cc998d38ac6611ee4c61a8d8ff011109.png)

调整所有标签和刻度标签以匹配其各自曲线颜色的提示。图片由作者提供。

ChatGPT 现在已经回应了我们的最终图表，我们现在可以轻松地看到 NPHI 和 RHOB 的刻度及其所属的线条。

![](img/06a8988b426109f549ba1162cf4a616f.png)

ChatGPT 和数据分析插件生成的最终井日志图表。图片由作者提供。

以下代码是 ChatGPT 提出的，用于生成上述图表。

```py
# Setting up the figure and axes again
fig, ax = plt.subplots(nrows=1, ncols=4, figsize=(15, 10), sharey=True)

# Plotting each log with the specified colors and scales, excluding NaN values
for i, log in enumerate(logs):
    valid_indices = well_data[log].dropna().index
    ax[i].plot(well_data[log].loc[valid_indices], well_data['DEPTH'].loc[valid_indices], color=colors[log])
    ax[i].set_xlim(scales[log])
    # Remove subplot title
    ax[i].set_title("")
    # Add axis label representing the name of the log measurement
    ax[i].set_xlabel(log, color=colors[log])
    # Color tick marks and tick labels in the same color as the line
    ax[i].tick_params(axis='x', colors=colors[log])
    # Adding top and right spines
    ax[i].spines['top'].set_visible(True)
    ax[i].spines['right'].set_visible(True)

    # Adding the fill for the GR subplot
    if log == 'GR':
        # Add color based on the value of the GR line
        for depth, value in zip(well_data['DEPTH'].loc[valid_indices], well_data[log].loc[valid_indices]):
            ax[i].fill_betweenx([depth-0.05, depth+0.05], 
                                0, value, 
                                facecolor=plt.cm.YlOrBr(value/150), 
                                edgecolor='none', 
                                zorder=0)  # Place the fill behind the line
        ax[i].grid(color="lightgrey")

    # Adding NPHI to the RHOB subplot with a secondary x-axis
    if log == 'RHOB':
        ax2 = ax[i].twiny()  # Create a secondary x-axis for NPHI
        valid_indices_nphi = well_data['NPHI'].dropna().index
        ax2.plot(well_data['NPHI'].loc[valid_indices_nphi], well_data['DEPTH'].loc[valid_indices_nphi], color=colors['NPHI'])
        ax2.set_xlim(scales['NPHI'])
        ax2.set_xlabel('NPHI', color=colors['NPHI'])
        ax2.tick_params(axis='x', colors=colors['NPHI'])
        # Remove gridlines for NPHI and display the ones for RHOB
        ax2.grid(False)
        ax[i].grid(color="lightgrey")

ax[0].set_ylabel('Depth (m)')
ax[0].invert_yaxis()  # Invert the y-axis to have depth increasing downwards

plt.tight_layout()
plt.show()
```

图表和代码看起来合理，我会做一些进一步的修改。然而，我认为此时继续使用 Jupyter Notebooks 修改图表是最好的选择。

这是因为我之前遇到的一些问题，如果我回去更改了早期的提示，它会删除该提示之后的任何内容并重新创建它。

如果我将 ChatGPT 窗口打开了几小时甚至几天，并且它超时，这尤其成问题。

# 总结

总的来说，我发现 ChatGPT 的数据分析插件（以前称为代码解释器）在生成岩石物理和地球科学的井日志图表方面是一个有用的工具。然而，我对使用它有几个保留意见和问题。

我发现很难在新的聊天实例中复制结果。我之前尝试过相同的过程，结果却完全不同，而且 ChatGPT 很难生成我想要的图表。这甚至发生在使用完全相同的提示时。

有时，数据分析插件生成的结果是值得怀疑的，甚至是错误的。与任何大型语言模型一样，审查输出并确保其在编程和技术上是合理的始终是明智的。

如果你在提示中出现任何错误，回去修改它们并不容易。如果你尝试更改其中一个提示，可能会导致该提示之后的任何聊天内容被删除。因此，我建议你在进行过程中将代码复制到 Jupyter Notebook 中，以免丢失信息。

**最终，我对使用 ChatGPT 和数据分析插件的最大保留意见** 是上传专有数据的难易程度（在这个例子中，我使用的是公开数据，允许使用）。数据、提示和输出都可能被用来训练未来的模型，而你可能并不知情。**版权和知识产权问题日益增加，在使用此工具和专有数据时建议极为谨慎。**

很高兴听到你对使用 ChatGPT 和数据分析插件的评论、想法和担忧。

*感谢阅读。在离开之前，你一定要订阅我的内容，以便将我的文章发送到你的邮箱中。* [***你可以在这里订阅！***](https://andymcdonaldgeo.medium.com/subscribe)*此外，如果你喜欢这些内容并希望表达你的感谢，请考虑给予一些点赞。*

# 本示例中使用的数据

本教程中使用的数据是从[**NLOG.nl**](https://www.nlog.nl/en/welcome-nlog)下载的，该网站包含了整个荷兰北海区域的井日志数据。这些数据可以免费下载和使用。有关数据许可的完整细节可以在[**这里**](https://www.nlog.nl/en/disclaimer)**找到。**
