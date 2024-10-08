# Parquet 最佳实践：在不加载数据的情况下发现你的数据

> 原文：[`towardsdatascience.com/parquet-best-practices-discover-your-data-without-loading-them-f854c57a45b6`](https://towardsdatascience.com/parquet-best-practices-discover-your-data-without-loading-them-f854c57a45b6)

## 元数据、行组统计、分区发现和重新分区

[](https://medium.com/@arli94?source=post_page-----f854c57a45b6--------------------------------)![Arli](https://medium.com/@arli94?source=post_page-----f854c57a45b6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f854c57a45b6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f854c57a45b6--------------------------------) [Arli](https://medium.com/@arli94?source=post_page-----f854c57a45b6--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f854c57a45b6--------------------------------) ·8 分钟阅读·2023 年 1 月 3 日

--

*如果你想亲自体验 Medium，可以考虑通过* [***注册会员***](https://medium.com/@arli94/membership) *来支持我和其他成千上万的写作者。这只需每月$5，它对我们写作者的支持巨大，而且你可以访问 Medium 上所有精彩的故事。*

![](img/c510a1ca6e02384122e5f50eb2674840.png)

由 [Jakarta Parquet](https://unsplash.com/@lantai_kayu?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

*这篇文章是关于 Parquet 系列文章中的下一篇。如果你没有 Parquet 知识，应该先查看之前的* [*Parquet 文章*](https://medium.com/towards-data-science/easy-parquet-tutorial-best-practices-237955e46cb7) *，但它也是对更高级用户的很好的提醒。如果你想重现这篇文章的输入数据，代码在文末。*

*Apache Parquet* 是一种用于大数据框架的列式存储格式，如*Apache Hadoop*和*Apache Spark*。它旨在通过使用*列式存储格式*以*压缩*和*高效*的方式存储数据，从而提高大数据处理的性能。

*Parquet* 的采用持续增加，因为越来越多的组织转向大数据技术来处理和分析大型数据集。随着这种持续发展，学习一些最佳实践以及如何浏览*Parquet*文件变得至关重要。

在本教程中，我们将展示如何作为*Parquet*用户在不依赖常见的暴力加载方式的情况下，深入洞察你的*Parquet*数据。

# 案例研究

为此，我们提供了一个案例研究，其中一个*数据工程师*给你提供了*贷款申请者*的数据，你需要使用这些数据创建预测模型。但首先，你需要*“技术性地”*发现数据。数据量非常大。

确实，准备数据的*数据工程师*告诉你，Parquet 文件夹的大小为*1TB*（*仅用于教育目的，这在我们的示例中并非如此*），因此如果你尝试加载所有内容，你的机器将会遇到内存错误。

不用担心，我们会提供最有效的方式来理解大型*Parquet*数据，甚至不需要将*Parquet*数据加载到内存中。

这意味着需要回答以下问题：

+   这个文件夹中的*Parquet*文件是什么样的？

+   *变量*里面有哪些？*类型*是什么？一些*统计数据*？

+   数据是如何分区的？

我们还会教你如何重新格式化*分区*，如果你发现数据分区的方式有问题的话。

# 阅读第一个 Parquet 文件

你在这个教程中需要的导入：

```py
import pyarrow as pa
import pyarrow.parquet as pq
import os
```

首先，我们想了解文件夹*‘APPLICATIONS_PARTITIONED’*包含什么，这里存储了数据。

由于你不知道数据是如何*分区*的，因此不能盲目地加载整个文件夹，因为你将会加载所有的*Parquet*文件，这不是你想做的（记住 1TB 的大小），而是你需要对数据进行概览。

这里，我给你一个函数`get_first_parquet_from_path()`，它会返回目录中的第一个*Parquet*文件。该函数将扫描每个目录和子目录，直到找到一个*Parquet*文件，并返回该单个文件的完整路径。

```py
def get_first_parquet_from_path(path):
    for (dir_path, _, files) in os.walk(path):
        for f in files:
            if f.endswith(".parquet"):
                first_pq_path = os.path.join(dir_path, f)
                return first_pq_path
```

看起来是个很酷的函数，让我们把它付诸实践。

```py
path = 'APPLICATIONS_PARTITIONED'
first_pq = get_first_parquet_from_path(path)
first_pq
#Output : APPLICATIONS_PARTITIONED/NAME_INCOME_TYPE=Commercial associate/CODE_GENDER=F/6183f182ab0b47c49cf56a3e09a3a7b1-0.parquet
```

从路径中我们可以注意到，这里按*NAME_INCOME_TYPE*和*CODE_GENDER*进行分区，知道这一点很重要。

现在要读取这个路径以获取行数和列数，以及宝贵的*Schema*，你可以这样做：

```py
first_ds = pq.read_table(first_pq)
first_ds.num_rows, first_ds.num_columns, first_ds.schema
```

![](img/28fe5e017ccb8531099e2f2908ff3087.png)![](img/574909a9cf4377be5dd29afccbf17a8f.png)

执行时间不到 1 秒，原因是`read_table()`函数读取一个*Parquet*文件并返回一个*PyArrow Table*对象，该对象代表你的数据，作为一个由*Apache Arrow*开发的优化数据结构。

现在，我们知道有 637800 行和 17 列（+2 来自路径），并对变量及其类型有了概览。

等等，我之前告诉过你，我们不需要在内存中加载任何东西来发现数据。所以这里有一个方法，可以在不读取任何表的情况下做到这一点。

# **元数据**

我在部分欺骗你，因为我们不会加载任何数据，而是会加载所谓的**元数据**。

在*Parquet*文件格式的上下文中，**metadata**指的是描述文件中存储的数据的*结构*和*特征*的数据。这包括每列的数据类型、列的名称、表中的行数和模式等信息。

让我们使用*pyarrow.parquet*中的`read_metadata()`和`read_schema()`函数：

```py
ts=pq.read_metadata(first_pq)
ts.num_rows, ts.num_columns, pq.read_schema(first_pq)
```

![](img/5036cc5bf641ce4f28e2b9e1c462cdfe.png)

这会给你与`read_table()`相同的输出。

然而，我们注意到执行时间上有很大差异，因为这里接近瞬时。这并不奇怪，因为读取**metadata**就像是读取*Parquet*文件中一个非常小的部分，它包含了你需要的所有信息来概述数据。

# 统计数据

现在假设我想多了解一下列，我该怎么办？

你可以从文件的第一个*Row Group*读取统计数据。

在*Parquet*文件格式中，*Row Group*是将行作为一个单位存储在一起的集合，并分成更小的块以便于查询和处理。

```py
parquet_file = pq.ParquetFile(first_pq)
ts=parquet_file.metadata.row_group(0)
for nm in range(ts.num_columns):
    print(ts.column(nm))
```

上面的代码会给你一个不太美观的输出，这里有一些代码可以将其格式化为一个漂亮的 DataFrame：

```py
beautiful_df = pd.DataFrame()
for nm in range(ts.num_columns):
    path_in_schema = ts.column(nm).path_in_schema
    compressed_size = ts.column(nm).total_compressed_size
    stats = ts.column(nm).statistics
    min_value = stats.min
    max_value = stats.max
    physical_type = stats.physical_type
    beautiful_df[path_in_schema] = pd.DataFrame([physical_type, min_value, max_value, compressed_size])
df = beautiful_df.T
df.columns = ['DTYPE', 'Min', 'Max', 'Compressed_Size_(KO)']
```

![](img/bc143c03768b4a4c06daabdcd3fc1434.png)

在 DataFrame 中，你可以看到列的类型、最小值、最大值和压缩大小。从这个文件中得到的一些学习点：

+   字符串列被转换为*BYTE_ARRAY*。

+   字符串列的最小值和最大值按字母顺序排序。

+   布尔型的压缩大小不比*BYTE_ARRAY*好多少。

+   最年轻的申请者 21 岁，最年长的是 68 岁。

要注意不要将统计数据泛化，这只是来自第一个*parquet*文件！

很好，现在我们对数据有了很好的理解，包括列的信息、类型、模式，甚至统计数据，但我们是否遗漏了什么？

# 分区

是的，我们不知道数据的*分区*！如前所述，我们可以从文件路径中至少猜测到分区列：

![](img/d6c24006151705c154c2edf0a52a3fc2.png)

数据按*NAME_INCOME_TYPE*和*CODE_GENDER*分区。但我们不知道其他分区值。假设我们想查看其他*NAME_INCOME_TYPE*？

但我会提供你一段代码，这样你可以以更系统的方式获取*分区*，以及所有可能的*分区*值：

```py
def get_all_partitions(path):
    partitions = {}
    i = 0
    for (_, partitions_layer, _) in os.walk(path):
        if len(partitions_layer)>0:
            key = partitions_layer[0].split('=')[0]
            partitions[key] = sorted([partitions_layer[i].split('=')[1] for i in range(len(partitions_layer))])
        else:
            break
    return partitions
```

让我们运行这个函数，它返回一个字典，其中键对应于*分区列*，值是与每个分区列关联的*分区值*。

```py
ps = get_all_partitions(path)
ps.keys(), ps.values()
```

我们现在知道数据工程师首先按*Income_Type*分区，然后按*Gender*分区。所有分区列的值如下：

![](img/20b37f7d962052bb684ae8055d32f3c8.png)

既然我们已经了解了分区列和分区值的知识，我们可以读取另一个感兴趣的分区。

假设我们想读取所有*‘Pensioner’*的数据，无论*Gender*是什么。

从上一个教程中，我们知道我们可以通过读取 Parquet 文件夹*‘APPLICATIONS_PARTITIONED/NAME_INCOME_TYPE=Pensioner’*来做到这一点

```py
df_pensioner = pd.read_parquet('APPLICATIONS_PARTITIONED/NAME_INCOME_TYPE=Pensioner/')
```

# 重新格式化分区

实际上，我们不打算按性别拆分数据，而且数据的大小允许我们在没有过多运行时间的情况下读取两个性别的数据。

不要过度分区数据，因为通常，执行时间会随着文件夹中的分区数量增加而增加。因此，你必须记住，分区即使使数据在功能上更易读，也可能有潜在的缺点。（来自[官方文档](https://parquet.apache.org/docs/file-format/configurations/) 512MB — 1GB 是分区的最佳大小）。

在这里，假设在检查数据后，我们认为性别的子文件夹足够小，并且发现性别的功能划分没有用处。我们决定将数据集重新格式化，仅按*NAME_INCOME_TYPE*进行分区：

```py
pq_table = pq.read_table('APPLICATIONS_PARTITIONED')
pq.write_to_dataset(pq_table, 'APPLICATIONS_REPARTITIONED', partition_cols=['NAME_INCOME_TYPE'])
```

我们刚刚在*PyArrow Table* 对象中读取了数据，然后我们写了一个*Parquet* 文件，仅按*NAME_INCOME_TYPE*分区，不再按*性别*分区。如果我们现在运行`get_all_partitions()`函数，值为：

```py
partitions = get_all_partitions('APPLICATIONS_REPARTITIONED')
partitions.keys(), partitions.values()
```

![](img/b2f89baa600c8886b3f42982505e126c.png)

我们注意到，我们不再按*性别*进行分区。

总之，你刚刚了解了如何浏览 Parquet 文件，了解有关数据的一切：例如列名、大小、模式、统计信息以及如何获取分区名称和值。你还发现了如何重新格式化分区以使其在技术上和功能上更为正确。

感谢阅读，下次故事见！

生成我们所用输入数据的完整代码：

*继续阅读我其他的 Parquet 文章：*

[](/easy-parquet-tutorial-best-practices-237955e46cb7?source=post_page-----f854c57a45b6--------------------------------) ## 简单的 Parquet 教程和最佳实践

### 实用教程，开始你的 Parquet 学习

towardsdatascience.com [](https://pub.towardsai.net/parquet-best-practices-the-art-of-filtering-d729357e441d?source=post_page-----f854c57a45b6--------------------------------) [## Parquet 最佳实践：筛选的艺术

### 理解如何筛选 Parquet 文件

pub.towardsai.net](https://pub.towardsai.net/parquet-best-practices-the-art-of-filtering-d729357e441d?source=post_page-----f854c57a45b6--------------------------------)

*通过我的推荐链接，你可以无额外费用地订阅 Medium。*

[](https://medium.com/@arli94/membership?source=post_page-----f854c57a45b6--------------------------------) [## 使用我的推荐链接加入 Medium — Arli

### 阅读 Arli 和成千上万其他 Medium 作者的每一个故事。你的会员费用直接支持 Arli 和…

[medium.com](https://medium.com/@arli94/membership?source=post_page-----f854c57a45b6--------------------------------)
