# 在你的 Medium 博客中展示 Streamlit 应用

> 原文：[`towardsdatascience.com/show-streamlit-apps-in-your-medium-blog-520e98c7d51d`](https://towardsdatascience.com/show-streamlit-apps-in-your-medium-blog-520e98c7d51d)

## 跟随这个教程将你的应用在线发布到 Medium 帖子中。

[](https://gustavorsantos.medium.com/?source=post_page-----520e98c7d51d--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----520e98c7d51d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----520e98c7d51d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----520e98c7d51d--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----520e98c7d51d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----520e98c7d51d--------------------------------) ·阅读时间 5 分钟·2023 年 5 月 30 日

--

![](img/584948e741977c4a9d6470463dfef5be.png)

图片由 [Lloyd Dirks](https://unsplash.com/@lloyddirks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/kz8pEH4IsUo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

我喜欢写关于`Streamlit`的文章。我会说它是我最喜欢的 Python 包之一。

**Streamlit** 是一个非常容易学习的包，它使人们能够快速创建仪表板、Web 应用，甚至将模型部署到生产环境中。我已经写过几次关于它的文章，甚至教你如何用这个优秀的包来构建和部署你的第一个应用（见下方链接）。

2022 年，Snowflake 收购了 Streamlit，从那时起，他们不断为这个包添加更多功能，使其变得更好。最近，他们推出了一个非常酷的功能，我想在这个简短的教程中展示：*如何将你的应用嵌入到像这样的 Medium 博客中*。

[](https://medium.com/gustavorsantos/streamlit-basics-build-your-first-web-app-ffd377a85666?source=post_page-----520e98c7d51d--------------------------------) [## Streamlit 基础：构建你的第一个 Web 应用

### 学习创建每个基本功能的代码，并在几分钟内部署你的 Web 应用。

[medium.com](https://medium.com/gustavorsantos/streamlit-basics-build-your-first-web-app-ffd377a85666?source=post_page-----520e98c7d51d--------------------------------) [](https://medium.com/gustavorsantos/streamlit-basics-deploy-your-first-web-app-198934d6f7a0?source=post_page-----520e98c7d51d--------------------------------) [## Streamlit 基础：部署你的第一个 Web 应用

### 学习如何使用 Streamlit 的分享功能部署一个 Web 应用。

[medium.com](https://medium.com/gustavorsantos/streamlit-basics-deploy-your-first-web-app-198934d6f7a0?source=post_page-----520e98c7d51d--------------------------------)

# 使用案例

在我看来，将应用程序嵌入 Medium 博客中的好处有很多。

首先，我认为这是任何数据科学家建立优秀作品集的好方法。如果你在科技行业工作，你可能知道拥有一个有趣的作品集来展示你的技能，是吸引注意力的好方法，同时也能保持你的专业形象光鲜亮丽。

另一个好处是将研究成果展示给世界或甚至是客户。你可以写一篇博客文章，并在良好的市场分析或书面报告之后，将你的应用程序添加进去，作为你工作的一个很好的视觉补充。

我甚至看到有人将餐厅菜单做成 Streamlit 应用程序，连接客户和厨房，这真的很棒。所以，看看我们可以想到多少种选项。

# 创建一个简单的应用程序

好的，为了嵌入一个应用程序，我们需要先创建一个。所以，让我们创建一个简单的应用程序，只需几行代码。

我决定使用`meteostat`包创建一个应用程序，该包允许你根据经纬度坐标从几乎任何位置检索温度信息。

下面是我将在这个快速项目中使用的包：

```py
# Imports
import pandas as pd
import streamlit as st
from datetime import datetime
from meteostat import Point, Monthly
```

接下来，让我们将 streamlit 页面设置为宽布局，这样我们的应用程序可以占据页面的更多部分，而不是将所有功能居中显示。作为输入数据，我使用了这个包含许多世界城市及其经纬度的 csv 文件。

```py
# Set Page Layout
st.set_page_config(layout='wide')

# Load the Dataset
cities = pd.read_csv('world_cities.csv')
```

很好。下一步是放置*两个下拉框*，让用户选择他们想要检索信息的城市和年份。

```py
# Select box
st.subheader('Temperature History App')

col1, col2 = st.columns(2, gap='medium')
# column 1 - Table weather history
with col1:
    # Title of the select box
    selected_city = st.selectbox(label='Select a city for weather information',
                                 options=cities['city'].sort_values().unique())

with col2:
    selected_year = st.selectbox(label= 'Select an year',
                                 options= range(2022,1999,-1) )
```

下一个代码片段展示了从`meteostat`包中实际获取的信息。我们首先设置一个时间框架，使用用户选择的年份。然后我们使用选择的城市从 CSV 中获取经纬度信息，使用简单的 Pandas 查询，将结果数据转换为浮点数。接下来，我们使用`meteostat`中的`Point()`函数创建一个对象，该对象将放入`Monthly()`函数中，后者接收时间框架和位置，创建一个最终对象`data`，用于提取我们应用程序所需的数据。

```py
# Collect the Weather Information
# Set time period
start = datetime(selected_year, 1, 1)
end = datetime(selected_year, 12, 31)

# Create Point
lat = cities.query('city == @selected_city')['latitude'].astype('float').tolist()[0]
long = cities.query('city == @selected_city')['longitude'].astype('float').tolist()[0]
city_loc = Point(lat, long)

# Get daily data for 2018
data = Monthly(city_loc, start, end)
data = data.fetch()
#data['mth'] = data.index.month
data = data[ ['tavg', 'tmax', 'tmin'] ]
```

收集了所需的数据框后，我们将创建两列来绘制选定位置的月度温度线图，并显示包含检索信息的表格。

```py
col1, col2 = st.columns(2, gap='large')
# column 1 - Table weather history
with col1:
    st.text('| TEMPERATURES IN °C')
    st.line_chart(data=data)

# column 2 - Graphics
with col2:
    # WEATHER INFORMATION TABLE
    st.text('| WEATHER HISTORY')
    st.write(data)
```

最后，我们将添加另一个部分，展示一个地图，显示所选位置。

```py
# Division
st.markdown('---')
# Map
st.subheader('| WHERE THIS CITY IS')
df_map = cities.query('city == @selected_city')
st.map(df_map, zoom=5)
```

生成的应用程序被保存为`.py`文件，与 requirement.txt 和城市 csv 一起放在一个[GitHub 存储库中，链接在此](https://github.com/gurezende/weather_app)，这是部署前所需的步骤。

然后我们可以直接去[`share.streamlit.io/`](https://share.streamlit.io/)将应用程序部署到网络上。一旦完成，你将获得应用程序的链接，并将其粘贴到你的 Medium 博客文章中。

在下面的序列中，你可以看到我们在最后几段中刚刚构建的嵌入应用程序。它在你的 Medium 帖子中直接功能齐全*(请耐心等待，可能需要几秒钟才能完全加载)*。

在本教程中创建的天气应用。图片由作者提供。

# 在你离开之前

哇！这真是太棒了。当我看到这个新功能时，我迫不及待地想要测试它并与你分享。希望你也喜欢，并找到很好的方法与大家分享和展示你的工作。

Streamlit 使用起来真的很简单，你可以通过他们的文档或互联网上的许多教程了解更多。我相信你会喜欢用它编写应用程序的简单性。

如果你喜欢这些内容，记得关注我获取更多信息。

[](https://gustavorsantos.medium.com/?source=post_page-----520e98c7d51d--------------------------------) [## Gustavo Santos - Medium

### 阅读 Gustavo Santos 在 Medium 上的文章。他是一名数据科学家，从数据中提取洞察，帮助个人和公司……

gustavorsantos.medium.com](https://gustavorsantos.medium.com/?source=post_page-----520e98c7d51d--------------------------------)

在 [LinkedIn](https://www.linkedin.com/in/gurezende/) 上找到我，或者通过 [TopMate.io 预约时间与我讨论数据科学](https://topmate.io/gustavo_santos)。

# 参考文献

[](https://docs.streamlit.io/streamlit-community-cloud/get-started/embed-your-app?utm_medium=email&_hsmi=259535966&_hsenc=p2ANqtz-9deEEF-Z6E5LeUsWM_TiXef4GoXNX6wpR27Fz5CYkwa9nRbwFaYVnGkLwIy9hmvE_gN6GwZsaFmkGDq8iQFCS3wfRp3g&utm_content=259535966&utm_source=hs_email&source=post_page-----520e98c7d51d--------------------------------#embedding-with-oembed) [## 嵌入你的应用 - Streamlit 文档

### 嵌入 Streamlit Community Cloud 应用可以通过集成交互式、数据驱动的应用程序来丰富你的内容……

docs.streamlit.io](https://docs.streamlit.io/streamlit-community-cloud/get-started/embed-your-app?utm_medium=email&_hsmi=259535966&_hsenc=p2ANqtz-9deEEF-Z6E5LeUsWM_TiXef4GoXNX6wpR27Fz5CYkwa9nRbwFaYVnGkLwIy9hmvE_gN6GwZsaFmkGDq8iQFCS3wfRp3g&utm_content=259535966&utm_source=hs_email&source=post_page-----520e98c7d51d--------------------------------#embedding-with-oembed) [](https://docs.streamlit.io/?source=post_page-----520e98c7d51d--------------------------------) [## Streamlit 文档

### Streamlit 不仅仅是创建数据应用的一种方式，它还是一个创作者社区，分享他们的应用和想法……

docs.streamlit.io](https://docs.streamlit.io/?source=post_page-----520e98c7d51d--------------------------------) [](https://techcrunch.com/2022/03/02/snowflake-acquires-streamlit-for-800m-to-help-customers-build-data-based-apps/?source=post_page-----520e98c7d51d--------------------------------) [## Snowflake 以 8 亿美元收购 Streamlit，帮助客户构建基于数据的应用

### Snowflake 帮助客户在云中存储和管理大量数据，而不受云供应商的锁定。Streamlit 是一个……

[techcrunch.com](https://techcrunch.com/2022/03/02/snowflake-acquires-streamlit-for-800m-to-help-customers-build-data-based-apps/?source=post_page-----520e98c7d51d--------------------------------) [](https://medium.com/gustavorsantos/streamlit-basics-build-your-first-web-app-ffd377a85666?source=post_page-----520e98c7d51d--------------------------------) [## Streamlit 基础知识：构建你的第一个 Web 应用

### 学习如何编写代码以创建每一个基本功能，并在几分钟内部署你的网页应用。

[medium.com](https://medium.com/gustavorsantos/streamlit-basics-build-your-first-web-app-ffd377a85666?source=post_page-----520e98c7d51d--------------------------------) [](https://medium.com/gustavorsantos/streamlit-basics-deploy-your-first-web-app-198934d6f7a0?source=post_page-----520e98c7d51d--------------------------------) [## Streamlit 基础知识：部署你的第一个 Web 应用

### 学习如何使用 Streamlit 的分享功能部署 Web 应用。

[medium.com](https://medium.com/gustavorsantos/streamlit-basics-deploy-your-first-web-app-198934d6f7a0?source=post_page-----520e98c7d51d--------------------------------)
