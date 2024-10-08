# 大学橄榄球会议重组——回归分析

> 原文：[`towardsdatascience.com/college-football-conference-realignment-regression-8f0776278d55`](https://towardsdatascience.com/college-football-conference-realignment-regression-8f0776278d55)

[](https://medium.com/@gspmalloy?source=post_page-----8f0776278d55--------------------------------)![乔瓦尼·马洛伊](https://medium.com/@gspmalloy?source=post_page-----8f0776278d55--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f0776278d55--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f0776278d55--------------------------------) [乔瓦尼·马洛伊](https://medium.com/@gspmalloy?source=post_page-----8f0776278d55--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----8f0776278d55--------------------------------) ·阅读时间 7 分钟·2023 年 8 月 6 日

--

欢迎来到我的系列文章第二部分，讨论会议重组！去年夏天，当会议重组如火如荼时，Tony Altimore 在 Twitter 上发布了一项[研究](https://twitter.com/TJAltimore/status/1536438310809247745)，激发了我进行自己的会议重组分析。该系列分为四部分（完整的动机在第一部分中可以找到）：

1.  [大学橄榄球会议重组——Python 中的探索性数据分析](https://medium.com/towards-data-science/college-football-conference-realignment-exploratory-data-analysis-in-python-6f4a74037572)

1.  大学橄榄球会议重组——回归分析

1.  [大学橄榄球会议重组——聚类分析](https://medium.com/p/6ca16840ed3d#0733-ef9637a21b53)

1.  [大学橄榄球会议重组——node2vec](https://medium.com/towards-data-science/college-football-conference-realignment-node2vec-ba2e931bb1c)

![](img/9ac3d6a3c19c4114817821937ef1c400.png)

图片由[诺伯特·布劳恩](https://unsplash.com/@medion4you?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

希望系列的每个部分都能为你提供对备受喜爱的大学橄榄球未来的新视角。对于那些没有阅读第一部分的人，简要概述是我创建了自己从网络各个来源汇编的数据集。这些数据包括[每个 FBS 项目的基本信息](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_programs)、所有[大学橄榄球对抗赛](https://en.wikipedia.org/wiki/List_of_NCAA_college_football_rivalry_games)的非官方近似值、[体育场大小](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_stadiums)、[历史表现](https://en.wikipedia.org/wiki/List_of_NCAA_Division_I_FBS_football_bowl_records)、[AP 前 25 名投票的出现频率](https://collegepollarchive.com/football/index.cfm)、学校是否为[AAU](https://www.aau.edu/) 或[R1](https://en.wikipedia.org/wiki/List_of_research_universities_in_the_United_States)机构（对加入 Big Ten 和 Pac 12 历史上非常重要）、[NFL 选秀人数](https://www.ncaa.com/news/football/article/2020-04-27/college-football-teams-most-nfl-draft-picks-2000)、[2017-2019 年程序收入数据](https://graphics.wsj.com/table/NCAA_2019)以及关于大学橄榄球粉丝基础规模的[近期估计](https://drive.google.com/file/d/1MiUOwx8X3H2bSkUOz8a1YhceyJWLLCoJ/view)。结果表明，体育场容量、2019 年收入和历史 AP 投票成功率与 Tony Altimore 的粉丝基础规模估计结果有很强的相关性。

![](img/4cc2b8e4d2962633d1dfb3b1ee4302bd.png)

相关矩阵显示了每个特征与自身之间的完美正相关关系。我们还看到体育场容量、粉丝基础规模、2019 年收入以及 2001 年至 2021 年间出现在 AP 前 25 名的周数之间存在较高的相关性。

**监督学习**

所以，这让我思考：我们能否创建一个简单的回归模型来估计粉丝基础的大小？

广泛来说，我们可以将机器学习分为监督学习和非监督学习。在监督学习中，目标是预测一个预定义的离散类别或连续变量。在非监督学习中，目标是发现数据中那些不明显的趋势。回归是一种监督学习类型，其中预测目标是一个连续变量。Shervine 和 Afshine Amidi 编写了一个极好的[参考指南和资源](https://stanford.edu/~shervine/teaching/)。 （它已经被翻译成 11 种其他语言！）

我们选择的回归模型受限于数据中观察数量较少，因为大学橄榄球中只有 133 支球队。不论我们选择什么模型，[scikit-learn](https://scikit-learn.org/stable/index.html) 包将为我们提供支持。它易于实现且文档详尽。

**特征工程**

现在我们有了方法，可以重构数据以获得最佳模型性能。这通常被称为特征工程。首先，我们导入依赖项并上传数据。

```py
#Import dependencies
import numpy as np
import pandas as pd
# Read csv of data
cfb_info_df = pd.read_csv(r'.\FBS_Football_Team_Info.csv', encoding = 'unicode_escape')
```

我们将仅保留对本分析相关的特征：

```py
# Drop Unused columns
cfb_info_df_regression = cfb_info_df[['Latitude', 'Longitude','Enrollment', 'Current_conference_2025','years_playing', 'years_playing_FBS', 'Stadium_capacity', 'is_aau_member', 'is_R1', 'total_draft_picks_2000_to_2020', 'first_rd_draft_picks_2000_to_2020', 'number_1_draft_picks_2000_to_2020',  'wsj_college_football_revenue_2019', 'wsj_college_football_value_2018', 'wsj_college_football_value_2017', 'bowl_games_played', 'bowl_game_win_pct', 'historical_win_pct', 'total_games_played','p_AP_Top_25_2001_to_2021', 'tj_altimore_fan_base_size_millions']]
```

现在，我们可以将这些数据分为特征 X 和标签 y。在这种情况下，特征是所有数据，除了估计的粉丝基础规模。该估计值作为标签。

```py
X = cfb_info_df_regression.drop(['tj_altimore_fan_base_size_millions'], axis = 1)
y = cfb_info_df_regression['tj_altimore_fan_base_size_millions']
```

现在，我们可以使用 pandas 将分类特征转换为 [独热编码](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.get_dummies.html) 向量。这将我们的会议名称列转换为几个布尔值列。

```py
X = pd.get_dummies(X, columns = ['Current_conference_2025'])
```

我们可以使用 scikit-learn 中的 train_test_split 函数轻松地对数据进行 70–30 的训练-测试集拆分。对于我们的目的，这将给我们 93 个训练观测值和 40 个测试观测值。

```py
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=0)
```

我们将使用最小-最大缩放对数值特征进行转换。最小-最大缩放很重要，因为它可以将每个数值分布的范围更改为 0 到 1 之间，同时保持分布的形状。实现这一点很重要，因为它是在将数据拆分为训练集和测试集之后进行的，以避免数据泄露。使用 scikit-learn 有预定义的方法来执行此操作，包括一个 [内置管道函数](https://scikit-learn.org/stable/modules/preprocessing.html#preprocessing-scaler)，我们可以在其中定义预处理类型，但为了我们的目的，我定义了自己的 min_max_scaling 函数，并使用此函数转换了所有列：

```py
def min_max_column(column):
    column = column.astype('float')
    column_scaled = (column - min(column)) / (max(column) - min(column))
    return column_scaled
```

现在，我们分别将训练集和测试集中的所有特征转换，以避免数据泄露：

```py
for col in X_train.columns:
    X_train[col] = min_max_column(X_train[col])

for col in X_test.columns:
    X_test[col] = min_max_column(X_test[col])
```

**线性回归**

这样，我们就准备好运行模型了。我们从简单的 [线性回归](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html#sklearn.linear_model.LinearRegression) 开始。

```py
from sklearn.linear_model import LinearRegression
reg = LinearRegression().fit(X_train, y_train)
```

我们可以使用通过 score() 函数计算的 R 平方度量来衡量回归的表现。

```py
from sklearn.linear_model import LinearRegression
reg = LinearRegression().fit(X_train, y_train)
```

不幸的是，R 平方度量只有大约 0.5，所以我们的预测效果不是很好。我们可以使用 plotly 绘制实际粉丝基础规模与预测粉丝基础规模的比较图。在下图中，散点图中每个点的大小是绝对百分比误差的大小。颜色表示是否存在不足预测。你可以直观地看到，模型对小型粉丝基础的表现最差：

```py
import plotly.express as px
import plotly.express as px
#Create a data frame for plot
plot_df = pd.DataFrame(cfb_info_df['Team'].iloc[list(y_test.index)], columns=['Team'])
plot_df['Actual Fan Base Size'] = y_test
plot_df['Predicted Fan Base Size'] = reg.predict(X_test)
plot_df['Absolute Percent Error'] = abs(plot_df['Actual Fan Base Size'] - plot_df['Predicted Fan Base Size'])/plot_df['Actual Fan Base Size']
plot_df['Under Predict'] = plot_df['Actual Fan Base Size'] > plot_df['Predicted Fan Base Size']

fig = px.scatter(plot_df, x='Actual Fan Base Size', y='Predicted Fan Base Size', size = 'Absolute Percent Error', 
                 color = 'Under Predict', hover_data = ['Team'])
fig.show()
```

![](img/8534abaf590404d820561c486a2429e5.png)

在图中，我们可以直观地看到线性回归的表现，比较实际的粉丝基础规模（以百万计）与预测的规模。点的大小是绝对百分比误差，颜色表示模型是过度预测还是不足预测。

**随机森林**

现在我们已经看到线性模型的表现，接下来让我们尝试一种更高级的机器学习模型——随机森林。随机森林依赖于一种叫做“自助法”（bagging）的概念来提高预测准确性。它实际上生成了许多不同的决策树，这些树由于引入的随机性而略有不同。它将这些树中学到的知识结合起来，以改善整体预测。

![](img/967633e86aaa08e59de2436f17ac00a6.png)

[维基百科](https://en.wikipedia.org/wiki/Random_forest)中包含了关于随机森林如何工作的优秀可视化图示。

方便的是，我们不需要对数据进行缩放，因为随机森林模型不基于距离度量来进行预测。因此，我们可以从 train_test_split()函数中重新采样：

```py
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=0)
```

现在，我们可以轻松地训练一个包含 100 棵树且每棵树深度无限的随机森林模型。

```py
from sklearn.ensemble import RandomForestRegressor
reg = RandomForestRegressor(n_estimators=100, max_depth=None, random_state=0)
reg.fit(X_train, y_train)
```

让我们看看 R 平方值是否有所提升：

```py
reg.score(X_test, y_test)
```

R 平方值现在为 0.78，这是一个很大的改进！如果我们使用新的随机森林模型创建与上面相同的图表，我们可以看到小规模粉丝群体的表现大大改善。重要的是，我们也不再预测负的粉丝基础规模。

![](img/b6289edeca74c65bdb565da4fa3872c5.png)

随机森林模型在将实际粉丝基础规模（以百万计）与预测规模进行比较时表现更好。现在没有更多的负粉丝数的团队了！

那么，是什么驱动了这些预测呢？随机森林在可解释性方面表现出色，因为它包括一个叫做**特征重要性**的属性。类似于线性回归中的系数，这些是随机森林模型在进行预测时依赖某个特征的程度。特征重要性是一个相对度量，所以它只能告诉我们某个特征在此模型中的有用程度。

```py
import plotly.express as px
#Create a data frame for plot
plot_df = pd.DataFrame(X_train.columns, columns=['Feature Name'])
plot_df['Importance'] = reg.feature_importances_
fig = px.bar(plot_df, x='Feature Name', y='Importance')
fig.show()
```

![](img/c3c6f903a7cbf54c377aaa35b4605d48.png)

随机森林模型中的特征重要性显示，场上表现是粉丝基础规模的驱动因素。

根据上面比较不同特征的条形图，场上表现似乎是预测的主要驱动因素。最重要的特征是过去 20 年中团队进入 AP 前 25 的周数百分比。其次重要的特征群体是《华尔街日报》的价值/收入数据。显然，拥有更多粉丝的团队赚得更多。然后我们看到 NFL 选秀、体育场容量（更多粉丝=更大的体育场）、碗赛出场次数以及历史胜率也很重要。地理位置、招生人数、踢足球年限和学术成功似乎不太适合用来预测。

我将最重要的结论留到最后，因为它与会议重组讨论相关。你注意到图表的右侧了吗？会议成员资格并不是粉丝基础规模的重要预测因素。正如我们在本博客系列第一部分讨论的那样，相关性并不等于因果关系，特征重要性也是如此。然而，这确实似乎表明你可以在任何会议中同样快速地获得或失去粉丝。关键在于场上的表现。

**模型改进**

我不会在这篇博客中包含这些内容，但我们可以花些时间通过更好的特征工程或超参数调整来改进这些模型。此外，报告交叉验证的准确性也是更好的做法。我们的数据集很小，所以我会将这部分留到另一篇博客中。

确保继续阅读本博客系列的第三部分，因为我们将最终深入探讨一些基于数据的大学橄榄球会议建议。

对我的内容感兴趣吗？请考虑[在 Medium 上关注我](https://medium.com/@gspmalloy)。

在 Twitter 上关注我：@malloy_giovanni

你发现过任何有趣的回归应用案例在大学橄榄球中吗？你会如何改进这个模型？
