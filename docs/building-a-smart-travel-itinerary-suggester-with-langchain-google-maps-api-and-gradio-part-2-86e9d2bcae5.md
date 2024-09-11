# 使用 LangChain、Google Maps API 和 Gradio 构建智能旅行路线建议器（第二部分）

> 原文：[https://towardsdatascience.com/building-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-2-86e9d2bcae5?source=collection_archive---------3-----------------------#2023-09-26](https://towardsdatascience.com/building-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-2-86e9d2bcae5?source=collection_archive---------3-----------------------#2023-09-26)

## 学习如何构建一个可能激发你下次公路旅行灵感的应用程序

[](https://medium.com/@rmartinshort?source=post_page-----86e9d2bcae5--------------------------------)[![Robert Martin-Short](../Images/e3910071b72a914255b185b850579a5a.png)](https://medium.com/@rmartinshort?source=post_page-----86e9d2bcae5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----86e9d2bcae5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----86e9d2bcae5--------------------------------) [Robert Martin-Short](https://medium.com/@rmartinshort?source=post_page-----86e9d2bcae5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F83d38eb39498&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-2-86e9d2bcae5&user=Robert+Martin-Short&userId=83d38eb39498&source=post_page-83d38eb39498----86e9d2bcae5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----86e9d2bcae5--------------------------------) ·11分钟阅读·2023年9月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F86e9d2bcae5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-2-86e9d2bcae5&user=Robert+Martin-Short&userId=83d38eb39498&source=-----86e9d2bcae5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F86e9d2bcae5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-2-86e9d2bcae5&source=-----86e9d2bcae5---------------------bookmark_footer-----------)

> **这篇文章是三部分系列中的第二部分，我们使用 OpenAI 和 Google API 构建了一个旅行路线建议应用程序，并通过 gradio 生成的简单 UI 展示。在这一部分，我们讨论了如何使用 Google Maps API 和 folium 从一组途经点生成交互式路线地图。只想看看代码？在** [**这里**](https://github.com/rmartinshort/travel_mapper)**。**

# **1\. 第一部分回顾**

在[第一部分](/building-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-1-4175ff480b74)的三部分系列中，我们使用 LangChain 和提示工程构建了一个系统，该系统顺序调用 LLM API——无论是谷歌的 PaLM 还是 OpenAI 的 ChatGPT——将用户的查询转换为旅行行程和格式化良好的地址列表。现在是时候看看如何将这些地址列表转换为带有路线标记的旅行路线了。为此，我们主要将使用通过[googlemaps](https://pypi.org/project/googlemaps/)包提供的 Google Maps API。我们还将使用[folium](https://pypi.org/project/folium/)进行绘图。让我们开始吧！

# 2\. 准备进行 API 调用

要生成 Google Maps 的 API 密钥，你首先需要在 Google Cloud 上创建一个账户。他们提供[90 天免费试用期](https://cloud.google.com/free/docs/free-cloud-features?_ga=2.153672123.-2071471501.1688189408)，之后你将按使用的 API 服务支付费用，类似于你在 OpenAI 上的操作。完成后，你可以创建一个项目（我的项目叫 LLMMapper），并导航到 Google Cloud 网站上的 Google Maps Platform 部分。从那里，你应该能访问“密钥与凭据”菜单以生成 API 密钥。你还应该查看“API 和服务”菜单，探索 Google Maps Platform 提供的众多服务。在这个项目中，我们只会使用方向和地理编码服务。我们将对每个途经点进行地理编码，然后查找它们之间的路线。

![](../Images/2592a69158a712deddee1ca9d26f5921.png)

截图显示了如何导航到 Google Maps Platform 网站的密钥和凭据菜单。在这里你将生成一个 API 密钥。

现在，可以将 Google Maps API 密钥添加到我们之前设置的 `.env` 文件中

```py
OPENAI_API_KEY = {your open ai key}
GOOGLE_PALM_API_KEY = {your google palm api key}
GOOGLE_MAPS_API_KEY = {your google maps api key here}
```

要测试这是否有效，请使用第一部分中描述的方法从 `.env` 文件加载机密。然后我们可以尝试如下进行地理编码调用

```py
import googlemaps

def convert_to_coords(input_address):
    return self.gmaps.geocode(input_address)

secrets = load_secets()
gmaps = googlemaps.Client(key=secrets["GOOGLE_MAPS_API_KEY"])

example_coords = convert_to_coords("The Washington Moment, DC")
```

谷歌地图能够将提供的字符串与实际地点的地址和详细信息匹配，并应返回如下列表

```py
[{'address_components': [{'long_name': '2',
    'short_name': '2',
    'types': ['street_number']},
   {'long_name': '15th Street Northwest',
    'short_name': '15th St NW',
    'types': ['route']},
   {'long_name': 'Washington',
    'short_name': 'Washington',
    'types': ['locality', 'political']},
   {'long_name': 'District of Columbia',
    'short_name': 'DC',
    'types': ['administrative_area_level_1', 'political']},
   {'long_name': 'United States',
    'short_name': 'US',
    'types': ['country', 'political']},
   {'long_name': '20024', 'short_name': '20024', 'types': ['postal_code']}],
  'formatted_address': '2 15th St NW, Washington, DC 20024, USA',
  'geometry': {'location': {'lat': 38.8894838, 'lng': -77.0352791},
   'location_type': 'ROOFTOP',
   'viewport': {'northeast': {'lat': 38.89080313029149,
     'lng': -77.0338224697085},
    'southwest': {'lat': 38.8881051697085, 'lng': -77.0365204302915}}},
  'partial_match': True,
  'place_id': 'ChIJfy4MvqG3t4kRuL_QjoJGc-k',
  'plus_code': {'compound_code': 'VXQ7+QV Washington, DC',
   'global_code': '87C4VXQ7+QV'},
  'types': ['establishment',
   'landmark',
   'point_of_interest',
   'tourist_attraction']}]
```

这非常强大！虽然请求有些模糊，但谷歌地图服务准确地匹配到了一个精确的地址，并提供了坐标以及其他可能对开发者有用的本地信息。我们只需要使用`formatted_address`和`place_id`字段即可。

# **3\. 构建路线**

地理编码对于我们的旅行地图应用程序很重要，因为地理编码 API 似乎在处理模糊或部分完成的地址时比方向 API 更加熟练。无法保证来自 LLM 调用的地址包含足够的信息，以便方向 API 能提供良好的响应，因此首先进行地理编码步骤可以减少错误的可能性。

让我们首先对起点、终点和中间途经点列表调用地理编码器，并将结果存储在字典中

```py
 def build_mapping_dict(start, end, waypoints):

    mapping_dict = {}
    mapping_dict["start"] = self.convert_to_coords(start)[0]
    mapping_dict["end"] = self.convert_to_coords(end)[0]

    if waypoints:
      for i, waypoint in enumerate(waypoints):
          mapping_dict["waypoint_{}".format(i)] = convert_to_coords(
                    waypoint
                )[0

    return mapping_dict
```

现在，我们可以利用方向 API 获取包含途经点的从起点到终点的路线

```py
 def build_directions_and_route(
        mapping_dict, start_time=None, transit_type=None, verbose=True
    ):
    if not start_time:
        start_time = datetime.now()

    if not transit_type:
        transit_type = "driving"

      # later we replace this with place_id, which is more efficient
      waypoints = [
            mapping_dict[x]["formatted_address"]
            for x in mapping_dict.keys()
            if "waypoint" in x
      ]
      start = mapping_dict["start"]["formatted_address"]
      end = mapping_dict["end"]["formatted_address"]

      directions_result = gmaps.directions(
            start,
            end,
            waypoints=waypoints,
            mode=transit_type,
            units="metric",
            optimize_waypoints=True,
            traffic_model="best_guess",
            departure_time=start_time,
      )

      return directions_result
```

指南 API 的完整文档在[这里](https://googlemaps.github.io/google-maps-services-python/docs/index.html)，并且可以指定许多不同的选项。注意我们指定了路线的起点和终点，以及途经点的列表，并选择了`optimize_waypoints=True`，这样 Google Maps 就知道可以调整途经点的顺序以减少总旅行时间。我们还可以指定交通类型，默认为`driving`，除非另有设置。请回忆一下在第 1 部分中我们让 LLM 返回交通类型及其行程建议，因此理论上我们也可以在这里利用这一点。

从方向 API 调用返回的字典具有以下键

```py
['bounds', 
'copyrights', 
'legs', 
'overview_polyline', 
'summary', 
'warnings', 
'waypoint_order'
]
```

在这些信息中，`legs` 和 `overview_polyline` 对我们最有用。`legs` 是一个路线段的列表，每个元素看起来像这样

```py
['distance', 
'duration', 
'end_address', 
'end_location', 
'start_address', 
'start_location', 
'steps', 
'traffic_speed_entry', 
'via_waypoint'
]
```

每个 `leg` 进一步细分为 `steps`，这是逐步指示和其关联的路线段的集合。这是一个包含以下键的字典列表

```py
['distance', 
'duration', 
'end_location', 
'html_instructions', 
'polyline', 
'start_location', 
'travel_mode'
]
```

`polyline` 键是存储实际路线信息的地方。每个 polyline 是一系列坐标的编码表示，Google Maps 生成这些编码作为将长列表的经纬度值压缩成一个字符串的方法。它们是编码字符串，看起来像

“e|peFt_ejVjwHalBzaHqrAxeE~oBplBdyCzpDif@njJwaJvcHijJ~cIabHfiFyqMvkFooHhtE}mMxwJgqK”

你可以在[这里](https://developers.google.com/maps/documentation/utilities/polylinealgorithm)阅读更多内容，但幸运的是，我们可以使用`decode_polyline`工具将它们转换回坐标。例如

```py
from googlemaps.convert import decode_polyline

overall_route = decode_polyline(
directions_result[0]["overview_polyline"]["points"]
)
route_coords = [(float(p["lat"]),float(p["lng"])) for p in overall_route]
```

这将提供沿路线的经纬度点列表。

这就是我们绘制一个简单地图的所有信息，显示途经点及其连接的正确驾驶路径。我们可以使用 `overview_polyline` 作为起点，尽管我们稍后会看到，这可能会在高缩放级别的地图上导致分辨率问题。

假设我们从以下查询开始：

“我想从旧金山到拉斯维加斯进行为期 5 天的公路旅行。我想沿着 HW1 参观漂亮的沿海城镇，然后在南加州欣赏山景”

我们的 LLM 调用提取了一个途经点字典，我们运行了`build_mapping_dict`和`build_directions_and_route`以从 Google Maps 获得我们的方向结果

我们可以这样提取途经点

```py
 marker_points = []
nlegs = len(directions_result[0]["legs"])
for i, leg in enumerate(directions_result[0]["legs"]):

  start, start_address = leg["start_location"], leg["start_address"]
  end,  end_address = leg["end_location"], leg["end_address"]

  start_loc = (float(start["lat"]),float(start["lng"]))
  end_loc = (float(end["lat"]),float(end["lng"]))

  marker_points.append((start_loc,start_address))

  if i == nlegs-1:
    marker_points.append((end_loc,end_address))
```

现在，使用 folium 和 branca，我们可以绘制一个漂亮的交互式地图，这个地图应该会出现在 Colab 或 Jupyter Notebook 中

```py
import folium
from branca.element import Figure

figure = Figure(height=500, width=1000)

# decode the route
overall_route = decode_polyline(
  directions_result[0]["overview_polyline"]["points"]
)
route_coords = [(float(p["lat"]),float(p["lng"])) for p in overall_route]

# set the map center to be at the start location of the route
map_start_loc = [overall_route[0]["lat"],overall_route[0]["lng"]]
map = folium.Map(
  location=map_start_loc, 
  tiles="Stamen Terrain", 
  zoom_start=9
)
figure.add_child(map)

# Add the waypoints as red markers 
for location, address in marker_points:
    folium.Marker(
        location=location,
        popup=address,
        tooltip="<strong>Click for address</strong>",
        icon=folium.Icon(color="red", icon="info-sign"),
    ).add_to(map)

# Add the route as a blue line
f_group = folium.FeatureGroup("Route overview")
folium.vector_layers.PolyLine(
    route_coords,
    popup="<b>Overall route</b>",
    tooltip="This is a tooltip where we can add distance and duration",
    color="blue",
    weight=2,
).add_to(f_group)
f_group.add_to(map)
```

当运行此代码时，Folium 将生成一个交互式地图，我们可以探索并点击每一个途经点。

![](../Images/0bef881abf974f02c318d7a36bf5c8f7.png)

从 Google Maps API 调用结果生成的交互式地图

# **4. 优化路线**

上述方法中，我们通过一个包含航点列表的单次调用来获取 Google Maps 方向 API，然后绘制 `overview_polyline`，作为概念验证效果很好，但仍存在一些问题：

1.  在调用 Google Maps 时，使用 `place_id` 来指定起点、终点和航点名称比使用 `formatted_address` 更有效。幸运的是，我们在地理编码调用的结果中获得了 `place_id`，因此我们应该使用它。

1.  单次 API 调用中可以请求的航点数量限制为 25（有关详细信息，请参见 [https://developers.google.com/maps/documentation/directions/get-directions](https://developers.google.com/maps/documentation/directions/get-directions)）。如果我们从 LLM 获得的行程中有超过 25 个停靠点，我们需要向 Google Maps 发出更多调用，然后合并响应。

1.  `overview_polyline` 在放大时分辨率有限，可能是因为它沿线的点数经过了大规模地图视图的优化。这对于一个概念验证来说不是主要问题，但如果能对路线分辨率进行更多控制，使其在高缩放级别下也能保持良好外观，那就更好了。方向 API 为我们提供了更细致的路段折线，我们可以利用这些信息。

1.  在地图上，将路线拆分为单独的路段并允许用户查看与每个路段相关的距离和旅行时间是很好的功能。同样，Google Maps 提供了这些信息，因此我们应该加以利用。

![](../Images/7ba947b395671f85e82c5cf9793fd9ce.png)

`overview_polyline` 的分辨率有限。在这里，我们已经缩放到圣巴巴拉，但尚不清楚我们应该走哪些道路。

问题 1 可以通过修改 `build_directions_and_route` 来使用 `mapping_dict` 中的 `place_id` 而不是 `formatted_address` 来轻松解决。问题 2 稍微复杂一些，需要将初始航点拆分成一些最大长度的块，从每个块中创建起点、终点和子列表，然后在这些块上运行 `build_mapping_dict` 和 `build_directions_and_route`。最终结果可以在最后合并。

问题 3 和 4 可以通过使用 Google Maps 返回的每段路程的单独步骤折线来解决。我们只需遍历这两个级别，解码相关的折线，然后构建一个新的字典。这也使我们能够提取距离和持续时间值，这些值被分配给每个解码的路段，然后用于绘图。

```py
def get_route(directions_result):
    waypoints = {}

    for leg_number, leg in enumerate(directions_result[0]["legs"]):
        leg_route = {}

        distance, duration = leg["distance"]["text"], leg["duration"]["text"]
        leg_route["distance"] = distance
        leg_route["duration"] = duration
        leg_route_points = []

        for step in leg["steps"]:
             decoded_points = decode_polyline(step["polyline"]["points"])
            for p in decoded_points:
              leg_route_points.append(f'{p["lat"]},{p["lng"]}')

            leg_route["route"] = leg_route_points
            waypoints[leg_number] = leg_route

    return waypoints
```

现在的问题是 `leg_route_points` 列表可能会变得非常长，当我们在地图上绘制这些点时，可能会导致 folium 崩溃或运行非常缓慢。解决方案是沿路线采样这些点，以确保有足够的点以便进行良好的可视化，但又不至于让地图加载困难。

一种简单且安全的方法是计算总路线应包含的点数（例如5000个点），然后确定每段路线应包含的点的比例，并均匀地从每段中采样相应数量的点。请注意，我们需要确保每段至少包含一个点，以便它能够显示在地图上。

以下函数将执行此采样，输入一个来自上面`get_route`函数的`waypoints`字典。

```py
def sample_route_with_legs(route, distance_per_point_in_km=0.25)):

    all_distances = sum([float(route[i]["distance"].split(" ")[0]) for i in route])
    # Total points in the sample
    npoints = int(np.ceil(all_distances / distance_per_point_in_km))

    # Total points per leg
    points_per_leg = [len(v["route"]) for k, v in route.items()]
    total_points = sum(points_per_leg)

    # get number of total points that need to be represented on each leg
    number_per_leg = [
      max(1, np.round(npoints * (x / total_points), 0)) for x in points_per_leg
      ]

    sampled_points = {}
    for leg_id, route_info in route.items():
        total_points = int(points_per_leg[leg_id])
        total_sampled_points = int(number_per_leg[leg_id])
        step_size = int(max(total_points // total_sampled_points, 1.0))
        route_sampled = [
                route_info["route"][idx] for idx in range(0, total_points, step_size)
            ]

        distance = route_info["distance"]
        duration = route_info["duration"]

        sampled_points[leg_id] = {
                "route": [
                    (float(x.split(",")[0]), float(x.split(",")[1]))
                    for x in route_sampled
                ],
                "duration": duration,
                "distance": distance,
            }
    return sampled_points
```

在这里我们指定了我们想要的点间距——每250米一个点——然后相应地选择点的数量。我们还可以考虑从路线长度估算所需的点间距，但这种方法在第一次尝试中似乎效果相当好，在地图上的中等高的缩放级别下提供了可接受的分辨率。

现在我们已经将路线拆分为具有合理样本点数量的段落，我们可以继续将它们绘制在地图上，并使用以下代码对每一段进行标注。

```py
for leg_id, route_points in sampled_points.items():
    leg_distance = route_points["distance"]
    leg_duration = route_points["duration"]

    f_group = folium.FeatureGroup("Leg {}".format(leg_id))
    folium.vector_layers.PolyLine(
                route_points["route"],
                popup="<b>Route segment {}</b>".format(leg_id),
                tooltip="Distance: {}, Duration: {}".format(leg_distance, leg_duration),
                color="blue",
                weight=2,
    ).add_to(f_group)
    # assumes the map has already been generated
    f_group.add_to(map)
```

![](../Images/eb81e046ba3eae34d2a5cdc93a96294b.png)

这是一个标注并注释过的路线段示例，以便它能够出现在地图上。

# **5\. 整合所有内容**

在代码库中，以上提到的所有方法都被打包在两个类中。第一个是`RouteFinder`，它接受`Agent`的结构化输出（见第1部分），并生成采样路线。第二个是`RouteMapper`，它接收采样路线并绘制一个folium地图，可以保存为html文件。

由于我们几乎总是希望在请求路线时生成地图，`RouteFinder`的`generate_route`方法处理这两个任务。

```py
class RouteFinder:
    MAX_WAYPOINTS_API_CALL = 25

    def __init__(self, google_maps_api_key):
        self.logger = logging.getLogger(__name__)
        self.logger.setLevel(logging.INFO)
        self.mapper = RouteMapper()
        self.gmaps = googlemaps.Client(key=google_maps_api_key)

    def generate_route(self, list_of_places, itinerary, include_map=True):

        self.logger.info("# " * 20)
        self.logger.info("PROPOSED ITINERARY")
        self.logger.info("# " * 20)
        self.logger.info(itinerary)

        t1 = time.time()
        directions, sampled_route, mapping_dict = self.build_route_segments(
            list_of_places
        )
        t2 = time.time()
        self.logger.info("Time to build route : {}".format((round(t2 - t1, 2))))

        if include_map:
            t1 = time.time()
            self.mapper.add_list_of_places(list_of_places)
            self.mapper.generate_route_map(directions, sampled_route)
            t2 = time.time()
            self.logger.info("Time to generate map : {}".format((round(t2 - t1, 2))))

        return directions, sampled_route, mapping_dict
```

回想一下在第1部分中我们构建了一个名为`Agent`的类，该类处理LLM调用。现在我们还有了`RouteFinder`，我们可以将它们组合到整个旅行映射器项目的基础类中。

```py
class TravelMapperBase(object):
    def __init__(
        self, openai_api_key, google_palm_api_key, google_maps_key, verbose=False
    ):
        self.travel_agent = Agent(
            open_ai_api_key=openai_api_key,
            google_palm_api_key=google_palm_api_key,
            debug=verbose,
        )
        self.route_finder = RouteFinder(google_maps_api_key=google_maps_key)

    def parse(self, query, make_map=True):

        itinerary, list_of_places, validation = self.travel_agent.suggest_travel(query)

        directions, sampled_route, mapping_dict = self.route_finder.generate_route(
            list_of_places=list_of_places, itinerary=itinerary, include_map=make_map
        )
```

这可以通过以下查询运行，这也是在`test_without_gradio`脚本中给出的示例。

```py
from travel_mapper.TravelMapper import load_secrets, assert_secrets
from travel_mapper.TravelMapper import TravelMapperBase

def test(query=None):
    secrets = load_secrets()
    assert_secrets(secrets)

    if not query:
        query = """
        I want to do 2 week trip from Berkeley CA to New York City.
        I want to visit national parks and cities with good food.
        I want use a rental car and drive for no more than 5 hours on any given day.
        """

    mapper = TravelMapperBase(
        openai_api_key=secrets["OPENAI_API_KEY"],
        google_maps_key=secrets["GOOGLE_MAPS_API_KEY"],
        google_palm_api_key=secrets["GOOGLE_PALM_API_KEY"],
    )

    mapper.parse(query, make_map=True)
```

就路线和地图生成而言，我们现在已经完成了！但是我们如何将所有这些代码打包成一个易于实验的漂亮UI呢？这将会在本系列的[第三部分](https://example.org/building-a-smart-travel-itinerary-suggester-with-langchain-google-maps-api-and-gradio-part-3-90dc7be627fb)中讲解。

感谢阅读！请随时在这里探索完整的代码库[https://github.com/rmartinshort/travel_mapper](https://github.com/rmartinshort/travel_mapper)。任何改进建议或功能扩展都会非常感谢！
