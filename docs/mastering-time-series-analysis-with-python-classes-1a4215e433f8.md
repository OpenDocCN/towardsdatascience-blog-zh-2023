# 使用 Python 类掌握时间序列分析

> 原文：[`towardsdatascience.com/mastering-time-series-analysis-with-python-classes-1a4215e433f8`](https://towardsdatascience.com/mastering-time-series-analysis-with-python-classes-1a4215e433f8)

## 面向对象编程在时间序列分析中的应用

[](https://spierre91.medium.com/?source=post_page-----1a4215e433f8--------------------------------)![Sadrach Pierre, Ph.D.](https://spierre91.medium.com/?source=post_page-----1a4215e433f8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1a4215e433f8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a4215e433f8--------------------------------) [Sadrach Pierre, Ph.D.](https://spierre91.medium.com/?source=post_page-----1a4215e433f8--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a4215e433f8--------------------------------) ·阅读时间 8 分钟·2023 年 1 月 10 日

--

![](img/197b5de13a8d4ab8c00930b07a2ff313.png)

图片由[Tima Miroshnichenko](https://www.pexels.com/@tima-miroshnichenko/)提供，发布于[Pexels](https://www.pexels.com/photo/vintage-alarm-clocks-displayed-on-a-wooden-shelf-8327953/)

时间序列分析是最常见的数据科学任务之一。它涉及分析按时间顺序排列的数据点的趋势。时间序列数据种类繁多，包括股票市场数据、天气数据、消费者需求数据等。时间序列分析在各个行业中都有应用，这使得它成为数据科学家和数据分析师的必备技能。

时间序列分析涉及许多技术，无法在一篇文章中总结。一些最常见的方法包括通过折线图可视化时间序列数据、构建时间序列预测模型、进行光谱分析以揭示周期性趋势、分析季节性趋势等。

由于时间序列分析涉及多种不同的技术，它自然适合面向对象编程。Python 类使得组织相关时间序列任务的属性和方法变得简单。例如，如果作为数据科学家，你经常进行折线图可视化、季节性分析和时间序列预测，类可以让你轻松组织这些任务的方法和属性。

当面向对象编程做得好的时候，可以提高时间序列实验的可读性、可重用性、可维护性和重复性。由于类是方法和属性的集合，它清楚地指出了哪些函数用于特定目的。这使得修改和维护现有函数变得简单。此外，一旦在你的类中定义了可靠的方法集合，你可以轻松地使用不同的参数、更新的训练数据等重新运行实验，而无需重新编写代码。

在这里，我们将演示如何编写一个类，以组织时间序列分析工作流中的步骤。工作流的每个部分将由一个类方法定义，该方法完成单个任务。我们将学习如何为时间序列可视化、统计测试、数据拆分用于训练和测试、训练预测模型和验证时间序列模型定义类方法。

在这个工作中，我将使用[Deepnote](https://deepnote.com/)编写代码，它是一个协作数据科学笔记本，使得运行可重复的实验变得非常容易。

对于我们的建模工作，我们将使用虚构的[Weather Climate Time Series data set](https://www.kaggle.com/datasets/sumanthvrao/daily-climate-time-series-data)，该数据集公开可在 Kaggle 上获取。该数据集可以免费使用、修改和共享，遵循[创作共用公共领域许可证 (CC0 1.0)](https://creativecommons.org/publicdomain/zero/1.0/)。

## 读取数据

首先，让我们导航到 Deepnote 并创建一个新项目（如果你还没有账户，可以免费注册）。

我们来创建一个名为‘time_series’的项目，并在该项目中创建一个名为‘time_series_oop’的笔记本。同时，将 DailyDelhiClimate.csv 文件拖放到页面左侧面板上“FILES”部分：

![](img/ff30092b2ac95908090c8d765fe773b0.png)

作者拍摄的截图

我们从导入 Pandas 库开始：

```py
import pandas as pd
```

接下来，我们将定义一个类来读取我们的天气数据。我们将把我们的 Python 类命名为 TimeSeriesAnalysis：

```py
class TimeSeriesAnalysis:
    def __init__(self, data):
        self.df = pd.read_csv(data)
```

我们可以定义一个类的实例，并通过我们的 TimeSeriesAnalysis 对象访问我们的数据框。让我们显示数据的前五行：

作者创建的嵌入

我们看到我们有一个日期对象和四个浮点列。我们有平均温度、湿度、风速和平均气压。让我们为我们的类添加一个准备时间序列的方法。该方法将接受一个数值列名，并返回一个时间序列，其中数据作为索引，值对应于选定的列：

```py
class TimeSeriesAnalysis:
    ...
    def get_time_series(self, target):
        self.df['date'] = pd.to_datetime(self.df['date'])
        self.df.set_index('date', inplace=True)
        self.ts_df = self.df[target]
```

现在我们可以定义我们类的一个新实例，并访问我们的时间序列数据：

作者创建的嵌入

## 生成摘要统计信息

接下来，我们可以添加一个生成时间序列基本统计信息的方法。例如，我们可以定义一个类方法，返回指定列的均值和标准差：

```py
class TimeSeriesAnalysis:
    ...
    def get_summary_stats(self):
        print(f"Mean {self.target}: ", self.ts_df.mean())
        print(f"Standard Dev. {self.target}: ", self.ts_df.std())
```

然后，我们可以定义一个新的类实例，在我们的对象实例上调用 get_time_series 方法，并将平均温度列作为输入，生成摘要统计数据：

嵌入由作者创建

我们可以对湿度、风速和平均压力做同样的处理

嵌入由作者创建

## 可视化时间序列

下一步我们可以定义一个方法来生成一些可视化。对于我们的可视化，我们需要导入 Seaborn、Matplotlib 和 statsmodels 包：

```py
import seaborn as sns 
import matplotlib.pyplot as plt 
import statsmodels.api as sm
```

让我们为平均温度定义一个新的类实例，创建一个折线图、直方图，并进行季节性分解：

```py
class TimeSeriesAnalysis:
    ...
    def visualize(self, line_plot, histogram, decompose):
        sns.set()
        if line_plot:
            plt.plot(self.ts_df)
            plt.title(f"Daily {self.target}")
            plt.xticks(rotation=45)
            plt.show()
        if histogram:
            self.ts_df.hist(bins=100)
            plt.title(f"Histogram of {self.target}")
            plt.show()
        if decompose:
            decomposition = sm.tsa.seasonal_decompose(self.ts_df, model='additive', period =180)
            fig = decomposition.plot()
            plt.show()
```

让我们让我们的方法提供生成目标值的直方图、时间序列折线图和时间序列分解的选项：

嵌入由作者创建

## 平稳性检验

***Dickey-Fuller 调整器***

我们可以添加的另一个方法是使用 Dickey-Fuller 检验进行平稳性测试。平稳性是指时间序列的均值和方差随时间不会改变。此外，如果时间序列是平稳的，它就没有任何趋势。通过检查我们的图表，我们可以看到天气数据是非平稳的，因为存在明显的季节性趋势。我们将使用 Dickey-Fuller 检验来检查平稳性。Dickey-Fuller 检验具有以下假设：

**原假设**：时间序列是非平稳的。

**备择假设**：时间序列是平稳的。

我们以以下方式解释结果：

> 如果检验统计量 < 临界值，我们拒绝原假设
> 
> 如果检验统计量 > 临界值，我们未能拒绝原假设

检验结果将具有 1%、5% 和 10% 显著性水平的临界值。结果还将包括检验统计量。让我们定义一个方法来运行这个测试：

```py
class TimeSeriesAnalysis:
    ...
    def stationarity_test(self):
        adft = adfuller(self.ts_df,autolag="AIC")
        output_df = pd.DataFrame({"Values":[adft[0],adft[1],adft[2],adft[3], adft[4]['1%'], adft[4]['5%'], adft[4]['10%']] , "Metric":["Test Statistics","p-value","No. of lags used","Number of observations used",
"critical value (1%)", "critical value (5%)", "critical value (10%)"]})
        self.adf_results = output_df 
```

在我们的笔记本的顶部，让我们从 statsmodels 包中导入 adfuller 方法：

```py
from statsmodels.tsa.stattools import adfuller
```

然后，我们可以在我们的气候对象上调用 stationarity_test 方法并访问测试结果：

嵌入由作者创建

我们看到在每种情况下，临界值都小于检验统计量，这是可以预期的。这意味着我们的数据是非平稳的。

***Kwiatkowski — Phillips — Schmidt — Shin (KPSS)***

**原假设**：时间序列是平稳的。

**备择假设**：时间序列是非平稳的。

类似于 ADF 测试，我们以以下方式解释结果：

> 如果检验统计量 < 临界值，我们拒绝原假设
> 
> 如果检验统计量 > 临界值，我们未能拒绝原假设

另一个平稳性测试是 Kwiatkowski — Phillips — Schmidt — Shin 测试 (KPSS)。该测试的原假设是时间序列是平稳的。ADF 和 KPSS 之间的区别在于 ADF 测试平稳性，而 KPSS 测试非平稳性。如果 ADF 测试结果未能拒绝原假设，则应使用 KPSS 来确认时间序列是非平稳的。我们扩展我们的平稳性测试方法，使其同时提供 ADF 和 KPSS 的选项。让我们从 `sstats` 模块导入 KPSS 方法：

```py
from statsmodels.tsa.stattools import adfuller, kpss
```

然后我们可以扩展我们平稳性测试方法的定义，以包括 KPSS：

```py
class TimeSeriesAnalysis:
    ...
    def stationarity_test(self):
        adft = adfuller(self.ts_df,autolag="AIC")
        kpsst = kpss(self.ts_df,autolag="AIC")

        adf_results = pd.DataFrame({"Values":[adft[0],adft[1],adft[2],adft[3], adft[4]['1%'], adft[4]['5%'], adft[4]['10%']] , "Metric":["Test Statistics","p-value","No. of lags used","Number of observations used",
"critical value (1%)", "critical value (5%)", "critical value (10%)"]})

        kpss_results = pd.DataFrame({"Values":[kpsst[0],kpsst[1],kpsst[2], kpsst[3]['1%'], kpsst[3]['5%'], kpsst[3]['10%']] , "Metric":["Test Statistics","p-value","No. of lags used",
    "critical value (1%)", "critical value (5%)", "critical value (10%)"]})

        self.adf_results = adf_results
        self.kpss_resulta = kpss_results 
```

然后我们可以在我们的气候对象上调用 `stationarity_test` 方法，并访问 KPSS 测试结果：

作者创建的嵌入

我们看到测试统计量小于临界值，这意味着我们拒绝原假设。原假设声明时间序列是平稳的。这意味着我们有证据支持时间序列是非平稳的。

我们可以定义一个执行 ADF 和 KPSS 测试并打印结果的方法：

```py
class TimeSeriesAnalysis:
    ...
    def stationarity_test(self):
            adft = adfuller(self.ts_df,autolag="AIC")
            kpsst = kpss(self.ts_df)

            adf_results = pd.DataFrame({"Values":[adft[0],adft[1],adft[2],adft[3], adft[4]['1%'], adft[4]['5%'], adft[4]['10%']] , "Metric":["Test Statistics","p-value","No. of lags used","Number of observations used",
    "critical value (1%)", "critical value (5%)", "critical value (10%)"]})

            kpss_results = pd.DataFrame({"Values":[kpsst[0],kpsst[1],kpsst[2], kpsst[3]['1%'], kpsst[3]['5%'], kpsst[3]['10%']] , "Metric":["Test Statistics","p-value","No. of lags used",
    "critical value (1%)", "critical value (5%)", "critical value (10%)"]})

            self.adf_results = adf_results
            self.kpss_results = kpss_results
            print(self.adf_results)
            print(self.kpss_results)
            self.adf_status = adf_results['Values'].iloc[1] > adf_results['Values'].iloc[4]
            self.kpss_status = kpss_results['Values'].iloc[1] < kpss_results['Values'].iloc[3]

            print("ADF Results: ", self.adf_status)
            print("KPSS Results: " ,self.kpss_status) 
```

结果如下：

作者创建的嵌入

最好使用两种测试来确认时间序列是否平稳。这两种测试的结果强烈表明时间序列是非平稳的。

有趣的是，最常见的时间序列预测方法，如 ARIMA 和 SARIMA 可以处理非平稳数据。通常建议在拟合 ARIMA 模型之前进行差分。Auto ARIMA 是一个 Python 包，它允许你自动搜索最佳差分参数。这很方便，因为不需要进行手动测试来找到最佳的差分阶数。

## 划分训练和测试数据

在准备训练预测模型的数据时，重要的是将数据划分为训练集和测试集。这有助于防止模型过拟合数据，从而导致泛化能力差。我们将训练集定义为 2016 年 7 月之前的所有数据，将测试集定义为 2016 年 7 月之后的所有数据点：

```py
def train_test_split(self):
        self.y_train = self.ts_df[self.ts_df.index <= pd.to_datetime('2016-07')]
        self.y_test = self.ts_df[self.ts_df.index > pd.to_datetime('2016-07')]
```

## 自动 ARIMA 模型

要使用 Auto ARIMA，首先让我们安装 `pdarima` 包：

```py
!pip install pmdarima
```

接下来，让我们从 `pdarima` 导入 `auto arima`：

```py
from pmdarima.arima import auto_arima
```

然后我们可以定义我们的拟合方法，用于将 ARIMA 模型拟合到训练数据中：

```py
def fit(self):
        self.y_train.fillna(0,inplace=True)
        model = auto_arima(self.y_train, trace=True,  error_action='ignore', suppress_warnings=True, stationary=True)
        model.fit(self.y_train)
        forecast = model.predict(n_periods=len(self.ts_df))
        self.forecast = pd.DataFrame(forecast,index = self.ts_df.index,columns=['Prediction'])

        self.ts_df = pd.DataFrame(self.ts_df, index = self.forecast.index)

        self.y_train = self.ts_df[self.ts_df.index < pd.to_datetime('2016-07')]

        self.y_test = self.ts_df[self.ts_df.index > pd.to_datetime('2016-07')]

        self.forecast = self.forecast[self.forecast.index > pd.to_datetime('2016-07')]
```

我们还将定义一个验证方法，该方法展示性能并可视化模型预测。我们将使用平均绝对误差来评估性能：

```py
def validate(self):
    plt.plot(self.y_train, label='Train')
    plt.plot(self.y_test, label='Test')
    plt.plot(self.forecast, label='Prediction')
    mae = np.round(mean_absolute_error(self.y_test, self.forecast), 2)
    plt.title(f'{self.target} Prediction; MAE: {mae}')
    plt.xlabel('Date')
    plt.ylabel(f'{self.target}')
    plt.xticks(rotation=45)
    plt.legend(loc='upper left', fontsize=8)
    plt.show()
```

完整的类如下所示：

作者创建的嵌入

现在，如果我们定义一个新的实例，对其进行拟合并验证模型，我们得到：

作者创建的嵌入

本文中的代码可以在 [GitHub](https://github.com/spierre91/deepnote/blob/main/time_series_oop.ipynb) 上找到。

## 结论

在这篇文章中，我们讨论了如何编写一个类来组织时间序列分析工作流程的步骤。首先，我们定义了一个方法，使我们能够读取和显示数据。然后，我们定义了一个方法来计算时间序列的均值和标准差。接着，我们定义了一个方法，使我们能够可视化时间序列数据的折线图、时间序列值的直方图以及数据的季节性分解。随后，我们编写了一个类方法，使我们能够执行平稳性统计检验。我们使用了 ADF 检验来测试平稳性，并使用了 KPSS 检验来测试非平稳性。最后，我们将一个 Auto ARIMA 模型拟合到训练数据中，验证了我们的模型，并为预测生成了可视化图。我希望你觉得这篇文章有用。我鼓励你尝试将这些方法应用到你的时间序列分析项目中！
