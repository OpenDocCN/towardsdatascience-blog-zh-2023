# 实用的探索性数据分析改进技巧

> 原文：[https://towardsdatascience.com/practical-tips-for-improving-exploratory-data-analysis-1c43b3484577?source=collection_archive---------3-----------------------#2023-08-11](https://towardsdatascience.com/practical-tips-for-improving-exploratory-data-analysis-1c43b3484577?source=collection_archive---------3-----------------------#2023-08-11)

## 使EDA更简单（和更美观）的简短指南

[](https://radmilamandzhi.medium.com/?source=post_page-----1c43b3484577--------------------------------)[![Radmila M.](../Images/f3722a0ca0c96b5f6abb8f23a1162488.png)](https://radmilamandzhi.medium.com/?source=post_page-----1c43b3484577--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c43b3484577--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1c43b3484577--------------------------------) [Radmila M.](https://radmilamandzhi.medium.com/?source=post_page-----1c43b3484577--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1b144e8ba52a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-tips-for-improving-exploratory-data-analysis-1c43b3484577&user=Radmila+M.&userId=1b144e8ba52a&source=post_page-1b144e8ba52a----1c43b3484577---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c43b3484577--------------------------------) · 11 min read · 2023年8月11日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1c43b3484577&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-tips-for-improving-exploratory-data-analysis-1c43b3484577&source=-----1c43b3484577---------------------bookmark_footer-----------)![](../Images/301efde45bcd4111da5a4bac594d1621.png)

图片由 [Sam Dan Truong](https://unsplash.com/@sam_truong?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **介绍**

探索性数据分析（EDA）是使用任何机器学习模型之前的必经步骤。EDA过程需要数据分析师和数据科学家的专注和耐心：在从分析的数据中获得有意义的洞察之前，通常需要花费大量时间积极使用一个或多个可视化库。

在这篇文章中，我将根据个人经验分享一些如何简化EDA过程并提高效率的技巧。特别是，我将给出三条在对抗EDA过程中学到的重要建议：

1.  使用最适合你任务的非平凡图表；

1.  充分利用可视化库的功能；

1.  寻找更快的方法来完成相同的任务。

注：为了在这篇文章中创建信息图，我们将使用Kaggle上的**风电生成数据**[[2](https://www.kaggle.com/datasets/bhavikjikadara/wind-power-generated-data?resource=download)]。我们开始吧！

# 提示 1：不要害怕使用复杂的图表。

当我在处理与风能分析和预测相关的研究论文时，我学会了如何应用这个技巧[[1](https://www.sciencedirect.com/science/article/pii/S2772508122000382)]。在这个项目的EDA过程中，我遇到了需要创建一个总结矩阵的问题，这个矩阵能反映风参数之间的所有关系，以便找出它们之间最强的相互影响。第一个想到的想法是建立一个我在许多数据科学/数据分析项目中看到的‘老牌’**相关矩阵**。

如你所知，**相关矩阵**用于量化和总结变量之间的线性关系。在以下代码片段中，`corrcoef`函数用于**风电生成数据**的特征列。这里我还应用了Seaborn的`heatmap`函数，将相关矩阵数组绘制为热图：

```py
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import numpy as np

# read data
data = pd.read_csv('T1.csv')
print(data)

# rename columns to make their titles shorter
data.rename(columns={'LV ActivePower (kW)':'P',
                     'Wind Speed (m/s)':'Ws',
                     'Theoretical_Power_Curve (KWh)':'Power_curve',
                     'Wind Direction (°)': 'Wa'},inplace=True)
cols = ['P', 'Ws', 'Power_curve', 'Wa']

# build the matrix
correlation_matrix = np.corrcoef(data[cols].values.T)
hm = sns.heatmap(correlation_matrix,
                 cbar=True, annot=True, square=True, fmt='.3f',
                 annot_kws={'size': 15},
                 cmap='Blues',
                 yticklabels=['P', 'Ws', 'Power_curve', 'Wa'],
                 xticklabels=['P', 'Ws', 'Power_curve', 'Wa'])

# save the figure
plt.savefig('image.png', dpi=600, bbox_inches='tight')
plt.show()
```

![](../Images/171c52b7f16c0b6a7b50031b3a884b4c.png)

相关矩阵的示例。图像由作者提供。

> 分析结果图表后，可以得出风速和有功功率之间有强相关性，但我认为许多人会同意我的观点，即使用这种可视化方式解释结果并不容易，因为这里只展示了数字。

一个良好的替代相关矩阵的方案是**散点图矩阵**，它允许你在一个地方可视化数据集中不同特征之间的成对相关性。在这种情况下，应该使用` sns.pairplot`：

```py
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

# read data
data = pd.read_csv('T1.csv')
print(data)

# rename columns to make their titles shorter
data.rename(columns={'LV ActivePower (kW)':'P',
                     'Wind Speed (m/s)':'Ws',
                     'Theoretical_Power_Curve (KWh)':'Power_curve',
                     'Wind Direction (°)': 'Wa'},inplace=True)
cols = ['P', 'Ws', 'Power_curve', 'Wa']

# build the matrix
sns.pairplot(data[cols], height=2.5)
plt.tight_layout()

# save the figure
plt.savefig('image2.png', dpi=600, bbox_inches='tight')
plt.show()
```

![](../Images/8a473f6d19a81c2bb82d63ed4cd6f9cc.png)

散点图矩阵的示例。图像由作者提供。

> 通过查看散点图矩阵，可以快速观察数据的分布情况以及是否包含异常值。然而，这种图表的主要缺点与成对绘制数据造成的重复有关。

最后，我决定将上述图表合并为一张，其中左下部分包含所选参数的散点图，右上部分包含不同大小和颜色的气泡：较大的圆圈表示所研究的参数具有更强的线性相关性。矩阵的对角线将显示每个特征的分布：这里的窄峰值表明该参数变化不大，而其他特征则变化较大。

构建此总结矩阵的代码如下。这里的图包括三部分——`fig.map_lower`、`fig.map_diag`、`fig.map_upper`：

```py
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# read data
data = pd.read_csv('T1.csv')
print(data)

# rename columns to make their titles shorter
data.rename(columns={'LV ActivePower (kW)':'P',
                     'Wind Speed (m/s)':'Ws',
                     'Theoretical_Power_Curve (KWh)':'Power_curve',
                     'Wind Direction (°)': 'Wa'},inplace=True)
cols = ['P', 'Ws', 'Power_curve', 'Wa']

# buid the matrix
def correlation_dots(*args, **kwargs):
    corr_r = args[0].corr(args[1], 'pearson')
    ax = plt.gca()
    ax.set_axis_off()
    marker_size = abs(corr_r) * 3000
    ax.scatter([.5], [.5], marker_size,
               [corr_r], alpha=0.5,
               cmap = 'Blues',
               vmin = -1, vmax = 1,
               transform = ax.transAxes)
    font_size = abs(corr_r) * 40 + 5

sns.set(style = 'white', font_scale = 1.6)
fig = sns.PairGrid(data, aspect = 1.4, diag_sharey = False)
fig.map_lower(sns.regplot)
fig.map_diag(sns.histplot)
fig.map_upper(correlation_dots)

# save the figure
plt.savefig('image3.jpg', dpi = 600, bbox_inches = 'tight')
plt.show()
```

![](../Images/8661c8dff8cc697e4c813c66526e23f7.png)

总结矩阵的示例。图片由作者提供。

> 总结矩阵结合了之前研究的两种图表的优点——它的下半部分（左侧）模拟散点图矩阵，上半部分（右侧）图形化地反映了相关矩阵的数值结果。

# 提示2：充分利用可视化库的功能

我时常需要向同事和客户展示EDA的结果，因此可视化是我在这项任务中重要的助手。我总是尽量在图表中添加各种元素，如箭头和注释，以使其更具吸引力和可读性。

让我们回到前面讨论的风电项目的EDA实施案例。当谈到风能时，**功率曲线**是最重要的参数之一。风力涡轮机（或整个风电场）的功率曲线是一个图表，展示了在不同风速下生成的电力。需要注意的是，涡轮机在低风速下不会运行。它们的启动与切入速度相关，通常在2.5–5 m/s的范围内。风速在12到15 m/s之间时，达到额定功率。最后，每台涡轮机都有一个上限风速，当风速超过此限值时，涡轮机将停止发电，直到风速降回到操作范围内。

> 研究的数据集包括理论功率曲线（这是来自制造商的典型曲线，没有任何离群点）和实际曲线（如果我们绘制风力与风速的关系，后者通常包含许多超出理想理论形状的点，这可能是由于涡轮机故障、不正确的SCADA测量或计划外维护所致）。

现在我们将创建一张图像，展示两种类型的功率曲线——首先是没有任何额外项（除了图例）的：

```py
import pandas as pd
import matplotlib.pyplot as plt

# read data
data = pd.read_csv('T1.csv')
print(data)

# rename columns to make their titles shorter
data.rename(columns={'LV ActivePower (kW)':'P',
                     'Wind Speed (m/s)':'Ws',
                     'Theoretical_Power_Curve (KWh)':'Power_curve',
                     'Wind Direction (°)': 'Wa'},inplace=True)

# build the plot
plt.scatter(data['Ws'], data['P'], color='steelblue', marker='+', label='actual')
plt.scatter(data['Ws'], data['Power_curve'], color='black', label='theoretical')
plt.xlabel('Wind Speed')
plt.ylabel('Power')
plt.legend(loc='best')

# save the figure
plt.savefig('image4.png', dpi=600, bbox_inches='tight')
plt.show()
```

![](../Images/b209aaa6983664e060421cedcdfb1cf6.png)

一张‘静默’的风力功率曲线图。图片由作者提供。

> 如你所见，这张图需要解释，因为它没有包含任何额外的细节。

但如果我们添加线条来突出显示图表的三个主要区域——标记了切入、额定和切出速度的区域，以及一个箭头注释来展示一个离群点会怎样呢？

让我们看看在这种情况下图表会是什么样子：

```py
import pandas as pd
import matplotlib.pyplot as plt

# read data
data = pd.read_csv('T1.csv')
print(data)

# rename columns to make their titles shorter
data.rename(columns={'LV ActivePower (kW)':'P',
                     'Wind Speed (m/s)':'Ws',
                     'Theoretical_Power_Curve (KWh)':'Power_curve',
                     'Wind Direction (°)': 'Wa'},inplace=True)

# build the plot
plt.scatter(data['Ws'], data['P'], color='steelblue', marker='+', label='actual')
plt.scatter(data['Ws'], data['Power_curve'], color='black', label='theoretical')

# add vertical lines, text notes and arrow
plt.vlines(x=3.05, ymin=10, ymax=350, lw=3, color='black')
plt.text(1.1, 355, r"cut-in", fontsize=15)
plt.vlines(x=12.5, ymin=3000, ymax=3500, lw=3, color='black')
plt.text(13.5, 2850, r"nominal", fontsize=15)
plt.vlines(x=24.5, ymin=3080, ymax=3550, lw=3, color='black')
plt.text(21.5, 2900, r"cut-out", fontsize=15)
plt.annotate('outlier!', xy=(18.4,1805), xytext=(21.5,2050),
            arrowprops={'color':'red'})

plt.xlabel('Wind Speed')
plt.ylabel('Power')
plt.legend(loc='best')

# save the figure
plt.savefig('image4_2.png', dpi=600, bbox_inches='tight')
plt.show()
```

![](../Images/2728dfe9cb60fefe2cd8e0883336744e.png)

一张‘多话’的风力曲线图。图像由作者提供。

# 提示 3：总是找到更快的方法来完成相同的工作

在分析风数据时，我们通常希望获得有关风能潜力的全面信息。因此，除了风能的动态，还需要一个图表显示风速如何依赖于风向。

为了展示风力的变化，可以使用以下代码：

```py
import pandas as pd
import matplotlib.pyplot as plt

# read data
data = pd.read_csv('T1.csv')
print(data)

# rename columns to make their titles shorter
data.rename(columns={'LV ActivePower (kW)':'P',
                     'Wind Speed (m/s)':'Ws',
                     'Theoretical_Power_Curve (KWh)':'Power_curve',
                     'Wind Direction (°)': 'Wa'},inplace=True)

# resample 10-min data into hourly time measurements
data['Date/Time'] = pd.to_datetime(data['Date/Time'])
fig = plt.figure(figsize=(10,8))
group_data = (data.set_index('Date/Time')).resample('H')['P'].sum()

# plot wind power dynamics
group_data.plot(kind='line')
plt.ylabel('Power')
plt.xlabel('Date/Time')
plt.title('Power generation (resampled to 1 hour)')

# save the figure
plt.savefig('wind_power.png', dpi=600, bbox_inches='tight')
plt.show()
```

下面是结果图：

![](../Images/78795512af4c6e737bf93061ae6dafc8.png)

风力的动态。图像由作者提供。

> 如人们可能注意到的那样，风力动力学的轮廓具有相当复杂、不规则的形状。

**风玫瑰**，或称极坐标玫瑰图，是一种特殊的图表，用于表示气象数据的分布，通常是按方向划分的风速 [3]。`matplotlib`库中有一个简单的模块`windrose`，可以轻松构建这种可视化图表，例如：

```py
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from windrose import WindroseAxes

# read data
data = pd.read_csv('T1.csv')
print(data)

# rename columns to make their titles shorter
data.rename(columns={'LV ActivePower (kW)':'P',
                     'Wind Speed (m/s)':'Ws',
                     'Theoretical_Power_Curve (KWh)':'Power_curve',
                     'Wind Direction (°)': 'Wa'},inplace=True)
wd  = data['Wa']
ws = data['Ws']

# plot normalized wind rose in a form of a stacked histogram
ax = WindroseAxes.from_ax()
ax.bar(wd, ws, normed=True, opening=0.8, edgecolor='white')
ax.set_legend()

# save the figure
plt.savefig('windrose.png', dpi = 600, bbox_inches = 'tight')
plt.show()
```

![](../Images/95b1702803dd9953b27970616165dbdb.png)

基于现有数据获得的风玫瑰图。图像由作者提供。

> 看着风玫瑰图，可以注意到有两个主要的风向——东北和西南。

但是如何将这两张图片合并为一张呢？最明显的选择是使用`add_subplot`。尽管由于`windrose`库的特殊性，这不是一个简单的任务：

```py
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from windrose import WindroseAxes

# read data
data = pd.read_csv('T1.csv')
print(data)

# rename columns to make their titles shorter
data.rename(columns={'LV ActivePower (kW)':'P',
                     'Wind Speed (m/s)':'Ws',
                     'Theoretical_Power_Curve (KWh)':'Power_curve',
                     'Wind Direction (°)': 'Wa'},inplace=True)
data['Date/Time'] = pd.to_datetime(data['Date/Time'])

fig = plt.figure(figsize=(10,8))

# plot both plots as subplots
ax1 = fig.add_subplot(211)
group_data = (data.set_index('Date/Time')).resample('H')['P'].sum()
group_data.plot(kind='line')
ax1.set_ylabel('Power')
ax1.set_xlabel('Date/Time')
ax1.set_title('Power generation (resampled to 1 hour)')

ax2 = fig.add_subplot(212, projection='windrose')

wd  = data['Wa']
ws = data['Ws']

ax = WindroseAxes.from_ax()
ax2.bar(wd, ws, normed=True, opening=0.8, edgecolor='white')
ax2.set_legend()

# save the figure
plt.savefig('image5.png', dpi=600, bbox_inches='tight')
plt.show()
```

在这种情况下，结果如下所示：

![](../Images/ef2a04ae18c233be22e2122751e2282a.png)

一张风力动力学和风玫瑰图的合成图。图像由作者提供。

> 这里的主要缺点是两个子图的大小不同，因此风玫瑰图周围有很多空白区域。

为了简化操作，我建议采取不同的方法，使用`Python Imaging Library`(PIL) [[4](https://pillow.readthedocs.io/en/stable/index.html)]，仅需11 (!) 行代码：

```py
import numpy as np
import PIL
from PIL import Image

# list images that needs to be merged
list_im = ['wind_power.png','windrose.png']
imgs = [PIL.Image.open(i) for i in list_im]

# resize all images to match the smallest
min_shape = sorted([(np.sum(i.size), i.size) for i in imgs])[0][1]

# for a vertical stacking - we use vstack
images_comb = np.vstack((np.asarray(i.resize(min_shape)) for i in imgs))
images_comb = PIL.Image.fromarray(imgs_comb)

# save the figure
imgages_comb.save('image5_2.png', dpi=(600,600))
```

这里的输出看起来更美观，因为两张图片大小相同，代码选择了最小的一张，并将其他图片调整为匹配：

![](../Images/697dd68f13d6c37a28de2eb1f287697c.png)

一张使用PIL获得的风力动力学和风玫瑰图。图像由作者提供。

顺便提一下，在使用`PIL`时，也可以使用水平堆叠——例如，让我们比较和对比一个‘沉默’的和一个‘多话’的功率曲线图：

```py
import numpy as np
import PIL
from PIL import Image

list_im = ['image4.png','image4_2.png']
imgs = [PIL.Image.open(i) for i in list_im]

# pick the image which is the smallest, and resize the others to match it (can be arbitrary image shape here)
min_shape = sorted([(np.sum(i.size), i.size) for i in imgs])[0][1]

imgs_comb = np.hstack((np.asarray(i.resize(min_shape)) for i in imgs))
### save that beautiful picture
imgs_comb = PIL.Image.fromarray(imgs_comb)
imgs_comb.save('image4_merged.png', dpi=(600,600))
```

![](../Images/dcb796c254bade88676cf057bbc45cda.png)

比较和对比两个功率曲线图。图像由作者提供。

# 结论

在这篇文章中，我与您分享了三个使EDA过程更轻松的技巧。希望这些建议对您有所帮助，并且您也能将它们应用到自己的数据任务中。

这些提示完美地匹配了我在进行EDA时总是尝试应用的公式：**定制 → 列出 → 优化**。

好吧，你可能会问，这到底为什么重要？我可以说，实际上这很重要，因为：

+   **根据你当前面临的具体需求定制图表**是非常重要的。例如，不要创建大量的信息图表，想想如何将几个图表合并成一个，就像我们在创建总结矩阵时一样，这样可以结合散点图和相关图表的优点。

+   你所有的图表都应该能自我说明。因此，你需要知道**如何在图表上列出重要信息，使其详细且易于阅读**。比较一下“沉默”和“健谈”的功率曲线之间的差异。

+   最后，**每个数据专家都应该学习如何优化EDA过程以提高效率**（让生活更轻松）。如果你需要将两张图像合并为一张，并不一定要一直使用`add_subplot`选项。

还有什么呢？我可以肯定地说，EDA是处理数据时非常有创意和有趣的一步（更不用说它也非常重要）。

让你的信息图表像钻石一样闪耀，不要忘记享受这个过程！

# 参考列表

1.  论文《数据驱动的风能分析与预测应用：“La Haute Borne”风电场的案例》。 [https://doi.org/10.1016/j.dche.2022.100048](https://doi.org/10.1016/j.dche.2022.100048)

1.  风力发电数据：[https://www.kaggle.com/datasets/bhavikjikadara/wind-power-generated-data?resource=download](https://www.kaggle.com/datasets/bhavikjikadara/wind-power-generated-data?resource=download)

1.  关于windrose库的教程：[https://windrose.readthedocs.io/en/latest/index.html](https://windrose.readthedocs.io/en/latest/index.html)

1.  PIL库：[https://pillow.readthedocs.io/en/stable/index.html](https://pillow.readthedocs.io/en/stable/index.html)

感谢阅读！
