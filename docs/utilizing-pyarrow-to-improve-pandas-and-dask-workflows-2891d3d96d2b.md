# 利用 PyArrow 改进 pandas 和 Dask 工作流

> 原文：[https://towardsdatascience.com/utilizing-pyarrow-to-improve-pandas-and-dask-workflows-2891d3d96d2b?source=collection_archive---------4-----------------------#2023-06-06](https://towardsdatascience.com/utilizing-pyarrow-to-improve-pandas-and-dask-workflows-2891d3d96d2b?source=collection_archive---------4-----------------------#2023-06-06)

## *现在最大限度地利用 PyArrow 在 pandas 和 Dask 中的支持*

[](https://medium.com/@patrick_hoefler?source=post_page-----2891d3d96d2b--------------------------------)[![Patrick Hoefler](../Images/35ca9ef1100d8c93dbadd374f0569fe1.png)](https://medium.com/@patrick_hoefler?source=post_page-----2891d3d96d2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2891d3d96d2b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2891d3d96d2b--------------------------------) [Patrick Hoefler](https://medium.com/@patrick_hoefler?source=post_page-----2891d3d96d2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F103b3417e0f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Futilizing-pyarrow-to-improve-pandas-and-dask-workflows-2891d3d96d2b&user=Patrick+Hoefler&userId=103b3417e0f5&source=post_page-103b3417e0f5----2891d3d96d2b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2891d3d96d2b--------------------------------) · 13 min 阅读 · 2023年6月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2891d3d96d2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Futilizing-pyarrow-to-improve-pandas-and-dask-workflows-2891d3d96d2b&user=Patrick+Hoefler&userId=103b3417e0f5&source=-----2891d3d96d2b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2891d3d96d2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Futilizing-pyarrow-to-improve-pandas-and-dask-workflows-2891d3d96d2b&source=-----2891d3d96d2b---------------------bookmark_footer-----------)![](../Images/1fd9277102c552e00b0d50732a3c12cd.png)

所有图片均由作者创建

## 介绍

本文探讨了我们现在可以在何处使用 PyArrow 来改进我们的 pandas 和 Dask 工作流。对 PyArrow 数据类型的总体支持在 pandas 2.0 中添加到 [pandas](https://pandas.pydata.org) 和 [Dask](https://www.dask.org/?utm_source=tds&utm_medium=pyarrow-in-pandas-and-dask) 中。这解决了两个库用户长期以来的一些痛点。pandas 用户常常向我抱怨 pandas 不支持任意数据类型中的缺失值，或非标准数据类型支持不够好。Dask 用户特别烦恼的问题是处理大数据集时内存不足。与 NumPy 对象列相比，PyArrow 支持的字符串列可以减少多达 70% 的内存消耗，因此有潜力缓解这个问题，并提供巨大的性能提升。

pandas 和 Dask 对 PyArrow 数据类型的支持仍然相对较新。建议在至少 pandas 2.1 发布之前，谨慎选择 PyArrow 的 `dtype_backend`。并非所有 API 部分都已优化。然而，在某些工作流中，你应该能获得显著的改进。本文将介绍几个我建议立即切换到 PyArrow 的示例，因为它已经提供了巨大的好处。

Dask 本身可以通过 PyArrow 数据类型在多方面受益。我们将探讨 PyArrow 支持的字符串如何轻松缓解 Dask 集群内存不足的问题，以及我们如何通过利用 PyArrow 改善性能。

我是 pandas 核心团队的一员，并且在实现和改进 pandas 中的 PyArrow 支持方面参与了大量工作。我最近加入了 [Coiled](https://www.coiled.io/?utm_source=tds&utm_medium=pyarrow-in-pandas-and-dask)，在那里我负责 Dask。我的任务之一是改进 Dask 中的 PyArrow 集成。

## PyArrow 支持的总体概述

PyArrow 数据类型最初是在 pandas 1.5 中引入的。该实现是实验性的，我不建议在 pandas 1.5.x 上使用它。对这些数据类型的支持仍然相对较新。pandas 2.0 提供了巨大的改进，包括使选择 PyArrow 支持的 DataFrames 变得容易。我们仍在努力在各处提供适当的支持，因此在至少 pandas 2.1 发布之前应谨慎使用。这两个项目都在不断努力改进对 Dask 和 pandas 的支持。

我们鼓励用户尝试使用它们！这将帮助我们更好地了解仍然缺乏支持或不够快的地方。反馈有助于我们改进支持，并将显著减少创建流畅用户体验所需的时间。

## 数据集

我们将使用包含所有 Uber 和 Lyft 乘车记录的纽约市出租车数据集。它具有一些有趣的属性，如价格、小费、司机薪酬等。数据集可以在[这里](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)找到（参见 [服务条款](https://www.nyc.gov/home/terms-of-use.page)），并存储在 parquet 文件中。在分析 Dask 查询时，我们将使用一个公开的 S3 存储桶以简化我们的查询：`s3://coiled-datasets/uber-lyft-tlc/`。我们将使用 2022 年 12 月的数据集进行 pandas 查询，因为这是在我的机器（24GB 内存）中舒适地适配的最大数据集。我们必须避免过度使用 RAM，因为这可能会在分析性能时引入副作用。

我们还将研究 `read_csv` 的性能。我们将使用可以在[这里](https://www.kaggle.com/datasets/utkarshx27/crimes-2001-to-present)找到的 *芝加哥犯罪* 数据集。

## Dask 集群

设置 Dask 集群有各种不同的选项，参见 [Dask 文档](https://docs.dask.org/en/stable/deploying.html?utm_source=tds&utm_medium=pyarrow-in-pandas-and-dask) 获取非详尽的部署选项列表。我将使用 [Coiled](https://docs.coiled.io/user_guide/index.html?utm_source=tds&utm_medium=pyarrow-in-pandas-and-dask) 在 AWS 上通过 30 台机器创建一个集群。

```py
import coiled

cluster = coiled.Cluster(
    n_workers=30,
    name="dask-performance-comparisons",
    region="us-east-2",  # this is the region of our dataset
    worker_vm_type="m6i.large",
)
```

Coiled 已连接到我的 AWS 账户。它在我的账户中创建集群并为我管理所有资源。30 台机器足以舒适地操作我们的数据集。我们将研究如何通过一些小的修改将所需的工作节点数减少到 15 台。

## pandas `StringDtype` 由 PyArrow 支持

我们从 pandas 1.0 中最初引入的一个功能开始。将 pandas 或 Dask 中的 dtype 设置为 `string` 会返回一个具有 `StringDtype` 的对象。这个功能相对成熟，应该能提供平滑的用户体验。

历史上，pandas 通过 dtype 为 `object` 的 NumPy 数组来表示字符串数据。NumPy 对象数据存储为指向内存中实际数据的指针数组。这使得遍历包含字符串的数组非常缓慢。pandas 1.0 最初引入了所谓的 `StringDtype`，允许对字符串进行更简单和一致的操作。这个 dtype 仍然由 Python 字符串支持，因此性能也不佳。它提供了字符串数据的清晰抽象。

pandas 1.3 最终引入了一种创建高效字符串 dtype 的增强功能。该数据类型由 PyArrow 数组支持。[PyArrow](https://arrow.apache.org/docs/python/index.html) 提供了一种数据结构，能够进行高效且节省内存的字符串操作。从那时起，用户可以使用在内存中连续的字符串 dtype，因此非常快速。该 dtype 可以通过 `string[pyarrow]` 请求。或者，我们可以通过指定 `string` 作为 dtype 并设置来请求它：

```py
pd.options.mode.string_storage = "pyarrow"
```

由于 Dask 构建在 pandas 之上，这里也可以使用这种字符串数据类型。除此之外，Dask 还提供了一个方便的选项，可以自动将所有字符串数据转换为 `string[pyarrow]`。

```py
dask.config.set({"dataframe.convert-string": True})
```

这是避免字符串列使用 NumPy 对象数据类型的方便方法。此外，它还具有创建原生 PyArrow 数组的优势，适用于处理 Arrow 对象的 I/O 方法。在提供显著性能提升的同时，PyArrow 字符串消耗的内存显著减少。与 NumPy 对象相比，使用 PyArrow 字符串的平均 Dask DataFrame 的内存消耗约为原始内存的 33-50%。这解决了 Dask 用户在处理大型数据集时内存不足的最大痛点。该选项启用了 Dask 测试套件中的全局测试。这确保了 PyArrow 支持的字符串足够成熟，以提供流畅的用户体验。

让我们看几个典型的字符串操作。我们将从一些 pandas 示例开始，然后切换到在 Dask 集群上执行的操作。

我们将使用 `df.convert_dtypes` 将我们的对象列转换为 PyArrow 字符串数组。还有更高效的方法来获取 pandas 中的 PyArrow 数据类型，我们会在后面探讨。我们将使用 2022 年 12 月的 Uber-Lyft 数据集，这个文件在我的机器上可以舒适地放入内存中。

```py
import pandas as pd

pd.options.mode.string_storage = "pyarrow"

df = pd.read_parquet(
    "fhvhv_tripdata_2022-10.parquet",
    columns=[
        "tips", 
        "hvfhs_license_num", 
        "driver_pay", 
        "base_passenger_fare", 
        "dispatching_base_num",
    ],
)
df = df.convert_dtypes(
    convert_boolean=False, 
    convert_floating=False, 
    convert_integer=False,
)
```

在这个示例中，我们的 DataFrame 对所有非字符串列都有 NumPy 数据类型。让我们开始筛选所有由 Uber 操作的乘车记录。

```py
df[df["hvfhs_license_num"] == "HV0003"]
```

这个操作创建了一个 True/False 值的掩码，指定 Uber 是否进行了乘车。这不使用任何特殊的字符串方法，但等式比较会交给 PyArrow。接下来，我们将使用 pandas 实现的 String 访问器，它允许你对每个元素执行各种字符串操作。我们想要找到所有从以 `"B028"` 开头的基地发出的乘车记录。

```py
df[df["dispatching_base_num"].str.startswith("B028")]
```

`startswith` 会遍历我们的数组，检查每个字符串是否以指定的子字符串开头。PyArrow 的优势显而易见。数据在内存中是连续的，这意味着我们可以高效地进行遍历。此外，这些数组还具有一个第二个数组，其中包含指向每个字符串首个内存地址的指针，这使得计算起始序列更快。

最后，我们查看一个 `GroupBy` 操作，它对 PyArrow 字符串列进行分组。分组的计算也可以交给 PyArrow，这比在 NumPy 对象数组上因子化要高效得多。

```py
df.groupby(
    ["hvfhs_license_num", "dispatching_base_num"]
).mean(numeric_only=True)
```

让我们看看这些操作与字符串列由 NumPy 对象数据类型表示的 DataFrames 的对比。

![](../Images/4194a8e1e1222a4c91881bdafb5fd7f2.png)

结果或多或少符合我们的预期。基于字符串的比较在 PyArrow 字符串上执行时显著更快。大多数字符串访问器应该能提供巨大的性能提升。另一个有趣的观察是内存使用，与 NumPy 对象 dtype 相比，减少了大约 50%。我们将进一步用 Dask 详细研究这一点。

Dask 镜像了 pandas API，并将大多数操作委派给 pandas。因此，我们可以使用相同的 API 来访问 PyArrow 字符串。上述选项是全局请求这些选项的便利方式，我们将在这里使用：

```py
dask.config.set({"dataframe.convert-string": True})
```

在开发过程中，这种选项的最大好处之一是能够轻松地在 Dask 中全局测试 PyArrow 字符串，以确保一切顺利。我们将利用 Uber-Lyft 数据集进行探索。该数据集在我们的集群上占用大约 240GB 的内存。我们的初始集群有 30 台机器，足以舒适地进行计算。

```py
import dask
import dask.dataframe as dd
from distributed import wait

dask.config.set({"dataframe.convert-string": True})

df = dd.read_parquet(
    "s3://coiled-datasets/uber-lyft-tlc/",
    storage_options={"anon": True},
)
df = df.persist()
wait(df)  # Wait till the computation is finished
```

我们将数据保存在内存中，以避免 I/O 性能对我们性能测量的影响。我们的数据现在已存储在内存中，这使得访问速度很快。我们将执行类似于 pandas 计算的操作。主要目标之一是展示 pandas 的好处如何转化为在 Dask 分布式环境中的计算。

第一个观察结果是，使用 PyArrow 支持的字符串列的数据框仅消耗 130GB 内存，仅为使用 NumPy 对象列时的一半。我们的数据框中只有几个字符串列，这意味着在切换到 PyArrow 字符串时，字符串列的内存节省实际上比约 50% 更高。因此，我们将在对 PyArrow 字符串列执行操作时将集群大小减少到 15 个工作节点。

```py
cluster.scale(15)
```

我们通过对数据框进行后续筛选来衡量掩码操作和一个字符串访问器的性能。

```py
df = df[df["hvfhs_license_num"] == "HV0003"]
df = df[df["dispatching_base_num"].str.startswith("B028")]
df = df.persist()
wait(df)
```

我们可以看到，可以使用与之前示例中相同的方法。这使得从 pandas 迁移到 Dask 相对容易。

此外，我们将再次对数据执行 `GroupBy` 操作。这在分布式环境中要困难得多，因此结果更具趣味性。之前的操作在大型集群上相对容易并行，而 `GroupBy` 则更具挑战性。

```py
df = df.groupby(
    ["hvfhs_license_num", "dispatching_base_num"]
).mean(numeric_only=True)

df = df.persist()
wait(df)
```

![](../Images/a371858da38112b3610ddc9d4bf49499.png)

我们得到了 2 和 3 倍的显著改进。这一点尤其引人注目，因为我们将集群的大小从 30 台机器减少到 15 台，成本降低了 50%。随后，我们还将计算资源减少了 2 倍，这使得我们的性能提升更加令人印象深刻。因此，性能分别提高了 4 倍和 6 倍。我们可以在较小的集群上执行相同的计算，这样既节省了成本，也在总体上更高效，并且仍然能够获得性能提升。

总结一下，我们看到 PyArrow 字符串列在与 DataFrame 中的 NumPy 对象列比较时有了巨大的改进。切换到 PyArrow 字符串是一个相对较小的改变，但可能极大地提升依赖于字符串数据的平均工作流程的性能和效率。这些改进在 pandas 和 Dask 中都同样明显！

## I/O 方法中的 Engine 关键字

我们现在将看看 pandas 和 Dask 中的 I/O 函数。一些函数有自定义实现，如 `read_csv`，而其他函数则委派给另一个库，如 `read_excel` 委派给 `openpyxl`。其中一些函数新增了 `engine` 关键字，使我们能够委派给 `PyArrow`。PyArrow 解析器默认是多线程的，因此可以提供显著的性能提升。

```py
pd.read_csv("Crimes_-_2001_to_Present.csv", engine="pyarrow")
```

这个配置将返回与其他引擎相同的结果。唯一的区别是使用了 PyArrow 来读取数据。`read_json` 也提供了相同的选项。添加 PyArrow-engines 是为了提供更快的数据读取方式。改进的速度只是一个优势。PyArrow 解析器返回的数据是一个 [PyArrow Table](https://arrow.apache.org/docs/python/generated/pyarrow.Table.html)。PyArrow Table 提供了内置功能来转换为 pandas `DataFrame`。根据数据的不同，这可能需要在转换为 NumPy（字符串、带缺失值的整数等）时进行拷贝，从而带来不必要的减速。这就是 PyArrow `dtype_backend` 的作用。它在 pandas 中实现为一个 `ArrowExtensionArray` 类，背后是一个 [PyArrow ChunkedArray](https://arrow.apache.org/docs/python/generated/pyarrow.ChunkedArray.html)。直接的结果是，从 PyArrow Table 转换为 pandas 非常便宜，因为它不需要任何拷贝。

```py
pd.read_csv(
    "Crimes_-_2001_to_Present.csv", 
    engine="pyarrow", 
    dtype_backend="pyarrow",
)
```

这将返回一个由 PyArrow 数组支持的 `DataFrame`。pandas 并未在所有方面都得到优化，因此这可能会在后续操作中造成减速。如果工作负载特别重 I/O，这可能是值得的。让我们看一下直接比较：

![](../Images/131457fefa9bd06c86d134bcffb68187.png)

我们可以看到，与 C-engine 相比，PyArrow-engine 和 PyArrow dtypes 提供了 15 倍的加速。

相同的优势适用于 Dask。Dask 封装了 pandas 的 csv 读取器，因此，它免费获得了相同的功能。

对 Dask 的比较要复杂一些。首先，我的示例从本地机器读取数据，而我们的 Dask 示例将从 S3 存储桶中读取数据。网络速度将是一个相关因素。此外，分布式计算也有一些开销，我们需要考虑到这一点。

我们这里只关注速度，所以我们将从一个公共 S3 存储桶中读取一些时间序列数据。

```py
import dask.dataframe as dd
from distributed import wait

df = dd.read_csv(
    "s3://coiled-datasets/timeseries/20-years/csv/",
    storage_options={"anon": True},
    engine="pyarrow",
    parse_dates=["timestamp"],
)
df = df.persist()
wait(df)
```

我们将为 `engine="c"`、`engine="pyarrow"` 以及额外的 `engine="pyarrow"` 配置 `dtype_backend="pyarrow"` 执行这个代码片段。让我们来看看一些性能比较。两个示例都在集群上的 30 台机器上执行。

![](../Images/f558e50482cde8aa566482578a74e017.png)

PyArrow引擎运行速度约为C引擎的两倍。两种实现使用了相同数量的机器。使用PyArrow `dtype_backend`时内存使用量减少了50%。如果仅将对象列转换为PyArrow字符串，也可以实现相同的减少，这能在后续操作中提供更好的体验。

我们已经看到Arrow引擎提供了显著的速度提升，超过了自定义C实现。它们尚未支持自定义实现的所有功能，但如果你的使用场景与支持的选项兼容，你应该能免费获得显著的速度提升。

使用PyArrow `dtype_backend`的情况稍微复杂一些。并非所有API区域都经过优化。如果你在I/O函数之外花费大量时间处理数据，那么这可能无法满足你的需求。如果你的工作流程在读取数据时花费大量时间，它会加速你的处理。

## PyArrow原生I/O读取器中的dtype_backend

其他一些I/O方法也有一个engine关键字。`read_parquet`是最常见的例子。不过这里的情况稍有不同。这些I/O方法默认已经使用PyArrow引擎。因此解析尽可能高效。另一个潜在的性能提升是使用`dtype_backend`关键字。通常，PyArrow会返回一个PyArrow表，然后转换为pandas DataFrame。PyArrow的数据类型会转换为其NumPy等效类型。设置`dtype_backend="pyarrow"`可以避免这种转换。这能显著提高性能并节省大量内存。

让我们看看pandas性能的比较。我们从2022年12月读取了Uber-Lyft出租车数据。

```py
pd.read_parquet("fhvhv_tripdata_2022-10.parquet")
```

我们读取了带有和不带有`dtype_backend="pyarrow"`的数据。

![](../Images/ce5a2300466ab2ccfef7c65e92ea5079.png)

我们可以很容易地看到，转换所花费的时间最多，读取Parquet文件后完成的转换。如果避免转换为NumPy数据类型，函数运行速度提高了三倍。

Dask为`read_parquet`提供了一个专门的实现，相比于pandas实现，具有一些针对分布式工作负载的优势。共同点是这两个函数都调度到PyArrow来读取parquet文件。两者都共同点是数据在成功读取文件后会转换为NumPy数据类型。我们正在读取整个Uber-Lyft数据集，这在我们的集群上消耗了约240GB的内存。

```py
import dask.dataframe as dd
from distributed import wait

df = dd.read_parquet(
    "s3://coiled-datasets/uber-lyft-tlc/",
    storage_options={"anon": True},
)
df = df.persist()
wait(df)
```

我们在3种不同的配置下读取了数据集。首先是使用默认的NumPy数据类型，然后打开PyArrow字符串选项：

```py
dask.config.set({"dataframe.convert-string": True})
```

最后是`dtype_backend="pyarrow"`。让我们看看这在性能方面意味着什么：

![](../Images/3a880dae2241fbb7eb36e94977d32ffb.png)

类似于我们的 pandas 示例，我们可以看到转换为 NumPy dtypes 会占用我们大量的运行时间。PyArrow dtypes 提供了显著的性能提升。两个 PyArrow 配置所使用的内存是 NumPy dtypes 的一半。

PyArrow 字符串比一般的 PyArrow `dtype_backend` 更成熟。根据我们得到的性能图表，当使用 PyArrow 字符串和 NumPy dtypes 对于所有其他 dtypes 时，我们得到的性能提升大致相同。如果某个工作流程在 PyArrow dtypes 上的表现还不够好，我建议仅启用 PyArrow 字符串。

## 结论

我们已经看到如何在 pandas 和 Dask 中利用 PyArrow。PyArrow 支持的字符串列有潜力以积极的方式影响大多数工作流程，并为 pandas 2.0 提供顺畅的用户体验。Dask 提供了一个方便的选项，可以在可能的情况下全局避免使用 NumPy 对象 dtype，这使得选择 PyArrow 支持的字符串变得更加容易。PyArrow 在其他可用领域也提供了巨大的加速。PyArrow 的 `dtype_backend` 仍然相当新，但有潜力显著减少 I/O 时间。探索它是否能解决性能瓶颈是非常值得的。许多工作正在进行中，以改进对一般 PyArrow dtypes 的支持，并有潜力在不久的将来加速平均工作流程。

pandas 目前有一个提议，从 pandas 3.0 开始默认将字符串推断为 PyArrow 支持的字符串。此外，这还包括更多领域，其中更多地依赖 PyArrow 是非常有意义的（例如：Decimals、结构化数据等）。您可以在[这里](https://github.com/pandas-dev/pandas/pull/52711)阅读提议的详细信息。

感谢阅读。欢迎在评论中分享您对两个库中 PyArrow 支持的想法和反馈。我将会写后续的文章，专注于这个话题以及 pandas 一般内容。如果您喜欢阅读关于 pandas 和 Dask 的更多内容，可以关注我在 Medium 上的更新。
