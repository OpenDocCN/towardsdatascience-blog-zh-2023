# 气候变化的时间序列：太阳辐射预测

> 原文：[`towardsdatascience.com/time-series-for-climate-change-solar-irradiance-forecasting-a972dac7418f`](https://towardsdatascience.com/time-series-for-climate-change-solar-irradiance-forecasting-a972dac7418f)

## 如何利用时间序列分析和预测应对气候变化

[](https://vcerq.medium.com/?source=post_page-----a972dac7418f--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----a972dac7418f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a972dac7418f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a972dac7418f--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----a972dac7418f--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a972dac7418f--------------------------------) ·8 分钟阅读·2023 年 4 月 5 日

--

![](img/8cb4875f364354ea926c19d6ebf8674d.png)

图片由 [Andrey Grinkevich](https://unsplash.com/@grin?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是系列文章 *气候变化的时间序列* 的第二部分。文章列表：

+   第一部分: 预测风能

# 太阳能系统

太阳能是一种越来越普及的清洁能源来源。

太阳光通过光伏设备转化为电能。由于这些设备不产生污染物，因此被认为是清洁能源的来源。除了环境效益外，太阳能因其低成本而具有吸引力。初期投资较大，但长期低成本是值得的。

生产的能源量取决于太阳辐射水平。然而，太阳条件可能会迅速变化。例如，云层可能会突然遮住太阳，降低光伏设备的效率。

因此，太阳能系统依赖于预测模型来预测太阳条件。像 [风能预测的情况](https://medium.com/towards-data-science/time-series-for-climate-change-forecasting-wind-power-8ed6d653a255) 一样，准确的预测直接影响这些系统的有效性。

## 超越能源生产

预测太阳辐射除了用于能源外，还有其他应用，例如：

+   农业：农民可以利用预测来优化作物生产。例如，估算何时种植或收获作物，或优化灌溉系统；

+   土木工程：预测太阳辐射对设计和建造建筑物也很有价值。预测可以用于最大化太阳辐射，从而减少取暖/制冷成本。预测还可以用于配置空调系统，这有助于建筑内能源的高效利用。

## 挑战及下一步

尽管其重要性，太阳条件高度可变且难以预测。这些条件依赖于几个气象因素，而这些信息有时无法获得。

在本文的其余部分，我们将开发一个太阳辐射预测模型。除此之外，你将学会如何：

+   可视化多变量时间序列；

+   转换多变量时间序列以进行监督学习；

+   根据相关性和重要性评分进行特征选择。

# 教程：预测太阳辐射

本教程基于由美国农业部收集的数据集。你可以在参考文献[1]中查看更多详细信息。本教程的完整代码可在 Github 上获取：

+   [`github.com/vcerqueira/tsa4climate`](https://github.com/vcerqueira/tsa4climate)

数据是一个多变量时间序列：在每个时刻，观测值由多个变量组成。这些变量包括以下天气和水文变量：

+   太阳辐射（每平方米瓦特）；

+   风向；

+   积雪深度；

+   风速；

+   露点温度；

+   降水量；

+   蒸汽压力；

+   相对湿度；

+   气温。

该系列从 2007 年 10 月 1 日到 2013 年 10 月 1 日，按小时收集，总计 52,608 次观测。

下载数据后，我们可以使用 pandas 读取它：

```py
import re
import pandas as pd
# src module available here: https://github.com/vcerqueira/tsa4climate/tree/main/src
from src.log import LogTransformation

# a sample here: https://github.com/vcerqueira/tsa4climate/tree/main/content/part_2/assets
assets = 'path_to_data_directory'

DATE_TIME_COLS = ['month', 'day', 'calendar_year', 'hour']
# we'll focus on the data collected at particular station called smf1
STATION = 'smf1'

COLUMNS_PER_FILE = \
    {'incoming_solar_final.csv': DATE_TIME_COLS + [f'{STATION}_sin_w/m2'],
     'wind_dir_raw.csv': DATE_TIME_COLS + [f'{STATION}_wd_deg'],
     'snow_depth_final.csv': DATE_TIME_COLS + [f'{STATION}_sd_mm'],
     'wind_speed_final.csv': DATE_TIME_COLS + [f'{STATION}_ws_m/s'],
     'dewpoint_final.csv': DATE_TIME_COLS + [f'{STATION}_dpt_C'],
     'precipitation_final.csv': DATE_TIME_COLS + [f'{STATION}_ppt_mm'],
     'vapor_pressure.csv': DATE_TIME_COLS + [f'{STATION}_vp_Pa'],
     'relative_humidity_final.csv': DATE_TIME_COLS + [f'{STATION}_rh'],
     'air_temp_final.csv': DATE_TIME_COLS + [f'{STATION}_ta_C'],
     }

data_series = {}
for file in COLUMNS_PER_FILE:
    file_data = pd.read_csv(f'{assets}/{file}')

    var_df = file_data[COLUMNS_PER_FILE[file]]

    var_df['datetime'] = \
        pd.to_datetime([f'{year}/{month}/{day} {hour}:00'
                        for year, month, day, hour in zip(var_df['calendar_year'],
                                                          var_df['month'],
                                                          var_df['day'],
                                                          var_df['hour'])])

    var_df = var_df.drop(DATE_TIME_COLS, axis=1)
    var_df = var_df.set_index('datetime')
    series = var_df.iloc[:, 0].sort_index()

    data_series[file] = series

mv_series = pd.concat(data_series, axis=1)
mv_series.columns = [re.sub('_final.csv|_raw.csv|.csv', '', x) for x in mv_series.columns]
mv_series.columns = [re.sub('_', ' ', x) for x in mv_series.columns]
mv_series.columns = [x.title() for x in mv_series.columns]

mv_series = mv_series.astype(float)
```

这段代码生成了以下数据集：

![](img/43edbf53cb2068dd53777e5d5e83ca07.png)

多变量时间序列样本

## 探索性数据分析

![](img/e4629c8b8d540b7861468340ee9b035d.png)

对数尺度的多变量时间序列图。为了可视化，系列被重新采样为每日频率。这是通过取每日平均值来完成的。图片由作者提供。

系列图表明有强烈的年度季节性。辐射水平在夏季达到峰值，其他变量也显示出类似的模式。除了季节性波动外，时间序列的水平在时间上是稳定的。

我们还可以单独可视化太阳辐射变量：

![](img/bd8d7456c63995ee1ee08293c91865c5.png)

每日总太阳辐射。图片由作者提供。

除了明显的季节性，我们还可以发现一些围绕系列水平的下降峰值。这些情况需要及时预测，以便高效利用备用能源系统。

我们还可以分析每对变量之间的相关性：

![](img/f2a4244cd0689ea4258ba2624a60acc1.png)

显示成对相关性的热图。图片由作者提供。

太阳辐射与一些变量相关。例如，气温、相对湿度（负相关）或风速。

我们已经探讨了如何使用单变量时间序列在上一篇文章中建立预测模型。然而，相关性热图表明，将这些变量纳入模型可能会很有价值。

我们该如何做到这一点？

## 自回归分布滞后建模入门

自回归分布滞后（ARDL）是一种用于多变量时间序列的建模技术。

ARDL 是一种识别多个变量随时间关系的有用方法。它通过将自回归技术扩展到多变量数据来工作。系列中给定变量的未来值是基于其滞后值和其他变量的滞后值来建模的。

在这种情况下，我们希望基于多个因素的滞后预测太阳辐射，如气温或蒸汽压。

## 将数据转换为 ARDL 格式

应用 ARDL 方法涉及将时间序列转换为表格格式。这是通过[应用时间延迟嵌入](https://medium.com/towards-data-science/machine-learning-for-forecasting-transformations-and-feature-extraction-bbbea9de0ac2)到每个变量，然后将结果连接成一个矩阵来完成的。可以使用以下函数来实现：

```py
import pandas as pd

def mts_to_tabular(data: pd.DataFrame,
                   n_lags: int,
                   horizon: int,
                   return_Xy: bool = False,
                   drop_na: bool = True):
    """
    Time delay embedding with multivariate time series
    Time series for supervised learning

    :param data: multivariate time series as pd.DataFrame
    :param n_lags: number of past values to used as explanatory variables
    :param horizon: how many values to forecast
    :param return_Xy: whether to return the lags split from future observations

    :return: pd.DataFrame with reconstructed time series
    """

    # applying time delay embedding to each variable
    data_list = [time_delay_embedding(data[col], n_lags, horizon)
                 for col in data]

    # concatenating the results in a single dataframe
    df = pd.concat(data_list, axis=1)

    if drop_na:
        df = df.dropna()

    if not return_Xy:
        return df

    is_future = df.columns.str.contains('\+')

    X = df.iloc[:, ~is_future]
    Y = df.iloc[:, is_future]

    if Y.shape[1] == 1:
        Y = Y.iloc[:, 0]

    return X, Y
```

这个函数应用于数据如下：

```py
from sklearn.model_selection import train_test_split

# target variable
TARGET = 'Solar Irradiance'
# number of lags for each variable
N_LAGS = 24
# forecasting horizon for solar irradiance
HORIZON = 48

# leaving the last 30% of observations for testing
train, test = train_test_split(mv_series, test_size=0.3, shuffle=False)

# transforming the time series into a tabular format
X_train, Y_train_all = mts_to_tabular(train, N_LAGS, HORIZON, return_Xy=True)
X_test, Y_test_all = mts_to_tabular(train, N_LAGS, HORIZON, return_Xy=True)

# subsetting the target variable
target_columns = Y_train_all.columns.str.contains(TARGET)
Y_train = Y_train_all.iloc[:, target_columns]
Y_test = Y_test_all.iloc[:, target_columns]
```

我们将预测视野设置为 48 小时。提前预测多个步骤对有效整合多种能源来源到电网中非常有价值。

很难事先确定应该包含多少个滞后。因此，每个变量的值设为 24。这导致总共有 216 个基于滞后的特征。

## 建立预测模型

在构建模型之前，我们基于日期和时间提取了 8 个额外特征。这些包括年份中的天数或小时等数据，这些数据对建模季节性很有用。

我们通过特征选择减少了解释变量的数量。首先，我们应用相关性过滤器，用于移除与任何其他解释变量相关性大于 95%的特征。然后，我们还基于随机森林的重要性分数应用递归特征消除（RFE）。在特征工程之后，我们使用随机森林训练模型。

我们利用 sklearn 的*Pipeline*和*RandomSearchCV*来优化不同步骤的参数：

```py
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import FunctionTransformer
from sklearn.feature_selection import RFE
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import RandomizedSearchCV
from sktime.transformations.series.date import DateTimeFeatures

from src.holdout import Holdout

# including datetime information to model seasonality
hourly_feats = DateTimeFeatures(ts_freq='H',
                                keep_original_columns=True,
                                feature_scope='efficient')

# building a pipeline
pipeline = Pipeline([
    # feature extraction based on datetime
    ('extraction', hourly_feats),
    # removing correlated explanatory variables
    ('correlation_filter', FunctionTransformer(func=correlation_filter)),
    # applying feature selection based on recursive feature elimination
    ('select', RFE(estimator=RandomForestRegressor(max_depth=5), step=3)),
    # building a random forest model for forecasting
    ('model', RandomForestRegressor())]
)

# parameter grid for optimization
param_grid = {
    'extraction': ['passthrough', hourly_feats],
    'select__n_features_to_select': np.linspace(start=.1, stop=1, num=10),
    'model__n_estimators': [100, 200]
}

# optimizing the pipeline with random search
model = RandomizedSearchCV(estimator=pipeline,
                           param_distributions=param_grid,
                           scoring='neg_mean_squared_error',
                           n_iter=25,
                           n_jobs=5,
                           refit=True,
                           verbose=2,
                           cv=Holdout(n=X_train.shape[0]),
                           random_state=123)

# running random search
model.fit(X_train, Y_train)

# checking the selected model
model.best_estimator_
# Pipeline(steps=[('extraction',
#                  DateTimeFeatures(feature_scope='efficient', ts_freq='H')),
#                 ('correlation_filter',
#                  FunctionTransformer(func=<function correlation_filter at 0x28cccfb50>)),
#                 ('select',
#                  RFE(estimator=RandomForestRegressor(max_depth=5),
#                      n_features_to_select=0.9, step=3)),
#                 ('model', RandomForestRegressor(n_estimators=200))])
```

## 评估模型

我们使用随机搜索选择了一个模型，并结合了[a validation split](https://medium.com/@vcerq/9-techniques-for-cross-validating-time-series-data-7828fc3f781d)。现在，我们可以评估其在测试集上的预测性能。

```py
# getting forecasts for the test set
forecasts = model.predict(X_test)
forecasts = pd.DataFrame(forecasts, columns=Y_test.columns)
```

选定的模型仅保留了原始 224 个解释变量中的 65 个。以下是前 20 个特征的重要性：

![](img/471bdbe1b1450e22f64699c66f950d52.png)

模型中每个特征的重要性。作者提供的图像。

特征中的一天小时和一年中的一天是排名前四的特征。这一结果突出了数据中季节性效应的强度。除了这些，一些变量的初始滞后期对模型也有用。

# 主要收获

+   太阳能是一种相关的清洁能源，依赖于太阳辐射；

+   预测太阳辐射是有效整合太阳能到电网中的一个重要方面；

+   太阳辐射依赖于许多变量。这些变量难以建模或可能无法轻易获得；

+   当这些变量可用时，它们表示一个多变量时间序列。这种类型的数据可以用 ARDL 技术建模；

+   你可以通过特征选择过程来估计每个变量的滞后期数量。

感谢阅读，下次故事见！

## 参考文献

[1] [来自美国爱达荷州西南部四个以西部刺柏为主的实验流域的天气、雪和径流数据。](https://data.nal.usda.gov/dataset/data-weather-snow-and-streamflow-data-four-western-juniper-dominated-experimental-catchments-south-western-idaho-usa) (许可证：美国公共领域)

[2] Rolnick, David, 等. “利用机器学习应对气候变化。” *ACM 计算调查 (CSUR)* 55.2 (2022): 1–96。
