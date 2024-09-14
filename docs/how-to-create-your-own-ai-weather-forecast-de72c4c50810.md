# 如何创建自己的 AI 天气预报

> 原文：[`towardsdatascience.com/how-to-create-your-own-ai-weather-forecast-de72c4c50810`](https://towardsdatascience.com/how-to-create-your-own-ai-weather-forecast-de72c4c50810)

## 使用预训练模型和再分析数据

[](https://medium.com/@caroline.arnold_63207?source=post_page-----de72c4c50810--------------------------------)![Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----de72c4c50810--------------------------------)[](https://towardsdatascience.com/?source=post_page-----de72c4c50810--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----de72c4c50810--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----de72c4c50810--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----de72c4c50810--------------------------------) ·8 分钟阅读·2023 年 10 月 2 日

--

![](img/f4097a4d9988202f559adae00eed3172.png)

图片由 [NOAA](https://unsplash.com/@noaa?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

基于数据的天气预报使用预训练模型进行创建成本低，并且能够提供与传统数值天气模型相当的预报准确度。多个公司和研究实验室已经开发了 AI 天气模型，包括：

+   PanguWeather [[华为]](https://github.com/198808xc/Pangu-Weather)

+   FourCastNet [[NVIDIA]](https://github.com/NVlabs/FourCastNet)

+   GraphCast [[Google DeepMind](https://github.com/google-deepmind/graphcast)]

欧洲中期天气预报中心（ECMWF）提供了使用这些模型生成天气预报的常规方法[1]。即使在笔记本电脑上也可以进行模型推断，尽管推荐使用 GPU。

在这篇文章中，我将要

+   向你展示如何创建自己的 AI 天气预报

+   比较模型在 GPU 和 CPU 上的推断时间

+   可视化 PanguWeather 和 FourCastNet 的天气预报，包括温度、水汽和急流

## 背景信息

传统的天气预报依赖于在全球网格上求解的数值天气模型。这需要大量的计算资源，只有少数天气服务机构能够生成全球天气预报。

AI 天气模型是基于过去的天气进行训练的，称为*再分析数据*。通过训练好的模型和当前的天气状态，可以只使用少量计算资源生成天气预报，而不需要像数值天气预报那样的高计算需求。示意图展示了 AI 天气预测如何利用当前天气作为输入来预测未来天气：

![](img/ac9aa344428b8687741641bd63a5b02e.png)

AI 气象预报示意图。左侧：2021 年 1 月 1 日 00:00 UTC 的风场。右侧：2021 年 1 月 1 日 23:00 UTC 的风场。数据来源：ERA5，通过 Copernicus 气候数据存储 [3]。图片：作者。

本文涵盖的三种 AI 气象模型均提供 0.25°（25 公里）的空间分辨率和 6 小时的时间分辨率的天气预报。

有关 AI 在天气预报中影响的更多背景信息，请参阅我之前的文章：

[## 气象预测中的人工智能革命](https://pub.towardsai.net/the-ai-revolution-in-weather-prediction-9b706f763a61?source=post_page-----de72c4c50810--------------------------------)

### 明天的天气，由深度学习提供

pub.towardsai.net](https://pub.towardsai.net/the-ai-revolution-in-weather-prediction-9b706f763a61?source=post_page-----de72c4c50810--------------------------------)

## 入门

**代码和环境** 从 ECMWF Github 页面 [1] 获取运行预训练气象模型的代码，并按照安装说明进行操作。我在本教程中使用了 v0.2.5。为所需的 Python 包设置一个 `conda` 环境。

```py
conda create -n ai-models python=3.10
conda activate ai-models
conda install cudatoolkit
pip install ai-models
```

要安装可用的 AI 模型，请使用

```py
pip install ai-models-panguweather
pip install ai-models-fourcastnet
```

GraphCast 需要特殊的安装程序，下面会介绍。

**预训练模型权重** PanguWeather、GraphCast 和 FourCastNet 的预训练模型权重需要下载：

```py
ai-models --download-assets --assets assets-panguweather panguweather
ai-models --download-assets --assets assets-fourcastnet  fourcastnet
ai-models --download-assets --assets assets-graphcast    graphcast
```

**初始化数据** AI 气象模型需要用当前的天气数据进行初始化才能进行预测。有两种获取数据的方式，要么从 ECMWF 服务 MARS 获取，要么从 Copernicus 气候数据存储 (CDS) 获取。这两个服务都免费提供初始化数据供非商业使用。

我在本教程中使用了来自 CDS 的再分析数据。请按照他们网站上的说明获取访问数据的 API 密钥 [2]。请注意，再分析数据的提供有大约五天的延迟。

## 创建 AI 气象预报

现在我们已经准备好生成自己的 AI 气象预报。剩下的工作就是选择初始化的时间和日期，然后使用预训练模型开始模型推理。

**PanguWeather** 要请求基于 CDS 初始化数据的从 2023 年 9 月 20 日 00:00 UTC 开始的 PanguWeather 预报，请使用：

```py
ai-models --input cds --date 20230920 --time 0000 --assets assets-panguweather panguweather
```

日志相当全面。预训练模型 `pangu_weather_6.onnx` 生成的天气预报提前 6 小时，这里它被迭代应用了 40 次，总计 240 小时（10 天）。

```py
2023-09-26 12:06:45,724 INFO Loading assets-panguweather/pangu_weather_24.onnx: 26 seconds.
2023-09-26 12:07:10,289 INFO Loading assets-panguweather/pangu_weather_6.onnx: 24 seconds.
2023-09-26 12:07:10,289 INFO Model initialisation: 52 seconds
2023-09-26 12:07:10,290 INFO Starting inference for 40 steps (240h).
```

对于每次迭代，日志会告诉用户花费了多少时间。在我的实验中，ONNX 运行时无法找到 GPU，因此推理是在 CPU 上进行的，每个天气预测步骤大约花费 3:15 分钟。

```py
2023-09-26 12:10:27,260 INFO Done 1 out of 40 in 3 minutes 16 seconds (6h), ETA: 2 hours 11 minutes 18 seconds.
```

**FourCastNet** 为了生成与相同初始化时间的预报，同时下载训练好的模型权重到子目录 `assets-fourcastnet`，我们使用：

```py
ai-models --download-assets --assets assets-fourcastnet --input cds --date 20230920 --time 0000 fourcastnet
```

日志再次提供了关于模型推断的全面信息。现在，ONNX 运行时能够找到 GPU，推断速度大大加快，仅需 2 分钟即可生成 10 天的预报。

```py
2023-09-26 13:23:48,633 INFO Using device 'CUDA'. The speed of inference depends greatly on the device.
2023-09-26 13:23:59,710 INFO Loading assets-fourcastnet/backbone.ckpt: 13 seconds.
2023-09-26 13:24:10,673 INFO Loading assets-fourcastnet/precip.ckpt: 10 seconds.
2023-09-26 13:24:10,733 INFO Model initialisation: 46 seconds
2023-09-26 13:24:10,733 INFO Starting inference for 40 steps (240h).
2023-09-26 13:24:14,247 INFO Done 1 out of 40 in 3 seconds (6h), ETA: 2 minutes 20 seconds.
```

**GraphCast** 对于 GraphCast，我们首先需要按照 [`github.com/ecmwf-lab/ai-models-graphcast`](https://github.com/ecmwf-lab/ai-models-graphcast) 中的安装步骤进行操作。然后，我们可以使用以下方法生成天气预报：

```py
ai-models --download-assets --assets assets-graphcast --input cds --date 20230920 --time 0000 graphcast
```

在撰写时，这导致了一个在 GitHub 上存在的错误（[问题 #10](https://github.com/ecmwf-lab/ai-models-graphcast)），我无法使用 GraphCast 完成预报。

## 预报输出文件

AI 气象模型生成的输出文件采用 GRIB 格式。对于 240 小时的预报，文件大小为

+   使用 FourCastNet（3 个压力层：850 hPa、500 hPa、250 hPa）时，文件大小为 2.3 GB。

+   使用 PanguWeather（13 个压力层）时，文件大小为 5.4 GB。

两个模型输出关键气象变量：

+   温度

+   风速和风向

PanguWeather 还提供了地势和湿度数据，而 FourCastNet 提供了水汽含量。

## 生成图表

对这些 AI 生成的预报进行适当的评估和基准测试是一个复杂的话题，超出了本文的范围。在这里，我们重点比较了两个 AI 气象模型的视觉效果。为了视觉分析天气预报，我们选择了温度变量以及初始化后 24 小时的预报。这是生成图表的模板：

```py
import xarray as xr
import seaborn as sns
from matplotlib import pyplot as plt
import cartopy.crs as ccrs

# load data
ds_fourcast = xr.open_dataset('fourcastnet.grib')

ix_step = 3  # 0 = first step in weather prediction
ix_level = 0 # 0 = surface pressure level
temp_degC = ds_fourcast['t'][ix_step, ix_level] - 273.15 # subtract 273.15 to convert from Kelvin to Celsius.

# plot settings
sns.set_style('ticks')
tmin = -45
tmax = +45
nlev = 19

# generate figure
fig = plt.figure()
ax = fig.add_subplot(1, 1, 1, projection=ccrs.PlateCarree())
ax.set_global()
ax.coastlines('110m', alpha=0.1)

# contour plot
img = ax.contourf(ds_fourcast.longitude.values, ds_fourcast.latitude.values, 
                  temp_degC.values, 
                  levels = np.linspace(tmin, tmax, nlev),
                  cmap='RdBu_r',
                  transform=ccrs.PlateCarree()
                 )

# hours passed since initialization
dt_hours = ((ds_fourcast['valid_time'][ix_step] - ds_fourcast['time']).values / 1e9 / 3600).astype(float)

# annotation text
ax.text(-180, 90, 
        f'After {dt_hours} hours ({ds_fourcast["valid_time"][ix_step].values})', 
        transform=ccrs.PlateCarree(),
        va='bottom', ha='left',
       )

plt.colorbar(img, orientation='horizontal', label='850 hPa temperature (°C)')
plt.show()
```

**温度** 下图比较了 PanguWeather 和 FourCastNet 在 850 hPa 层的温度。这个层级并不直接位于地表，但它是两个输出文件中唯一包含的层级。从视觉上看，我们可以看到温度场显示了相似的模式。

![](img/c14d4da848ac6978ab1d8b221277f9e3.png)

24 小时后的 850 hPa 温度。图片来源：作者。

**水汽** FourCastNet 提供了总大气水汽质量，这可以与带有雨滴的云量进行比较。在下图中，我们比较了四个不同时间点的这一量：1 天（24 小时）、3 天（72 小时）、5 天（96 小时）和 10 天（240 小时）后。

![](img/36605e16dfcc88f25dc545bab18b46b5.png)

使用 FourCastNet 预报 1、3、5 和 10 天后的总大气水汽。图片来源：作者。

我们观察到一个在深度学习中非常常见的现象：由于模型使用均方根误差（RMSE）进行训练，它倾向于生成平滑的场。随着模型的迭代应用，预测结果变得不那么明显。经过 10 天，我们甚至观察到南美洲南端的伪影。请注意，目前的 AI 气象模型并不使用生成式 AI 技术，因为它们更注重物理准确性而非视觉效果。

**风速** PanguWeather 提供不同压力水平的风速。我们选择了 250 hPa 压力水平，这对应于喷流最为明显的高度。喷流是一种极地环流，影响中纬度地区如美国和欧洲的天气。图中显示了南半球喷流的风速，该喷流在我的预报期间更加显著。

![](img/d4b4733fe19ed791c2fcc2d9bfcaacce.png)

南半球喷流预报使用 PanguWeather。图片：作者。

为了创建正射投影并计算风速，我们使用了以下模板：

```py
import xarray as xr
import seaborn as sns
from matplotlib import pyplot as plt
import cartopy.crs as ccrs

# load data
#ds_pangu = xr.open_dataset('/home/k/k202141/rootgit/ai-models/panguweather.grib')

ix_step = 3  # 0 = first step in weather prediction
ix_level = 8 # 0 = surface pressure level, 2 - 850 hPa level
windspeed = (ds_pangu['u'][ix_step, ix_level]**2 + ds_pangu['u'][ix_step, ix_level]**2)**0.5

# plot settings
sns.set_style('ticks')
tmin = 0
tmax = 150
nlev = 16

# generate figure
fig = plt.figure()
ax = fig.add_subplot(1, 1, 1, projection=ccrs.Orthographic(central_latitude=-30))
ax.set_global()
ax.coastlines('110m', alpha=1.0)

# contour plot
img = ax.contourf(ds_pangu.longitude.values, ds_pangu.latitude.values, 
                  windspeed.values, 
                  levels = np.linspace(tmin, tmax, nlev),
                  cmap='gist_ncar',
                  transform=ccrs.PlateCarree()
                 )

# hours passed since initialization
dt_hours = ((ds_pangu['valid_time'][ix_step] - ds_pangu['time']).values / 1e9 / 3600).astype(float)
print(dt_hours)

plt.colorbar(img, orientation='horizontal', label='Wind speed (m/s)', shrink=0.5)
plt.show()
```

## 摘要

在本文中，我们展示了如何使用 ECMWF 提供的 AI-Models 资源库[1]生成 AI 天气预报。通过使用气候数据存储中的初始化数据，我们能够使用两个模型生成预报：华为的 PanguWeather 和 NVIDIA 的 FourCastNet。使用预训练模型生成 10 天的天气预报，在 GPU 上大约需要 2 分钟，在 CPU 上大约需要 3 小时。与生成数值天气预报所需的巨大计算努力相比，推理成本非常低。

现在，个体消费者和资金不足的天气服务可以生成自己的 AI 天气预报，至少可以使用 ERA5 训练数据提供的空间和时间分辨率。ECMWF 的资源库提供了对预训练模型的便捷访问。

## 参考文献

+   [1] [`github.com/ecmwf-lab/ai-models`](https://github.com/ecmwf-lab/ai-models)（版本 0.2.5）

+   [2] Copernicus 气候数据存储 API: [`cds.climate.copernicus.eu/api-how-to`](https://cds.climate.copernicus.eu/api-how-to)

+   [3] Hersbach, H., et al (2017): Complete ERA5 from 1940: Fifth generation of ECMWF atmospheric reanalyses of the global climate. Copernicus Climate Change Service (C3S) Data Store (CDS). DOI: [10.24381/cds.143582cf](https://doi.org/10.24381/cds.143582cf) (访问日期：2023 年 9 月 26 日)
