# 你的 Rust 代码的 SIMD 加速九大规则（第2部分）

> 原文：[https://towardsdatascience.com/nine-rules-for-simd-acceleration-of-your-rust-code-part-2-6a104b3be6f3?source=collection_archive---------7-----------------------#2023-12-15](https://towardsdatascience.com/nine-rules-for-simd-acceleration-of-your-rust-code-part-2-6a104b3be6f3?source=collection_archive---------7-----------------------#2023-12-15)

## 从将 `range-set-blaze` Crate 数据摄取速度提升 7 倍的经验中得到的一般性教训

[](https://medium.com/@carlmkadie?source=post_page-----6a104b3be6f3--------------------------------)[![Carl M. Kadie](../Images/9dbe27c76e9567136e5a7dc587f1fb15.png)](https://medium.com/@carlmkadie?source=post_page-----6a104b3be6f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6a104b3be6f3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6a104b3be6f3--------------------------------) [Carl M. Kadie](https://medium.com/@carlmkadie?source=post_page-----6a104b3be6f3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5e87027005f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-simd-acceleration-of-your-rust-code-part-2-6a104b3be6f3&user=Carl+M.+Kadie&userId=a5e87027005f&source=post_page-a5e87027005f----6a104b3be6f3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6a104b3be6f3--------------------------------) ·9 分钟阅读·2023年12月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6a104b3be6f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-simd-acceleration-of-your-rust-code-part-2-6a104b3be6f3&user=Carl+M.+Kadie&userId=a5e87027005f&source=-----6a104b3be6f3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6a104b3be6f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-simd-acceleration-of-your-rust-code-part-2-6a104b3be6f3&source=-----6a104b3be6f3---------------------bookmark_footer-----------)![](../Images/7c6265c7eac5b0a502b4d8cecf7e15f3.png)

一只螃蟹将计算任务委托给小螃蟹——来源：[https://openai.com/dall-e-2/](https://openai.com/dall-e-2/)。所有其他图形来自作者。

> 感谢 Ben Lichtman (B3NNY) 在 Seattle Rust Meetup 上为我指引了正确的 SIMD 方向。

这是关于在 Rust 中创建 SIMD 代码的文章的第2部分。（参见 [第1部分](https://medium.com/towards-data-science/nine-rules-for-simd-acceleration-of-your-rust-code-part-1-c16fe639ce21)。）我们将查看第7到第9条规则：

+   7\. 使用 Criterion 基准测试来选择算法，并发现通道数（LANES）应该（几乎总是）为 32 或 64。

+   8\. 将你最佳的 SIMD 算法集成到你的项目中，使用 `as_simd`、专门的 `i128`/`u128` 代码和额外的上下文基准测试。

+   9\. 使用可选的 cargo 特性将你最佳的 SIMD 算法从项目中抽取出来（暂时）。

回顾规则 1 到 6：

1.  使用 nightly Rust 和 `core::simd`，这是 Rust 的实验性标准 SIMD 模块。

1.  CCC：检查、控制和选择你计算机的 SIMD 能力。

1.  学习 `core::simd`，但要有选择地学习。

1.  头脑风暴候选算法。

1.  使用 Godbolt 和 AI 来理解你代码的汇编，即使你不知道汇编语言。

1.  通过内联泛型（如果这不起作用）宏（如果这不起作用）特质来通用化到所有类型和通道数。

这些规则基于我尝试加速 `[range-set-blaze](https://crates.io/crates/range-set-blaze)` 的经验，这是一个用于操作“密集”整数集合的 Rust crate。

请回忆规则 6，来自于 [第 1 部分](https://medium.com/towards-data-science/nine-rules-for-simd-acceleration-of-your-rust-code-part-1-c16fe639ce21)，展示了如何使 Rust SIMD 算法在类型和通道数上完全通用。接下来我们需要选择算法并设置通道数。

# 规则 7：使用 Criterion 基准测试来选择算法，并发现通道数（LANES）应该（几乎总是）为 32 或 64。

在这一规则中，我们将看到如何使用流行的 [criterion](https://docs.rs/criterion/latest/criterion/) crate 来基准测试和评估我们的算法及选项。在 `range-set-blaze` 的上下文中，我们将进行评估：

+   5 种算法 — Regular、Splat0、Splat1、Splat2、Rotate

+   3 种 SIMD 扩展级别 — `sse2`（128 位）、`avx2`（256 位）、`avx512f`（512 位）

+   10 种元素类型 — `i8`、`u8`、`i16`、`u16`、`i32`、`u32`、`i64`、`u64`、`isize`、`usize`

+   5 种通道数量 — 4、8、16、32、64

+   4 种输入长度 — 1024；10,240；102,400；1,024,000

+   2 个 CPU — AMD 7950X 配有 `avx512f`，Intel i5–8250U 配有 `avx2`

基准测试测量每种组合的平均运行时间。然后，我们计算每秒传输的兆字节（Mbytes/sec）。

请参阅这篇 [新伴随文章](https://medium.com/towards-data-science/benchmarking-rust-compiler-settings-with-criterion-62db50cd62fb)，了解如何开始使用 Criterion。那篇文章还展示了如何推动（滥用？）Criterion 来测量编译器设置的效果，例如 SIMD 扩展级别。

运行基准测试会生成一个 5000 行的 `*.csv` 文件，内容如下：

```py
Group,Id,Parameter,Mean(ns),StdErr(ns)
vector,regular,avx2,256,i16,16,16,1024,291.47,0.080141
vector,regular,avx2,256,i16,16,16,10240,2821.6,3.3949
vector,regular,avx2,256,i16,16,16,102400,28224,7.8341
vector,regular,avx2,256,i16,16,16,1024000,287220,67.067
vector,regular,avx2,256,i16,16,32,1024,285.89,0.59509
...
```

此文件适合通过 [电子表格数据透视表](https://support.microsoft.com/en-us/office/create-a-pivotchart-c1b1e057-6990-4c38-b52b-8255538e7b1c) 或数据框工具如 [Polars](https://pola-rs.github.io/polars/user-guide/transformations/pivot/) 进行分析。

## 算法和通道

这里是一个 Excel 数据透视表，显示 — 对于每种算法 — 吞吐量（MBytes/sec）与 SIMD 通道数的关系。该表格对 SIMD 扩展级别、元素类型和输入长度进行了吞吐量的平均计算。

在我的 AMD 桌面机器上：

![](../Images/b595995d889d6a23cec98a2ee457591b.png)

在一台Intel笔记本电脑上：

![](../Images/452fad0ef42294b0bd627b3fcd3f3457.png)

表格显示Splat1和Splat2表现最佳。它们还显示更多的lanes始终在32或64之间更好。

> 例如，`*sse2*`（128位宽）如何处理64条`*i64*`（4096位宽）的数据？Rust的`*core::simd*`模块通过自动而高效地将4096位分成32个128位块，使这一切成为可能。将32个128位块一起处理（显然）能够进行超越独立处理128位块的优化。

**SIMD扩展级别**

让我们将LANES设置为64，并比较AMD机器上不同的SIMD扩展级别。表格展示了各元素类型和输入长度下的平均吞吐量。

![](../Images/36e4721f810d3f192895ebc927c8ac2b.png)

在我的AMD机器上，当使用64条lanes时，`sse2`最慢。比较`avx2`和`avx512f`时，结果各有不同。再一次，算法Splat1和Splat2表现最好。

## 元素类型

接下来，我们将SIMD扩展级别设置为`avx512f`并比较不同的元素类型。我们将`LANES`保持在64，并对输入长度进行平均吞吐量测试。

![](../Images/09c769e64b33873ba8ecf223b6c45005.png)

我们看到按位、32位和64位元素的处理速度最快。（然而，按元素来说，更小的类型更快。）Splat1和Splat2是最快的算法，其中Splat1表现略好。

## 输入长度

最后，让我们将元素类型设置为`i32`，看看输入长度与吞吐量的关系。

![](../Images/4d4945fd8fb6a7d6c7bcd9222857b69d.png)

我们看到所有的SIMD算法在100万个输入下表现差不多。Splat1显然在短输入上表现优于其他算法。

看起来更短的处理速度比更长的更快。这可能是缓存的结果，或者可能是基准测试丢弃未对齐数据的结果。

## 基准测试结论

基于这些基准测试，我们将使用Splat1算法。现在，我们将LANES设置为32或64，但请参见下一条规则以了解复杂情况。最后，我们建议用户将其SIMD扩展级别设置为至少`avx2`。

# 规则8：将您最好的SIMD算法集成到您的项目中，使用`as_simd`，为`i128`/`u128`编写特殊代码，并进行额外的上下文基准测试。

## `as_simd`

在添加SIMD支持之前，`RangeSetBlaze`的主要构造函数是`from_iter`：

```py
let a = RangeSetBlaze::from_iter([1, 2, 3]);
```

然而，SIMD操作在数组上效果最佳，而不是在迭代器上。此外，从数组构造`RangeSetBlaze`通常是一个自然的操作，因此我添加了一个新的`from_slice`构造函数：

```py
#[inline]
    pub fn from_slice(slice: impl AsRef<[T]>) -> Self {
        T::from_slice(slice)
    }
```

新构造函数对每个整数的`from_slice`方法进行内联调用。对于所有整数类型，除了`i128`/`u128`，接下来的调用是：

```py
let (prefix, middle, suffix) = slice.as_simd();
```

Rust的夜间版`as_simd`方法安全快速地将切片转换为：

1.  一个未对齐的`prefix`——我们像之前一样用`from_iter`处理它。

1.  `middle`，一个对齐的`Simd`结构块数组

1.  一个未对齐的`suffix`——我们像之前一样用`from_iter`处理它。

将`middle`视为将我们的输入整数分成大小为16的块（或者`LANES`设置的任何大小）。然后，我们通过`is_consecutive`函数迭代这些块，寻找`true`的连续段。每个连续段成为一个单一的范围。例如，从1000到1159（含）的160个连续整数将被识别并替换为一个Rust `RangeInclusive` `1000..=1159`。然后，这个范围由`from_iter`处理，比`from_iter`处理160个单独整数要快得多。当`is_consecutive`返回`false`时，我们退回到使用`from_iter`处理块中的单独整数。

## `i128`/`u128`

我们如何处理`core::simd`不处理的类型数组，即`i128`/`u128`？目前，我只是用较慢的`from_iter`来处理它们。

## 上下文基准测试

作为最后一步，在你的主要代码的上下文中对SIMD代码进行基准测试，理想情况下使用具有代表性的数据。

`range-set-blaze`库已经包括了[基准测试](https://github.com/CarlKCarlK/range-set-blaze/blob/main/docs/bench.md)。一个基准测试测量了在不同块大小下处理1,000,000个整数的性能。平均块大小范围从1（无块）到100,000块。让我们在`LANES`设置为4、8、16、32和64时运行该基准测试。我们将使用算法Splat1和SIMD扩展级别`avx512f`。

对于每种块大小，条形图显示了处理1,000,000个整数的相对速度。对于每种块大小，最快的`LANES`设置为100%。

![](../Images/074b373e7a1c5bb4cda1d0ebb52c46e8.png)

我们看到，对于大小为10和100的块，`LANES`=4是最好的。然而，对于大小为100,000的块，`LANES`=4的表现比最佳值差4倍。在另一个极端，`LANES`=64在大小为100,000的块上表现良好，但在大小为100和1000的块上分别比最佳值差1.8倍和1.5倍。

我决定将`LANES`设置为16。它对于大小为1000的块是最好的。此外，它的表现从未比最佳值差超过1.25倍。

使用此设置，我们可以运行其他基准测试。下图显示了各种范围集库（包括`range-set-blaze`）在相同任务上工作的情况——处理1,000,000个具有不同块大小的整数。`y`轴是毫秒，值越低越好。

![](../Images/f21111be1939935d2862b1b6c41bb6b4.png)

对于大小为1000的块，现有的`RangeSetBlaze::into_iter`方法（红色）已经比HashSet（橙色）快30倍。注意，刻度是对数的。使用`avx512f`，新的SIMD加速的`RangeSetBlaze::into_slice`算法（浅蓝色）比HashSet快230倍。使用`sse2`（深蓝色），快220倍。使用`avx2`（黄色），快180倍。在这个基准测试中，与`RangeSetBlaze::into_iter`相比，`avx512f`的`RangeSetBlaze::into_slice`快7倍。

我们还应该考虑最坏的情况，即处理没有聚集的数据。[我进行了基准测试](https://github.com/CarlKCarlK/range-set-blaze/blob/dec23/docs/bench.md)。结果显示，现有的`RangeSetBlaze::into_iter`比HashSet慢约2.2倍。新的`RangeSetBlaze::into_slice`比HashSet慢2.4倍。

所以，总的来说，新SIMD代码为假定数据有聚集的情况提供了巨大的优势。如果假设是错误的，它会更慢，但不会灾难性地慢。

由于SIMD代码集成到我们的项目中，我们准备好发布了，对吗？可惜不是。因为我们的代码依赖于Rust夜间版本，我们应该使其可选。我们将在下一条规则中看到如何做到这一点。

# 规则9：暂时将你最佳的SIMD算法从项目中分离（使用可选的cargo功能）。

我们美丽的新SIMD代码依赖于Rust夜间版本，它会发生变化。要求用户依赖Rust夜间版本是不合理的。（另外，当事情出错时收到抱怨也会很烦人。）解决方案是将SIMD代码隐藏在cargo功能之后。

> 功能、功能、功能——在处理SIMD和Rust的上下文中，“功能”一词有三种不同的含义。首先，“CPU/target features”——这些描述了CPU的能力，包括它支持的SIMD扩展。见`[*target-feature*](https://doc.rust-lang.org/std/arch/index.html)` [和](https://doc.rust-lang.org/std/arch/index.html) `[*is_x86_feature_detected!*](https://doc.rust-lang.org/std/arch/index.html)`。第二，“nightly feature gates”——Rust通过[功能开关](https://prev.rust-lang.org/en-US/faq.html#what-are-feature-gates)控制Rust夜间版本中新语言功能的可见性。例如，`[*#![feature(portable_simd)]*](https://github.com/rust-lang/portable-simd)`。第三，“[cargo features](https://doc.rust-lang.org/cargo/reference/features.html)”——这些允许任何Rust crate或库提供/限制其部分功能的访问。你在`*Cargo.toml*`中看到这些，例如，当你添加对`*itertools/use_std*`的依赖时。

以下是`range-set-blaze` crate采取的步骤，以使夜间依赖的SIMD代码可选：

+   在`Cargo.toml`中，定义一个与SIMD代码相关的cargo功能：

```py
[features]
from_slice = []
```

+   在顶部的`lib.rs`文件中，设置夜间`portable_simd`功能开关，依赖于`from_slice`的cargo功能：

```py
#![cfg_attr(feature = "from_slice", feature(portable_simd))]
```

+   使用条件编译属性，例如`#[cfg(feature = “from_slice”)]`，来选择性地包含SIMD代码。这包括测试。

```py
/// Creates a [`RangeSetBlaze`] from a collection of integers. It is typically many
/// times faster than [`from_iter`][1]/[`collect`][1].
/// On a representative benchmark, the speed up was 6×.
///
/// **Warning: Requires the nightly compiler. Also, you must enable the `from_slice`
/// feature in your `Cargo.toml`. For example, with the command:**
/// ```bash

/// cargo add range-set-blaze --features "from_slice"

/// ```py
///
/// **Caution**: Compiling with `-C target-cpu=native` optimizes the binary for your current CPU architecture,
/// which may lead to compatibility issues on other machines with different architectures.
/// This is particularly important for distributing the binary or running it in varied environments.
/// [1]: struct.RangeSetBlaze.html#impl-FromIterator<T>-for-RangeSetBlaze<T>
#[cfg(feature = "from_slice")]
#[inline]
pub fn from_slice(slice: impl AsRef<[T]>) -> Self {
    T::from_slice(slice)
}
```

+   如上文档所示，向文档中添加警告和注意事项。

+   使用`--features from_slice`来检查或测试你的SIMD代码。

```py
cargo check --features from_slice
cargo test --features from_slice
```

+   使用`--all-features`运行所有测试，生成所有文档，并发布所有cargo功能：

```py
cargo test --all-features --doc
cargo doc --no-deps --all-features --open
cargo publish --all-features --dry-run
```

# 结论

所以，这就是了：向 Rust 代码中添加 SIMD 操作的九条规则。这个过程的简便性反映了`core::simd`库的卓越设计。你应该在适用的地方总是使用 SIMD 吗？最终，应该的，当库从 Rust nightly 版本转到稳定版时。目前，在 SIMD 的性能优势至关重要的地方使用它，或者让它的使用成为可选。

有关提升 Rust 中 SIMD 体验的想法？`core::simd` 的质量已经很高，主要需要的是将其稳定化。

感谢你和我一起探讨 SIMD 编程。我希望如果你有一个适合 SIMD 的问题，这些步骤能帮助你加速处理。

*请* [*关注 Carl 的 Medium 文章*](https://medium.com/@carlmkadie)*。我在 Rust 和 Python 的科学编程、机器学习以及统计学方面写作。我倾向于每月撰写一篇文章。*
