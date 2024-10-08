# 揭开锯齿状 COVID 图表的谜团

> 原文：[`towardsdatascience.com/solve-the-mystery-of-the-serrated-covid-chart-b0b517b224ef`](https://towardsdatascience.com/solve-the-mystery-of-the-serrated-covid-chart-b0b517b224ef)

## 使用 pandas 将数据下采样到合适的分辨率

[](https://medium.com/@lee_vaughan?source=post_page-----b0b517b224ef--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----b0b517b224ef--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b0b517b224ef--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0b517b224ef--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----b0b517b224ef--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0b517b224ef--------------------------------) ·阅读时间 7 分钟·2023 年 9 月 12 日

--

![](img/1640a51c93747c35858a5ab009a0dbde.png)

DreamShaper_v7_A_computer_monitor_displaying_a_chart_with_a_jagged_blue_line (作者 & Leonardo AI)

在 COVID-19 疫情的第一年，疾病的死亡人数引起了许多争议。争议的问题包括由于缺乏检测导致的早期低估、医院外死亡人数未被记录，以及区分 COVID-19 的死亡和伴随 COVID-19 的死亡 *的* [1][2]。

最终，所有的麻烦，以及大家的巨大不幸，疫情迅速变得政治化。党派评论员对每一条数据都跃跃欲试，寻找可以扭曲数据以谋取自己利益的方式。确认偏误猖獗。如果你当时在社交媒体上，可能见过质疑官方图表和数据准确性的帖子。

在这个 *快速成功的数据科学* 项目中，我们将查看一张当时出现在我 Facebook 动态中的特定图表。该图表记录了疫情第一年美国 COVID-19 的死亡人数，并显示出明显的锯齿状或“锯齿形”特征。

![](img/6f7d201b6f793aed44e26c2b38b1f820.png)

疫情第一年美国 COVID-19 死亡人数（作者来自《大西洋月刊》的“The COVID Tracking Project” [3]）

曲线的振荡频率很高，疾病是否以这种方式进展令人怀疑。虽然一些人认为这证明了 COVID 死亡统计明显错误且不可信，但我们这些拥有数据科学技能的人很快就揭开了这个被夸大的谜团。

# 数据集

我们将使用的数据是作为“[COVID 跟踪项目](https://covidtracking.com/)”的一部分收集的，数据来源于*大西洋月刊* [3]。它包含了从 2020 年 3 月 3 日到 2021 年 3 月 7 日的 COVID-19 统计数据。为了减少数据集的大小，我只下载了德克萨斯州的数据，并将其保存为 CSV 文件，文件在这个 [Gist](https://gist.github.com/rlvaugh/fa0491043140f68864625a515430c94d) 中。

你可以在 [这里](https://covidtracking.com/data/state/texas) 找到原始数据集，在 [这里](https://covidtracking.com/about-data/license) 找到数据的许可信息。

# 安装库

除了 Python，我们还需要 pandas 库。你可以通过以下两种方式之一安装它：

`conda install pandas`

或者

`pip install pandas`

pandas 的一个好处是它内置了绘图和处理*时间序列*的功能，即按时间顺序排列的数据点。Python 和 pandas 都将日期和时间视为特殊对象，这些对象“了解”公历、性别（60 基数）时间系统、时区、夏令时、闰年等的机制。

原生 Python 通过其 `[datetime](https://docs.python.org/3/library/datetime.html)` 模块支持时间序列。Pandas 的 [datetime 功能](https://pandas.pydata.org/docs/user_guide/timeseries.html) 基于 NumPy 的 `datetime64` 和 `timedelta64` 数据类型。通过将“字符串”日期转换为“真实”日期，我们可以做一些有用的事情，如提取星期几或对数据进行按周或按月平均。

# 代码

以下代码在 JupyterLab 中编写，按单元格描述。

## 导入库和加载数据

导入 pandas 后，我们将 CSV 文件加载到 DataFrame 中，只保留“date”和“mortalities”列。然后，我们使用 pandas 的 `to_datetime()` 方法将“date”列转换为 datetime，排序，并将其设置为 DataFrame 的索引。

```py
import pandas as pd

df = pd.read_csv('https://bit.ly/3ZgrmW0', 
                 usecols=['date', 'mortalities'])
df.date = pd.to_datetime(df.date)
df = df.sort_values('date')
df = df.set_index('date')
df.tail()
```

![](img/fa271d8e465c39dc42abaa01e7fb7b18.png)

## 绘制初始数据

Pandas 的绘图功能有限，但对于数据探索和“快速查看”分析来说足够了。

```py
df.plot();

# Optional code to save the figure:
# fig = df.plot().get_figure();
# fig.savefig('file_name.png',  bbox_inches='tight', dpi=600)
```

![](img/4a75d11c4126bee6e1d8c93111b16146.png)

初始德克萨斯数据集（作者提供的图片）

这个图表呈现了与国家数据相同的“波动性”。它还包括了在 7 月底附近的一个明显峰值。我们在调查振荡之前先看看这个峰值。

## 处理峰值

由于这个峰值显然是一个*最大*值，我们可以很容易地通过使用 `max()` 和 `idxmax()` 方法分别检索其值和对应的日期索引。

```py
print(f"Max. Value: {df['mortalities'].max()}")
print(f"Date: {df['mortalities'].idxmax()}")
```

![](img/4bbb2e1fb0f83f6d33b439034387f7d9.png)

这很可能是一个异常值，特别是考虑到 [疾病控制中心](https://covid.cdc.gov/covid-data-tracker/#trends_dailydeaths/) (CDC) 在这一天仅记录了 239 人死亡，这与相邻数据更一致。我们将使用 CDC 的数据。要更改 DataFrame，我们将应用 `.loc` 索引器，并传递日期和列名。

```py
# Set aberrant spike at 2020-7-27 to CDC value of 239 deaths:
df.loc['2020-7-27', 'mortalities'] = 239  
df.plot();
```

![](img/329295b22972739d27b71f957ce9512d.png)

修复了尖峰的德克萨斯数据（图像由作者提供）

看起来更好。现在让我们评估曲线的锯齿状特性。从视觉上看，每个月大约有 4-5 次波动，暗示了*每周*的频率。

## 检查每周数据

为了更详细地研究这一问题，我们将按星期几绘制数据的随机子集。我们将首先制作一个名为“df_weekdays”的 DataFrame 复制，并添加一列用于星期几。然后我们将绘制约两周的数据，范围为（任意）索引 103–120。

```py
# Examine values by weekday:
df_weekdays = df.copy()
df_weekdays['weekdays'] = df.index.day_name()

df_weekdays.iloc[103:120].plot(figsize=(10, 5), rot=90, x='weekdays');
```

![](img/9181cf1813ecc8915a25900e3f707af9.png)

按星期几绘制的德克萨斯数据子集（图像由作者提供）

计数在周末期间和之后似乎下降。让我们使用表格格式进一步调查这一点。我们将查看 3 周的间隔，并在打印输出中突出显示周一。

```py
# Highlight Mondays in the DataFrame printout:
df_weekdays = df_weekdays.iloc[90:115]
df_weekdays.style.apply(lambda x: ['background: lightgrey' 
                                   if x.weekdays == 'Monday'
                                   else '' for i in x], axis=1)
```

![](img/8bca768bac6d28521dffcf9f7f73f419.png)

最低报告的死亡人数始终发生在周一，周日的结果也似乎被抑制。这表明周末存在*报告*问题。

确实，这一问题已被[确认](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7363007/)为艾尔伯特·爱因斯坦医学院和约翰霍普金斯公共卫生学院的研究人员所发现[4]。他们发现，这些波动仅出现在死亡日期反映*报告*日期的数据集中。在*追溯*到*事件*日期的数据集中，这些波动是不存在的。

## 从天到周的降采样

由于报告问题，这些数据的适当分辨率是*每周*，而不是*每日*。为了以每周间隔绘制数据，我们需要使用 pandas 的 `resample()` 方法从较高频率降采样到较低频率。由于必须将多个样本合并为一个，`resample()` 方法通常会链式调用用于*聚合*数据的方法，如下图所示。

![](img/c2f88b93cb678960948b354209176a7f.png)

有用的 pandas 聚合方法（摘自“*Python 工具科学家*”[5]）

再次，由于报告问题影响了*每日*计数，但在*每周*基础上进行修正，因此将数据从每日降采样到每周应当可以合并低报告和高报告，并平滑曲线。我们将通过将 `resample()` 方法的参数设置为 `W` 并链式调用 `sum()` 聚合方法来实现这一点，从而求和每日值。除了 `W`，其他有用的时间序列频率见下表。

![](img/e559688f0c3179bed1f5d2fc90ea46f4.png)

有用的 pandas 时间序列频率（摘自“Python 工具科学家”[5]）

```py
# Resample weekly to remove serrations:
df.resample('W').sum().plot(grid=True);
```

![](img/7f7fe1284144c7891f352e32c6a1d69f.png)

将德克萨斯数据重新采样到每周频率（图像由作者提供）

将报告偏差“折叠”到新的降采样时间序列中，曲线看起来更平滑，正如我们所期望的那样。

# 总结

在这个项目中，我们应用了数据科学技术来解释历史 COVID-19 死亡率图表中奇怪的锯齿状特征。这些波动被一些人用来质疑死亡率数据的真实性。

我们发现这些波动似乎反映了每周报告的节奏，指向了病例报告中的偏见行为。在建议其他机制之前，例如周末医院护理质量或政府干预报告，应该考虑像这样的操作性解释。

我们使用 pandas 完成了这个项目，pandas 是 Python 的主要数据分析包。Pandas 非常适合像这样的“快速查看”分析。除了其类似电子表格的功能外，它还包括用于绘图和处理日期的内置工具。

我们使用的过程很简单，但这正是要点。编织一个阴谋论可能比揭穿一个阴谋论需要更多的努力。我在企业界的经历也类似。我们有时会争论 3 周是否执行一个任务，而这个任务如果有人坐下来做的话，仅需 3 小时就能完成！

# 谢谢！

感谢阅读，关注我获取更多*快速成功数据科学*项目。

# 引用

1.  Lang, Katherine, 2022 年 3 月 11 日，[我们是否高估了 COVID-19 死亡人数？ (medicalnewstoday.com)](https://www.medicalnewstoday.com/articles/how-are-covid-19-deaths-counted-and-what-does-this-mean)。

1.  Fichera, Angelo, 2021 年 4 月 2 日，[缺陷报告助长了关于 COVID-19 死亡人数的错误说法 — FactCheck.org](https://www.factcheck.org/2021/04/scicheck-flawed-study-fuels-erroneous-claims-about-covid-19-death-toll/)

1.  [*大西洋*的 COVID 跟踪项目](https://covidtracking.com/)

1.  Bergman, A., Sella, Y., Agre, P., & Casadevall, A. (2020), “美国 COVID-19 发病率和死亡率数据中的波动反映了诊断和报告因素，”MSystems, 5(4), [`doi.org/10.1128/mSystems.00544-20`](https://doi.org/10.1128/mSystems.00544-20)

1.  Vaughan, Lee, 2023 年，[*Python 工具：Anaconda、JupyterLab 和 Python 科学库的入门*](https://a.co/d/1SfWbGf)，No Starch Press, 旧金山。
