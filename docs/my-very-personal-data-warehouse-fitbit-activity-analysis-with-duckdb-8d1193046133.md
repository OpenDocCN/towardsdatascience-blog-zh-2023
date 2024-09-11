# 我的（非常）个人数据仓库

> 原文：[https://towardsdatascience.com/my-very-personal-data-warehouse-fitbit-activity-analysis-with-duckdb-8d1193046133?source=collection_archive---------3-----------------------#2023-05-31](https://towardsdatascience.com/my-very-personal-data-warehouse-fitbit-activity-analysis-with-duckdb-8d1193046133?source=collection_archive---------3-----------------------#2023-05-31)

## 使用DuckDB分析Fitbit活动数据

[](https://simon-aubury.medium.com/?source=post_page-----8d1193046133--------------------------------)[![Simon Aubury](../Images/fb757b7175c211450dcfa7249549c31e.png)](https://simon-aubury.medium.com/?source=post_page-----8d1193046133--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8d1193046133--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8d1193046133--------------------------------) [Simon Aubury](https://simon-aubury.medium.com/?source=post_page-----8d1193046133--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb7b3bb643843&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmy-very-personal-data-warehouse-fitbit-activity-analysis-with-duckdb-8d1193046133&user=Simon+Aubury&userId=b7b3bb643843&source=post_page-b7b3bb643843----8d1193046133---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d1193046133--------------------------------) · 13分钟阅读 · 2023年5月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8d1193046133&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmy-very-personal-data-warehouse-fitbit-activity-analysis-with-duckdb-8d1193046133&user=Simon+Aubury&userId=b7b3bb643843&source=-----8d1193046133---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8d1193046133&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmy-very-personal-data-warehouse-fitbit-activity-analysis-with-duckdb-8d1193046133&source=-----8d1193046133---------------------bookmark_footer-----------)![](../Images/d002b45123ebfc0803be779dbe0d1fe1.png)

图片由[Jake Hills](https://unsplash.com/es/@jakehills?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 可穿戴健身追踪器已成为我们生活中不可或缺的一部分，收集并跟踪我们日常活动、睡眠模式、位置、心率等数据。我已经使用 Fitbit 设备 6 年来监测我的健康。然而，我一直觉得数据分析功能不足 — 尤其是在我想跟踪长期健身目标的进展时。我的个人健身活动数据存档中埋藏了哪些见解？要开始探索，我需要一种有效的方法来对成千上万的记录不完善的 JSON 和 CSV 文件进行数据分析……额外加分的是分析过程中不需要将我的数据从笔记本电脑上移走。

进入 [DuckDB](https://duckdb.org/why_duckdb) — 一个轻量级、免费但强大的分析数据库，旨在简化数据分析工作流 — 它在本地运行。在这篇博客中，我想使用 DuckDB 来探索我的 Fitbit 数据，并分享使用 [Seaborn](https://seaborn.pydata.org/) 数据可视化的各种数据格式分析方法以及绘制我的健康和健身目标的途径。

# 导出 Fitbit 数据存档

首先，我需要获取我所有的历史健身数据。通过遵循 [导出您的账户存档](https://www.fitbit.com/settings/data/export) 的说明，Fitbit 使得导出您账户生命周期中的 Fitbit 数据变得相当简单。

![](../Images/aa73415b842cc76279e913c0efaac903.png)

使用导出 Fitbit 数据存档的说明 — 作者截图。

您需要确认您的请求……并保持耐心。我的存档创建了超过三天 — 但我最终收到了含有下载 ZIP 文件说明的电子邮件。该文件应包含由我的 Fitbit 或相关服务记录的所有个人健身活动。解压存档后会显示出大量的文件 — 例如，我在解压 79MB 文件后总共有 7,921 个文件。

![](../Images/ce1b07a83af019cead0416c777cb7f8f.png)

数以千计的嵌套文件中的一小部分 — 作者截图。

让我们开始查看存档中可用的数据种类。

# 为什么选择 DuckDB？

有许多优秀的博客 ([1](https://betterprogramming.pub/duckdb-whats-the-hype-about-5d46aaa73196),[2](https://mattpalmer.io/posts/whats-the-hype-duckdb/),[3](/a-serverless-query-engine-from-spare-parts-bd6320f10353)) 描述了 DuckDB — [TL;DR](https://www.dictionary.com/browse/tl-dr) 摘要是 DuckDB 是一个开源的内存 OLAP 数据库，专为分析查询而构建。它本地运行，支持广泛的 SQL，并能直接在 Pandas 数据、Parquet、JSON 数据上运行查询。额外加分的是它与 Python 和 R 的无缝集成。它的极速处理能力和大部分内存处理使其成为构建个人数据仓库的好选择。

# Fitbit 活动数据

我查看的第一个文件集合是活动数据。物理活动和广泛的锻炼信息似乎存储在编号的文件中，例如 `Physical Activity/exercise-1700.json`。

我无法弄清楚文件编号实际意味着什么，我猜这些编号只是用于一组锻炼文件的递增整数。在我的数据导出中，最早的文件从 0 开始，经过 6 年的时间到达文件编号 1700。里面是一个记录数组，每个记录都有一个活动的描述。记录似乎会根据活动的不同而变化——这是一个“步行”的示例。

```py
"activityName" : "Walk",
  "averageHeartRate" : 79,
  "calories" : 122,
  "duration" : 1280000,
  "steps" : 1548,
  "startTime" : "01/06/23 01:08:57",
  "elevationGain" : 67.056,
  "hasGps" : false,
  : : : :
  "activityLevel" : [
    { "minutes" : 1, "name" : "sedentary"},
    { "minutes" : 2, "name" : "lightly"},
    { "minutes" : 6, "name" : "fairly"},
    { "minutes" : 6, "name" : "very"
  }]
```

这些物理活动数据是我笔记本电脑上7,921个文件中的一个。幸运的是，DuckDB 可以使用 [read_json](https://duckdb.org/docs/data/json/overview.html#read_json_auto-function) 函数从 JSON 文件中读取（并自动检测模式），让我可以通过一个 SQL 语句将所有锻炼文件加载到 `physical_activity` 表中。值得注意的是，我需要指定日期格式掩码，因为 Fitbit 导出的日期格式非常 [美国风格](https://en.wikipedia.org/wiki/Date_and_time_notation_in_the_United_States) 😕。

```py
CREATE OR REPLACE TABLE physical_activity
as
SELECT 
  startTime + INTERVAL 11 hours as activityTime
, activityName
, activityLevel
, averageHeartRate
, calories
, duration / 60000 as duration_minutes
, steps
, distance
, distanceUnit
, tcxLink
, source
FROM read_json('./Physical Activity/exercise-*.json'
, format='array'
, timestampformat='%m/%d/%y %H:%M:%S');
```

这个 SQL 命令从磁盘读取物理活动数据，将活动和持续时间转换并加载到内存中的 DuckDB 表中。

# 将物理活动数据加载到数据框中

我想了解我每个月的时间花费情况。由于活动数据存储在非常细粒度的级别上，我使用了 DuckDB SQL [time_bucket](https://duckdb.org/docs/sql/functions/timestamp.html) 函数将 *activityTime* 时间戳截断为每月的桶。将分组的物理活动数据加载到数据框中可以通过这个汇总 SQL 完成，查询结果可以通过 << 运算符导入 Pandas 数据框中。

```py
activity_df <<
  select time_bucket(interval '1 month', activityTime) as activity_day
  , activityName
  , sum(duration_minutes) as duration
  from physical_activity
  where activityTime between '2022-09-01' and '2023-05-01'
  group by 1, 2
  order by 1;
```

这个 SQL 查询将我的活动数据（骑车、步行、跑步等）分组到每月的桶中，让我可以真实地反映我投入到物理活动中的时间。

# 绘制每月活动分钟数

我现在想要通过视觉方式探索我的活动数据——所以我们来获取 Fitbit 数据并制作一些统计图形。我将使用 Python 的 [Seaborn](https://seaborn.pydata.org/) 数据可视化库直接从 *activity_df* 数据框中创建一个每月活动分钟数的条形图。

```py
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib.dates import DateFormatter
plt.figure(figsize=(15, 6))
plt.xticks(rotation=45)

myplot =sns.barplot(data=activity_df, x="activity_day", y="duration", hue="activityName")
myplot.set(xlabel='Month of', ylabel='Duration (min)', title='Monthly Activity Minutes')
plt.legend(loc="upper right", title='Activity') 
plt.show()
```

执行此操作会生成这个条形图。

![](../Images/0e444629dea3ac0ca0427b4ec89659ba.png)

锻炼活动细分——作者截图。

看起来我的主要活动仍然是步行，而且我在2023年的新年决心是更频繁地跑步，但实际上并没有发生（还？）。

# 睡眠

关于 [三分之一的成年人睡眠不足](https://www.health.harvard.edu/heart-health/are-you-getting-enough-sleep)，所以我想探索我的长期睡眠模式。在我的Fitbit档案中，睡眠数据似乎被记录在以日期命名的文件中，例如`Sleep/sleep-2022-12-28.json`。每个文件包含一个月的数据，但混淆的是，文件的日期为事件发生前的月份。例如，文件`sleep-2022-12-28.json`似乎包含了2023年1月2日至2023年1月27日的数据。不管怎样 — 文件命名的奇怪之处暂且不提，我们可以探讨文件的内容。在记录中有一个扩展的“levels”块，详细描述了睡眠类型（清醒、浅睡、快速眼动、深睡）。

```py
"logId" : 39958970367,
  "startTime" : "2023-01-26T22:47:30.000",
  "duration" : 26040000,
  :: :: ::
  "levels": 
    "summary" : {
      {
      "light": { "count": 30, "minutes": 275},
      "rem": { "count": 4, "minutes": 48 },
      "wake": { "count" : 29, "minutes" : 42 },
      "deep" : { "count" : 12, "minutes" : 75}
      }
    }
```

如果查看一些较旧的文件（可能是用我以前的Fitbit Surge设备创建的），会发现睡眠类型的分类有所不同（躁动、不清醒、睡眠）。

```py
"logId" : 18841054316,
  "startTime" : "2018-07-12T22:42:00.000",
  "duration" : 25440000,
  :: :: ::
  "levels" : {
    "summary" : {
      "restless" : {"count" : 9, "minutes" : 20 },
      "awake" : { "count" : 2, "minutes" : 5 },
      "asleep" : { "count" : 0,   "minutes" : 399}
    }
  }
```

无论数据模式如何，我们都可以使用 [DuckDB JSON](https://duckdb.org/docs/extensions/json.html) 读取器将记录读入一个表格中。

```py
CREATE OR REPLACE TABLE sleep_log
as
select dateOfSleep 
, levels
from read_json('./Sleep/sleep*.json'
, columns={dateOfSleep: 'DATE', levels: 'JSON'}
, format='array') ;
```

# 睡眠数据的模式变化

我想处理我所有的睡眠数据，并处理记录睡眠的模式变化（很可能是因为我更换了Fitbit设备的型号）。一些记录的时间标记在`$.awake`上，这与`$.wake`类似（但不完全相同）。

我使用了SQL中的 [coalesce](https://duckdb.org/docs/sql/functions/utility.html) 函数 — 它返回第一个计算结果为非NULL值的表达式，以结合类似类型的睡眠阶段。

```py
sleep_log_df <<
  select dateOfSleep
  , cast(coalesce(json_extract(levels, '$.summary.awake.minutes'), json_extract(levels, '$.summary.wake.minutes')) as int) as min_wake
  , cast(coalesce(json_extract(levels, '$.summary.deep.minutes'), json_extract(levels, '$.summary.asleep.minutes')) as int) as min_deep
  , cast(coalesce(json_extract(levels, '$.summary.light.minutes'), json_extract(levels, '$.summary.restless.minutes')) as int) as min_light
  , cast(coalesce(json_extract(levels, '$.summary.rem.minutes'), 0) as int) as min_rem
  from sleep_log
  where dateOfSleep between '2023-04-01' and '2023-04-30'
  order by 1;
```

使用DuckDB，我可以通过 [json_extract](https://duckdb.org/docs/extensions/json.html#json-extraction-functions) 提取嵌套JSON中的时长阶段，以生成一个 *sleep_log_df* 数据框，将所有历史睡眠阶段进行分组。

# 绘制睡眠活动图

我们可以将每日睡眠日志制作成堆叠条形图，显示每晚清醒、浅睡、深睡和 [REM](https://en.wikipedia.org/wiki/Rapid_eye_movement_sleep) 睡眠的分类。

```py
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.dates as mdates

#create stacked bar chart
fig, axes = plt.subplots(figsize=(15,6))
myplot = sleep_log_df.set_index('dateOfSleep').plot(ax=axes, kind='bar', stacked=True, color=['chocolate', 'palegreen', 'green', 'darkblue'])
myplot.set(xlabel='Date', ylabel='Duration (min)', title='Sleep')
axes.xaxis.set_major_locator(mdates.DayLocator(interval=7))
plt.legend(loc="upper right", labels = ['Awake', 'Deep', 'Light', 'REM']) 
plt.xticks(rotation=45)
plt.show()
```

加载一个月的睡眠数据让我能够进行更广泛的睡眠时长分析。

![](../Images/3e9cfbb697bb69a1449463626e94ab1e.png)

每晚的睡眠周期时长 — 作者截图。

将多晚的睡眠数据绘制在一个图表上，使我能够开始理解星期几和周期性事件如何影响我的睡眠时长和质量。

# 心率

心率数据被非常频繁地捕捉（每`10–15秒`一次），存储在名为`Physical Activity/heart_rate-2023-01-26.json`的每日文件中。这些文件非常大 — 每天约有70,000行 — 所有数据都包装在一个数组中。

```py
[{{"dateTime": "01/25/25 13:00:07", "value": {"bpm": 54, "confidence": 2}},
  {"dateTime": "01/25/25 13:00:22", "value": {"bpm": 54, "confidence": 2}},
  {"dateTime": "01/25/25 13:00:37", "value": {"bpm": 55, "confidence": 2}},
  : : : : : :
  {"dateTime": "01/26/26 12:59:57", "value": {"bpm": 55, "confidence": 3}
}]
```

我的理论是文件名表示用户的时区。例如，在我的时区（GMT+11），命名为`heart_rate-2023-01-26.json`的数据覆盖了26日00:00（AEST）至23:59（AEST） - 如果文件中的日期为GMT，则逻辑上是合理的。

# 转换JSON文件

到目前为止，我已经成功处理了包含 DuckDB 函数的 Fitbit 数据。然而，在处理这些巨大的心率文件时，我遇到了问题。当尝试处理 JSON 文件中的大数组记录时，DuckDB 给出了这个错误。

**(duckdb.InvalidInputException) “INTERNAL Error: Unexpected yyjson tag in ValTypeToString”**

我认为这个错误信息是一个突如其来的提醒，告诉我期望一个 JSON 数组有这么多元素是不合理的。解决办法是预处理文件，使其不是 JSON 记录数组，而是转换为换行符分隔的 JSON 或 [ndjson](http://ndjson.org/)。

```py
{"dateTime": "01/25/23 13:00:07", "value": {"bpm": 54, "confidence": 2}
{"dateTime": "01/25/23 13:00:22", "value": {"bpm": 54, "confidence": 2}
{"dateTime": "01/25/23 13:00:37", "value": {"bpm": 55, "confidence": 2}
  : : : : : :
{"dateTime": "01/26/23 12:59:57", "value": {"bpm": 55, "confidence": 3}
```

为了将心率数组记录转换为换行符分隔的 JSON，我使用了一点小巧的 Python 代码来转换每个文件。

```py
import glob
import json
import ndjson
import re

for json_src_file in sorted(glob.glob('./Physical Activity/steps-*.json')):
  json_dst_file = re.sub('\.[a-z]*$', '.ndjson', json_src_file)
  print(f'{json_src_file} -->  {json_dst_file}')
  with open(json_src_file) as f_json_src_file:
    json_dict =json.load(f_json_src_file) 
    with open(json_dst_file, 'w') as outfile:
      ndjson.dump(json_dict, outfile)
```

这将查找每个 *.json* 文件，读取内容并将其转换为换行符分隔的 JSON，并用 *.ndjson* 文件扩展名创建新文件。这将一个包含70,000条记录的数组转换为一个包含70,000行的文件——每条 JSON 记录现在存储在新的一行上。

# 将心率数据加载到表中

使用新转换的 *ndjson* 文件，我现在准备将心率数据加载到 DuckDB 表中。注意使用 `timestampformat='%m/%d/%y %H:%M:%S');` 来描述日期中的前导月份（例如 *"01/25/23 13:00:07"*)

```py
CREATE OR REPLACE TABLE heart_rate
as
SELECT dateTime + INTERVAL 11 hours as hr_date_time
, cast(value->'$.bpm' as integer) as bpm
FROM read_json('./Physical Activity/*.ndjson'
, columns={dateTime: 'TIMESTAMP', value: 'JSON'}
, format='newline_delimited'
, timestampformat='%m/%d/%y %H:%M:%S');
```

我们可以通过将格式设置为 ’newline_delimited’ 来加载所有 .ndjson 文件。注意我们可以通过 JSON 提取来提取 BPM（每分钟心跳次数）并将其转换为整数。

![](../Images/3085503bcdcd2945270d26a383b88a08.png)

DuckDB 在处理 JSON 时非常快速 — 作者截图。

值得在这里强调 DuckDB 的速度是多么惊人——加载 1200 万条记录仅用了 2.8 秒！

# 将心率加载到数据框中

在加载了 1200 万条心率测量记录后，我们将 5 月 21 日的一天数据加载到数据框中。

```py
hr_df << 
  SELECT time_bucket(interval '1 minutes', hr_date_time) as created_day
  ,  min(bpm) as bpm_min
  ,  avg(bpm) as bpm_avg
  ,  max(bpm) as bpm_max
  FROM heart_rate
  where hr_date_time between '2023-05-21 00:00' and '2023-05-21 23:59'
  group by 1;
```

这个 DuckDB 查询将心率的变异性聚合到 1 分钟的时间块中；在每个周期内进行最小值、平均值和最大值的分类。

# 绘制心率图

我可以使用这样的图来绘制心率（顺便炫耀一下，我确实在早上 6 点去跑步了）

```py
import matplotlib.pyplot as plt
from matplotlib.dates import DateFormatter
plt.figure(figsize=(15, 6))
plt.xticks(rotation=45)

myplot = sns.lineplot(data=hr_df, x="created_day", y="bpm_min")
myplot = sns.lineplot(data=hr_df, x="created_day", y="bpm_avg")
myplot = sns.lineplot(data=hr_df, x="created_day", y="bpm_max")
myFmt = DateFormatter("%H:%M")
myplot.xaxis.set_major_formatter(myFmt)
myplot.set(xlabel='Time of day', ylabel='Heart BPM', title='Heart rate')
plt.show()
```

![](../Images/9962db0f04ea299fd471bc8b221b5da7.png)

一天中的心率 — 作者截图。

以细粒度探索心率使我能够跟踪我的健身目标——特别是如果我坚持我的常规跑步计划。

## 步骤

步骤记录在名为 `Physical Activity/steps-2023-02-26.json` 的每日文件中。这似乎是对一天中每个时间段块（每 5 到 10 分钟）内的步骤的详细计数。

```py
[{
  "dateTime" : "02/25/23 13:17:00",
  "value" : "0"
},{
  "dateTime" : "02/25/23 13:52:00",
  "value" : "5"
},{
  "dateTime" : "02/25/23 14:00:00",
  "value" : "0"
},{
:: :: ::
},{
  "dateTime" : "03/24/23 08:45:00",
  "value" : "15"
}]
```

为了将步骤聚合为每日统计，我需要将 GMT 转换为我的本地时区（GMT+11）。

```py
steps_df <<
select cast(time_bucket(interval '1 day', dateTime + INTERVAL 11 hours	) as DATE) as activity_day
, sum(value) as steps
from read_json('./Physical Activity/steps-2023-04-27.ndjson'
, auto_detect=True
, format='newline_delimited'
, timestampformat='%m/%d/%y %H:%M:%S') 
group by 1;
```

将每日步骤聚合到 *steps_df* 数据框中，使我能够探索长期的活动趋势，因为我努力超越 10,000 步以实现[健康益处的提升](https://www.10000steps.org.au/articles/healthy-lifestyles/health-check-do-we-really-need-take-10000-steps-day/)。

# 绘制每日步骤

现在我们可以将数据框绘制为每日步数

```py
import matplotlib.pyplot as plt
from matplotlib.dates import DateFormatter
plt.figure(figsize=(15, 6))
plt.xticks(rotation=45)

myplot = sns.barplot(data=steps_df, x="activity_day", y="steps")
myplot.set(xlabel='Day', ylabel='Steps', title='Daily steps')
plt.show()
```

![](../Images/8a95077d4b6046f60e34eef75c8ff1de.png)

每日步数统计——作者截图。

这显示我还需要努力达到我的每日步数目标——这对我的新年健身决心来说又是一次打击。

# GPS 映射

Fitbit 将 GPS 记录的活动存储为[TCX (训练中心 XML)](https://en.wikipedia.org/wiki/GPS_Exchange_Format) 文件。这些 XML 文件在下载的 ZIP 文件中*没有*，但我们在身体活动文件中有其位置的参考，我可以像这样查询。

```py
select tcxLink 
from physical_activity
where tcxLink is not null;
```

tcxLink 字段是对身体活动文件中位置的 URL 参考。

![](../Images/6037d84f1ca242c03cced5f82d2e7f4f.png)

每个 TCX 文件的 URL——作者截图。

我们可以直接在浏览器中使用这个 URL（登录 Fitbit 网站后）来下载 GPS XML 文件。查看 TCX 文件内部，我们会发现每隔几秒钟就有低级别的 GPS 位置数据。

![](../Images/c850bc9f8b7d837fb45767fd1fa6780c.png)

TCX GPS XML 文件样本内容——作者截图。

好消息是数据中有一些明显的字段，如纬度、经度和时间。不太好的消息是数据是 XML 格式的，因此我们需要在加载到 DuckDB 之前预处理这些文件，因为目前 XML 格式不被文件读取器支持。我们可以通过另一段 Python 代码将 XML 文件转换为 JSON 文件，循环遍历每个 *.tcx* 文件。

这里有一些复杂的 XML 嵌套，位置数据位于 *TrainingCenterDatabase/Activities/Activity/Lap* 下。

```py
import glob
import json
import ndjson
import xmltodict
import re

for xml_src_file in sorted(glob.glob('MyFitbitData/tcx/*.tcx')):
    json_dst_file = re.sub('\.[a-z]*$', '.ndjson', xml_src_file)
    print(f'{xml_src_file} -->  {json_dst_file}')
    with open(xml_src_file) as f_xml_src_file:
        # erase file if it exists
        open(json_dst_file, 'w') 
        data_dict = xmltodict.parse(f_xml_src_file.read())
        # Loop over the "laps" in the file; roughly every 1km
        for lap in data_dict['TrainingCenterDatabase']['Activities']['Activity']['Lap']:
            data_dict_inner = lap['Track']['Trackpoint']
            # append file
            with open(json_dst_file, 'a') as outfile:
                ndjson.dump(data_dict_inner, outfile)
                outfile.write('\n')
```

## 加载 GPS 地理空间数据

我们可以这样加载地理空间数据

```py
route_df <<
SELECT time
, position
, cast(json_extract_string(position, '$.LatitudeDegrees') as float) as latitude
, cast(json_extract_string(position, '$.LongitudeDegrees') as float) as longitude
FROM read_json('MyFitbitData/tcx/54939192717.ndjson'
, columns={Time: 'TIMESTAMP', Position: 'JSON', AltitudeMeters: 'FLOAT', DistanceMeters: 'FLOAT', HeartRateBpm: 'JSON'}
, format='newline_delimited'
, timestampformat='%Y-%m-%dT%H:%M:%S.%f%z');
```

这个 DuckDB 查询扁平化了 JSON，将纬度、经度和时间转换为正确的数据类型，并加载到 *route_df* 数据框中。

# 使用 Folium 可视化 GPS 路线

拥有一个位置信息的表格并不够直观，因此我想开始在交互式地图上绘制我的跑步路线。我使用了这篇博客来帮助[使用 Folium 可视化路线](https://betterdatascience.com/data-science-for-cycling-how-to-visualize-gpx-strava-routes-with-python-and-folium/)。修改代码帮助我绘制了自己的跑步路线，例如这是我在[堪培拉](https://en.wikipedia.org/wiki/Canberra)度假时的一次跑步路线图。

```py
import folium

route_map = folium.Map(
    location=[-35.275, 149.129],
    zoom_start=13,
    tiles='openstreetmap',
    width=1024,
    height=600
)

coordinates = [tuple(x) for x in route_df[['latitude', 'longitude']].to_numpy()]
folium.PolyLine(coordinates, weight=8, color='red').add_to(route_map)
display(route_map)
```

![](../Images/9d1e52b607975ce6d097d1cb3e5b5d7f.png)

跑步的 Folium 地图图示——作者截图。

这生成了一个使用[开放街图](https://openmaptiles.org/) 瓦片的跑步图，给我提供了一个很好的交互式详细跑步地图。

# 数据目标和健身目标总结

我是否更接近分析我的 Fitbit 设备数据的目标——绝对是的！DuckDB 证明是一个理想的灵活轻量级分析工具，能够快速处理我的大量混乱的 Fitbit 数据。通过广泛的 SQL 支持和灵活的文件解析选项，DuckDB 能够在几秒钟内处理数百万条记录，将数据本地导入数据框，这使得 DuckDB 成为构建个人数据仓库的理想选择。

至于我的健身目标——我还有一些工作要做。我觉得我现在应该离开这个博客，因为我今天的步数目标还差一点。

# Code

🛠️用于Fitbit活动分析的代码——[https://github.com/saubury/duckdb-fitbit](https://github.com/saubury/duckdb-fitbit)
