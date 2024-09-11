# 类型提示数据框用于静态分析和运行时验证

> 原文：[https://towardsdatascience.com/type-hinting-dataframes-for-static-analysis-and-runtime-validation-3dedd2df481d?source=collection_archive---------3-----------------------#2023-11-16](https://towardsdatascience.com/type-hinting-dataframes-for-static-analysis-and-runtime-validation-3dedd2df481d?source=collection_archive---------3-----------------------#2023-11-16)

## StaticFrame如何实现全面的数据框类型提示

[](https://medium.com/@flexatone?source=post_page-----3dedd2df481d--------------------------------)[![Christopher Ariza](../Images/35208ace15080724e4cd6690e43d6502.png)](https://medium.com/@flexatone?source=post_page-----3dedd2df481d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3dedd2df481d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3dedd2df481d--------------------------------) [Christopher Ariza](https://medium.com/@flexatone?source=post_page-----3dedd2df481d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6a4f500b1e4f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftype-hinting-dataframes-for-static-analysis-and-runtime-validation-3dedd2df481d&user=Christopher+Ariza&userId=6a4f500b1e4f&source=post_page-6a4f500b1e4f----3dedd2df481d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3dedd2df481d--------------------------------) · 9分钟阅读 · 2023年11月16日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3dedd2df481d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftype-hinting-dataframes-for-static-analysis-and-runtime-validation-3dedd2df481d&source=-----3dedd2df481d---------------------bookmark_footer-----------)![](../Images/fcedb934dd797193272db95f8b6f913b.png)

作者照片

自从Python 3.5引入类型提示以来，静态类型数据框通常只限于指定类型：

```py
def process(f: DataFrame) -> Series: ...
```

这是不足的，因为它忽略了容器中包含的类型。一个 DataFrame 可能具有字符串列标签和三列整数、字符串和浮点值；这些特征定义了类型。具有此类类型提示的函数参数为开发人员、静态分析器和运行时检查器提供了理解接口期望的所有信息。 [StaticFrame](https://github.com/static-frame/static-frame) 2（我是该开源项目的主要开发者）现在允许这样做：

```py
from typing import Any
from static_frame import Frame, Index, TSeriesAny

def process(f: Frame[   # type of the container
        Any,            # type of the index labels
        Index[np.str_], # type of the column labels
        np.int_,        # type of the first column
        np.str_,        # type of the second column
        np.float64,     # type of the third column
        ]) -> TSeriesAny: ...
```

所有核心的 StaticFrame 容器现在都支持泛型规范。虽然可以静态检查，但一个新的装饰器 `[@CallGuard](http://twitter.com/CallGuard).check` 允许在函数接口上对这些类型提示进行运行时验证。此外，使用 `Annotated` 泛型，新的 `Require` 类定义了一系列强大的运行时验证器，允许按列或按行数据进行检查。最后，每个容器都暴露了一个新的 `via_type_clinic` 接口，用于推导和验证类型提示。这些工具共同提供了一种完整的类型提示和验证 DataFrame 的方法。

## 泛型 DataFrame 的要求

Python 的内置泛型类型（例如 `tuple` 或 `dict`）需要指定组件类型（例如 `tuple[int, str, bool]` 或 `dict[str, int]`）。定义组件类型可以更准确地进行静态分析。尽管对于 DataFrame 也是如此，但很少有人尝试定义全面的 DataFrame 类型提示。

即使使用 `pandas-stubs` 包，Pandas 也不允许指定 DataFrame 组件的类型。Pandas DataFrame 允许广泛的原地变异，可能不适合进行静态类型化。幸运的是，StaticFrame 中提供了不可变的 DataFrame。

此外，直到最近，Python 用于定义泛型的工具并不适合 DataFrame。一个 DataFrame 具有可变数量的异构列类型对于泛型规范是一个挑战。使用在 Python 3.11 中引入的新的 `TypeVarTuple`（并在 `typing_extensions` 包中进行了回溯）更容易地对这种结构进行类型化。

`TypeVarTuple` 允许定义可以接受多个类型的泛型。 (详见 [PEP 646](https://peps.python.org/pep-0646))。借助这种新的类型变量，StaticFrame 可以定义一个通用的 `Frame`，它具有用于索引的 `TypeVar`、用于列的 `TypeVar`，以及用于零个或多个列类型的 `TypeVarTuple`。

泛型 `Series` 定义了一个用于索引的 `TypeVar` 和一个用于值的 `TypeVar`。StaticFrame 的 `Index` 和 `IndexHierarchy` 也是泛型的，后者再次利用 `TypeVarTuple` 来定义每个深度级别的可变数量的组件 `Index`。

StaticFrame使用NumPy类型定义`Frame`的列类型，或者`Series`或`Index`的值类型。这允许严格指定大小的数值类型，如`np.uint8`或`np.complex128`；或广泛指定类型的类别，如`np.integer`或`np.inexact`。由于StaticFrame支持所有NumPy类型，因此对应关系是直接的。

## 使用泛型数据框定义接口

在上述示例的基础上，以下函数接口显示了将三列`Frame`转换为`Series`字典。通过组件类型提示提供了更多信息，函数的目的几乎显而易见。

```py
from typing import Any
from static_frame import Frame, Series, Index, IndexYearMonth

def process(f: Frame[
        Any,
        Index[np.str_],
        np.int_,
        np.str_,
        np.float64,
        ]) -> dict[
                int,
                Series[                 # type of the container
                        IndexYearMonth, # type of the index labels
                        np.float64,     # type of the values
                        ],
                ]: ...
```

此函数处理来自[开源资产定价](https://www.openassetpricing.com)（OSAP）数据集（公司级特征/个体/预测器）的信号表。每个表具有三列：安全标识符（标记为“permno”）、年和月（标记为“yyyymm”）以及信号（具有特定信号的名称）。

该函数忽略所提供`Frame`的索引（类型为`Any`），并创建由第一列“permno” `np.int_`值定义的组。返回以“permno”为键的字典，其中每个值是该“permno”的`np.float64`值的`Series`；索引是从`np.str_`“yyyymm”列创建的`IndexYearMonth`。（StaticFrame使用NumPy `datetime64`值来定义单位类型的索引：`IndexYearMonth`存储`datetime64[M]`标签。）

以下函数不返回`dict`，而是返回具有分层索引的`Series`。`IndexHierarchy`泛型指定了每个深度级别的组件`Index`；在此处，外部深度是从“permno”列派生的`Index[np.int_]`，内部深度是从“yyyymm”列派生的`IndexYearMonth`。

```py
from typing import Any
from static_frame import Frame, Series, Index, IndexYearMonth, IndexHierarchy

def process(f: Frame[
        Any,
        Index[np.str_],
        np.int_,
        np.str_,
        np.float64,
        ]) -> Series[                    # type of the container
                IndexHierarchy[          # type of the index labels
                        Index[np.int_],  # type of index depth 0
                        IndexYearMonth], # type of index depth 1
                np.float64,              # type of the values
                ]: ...
```

丰富的类型提示提供了一个自描述的接口，使功能明确。更好的是，这些类型提示可以用于与Pyright（现在）和Mypy（待完全支持`TypeVarTuple`）的静态分析。例如，使用两列`np.float64`的`Frame`调用此函数将在编辑器中失败静态分析类型检查或提供警告。

## 运行时类型验证

静态类型检查可能不足够：运行时评估提供了更强的约束，特别是对于动态或未完全（或错误地）类型提示的值。

基于名为`TypeClinic`的新运行时类型检查器，StaticFrame 2引入了`[@CallGuard](http://twitter.com/CallGuard).check`，一个用于类型提示接口运行时验证的装饰器。支持所有StaticFrame和NumPy泛型，并支持大多数内置Python类型，即使嵌套深度很深。以下函数添加了`[@CallGuard](http://twitter.com/CallGuard).check`装饰器。

```py
from typing import Any
from static_frame import Frame, Series, Index, IndexYearMonth, IndexHierarchy, CallGuard

@CallGuard.check
def process(f: Frame[
        Any,
        Index[np.str_],
        np.int_,
        np.str_,
        np.float64,
        ]) -> Series[
                IndexHierarchy[Index[np.int_], IndexYearMonth],
                np.float64,
                ]: ...
```

现在使用 `[@CallGuard](http://twitter.com/CallGuard).check` 装饰，如果上述函数用于未标记的 `np.float64` 两列的 `Frame`，则会引发 `ClinicError` 异常，说明预期有三列，但只提供了两列，并且预期字符串列标签，但提供了整数标签。（要发出警告而不是引发异常，请使用 `[@CallGuard](http://twitter.com/CallGuard).warn` 装饰。）

```py
ClinicError:
In args of (f: Frame[Any, Index[str_], int64, str_, float64]) -> Series[IndexHierarchy[Index[int64], IndexYearMonth], float64]
└── Frame[Any, Index[str_], int64, str_, float64]
    └── Expected Frame has 3 dtype, provided Frame has 2 dtype
In args of (f: Frame[Any, Index[str_], int64, str_, float64]) -> Series[IndexHierarchy[Index[int64], IndexYearMonth], float64]
└── Frame[Any, Index[str_], int64, str_, float64]
    └── Index[str_]
        └── Expected str_, provided int64 invalid
```

## 运行时数据验证

其他特性可以在运行时进行验证。例如，`shape` 或 `name` 属性，或者索引或列上的标签顺序。StaticFrame 的 `Require` 类提供了一系列可配置的验证器。

+   `Require.Name`: 验证容器的 `name` 属性。

+   `Require.Len`: 验证容器的长度。

+   `Require.Shape`: 验证容器的 `shape` 属性。

+   `Require.LabelsOrder`: 验证标签的顺序。

+   `Require.LabelsMatch`: 验证包含标签而不考虑顺序。

+   `Require.Apply`: 将返回布尔值的函数应用于容器。

符合增长趋势，这些对象作为一个或多个额外参数提供给 `Annotated` 泛型的类型提示。 （有关详细信息，请参阅 [PEP 593](https://peps.python.org/pep-0593)。）第一个 `Annotated` 参数引用的类型是后续参数验证器的目标。例如，如果将 `Index[np.str_]` 类型提示替换为 `Annotated[Index[np.str_], Require.Len(20)]` 类型提示，则会对与第一个参数关联的索引应用运行时长度验证。

扩展处理 OSAP 信号表的示例，我们可以验证列标签的期望。`Require.LabelsOrder` 验证器可以定义一系列标签，可选地使用 `…` 表示零个或多个未指定的标签。为了指定表的前两列标签为 “permno” 和 “yyyymm”，而第三个标签是可变的（取决于信号），可以在 `Annotated` 泛型内定义以下 `Require.LabelsOrder`：

```py
from typing import Any, Annotated
from static_frame import Frame, Series, Index, IndexYearMonth, IndexHierarchy, CallGuard, Require

@CallGuard.check
def process(f: Frame[
        Any,
        Annotated[
                Index[np.str_],
                Require.LabelsOrder('permno', 'yyyymm', ...),
                ],
        np.int_,
        np.str_,
        np.float64,
        ]) -> Series[
                IndexHierarchy[Index[np.int_], IndexYearMonth],
                np.float64,
                ]: ...
```

如果接口期望小集合的 OSAP 信号表，我们可以使用 `Require.LabelsMatch` 验证器验证第三列。该验证器可以指定必需的标签、标签集合（其中至少一个必须匹配）和正则表达式模式。如果只期望来自三个文件的表（即 “Mom12m.csv”、“Mom6m.csv” 和 “LRreversal.csv”），我们可以通过定义 `Require.LabelsMatch` 与集合来验证第三列的标签：

```py
@CallGuard.check
def process(f: Frame[
        Any,
        Annotated[
                Index[np.str_],
                Require.LabelsOrder('permno', 'yyyymm', ...),
                Require.LabelsMatch({'Mom12m', 'Mom6m', 'LRreversal'}),
                ],
        np.int_,
        np.str_,
        np.float64,
        ]) -> Series[
                IndexHierarchy[Index[np.int_], IndexYearMonth],
                np.float64,
                ]: ...
```

`Require.LabelsOrder` 和 `Require.LabelsMatch` 都可以将函数与标签说明符关联，以验证数据值。如果验证器应用于列标签，则将一系列列值提供给函数；如果验证器应用于索引标签，则将一系列行值提供给函数。

类似于`Annotated`的用法，标签说明符被替换为一个列表，其中第一个项目是标签说明符，其余项目是返回布尔值的行或列处理函数。

为了扩展上述示例，我们可能需要验证所有“permno”值是否大于零，以及所有信号值（“Mom12m”、“Mom6m”、“LRreversal”）是否大于或等于-1。

```py
from typing import Any, Annotated
from static_frame import Frame, Series, Index, IndexYearMonth, IndexHierarchy, CallGuard, Require

@CallGuard.check
def process(f: Frame[
        Any,
        Annotated[
                Index[np.str_],
                Require.LabelsOrder(
                        ['permno', lambda s: (s > 0).all()],
                        'yyyymm',
                        ...,
                        ),
                Require.LabelsMatch(
                        [{'Mom12m', 'Mom6m', 'LRreversal'}, lambda s: (s >= -1).all()],
                        ),
                ],
        np.int_,
        np.str_,
        np.float64,
        ]) -> Series[
                IndexHierarchy[Index[np.int_], IndexYearMonth],
                np.float64,
                ]: ...
```

如果验证失败，`[@CallGuard](http://twitter.com/CallGuard).check`将引发异常。例如，如果调用上述函数时遇到意外的第三列标签，将引发以下异常：

```py
ClinicError:
In args of (f: Frame[Any, Annotated[Index[str_], LabelsOrder(['permno', <lambda>], 'yyyymm', ...), LabelsMatch([{'Mom12m', 'LRreversal', 'Mom6m'}, <lambda>])], int64, str_, float64]) -> Series[IndexHierarchy[Index[int64], IndexYearMonth], float64]
└── Frame[Any, Annotated[Index[str_], LabelsOrder(['permno', <lambda>], 'yyyymm', ...), LabelsMatch([{'Mom12m', 'LRreversal', 'Mom6m'}, <lambda>])], int64, str_, float64]
    └── Annotated[Index[str_], LabelsOrder(['permno', <lambda>], 'yyyymm', ...), LabelsMatch([{'Mom12m', 'LRreversal', 'Mom6m'}, <lambda>])]
        └── LabelsMatch([{'Mom12m', 'LRreversal', 'Mom6m'}, <lambda>])
            └── Expected label to match frozenset({'Mom12m', 'LRreversal', 'Mom6m'}), no provided match
```

## `TypeVarTuple`的表达能力

如上所示，`TypeVarTuple`允许指定具有零个或多个异构列类型的`Frame`。例如，我们可以为两个浮点数或六种混合类型的`Frame`提供类型提示：

```py
>>> from typing import Any
>>> from static_frame import Frame, Index

>>> f1: sf.Frame[Any, Any, np.float64, np.float64]
>>> f2: sf.Frame[Any, Any, np.bool_, np.float64, np.int8, np.int8, np.str_, np.datetime64]
```

虽然这适用于各种DataFrame，但对宽型DataFrame（例如具有数百列的DataFrame）的类型提示可能会显得笨拙。Python 3.11引入了一种新的语法，通过`TypeVarTuple`泛型提供可变范围的类型：`tuple`泛型别名的星号表达式。例如，要对具有日期索引、字符串列标签和任意列类型配置的`Frame`进行类型提示，我们可以星号解包零个或多个`All`的`tuple`。

```py
>>> from typing import Any
>>> from static_frame import Frame, Index

>>> f: sf.Frame[Index[np.datetime64], Index[np.str_], *tuple[All, ...]]
```

`tuple`星号表达式可以出现在类型列表中的任何位置，但只能有一个。例如，下面的类型提示定义了一个必须以布尔值和字符串列开始的`Frame`，但对后续的`np.float64`列数量有灵活的规定。

```py
>>> from typing import Any
>>> from static_frame import Frame

>>> f: sf.Frame[Any, Any, np.bool_, np.str_, *tuple[np.float64, ...]]
```

## 类型提示工具

使用如此详细的类型提示可能会很具挑战性。为了帮助用户，StaticFrame提供了便捷的运行时类型提示和检查工具。所有StaticFrame 2容器现在都具备`via_type_clinic`接口，允许访问`TypeClinic`功能。

首先，提供了将容器（例如完整的`Frame`）转换为类型提示的工具。`via_type_clinic`接口的字符串表示提供了容器类型提示的字符串表示；另外，`to_hint()`方法返回一个完整的泛型别名对象。

```py
>>> import static_frame as sf
>>> f = sf.Frame.from_records(([3, '192004', 0.3], [3, '192005', -0.4]), columns=('permno', 'yyyymm', 'Mom3m'))

>>> f.via_type_clinic
Frame[Index[int64], Index[str_], int64, str_, float64]

>>> f.via_type_clinic.to_hint()
static_frame.core.frame.Frame[static_frame.core.index.Index[numpy.int64], static_frame.core.index.Index[numpy.str_], numpy.int64, numpy.str_, numpy.float64]
```

其次，提供了用于运行时类型提示测试的工具。`via_type_clinic.check()`函数允许根据提供的类型提示验证容器。

```py
>>> f.via_type_clinic.check(sf.Frame[sf.Index[np.str_], sf.TIndexAny, *tuple[tp.Any, ...]])
ClinicError:
In Frame[Index[str_], Index[Any], Unpack[Tuple[Any, ...]]]
└── Index[str_]
    └── Expected str_, provided int64 invalid
```

为支持渐进类型，StaticFrame定义了几个配置为每种组件类型的`Any`的泛型别名。例如，`TFrameAny`可用于任何`Frame`，而`TSeriesAny`用于任何`Series`。如预期的那样，`TFrameAny`将验证上面创建的`Frame`。

```py
>>> f.via_type_clinic.check(sf.TFrameAny)
```

## 结论

更好的DataFrame类型提示早已迫切需要。凭借现代Python类型工具和基于不可变数据模型构建的DataFrame，StaticFrame 2满足了这一需求，为优先考虑可维护性和可验证性的工程师提供了强大的资源。
