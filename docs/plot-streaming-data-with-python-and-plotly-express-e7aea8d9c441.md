# 使用 Python 和 Plotly Express 绘制流数据

> 原文：[`towardsdatascience.com/plot-streaming-data-with-python-and-plotly-express-e7aea8d9c441`](https://towardsdatascience.com/plot-streaming-data-with-python-and-plotly-express-e7aea8d9c441)

## 实时跟踪国际空间站

[](https://medium.com/@lee_vaughan?source=post_page-----e7aea8d9c441--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----e7aea8d9c441--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7aea8d9c441--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7aea8d9c441--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----e7aea8d9c441--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7aea8d9c441--------------------------------) ·8 分钟阅读·2023 年 3 月 26 日

--

![](img/962b0ecd54ae50e1fb81942915e0c069.png)

国际空间站（图片来自 [NASA 图片库](https://images.nasa.gov/)）

*流数据* 指的是实时数据，它不断地从源头流向目标。它包括音频、视频、文本或由社交媒体平台、传感器和服务器等来源生成的数值数据。数据传输以稳定的流形式进行，没有固定的开始或结束。流数据在医疗保健、金融和运输等领域非常重要，并且它是 [*物联网 (IoT)*](https://www.networkworld.com/article/3207535/what-is-iot-the-internet-of-things-explained.html) 的关键组成部分。

处理流数据的能力是数据科学家的重要技能。在这个 *快速成功数据科学* 项目中，我们将使用流数据来跟踪国际空间站（ISS）绕地球轨道运行。我们将使用 Python 和 Plotly Express 在 Jupyter Notebook 中进行编码。

# 国际空间站遥测

*遥测* 是 *原位* 收集和自动传输远程传感器数据到接收设备以进行监测。虽然有众多国际空间站的遥测数据来源，我们将使用 [*WTIA REST API*](https://wheretheiss.at/w/developer)（WTIA 代表 *国际空间站在哪里？*）。

这个 API 由 Bill Shupp 编写，包含了比典型的国际空间站跟踪/通知网站提供的更多功能。文章末尾，我将列出一些额外的国际空间站遥测和跟踪来源，以备你想尝试或将我们的结果与他们的结果进行比较。

# Plotly Express 库

[*Plotly Express*](https://plotly.com/python/plotly-express/) 是 Plotly 图形库的内置部分。作为 Plotly 的简化、高级版本，它是创建大多数常见图形的推荐起点。

Plotly Express 包含 30 多个用于一次性创建整个图形的函数，这些函数的 API 设计得非常一致且易于学习。这使得在数据探索过程中可以轻松切换图形类型。

虽然 Plotly Express 易于使用且能创建美观的交互图，但它们的自定义程度不如使用 Plotly 或 matplotlib 等低级库生成的图表。像往常一样，你必须为了易用性而放弃一些控制权。

安装 Plotly 可以访问 Plotly Express。你可以使用`pip`安装它：

`pip install plotly==5.13.1`

或 conda：

`conda install -c plotly plotly=5.13.1`

请注意，版本号会随着时间变化，因此请务必检查 Plotly 的[文档](https://plotly.com/python/getting-started/#installation)，其中还包括对经典 Jupyter Notebook 和 JupyterLab 的支持（如有需要）。此外，你可以在[这里](https://pypi.org/project/plotly-express/)找到 Plotly Express 的*PyPi*页面。

# 过程

数据流并不是真正连续的。网站更新的频率是*某个*频率，比如每秒一次。同样，我们不能比数据生成的速度更快地获取数据，也不应该尝试尽可能快地获取。如果你频繁地 ping 一个网站，它可能会认为受到攻击并阻止你的访问。因此，在可能的情况下，我们应该以每 5 或 10 秒的礼貌速率请求数据。

在本项目中，我们将*逐步*处理流数据。以下是工作流程：

1.  从网站获取数据。

1.  将其放入 pandas DataFrame 中。

1.  使用 Plotly Express 绘制图表。

1.  清除图表。

1.  重复。

# 导入库和分配常量

除了 Plotly Express 外，我们还需要`time`模块来控制访问流数据的频率；requests 库用于从 URL 获取数据；pandas 库用于准备数据以供绘图；以及`IPython.display`，用于在发布更新的 ISS 位置之前清除图表。

我们将使用名为`ISS_URL`的常量来存储 WTIA REST API 的 URL。由于流数据永无止境，我们将分配另一个常量`ORBIT_TIME_SECS`，帮助我们确定何时结束程序。该常量代表一个完整的 ISS 绕地球运行的时间（约 92 分钟）。

```py
import time
import requests
import pandas as pd
import plotly.express as px

from IPython.display import clear_output

ISS_URL = 'https://api.wheretheiss.at/v1/satellites/25544'
ORBIT_TIME_SECS = 5_520  # Time required for ~1 complete orbit of ISS.
```

# 获取流数据

以下`get_iss_telemetry`函数从 URL 获取流数据，将其加载为 pandas DataFrame，删除不必要的列，并返回 DataFrame。每次调用此函数时，它会记录 ISS 的*单个瞬间*的遥测数据。稍后，我们将在循环中调用此函数，以*随时间跟踪* ISS。当然，为了使这一切正常工作，你需要一个活跃的互联网连接。

```py
def get_iss_telemetry(url):
    """Return DataFrame of current ISS telemetry from URL."""
    response = requests.get(url)
    if response.status_code != 200:
        raise Exception(f"Failed to fetch ISS position from {url}. \
        Status code: {response.status_code}")
    data = response.json()
    telemetry = pd.DataFrame(data, index=[0])
    telemetry = telemetry.drop(['id', 'footprint', 'daynum', 
                              'solar_lat', 'solar_lon'], axis=1)
    return telemetry
```

首先从网站获取遥测数据。requests 库抽象化了 Python 中 HTTP（*超文本传输协议*）请求的复杂性，使其更加简单和人性化。`get()`方法检索 URL 并将输出分配给一个`response`变量，该变量引用了网页返回的`Response`对象。此对象的*text 属性*包含可读的网页文本字符串。

下一步是检查`response`对象的 HTTP 状态码。状态码 200 表示客户端已请求服务器上的文档，服务器已应允。虽然 200 是最常见的成功响应，但你可能还会看到 201、202、205 或 206。

网站的响应格式为 JSON（*JavaScript 对象表示法*），因此我们使用`.json()`方法将文本字符串加载为 Python 字典，名为`data`。以下是输出示例：

```py
{'name': 'iss', 'id': 25544, 'latitude': 38.880080494467, 'longitude': -67.403143848187, 'altitude': 420.39039975637, 'velocity': 27592.859206857, 'visibility': 'daylight', 'footprint': 4509.4491061861, 'timestamp': 1679690648, 'daynum': 2460028.3639815, 'solar_lat': 1.5714095683049, 'solar_lon': 230.52865473588, 'units': 'kilometers'}
```

接下来，我们将`data`字典转换为名为`telemetry`的 pandas DataFrame。请注意，`index=[0]`参数可以避免“避免没有索引的标量值”错误。由于我们不需要诸如“id”或“solar_lat”之类的信息，我们将在返回 DataFrame 之前删除这些列。

![](img/10b4322ac9ce4cb44288b878a47bb6a1.png)

最终的“遥测”DataFrame（图像来源于作者）

# 绘制国际空间站

最后一步是定义一个函数，用于绘制给定数量轨道的遥测数据。这个函数将使用`for`循环调用之前的函数。最终的图形将包括一个*标记*表示国际空间站，并且有一条*线*记录其[ *地面轨迹*](https://en.wikipedia.org/wiki/Ground_track)随时间变化。

由于我们将在二维地图上绘制一个球体，这条圆形路径将表现为一个 S 形的*正弦曲线*。这是一个[有趣的视频](https://www.youtube.com/watch?v=JyfEffMrglI)，解释了原因。

![](img/36ac42ff099734662399a2c09c8a34ff.png)

当扁平化时，圆形轨道呈现为正弦波形（图像来源于作者）。

```py
def track_iss(url, num_orbits=2, interval=10):
    """
    Plot current ISS location from URL and record and plot its track.

    Arguments:
       url = ISS telemetry URL -> string
       num_orbits = Number of ISS orbits to plot -> integer
       interval = Wait period (seconds) between calls to URL -> integer
   """

    num_pulls = int(ORBIT_TIME_SECS * num_orbits / interval)
    latitudes = []
    longitudes = []

    for _ in range(num_pulls):
        df = get_iss_telemetry(url).round(2)
        latitudes.append(df['latitude'].iloc[0])
        longitudes.append(df['longitude'].iloc[0])        
        clear_output(wait=True)

        fig = px.scatter_geo(df, 
                             lat='latitude', 
                             lon='longitude', 
                             color='visibility',
                             color_discrete_map = {'daylight': 'red',
                                                   'visible': 'orange',
                                                   'eclipsed': 'black'},
                             hover_data=['timestamp', 'altitude', 
                                         'velocity', 'units'])
        fig.add_trace(px.line_geo(lat=latitudes, lon=longitudes).data[0])
        fig.update_traces(marker=dict(symbol='x-open', size=10), 
                          line_color='blue')
        fig.update_layout(width=700, height=500, 
                          title='International Space Station Tracking')
        fig.show()

        time.sleep(interval)
```

`track_iss`函数接受一个 URL、要绘制的轨道数量和一个暂停执行的时间间隔（秒）作为参数。这个暂停是为了防止网站被过多请求淹没。最后两个参数使用关键字，这意味着它们将被视为默认值。

`num_pulls`变量指的是我们调用`get_iss_telemetry()`函数并从网站“拉取”数据的次数。这个数字基于轨道时间、轨道数量和暂停间隔。

为了在地图上绘制国际空间站的轨迹，我们需要将每次提取的经纬度对存储在一个列表中。因此，我们在开始循环之前初始化每个属性的空列表。

要绘制遥测数据，我们遍历`num_pulls`变量，首先调用`get_iss_telemetry()`函数，将结果四舍五入到小数点后两位。然后，我们将纬度和经度结果附加到我们的列表中，并清除屏幕，以便每次循环都以新的图形开始。

现在来生成图形。首先，我们调用`px.scatter_geo()`方法在地球地图上绘制点。输入直观易懂。我们的 ISS 标记的颜色由 DataFrame 的“visibility”列决定。输出包括“daylight”、“eclipsed”和“visible”。后者指的是 ISS 仍在反射阳光，从而在夜空中可见。要将这些结果转换为颜色，我们将`color_discrete_map`参数传递一个字典。

为了支持“悬停窗口”弹出，我们将`hover_data`参数传递为我们想要查看的数据列名列表。要查看*完整*的 DataFrame，我们应传递`df.columns`，而不是列表。

在 Plotly 中，*图形*由一个或多个*轨迹*组成，每个轨迹是一个绘图元素，例如散点图、折线图或条形图。`px.line_geo()`方法返回一个包含*单个*轨迹对象的图形对象，该对象表示用给定的纬度和经度绘制的线。我们使用其*数据属性*（`.data[0]`）来添加该对象。索引为“0”，因为只有一个轨迹。

添加轨迹后，我们需要使用`update_traces()`方法更新图中的轨迹。我们的 ISS 标记参数使用字典指定。我们使用开放的“X”作为标记符号。对于折线图，我们指定颜色为蓝色。

为了完成图形，我们调用`update_layout()`方法并传入宽度、高度和标题。然后我们调用`show()`方法来显示结果。

循环通过调用`time`模块的`sleep()`方法并传入`interval`变量来结束。在这种情况下，它将使循环暂停 10 秒。尽管 WTIA REST API 的*请求速率*被限制为每秒一次，但没有必要贪婪！

剩下的就是调用我们的函数：

```py
track_iss(ISS_URL)
```

![](img/bdc0eab246346956ceb3b0fd95ca05c1.png)

显示约 2 圈 ISS 轨道的 ISS 跟踪器（图片作者提供）

# 结果

尽管代码量微不足道，但这个图表拥有*很多*功能。

如果将光标悬停在标记上，弹出窗口将显示诸如站点的高度、速度（以公里/小时为单位）、可见性等遥测数据。

![](img/d29dd5bf5fed7a1d1647be2580d74ca2.png)

Plotly Express 图形与激活的 ISS 标记悬停窗口（图片作者提供）

你可以使用鼠标滚轮或工具栏进行缩放。你可以截图、平移和重置视图。ISS 标记的颜色会根据 ISS 的照明情况而变化。

![](img/72427d3e655e41725bf8bf40fcf87b9e.png)

当 ISS 标记的位置对应于白天时，标记会变成红色（图片作者提供）

最后，我们通过*增量*处理流式 ISS 数据。尽管它以每秒一赫兹的频率流式传输，我们还是每十秒拉取一次数据，以便对源 API 更为友好。

# 更多跟踪器

你还可以通过[*Open-Notify-API*](http://open-notify.org/Open-Notify-API/ISS-Location-Now/)访问 ISS 的遥测数据。

正如使用 Python 一样，总有多种方法可以完成任务。以下是一些追踪空间站的替代方法：

+   使用 Plotly Express 和[正射投影（地球）](https://medium.com/codex/tracking-the-space-station-live-with-python-155060d1df45)。

+   使用 Plotly Express 并[计算空间站的速度](https://pythonawesome.com/international-space-station-data-with-python-research/)。

+   使用一个[ISS 图标](https://www.geeksforgeeks.org/how-to-track-iss-international-space-station-using-python/)进行追踪，并包含船员信息。

+   使用[Raspberry Pi](https://www.hackster.io/sridhar-rajagopal/international-space-station-tracker-6afdca#:~:text=Run%20the%20iss.py%20python%20script%20%28using%20python3%29%3A%20%24,the%20International%20Space%20Station%20on%20your%20ePaper%20Display%21)追踪空间站。

# 谢谢！

感谢阅读，确保关注我以获取未来更多*快速成功数据科学*项目。
