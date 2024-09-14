# 使用 Python 分析顶级科技 YouTube 频道

> 原文：[`towardsdatascience.com/an-analysis-of-the-top-tech-youtube-channels-with-python-ad42c0291723`](https://towardsdatascience.com/an-analysis-of-the-top-tech-youtube-channels-with-python-ad42c0291723)

## 使用 YouTube API 理解顶级 YouTube 科技频道的表现

[](https://guillaume-weingertner.medium.com/?source=post_page-----ad42c0291723--------------------------------)![Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----ad42c0291723--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ad42c0291723--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad42c0291723--------------------------------) [Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----ad42c0291723--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad42c0291723--------------------------------) ·阅读时间 9 分钟·2023 年 9 月 8 日

--

![](img/3c9e03b46832b007aa1cde6d70ac1d43.png)

[Szabo Viktor](https://unsplash.com/fr/@vmxhu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 的照片，来源于 [Unsplash](https://unsplash.com/fr/photos/UfseYCHvIH0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# #0 YouTube API

你知道 YouTube 有一个 API 吗？你知道这个 API 可以用来获取你需要的所有数据以进行一个酷炫的数据科学项目吗？好吧，现在你知道了。

在这篇文章中，我们将演示如何使用它来获取丰富的数据集，以便分析和比较顶级科技频道。

为了能够请求 YouTube API，我们需要遵循以下步骤：

1.  在你的 [Google Developers Console](https://console.developers.google.com/) 中创建一个新项目——只需要一个 Google 帐户即可完成此操作

1.  在“CREDENTIALS”标签下请求一个 API 密钥，点击“CREATE CREDENTIALS”

1.  通过点击仪表板上的“ENABLE APIS AND SERVICES”启用 YouTube API 服务，然后搜索并勾选“YouTube Data API v3**”**

现在我们已经准备好调用 API 以获取数据。

如果需要，更详细的解释可以在 Google 文档中找到：

[](https://developers.google.com/youtube/v3/getting-started?source=post_page-----ad42c0291723--------------------------------) [## YouTube Data API 概述 | Google for Developers

### 编辑描述

developers.google.com](https://developers.google.com/youtube/v3/getting-started?source=post_page-----ad42c0291723--------------------------------)

现在我们已经准备好一切，让我们开始编码吧！

在这篇文章中，我们将：

1.  获取频道基本信息（创建日期、描述、视频数量、订阅者数量等）

1.  获取该频道的所有视频详细信息（标题、描述、时长、观看次数、点赞数等）

1.  对我们想要调查的 5 个频道进行此操作

1.  在这些数据基础上进行一些（有趣的）分析

为了启动我们的项目，我们首先需要安装 Google API 客户端库（`pip install google-api-python-client`），并导入我们在整个过程中将使用的两个库（pandas 和 Google API 客户端库）。我们还存储了我们的 API 密钥，并构建了这个*youtube*对象，这将允许我们进行 API 调用，如下面的代码所示。

```py
import pandas as pd
from googleapiclient.discovery import build

# API configuration
api_key = 'YOUR API KEY FROM STEP 0'
youtube = build('youtube', 'v3', developerKey=api_key)
```

现在我们准备好了，真的开始吧。

# **#1 获取 MKBHD 频道基本信息**

Google 文档组织良好且易于导航，每个端点都有自己的章节：

[](https://developers.google.com/youtube/v3/docs?source=post_page-----ad42c0291723--------------------------------) [## API 参考 | YouTube 数据 API | Google 开发者]

### YouTube 数据 API 让你可以将通常在 YouTube 网站上执行的功能集成到你自己的网站中，或…

[开发者文档](https://developers.google.com/youtube/v3/docs?source=post_page-----ad42c0291723--------------------------------)

在本节中，我们使用**channels endpoint**。

我们真正需要的只是我们考虑的 YouTube 频道的频道 ID。在这里，我专注于*MKBHD*频道。

频道 ID 曾经在频道页面 URL 中非常显眼，但现在已经不是这样了。然而，在页面的源代码中查找“channel_id”也有效：

![](img/758fd9119e21e3e753313152fc5a684f.png)

如何找到频道 ID —— 作者截图

一旦我们获得了 channel_id，我们可以调用 API 以 JSON 格式获取频道概况数据。我们的目的是从这个 JSON 中获取对我们相关的数据，并将其存储在 DataFrame 中，如下所示：

```py
# Youtube Channel ID
mkhbh_id = 'UCBJycsmduvYEL83R_U4JriQ'

# Put channel overview in a DataFrame
request = youtube.channels().list(part="snippet,contentDetails,statistics", id=mkhbh_id)
response = request.execute()

channel_overview = {
    'title' : response['items'][0]['snippet']['title'],
    'description' : response['items'][0]['snippet']['description'],
    'publishedAt' : response['items'][0]['snippet']['publishedAt'],
    'viewCount' : response['items'][0]['statistics']['viewCount'],
    'subscriberCount' : response['items'][0]['statistics']['subscriberCount'],
    'videoCount' : response['items'][0]['statistics']['videoCount'],
    'uploads' : response['items'][0]['contentDetails']['relatedPlaylists']['uploads']
}

df_channel_overview = pd.DataFrame([channel_overview])
df_channel_overview
```

这是结果：

![](img/18ad70eee8484b38851426a4cec95ea0.png)

MKBHD 频道概况 —— 作者图像

我们现在在 DataFrame 中很好地存储了*MKBHD*频道的概况（标题、描述、创建日期、观看次数、订阅数和视频数）。

“uploads”变量是后续访问频道视频列表的关键。

# #2 获取 MKBHD 频道的所有视频详细信息

在本节中，我们使用**playlistItems**和**videos**端点。

+   **playlistItems** 端点将允许我们获取上面描述的**“uploads”变量中的所有视频 ID**。

+   **videos** 端点将允许我们从视频 ID 获取所有**视频详细信息**。

下面的代码完成了第一部分。我将“uploads”变量存储在我称之为“playlistId”的变量中，这对我来说更有意义。

每次请求 playlistItems 端点会返回最多 50 个视频 ID。如果频道的视频超过 50 个，我们需要进行多次请求。

每次响应将包含一个“nextPageToken”，如果还有更多视频 ID 待获取。这里的思路是首先向 API 发送请求，将视频 ID 添加到列表中，并检查是否有“nextPageToken”。如果有，则开始一个 while 循环，重复请求，直到没有更多“nextPageToken”，这意味着我们已经到达末尾，没有更多视频 ID 待获取。

```py
# Get all the video IDs from the channel and put them in a list
playlistId = df_channel_overview['uploads'].iloc[0]
video_ids = []

request = youtube.playlistItems().list(part="snippet,contentDetails", playlistId=playlistId, maxResults = 50)
response = request.execute()
nextPageToken = response.get('nextPageToken')

for item in response['items']:
    video_ids.append(item['contentDetails']['videoId'])

while nextPageToken is not None:
    request = youtube.playlistItems().list(part="snippet,contentDetails", playlistId=playlistId, maxResults = 50, pageToken = nextPageToken)
    response = request.execute()
    nextPageToken = response.get('nextPageToken')
    for item in response['items']:
        video_ids.append(item['contentDetails']['videoId'])
```

现在我们已经在列表中获得了所有*MKBHD*的视频 ID，我们可以使用**videos endpoint**来获取这些视频的更多细节。我们将借助以下代码将所有这些存储在一个 DataFrame 中：

```py
# Put video details in data frame
videos = []

for i in range(0, len(video_ids), 50):
    request = youtube.videos().list(part="snippet,contentDetails,statistics", id=video_ids[i:i+50])
    response = request.execute()

    for item in response['items']:
        video = {
            'channelTitle' : df_channel_overview['title'].iloc[0],
            'videoId' : item['id'],
            'categoryId' : item['snippet']['categoryId'],
            'publishedAt' : item['snippet']['publishedAt'],
            'title' : item['snippet']['title'],
            'description' : item['snippet']['description'],
            'tags' : item['snippet'].get('tags','no_tags'),
            'duration' : item['contentDetails']['duration'],
            'viewCount' : item['statistics'].get('viewCount',0),
            'likeCount' : item['statistics'].get('likeCount', 0),
            'commentCount' : item['statistics'].get('commentCount',0)
        }
        videos.append(video)

df_videos = pd.DataFrame(videos)
df_videos
```

这个思路类似于我们在后面部分解释的内容。API 每页最多返回 50 个视频，所以我们将视频 ID 列表分成 50 个为一组，并构建一个 for 循环。

就这样。所有视频详情都存储在一个格式良好的 DataFrame 中！

![](img/298023e651d6e4cf9c6a05aace2895f7.png)

MKBHD 视频详情 DataFrame— 作者提供的图片

你可能会注意到“duration”列使用了一种不寻常的格式。让我们将其转换为秒，并进行一些清理：

```py
import isodate
# Convert duration column to seconds
df_videos['duration_sec'] = df_videos['duration'].apply(lambda x: isodate.parse_duration(x).total_seconds())

# Convert specific columns to numeric type
numeric_columns = ['viewCount', 'likeCount', 'commentCount', 'duration_sec']
df_videos[numeric_columns] = df_videos[numeric_columns].apply(pd.to_numeric, errors='coerce')

# Convert column to Datetime to access the year
df_videos['publishedAt'] = pd.to_datetime(df_videos['publishedAt'])
df_videos['year'] = df_videos['publishedAt'].dt.year
```

我们完成了。

在本节末尾，我们现在拥有一个特定频道的 2 个 DataFrame：

1.  **df_channel_overview** — 包含频道概述信息

1.  **df_videos** — 列出频道中的所有视频

# **#3 5 个 Youtube 频道**

唯一剩下的就是遍历我们分析中要考虑的不同频道 ID，以获取 df_channel_overview DataFrame 和 df_videos DataFrame，并填充所有数据。

我们在这个项目中考虑的 5 个科技频道是：

1.  *MKBHD*

1.  *Unbox Therapy*

1.  *Linus Tech Tips*

1.  *Dave 2D*

1.  *Austin Evans*

最终，这就是我们 2 个数据集的样子：

**1- df_channel_overview**

![](img/d9a92e1f0e6c8e2af0b24a288bda4391.png)

YouTube 上 5 个最大科技频道的概览 — 来源：YouTube API — 作者提供的图片

**2- df_videos**

![](img/0c08e9d5a2d9ea49bebba853758a051a.png)

所有考虑中的科技频道的视频 DataFrame — 来源：YouTube API — 作者提供的图片

现在，让我们基于这些数据构建一些漂亮的图表！

# #4 频道分析与比较

从 5 个频道的概览数据中，已经可以揭示出一些有趣的见解。

![](img/2142739713f044e4ebfb2feebceff1dc.png)

前十大科技频道的观看数与视频数 — 作者提供的图片

上面的图表显示，*Linus Tech Tips*的频道视频和观看数远超其他频道，但*MKHD*每个视频的平均观看数更高，紧随其后的是*Unbox Therapy*。

“videos” DataFrame 使我们可以更深入分析，每年的视频发布数量讲述了一个有趣的故事。我们可以排除 2023 年，因为这一年尚未结束，因此下图中的下降。

![](img/4ca12481c197007187ef1eb1e4f66a5b.png)

看起来*MKBHD*、*Austin Evans*和*Dave 2D*已经找到他们多年来的巡航节奏，每年平均发布约 100 个视频。

**Unbox Therapy** 另一方面在 2016 年将视频制作量提高到每年 200 多个视频，之后在 2022 年缓慢减少到 100 个视频。也许工作量太大了？不过，对 **Linus Tech Tips** 来说，每年发布 400 到 500 个视频！

制作大量视频是好的，但它们在观看次数方面表现如何？

![](img/97a6890896ecc5cf3ee3b7117ad9d9c8.png)

每个视频的平均观看次数与发布时间的关系——作者提供的图像

每个视频的平均观看次数与视频发布时间的关系，可以指示每个频道所遵循的趋势。但有一个问题。如果我在 2023 年观看了 2020 年发布的视频，这个观看次数将在上面的图表中显示为 2020 年。因此，这并不真正显示频道随时间的观看次数，而是发布年份的每个视频的观看次数。

值得注意的是，自 2017 年以来，**MKBHD** 制作的视频的平均观看次数在 400 万到 500 万之间，而 **Linus Tech Tips** 在同一时期内每个视频的平均观看次数在 200 万到 250 万之间。

**Unbox Therapy** 在 2018 年达到了巅峰，但此后一直在下降。

那么视频时长呢？视频的长度与观看次数之间是否存在相关性？

![](img/4ff38cc0bfae39baa280a32256300d8c.png)

顶级科技频道的观看次数与时长——作者提供的图像

仅通过绘制散点图很难判断，但没有明确的分界线。似乎完全没有相关性。然而，根据以下图表，并不是所有的视频创作者在视频时长方面采用了相同的策略。

![](img/830c06c38ed1e5c7f9026f7a9b039f4e.png)

顶级科技频道每年发布的视频的平均时长——作者提供的图像

**MKBHD** 和 **Unbox Therapy** 现在正在制作较短的视频，而 **Linus Tech Tips** 不仅制作了更多视频，而且视频时长也更长。

这可能是因为一些频道现在发布了“短视频”，这将降低给定年份视频的平均时长。

# #5 结论

这篇文章仅触及了可以利用这些丰富数据集进行的分析的一些方面。

其他分析思路可能包括：

1.  调查点赞与观看次数（或评论与观看次数）之间的相关性。

1.  隔离 2021 年在 YouTube 上出现的“短视频”，看看它们与常规视频的表现相比如何。

1.  确定在平衡“短视频”和常规视频方面的最佳策略。

这表明，理解 API 及其如何被利用来从平台上提取数据，可以有助于一些出色的数据科学项目的发展！

*感谢阅读完整篇文章。*

关注以获取更多信息！

如果有任何问题或意见，请随时在下面留言，或通过 [**LinkedIn**](https://www.linkedin.com/in/guillaume-weingertner-a4a27972/) / [**X**](https://twitter.com/GuillaumeWein) 联系我！

[](https://guillaume-weingertner.medium.com/membership?source=post_page-----ad42c0291723--------------------------------) [## 通过我的推荐链接加入 Medium - Guillaume Weingertner

### 作为 Medium 会员，你的会员费用的一部分将会分配给你阅读的作者，同时你可以完全访问每一个故事……

guillaume-weingertner.medium.com](https://guillaume-weingertner.medium.com/membership?source=post_page-----ad42c0291723--------------------------------)
