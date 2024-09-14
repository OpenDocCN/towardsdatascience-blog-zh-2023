# 利用这个 Python 库弥合数据与人类之间的差距

> 原文：[`towardsdatascience.com/bridge-the-gap-between-data-and-humanity-with-the-power-of-this-python-library-553a9f55908b`](https://towardsdatascience.com/bridge-the-gap-between-data-and-humanity-with-the-power-of-this-python-library-553a9f55908b)

## 让你的 Python 输出更易于理解

[](https://zoumanakeita.medium.com/?source=post_page-----553a9f55908b--------------------------------)![Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----553a9f55908b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----553a9f55908b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----553a9f55908b--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----553a9f55908b--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----553a9f55908b--------------------------------) ·4 分钟阅读·2023 年 2 月 22 日

--

# 介绍

我们无需依赖任何统计数据就能意识到，Python 是软件开发人员、数据科学家等最常用的编程语言之一。这不仅因为其灵活性和易用性，还因为有大量的库可以使我们的日常任务更加轻松。

本文介绍了另一个强大的库：`humanize`。它通过使输出更易于理解，帮助弥合人类与 Python 输出之间的差距。让我们看看一些例子。

# 开始使用

为了使用`humanize`，第一步是使用 Python 包管理器`pip`进行安装，如下所示：

```py
!pip3 install humanize
```

接下来，你需要导入以下相关库以成功完成教程。

+   `getsize()`来自`os`库，用于获取给定文件的大小。

+   `datetime`用于处理时间。

+   最后，是本文的核心——`humanize`库。

```py
from os.path import getsize
import datetime as dt
import humanize as h
```

一切准备就绪，开始探索大数字。

## 使大数字更具可读性

> 这个数字 1034503576643 是什么？

理解这个数字是否在十亿还是万亿范围内需要一些脑力。`humanizer`通过提供更友好的输出，试图减轻这种负担。

一种方法是使用正确的逗号`**','**`来分隔，方法是使用`intcomma`函数，如下所示：

```py
big_num = 1034503576643

human_big_num_coma = h.intcomma(big_num)

print(human_big_num_coma)
```

上述代码的输出是***1,034,503,576,643***，比没有分隔符的原始数字要好得多。

此外，结果可以使用`intword`函数生成自然语言格式，如下所示：

```py
human_big_num = h.intword(big_num)

print(human_big_num)
```

这会产生以下结果：`1.0 trillion.`

## 处理日期时间

> 2022 年 9 月 6 日（YYYY/MM/DD 格式）是 2022 年 9 月 6 日

第二种格式（Sep 6 2022）比第一种 YYYY/MM/DD 格式更容易被理解，因为它符合我们日常的口头交流。这种结果可以通过`naturaldate`函数获得。

```py
date = dt.date(2022, 9, 6)

human_date = h.naturaldate(date)

print(human_date) 
```

这将生成以下结果：`Sep 06 2022`。

可以使用`naturalday`函数将结果限制为月份和日期，而不是使用`naturaldate`。

```py
human_day = h.naturalday(date)

print(human_day)
```

结果是`Sep 06`

## 处理持续时间

类似于 DateTime，也可以使用`naturaltime`函数使持续时间具有可读性，如下所示。

```py
# Get today's date
current_time = dt.datetime.now()

# Get the date of 3 days before
few_days_before = dt.timedelta(days=3, hours=23, minutes=40)

# Compute the difference of time
past_time = current_time - few_days_before
human_time = h.naturaltime(past_time)

print(human_time)
```

之前的代码生成了`3 days ago`，这是任何人都可以理解的。

## 获取文件的大小和单位

> 我的文件大小是 278。

这个声明最明显的问题是

> 你使用的是什么单位？字节、千字节、兆字节、千兆字节、太字节？

这个谜题可以通过使用`naturalsize`函数解决，如下所示：

+   首先，使用`getsize`函数获取`CSV`文件的大小。

+   然后使用`naturalsize`函数生成更合适的输出。

```py
fize_size = getsize("./candidates.csv")

# Before Humanize
print(fize_size)

# After Humanize
print(h.naturalsize(fize_size))
```

+   人性化处理前的结果是 278。

+   人性化处理后，我们得到了**278 Bytes**。

## 科学记数法和分数

给定数字的科学记数法在某些场景中可能更有用，例如使用`power of the ten`记法。这可以通过使用`scientific`函数来实现。

使用`precision`参数，用户可以指定小数点后的精度值数量。如果未指定，精度值为 2。

下面是一个示例。

```py
# Number to convert to scientific format
value = 2304355

# Without Precision
scientic_notation = h.scientific(value)
print(scientic_notation)

# With precision
scientic_notation = h.scientific(value, precision = 5)
print(scientic_notation)
```

输出结果按`print`语句的顺序给出。

+   使用默认函数：2.30 x 10⁶

+   使用`precision`参数：2.30436 x 10⁶

> 你认为 0.4646 的分数表示是什么？

避免过多的数学计算，只需使用`fractional`函数，如下所示：

```py
float_value = 0.4646

# Get the fractional representation
fraction = h.fractional(float_value)

print(fraction)
```

答案是 105/226。这真的很酷，不是吗！

## 如果我处理的是另一种语言怎么办

之前的所有结果都是用英语给出的。其他语言如法语、俄语等也可以实现相同的效果。

实现这一点的第一步是使用`i18n.activate`函数激活国际化（`i18n`）功能。

例如，可以创建一个持续时间为 3 秒的时间差对象，但这次用`法语`。

```py
# Activate the French Language
_t = h.i18n.activate("fr")

# Generate the time delta
h.naturaltime(dt.timedelta(seconds=3))
```

结果是`il y a 3 secondes`，这在英语中表示`3 seconds ago`。

# 结论

感谢阅读！ 🎉 🍾

希望你觉得这篇文章有帮助！

如果你喜欢阅读我的故事并希望支持我的写作，可以考虑[成为 Medium 会员](https://zoumanakeita.medium.com/membership)。通过每月$5 的承诺，你可以无限制访问 Medium 上的故事。

你想请我喝咖啡吗☕️？→ [在这里请我](http://www.buymeacoffee.com/zoumanakeig)!

随时欢迎关注我的 [Medium](https://zoumanakeita.medium.com/)、[Twitter](https://twitter.com/zoumana_keita_) 和 [YouTube](https://www.youtube.com/channel/UC9xKdy8cz6ZuJU5FTNtM_pQ)，或者在 [LinkedIn](https://www.linkedin.com/in/zoumana-keita/) 上打个招呼。讨论人工智能、机器学习、数据科学、自然语言处理和 MLOps 的内容总是很愉快的！

在你离开之前，请查看下面的该系列的最后两部分：

[Pandas 和 Python 数据科学与数据分析技巧 — 第一部分](https://medium.com/towards-data-science/pandas-and-python-tips-and-tricks-for-data-science-and-data-analysis-1b1e05b7d93a)

[Pandas 和 Python 数据科学与数据分析技巧 — 第二部分](https://medium.com/towards-data-science/pandas-python-tricks-for-data-science-data-analysis-part-2-dc36460de90d)

Pandas 和 Python 数据科学与数据分析技巧 — 第三部分
