# 为什么箱线图不应单独使用及与之配合使用的3种图表

> 原文：[https://towardsdatascience.com/why-a-box-plot-should-not-be-used-alone-and-some-plots-to-use-it-with-23381f7e3cb6?source=collection_archive---------12-----------------------#2023-01-16](https://towardsdatascience.com/why-a-box-plot-should-not-be-used-alone-and-some-plots-to-use-it-with-23381f7e3cb6?source=collection_archive---------12-----------------------#2023-01-16)

## 解释箱线图中的缺失信息及使用Python代码解决该问题的替代方法

[](https://medium.com/@borih.k?source=post_page-----23381f7e3cb6--------------------------------)[![Boriharn K](../Images/1b23a79640f5272c1382918bfdba03b0.png)](https://medium.com/@borih.k?source=post_page-----23381f7e3cb6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23381f7e3cb6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----23381f7e3cb6--------------------------------) [Boriharn K](https://medium.com/@borih.k?source=post_page-----23381f7e3cb6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe20a7f1ba78f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-a-box-plot-should-not-be-used-alone-and-some-plots-to-use-it-with-23381f7e3cb6&user=Boriharn+K&userId=e20a7f1ba78f&source=post_page-e20a7f1ba78f----23381f7e3cb6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23381f7e3cb6--------------------------------) ·7分钟阅读·2023年1月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F23381f7e3cb6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-a-box-plot-should-not-be-used-alone-and-some-plots-to-use-it-with-23381f7e3cb6&user=Boriharn+K&userId=e20a7f1ba78f&source=-----23381f7e3cb6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23381f7e3cb6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-a-box-plot-should-not-be-used-alone-and-some-plots-to-use-it-with-23381f7e3cb6&source=-----23381f7e3cb6---------------------bookmark_footer-----------)![](../Images/ce42d5b8814e2083af77f22afef09db5.png)

照片由[Lance Asper](https://unsplash.com/@lance_asper)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

箱形图或 [箱形图和须图](https://en.wikipedia.org/wiki/Box_plot) 是 [描述性统计](https://en.wikipedia.org/wiki/Descriptive_statistics) 中常见的数据可视化图表。该图通过其 [四分位数](https://en.wikipedia.org/wiki/Quartile) 直观地展示数值数据，这些四分位数包括最小值、第一个四分位数、中位数、第三个四分位数和最大值。此外，它有助于轻松发现异常值。

然而，没有什么是完美的。箱形图有一些主要的限制。在解释这些限制之前，让我们看看下面的图片。

![](../Images/89a3fd4ee36fd719791d5cbcf47d6b31.png)

箱形图显示了三组数据的分布。图像由作者提供。

这些是三组不同数值数据的箱形图。可以注意到它们看起来几乎一样。我知道你可能有一种直觉感觉它们并不相同。

事实上，当使用直方图绘制真实数据时，数据完全不同，如下图所示。在这种情况下，使用箱形图可能会误导数据的解释。

![](../Images/4b06924823da179aaaac1046a5db6029.png)

直方图和箱形图显示了三组数据的分布。图像由作者提供。

> *这个问题可以简单解释为：* ***箱形图无法显示数据集的*** [***众数***](https://en.wikipedia.org/wiki/Mode_(statistics))***。***

此外，作为出现频率最高的值，众数也指的是分布的 [局部最大值](https://en.wikipedia.org/wiki/Mode_(statistics))。在处理 [双峰分布](https://www.statisticshowto.com/what-is-a-bimodal-distribution/)、它有两个众数（或峰值），或 [多峰分布](https://www.statisticshowto.com/multimodal-distribution/)、它有多个峰值时，箱形图无法表达这些洞察信息。

此外，箱形图的形状为矩形，因此它可能会误导我们认为四分位数范围内有数据。如果四分位数范围内存在空白或没有数据，这些情况可能不会被注意到。

![](../Images/202c1c534403af229b34fa93e1e43a84.png)![](../Images/723f1767f9933c6a9d2029c4169f6a29.png)

本文推荐的用于与箱形图配合使用的可视化示例。图像由作者提供。

鉴于这些限制，箱形图应该与其他图表一起使用。本文将介绍一些可以与箱形图配合使用的替代图表，以可视化数据特征。

开始吧。

**免责声明：** 本文对使用箱形图没有任何异议。如前所述，一切都有利有弊。就个人而言，我仍然使用箱形图，因为它帮助我轻松找到异常值。然而，我不会单独使用它。要彻底理解数据，还需要使用其他图表。

# 获取数据

从导入库开始。

```py
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

%matplotlib inline
```

作为示例，本文将使用一个模拟数据集。我们将使用[Numpy](https://numpy.org/doc/stable/reference/random/generated/numpy.random.normal.html)创建一个正态分布的数据集。

```py
mu, sigma = 0, 1 #mean and standard deviation
s1_pre = np.random.normal(mu, sigma, size=5000)
s1 = [i for i in s1_pre if i<2.6 and i>-2.6]
```

通过下面的代码修改生成的数据，以获得另外两个具有不同分布和数据间隙的数据集。

```py
#defnie a function
def pair_sum(input_):
    input_.sort()
    if len(input_)%2 == 0:
        loc = [i for i in range(len(input_)) if i%2 == 0]
        output = [input_[l] + input_[len(input_)-(l+1)] for l in loc]
    else:
        loc = [i for i in range(len(input_)) if i%2 == 0]
        output = [input_[l] + input_[len(input_)-(l+1)] for l in loc[0:-1]]
        output.append(input_[-1])
    return output

#bimodal distribution with gaps
s2_out, s2_sub1, s2_sub2 = [], [], []
for i in s1:
    if i>0.05 and i<0.6:
        s2_sub1.append(i)
    elif i<-0.05 and i>-0.6:
        s2_sub2.append(i)
    else:
        s2_out.append(i)

sub1_mod = pair_sum(s2_sub1)
sub2_mod = pair_sum(s2_sub2)
s2 = s2_out + sub1_mod*2 + sub2_mod*2

#distribution with gaps
s3_out, s3_sub = [], []
for i in s1:
    if i>-0.4 and i<0.4:
        s3_sub.append(i)
    else:
        s3_out.append(i)

sub3 = pair_sum(s3_sub)
s3 = s3_out + sub3*2
```

绘制模拟数据以查看结果。

```py
c_list = ['#1a936f', '#F77F05', '#D62829']

n = 25
f, axes = plt.subplots(3, 2 ,figsize=(12,5))
ax1=sns.boxplot(s1,ax= axes[0,0],color= c_list[0],boxprops= dict(alpha=.9))
ax2=sns.boxplot(s2,ax= axes[1,0],color= c_list[1],boxprops= dict(alpha=.9))
ax3=sns.boxplot(s3,ax= axes[2,0],color= c_list[2],boxprops= dict(alpha=.9))

ax4=sns.histplot(s1, bins=n, ax=axes[0,1], color=c_list[0])
ax5=sns.histplot(s2, bins=n, ax=axes[1,1], color=c_list[1])
ax6=sns.histplot(s3, bins=n, ax=axes[2,1], color=c_list[2])

ax4.set(ylabel=None, yticklabels=[])
ax5.set(ylabel=None, yticklabels=[])
ax6.set(ylabel=None, yticklabels=[])
plt.show()
```

![](../Images/4b06924823da179aaaac1046a5db6029.png)

直方图和箱线图显示了模拟数据的分布。图片来源：作者。

在继续之前，让我们创建一个 DataFrame。在这一步中，我们将使用[panda.cut](https://pandas.pydata.org/docs/reference/api/pandas.cut.html)将数据值分段并排序，以备后用。

```py
bins1 = pd.cut(s1, bins=n, labels=False)
bins2 = pd.cut(s2, bins=n, labels=False)
bins3 = pd.cut(s3, bins=n, labels=False)

df1 = pd.DataFrame(zip(s1, bins1), columns=['v1','bins'])
df2 = pd.DataFrame(zip(s2, bins2), columns=['v2','bins'])
df3 = pd.DataFrame(zip(s3, bins3), columns=['v3','bins'])

df1_m = pd.melt(df1, id_vars=['bins'],value_vars=['v1'])
df2_m = pd.melt(df2, id_vars=['bins'],value_vars=['v2'])
df3_m = pd.melt(df3, id_vars=['bins'],value_vars=['v3'])

df = pd.concat([df1_m, df2_m, df3_m])
df.head()
```

![](../Images/f21f70408dd8b2aa04a7bf58a3b3bcb6.png)

# 替代的数据可视化

## 1\. 小提琴图

小提琴图的概念基于箱线图和[核密度](https://en.wikipedia.org/wiki/Kernel_density_estimation)图。因此，结果可以表达数据集的峰值。这是检查数据集是否具有单峰、双峰或多峰分布的好方法。

在本文中，我们将主要使用[Plotly](https://plotly.com/python/histograms/)，一个有用的数据可视化库，来可视化数据。我们可以用几行代码绘制小提琴图，如下所示。

```py
import plotly.express as px
fig = px.violin(df, x = df['value'], box=True, width=900, height=600,
                color=df['variable'], color_discrete_sequence=color_list)
fig.update_layout(legend_title="", xaxis_title=None)
fig.show()
```

![](../Images/d0961bec8c0b642a57375e5ea82df271.png)

小提琴图显示了模拟数据的分布。图片来源：作者。

## 2\. 直方图

与箱线图相比，直方图是另一种能够展示数据分布的数据可视化方式。每个区间的数据密度显示了是否存在间隙或多个峰值，或四分位数范围之间的间隙。

先用下面的代码分别绘制每个直方图。

```py
fig1 = px.histogram(x= df[df['variable']=='v1']['value'],
                    nbins = 50, width=900, height=400,
                    color_discrete_sequence=[color_list[0]])
fig2 = px.histogram(x= df[df['variable']=='v2']['value'],
                    nbins = 50, width=900, height=400,
                    color_discrete_sequence=[color_list[1]])
fig3 = px.histogram(x= df[df['variable']=='v3']['value'],
                    nbins = 50, width=900, height=400,
                    color_discrete_sequence=[color_list[2]])

fig1.update_layout(xaxis_title=None, yaxis_title=None)
fig2.update_layout(xaxis_title=None, yaxis_title=None)
fig3.update_layout(xaxis_title=None, yaxis_title=None)

fig1.show()
fig2.show()
fig3.show()
```

![](../Images/5c99bf1f7e9cf93952052b7ad0b4cff5.png)

直方图显示了模拟数据的分布。图片来源：作者。

我们还可以叠加直方图并添加[地毯图](https://en.wikipedia.org/wiki/Rug_plot#:~:text=A%20rug%20plot%20is%20a,a%20one%2Ddimensional%20scatter%20plot.)来展示分布标记。

```py
fig = px.histogram(df, x= 'value', nbins = 60, color = 'variable',
                   opacity=0.8, width=900, height=600, marginal="rug",
                   color_discrete_sequence=color_list)
fig.update_layout(yaxis_title=None, legend_title="", xaxis_title=None)
fig.show()
```

![](../Images/2efd12d106c0cc03f5e40c70f8a30d10.png)

叠加直方图和地毯图显示了模拟数据的分布。图片来源：作者。

可以添加核密度估计（KDE）线，以使结果更具信息性。

```py
import plotly.figure_factory as ff

group_labels = list(set(df['variable']))
hist_data = [df[df['variable']=='v1']['value'],
             df[df['variable']=='v2']['value'],
             df[df['variable']=='v3']['value']]
fig = ff.create_distplot(hist_data, group_labels, bin_size=0.25,
                         colors=color_list)
fig.update_layout(width=900, height=600)
fig.show()
```

![](../Images/4224346f1ca4646b839c414bc45167c7.png)

叠加直方图、KDE 线和地毯图显示了模拟数据的分布。图片来源：作者。

## **3\. 条形图**

如果之前的结果看起来复杂，条形图可能是创建简洁可视化的一个好方案。

可以创建条形图，如下所示。

```py
import plotly.express as px
fig = px.strip(df, x = 'value', y ='variable', stripmode='overlay',
               width=1000, height=600
              )
fig.update_layout(xaxis_range=[-2.69, 2.69])
fig.update_layout(legend_title="", yaxis_title=None,
                  xaxis_title=None,showlegend=False)
fig.show()
```

![](../Images/9969bfab864dc56bff5b21dd8e588eb6.png)

条形图显示了模拟数据的分布。图片来源：作者。

然而，由于我们无法区分每个数据范围内的数据密度，结果可能不会太有用。通过分配颜色以表达密度，结果可以得到改善。

下面的代码展示了如何定义一个函数，以创建一个字典来为每个数据范围中的数据点着色。

```py
#add a new columns to facilitate creating a dictionary
df['vari_bins'] = [v+'_'+str(b) for v,b in zip(df['variable'],df['bins'])]

def strip_color(input_list, prefix, color, n_color):
    #count data in each bin 
    c_bin = [input_list.count(i) for i in range(n)]
    #create a dictionary by enumerate each bin  
    dict_c = {prefix+str(c):i for c,i in enumerate(c_bin)}

    #sort the dictionary
    sort = {i[0]:i[1] for i in sorted(dict_c.items(), key=lambda x: x[1])}
    #extract color code from color palette
    pal = list(sns.color_palette(palette=color, n_colors=n_color).as_hex())

    #assign color code to the dictionary
    dict_out = {c:p for c,p in zip(sort.keys(), pal)}

    return dict_out
```

应用函数

```py
dict_p1 = strip_color(list(df1_m['bins']), 'v1_', 'YlGn', 25)
dict_p2 = strip_color(list(df2_m['bins']), 'v2_', 'Oranges', 25)
dict_p3 = strip_color(list(df3_m['bins']), 'v3_', 'YlOrRd', 25)

dict_ = {}
dict_.update(dict_p1)
dict_.update(dict_p2)
dict_.update(dict_p3)
```

绘制条形图

```py
import plotly.express as px
fig = px.strip(df, x = 'value', y ='variable',
               color = 'vari_bins',
               color_discrete_map=dict_, stripmode='overlay',
               width=1000, height=600,
              )
fig.update_layout(xaxis_range=[-2.69, 2.69])
fig.update_layout(legend_title="", yaxis_title=None,
                  xaxis_title=None, showlegend=False)
fig.show()
```

看呐!!

![](../Images/c59792ad9f9fdb9123870445ca77afd8.png)

带有颜色的条形图展示了模拟数据的分布。图像由作者提供。

## 总结

这篇文章展示了箱线图在展示众数时的局限性，这可能导致误导性的解释。在绘制具有两个或更多峰值的数据集或在四分位数范围内显示数据缺口时，它并不有用。顺便说一下，箱线图有一些优点，例如简单的可视化和显示异常值。

我相信还有比这篇文章中提到的更多图表可以弥补箱线图的缺点。

如果你有任何建议，请随时留言。我很乐意看到它。

感谢阅读

这些是我撰写的数据可视化文章，你可能会感兴趣：

+   7 种使用 Python 表达排名随时间变化的可视化方法 ([链接](https://medium.com/towards-data-science/7-visualizations-with-python-to-express-changes-in-rank-over-time-71c1f11d7e4b))

+   8 种使用 Python 处理多个时间序列数据的可视化方法 ([链接](/8-visualizations-with-python-to-handle-multiple-time-series-data-19b5b2e66dd0))

+   9 种使用 Python 的可视化方法，比条形图更引人注目 ([链接](/9-visualizations-that-catch-more-attention-than-a-bar-chart-72d3aeb2e091))

+   9 种使用 Python 的可视化方法来展示比例，而不是饼图 ([链接](https://medium.com/p/4e8d81617451/))

## 参考资料

+   Wikimedia Foundation. (2023年1月4日). *箱线图*。维基百科。取自 2023 年 1 月 8 日，来源于 [https://en.wikipedia.org/wiki/Box_plot](https://en.wikipedia.org/wiki/Box_plot)

+   *用于多峰分布的箱线图*。交叉验证。取自 2023 年 1 月 8 日，来源于 [https://stats.stackexchange.com/questions/137965/box-and-whisker-plot-for-multimodal-distribution](https://stats.stackexchange.com/questions/137965/box-and-whisker-plot-for-multimodal-distribution)

+   Stephanie. (2022年9月25日). *双峰分布：是什么？* Statistics How To. 取自 2023 年 1 月 10 日，来源于 [https://www.statisticshowto.com/what-is-a-bimodal-distribution](https://www.statisticshowto.com/what-is-a-bimodal-distribution)
