# 时间旅行轻松掌握：Python Datetime 的全面指南

> 原文：[https://towardsdatascience.com/time-travel-made-easy-a-comprehensive-guide-to-python-datetime-326dd1c57391?source=collection_archive---------2-----------------------#2023-05-04](https://towardsdatascience.com/time-travel-made-easy-a-comprehensive-guide-to-python-datetime-326dd1c57391?source=collection_archive---------2-----------------------#2023-05-04)

## 可能是你对 Python Datetime 所需的一切 [⌛](https://emojiterra.com/time/)

[](https://medium.com/@andreas030503?source=post_page-----326dd1c57391--------------------------------)[![安德烈亚斯·卢基塔](../Images/8660ca1fea5da34ce3475281c1f52152.png)](https://medium.com/@andreas030503?source=post_page-----326dd1c57391--------------------------------)[](https://towardsdatascience.com/?source=post_page-----326dd1c57391--------------------------------)[![面向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----326dd1c57391--------------------------------) [安德烈亚斯·卢基塔](https://medium.com/@andreas030503?source=post_page-----326dd1c57391--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F955ef38ea7b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-travel-made-easy-a-comprehensive-guide-to-python-datetime-326dd1c57391&user=Andreas+Lukita&userId=955ef38ea7b&source=post_page-955ef38ea7b----326dd1c57391---------------------post_header-----------) 发表在 [面向数据科学](https://towardsdatascience.com/?source=post_page-----326dd1c57391--------------------------------) · 11 分钟阅读 · 2023年5月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F326dd1c57391&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-travel-made-easy-a-comprehensive-guide-to-python-datetime-326dd1c57391&user=Andreas+Lukita&userId=955ef38ea7b&source=-----326dd1c57391---------------------clap_footer-----------)

--

![](../Images/6ee934bfa819f8d3081f3b0aa36d2e4d.png)

图片由 [Zulfa Nazer](https://unsplash.com/@zul_naz?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

处理具有日期和时间的数据可能会令人感到难以应对，尤其是当你不太熟悉`datetime`操作的细节时。许多术语，如`DatetimeIndex`、`Timestamp`、`Timedelta`、`Timezone`和`Offset`，可能会让中级分析师感到困惑，难以理解和记忆。本指南将帮助你掌握`datetime`操作，并从数据中解锁强大的洞察。让我们开始吧！

Python标准库中的`**datetime**`模块提供了可以处理日期、时间和时间间隔的类[¹](#ac6d)。该模块在数据分析中尤其重要，因为日期和时间通常是数据的关键组成部分，而准确地操作它们对于时间序列分析和金融建模等项目至关重要。通过使用`datetime`，分析师可以更好地理解数据中的时间趋势和模式，从而从数据集中获得更准确的洞察和预测。`**datetime**`模块下的6个类包括`**date**`、`**time**`、`**datetime**`、`**timedelta**`、`**tzinfo**`和`**timezone**`。

**内容目录**

+   [带时区与无时区](#9ae9) `[**datetime**](#9ae9)` [对象](#9ae9)

+   [协调世界时间 (UTC)，时间，时区，偏移量](#3387)

+   [属性和方法](#15c7) `[**datetime**](#15c7)` [对象，ISO 8601 标准](#15c7)

+   [格式化](#b3e8) `[**datetime**](#b3e8)` [对象，使用](#b3e8) `[**strftime**(format)](#b3e8)` [和将字符串解析为](#b3e8) `[datetime](#b3e8)` [对象，使用](#b3e8) `[**strptime**(format)](#b3e8)`

+   `[**timedelta**](#18df)` [对象](#18df)

+   [POSIX 时间戳](#0d93)

+   [Pandas,](#18ee) `[**.dt**](#18ee)` [访问器和](#18ee) `[**datetime64[ns]**](#18ee)`

+   [将日期作为索引，](#e9d4) `[**.resample(), .agg(), .transform()**](#e9d4)`[方法](#e9d4)

# 带时区与无时区`datetime`对象

简单来说，一个**带时区的日期时间对象**包含时区信息，使得特定日期和时间的时区变得明确[¹](#ac6d)。要创建一个带时区的日期时间对象，需要借助`**pytz**`模块将时区对象附加到日期时间对象上。

```py
from datetime import datetime
import pytz

tz = pytz.timezone('Asia/Singapore')
dt = datetime(2023, 5, 4, 10, 30, 0, tzinfo=tz)
```

这将创建一个表示2023年5月4日新加坡时间上午10:30的带时区`datetime`对象。`tzinfo`参数指定了`datetime`对象的时区。打印`dt`将给出以下信息`datetime.datetime(2023, 5, 4, 10, 30, **tzinfo**=<DstTzInfo ‘Asia/Singapore’ LMT+6:55:00 STD>)`。

另一方面，一个**无时区的日期时间对象**不包含时区信息。它确实表示日期和时间，但不清楚这些日期和时间指的是哪个时区[¹](#ac6d)。

```py
from datetime import datetime

dt = datetime(2023, 5, 4, 10, 30, 0)
```

调用属性`dt.tzinfo`和`dt.utcoffset`将返回`**None**`。

值得注意的是，感知的`**datetime**`对象内部始终是 UTC 时间，并在显示或用于计算时调整到指定的时区。这意味着你可以直接比较来自不同时区的感知`**datetime**`对象，因为它们内部都是以 UTC 时间表示的。通常情况下，最好在可能的情况下使用感知的`**datetime**`对象，特别是在处理来自不同时区的数据的应用中。

# 协调世界时（UTC）、时间、时区和偏移量

**协调世界时（UTC）**是全球用于调节时钟和时间的主要时间标准。1972年之前，它被称为格林威治标准时间（GMT）[²](#7fc1)。UTC 是一个全球公认且协调的时间标准，对于国际通信、导航和科学研究至关重要。值得注意的是，UTC 不受夏令时的影响，使其成为时间相关活动的稳定参考点。相反，UTC 基于原子钟，并根据需要通过添加或减去闰秒来与地球的自转保持同步[³](#cd28)。因此，UTC 时间在全球范围内保持一致，无论不同时间区的本地时间如何。

> **UTC 时间是明确的，不会重复。**

**时区：**时区指的是地球上的一个区域，在该区域内所有时钟的偏移量与协调世界时（UTC）相同。它很重要，因为它影响世界各地的本地时间。

**偏移量：**偏移量指的是从协调世界时（UTC）中加上或减去的一段时间，以获得特定时区的本地时间。这一点很重要，因为它影响不同地区的本地时间。我们可以使用`**timedelta**`类从`**datetime**`模块来创建偏移量。

```py
from datetime import timedelta

offset = timedelta(hours=1)
dt2 = dt + offset
```

打印 `dt2` 会给出以下信息 `datetime.datetime(2023, 5, 4, **11**, 30, **tzinfo**=<DstTzInfo ‘Asia/Singapore’ LMT+6:55:00 STD>)`。注意，在添加偏移量后，小时属性从 10 变为 1。

时间、时区和偏移量的相互作用在操控 Python 中的`datetime`时至关重要，因为它们决定了特定时区中的真实时间，包括夏令时的调整。

# `datetime` 对象的属性和方法，ISO 8601 标准

`datetime` 类有几个常用的**属性**，它们在 `datetime` 操作中非常重要。这些属性包括 `year` 、`month` 、`day` 、`hour` 、`minute` 、`second` 、`microsecond` 和 `tzinfo`。从我们上面的例子中，

```py
print(dt.year) #2023
print(dt.month) #5 
print(dt.day) #4
print(dt.hour) #11
print(dt.minute) #30
print(dt.second) #0
print(dt.microsecond) #0
print(dt.tzinfo) #Asia/Singapore
```

一些基本的方法包括 `date()` 、`time()` 、`replace()` 、`isoformat()` 、`isocalendar()` 、`strftime(format)`。等等，ISO 格式到底是什么？

**ISO日历格式**是一种全球使用的标准，旨在以易于计算机程序读取的格式表示日期和时间[⁴](#055e)。该格式包含特定的语法，日期使用四位数字表示年份，两位数字表示月份，两位数字表示日期（YYYY-MM-DD）。例如，2023年1月1日表示为“2023–01–01”。此外，它还可以包含更复杂的信息，如时间和时区，如下面的代码所示。

```py
print(dt.isoformat())
#return '2023-05-04T10:30:00+06:55'

print(dt.isocalendar()) 
#return tuple of datetime.IsoCalendarDate(year=2023, week=18, weekday=4)

datetime.fromisoformat("2023-01-05")
#return datetime.datetime(2023, 1, 5, 0, 0)

datetime.fromisoformat('2011-11-04T00:05:23')
#return datetime.datetime(2011, 11, 4, 0, 5, 23)

datetime.fromisoformat('2011-11-04 00:05:23.283')
#return datetime.datetime(2011, 11, 4, 0, 5, 23, 283000)

datetime.fromisoformat('2011-11-04 00:05:23.283+00:00')
#return datetime.datetime(2011, 11, 4, 0, 5, 23, 283000, tzinfo=datetime.timezone.utc)

datetime.fromisoformat('2011-11-04T00:05:23+04:00')
#return datetime.datetime(2011, 11, 4, 0, 5, 23, tzinfo=datetime.timezone(datetime.timedelta(seconds=14400))) 
```

让我们进一步**深入**最后一行代码。

+   `2011-11-04`：日期组件，表示2011年11月4日。

+   `T`：一个分隔符字符，表示时间组件的开始。

+   `00:05:23`：时间组件，表示12:05:23am。

+   `+04:00`：时区偏移量组件，表示与协调世界时（UTC）相差4小时，方向为正（比UTC早）。

# 使用`strftime(format)`格式化`datetime`对象，以及使用`**strptime**(format)`解析字符串为`datetime`对象。

在Python中，你可以使用`**strftime(format)**`方法将***datetime对象转换为字符串***。你只需提供一个字符串，告诉它你希望字符串的格式。相反，你也可以使用`**strptime(input_string, input_format)**`方法将***字符串解析为datetime对象***。

格式字符串可以包含**格式代码**和**文字字符**的组合。格式代码是由符号**%**表示的特殊字符序列，替换为`datetime`对象中的相应值。文字字符按原样包含在结果字符串中。以下是Python用户指南中常见格式代码的列表[¹](#ac6d)。

+   `%Y`：四位数字表示的年份。

+   `%m`：零填充的十进制数字表示的月份（01-12）。

+   `%d`：零填充的十进制数字表示的日期（01-31）。

+   `%H`：零填充的十进制数字表示的小时（00-23）。

+   `%M`：零填充的十进制数字表示的分钟（00-59）。

+   `%S`：零填充的十进制数字表示的秒（00-59）。

+   `%a`：缩写的星期几名称（周日、周一、周二等）。

+   `%A`：完整的星期几名称（星期日、星期一、星期二等）。

+   `%b`：缩写的月份名称（1月、2月、3月等）。

+   `%B`：完整的月份名称（1月、2月、3月等）。

+   `%p`：AM/PM标识（AM或PM）。

```py
from datetime import datetime
import pytz

tz = pytz.timezone('Asia/Singapore')
dt = datetime(2023, 5, 4, 10, 30, tzinfo=tz)

# Format as YYYY-MM-DD
# Output: 2023-05-04
print(dt.strftime('%Y-%m-%d'))

# Format as MM/DD/YYYY
# Output: 05/04/2023
print(dt.strftime('%m/%d/%Y'))   

# Format as Weekday, Month DD, YYYY HH:MM PM/AM
# Output: Thursday, May 04, 2023 10:30 AM
print(dt.strftime('%A, %B %d, %Y %I:%M %p'))

# .strptime() converts string to datetime object
# Output: 2023-05-04 20:30:45 with type datetime.datetime
dt_parsed = datetime.strptime("2023-05-04 20:30:45", '%Y-%m-%d %H:%M:%S')
print(dt_parsed)

# Another example of .strptime()
# Output: 2022-05-05 12:30:00
input_str = '5 May 2022 12:30 PM'
input_format = '%d %B %Y %I:%M %p'
dt = datetime.strptime(input_str, input_format)
```

# `timedelta`对象

在Python中，`**timedelta**`对象表示**两个日期或时间之间的持续时间或差异**。它可以用于与`datetime`对象进行算术运算，例如加减时间间隔，或计算两个`datetime`对象之间的差异[¹](#ac6d)。

要创建一个 `**timedelta**` 对象，可以使用 `**datetime.timedelta()**` 构造函数，该函数接受一个或多个参数来指定持续时间。这些参数可以是整数或浮点数，表示**天数、秒数、微秒数**，或者它们的组合。例如，`**timedelta(days=1, hours=3)**` 创建了一个 `**timedelta**` 对象，表示一天和三小时。你可以对 `**timedelta**` 对象执行加法、减法、乘法和除法等算术操作，也可以使用比较运算符进行比较。例如，

```py
from datetime import timedelta

delta_1 = timedelta(days=5, seconds=55, microseconds=555)
delta_2 = timedelta(days=1, seconds=11, microseconds=111)

print(delta_1.total_seconds()) #return 432055.000555
print(delta_1 - delta_2) #return 4 days, 0:00:44.000444
print(delta_1 > delta_2) #return True
print(delta_1 * 2) #return datetime.timedelta(days=10, seconds=110, microseconds=1110)
print(delta_1 / 2) #datetime.timedelta(days=2, seconds=43227, microseconds=500278)
```

# POSIX 时间戳

POSIX 时间戳，也称为 Unix 时间戳或纪元时间戳，是一种以单一整数值表示时间的方法，这种方法便于比较和操作[⁵](#8ebd)。它在计算机系统和编程语言（如 Python）中被广泛使用。它表示***从 1970 年 1 月 1 日 00:00:00 UTC 起的秒数***，这被称为 Unix 纪元时间。由于它不受时区和夏令时 (DST) 的影响，因此在计算机系统中存储和操作日期和时间时非常有用。

```py
from datetime import datetime

# create a datetime object for a specific date and time
dt1 = datetime(2023, 5, 4, 10, 30, 0)
dt2 = datetime(2023, 5, 4, 11, 30, 0)

# convert the datetime object to a POSIX timestamp
timestamp1 = dt1.timestamp()
timestamp2 = dt2.timestamp()

print(timestamp1)  # output: 1683171000.0
print(timestamp2)  # output: 1683174600.0
print(timestamp2 - timestamp1)  # output: 3600.0
```

请注意，时间戳之间的差异为 3600 秒，相当于一个小时的间隔。

类似地，我们也可以将 POSIX 时间戳转换为 `datetime` 对象。

```py
from datetime import datetime

# create a POSIX timestamp for a specific date and time
timestamp = 1680658200.0

# convert the POSIX timestamp to a datetime object
dt = datetime.fromtimestamp(timestamp)

print(dt)  # output: 2023-04-05 08:30:00
```

# Pandas，.dt 访问器和 `datetime64[ns]`

`**datetime64[ns]**` 数据类型是一种表示日期和时间的类型，其精度高达纳秒。它是 Python 中 NumPy 库的一部分，类似于 `datetime` 模块，但在处理大量日期和时间数据时表现更佳。这使得在处理大型数据集时更加高效，特别是与其他 NumPy 函数结合使用时，用于处理数组和矩阵。

在使用 Pandas 库时，`datetime64[ns]` 是一个非常好的数据类型，因为它允许我们访问强大的 `dt` 属性来处理 `datetime` 对象。注意，这个属性在 `pd.Timestamp` 对象中不可用，因此建议将数据类型转换为 `datetime64[ns]`，以便更方便地操作 `datetime` 对象。让我们开始吧。我们将从 CSV 文件中导入包含 500 条记录的随机时间戳（包括两个城市：曼谷/新加坡）。默认情况下，数据类型将是 `**str**`。这一部分和代码风格受到《有效 Pandas》一书的启发[⁶](#bb43)。

```py
random_timestamp = pd.read_csv("random_timestamp.csv")
random_timestamp
```

![](../Images/ed6fa968f921e7af508fabff8fe6e09a.png)

供参考，曼谷的时间偏移为 GMT+7，而新加坡的时间偏移为 GMT+8。我们的目标是将所有时间转换为新加坡时间。处理 Pandas 中的 `datetime` 对象…

```py
#Creating the offset
offset = np.where(random_timestamp.country == "Bangkok", "+07:00", "+08:00")
offset

#Convert data type using pd.to_datetime, groupby offset, convert to SG time
(pd
 .to_datetime(random_timestamp.timestamp)
 .groupby(offset)
 .transform(lambda s: s.dt.tz_localize(s.name)
                       .dt.tz_convert('Asia/Singapore'))
)
```

如果你对上一个方法的操作感到困惑，下面是详细说明：首先，`datetime` 系列或索引被假设为没有时区信息，即它不附加任何时区信息。

1.  `.dt.tz_localize()` 方法用于将时区附加到 `datetime` 系列或索引，这样它就会变成具有时区感知的。

1.  该方法接受一个参数，即 `datetime` 系列或索引应该本地化到的时区。

1.  一旦时区附加到 `datetime` 系列或索引上，你可以执行需要时区信息的 `datetime` 操作，例如将列转换为特定时区（即新加坡）。

![](../Images/6b0a3733c97f2a400a4011e22c7a8533.png)

左侧 vs 右侧输出: 未转换 vs 转换为特定时区

`.dt` 访问器允许我们检索 `datetime` 信息，如下所示

```py
offset = np.where(random_timestamp.country == "Bangkok", "+07:00", "+08:00")
offset

(random_timestamp
 .assign(sg_time = (pd
                     .to_datetime(random_timestamp.timestamp)
                     .groupby(offset)
                     .transform(lambda s: s.dt.tz_localize(s.name)
                                           .dt.tz_convert('Asia/Singapore'))),
         sg_hour = lambda df_: df_.sg_time.dt.hour,
         sg_minute = lambda df_: df_.sg_time.dt.minute,
         sg_second = lambda df_: df_.sg_time.dt.second,
         sg_weekday = lambda df_: df_.sg_time.dt.weekday,
         sg_weekofyear = lambda df_: df_.sg_time.dt.isocalendar().week,
         sg_strftime = lambda df_: df_.sg_time.dt.strftime('%A, %B %d, %Y %I:%M %p'))
)
```

![](../Images/cf6c396319e7c7199587226da31cb290.png)

# 作为索引的日期，`.resample(), .agg(), .transform()` 方法

本节和编码风格受到《Effective Pandas[⁶](#bb43)》一书的启发。假设我们有一个数据集，记录了一天中多个时间点的温度，如下所示。

![](../Images/87b070d842b5d13e3f6a4d64bcde01bb.png)

我们希望对数据进行聚合，以找出一天中的最低、最高和平均温度。我们可以通过将 DataFrame 的索引设置为包含 `**datetime**` 对象的列，然后使用 `**resample**` 方法并指定聚合的频率来实现这一点。

```py
(temp_record
 .set_index("timestamp")
 .resample('D')
 .agg(['min', 'max', 'mean'])
 .round(1)
)
```

![](../Images/ad82f8f9a3efdc0f24e9fa4082847d96.png)

`.agg()` 总结并缩减 DataFrame 的记录

如果我们希望保留行数而不是将其缩减为 7 天，我们可以使用方法 `transform()` 而不是 `agg()`。但是，请注意 `transform()` 不能接受一个聚合方法列表，而是限制为一次只能使用一个聚合方法。

```py
(temp_record
 .set_index("timestamp")
 .resample('D')
 .transform('min')
 .round(1)
)
```

![](../Images/92117da88e31189289a06fb0b4e3538c.png)

`.transform()` 保留了 DataFrame 的所有记录

# 后记

理解和操作 `datetime` 对象是任何数据分析师或科学家的关键技能。通过掌握各种类和方法，你可以从数据中解锁强大的洞察，并做出明智的决策。答应我，下次当你遇到带有日期和时间的数据集时，不要退缩——拥抱它，让 `**datetime**` 的魔力开始吧！

如果你从这篇文章中获得了一些有用的信息，请考虑在 Medium 上给我一个 [***关注***](https://medium.com/@andreas030503)。很简单，每周一篇文章，让你保持更新并保持领先！

***你可以在 LinkedIn 上与我联系:*** [***https://www.linkedin.com/in/andreaslukita7/***](https://www.linkedin.com/in/andreaslukita7/)

***参考资料:***

1.  Python 文档: [https://docs.python.org/3/library/datetime.html#timezone-objects](https://docs.python.org/3/library/datetime.html#timezone-objects)

1.  国家飓风中心和中太平洋飓风中心。*什么是 UTC 或 GMT 时间？* [https://www.nhc.noaa.gov/aboututc.shtml#:~:text=Prior%20to%201972%2C%20this%20time,%22%20or%20%22Zulu%20Time%22](https://www.nhc.noaa.gov/aboututc.shtml#:~:text=Prior%20to%201972%2C%20this%20time,%22%20or%20%22Zulu%20Time%22).

1.  国家标准与技术研究院。*什么是 USNO 时间或 UTC(USNO)?* [https://www.nist.gov/pml/time-and-frequency-division/nist-time-frequently-asked-questions-faq#:~:text=USNO%20has%20an%20ensemble%20of,scale%20called%20UTC(USNO).](https://www.nist.gov/pml/time-and-frequency-division/nist-time-frequently-asked-questions-faq#:~:text=USNO%20has%20an%20ensemble%20of,scale%20called%20UTC(USNO).)

1.  国际标准化组织。*ISO 8601 日期和时间格式。* [https://www.iso.org/iso-8601-date-and-time-format.html](https://www.iso.org/iso-8601-date-and-time-format.html)

1.  UNIX 时间。 [https://unixtime.org/](https://unixtime.org/)

1.  Matt Harrison 的《有效 Pandas》: [https://store.metasnake.com/effective-pandas-book](https://store.metasnake.com/effective-pandas-book)
