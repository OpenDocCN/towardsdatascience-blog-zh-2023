# Rust Polars：解锁高性能数据分析 — 第二部分

> 原文：[https://towardsdatascience.com/rust-polars-unlocking-high-performance-data-analysis-part-2-7c58a3cb7a1f?source=collection_archive---------3-----------------------#2023-05-18](https://towardsdatascience.com/rust-polars-unlocking-high-performance-data-analysis-part-2-7c58a3cb7a1f?source=collection_archive---------3-----------------------#2023-05-18)

## 探索 Rust 的 Polars 数据框架、聚合函数及更多

[](https://wiseai.medium.com/?source=post_page-----7c58a3cb7a1f--------------------------------)[![Mahmoud Harmouch](../Images/d61617549d25565399975debaad5908f.png)](https://wiseai.medium.com/?source=post_page-----7c58a3cb7a1f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7c58a3cb7a1f--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7c58a3cb7a1f--------------------------------) [Mahmoud Harmouch](https://wiseai.medium.com/?source=post_page-----7c58a3cb7a1f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb15db3da5667&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frust-polars-unlocking-high-performance-data-analysis-part-2-7c58a3cb7a1f&user=Mahmoud+Harmouch&userId=b15db3da5667&source=post_page-b15db3da5667----7c58a3cb7a1f---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----7c58a3cb7a1f--------------------------------) ·24分钟阅读·2023年5月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7c58a3cb7a1f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frust-polars-unlocking-high-performance-data-analysis-part-2-7c58a3cb7a1f&user=Mahmoud+Harmouch&userId=b15db3da5667&source=-----7c58a3cb7a1f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7c58a3cb7a1f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frust-polars-unlocking-high-performance-data-analysis-part-2-7c58a3cb7a1f&source=-----7c58a3cb7a1f---------------------bookmark_footer-----------)![](../Images/700d954aea76580261342f38363b4daf.png)

图片来源：[艾伦](https://pixabay.com/users/ad_images-6663717/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3348777) 来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3348777)

## TL;DR

Rust 编程语言在业界掀起了波澜，并且在数据科学领域逐渐获得关注。它卓越的速度和强大的安全特性受到需要有效管理大型数据集的开发者的高度追捧。Polars 库充分利用了 Rust 的能力，提供了快速高效的复杂数据集处理方法。凭借其卓越的性能，对于那些从事需要快速处理能力的复杂项目的工作者来说，它是一个极具吸引力的选择。

本文作为该系列的延续，旨在揭开 Polars 世界的神秘面纱。[**在系列的第一部分**](/rust-polars-unlocking-high-performance-data-analysis-part-1-ce42af370ece)中，我们学习了 Rust 的 Polars 系列对象及其应用等内容。在这一部分中，我们将探索另一个 Polars 的基本数据结构，即 **DataFrame** 对象。通过实际操作和代码片段，你将获得执行各种 DataFrame 操作等重要技能。

> **注意：** 本文假设你对 Rust 编程语言有相当基础的了解。
> 
> 为了本文而开发的笔记本名为 [**4-polars-tutorial-part-2.ipynb**](https://github.com/wiseaidev/rust-data-analysis/blob/main/4-polars-tutorial-part-2.ipynb)，可以在以下仓库中找到：

[](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----7c58a3cb7a1f--------------------------------) [## GitHub - wiseaidev/rust-data-analysis: 终极 Rust 数据分析课程。]

### 本仓库包含了一系列 Jupyter 笔记本，所有笔记本都由 Rust 内核支持。通过这些笔记本，你将能够…

github.com](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----7c58a3cb7a1f--------------------------------)

## 目录（TOC）

∘ [DataFrame 对象](#6752)

∘ [索引与切片](#5a00)

∘ [数据清理](#bf0b)

∘ [集中趋势测量](#406b)

∘ [Ndarray](#b0f8)

∘ [聚合函数](#6902)

∘ [合并 DataFrame](#b011)

∘ [结论](#4c07)

∘ [结束语](#7e8f)

∘ [资源](#7546)

## DataFrame 对象

![](../Images/042a84b685ff0191afef47ee5ecb0fb1.png)

Polars 数据框表示（作者提供的图片）

在 Polars 库的核心是一个重要的组件，它作为其基础；即 [**DataFrame**](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html#) 结构。这一巧妙的 **二维数据** 表示以 **行和列** 组织，类似于系列对象，但增加了维度。

## DataFrame 初始化

在 Polars 中，初始化数据框和使用强大的 [**DataFrame**](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html) 结构一样简单。为了说明 DataFrame 初始化的简单性，下面是创建一个空数据框的代码片段：

```py
let df = DataFrame::default();
```

现在，让我们深入探讨 Polars 库的灵活性。看看这段代码片段，其中 series 轻松转换为**二维 DataFrame**：

```py
let s1 = Series::new("Name", &["Mahmoud", "Ali"]);
let s2 = Series::new("Age", &[23, 27]);
let s3 = Series::new("Height", &[1.84, 1.78]);
let df: PolarsResult<DataFrame> = DataFrame::new(vec![s1, s2, s3])?;
println!("{:?}", df.unwrap());

// Output:

// shape: (2, 3)
// ┌─────────┬─────┬────────┐
// │ Name    ┆ Age ┆ Height │
// │ ---     ┆ --- ┆ ---    │
// │ str     ┆ i32 ┆ f64    │
// ╞═════════╪═════╪════════╡
// │ Mahmoud ┆ 23  ┆ 1.84   │
// │ Ali     ┆ 27  ┆ 1.78   │
// └─────────┴─────┴────────┘
```

Polars DataFrame 的初始化过程非常简单，从其轻松实现中可以明显看出。此外，`[**df!**](https://docs.rs/polars/latest/polars/prelude/macro.df.html)` 宏使您能够轻松创建数据框架。以下是利用此宏的示例：

```py
let df: PolarsResult<DataFrame> = df!("Name" => &["Mahmoud", "Ali"],
                                      "Age" => &[23, 27],
                                      "Height" => &[1.84, 1.78]);
```

## 描述

Polars 中的 `[**describe**](https://docs.rs/polars/latest/polars/frame/struct.DataFrame.html#method.describe)` 方法是一个广泛使用的技术，为数据集提供**统计指标概述**。该方法创建了一个详尽的表格，包括每列的**计数**、**平均值**、**标准差**、**最小值**和**最大值**，以及**第 25** 到**第 75** 百分位数范围（**中位数**）。通过使用此方法，您可以获取有关数据特征的宝贵洞察，例如识别潜在的异常值并有效理解其分布模式。

```py
let df1: DataFrame = df!("categorical" => &["d","e","f"],
                         "numeric" => &[1, 2, 3],
                         "object" => &["a", "b", "c"]).unwrap();
println!("{}", df1);

// Output:

// shape: (3, 3)
// ┌─────────────┬─────────┬────────┐
// │ categorical ┆ numeric ┆ object │
// │ ---         ┆ ---     ┆ ---    │
// │ str         ┆ i32     ┆ str    │
// ╞═════════════╪═════════╪════════╡
// │ d           ┆ 1       ┆ a      │
// │ e           ┆ 2       ┆ b      │
// │ f           ┆ 3       ┆ c      │
// └─────────────┴─────────┴────────┘

let df2: DataFrame = df1.describe(None).unwrap();
println!("{}", df2);

// Output:

// shape: (9, 4)
// ┌────────────┬─────────────┬─────────┬────────┐
// │ describe   ┆ categorical ┆ numeric ┆ object │
// │ ---        ┆ ---         ┆ ---     ┆ ---    │
// │ str        ┆ str         ┆ f64     ┆ str    │
// ╞════════════╪═════════════╪═════════╪════════╡
// │ count      ┆ 3           ┆ 3.0     ┆ 3      │
// │ null_count ┆ 0           ┆ 0.0     ┆ 0      │
// │ mean       ┆ null        ┆ 2.0     ┆ null   │
// │ std        ┆ null        ┆ 1.0     ┆ null   │
// │ …          ┆ …           ┆ …       ┆ …      │
// │ 25%        ┆ null        ┆ 1.5     ┆ null   │
// │ 50%        ┆ null        ┆ 2.0     ┆ null   │
// │ 75%        ┆ null        ┆ 2.5     ┆ null   │
// │ max        ┆ f           ┆ 3.0     ┆ c      │
// └────────────┴─────────────┴─────────┴────────┘
```

## 头部

与 series 对象类似，`[**head**](https://docs.rs/polars/latest/polars/frame/struct.DataFrame.html#method.head)` 方法允许我们快速预览 DataFrame 对象的前几行。此方法节省时间和精力，因为它消除了滚动查看大量记录的需求，这可能会很繁琐和压倒性。当调用时，此函数根据用户定义的参数从原始数据集中返回包含 n 行的新 DataFrame。当传入`**None**`时，默认显示十（10）行。让我们考虑以下示例：

```py
let df: DataFrame = df!("Name" => &["Mahmoud", "Bob"],
                        "Age" => &[23, 27],
                        "Height" => &[1.84, 1.78]).unwrap();
println!("{}", df.head(None));

// Output:

// shape: (2, 3)
// ┌─────────┬─────┬────────┐
// │ Name    ┆ Age ┆ Height │
// │ ---     ┆ --- ┆ ---    │
// │ str     ┆ i32 ┆ f64    │
// ╞═════════╪═════╪════════╡
// │ Mahmoud ┆ 23  ┆ 1.84   │
// │ Bob     ┆ 27  ┆ 1.78   │
// └─────────┴─────┴────────┘
```

默认情况下，`**head**` 方法显示**前十行**，但可以通过其参数自定义显示任意数量。例如，`**df.head(Some(3))**` 将仅返回数据的前三行。此功能使我们能够在深入分析之前验证列名和内容，并提供内容概述。

## 尾部

就像 series 一样，Polars 中的 `[**tail**](https://docs.rs/polars/latest/polars/frame/struct.DataFrame.html#method.tail)` 函数是一个强大的方法，允许您预览任何 DataFrame 对象的**最后几行**。例如，如果您的 DataFrame 包含有关员工的信息，如姓名、年龄和身高；使用此方法可以快速验证列数据和结构。

```py
let df: DataFrame = df!("Name" => &["Mahmoud", "Bob"],
                        "Age" => &[23, 27],
                        "Height" => &[1.84, 1.78]).unwrap();
println!("{}", df.tail(None));

// Output:

// shape: (2, 3)
// ┌─────────┬─────┬────────┐
// │ Name    ┆ Age ┆ Height │
// │ ---     ┆ --- ┆ ---    │
// │ str     ┆ i32 ┆ f64    │
// ╞═════════╪═════╪════════╡
// │ Mahmoud ┆ 23  ┆ 1.84   │
// │ Bob     ┆ 27  ┆ 1.78   │
// └─────────┴─────┴────────┘
```

默认情况下，`**tail**` 方法显示数据集的最后十行，但可以通过指定参数来自定义显示行数。为了进一步说明：`**df.tail(Some(3))**` 将仅显示示例员工数据框的最后三行。

实质上，使用`**tail**`对DataFrames进行操作有助于在验证内容或获取其整体布局时节省时间。它提供了一目了然的信息，而无需手动逐行查看！

## 索引与切片

与系列不同，DataFrame对象可以使用方括号`**[]**`进行索引：

```py
// Create a sample DataFrame
let df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 29],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[50000, 60000, 250000]).unwrap();

// Indexing using brackets
// Select a single column by name

let name_col = &df["Name"];
// Or
let name_col1 = &df[0];
println!("{:?}", name_col);
println!("{:?}", name_col1);

// Select a single column by name

// Output:

// shape: (3,)
// Series: 'Name' [str]
// [
//     "Mahmoud"
//     "Ali"
//     "ThePrimeagen"
// ]

// Select multiple columns by slicing

subset = &df[..2];
println!("{:?}", subset);

// Output:

// [shape: (3,)
// Series: 'Name' [str]
// [
//     "Mahmoud"
//     "Ali"
//     "ThePrimeagen"
// ],shape: (3,)
// Series: 'Age' [i32]
// [
//     22
//     30
//     29
// ]]
```

在此示例中，我们构建了一个包含四列的DataFrame——“Name”、“Age”、“Gender”和“Salary”。然后，我们展示了使用方括号对数据框进行索引的各种技术。为了根据名称提取单列，我们使用了`**df[‘Name’]**`。该方法返回一个包含指定列所有值的Polars Series——在我们的例子中是‘Name’列。采用这种方法在需要从数据框中提取特定信息时非常有用。

随后，通过使用**df[..2]**进行切片，我们仅选择了某些列的子集，从而创建了**另一个** **新的** **DataFrame**，只包含**前两列**：即`**Name**`和`**Age**`。这种快速而高效的方法非常适合轻松选择数据框中的多个所需属性。同样，我们可以使用select方法选择列的子集，例如，调用`[**df.select([“Name”, “Age”])**](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html#method.select)`将仅返回`**Name**`和‘**Gender**’列。

```py
let name_age_cols = df.select(["Name", "Age"]).unwrap();
println!("{:?}", name_age_cols);

// Output:

// shape: (3, 2)
// ┌──────────────┬─────┐
// │ Name         ┆ Age │
// │ ---          ┆ --- │
// │ str          ┆ i32 │
// ╞══════════════╪═════╡
// │ Mahmoud      ┆ 22  │
// │ Ali          ┆ 25  │
// │ ThePrimeagen ┆ 29  │
// └──────────────┴─────┘
```

或者，我们也可以使用`[**column**](https://docs.rs/polars/latest/polars/frame/struct.DataFrame.html#method.column)`方法来检索特定的列，如下所示：

```py
let name_col = df.column("Name");
println!("{:?}", name_col);

// Output:

// shape: (3,)
// Series: 'Name' [str]
// [
//     "Mahmoud"
//     "Ali"
//     "ThePrimeagen"
// ]
```

你也可以使用**布尔索引**来选择行，这被称为**掩码**。考虑以下示例：

```py
// Create a sample DataFrame
let df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[50000, 60000, 250000]).unwrap();

let mask = df.column("Age").expect("Age must exist!").gt(25).unwrap();
let filtered_data = df.filter(&mask).unwrap();

println!("{:?}", filtered_data);

// Output:

// shape: (1, 4)
// ┌──────────────┬─────┬────────┬────────┐
// │ Name         ┆ Age ┆ Gender ┆ Salary │
// │ ---          ┆ --- ┆ ---    ┆ ---    │
// │ str          ┆ i32 ┆ str    ┆ i32    │
// ╞══════════════╪═════╪════════╪════════╡
// │ ThePrimeagen ┆ 36  ┆ M      ┆ 250000 │
// └──────────────┴─────┴────────┴────────┘
```

此外，`[**slice**](https://docs.rs/polars/latest/polars/frame/struct.DataFrame.html#method.slice)`方法允许我们从数据框对象中选择特定的行和列子集。例如，如果我们使用**df.slice(2,3)**，将从索引2开始返回三行（使用零基索引）。此外，此选择将包括所有列，从而产生一个完全由三行（如果存在）和四列组成的新数据框。

```py
println!("{:?}", df.slice(2, 3));

// Output:

// shape: (1, 4)
// ┌──────────────┬─────┬────────┬────────┐
// │ Name         ┆ Age ┆ Gender ┆ Salary │
// │ ---          ┆ --- ┆ ---    ┆ ---    │
// │ str          ┆ i32 ┆ str    ┆ i32    │
// ╞══════════════╪═════╪════════╪════════╡
// │ ThePrimeagen ┆ 36  ┆ M      ┆ 250000 │
// └──────────────┴─────┴────────┴────────┘
```

另一种选择是使用`[**transpose**](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html#method.transpose)`函数，该函数翻转矩阵的行和列。这使我们能够通过对其转置形式进行索引来访问单行作为系列。

```py
// Create a sample DataFrame
let df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[50000, 60000, 250000]).unwrap();

println!("{:?}", df.transpose().unwrap()[0]);

// Output:

// shape: (4,)
// Series: 'column_0' [str]
// [
//     "Mahmoud"
//     "22"
//     "M"
//     "50000"
// ]
```

请注意，如文档中所述，这是一项非常昂贵的操作。

## 数据清理

![](../Images/406554a8f993a79109604500e3583878.png)

照片由 [Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据清理的过程涉及一个关键步骤，即检测和解决缺失信息。空值的存在会显著影响分析或决策的精确性。幸运的是，Rust 的 Polars 库提供了许多强大的技术来有效地管理这些空白。

## 空值计数

计算给定数据帧中空值或缺失值的数量是创建另一个数据帧的重要步骤，该数据帧显示每列此类事件的计数。这些数据极其有用，因为它可以快速识别包含缺失信息的列，并量化缺失的数据量。基于这些信息，我们可以做出明智的决策，比如删除那些包含不完整条目的行，或采用插补方法来填补这些值。

```py
// Create a sample DataFrame
let df = df!("Name" => &[Some("Mahmoud"),  None, None],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[50000, 60000, 250000]).unwrap();

println!("{:?}", df.null_count());

// Output:

// shape: (1, 4)
// ┌──────┬─────┬────────┬────────┐
// │ Name ┆ Age ┆ Gender ┆ Salary │
// │ ---  ┆ --- ┆ ---    ┆ ---    │
// │ u32  ┆ u32 ┆ u32    ┆ u32    │
// ╞══════╪═════╪════════╪════════╡
// │ 2    ┆ 0   ┆ 0      ┆ 0      │
// └──────┴─────┴────────┴────────┘
```

## 重复项

使用此方法可以获得一个布尔掩码，指明数据帧中所有的重复行。这个掩码作为一个有效工具，可以过滤掉这些重复项，并精准地得到一个新的数据帧。要使用 `[**is_duplicated**](https://docs.rs/polars/latest/polars/frame/struct.DataFrame.html#method.is_duplicated)` 函数，在你的数据帧上调用它，并将结果掩码分配给一个新变量。随后，将相同的过滤器应用于你的原始数据帧，以消除这些副本。

```py
let df = df!("Name" => &["Mahmoud",  "Mahmoud", "ThePrimeagen"],
             "Age" => &[22, 22, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[50000, 50000, 250000]).unwrap();
let mask = df.is_duplicated().unwrap();
let filtered_data = df.filter(&mask).unwrap();
println!("{:?}", filtered_data);

// Output:

// shape: (2, 4)
// ┌─────────┬─────┬────────┬────────┐
// │ Name    ┆ Age ┆ Gender ┆ Salary │
// │ ---     ┆ --- ┆ ---    ┆ ---    │
// │ str     ┆ i32 ┆ str    ┆ i32    │
// ╞═════════╪═════╪════════╪════════╡
// │ Mahmoud ┆ 22  ┆ M      ┆ 50000  │
// │ Mahmoud ┆ 22  ┆ M      ┆ 50000  │
// └─────────┴─────┴────────┴────────┘
```

## 唯一值

`[**is_unique**](https://docs.rs/polars/latest/polars/frame/struct.DataFrame.html#method.is_unique)` 方法提供了一种确定数据帧中每一行是否包含唯一值的方法。这个方法使你能够获得数据集中所有独特行的掩码，这在处理大量数据或进行复杂操作时尤为有利。

要应用这个技术，只需在 **DataFrame** 对象上调用 `**is_unique**` 函数。它将生成一个布尔数组，突出显示那些包含唯一元素的行。然后，你可以利用这个数组作为过滤机制，高效地从原始 **DataFrame** 中提取唯一行。

```py
let df = df!("Name" => &["Mahmoud",  "Mahmoud", "ThePrimeagen"],
             "Age" => &[22, 22, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[50000, 50000, 250000]).unwrap();
let mask = df.is_unique().unwrap();
let filtered_data = df.filter(&mask).unwrap();
println!("{:?}", filtered_data);

// Output:

// shape: (1, 4)
// ┌──────────────┬─────┬────────┬────────┐
// │ Name         ┆ Age ┆ Gender ┆ Salary │
// │ ---          ┆ --- ┆ ---    ┆ ---    │
// │ str          ┆ i32 ┆ str    ┆ i32    │
// ╞══════════════╪═════╪════════╪════════╡
// │ ThePrimeagen ┆ 36  ┆ M      ┆ 250000 │
// └──────────────┴─────┴────────┴────────┘
```

## 删除

从数据帧或序列中删除不必要的信息在数据分析中至关重要。幸运的是，Polars 提供了多种有效的方法来做到这一点。其中一种方法是使用 `[**drop**](https://docs.rs/polars/latest/polars/frame/struct.DataFrame.html#method.drop)` 函数，该函数允许你删除特定的行或列。

要使用此方法，请指定目标列的名称/标签作为`**drop**`方法的参数。值得注意的是，默认情况下，此函数返回一个新的**DataFrame**对象，其中仅删除了指定的行——原始**DataFrame**保持不变。这对于期望初始数据集在运行特定函数后永久改变的初学者特别有帮助。例如，考虑一个基于**Fruit**和**Color**的**DataFrame**对象，其中列“**Color**”在进一步分析中不再需要：

```py
let df: DataFrame = df!("Fruit" => &["Apple", "Apple", "Pear"],
                        "Color" => &["Red", "Yellow", "Green"]).unwrap();
```

我们可以使用`**drop**`函数从此数据框中删除标签为“**Color**”的列：

```py
let df_remain = df.drop("Color").unwrap(); 
println!("{}", df_remain);

// Output:

// shape: (3, 1)
// ┌───────┐
// │ Fruit │
// │ ---   │
// │ str   │
// ╞═══════╡
// │ Apple │
// │ Apple │
// │ Pear  │
// └───────┘
```

现在，一个新的数据框对象**df_remain**持有与原始数据相同的数据，除了“**Color**”列。经过检查初始数据框，我们可以确认其信息保持不变。

```py
println!("{}", df); // the original DataFrame

// Output:

// shape: (3, 2)
// ┌───────┬────────┐
// │ Fruit ┆ Color  │
// │ ---   ┆ ---    │
// │ str   ┆ str    │
// ╞═══════╪════════╡
// │ Apple ┆ Red    │
// │ Apple ┆ Yellow │
// │ Pear  ┆ Green  │
// └───────┴────────┘
```

如果你希望直接对原始**DataFrame**进行更改，考虑使用`[**drop_in_place**](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html#method.drop_in_place)`函数，而不是`**drop**`。此方法的操作类似于`**drop**`，但它在不生成新对象的情况下修改数据框。

```py
let mut df: DataFrame = df!("Fruit" => &["Apple", "Apple", "Pear"],
                            "Color" => &["Red", "Yellow", "Green"]).unwrap();
df.drop_in_place("Color"); // remove the row with index 1 ("Color") from df
println!("{:?}", df);

// Output:

// shape: (3, 1)
// ┌───────┐
// │ Fruit │
// │ ---   │
// │ str   │
// ╞═══════╡
// │ Apple │
// │ Apple │
// │ Pear  │
// └───────┘
```

此外，你还可以通过指定列名作为`[**drop_many**](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html#method.drop_many)`函数的参数来删除多个列：

```py
let df_dropped_col = df.drop_many(&["Color", ""]);
println!("{:?}", df_dropped_col);

// Output:

// shape: (3, 1)
// ┌───────┐
// │ Fruit │
// │ ---   │
// │ str   │
// ╞═══════╡
// │ Apple │
// │ Apple │
// │ Pear  │
// └───────┘
```

最后，我们可以使用`[**drop_nulls**](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html#method.drop_nulls)`函数来删除包含空值或缺失值的任何行：

```py
let df: DataFrame = df!("Fruit" => &["Apple", "Apple", "Pear"],
                        "Color" => &[Some("Red"), None, None]).unwrap();
let df_clean = df.drop_nulls::<String>(None).unwrap();
println!("{:?}", df_clean);

// Output:

// shape: (1, 2)
// ┌───────┬───────┐
// │ Fruit ┆ Color │
// │ ---   ┆ ---   │
// │ str   ┆ str   │
// ╞═══════╪═══════╡
// │ Apple ┆ Red   │
// └───────┴───────┘
```

通过使用`[**is_not_null**](https://docs.rs/polars/latest/polars/prelude/fn.is_not_null.html)`方法，我们可以为**DataFrame**中的任何列创建一个非空掩码。此方法返回一个布尔掩码，用于区分包含空值和不包含空值的值。应用于特定列后，这会创建一个过滤器，其中每个值与其各自行的**null**或**not null**状态对应。通过使用这种有效的技术来提取仅符合特定标准的行，我们可以轻松地从新数据框中移除所有缺失数据。例如，要为**DataFrame**中的“**Salary**”列创建一个空值掩码，我们可以使用以下代码：

```py
let df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[Some(50000), Some(60000), None]).unwrap();
let mask = df.column("Salary").expect("Salary must exist!").is_not_null();
println!("{:?}", mask.head(None));

// Output:

// shape: (3,)
// ChunkedArray: 'Age' [bool]
// [
//     true
//     true
//     false
// ]
```

代码片段为**df** **DataFrame**对象的“**Salary**”列创建了一个空的掩码。它还显示了布尔掩码生成的一些初始值。此过滤器可以应用于仅提取“**Salary**”列中存在非空条目的行的数据。

```py
let filtered_data = df.filter(&mask).unwrap();
println!("{:?}", filtered_data);

// Output:

// shape: (2, 4)
// ┌─────────┬─────┬────────┬────────┐
// │ Name    ┆ Age ┆ Gender ┆ Salary │
// │ ---     ┆ --- ┆ ---    ┆ ---    │
// │ str     ┆ i32 ┆ str    ┆ i32    │
// ╞═════════╪═════╪════════╪════════╡
// │ Mahmoud ┆ 22  ┆ M      ┆ 50000  │
// │ Ali     ┆ 25  ┆ M      ┆ 60000  │
// └─────────┴─────┴────────┴────────┘
```

使用空值掩码可以在管理过滤过程时提供更高的精度。当我们希望基于不同列中的各种条件或空值组合进行过滤时，此方法尤其有用。然而，它需要编写比仅使用`**drop_nulls**`函数更多的代码，并且对于大型数据集可能不够高效。

总结这一部分，丢弃行是从 Polars 数据框中删除行或列的常见且方便的操作。修改原始数据或删除空值有几种选项。

## 填充

Polars 提供了处理缺失数据的有价值的方法——`[**fill_null**](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html#method.fill_null)` 方法。此函数允许我们用指定的方法或值替代**DataFrame**或**Series**对象中的空值或缺失值。`**fill_null**`的一个常见应用是用一个单一的值替换**DataFrame**或**Series**中的所有坏项。你可以通过将标量参数传递给`**fill_null**`来实现。例如，如果你想用前面的值替换 DataFrame 中的每个缺失项，只需使用如下所示的`**fill_null**`：

```py
let mut df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[Some(50000), Some(60000), None]).unwrap();

let filtered_nulls = df.fill_null(FillNullStrategy::Forward(None)).unwrap();

println!("{:?}", filtered_nulls);

// Output:

// shape: (3, 4)
// ┌──────────────┬─────┬────────┬────────┐
// │ Name         ┆ Age ┆ Gender ┆ Salary │
// │ ---          ┆ --- ┆ ---    ┆ ---    │
// │ str          ┆ i32 ┆ str    ┆ i32    │
// ╞══════════════╪═════╪════════╪════════╡
// │ Mahmoud      ┆ 22  ┆ M      ┆ 50000  │
// │ Ali          ┆ 25  ┆ M      ┆ 60000  │
// │ ThePrimeagen ┆ 36  ┆ M      ┆ 60000  │
// └──────────────┴─────┴────────┴────────┘
```

需要注意的是，统计函数在分析数据时通常会默认忽略**DataFrame**中的任何缺失值。然而，理解这些缺失的原因至关重要，因为它们可能会显著影响你的分析结果。此外，是否对这些缺失项进行填充或插补是否合适，取决于多种因素，如其出现的原因——故意的还是收集过程中的偶然错误等，以及进一步处理任务所需的准确性等。

本质上，在决定如何处理缺失数据之前，仔细考虑所有相关方面，将确保准确分析而不影响质量结果！

## 集中趋势的度量

## 均值

与 Series 类似，我们可以计算给定数据框中每个单独列的均值。

```py
let df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[Some(50000), Some(60000), None]).unwrap();

println!("{:?}", df.mean());

// Output:

// shape: (1, 4)
// ┌──────┬───────────┬────────┬─────────┐
// │ Name ┆ Age       ┆ Gender ┆ Salary  │
// │ ---  ┆ ---       ┆ ---    ┆ ---     │
// │ str  ┆ f64       ┆ str    ┆ f64     │
// ╞══════╪═══════════╪════════╪═════════╡
// │ null ┆ 27.666667 ┆ null   ┆ 55000.0 │
// └──────┴───────────┴────────┴─────────┘
```

## 中位数

我们还可以计算给定数据框中每个单独列的中位数。

```py
let df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[Some(50000), Some(60000), None]).unwrap();

println!("{:?}", df.median());

// Output:

// shape: (1, 4)
// ┌──────┬──────┬────────┬─────────┐
// │ Name ┆ Age  ┆ Gender ┆ Salary  │
// │ ---  ┆ ---  ┆ ---    ┆ ---     │
// │ str  ┆ f64  ┆ str    ┆ f64     │
// ╞══════╪══════╪════════╪═════════╡
// │ null ┆ 25.0 ┆ null   ┆ 55000.0 │
// └──────┴──────┴────────┴─────────┘
```

## 离散度量

## Std

```py
let df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[Some(50000), Some(60000), None]).unwrap();

println!("{:?}", df.std(1));

// Output:

// shape: (1, 4)
// ┌──────┬──────────┬────────┬─────────────┐
// │ Name ┆ Age      ┆ Gender ┆ Salary      │
// │ ---  ┆ ---      ┆ ---    ┆ ---         │
// │ str  ┆ f64      ┆ str    ┆ f64         │
// ╞══════╪══════════╪════════╪═════════════╡
// │ null ┆ 7.371115 ┆ null   ┆ 7071.067812 │
// └──────┴──────────┴────────┴─────────────┘
```

## 方差

```py
let df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[Some(50000), Some(60000), None]).unwrap();

println!("{:?}", df.var(1));

// Output:

// shape: (1, 4)
// ┌──────┬───────────┬────────┬────────┐
// │ Name ┆ Age       ┆ Gender ┆ Salary │
// │ ---  ┆ ---       ┆ ---    ┆ ---    │
// │ str  ┆ f64       ┆ str    ┆ f64    │
// ╞══════╪═══════════╪════════╪════════╡
// │ null ┆ 54.333333 ┆ null   ┆ 5e7    │
// └──────┴───────────┴────────┴────────┘
```

## Ndarray

如我们在[**第一篇文章**](https://wiseai.medium.com/rust-the-next-big-thing-in-data-science-319a03305883)中所见，你可以将数据框转换为 ndarray。这种方法从**DataFrame**对象创建一个二维`**ndarray::Array**`对象。要求 DataFrame 中的所有列都不能为空且为数字类型。它们将被强制转换为相同的数据类型（如果尚未）。它会隐式地将**None**转换为**NaN**，对于浮点数据不会失败。

```py
let df = df!("Name" => &["Mahmoud", "Ali", "ThePrimeagen"],
             "Age" => &[22, 25, 36],
             "Gender" => &["M", "M", "M"],
             "Salary" => &[Some(50000), Some(60000), None]).unwrap();

println!("{:?}", df.to_ndarray::<Float64Type>().unwrap());

// Output:

// [[NaN, 22.0, NaN, 50000.0],
//  [NaN, 25.0, NaN, 60000.0],
//  [NaN, 36.0, NaN, NaN]], shape=[3, 4], strides=[1, 3], layout=Ff (0xa), const ndim=2
```

现在，你可以对这个数组应用在上一篇文章中讨论的不同操作，标题为：[**终极 Ndarray 手册：掌握 Rust 科学计算的艺术**](/the-ultimate-ndarray-handbook-mastering-the-art-of-scientific-computing-with-rust-ef5ab767212a)。

## 聚合函数

![](../Images/2b919c5d4ef5da4f0a51f0cc20e9c348.png)

[Nicolas COMTE](https://unsplash.com/@rotor_?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

在处理大量数据时，分类和理解分组数据是至关重要的。幸运的是，Polars 通过其 `[**groupby**](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html#method.groupby)` 函数提供了一个出色的解决方案。该方法根据特定的键值将数据框拆分成多个块，然后应用计算，并将结果合并回另一个数据框中，这种模式被称为 [**split-apply-combine**](https://www.jstatsoft.org/article/view/v040i01)。

使用聚合函数，我们可以在这些组内快速执行**count**、**sum**或**mean**等各种操作；这在处理大型数据集时显著提高了计算速度和效率。其他常见的聚合函数包括**variance**和**std**。以下是`**.groupby**`使用的一些示例：

+   一家零售公司使用`**groupby**`方法按地区和产品类别分析销售数据。这种分析使他们能够确定哪些产品在哪些地区销售良好，并就库存管理和产品促销做出更明智的决策。

```py
let sales_revenue_df: DataFrame = sales_df.groupby(["Region", "Product_Category"]).expect("Columns must exist!").select(["Sales Revenue"]).sum().unwrap();
```

+   利用`**groupby**`方法，医疗组织可以根据年龄组和病情分析患者数据。

```py
let patient_by_age_condition_df: DataFrame = patients_data.groupby(["Age_Group", "Condition"]).expect("Columns must exist!").select(["Patient ID", "Length of Stay"]).count().unwrap();
```

+   运输企业可以利用`**groupby**`技术，根据司机和车辆类别分析其车辆的燃油使用情况。这种分析使他们能够发现燃油消耗中的低效环节，从而采取及时的纠正措施，提高燃油效率。

```py
let average_fuel_consumption_df: DataFrame = fuel_data.groupby(["Vehicle_Type", "Driver"]).expect("Columns must exist!").select(["Fuel Consumption"]).mean().unwrap();
```

+   通过利用`**groupby**`方法，保险公司可以有效地分析基于保单类型和客户人口统计数据的索赔数据。这种分析使他们能够识别高风险客户，同时制定符合其个人需求的保单。

```py
let claims_amount_df: DataFrame = claims_data.groupby(["Policy_Type", "Customer_Demographics"]).expect("Columns must exist!").select(["Claims Amount"]).sum().unwrap();
```

总之，`**.groupby**`方法是一个强大的数据分析工具，允许您以任何想象得到的方式对数据进行分组，并对每个组独立应用任何类型的函数，然后返回单一的数据集。

## 聚合示例

![](../Images/645783e9fe9a381fdaf1ab40cb839e39.png)

照片由 [Alexander Schimmeck](https://unsplash.com/@alschim?utm_source=medium&utm_medium=referral) 提供，拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

让我们深入研究一个在 Kaggle 上托管的 [**flights dataset**](https://www.kaggle.com/datasets/deepak007chaubey/flight-on-time-dataset?select=Flight_on_time_HIX.csv)，并进行一个基本的聚合操作，涉及一个分组列、一个聚合列和一个单独的聚合函数。我们的目标是确定每家航空公司的平均到达延误时间。Polars 提供了多种语法来创建这样的聚合，我们将在本节中探讨。

```py
use std::path::Path;
use polars::prelude::*;

fn read_data_frame_from_csv(
    csv_file_path: &Path,
) -> DataFrame {
    CsvReader::from_path(csv_file_path)
        .expect("Cannot open file.")
        .has_header(true)
        .finish()
        .unwrap()
}

let flights_file_path: &Path = Path::new("/path/to/Flight_on_time_HIX.csv");
let columns = ["Airline", "Origin_Airport", "Destination_Airport", "Departure_Delay_Minutes", "Arrival_Delay_Minutes"];
let flights_df: DataFrame = read_data_frame_from_csv(flights_file_path).select(columns).unwrap();
println!("{:?}", flights_df.head(Some(5)));

// Output:

// shape: (5, 5)
// ┌─────────┬────────────────┬─────────────────────┬─────────────────────────┬───────────────────────┐
// │ Airline ┆ Origin_Airport ┆ Destination_Airport ┆ Departure_Delay_Minutes ┆ Arrival_Delay_Minutes │
// │ ---     ┆ ---            ┆ ---                 ┆ ---                     ┆ ---                   │
// │ str     ┆ str            ┆ str                 ┆ i64                     ┆ i64                   │
// ╞═════════╪════════════════╪═════════════════════╪═════════════════════════╪═══════════════════════╡
// │ TR      ┆ IYF            ┆ HIX                 ┆ 62                      ┆ 52                    │
// │ TR      ┆ HEN            ┆ HIX                 ┆ 15                      ┆ 8                     │
// │ RO      ┆ HIX            ┆ IZN                 ┆ 0                       ┆ 0                     │
// │ XM      ┆ HIX            ┆ IZU                 ┆ 34                      ┆ 44                    │
// │ XM      ┆ HIX            ┆ LKF                 ┆ 144                     ┆ 146                   │
// └─────────┴────────────────┴─────────────────────┴─────────────────────────┴───────────────────────┘
```

为了有效地在 DataFrame 中对数据进行分组，重要的是定义分组列，如 `**Airline**`，并选择像 `**mean**` 这样的聚合函数来处理 `**Arrival_Delay_Minutes**` 列。一旦完成这些操作，只需将分组列放在 `**groupby**` 方法中，并选择你想要显示的列，然后对其应用聚合函数。这将生成一个新的 DataFrame。

```py
let arr_delay_mean_df: DataFrame = flights_df.groupby(["Airline"]).expect("Airline Column must exist!").select(["Arrival_Delay_Minutes"]).mean().unwrap();
println!("{:?}", arr_delay_mean_df.head(Some(5)));
// Output:

// shape: (5, 2)
// ┌─────────┬────────────────────────────┐
// │ Airline ┆ Arrival_Delay_Minutes_mean │
// │ ---     ┆ ---                        │
// │ str     ┆ f64                        │
// ╞═════════╪════════════════════════════╡
// │ UG      ┆ 34.374332                  │
// │ WC      ┆ 158.221406                 │
// │ TR      ┆ 281.309919                 │
// │ TO      ┆ 24.833333                  │
// │ YJ      ┆ 11.839243                  │
// └─────────┴────────────────────────────┘
```

通过多个列，可以实现 **分组** 和 **聚合**。然而，语法与单列操作有所不同。为了确保在任何类型的分组函数过程中顺利执行，重要的是识别三个关键组件：聚合函数、分组列和聚合列。例如；在这个例子中，我们正在计算 **按航空公司** 统计的每个 **起始机场** 的平均出发延迟。

```py
let dep_delay_mean_def: DataFrame = flights_df.groupby(["Airline", "Origin_Airport"]).expect("Airline and Origin_Airport Columns must exist!").select(["Departure_Delay_Minutes"]).mean().unwrap();
println!("{:?}", dep_delay_mean_def.head(Some(5)));
// Output:

// shape: (5, 3)
// ┌─────────┬────────────────┬──────────────────────────────┐
// │ Airline ┆ Origin_Airport ┆ Departure_Delay_Minutes_mean │
// │ ---     ┆ ---            ┆ ---                          │
// │ str     ┆ str            ┆ f64                          │
// ╞═════════╪════════════════╪══════════════════════════════╡
// │ TR      ┆ ERM            ┆ 9.7890625                    │
// │ NR      ┆ ULZ            ┆ 29.857143                    │
// │ RO      ┆ VYM            ┆ 10.722222                    │
// │ TJ      ┆ ERR            ┆ 20.290323                    │
// │ NR      ┆ XNL            ┆ 16.351064                    │
// └─────────┴────────────────┴──────────────────────────────┘
```

如果你熟悉 Python pandas，那么使用 `**groupby**` 会导致一个 [**MultiIndex**](https://pandas.pydata.org/docs/reference/api/pandas.MultiIndex.html#pandas.MultiIndex) 对象。**MultiIndexes** 的出现可以在索引和列中找到。然而，**Polars** 通过不要求开发者进行此类操作，完全消除了这一问题，使其成为数据处理方面比 Pandas 更具优势的替代方案。

## 合并 DataFrames

![](../Images/9bb2f0879cd6130877721f8c0c3ccc0e.png)

不同的 Polars 连接方法（图像作者提供）

Polars 提供了一系列数据操作工具，用于执行诸如合并数据集之类的任务。其中一个工具是 join 方法，它便于连接不同的 DataFrame 对象。要执行此操作，你需要在任意一个 DataFrame 上调用 [**join**](https://docs.rs/polars/latest/polars/prelude/trait.DataFrameJoinOps.html#method.join) 函数，并指定其他参数。为了更好地理解其实际操作，考虑以下代码示例。

```py
let df3: DataFrame = df1.join(other=&df2, left_on=["variable1"], right_on=["variable2"], how=JoinType::Inner, suffix=None).unwrap();
```

在执行合并操作时，`**how**` 参数在决定将进行何种类型的合并中起着重要作用。你有几种选项可供选择，包括内连接、左连接、右连接和外连接。

为了确定将作为每个 DataFrame 连接键的确切变量，按需使用 `**left_on**` 和 `**right_on**` 参数。这些具体值使得将两个 DataFrame 中的对应行轻松连接起来。

如果有另一个数据框需要与第一个数据框合并，只需使用 `**other**` 参数来指示这个第二个数据集。根据你希望如何组织数据，这些新信息可以被添加到现有数据集的顶部或追加到其上！

最后，如果两个列在被合并的不同数据集中具有相同的名称，那么使用后缀可以通过在各自列标题的末尾附加唯一字符串来轻松区分它们。以下是如何使用 `**join**` 函数的示例：

```py
let df1: DataFrame = df!("Carrier" => &["HA", "EV", "VX", "DL"],
                         "ArrDelay" => &[-3, 28, 0, 1]).unwrap();
let df2: DataFrame = df!("Airline" => &["HA", "EV", "OO", "VX"],
                         "DepDelay" => &[21, -8, 11, -4]).unwrap();

let df3: DataFrame = df1.join(&df2, ["Carrier"], ["Airline"], JoinType::Inner, None).unwrap();
// or: let df3: DataFrame = df1.inner_join(&df2, ["Carrier"], ["Airline"]).unwrap();
println!("{:?}", df3.head(Some(5)));

// Output:

// shape: (3, 3)
// ┌─────────┬──────────┬──────────┐
// │ Carrier ┆ ArrDelay ┆ DepDelay │
// │ ---     ┆ ---      ┆ ---      │
// │ str     ┆ i32      ┆ i32      │
// ╞═════════╪══════════╪══════════╡
// │ HA      ┆ -3       ┆ 21       │
// │ EV      ┆ 28       ┆ -8       │
// │ VX      ┆ 0        ┆ -4       │
// └─────────┴──────────┴──────────┘
```

在 Polars 中合并 DataFrames 时，了解不同类型的连接是很重要的。**内连接** 仅保留两个 DataFrames 中 **共同** 的行，而 **左连接** 和 **右连接** 则保留一个 DataFrame 中的 **所有** 行，并根据匹配的值从另一个 DataFrame 添加相关数据。

```py
let df1: DataFrame = df!("Carrier" => &["HA", "EV", "VX", "DL"],
                         "ArrDelay" => &[-3, 28, 0, 1]).unwrap();
let df2: DataFrame = df!("Airline" => &["HA", "EV", "OO", "VX"],
                         "DepDelay" => &[21, -8, 11, -4]).unwrap();

// left join
let df3: DataFrame = df1.left_join(&df2, ["Carrier"], ["Airline"]).unwrap();
println!("{:?}", df3.head(Some(5)));

// Right join
let df4: DataFrame = df2.left_join(&df1, ["Airline"], ["Carrier"]).unwrap();
println!("{:?}", df4.head(Some(5)));

let df5: DataFrame = df1.outer_join(&df2, ["Carrier"], ["Airline"]).unwrap();
println!("{:?}", df5.head(Some(5)));

// Output:

// Left Join
// shape: (4, 3)
// ┌─────────┬──────────┬──────────┐
// │ Carrier ┆ ArrDelay ┆ DepDelay │
// │ ---     ┆ ---      ┆ ---      │
// │ str     ┆ i32      ┆ i32      │
// ╞═════════╪══════════╪══════════╡
// │ HA      ┆ -3       ┆ 21       │
// │ EV      ┆ 28       ┆ -8       │
// │ VX      ┆ 0        ┆ -4       │
// │ DL      ┆ 1        ┆ null     │
// └─────────┴──────────┴──────────┘

// Right Join
// shape: (4, 3)
// ┌─────────┬──────────┬──────────┐
// │ Airline ┆ DepDelay ┆ ArrDelay │
// │ ---     ┆ ---      ┆ ---      │
// │ str     ┆ i32      ┆ i32      │
// ╞═════════╪══════════╪══════════╡
// │ HA      ┆ 21       ┆ -3       │
// │ EV      ┆ -8       ┆ 28       │
// │ OO      ┆ 11       ┆ null     │
// │ VX      ┆ -4       ┆ 0        │
// └─────────┴──────────┴──────────┘

// Outer Join
// shape: (5, 3)
// ┌─────────┬──────────┬──────────┐
// │ Carrier ┆ ArrDelay ┆ DepDelay │
// │ ---     ┆ ---      ┆ ---      │
// │ str     ┆ i32      ┆ i32      │
// ╞═════════╪══════════╪══════════╡
// │ HA      ┆ -3       ┆ 21       │
// │ EV      ┆ 28       ┆ -8       │
// │ OO      ┆ null     ┆ 11       │
// │ VX      ┆ 0        ┆ -4       │
// │ DL      ┆ 1        ┆ null     │
// └─────────┴──────────┴──────────┘
```

如果某个 DataFrame 中的行没有对应的数据，则会相应地填充空值或缺失值。右连接的工作方式类似，但保留的是右侧 DataFrame 中的所有行。

合并 DataFrames 可以极大地提高你用 Polars 的 **join** 方法有效分析数据的能力。

## 结论

![](../Images/baa60e628868134899f091005f89931b.png)

图片由 [Adeolu Eletu](https://unsplash.com/@adeolueletu?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文让你熟悉了 Polars 中的一个基础数据结构 —— **DataFrame**。此外，我们还探讨了 Polars 中查询、修改和连接数据框的基本概念。因此，这应该能让你在处理 DataFrames 时更有信心。它将成为本系列文章中一个重要的组成部分。

在本文中，我们讨论了以下主题：

+   Polars 的 DataFrame 对象。

+   探索 Polars 中不同的聚合函数。

+   如何在 Polars 中合并 DataFrames 以及这与 Pandas 有何不同。

还有更多。在接下来的文章中，你对 Polars 的知识将扩展到更高级的功能和技巧。通过掌握这个库，你将获得坚实而有价值的技能，使你能够轻松处理复杂的数据分析任务，同时轻松处理大量数据集。

## 结束语

![](../Images/c732ae653e11e63222cc5fd9bc49b0e4.png)

图片由 [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

当我们结束本教程时，我想向所有投入时间和精力完成它的人员表示诚挚的感谢。与您一起展示 Rust 编程语言的非凡能力，真是我的荣幸。

一如既往，作为对数据科学充满热情的我，承诺从现在开始，每周至少会撰写一篇关于相关主题的综合文章。如果你对我工作的最新动态感兴趣，可以考虑在各种社交媒体平台上与我联系，或者直接联系我，如果有其他需要帮助的地方。

感谢！

## 资源

[](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----7c58a3cb7a1f--------------------------------) [## GitHub - wiseaidev/rust-data-analysis: 终极 Rust 数据分析课程]

### 这个代码库包含了一系列 Jupyter 笔记本，所有笔记本都由 Rust 内核驱动。通过这些笔记本，你将会...

[github.com](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----7c58a3cb7a1f--------------------------------) [## polars::prelude 中的DataFrame - Rust

### 一组长度相同的`Series`的连续可增长集合。

[docs.rs](https://docs.rs/polars/latest/polars/prelude/struct.DataFrame.html?source=post_page-----7c58a3cb7a1f--------------------------------) [## Rust By Example

### Rust是一种现代系统编程语言，专注于安全性、速度和并发性。它实现这些目标的方式…

[文档](https://doc.rust-lang.org/rust-by-example/?source=post_page-----7c58a3cb7a1f--------------------------------) [## Rust编程语言

### 由Steve Klabnik和Carol Nichols编写，Rust社区贡献者参与。本版本的文本假设您…

[文档](https://doc.rust-lang.org/book/?source=post_page-----7c58a3cb7a1f--------------------------------)
