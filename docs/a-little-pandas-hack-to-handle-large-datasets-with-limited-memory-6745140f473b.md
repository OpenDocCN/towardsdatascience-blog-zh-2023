# 处理大数据集的小技巧，适用于有限内存

> 原文：[`towardsdatascience.com/a-little-pandas-hack-to-handle-large-datasets-with-limited-memory-6745140f473b`](https://towardsdatascience.com/a-little-pandas-hack-to-handle-large-datasets-with-limited-memory-6745140f473b)

## Pandas 默认设置并不理想。一个小小的配置就可以将你的数据框压缩到适合你的内存。

[](https://thuwarakesh.medium.com/?source=post_page-----6745140f473b--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----6745140f473b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6745140f473b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6745140f473b--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----6745140f473b--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6745140f473b--------------------------------) ·阅读时间 8 分钟·2023 年 1 月 19 日

--

![](img/c4b1299e06f03069b94110ff87456e73.png)

你可以在不丢失属性的情况下压缩一个巨大的 Pandas 数据框，就像挤压一个汉堡一样。通过这个小技巧节省内存，并提高工作效率。— 图片由 [Leonardo Luz](https://www.pexels.com/photo/photo-of-a-burger-between-a-person-s-hands-14001304/) 提供

我从未认为我的代码需要改进。我总是抱怨内存不足或数据集过大无法处理。

我常用的解决方案是将数据放在 Postgres 数据库中，并编写 SQL 查询。毕竟，这是一种处理大规模数据集的可接受方式。每当我获得一个大数据集时，我都会这样做。

但我无法获得 Python 中的完全灵活性。因此，我不得不将两者结合使用并交替进行。例如，我将数据集加载到 SQL 数据库中，并编写一个 Python 脚本来运行 SQL 查询，通常使用 Sqlalchemy。

虽然这让我在两者之间拥有灵活性，但我在与其他团队成员共享代码时遇到了问题。其他成员应该具备关系数据库的知识和设置。

这个问题的经典解决方案是增加内存并进行并行任务。在基础设施方面，迁移到云端是我最喜欢的解决方案。我可以只为高性能资源的使用付费。而在并行执行方面，像 [Dask](https://www.dask.org/) 这样的技术可以为我提供支持。

然而，我最近发现代码优化是首选且通常是最有成效的解决方案。如果你的代码在单线程下表现不佳，我们如何保证并行化能够提升性能？如果你的代码没有充分利用本地计算机的资源，那迁移到云端又有什么意义呢？

在我学到的许多 Pandas 优化技术中，有一种特别深刻。

诀窍是…

## 使用正确的数据类型来节省内存。

Pandas 根据每列中存在的值来推断数据类型。但它没有上下文。而你有！

如果列中的所有值都是整数，Pandas 通常会将 int64 分配为该列的数据类型。然而，还有更多的 int 变体可供使用，例如 int8、int16 等。

对于大多数情况，默认设置是可以的。但有时，对于较小的值，这个占位符过大。打开一个笔记本，输入以下代码并查看其输出。

```py
import numpy as np
np.iinfo('int64')

# >> iinfo(min=-9223372036854775808, max=9223372036854775807, dtype=int64)
```

Int64 可以容纳从 -9223372036854775808 到 9223372036854775807 的值。我甚至不知道这些数字怎么称呼。

但在大多数实际情况下，你可以推测可能的列值。例如，在酒店预订系统中，房间中的客人数几乎总是个位数。如果是宿舍，可能是两位数。

[## 如何检测 Python 应用中的内存泄漏](https://example.org/how-to-detect-memory-leakage-in-your-python-application-f83ae1ad897d?source=post_page-----6745140f473b--------------------------------)

### 标准的 Python 库可以告诉你每一行的内存使用情况和执行时间。

[towardsdatascience.com](https://example.org/how-to-detect-memory-leakage-in-your-python-application-f83ae1ad897d?source=post_page-----6745140f473b--------------------------------)

为什么不使用 int8 呢？它可以容纳从 -128 到 +128 的值。这个更小的占位符相比浪费的 int64 占用更少的空间。

我们在一个示例数据集上测试一下这个方法。

以下脚本生成了一个随机数据集。在实际生活中，你不会生成数据集。注意它的数据类型和内存使用情况。

```py
import pandas as pd
import numpy as np

size = 1000000
df = (
    pd.DataFrame(
        {
            "room_rate": np.round(np.random.randint(35, 100, size) / 23 * 100, 2),
            "number_of_guests": np.random.randint(0, 6, size),
            "channel": np.random.choice(
                ["Online", "Corporate", "Walk In", "Complementary"], size
            ),
            "booking_status": np.where(
                np.random.randint(0, 100, size) > 90, "Canceled", "Not-Canceled"
            ),
        }
    )
#     .pipe(lambda df: display(df) or df)
    .pipe(lambda df: display(df.dtypes) or df)
)

"""
room_rate           float64
number_of_guests      int64
channel              object
booking_status       object
dtype: object
"""

df.memory_usage(deep=True)

"""
Index                    128
room_rate            8000000
number_of_guests     8000000
channel             65748930
booking_status      68640436
dtype: int64
"""

df.memory_usage(deep=True).sum()

"""
150389494
"""

df.describe(include='all').fillna('-')

"""
|        | room_rate         | number_of_guests   | channel   | booking_status   |
|:-------|:------------------|:-------------------|:----------|:-----------------|
| count  | 1000000.0         | 1000000.0          | 1000000   | 1000000          |
| unique | -                 | -                  | 4         | 2                |
| top    | -                 | -                  | Corporate | Not-Canceled     |
| freq   | -                 | -                  | 250099    | 910109           |
| mean   | 291.2777862699999 | 2.50315            | -         | -                |
| std    | 81.59777471018273 | 1.708107430793215  | -         | -                |
| min    | 152.17            | 0.0                | -         | -                |
| 25%    | 221.74            | 1.0                | -         | -                |
| 50%    | 291.3             | 3.0                | -         | -                |
| 75%    | 360.87            | 4.0                | -         | -                |
| max    | 430.43            | 5.0                | -         | -                |
""" 
```

生成的数据集有一百万条记录，占用约 180 MB 的内存。room_rate 变量的范围是 152.17–430.43\。但它被分配了 float64 数据类型。实际上，float16 已经足够了。同样，将 number_of_guests 转换为整数，将 channel 转换为类别。

有趣的是，预订状态是以分类方式表示的布尔值。为了分析目的，我们可以将其更改为布尔值。但也有人可能会争辩说还是将其保留为类别更好。

下面是我们如何将数据类型转换为更理想的类型，以及现在需要多少内存。

```py
(
    df.assign(
        room_rate=df.room_rate.astype("float16"),
        number_of_guests=df.number_of_guests.astype("int8"),
        channel=df.channel.astype("category"),
        booking_status=df.booking_status == "Canceled",
    )
    .pipe(lambda df: display(df.dtypes) or df)
    .pipe(lambda df: display(df.memory_usage(deep=True)) or df)
    .memory_usage(deep=True)
    .sum()
)

"""
room_rate            float16
number_of_guests        int8
channel             category
booking_status          bool
dtype: object

Index                   128
room_rate           2000000
number_of_guests    1000000
channel             1000435
booking_status      1000000
dtype: int64

5000563
""" 
```

我们通过将数据类型转换将 180MB 的数据集减少到 5MB。**这大约节省了 96%**。

压缩数据集并将其存储为最佳格式以备后用是个好主意。

[## CSV 文件被高估了！我放弃了一些它的优点以获得更多的好处。](https://example.org/best-file-format-to-store-large-data-dfa47701929f?source=post_page-----6745140f473b--------------------------------)

### 我使用了什么来获得更小的文件大小和更好的性能。

[towardsdatascience.com](https://example.org/best-file-format-to-store-large-data-dfa47701929f?source=post_page-----6745140f473b--------------------------------)

## 如何为每一列选择数据类型？

Pandas 的默认数据类型几乎总是不最优的。正如我们在上一节中看到的，整数无论范围如何，都被分配为 int64。对于较小的数据集，我们不需要担心这个问题。即使在数十万行数据中，性能影响通常也不显著。

但是当我们的计算变得复杂且数据集规模更大时，我们应该更认真对待这个问题。挑战在于在不丢失信息的情况下，分配尽可能小的占位符。

第一步是获取数据集的描述。它可以在一个地方提供大量的信息。查看我们上面示例中的 `describe` 输出。

对于非数值变量，你将获得唯一值的数量、最频繁的值及其计数。看着这些数据，我们可以猜测 `channel` 可能是一个类别，而 `booking_status` 可能是布尔值。因为在我们 1M 的记录中，只有 4 个独特的渠道在重复出现。

对于数值列，我们得到最小值和最大值。通过查看四分位数值，你可以得出这个列是否需要是整数或浮点数。在每种情况下，你也可以决定其大小。

这是一个数值数据类型及其值范围的表格。

```py
__Integer Types__
| dtype   |                  min |                  max |
|:--------|---------------------:|---------------------:|
| int8    |                 -128 |                  127 |
| int16   |               -32768 |                32767 |
| int32   |          -2147483648 |           2147483647 |
| int64   | -9223372036854775808 |  9223372036854775807 |
| uint8   |                    0 |                  255 |
| uint16  |                    0 |                65535 |
| uint32  |                    0 |           4294967295 |
| uint64  |                    0 | 18446744073709551615 |

__Float Types__
| dtype   |               min |              max |   resolution |
|:--------|------------------:|-----------------:|-------------:|
| float16 | -65504            | 65504            |    0.0010004 |
| float32 |     -3.40282e+38  |     3.40282e+38  |    1e-06     |
| float64 |     -1.79769e+308 |     1.79769e+308 |    1e-15     |
```

仔细观察你的数据集，并选择能够工作的最小值。

对于非数值数据集，Pandas 通常将其类型指定为 ‘object’。对象数据类型在大多数情况下是复杂且低效的。因此，如果列值不是自由格式的，你可以将其数据类型指定为 ‘category’。以下是它节省了多少内存：

```py
df.channel.astype('object').memory_usage()
# >> 8000128

df.channel.astype('category').memory_usage()
# >> 1000332
```

通过简单地转换为类别数据类型，你可以节省大约 8 倍的内存。

[](/how-to-speed-up-python-data-pipelines-up-to-91x-80d7accfe7ec?source=post_page-----6745140f473b--------------------------------) ## 如何将 Python 数据管道的速度提高到 91 倍？

### 一个 5 分钟的教程，可能会为你的大数据项目节省数月的时间。

towardsdatascience.com

## 转换数据类型需要更多的领域专业知识。

我们的演示过于简单，因为我们生成了数据集。但是现实世界中的数据集通常是复杂的，类型转换也是如此。

我们可以在任何数据框上使用 `.astype` 方法直接转换类型。但在我的例子中，我使用了 `assign` 方法。因为我们通常需要在进行类型转换之前修改列。

想象一个情况，当 `number_of_guests` 列有空值时。我们不能在没有填补合理默认值的情况下将该列转换为 int8。零可能没有意义，因为不可能有一个没有人的预订。我们可能认为 `1` 是一个好的默认值。但是，如果大多数酒店的预订有两个人，那么 `2` 更好。我们还可以引入 `channel` 变量来仔细选择每个渠道的默认值。

以下示例是条件分配默认值的方式。注意，我们仅在填补适当的缺失值后才将其转换为正确的数据类型。

```py
df.assign(
    number_of_guests=np.where(
        df.channel == "Online",
        df.number_of_guests.fillna(2),
        np.where(
            (df.channel == "Corporate") | (df.channel == "Walk In"),
            df.number_of_guests.fillna(1),
            df.number_of_guests.fillna(3),
        ),
    ).astype("int8")
)
```

同样，选择默认值需要领域知识，这在数据集中并不存在。

另一个挑战是未来的值。如果你进行离线分析，你可以仅通过查看数据集来选择数据类型。但如果你的代码在数据管道中运行，你还应考虑未来的值。

[## 完美方法自动化和协调数据管道](https://towardsdatascience.com/the-prefect-way-to-automate-orchestrate-data-pipelines-d4465638bac2?source=post_page-----6745140f473b--------------------------------)

### 我正在将我所有的 ETL 工作从 Airflow 迁移到这个超级酷的框架。

[完美方法自动化和协调数据管道](https://towardsdatascience.com/the-prefect-way-to-automate-orchestrate-data-pipelines-d4465638bac2?source=post_page-----6745140f473b--------------------------------)

当你获得不兼容的列值时，你可能会得到错误或错误的值。请参见以下示例。

```py
s = np.random.randint(0, 255, 10)
pd.DataFrame({"original": s, "int8": s.astype("int8")})

|   original |   int8 |
|-----------:|-------:|
|        221 |    -35 |
|        184 |    -72 |
|        216 |    -40 |
|        177 |    -79 |
|          9 |      9 |
|         19 |     19 |
|         16 |     16 |
|        191 |    -65 |
|          6 |      6 |
|         71 |     71 |
```

请注意，任何小于 128 的值已正确转换。但 128 及以上的值转换不正确。

或许你应该在将数据集传递给转换之前进行验证。

[## 高质量的数据伴随高质量的验证](https://towardsdatascience.com/data-validation-for-pandas-b24613959364?source=post_page-----6745140f473b--------------------------------)

### 这里是你如何确保 Pandas 数据框在数据管道每个阶段的质量

[高质量的数据需要高质量的验证](https://towardsdatascience.com/data-validation-for-pandas-b24613959364?source=post_page-----6745140f473b--------------------------------)

## 结语

作为数据专业人士，我们处理的是大规模的数据集。我们对性能问题的终极解决方案是增加更多资源和并行执行。

我发现一些优化可以让我们在较小的计算机上处理更大的数据框。诀窍是分配正确的数据类型。

选择正确的数据类型需要领域知识。此外，如果你正在构建数据管道，你还应考虑未来的值。在离线分析中你不需要担心这一点。

希望这对你有所帮助。

> 谢谢你的阅读，朋友！在 [**LinkedIn**](https://www.linkedin.com/in/thuwarakesh/)、[**Twitter**](https://twitter.com/Thuwarakesh) 和 [**Medium**](https://thuwarakesh.medium.com/) 上跟我打招呼吧。
> 
> 还不是 Medium 会员？请使用这个链接 [**成为会员**](https://thuwarakesh.medium.com/membership)，因为你没有额外费用，我会因为推荐你而获得小额佣金。
