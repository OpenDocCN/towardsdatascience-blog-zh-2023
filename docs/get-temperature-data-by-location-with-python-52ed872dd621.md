# 使用 Python 按位置获取气温数据

> 原文：[`towardsdatascience.com/get-temperature-data-by-location-with-python-52ed872dd621`](https://towardsdatascience.com/get-temperature-data-by-location-with-python-52ed872dd621)

## 按经纬度检索历史气温数据。

[](https://gustavorsantos.medium.com/?source=post_page-----52ed872dd621--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----52ed872dd621--------------------------------)[](https://towardsdatascience.com/?source=post_page-----52ed872dd621--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----52ed872dd621--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----52ed872dd621--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----52ed872dd621--------------------------------) ·阅读时长 3 分钟·2023 年 5 月 18 日

--

![](img/0a6e05461c10ccab94bd840c64e8f53b.png)

[Tom Barrett](https://unsplash.com/es/@wistomsin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/photos/hgGplX3PFBg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上的照片

# 介绍

我一直在处理一些项目，需要按位置找到平均气温，以便将其作为可能的变量添加到我的模型中。在经过一段时间的研究后，我发现了一个非常棒且易于使用的 Python 包，叫做`meteostat`。

这个包有一些有趣的方法，其中一个正是我需要的：如果我输入经纬度，它可以检索该地点的历史气温。此外，你还可以按时间范围请求数据。所以，你可以获取一年的数据或多年的数据。

另一个很酷的功能是，你可以请求你的数据为每日或每月数据。太棒了！

让我们看看代码是如何工作的。

# 代码

首先，让我们使用`pip install meteostat`安装 meteostat。接下来，我们可以导入所需的包。

```py
# Import packages
import pandas as pd
from datetime import datetime
import matplotlib.pyplot as plt
from meteostat import Point, Monthly
```

如果你访问[文档](https://dev.meteostat.net/python/#installation)，你会看到这段*入门*代码，这基本上是你需要的工作内容。我们设置了一个时间范围——我用了 2022 年。然后我们创建一个变量来保存经纬度信息。我将使用这个`Point(40.78325, -73.96565)`，它是纽约市中央公园的经纬度。最后，我们会请求在选定日期之间的`Monthly`数据，然后获取这些数据。

```py
# Set time period
start = datetime(2022, 1, 1)
end = datetime(2022, 12, 31)

# Create Point for New York, NY
tpr = Point(40.78325, -73.96565)

# Get monthly data for 2022
data = Monthly(tpr, start, end)
data = data.fetch()
```

这就是你需要做的基本内容。以下是获取的数据的样子。

![](img/009591c47bee0d91566667749cb62c7a.png)

2022 年纽约市——曼哈顿的气温（摄氏度）。作者提供的图片。

如果你需要每日数据，可以导入`Daily`而不是`Monthly`，并在代码中进行替换。

你也可以使用 Pandas 轻松绘制数据。

```py
# Plot line chart including average, minimum and maximum temperature
data.plot(y=['tavg', 'tmin', 'tmax'], 
          title='Temperature 2022 - Central Park, NYC')
plt.show()
```

代码生成的图如下所示。

![](img/cf425e9c7803c7f920bf099f1560f4f5.png)

温度图。图片由作者提供。

如果我们想查看提供这些结果的站点，以下是代码。

```py
# Import Meteostat library
from meteostat import Stations

# Get nearby weather stations from our interest point
stations = Stations()
stations = stations.nearby(40.78325, -73.96565)
# Fetching 5 stations
station = stations.fetch(5)

# Print DataFrame
pd.DataFrame(station)
```

![](img/98321a2ad9a823de323be79feac07149.png)

5 个位于中央公园附近的气象站。图片由作者提供。

# 离开之前

就这些了。一个快速但有用的教程。

GitHub 仓库：

[](https://github.com/gurezende/Studying/tree/master/Python/meteostat?source=post_page-----52ed872dd621--------------------------------) [## Studying/Python/meteostat at master · gurezende/Studying

### 目前你无法执行该操作。你在另一个标签页或窗口中登录了。你在另一个标签页或窗口中登出了…

[github.com](https://github.com/gurezende/Studying/tree/master/Python/meteostat?source=post_page-----52ed872dd621--------------------------------)

如果你需要用于项目的温度数据，我相信`meteostat`可以帮助你。查看他们的文档获取更多信息。

如果你喜欢这个内容，记得关注我以获取更多信息。

[](https://gustavorsantos.medium.com/?source=post_page-----52ed872dd621--------------------------------) [## Gustavo Santos - Medium

### 在 Medium 上阅读 Gustavo Santos 的文章。数据科学家。我从数据中提取见解，以帮助个人和公司…

[gustavorsantos.medium.com](https://gustavorsantos.medium.com/?source=post_page-----52ed872dd621--------------------------------)

另外，可以在 [LinkedIn](https://www.linkedin.com/in/gurezende/) 上找到我，或在 [topmate](https://topmate.io/gustavo_santos) 上预约时间与我讨论数据科学。

# 参考

[## Python 库 | Meteostat 开发者

### Meteostat Python 库提供了通过 Pandas 简单访问开放天气和气候数据的功能。历史数据…

[dev.meteostat.net](https://dev.meteostat.net/python/?source=post_page-----52ed872dd621--------------------------------#installation)
