# 气候变化时间序列：极端天气事件预测

> 原文：[`towardsdatascience.com/times-series-for-climate-change-forecasting-extreme-weather-events-335dc199fb6f`](https://towardsdatascience.com/times-series-for-climate-change-forecasting-extreme-weather-events-335dc199fb6f)

## 如何利用时间序列分析和预测应对气候变化

[](https://vcerq.medium.com/?source=post_page-----335dc199fb6f--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----335dc199fb6f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----335dc199fb6f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----335dc199fb6f--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----335dc199fb6f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----335dc199fb6f--------------------------------) ·阅读时间 7 分钟·2023 年 5 月 15 日

--

![](img/0d01fb5fec458a97752c2df8d11c5c87.png)

图片由[DESIGNECOLOGIST](https://unsplash.com/@designecologist?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是《*气候变化时间序列*》系列的第五部分。文章列表：

+   第一部分: 风力预测

+   第二部分: 太阳辐射预测

+   第三部分: [大洋波浪预测](https://medium.com/towards-data-science/time-series-for-climate-change-forecasting-large-ocean-waves-78484536be36)

+   第四部分: [能源需求预测](https://medium.com/towards-data-science/time-series-for-climate-change-forecasting-energy-demand-79f39c24c85e)

# 极端天气事件

气候变化导致极端天气事件数量不断增加。这些事件对人类生命和基础设施构成了重大风险。

![](img/31d70187d859e91c3632d05ed9966c6f.png)

美国每年的冰雹和雷暴风事件数量。[数据来源](https://www.ncdc.noaa.gov/stormevents/ftp.jsp)。图片由作者提供。

极端天气事件可能导致人员伤亡，并且成本高达数十亿美元。这些财务影响源于基础设施或农业资源的破坏。这些事件对受影响地区的社会经济发展有持久的影响。

![](img/502ddf62473f370d0f9ca5cd57ce911b.png)

各种极端天气事件的年度估算成本。[数据来源](https://www.ncdc.noaa.gov/stormevents/ftp.jsp)。图片由作者提供。

极端天气事件包括多种天气现象。这些现象包括飓风、龙卷风、洪水、干旱、冰雹和热带风暴。下图显示了自 1950 年以来美国 20 种最常见的事件类型。

![](img/526dd1b633fe38849b6fb47acb5f435f.png)

自 1950 年以来，美国 20 种最常见的极端天气事件类型。[数据来源](https://www.ncdc.noaa.gov/stormevents/ftp.jsp)。图片作者提供。

## 准确预测的需求

准确的极端天气事件预测在我们适应气候变化影响中发挥了关键作用。

及时发布警报可以让人们撤离或保护自己及其资产。应急团队可以更有效地调动资源，减少事件的影响。

然而，预测极端天气事件是一项困难的任务。

一个问题是数据访问。极端天气事件是大气、海洋和陆地许多因素相互作用的结果。这些因素可能难以建模，或者可能不易获得。此外，极端天气事件本质上是稀有的。机器学习模型往往在数据集分布不平衡的情况下表现不佳——[即，当某个类别稀少时](https://medium.com/towards-data-science/how-to-tackle-class-imbalance-without-resampling-47bbeb2180aa)。

另一个困难是天气条件可能迅速变化。这意味着对未来有很大的不确定性。预测模型必须传达不确定性以便有效沟通。这个方面对政策制定者和公众来说都很重要。

应对这些挑战是减缓气候变化影响的关键一步。

# 实践：预测佛罗里达的极端天气事件

在本文的其余部分，我们将建立一个模型来预测美国佛罗里达的冰雹和雷暴事件。冰雹是由雷暴引起的冰冻降水。

本教程中使用的完整代码可以在 GitHub 上找到：

+   [`github.com/vcerqueira/tsa4climate`](https://github.com/vcerqueira/tsa4climate)

## 数据集

我们使用 NOAA 收集的数据集，描述了自 1950 年以来发生在美国的风暴事件[1]。数据包括以下信息：

+   事件类型（例如冰雹、龙卷风）；

+   位置（坐标及相应州）；

+   日期和时间；

+   估计的成本。

这里是关于佛罗里达风暴事件的数据样本：

![](img/448535fbcd977ea79e47fc05f9fa042b.png)

风暴事件数据集的样本。图片作者提供。

风暴检测和追踪通常使用基于遥感的方法。但我们也可以基于气象数据建模极端天气事件。我们将使用由智能浮标捕获的气象数据来预测即将发生的冰雹或雷暴事件。这些数据也是由 NOAA 收集的[2]。

![](img/69552476341b7aa3ceffff39024ffd71.png)

几个智能浮标被安置在佛罗里达海岸。截图来自[这里](https://www.ndbc.noaa.gov/obs.shtml)。

## 读取数据

我们可以直接从 NOAA 读取浮标数据，如下所示：

```py
import re

import urllib3
import numpy as np
import pandas as pd

# Subset of stations on the coast of Florida
STATION_LIST = ['41009', 'SPGF1', 'VENF1',
                '42036', 'SAUF1', 'FWYF1',
                'LONF1', 'SMKF1']

# URL template
URL = 'https://www.ndbc.noaa.gov/view_text_file.php?filename={station_id}h{year}.txt.gz&dir=data/historical/stdmet/'

def read_buoy_remote(station_id: str, year: int):
    TIME_COLUMNS = ['YYYY', 'MM', 'DD', 'hh']

    # formatting the URL
    file_url = URL.format(station_id=station_id.lower(), year=year)

    http = urllib3.PoolManager()

    # get request
    response = http.request('GET', file_url)

    # decoding
    lines = response.data.decode().split('\n')

    # lots of data cleaning below
    data_list = []
    for line in lines:
        line = re.sub('\s+', ' ', line).strip()
        if line == '':
            continue
        line_data = line.split(' ')
        data_list.append(line_data)

    df = pd.DataFrame(data_list[2:], columns=data_list[0]).astype(float)
    df[(df == 99.0) | (df == 999.0)] = np.nan

    if 'BAR' in df.columns:
        df = df.rename({'BAR': 'PRES'}, axis=1)

    if '#YY' in df.columns:
        df = df.rename({'#YY': 'YYYY'}, axis=1)

    if 'mm' in df.columns:
        TIME_COLUMNS += ['mm']

    df[TIME_COLUMNS] = df[TIME_COLUMNS].astype(int)

    if 'mm' in df.columns:
        df['datetime'] = \
            pd.to_datetime([f'{year}/{month}/{day} {hour}:{minute}'
                            for year, month, day, hour, minute in zip(df['YYYY'],
                                                                      df['MM'],
                                                                      df['DD'],
                                                                      df['hh'],
                                                                      df['mm'])])

    else:
        df['datetime'] = \
            pd.to_datetime([f'{year}/{month}/{day} {hour}:00'
                            for year, month, day, hour in zip(df['YYYY'],
                                                              df['MM'],
                                                              df['DD'],
                                                              df['hh'])])

    df = df.drop(TIME_COLUMNS, axis=1)

    df.set_index('datetime', inplace=True)

    return df
```

我们从 8 个浮标中收集了以下变量：

+   风速；

+   波高；

+   大气压力；

+   水温；

+   平均波周期。

这是一个浮标提供的数据样本：

![](img/f6939a36d3b3f297105b3e3adef45000.png)

来自智能浮标的气象数据样本。图片由作者提供。

## 建立数据集

我们需要合并这两个数据源以创建用于训练预测模型的数据集。

在每个时间步，我们从浮标获取最近的气象数据值。这些数据用作解释变量。目标变量是一个二元变量，表示在接下来的 12 小时内是否会发生冰雹或雷暴事件。

```py
import numpy as np
import pandas as pd

from config import ASSETS, OUTPUTS

PART = 'Part 5'
assets = ASSETS[PART]

# focusing on hail and thunderstorm events
TARGET_EVENTS = ['Hail', 'Thunderstorm Wind']
# wind speed, wave height, pressure, water temp, avg wave period
METEOROLOGICAL_DATA = ['WSPD', 'WVHT', 'PRES', 'WTMP', 'APD']

# using past 4 hours as explanatory variables
N_LAGS = 4
# forecasting events in the next 12 hours
HORIZON = 12

# loading storm events data
storms = pd.read_csv(f'{assets}/storms_data.csv', index_col='storm_start')
storms.index = pd.to_datetime(storms.index)
hail_df = storms.loc[storms['EVENT_TYPE'].isin(TARGET_EVENTS), :]

# loading the meteorological data
buoys = pd.read_csv(f'{assets}/buoys.csv', index_col='datetime')
buoys.index = pd.to_datetime(buoys.index).tz_localize('UTC')
buoys['STATION'] = buoys['STATION'].astype(str)
# resampling the data to an hourly granularity
buoys_h = buoys.groupby('STATION').resample('H').mean()
# subsetting the variables
buoys_h = buoys_h[METEOROLOGICAL_DATA]
buoys_df = buoys_h.reset_index('STATION')

# getting all unique time steps
base_index = buoys_df.index.unique()

# getting list of stations
station_list = buoys_df['STATION'].unique().tolist()

X, y = [], []
# iterating over each time step
for i, dt in enumerate(base_index[N_LAGS + 1:]):

    features_by_station = []
    # iterating over each buoy station
    for station_id in station_list:

        # subsetting the data by station and time step (last n_lags observations)
        station_df = buoys_df.loc[buoys_df['STATION'] == station_id]
        station_df = station_df.drop('STATION', axis=1)
        station_df_i = station_df[:dt].tail(N_LAGS)

        if station_df_i.shape[0] < N_LAGS:
            break

        # transforming lags into features
        station_timestep_values = []
        for col in station_df_i:
            series = station_df_i[col]
            series.index = [f'{station_id}({series.name})-{i}'
                            for i in list(range(N_LAGS, 0, -1))]

            station_timestep_values.append(series)

        station_values = pd.concat(station_timestep_values, axis=0)

        features_by_station.append(station_values)

    if len(features_by_station) < 1:
        continue

    # combining features from all stations
    feature_set_i = pd.concat(features_by_station, axis=0)

    X.append(feature_set_i)

    # determining the target variable
    # whether an extreme weather events in the next HORIZON hours
    td = (hail_df.index - dt)
    td_hours = td / np.timedelta64(1, 'h')
    any_event_within = pd.Series(td_hours).between(0, HORIZON)

    y.append(any_event_within.any())

# combining all data points
X = pd.concat(X, axis=1).T
y = pd.Series(y).astype(int)
```

## 建立模型

从机器学习的角度来看，预测任务是一个稀有事件二元分类问题。此外，我们需要一个概率模型来有效传达不确定性。

我们将使用 LightGBM 算法来建立模型。其参数可以使用 *Optuna* 优化：

```py
import optuna

import lightgbm as lgb
from sklearn.metrics import roc_auc_score
from sklearn.model_selection import train_test_split

# objective function for optuna
def objective(trial, X, y):

    train_x, valid_x, train_y, valid_y = \
        train_test_split(X, y, test_size=0.2, shuffle=False)

    dtrain = lgb.Dataset(train_x, label=train_y)

    param = {
        'objective': 'binary',
        'metric': 'binary_logloss',
        'verbosity': -1,
        'boosting_type': 'gbdt',
        'linear_tree': True,
        'lambda_l1': trial.suggest_float('lambda_l1', 1e-8, 10.0, log=True),
        'lambda_l2': trial.suggest_float('lambda_l2', 1e-8, 10.0, log=True),
        'num_leaves': trial.suggest_int('num_leaves', 2, 256),
        'feature_fraction': trial.suggest_float('feature_fraction', 0.4, 1.0),
        'bagging_fraction': trial.suggest_float('bagging_fraction', 0.4, 1.0),
        'bagging_freq': trial.suggest_int('bagging_freq', 1, 7),
        'min_child_samples': trial.suggest_int('min_child_samples', 5, 100),
    }

    gbm = lgb.train(param, dtrain)
    preds = gbm.predict(valid_x)

    # optimizing for AUC
    auc = roc_auc_score(valid_y, preds)

    return auc

def optimize_params(X, y, n_trials: int):
    func = lambda trial: objective(trial, X, y)

    # auc should be maximized
    study = optuna.create_study(direction='maximize')
    study.optimize(func, n_trials=n_trials)

    # getting best parameter setup
    trial = study.best_trial

    return trial.params

# train/test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, shuffle=False)

# optimization
params = optimize_params(X_train, y_train, n_trials=100)

# retraining with best parameters
dtrain = lgb.Dataset(X_train, label=y_train)
gbm = lgb.train(params, dtrain)

# inference
preds = gbm.predict(X_test)
```

这导致 AUC 分数为 0.78。

![](img/c13dab402e3faaca90278d74f4d040fc.png)

模型的 ROC 曲线，AUC 为 0.78。图片由作者提供。

AUC 分数表明我们可以基于浮标数据检测极端天气事件。我们可以做几件事来改进这个模型：

+   更好地选择浮标。我选择了一些靠近海岸但彼此距离较远的浮标。然而，不同的浮标组合可能会更好；

+   额外的气象变量。我们使用了从每个浮标捕获的 5 个变量，但其他变量也可能有价值。例如，来自卫星的云层覆盖信息。

# 主要收获

+   极端天气事件对人类生命和基础设施构成了重大风险；

+   气候变化与极端天气事件的增加有关。因此，预测极端天气事件对我们适应气候变化至关重要；

+   我们可以基于智能浮标收集的气象数据来建模事件的发生；

+   基于 LightGBM 的概率分类器可以以不错的 AUC 分数检测极端天气事件；

+   更好的特征工程过程可能有助于改进这个模型。

感谢阅读，我们下个故事见！

## 参考文献

[1] 风暴事件数据库，数据来源于 [`www.ncdc.noaa.gov/stormevents/ftp.jsp`](https://www.ncdc.noaa.gov/stormevents/ftp.jsp) (许可：公有领域)

[2] 国家数据浮标中心，数据来源于 [`www.ndbc.noaa.gov/`](https://www.ndbc.noaa.gov/) (许可：公有领域)

[3] McGovern, Amy, 等. “利用人工智能改善高影响天气的实时决策。” *美国气象学会公报* 98.10 (2017): 2073–2090。

[4] Ramachandra, Vikas. “利用浮标数据和机器学习预测天气事件的严重性。” *arXiv 预印本 arXiv:1911.09001* (2019)。
