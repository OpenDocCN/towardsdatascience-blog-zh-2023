# 使用 Python 进行有效的日期和时间编码

> 原文：[https://towardsdatascience.com/effective-coding-with-dates-and-times-in-python-3be13a77b45b?source=collection_archive---------5-----------------------#2023-08-26](https://towardsdatascience.com/effective-coding-with-dates-and-times-in-python-3be13a77b45b?source=collection_archive---------5-----------------------#2023-08-26)

## 使用datetime、zoneinfo、dateutil 和 pandas

[](https://aliciahorsch.medium.com/?source=post_page-----3be13a77b45b--------------------------------)[![Alicia Horsch](../Images/1e3c5dcafb062d0aa710934c6382cb32.png)](https://aliciahorsch.medium.com/?source=post_page-----3be13a77b45b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3be13a77b45b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3be13a77b45b--------------------------------) [Alicia Horsch](https://aliciahorsch.medium.com/?source=post_page-----3be13a77b45b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fba4ebbd0afd2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffective-coding-with-dates-and-times-in-python-3be13a77b45b&user=Alicia+Horsch&userId=ba4ebbd0afd2&source=post_page-ba4ebbd0afd2----3be13a77b45b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3be13a77b45b--------------------------------) ·5分钟阅读·2023年8月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3be13a77b45b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffective-coding-with-dates-and-times-in-python-3be13a77b45b&user=Alicia+Horsch&userId=ba4ebbd0afd2&source=-----3be13a77b45b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3be13a77b45b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feffective-coding-with-dates-and-times-in-python-3be13a77b45b&source=-----3be13a77b45b---------------------bookmark_footer-----------)![](../Images/43a2f0db3816eb12fffff9e553ce47db.png)

图片由 [Jordan Benton](https://www.pexels.com/@bentonphotocinema/) 提供，来源于 [Pexels](https://www.pexels.com/photo/shallow-focus-of-clear-hourglass-1095601/)

最近我一直在广泛处理时间序列数据，并且处理了Python中的日期和时间对象。为此，我学到了一些关于在Python中处理*datetime*对象的有用技巧，这些技巧简化了我的代码。在本文中，我将分享并总结我学到的最有价值的提示和技巧。

为了演示，我将使用两个Kaggle数据集，当我使用它们时会提供链接。如果你想跟着操作，可以导入以下库。

# 日期时间、zoneinfo、dateutil 和 pytz

处理 Python 中日期和时间的常用包包括 *datetime*、*dateutil*、*pytz* 和 [最近的 *zoneinfo*](https://docs.python.org/3/library/zoneinfo.html)。*Datetime* 是用于处理 Python 中时间和日期的内置模块，能够处理大多数基本操作。*Dateutil* 和 *pytz* 是第三方模块，是 *datetime* 在处理更复杂的操作（如相对时间差、时区和字符串解析）时的强大扩展。

然而，自 Python 3.9 版本以来，[*zoneinfo*](https://docs.python.org/3/library/zoneinfo.html) 被纳入 Python 标准库，因此，被认为在**时区支持**方面比 *dateutil* 或 [*pytz*](https://docs.python.org/3/library/zoneinfo.html) 更“方便”。

> *因此，根据你使用的 Python 版本，Python 内置模块可能已经足够处理不同的时区，而无需第三方模块（*dateutil* 和 *pytz*）！*

在文章的其余部分，我将主要集中于使用 *datetime* 处理日期和时间，但也会提及 *zoneinfo* 或 *dateutil* 的潜在选项。文章将首先关注单个 *datetime* 对象，然后处理数组和数据框中的日期，使用 *numpy* 和 *pandas*：

1.  从头创建日期或日期时间

1.  转换和解析字符串：strftime()、strptime() 和 dateutil

1.  *numpy* 中的日期和时间 — numPy 的 datetime64

1.  *pandas* 中的日期和时间

1.  从日期和时间创建特征

# 1. 从头创建日期或日期时间

*datetime* 包允许你轻松从头创建日期和日期时间对象，例如，可以将其用作筛选的阈值（尝试打印下面创建的对象及其类型，以更好地理解它们的格式）。

此外，*datetime* 让你创建指向今天或现在的日期和时间对象。

> *请小心，因为* datetime *对象通常是“时区无关”的，并不指向特定时区，这在与国际同事合作时可能会给你带来麻烦！*

借助 *zoneinfo* 模块（自 Python 3.9 版本起内置），你可以使用 *astimezone()* 的 *tz* 参数设置时区。

# 2. 转换和解析字符串：strftime()、strptime() 和 dateutil

你可能会发现自己希望将 *datetime* 对象显示为字符串，或者将字符串转换为 *datetime* 对象。在这种情况下，函数 *strftime()* 和 *strptime()* 会很有帮助。

## 将日期时间对象（或其部分）转换为字符串

描述日期时间对象的常用格式代码可以在 [这里](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes) 找到。

## 将字符串转换为日期时间对象

## 使用 dateutil 解析复杂字符串

# 3. *numpy* 中的日期和时间 - numPy 的 datetime64

如果你处理的是大数据集，*numpy* 的 datetime64 可能会很有帮助，因为由于其设计，它比处理 *datetime* 和 *dateutil* 对象更快。*numpy* 中的 datetime64 数据类型将日期和时间编码为 64 位整数。

这紧凑地存储日期和时间，并允许矢量化操作（对 numpy 数组的每个元素应用的重复操作）。

正如运行上述代码时所见，使用 *datetime* 或 *dateutil* 对象时，矢量化操作会出现错误。

# 4\. pandas 中的日期和时间

*Pandas* 在处理时间序列数据项目时可能是一个不错的选择。

> *著名的数据处理库* pandas *结合了* datetime *和* dateutil *的便利性以及* numpy* 的有效存储和操作功能。*

## 创建一个 pandas 数据框（从 CSV）并解析日期列

现在，我们对使用 *numpy* 和 *pandas* 处理日期和时间有了基本了解。然而，我们往往不会自己创建日期和时间，而是它们已经是我们处理的数据集的一部分。让我们创建一个带有日期列的 *pandas* 数据框（[Kaggle 数据集 NFL](https://www.kaggle.com/c/nfl-big-data-bowl-2022/data?select=games.csv)）。

正如你所见，从 CSV 加载时，如果没有在任何地方明确指定，存储日期的列会转换为字符串格式。要获得日期格式，你可以创建一个额外的列“gameDate_dateformat”或直接通过 *parse_dates* 参数传递日期列到 pd.read_csv() 中。

处理时间序列数据时，另一个有用的操作是能够按日期/时间进行过滤或使用日期/时间对子集数据框。有两种方法来做到这一点：过滤/子集或索引。

## 按时间过滤 pandas 数据框

> *确保用于子集的阈值日期与列的格式相同！*

如果你想过滤的列的格式是 datetime（如示例中），比较日期不能是日期，而需要具有 datetime 格式！

## 按时间索引 pandas 数据框

更强大的操作是按日期或时间索引 *pandas* 数据框。

在处理时间序列时，索引特别有用，因为有如滚动窗口和时间移位等方法。

# 5\. 从日期和时间创建特征

通常，我们对日期本身并不感兴趣，而可能对持续时间、星期几或仅仅是日期时间的一部分（例如年份）感兴趣。为此，*datetime* 以及 *pandas* 提供了一些有用的操作。

## Timedelta

使用 *pandas*，你可以计算例如两个 datetime 之间的差异。为此，我们将查看一个不同的 Uber 行程数据集（[Kaggle 数据集 Uber](https://www.kaggle.com/datasets/zusmani/uberdrives/code?resource=download)），其中包含开始和结束时间戳。一些预处理是必需的（删除 Total Row），以便开始查看 timedelta。

## 提取星期几或月份

对于单一的 *datetime* 和 *pandas* Series，它们的工作方式稍有不同。虽然可以通过添加属性（例如，*.month*）或方法（例如，*weekday()*）直接访问单一 *datetime* 对象的星期几或月份，但 *pandas* Series 总是需要 *.dt* 访问器。

> *dt. 访问器* *允许你从一个* datetime *Series中访问特定于日期时间的属性和方法。*

## 创建日期/时间滞后

对于时间序列数据，另一个有用的操作是添加一个额外的列，用于增加日期或日期时间的滞后。

# 总结

在 Python 中处理日期或时间对象时，了解内置包 *datetime*（例如 *date()* 或 *strftime()* 和 *strptime()*）的基础知识是有益的。*Zoneinfo* 是一个新的内置包（自 3.9 版本起），在处理不同的时区时比第三方模块更方便。*Dateutil* 是一个有价值的库，用于处理单个日期对象时的高级日期和时间操作，例如解析复杂字符串。在数据框、Series 或数组中处理日期和时间时，*pandas* 结合了 *datetime*、*dateutil* 和 *numpy* 的优点，是一个方便的库。

## 来源

+   Alessandro Molina 的《现代 Python 标准库——食谱》（2018）

+   Jake VanderPlas 的《Python 数据科学手册》（2017）

+   Datacamp 课程：Python 中的日期和时间处理（2022）

+   [https://dateutil.readthedocs.io/en/stable/](https://dateutil.readthedocs.io/en/stable/)

+   [https://docs.python.org/3/library/datetime.html](https://docs.python.org/3/library/datetime.html)

+   [https://docs.python.org/3/library/zoneinfo.html](https://docs.python.org/3/library/zoneinfo.html)

+   [https://peps.python.org/pep-0615/](https://peps.python.org/pep-0615/)

+   [https://github.com/stub42/pytz/blob/master/src/README.rst#issues--limitations](https://github.com/stub42/pytz/blob/master/src/README.rst#issues--limitations)
