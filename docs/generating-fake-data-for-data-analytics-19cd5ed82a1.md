# 为数据分析生成虚假数据

> 原文：[`towardsdatascience.com/generating-fake-data-for-data-analytics-19cd5ed82a1`](https://towardsdatascience.com/generating-fake-data-for-data-analytics-19cd5ed82a1)

## 如果你没有真实数据，那就伪造吧！

[](https://weimenglee.medium.com/?source=post_page-----19cd5ed82a1--------------------------------)![Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----19cd5ed82a1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----19cd5ed82a1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----19cd5ed82a1--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----19cd5ed82a1--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----19cd5ed82a1--------------------------------) ·阅读时间 8 分钟·2023 年 3 月 14 日

--

![](img/50a67f9bb4a4499679e187efc9c4e02f.png)

[Leif Christoph Gottwald](https://unsplash.com/@project2204?utm_source=medium&utm_medium=referral) 的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在数据分析的世界里，获取一个好的数据集至关重要。在现实世界中，你可能会接触到大量未清理的数据，你可能需要花费一些时间来清理。如果你没有所需的数据并且想要快速制作一个概念验证演示怎么办？在这种情况下，你通常需要自己制作数据，同时你需要这些数据具有一定的现实感。那么你会怎么做？是手动费心制作数据，还是有一种自动化的方式？

在这篇文章中，我将展示一些有趣的方式来伪造数据，并使其看起来真实！

# 生成名字

要生成一些虚假的名字，你可以使用 `names` 包。要使用它，首先你需要安装它：

`!pip install names`

你现在可以使用包中的各种函数来生成性别特定的名字：

```py
import names

display(names.get_full_name('male'))
display(names.get_first_name())
display(names.get_last_name())

display(names.get_full_name('female'))
display(names.get_first_name())
display(names.get_last_name())
```

以下是一些生成的名字：

```py
'Gerald Paez'
'Matthew'
'Wiese'
'Dana Mcmullen'
'Heather'
'Oxley'

'Walter Walters'
'Connie'
'Vildosola'
'Nancy Correra'
'Aaron'
'Dawes'

'Randy Meli'
'Yvonne'
'Owen'
'Loretta Patague'
'Sidney'
'Oliver'
```

# 生成 UUID

除了名字，你可能还想生成另一种数据类型——UUID。**UUID**（**通用唯一标识符**）是一个 128 位的值，用于唯一标识互联网上的对象或实体。在移动领域，UUID 通常用于识别安装在设备上的应用程序。

要生成示例 UUID，你可以使用 `uuid` 包：

```py
!pip install uuid
```

你可以将生成的 UUID 转换为字符串：

```py
import uuid

str(uuid.uuid4())
```

这是一个生成的 UUID 示例：

```py
'54487fd7-0632-450e-b6e3-bcc54bc83133'
```

# `Faker` 包

在使用 Python 生成虚假数据时，`faker` 包绝对值得一提。`faker` 包生成各种虚假数据供你使用。你可以生成的数据包括：

+   地址

+   条形码

+   信用卡信息

+   ISBN

+   电话号码

+   等等！

在接下来的章节中，我将向你展示如何生成一些常用的数据。

## 生成用户资料

`faker`包可以生成用户资料，例如用户名、性别、地址、电子邮件和出生日期。以下代码片段创建了一个男性的简单资料：

```py
from faker import Faker

fake = Faker()
fake.simple_profile(sex='M')  # use 'F' for female
```

输出是一个包含男性详细信息的字典：

```py
{'username': 'lisa38',
 'name': 'Brandon Gibson',
 'sex': 'M',
 'address': '406 Brandi Inlet\nWest Christopherville, PR 41632',
 'mail': 'johnsondesiree@gmail.com',
 'birthdate': datetime.date(2008, 9, 10)}
```

## 生成日期

我特别想生成的一种数据是一个人的出生日期（DOB）。在存储个人详细信息时，总是推荐存储出生日期而不是年龄（出于非常明显的原因）。

使用`faker`包，你可以生成一个年龄在 18 到 60 岁之间的人的出生日期：

```py
fake.date_between(start_date='-60y', end_date='-18y')
```

返回的数据是一个`date`对象：

```py
datetime.date(1963, 4, 18)
```

如果你想将结果转换为字符串，可以使用`strftime()`函数：

```py
fake.date_between(start_date='-60y', end_date='-18y').strftime('%Y-%m-%d')
# '1973-07-16'
```

> 注意，每次你调用`Faker`对象中的一个函数时，都会生成一组新的数据。如果你希望生成的数据是确定性的（即总是相同的），你可以使用`seed()`函数，如：`Faker.seed(0)`。

## 生成位置

我想生成的下一种数据是位置数据。例如，你想获取美国某个位置的经纬度。你可以使用`local_latlng()`函数并指定`country_code`参数：

```py
fake.local_latlng(country_code = 'US')
```

该函数返回一个在`country_code`指定的国家中已知存在的地点。信息以一个元组的形式返回，如下所示：

```py
('33.72255', '-116.37697', 'Palm Desert', 'US', 'America/Los_Angeles')
```

如果你只需要经纬度而不需要其他信息，将`coords_only`设置为`True`：

```py
fake.local_latlng(country_code = 'US', coords_only=True)
```

`country_code`参数接受来自`land_coords`常量的值，例如`AU`代表澳大利亚：

```py
fake.local_latlng(country_code = 'AU')
# ('-25.54073', '152.70493', 'Maryborough', 'AU', 'Australia/Brisbane')
```

> 我在 Faker 文档中找不到`land_coords`常量的定义，但你可以参考[`rdrr.io/github/LuYang19/faker/src/R/init.R`](https://rdrr.io/github/LuYang19/faker/src/R/init.R)中定义的`*land_coords*`变量。

如果你需要一对保证存在于陆地上的坐标，可以使用`location_on_land()`函数：

```py
fake.location_on_land(coords_only=True)
# ('54.58048', '16.86194')
```

## 生成地址

如果你想生成一些示例地址，可以使用`address()`、`current_country()`、`city()`、`country()`和`country_code()`函数：

```py
display(fake.address())   
# '910 Jason Green Apt. 954\nJonesland, IL 76881'

display(fake.current_country())    # based on the address returned by address()
# 'United States'

display(fake.city())
# 'North Carolyn'

display(fake.country())
# 'Holy See (Vatican City State)'

display(fake.country_code())   
# MU
```

## Faker 中的区域设置支持

到目前为止，生成的所有名称和地址都是英语的。然而，`faker`包也支持不同的区域设置。支持的区域设置列表可以在以下网址找到：[`faker.readthedocs.io/en/master/locales.html`](https://faker.readthedocs.io/en/master/locales.html)。

下图显示了一个示例区域设置——`zh_CN`：

![](img/d2545a338ffe671e76d8219e89d8168b.png)

所有图片均由作者提供

例如，在`zh_CN`区域设置中，你可以找到以下提供者：

+   `faker.providers.address`

+   `faker.providers.company`

+   `faker.providers.date_time`

+   `faker.providers.internet`

+   `faker.providers.job`

+   `faker.providers.lores`

+   `faker.providers.person`

+   `faker.providers.phone_number`

+   `faker.providers.ssn`

这意味着上述所有提供者都支持`zh_CN`地区设置。以`faker.providers.address` ([`faker.readthedocs.io/en/master/locales/zh_CN.html#faker-providers-address`](https://faker.readthedocs.io/en/master/locales/zh_CN.html#faker-providers-address))为例。当实例化`Faker`对象时，可以传入一个或多个地区设置：

```py
fake = Faker(['zh_CN'])   # Chinese in China locale
fake.address()
```

上述`address()`函数返回中文地址：

```py
'内蒙古自治区飞市兴山深圳路 b 座 104347'
```

如果使用`zh_CN`地区设置，一些函数将绑定到该地区设置，例如：

+   `fake.name()`

+   `fake.address()`

+   `fake.current_country()`

以下是一些示例：

```py
'洪凤兰'
'辽宁省波县永川王路 s 座 292815'
"People's Republic of China"

'吕峰'
'广东省凤英市吉区李路 t 座 385879'
"People's Republic of China"

'何秀梅'
'浙江省齐齐哈尔市上街潮州路 M 座 218662'
"People's Republic of China"
```

地址结果将是中国的这些地点。

调用其他函数，如`fake.country()`将返回其他国家，但结果将以中文显示（基于`zh-CN`地区设置）：

```py
'越南'
```

> 越南就是 Vietnam。

你还可以使用`zh_CN`地区设置生成中文姓名：

```py
fake = Faker(['zh_CN'])
display(fake.first_name_male())
display(fake.last_name_male())
display(fake.name())
```

这里是上述代码片段的示例输出：

```py
'龙'
'马'
'雷春梅'
```

# 汇总它们

使用所有生成不同类型虚假数据的方法，我想把它们汇总在一起，以便进行一些数据分析。

以下代码片段生成 1000 组以下数据：

+   UUID

+   用户名

+   来自七个国家中的一个的纬度、经度和国家

+   性别

+   出生日期

```py
from faker import Faker
import random
import uuid

uuids = []
usernames = []
latitudes = []
longitudes = []
genders = []
countries = []
dobs = []
n = 1000

fake = Faker()
country_codes = ['US','GB','AU','CN','FR','CH','DE']
for gender in ['M','F']:
    for i in range(n // 2):                 # 500 males and 500 females
        # uuids
        uuids.append(str(uuid.uuid4()))

        # username and sex
        profile = fake.simple_profile(sex=gender)
        usernames.append(profile['username'])
        genders.append(profile['sex'])

        # dob
        dobs.append(fake.date_between(start_date='-78y', end_date='-18y'))

        # lat and lng, and country
        location = fake.local_latlng(country_code = country_codes[random.randint(0, len(country_codes) -1)])
        latitudes.append(location[0])
        longitudes.append(location[1])
        countries.append(location[3]) 
```

然后，我将 1000 组数据合并到一个 Pandas DataFrame 中：

```py
import pandas as pd
df = pd.DataFrame(data = [uuids, usernames, genders, countries, latitudes, longitudes, dobs])
df = df.T
df.columns = ['uuid', 'username', 'gender', 'country', 'latitude', 'longitude', 'dob']
df
```

数据框现在包含 1000 个虚构的用户账户及其个人详细信息，如应用 ID、位置信息、性别和出生日期：

![](img/f9c84aa461c95f3894f4c6a0d879f22b.png)

## 绘制地图

使用纬度和经度，绘制用户的地理位置会很有趣。为此，我使用了 Folium：

```py
import folium                    # pip install folium

mymap = folium.Map(location = [22.827806844385826, 4.363328554220703], 
                   width = 950, 
                   height = 600,
                   zoom_start = 2, 
                   tiles = 'openstreetmap')

folium.TileLayer('Stamen Terrain').add_to(mymap)
folium.TileLayer('Stamen Toner').add_to(mymap)
folium.TileLayer('Stamen Water Color').add_to(mymap)
folium.TileLayer('cartodbpositron').add_to(mymap)
folium.TileLayer('cartodbdark_matter').add_to(mymap)
folium.LayerControl().add_to(mymap)

for lat, lng in zip(df['latitude'], df['longitude']):    
    station = folium.CircleMarker(
            location = [lat, lng],
            radius = 5,
            color = 'red',
            fill = True,
            fill_color = 'yellow',
            fill_opacity = 0.3)   

    # add the circle marker to the map
    station.add_to(mymap)
mymap
```

[](/visualization-in-python-visualizing-geospatial-data-122bf85d128f?source=post_page-----19cd5ed82a1--------------------------------) ## Python 中的可视化 — 可视化地理空间数据

### 学习如何使用 folium 轻松显示地图和标记

[towardsdatascience.com

这是显示用户分布的地图：

![](img/c4bbf143ad1163f130531532f73b8cd6.png)

我可以放大地图：

![](img/43f3c20c5001cf6e963ce9f00be85059.png)

我还可以更改图层集：

![](img/a42066beed010006357b69a864a0e64d.png)

## 绘制饼图

我可以可视化我的用户来自哪里：

```py
df.groupby('country').count().plot.pie(y='username')
```

![](img/b2faa2463ba28ca7946e9cc6465b44f5.png)

我还可以使饼图更具描述性：

```py
total = df.shape[0]
def fmt(x):    
    return '{:.2f}%\n({:.0f})'.format(x, total * x / 100)

df.groupby('country').count().plot.pie(y='username', autopct=fmt)
```

![](img/e349f316c94299e2d9cf41678d823bfb.png)

## 绘制条形图

每个国家的总用户数也可以通过条形图绘制：

```py
from matplotlib import cm
import numpy as np

color = cm.inferno_r(np.linspace(.4, .8, len(country_codes)))

df.groupby('country').count().plot.bar(y = 'username',
                                       color = color, 
                                       legend = False
                                      )
```

从图表中可以看到，大不列颠的用户最多，而中国的用户最少：

![](img/2cdd221dd33cfa1c40df1df6d9d2beba.png)

## 绘制直方图

我还可以了解用户的年龄分布。为此，我需要先根据他们的出生日期计算他们的当前年龄：

```py
from datetime import datetime, date
from dateutil import relativedelta

def cal_age(born):
    return relativedelta.relativedelta(date.today(), born).years

df['age'] = df['dob'].apply(cal_age)
df
```

数据框现在增加了一列，显示每个用户的年龄：

![](img/6e5d5014270a16a6264e0dadf2b846ee.png)

你现在可以绘制一个显示年龄分布的直方图：

```py
ax = df['age'].hist(bins=15, edgecolor='black', linewidth=1.2, color='yellow')
ax.set_xlabel("Age")
ax.set_ylabel("Total")
ax.set_xticks(range(18,80,5))
ax.set_title("Users age distribution")
```

![](img/e0125ec918f69eeb4f38fbc460fd402b.png)

**如果你喜欢阅读我的文章并且对你的职业/学习有帮助，请考虑注册成为 Medium 会员。每月$5，可以无限访问 Medium 上的所有文章（包括我的文章）。如果你使用以下链接注册，我将获得一小笔佣金（对你没有额外费用）。你的支持意味着我可以花更多时间写这样的文章。**

[](https://weimenglee.medium.com/membership?source=post_page-----19cd5ed82a1--------------------------------) [## 通过我的推荐链接加入 Medium - 韦孟李

### 阅读韦孟李的每个故事（以及 Medium 上其他成千上万的作家）。你的会员费直接支持…

weimenglee.medium.com](https://weimenglee.medium.com/membership?source=post_page-----19cd5ed82a1--------------------------------)

# 总结

希望你现在更有能力生成项目所需的任何额外数据。生成逼真的演示数据不仅能让你更准确地测试算法，还能在演示中提供更多的真实感。请在评论中告诉我你通常需要生成其他类型的数据！
