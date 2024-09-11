# 在 Rust 中创建快速、安全和兼容的数据结构的九条规则（第 2 部分）

> 原文：[https://towardsdatascience.com/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-2-da5e6961a0b7?source=collection_archive---------8-----------------------#2023-04-12](https://towardsdatascience.com/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-2-da5e6961a0b7?source=collection_archive---------8-----------------------#2023-04-12)

## 来自 RangeSetBlaze 的经验教训

[](https://medium.com/@carlmkadie?source=post_page-----da5e6961a0b7--------------------------------)[![Carl M. Kadie](../Images/9dbe27c76e9567136e5a7dc587f1fb15.png)](https://medium.com/@carlmkadie?source=post_page-----da5e6961a0b7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----da5e6961a0b7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----da5e6961a0b7--------------------------------) [Carl M. Kadie](https://medium.com/@carlmkadie?source=post_page-----da5e6961a0b7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5e87027005f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-2-da5e6961a0b7&user=Carl+M.+Kadie&userId=a5e87027005f&source=post_page-a5e87027005f----da5e6961a0b7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----da5e6961a0b7--------------------------------) ·14 min read·Apr 12, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fda5e6961a0b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-2-da5e6961a0b7&user=Carl+M.+Kadie&userId=a5e87027005f&source=-----da5e6961a0b7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fda5e6961a0b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-2-da5e6961a0b7&source=-----da5e6961a0b7---------------------bookmark_footer-----------)![](../Images/f0769b1a5bf56fdbbe9a841255f915a1.png)

将数字存储在树中 — 来源: [必应图像创作者](https://www.bing.com/create)

这是关于在 Rust 中创建数据结构的文章第 2 部分。我们将探讨第 6 到第 9 条规则：

+   6\. 定义操作符和快速操作。

+   7\. 遵循“九条良好 API 设计规则”，尤其是“编写良好的文档”。

+   8\. 使用代表性数据、Criterion 基准测试和分析工具来优化性能。

+   9\. 测试覆盖率、文档、特性、编译器错误和正确性。

查看[第1部分](https://medium.com/towards-data-science/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-1-c0973092e0a3)以获取规则1至5。

1.  抄袭你的API、文档，甚至代码——来自标准库。

1.  设计易于使用、兼容且高效的构造函数。

1.  创建比预期更多的Rust迭代器。

1.  使用特性使非法值无法表示。

1.  定义具有保证属性和有用方法的泛型迭代器。

这些规则是根据我创建的新范围集合crate `[range-set-blaze](https://crates.io/crates/range-set-blaze)`的经验得出的。范围集合是一种数据结构，用于表示整数等集合，形式为排序且不重叠的范围。例如，`0..=5, 10..=10`。与其他范围集合crate相比，`range-set-blaze`提供了完整的集合操作，并且性能优越。

让我们首先看看这个crate如何提供集合操作。

# 规则6：定义操作符和快速操作

标准的`BTreeSet`提供了集合操作符，例如，如果`a`和`b`都是`BTreeSet`，那么`a | b`将是它们并集的`BTreeSet`。决定操作符是否适合你的数据结构。操作符可以是逻辑操作符，例如`|`、`&`、`!`，和/或算术操作符，例如`+`、`-`、`*`、`/`、`^`。[这个表格](https://doc.rust-lang.org/book/appendix-02-operators.html)列出了Rust的“可重载”操作符及其方法名称。

`range-set-blaze` crate在`RangeSetBlaze`结构上提供了与集合相关的操作符：

```py
use range_set_blaze::RangeSetBlaze;
let a = RangeSetBlaze::from_iter([1..=4]);
let b = RangeSetBlaze::from_iter([0..=0, 3..=5, 10..=10]);
let union = a | b;
assert_eq!(union, RangeSetBlaze::from_iter([0..=5, 10..=10]));
```

以及任何实现了`SortedDisjoint`特性的迭代器（见[第1部分](https://medium.com/towards-data-science/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-1-c0973092e0a3)中的规则5）：

```py
use range_set_blaze::prelude::*;

let a = CheckSortedDisjoint::new(vec![1..=2, 5..=100].into_iter());
let b = CheckSortedDisjoint::from([2..=6]);
let union = a | b;
assert_eq!(union.to_string(), "1..=100");
```

`BitOr`是Rust中`|`操作符的方法名称（再次见[表格](https://doc.rust-lang.org/book/appendix-02-operators.html)）。下面是`RangeSetBlaze`上并集操作符的定义：

```py
impl<T: Integer> BitOr<RangeSetBlaze<T>> for RangeSetBlaze<T> {
    type Output = RangeSetBlaze<T>;
    fn bitor(mut self, other: Self) -> RangeSetBlaze<T> {
      // code omitted for now
    }
}
```

这意味着对于所有的`RangeSetBlaze`，我们定义了一个`|`操作符，它接受一个`RangeSetBlaze`作为第二个输入，并返回一个`RangeSetBlaze`。

**子规则 #1：支持借用输入**：上述定义允许用户进行`a | b`操作，但不支持`&a | &b`、`&a | b`或`a | &b`。这些“借用组合”需要三个额外的定义，如下所示：

```py
impl<T: Integer> BitOr<&RangeSetBlaze<T>> for &RangeSetBlaze<T> {...}
impl<T: Integer> BitOr<RangeSetBlaze<T>> for &RangeSetBlaze<T> {...}
impl<T: Integer> BitOr<&RangeSetBlaze<T>> for RangeSetBlaze<T> {...}
```

或者，你可以使用`[gen_ops](https://crates.io/crates/gen_ops)` crate。它允许你在一个声明中定义多个操作符的所有借用组合。

**子规则 #2：在特性上提供操作符（类似）：** 与集合相关的方法，如`complement`和`union`，可以在我们的泛型`SortedDisjoint`迭代器上高效地定义。这些方法在一次遍历中运行，并且需要的内存非常少。（在[第1部分](https://medium.com/towards-data-science/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-1-c0973092e0a3)的规则5中，我们看到了`complement`的定义。）然而，我们希望使用`!a`来代替`a.complement()`。可以吗？可以，也不可以。

不，你不能在`SortedDisjoint`上实现特征`ops::Not`（`!`运算符）——[参见互联网讨论](https://stackoverflow.com/questions/61108590/default-implementation-of-supertrait)。（你可以，但不应该使`ops::Not`成为`SortedDisjoint`的超类型，因为这将使在外部类型上实现`SortedDisjoint`变得不可能，例如`[Itertools::Tee](https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.tee)`。）

然而，你可以并且应该在尽可能多的自定义类型上定义这些运算符。例如，这里是`CheckSortedDisjoint`的`ops::Not`的实现：

```py
impl<T: Integer, I> ops::Not for CheckSortedDisjoint<T, I>
where
    I: Iterator<Item = RangeInclusive<T>>,
{
    type Output = NotIter<T, Self>;

    fn not(self) -> Self::Output {
        self.complement()
    }
}
```

这让我们可以在这个函数中使用`!`：

```py
fn print_first_complement_gap() {
    let a = CheckSortedDisjoint::from([-10i16..=0, 1000..=2000]);
    println!("{:?}", (!a).next().unwrap()); // prints -32768..=-11
}
```

> 额外说明：这个示例函数运行在常量时间和内存中。

**子规则 #3: 利用所有权：** 我最初不支持`|=`（`BitOrAssign`）运算符。我认为如果用户想将两个`RangeSetBlaze`对象进行并集并将结果放入第一个对象中，他们可以直接写`a = a | b`。

然而，后来我发现了一个潜在的加速机会。具体来说，当`b`中的范围数量相对于`a`非常少时，我们应该一次一个地将`b`的范围添加到`a`中，而不是将它们的两个`ranges`迭代器合并在一起。因此，我实现了`BitOrAssign`。（我没有针对交集的加速方法，所以没有实现`BitAndAssign`。）

但等等，还有更多！当用户调用`a | &b`时，运算符拥有第一个输入。这意味着代码可以在原地修改第一个输入（如果愿意的话）并将其作为结果返回。这是该运算符的定义：

```py
impl<T: Integer> BitOr<&RangeSetBlaze<T>> for RangeSetBlaze<T> {
    type Output = RangeSetBlaze<T>;
    fn bitor(mut self, other: &Self) -> RangeSetBlaze<T> {
        self |= other;
        self
    }
}
```

我最终为`a | &b`和`a | b`以及`&a | b`创建了特殊的优化实现。Rust甚至允许我优化`&mut a |= b`，无论是`b`更小（如之前所述）还是`a`更小（我们将`a`放入拥有的`b`中，一次一个范围，然后将`a`赋值给`b`）。

**子规则 #4: 提供快速的多路操作：** 我们可以通过定义两个`RangeSetBlaze`对象的交集（运算符`&`）来实现对其`ranges`的交集。

```py
(a.ranges() & b.ranges()).into_range_set_blaze()
```

并且，得益于[布尔代数](https://en.wikipedia.org/wiki/Boolean_algebra)的奇迹，我们可以通过补集和并集来定义两个`RangesIter`的交集：

```py
!(self.complement() | other.into_iter().complement())
```

这为所有`SortedDisjoint`迭代器提供了一个常量内存的一次性交集。（同样的技巧也适用于集合差异。对称差异需要两次遍历，但仍然只需要常量内存。）

> 额外说明：`BTreeSet`不提供`complement`。为什么？因为例如`[0, 3, 4, 5, 10]`的补集将需要40亿个条目。然而，基于范围的表示法只存储4个条目：`-2147483648..=-1, 1..=2, 6..=9, 11..=2147483647`。

那么多个`RangeSetBlaze`对象的交集呢？我们可以一次做两个，但那样会产生不必要的中间对象。相反，我们可以提供多路交集功能，这样用户就可以这样做：

```py
use range_set_blaze::prelude::*;

let a = RangeSetBlaze::from_iter([1..=6, 8..=9, 11..=15]);
let b = RangeSetBlaze::from_iter([5..=13, 18..=29]);
let c = RangeSetBlaze::from_iter([-100..=100]);

let intersection = [a, b, c].intersection();

assert_eq!(intersection, RangeSetBlaze::from_iter([5..=6, 8..=9, 11..=13]));
```

多路交集被定义为对`RangeSetBlaze`对象的迭代器的一种方法：

```py
pub trait MultiwayRangeSetBlaze<'a, T: Integer + 'a>:
    IntoIterator<Item = &'a RangeSetBlaze<T>> + Sized
{
    fn intersection(self) -> RangeSetBlaze<T> {
        self.into_iter()
            .map(RangeSetBlaze::ranges)
            .intersection()
            .into_range_set_blaze()
    }
}
```

这调用了新的多路`SortedDisjoint` `intersection`和`union`：

```py
pub trait MultiwaySortedDisjoint<T: Integer, I>: IntoIterator<Item = I> + Sized
where
    I: SortedDisjoint<T>,
{
    fn intersection(self) -> BitAndKMerge<T, I> {
        self.into_iter()
            .map(|seq| seq.into_iter().complement())
            .union()
            .complement()
    }

    fn union(self) -> BitOrKMerge<T, I> {
        UnionIter::new(KMerge::new(self))
    }
}
```

将这些结合起来，我们能够在一次通过中交集任意数量的`RangeSetBlaze`对象，并且使用恒定的内存（加上最终`RangeSetBlaze`结果的内存）。

**子规则 #5: 如有需要，使用** `**Box<dyn**` **…**`**>**` **类型在不同类型之间提供多路操作：** `SortedDisjoint`上的多路运算符可能会遇到问题。你可以对相同类型的输入进行并集，这里是`RangesIter<T>`：

```py
let _i0 = [a.ranges(), b.ranges(), c.ranges()].intersection();
```

但不能处理混合类型的输入，这里是`NotIter<T, RangesIter<T>>`和`RangesIter<T>`：

```py
// doesn't compile: let _i1 = [!a.ranges(), b.ranges(), c.ranges()].intersection();
```

解决方案是将所有输入封装在一个新的通用类型中（详细信息见[GitHub](https://github.com/CarlKCarlK/range-set-blaze/blob/main/src/dyn_sorted_disjoint.rs)）：

```py
pub struct DynSortedDisjoint<'a, T: Integer> {
    iter: Box<dyn SortedDisjoint<T> + 'a>,
}
```

你可以显式地使用新类型或通过宏来使用它。

```py
let _i2 = [
        DynSortedDisjoint::new(!a.ranges()),
        DynSortedDisjoint::new(b.ranges()),
        DynSortedDisjoint::new(c.ranges()),
    ]
    .intersection();
// or
let _i3 = intersection_dyn!(!a.ranges(), b.ranges(), c.ranges());
```

# 规则 7: 遵循九条良好 API 设计规则，特别是“编写良好文档”

文章[优雅的 Rust 库 API 的九条规则](/nine-rules-for-elegant-rust-library-apis-9b986a465247)中的所有规则都适用。这里讨论了最有趣的六条，首先是：

> 编写良好的文档以保持设计的诚实。
> 
> 创建不会让你感到尴尬的示例。

你应该在你的`lib.rs`中加入`#![warn(missing_docs)]`。这将提醒你为每个公共类型、特性和方法编写文档。在（几乎）每一部分文档中包含一个示例。

在某个情况下，我厌倦了向用户警告某些迭代器应仅包含排序且不相交的范围。这使我回到规则5，并让编译器强制执行所需的保证。

在另一个情况下，我发现自己在多个地方解释如何调用并集。具体来说，我告诉用户在短的第二输入和`| (union)`运算符的情况下使用`extend`方法。对此我感到解释很尴尬。这导致我提供了一个`|=`运算符，它总是做最快的事情。

> 使用[Clippy](https://doc.rust-lang.org/stable/clippy/usage.html)

这个规则很重要。我现在认为这是理所当然的。所以，使用[Clippy](https://doc.rust-lang.org/stable/clippy/usage.html)。

> 接受所有类型的类型。

我主要尝试遵循这个规则，但有一个重大例外。虽然 Rust 提供了[许多范围类型](https://doc.rust-lang.org/reference/expressions/range-expr.html)，但我只接受`start..=end`形式的范围。这不仅简化了代码，而且我认为它也简化了文档和示例。（根据用户的请求，我会重新考虑这个问题。）

> 定义并返回友好的错误信息。

出乎意料的是，这个规则在许多数据结构中并不常见。例如，标准的`BTreeSet`不会返回可能是错误的结果。同样，`RangeSetBlaze`在几个地方可能会引发恐慌——例如，如果`CheckSortedDisjoint`发现问题——但它从不返回错误结果。

> 了解用户的需求，最好通过使用自己的产品来了解。

我感到很遗憾没有遵循这个规则。我创建`RangeSetBlaze`是为了好玩。我的当前项目中没有需要它的地方。我希望其他人能够在他们的项目中使用它，并给我反馈他们需要什么。

# 规则 8：使用代表性数据、Criterion 基准测试和分析工具来优化性能

> 附注： [基准测试报告的最新版本](https://github.com/CarlKCarlK/range-set-blaze/blob/main/docs/bench.md)显示了最新结果，包括与流行的 Roaring 压缩位图的比较。

**子规则 #1：使用代表性数据：** 在某些数据上，`RangeSetBlaze`会比标准的`HashSet`和`BTreeSet`表现更差。例如，从随机整数（均匀且有替换）构造的速度大约比`HashSet`慢2.5倍。以下是构造时间与随机整数数量的关系图：

![](../Images/09edd05de2bff783c9e533a2f9d5e3a2.png)

但如果整数是“聚集的”呢？例如，12、13、14、998、999、1000、1001、-333、-332等。为了查看，我们将构造100万个聚集的整数。我们将使平均聚集大小从1（没有聚集）变化到100K（十个大聚集）。（更多细节，请参见[（某些）范围相关的 Rust Crate 基准测试](https://github.com/CarlKCarlK/range-set-blaze/blob/main/docs/bench.md)）。有了这些数据，当聚集大小达到100时，`RangeSetBlaze`比`HashTable`快30倍，比`BTreeSet`快15倍。如果我们可以将聚集作为范围（而不是单独的整数）提供，则当聚集大小为1000时，`RangeSetBlaze`比标准数据类型快700倍。

![](../Images/32f98d86f37b16ade7c3565eb2e6d2ed.png)

这里的建议是对你认为有趣的数据（无论是实际数据还是合成数据）进行测试。对于这个项目，我创建了“随机聚集整数迭代器”，并控制了以下内容：

+   整数的范围（例如，所有整数都在`0..=9_999_999`中）

+   整数的数量（例如，100万）

+   平均聚集大小（例如，100）

+   迭代器的数量（例如，1）

+   coverage，这是期望由迭代器（如果有多个则取并集或交集）覆盖的范围的比例——（例如，10% coverage）。

**子规则 #2：使用** [**Criterion crate**](https://docs.rs/criterion/latest/criterion/) **在 Rust 中基准测试程序：** 以下是一些提示：

+   要获得良好的 HTML 报告和出色的图表，在你的`Cargo.toml`的`[dev-dependencies]`部分中加入`criterion = { version = "`当前版本`", features = ["html_reports"] }`。（此说明目前在[Criterion 文档中缺失](https://stackoverflow.com/questions/75808243/rust-criterion-wont-create-html-report)）。

+   在你的`Cargo.toml`中加入以下内容。它声明了一个名为“bench”的基准测试，使用第三方基准测试工具 Criterion。

```py
[[bench]]
name = "bench"
harness = false
```

+   在项目的顶层创建文件夹`benches`和文件`bench.rs`。文件的最后一行应为`criterion_main!(benches);`

+   根据 [Criterion 用户指南](https://bheisler.github.io/criterion.rs/book/index.html) 中的描述创建基准测试。

+   不要和 Criterion 抵触。它可能会抱怨你的基准测试太慢。如果可以，听取它的意见并加快速度。这将帮助它生成更好的统计数据。

> 附注：你可以在调试器下和作为集成测试运行 Criterion 实验。有关示例，请参见 `[fn debug_k_play](https://github.com/CarlKCarlK/range-set-blaze/blob/main/tests/integration_test.rs#L634)` 和 `[tests/integration.rs](https://github.com/CarlKCarlK/range-set-blaze/blob/main/tests/integration_test.rs)`。关键是将公共代码放到自己的项目中（例如，`[tests_common](https://github.com/CarlKCarlK/range-set-blaze/tree/main/tests_common)`），然后在主 `[Cargo.toml](https://github.com/CarlKCarlK/range-set-blaze/blob/main/Cargo.toml#LL16C1-L18C32)` 文件中创建一个工作区。

**子规则 #3: 使用基准测试来驱动设计决策：** 八年前，当我创建这个数据结构的 [Python 版本](https://fastlmm.github.io/PySnpTools/#util-intrangeset)时，我将排序后的范围存储在一个向量中。对于新版本，我想知道使用 Rust 的标准 `BTreeMap` 是否会更快。为了验证，让我们将两个基于 `BTreeMap` 的库 — `[rangemap](https://crates.io/crates/rangemap)` 和 `RangeSetBlaze` — 与两个基于 `SmallVec` 的库 — `[Range-collections](https://crates.io/crates/range-collections)` 和 `[range-set](https://crates.io/crates/range-set)` 进行比较：

![](../Images/7a9464c61a2b308d82806ef7d34df6e9.png)

（有关详细信息，请参见 [基准测试报告](https://github.com/CarlKCarlK/range-set-blaze/blob/main/docs/bench.md)）。总结一下，最快的基于向量的方法比最慢的基于树的方法慢 14 倍。它还比 `RangeSetBlaze` 慢 50 倍。这并不令人惊讶，因为基于向量的方法并不是为我们关注的数据的大量插入设计的。

这是另一个例子。当我们将多个 `RangeSet` 对象相交（或并集）时，逐个处理可以吗，还是多路处理更快？基准测试给出了答案：

![](../Images/0288dffabbcc82ca0ebc572912515a2b.png)

在两个集合上，所有方法都相似，但随着集合数量的增加，逐个处理的速度就会落后。到 100 个集合时，逐个处理必须创建大约 100 个中间集合，速度比多路处理慢约 14 倍。动态多路处理未被 `RangeSetBlaze` 使用，但 `SortedDisjoint` 迭代器有时需要。它比静态多路处理慢 5% 到 10%。（[详细信息](https://github.com/CarlKCarlK/range-set-blaze/blob/main/docs/bench.md)）。

**子规则 #4: 对代码进行分析以查找瓶颈：** 最受欢迎的 Rust 分析工具是 `[flamegraph](https://github.com/flamegraph-rs/flamegraph)`。我发现它很容易使用，但我错过了能够进行互动缩放和操作结果的功能。然而，实际上你可以使用几乎任何分析工具来配合 Rust。

例如，我订阅了 Visual Studio 2022 的付费版。我以前在 C++ 和 C# 代码上使用了它的全功能分析器。要在 Rust 上使用它，我只需将其（临时）添加到我的 `Cargo.toml` 文件中，并在发布模式下重新编译。

```py
[profile.release]
debug = true
```

其他功能全面的分析器，虽然我没有使用过，但看起来可以与 Rust 配合使用的包括 [AMD μProf](https://www.amd.com/en/developer/uprof.html) 和在 Windows 上的 [Superluminal](https://superluminal.eu/rust/)。

# 规则9：测试覆盖率、文档、特性、编译器错误和正确性

数据结构通常支持多种方法，并被其他项目使用。因此，它们需要广泛的测试。

**规则1：结合集成测试和覆盖测试：** 集成测试只能看到数据结构的公共接口。我将我的测试放在 `tests/integration_test.rs` 中。每个测试函数都以 `#[test]` 开头。

覆盖测试确保你的代码中的每一行至少被测试运行一次。使用强大且易用的 `[llvm-cov](https://github.com/taiki-e/cargo-llvm-cov#)` 来测量覆盖率。安装说明见 [这里](https://github.com/taiki-e/cargo-llvm-cov#installation)。用命令运行它：`cargo llvm-cov --open.` 

要将覆盖率提升到100%，你需要添加测试。我建议尽可能将这些覆盖测试纳入你的集成测试中。这将使你能够体验到公共接口的可用性。

要在你支持的类型之间创建覆盖（以及其他）测试，请使用神奇的 [syntactic-for](https://crates.io/crates/syntactic-for) crate：

```py
#[test]
fn lib_coverage_6() {
    syntactic_for! { ty in [i8, u8, isize, usize,  i16, u16, i32, u32, i64, u64, isize, usize, i128, u128] {
        $(
            let mut a = RangeSetBlaze::<$ty>::from_iter([1..=3, 5..=7, 9..=120]);
            a.ranges_insert(2..=100);
            assert_eq!(a, RangeSetBlaze::from_iter([1..=120]));

        )*
    }};
}
```

**规则2：在文档中添加多个示例并将其作为测试运行**：以下是 `SortedDisjoint::equals` 文档中的一个示例：

```py
 /// Given two [`SortedDisjoint`] iterators, efficiently tells if they
    /// are equal. Unlike most equality testing in Rust,
    /// this method takes ownership of the iterators and consumes them.
    ///
    /// # Examples
    ///
    /// ```

    /// use range_set_blaze::prelude::*;

    ///

    /// let a = CheckSortedDisjoint::from([1..=2]);

    /// let b = RangeSetBlaze::from_iter([1..=2]).into_ranges();

    /// assert!(a.equal(b));

    /// ```py
```

这可以通过 `cargo test` 或 `cargo test --doc` 运行。（不过，在测量覆盖率时，它不会运行。）

**规则3：测试所有结构体和枚举的缺失特性：** 我希望 `RangeSetBlaze::Iter` 一般支持与标准 `BTreeSet::Iter` 相同的特性。此外，所有类型应该在实际情况允许的情况下自动实现 `Sized`、`Send`、`Sync` 和 `Unpin`。（参见这个 [Let’s Get Rusty](https://www.youtube.com/watch?v=Nzclc6MswaI&t=528s) 视频）。以下是测试特性的方法：

```py
fn is_sssu<T: Sized + Send + Sync + Unpin>() {}
fn is_like_btreeset_iter<T: Clone + std::fmt::Debug + FusedIterator + Iterator>() {}
// removed DoubleEndedIterator +ExactSizeIterator for now
#[test]
fn iter_traits() {
    type ARangesIter<'a> = RangesIter<'a, i32>;
    type AIter<'a> = Iter<i32, ARangesIter<'a>>;
    is_sssu::<AIter>();
    is_like_btreeset_iter::<AIter>();
}
```

当我第一次做这个时，我发现我缺少 `Debug`、`FusedIterator`、`DoubleEndedIterator` 和 `ExactSizeIterator`。我添加了前两个，并决定暂时不添加后两个。

类似地，一个将 `RangeSetBlaze` 的特性与标准 `BTreeSet` 的特性进行比较的测试提醒我实现 `Clone`、`PartialOrd`、`Hash`、`Eq`、`Ord` 和 `IntoIterator`。

**子规则 #4: 测试非法值确实无法表示：** 你希望你的数据结构在用户应用于错误类型时（例如，尝试创建一个字符集而不是整数集）引发编译器错误。同样，如果用户给出的是普通迭代器而不是像 `SortedDisjoint` 这样的迭代器，你的一些方法也应该引发编译器错误。我们如何测试这一点？

使用 `[trybuild](https://docs.rs/trybuild/latest/trybuild/)` 创建“ui”测试。首先，创建一个像这样的集成测试：

```py
#[test]
fn ui() {
    let t = trybuild::TestCases::new();
    t.compile_fail("tests/ui/*.rs");
}
```

然后在 `tests/ui` 中，创建如 `untrusted_pairs.rs` 这样的文件，以测试编译器错误信息。

```py
use range_set_blaze::RangeSetBlaze;

fn main() {
    let guaranteed = RangeSetBlaze::from_iter([1..=2, 3..=4, 5..=6]).into_ranges();
    let _range_set_int = RangeSetBlaze::from_sorted_disjoint(guaranteed); // yep
    let not_guaranteed = [5..=6, 1..=3, 3..=4].into_iter();
    let _range_set_int = RangeSetBlaze::from_sorted_disjoint(not_guaranteed); // nope
}
```

设置环境变量 `TRYBUILD=overwrite` 以记录预期的编译器错误信息。详细信息见 `[trybuild](https://docs.rs/trybuild/latest/trybuild/)`。如果你在本地无法得到与 CI（例如 GitHub Action）相同的结果，请参见 [这个线程](https://github.com/dtolnay/trybuild/issues/236#issuecomment-1620950759)。

**子规则 #5: 使用自动化 QuickCheck 测试验证正确性：** 根据 [Rüdiger Klaehn](https://github.com/rklaehn/sorted-iter) 的推荐，我实现了自动化的 `[QuickCheck](https://github.com/BurntSushi/quickcheck)` 测试。这里是一个测试，它检查 `RangeSetBlaze::is_disjoint` 是否在 `QuickCheck` 生成的值上与 `BTreeSet::is_disjoint` 得到相同的答案。

```py
type Element = i64;
type Reference = std::collections::BTreeSet<Element>;

#[quickcheck]
fn disjoint(a: Reference, b: Reference) -> bool {
    let a_r = RangeSetBlaze::from_iter(&a);
    let b_r = RangeSetBlaze::from_iter(&b);
    a.is_disjoint(&b) == a_r.is_disjoint(&b_r)
}
```

所以，你了解了，在 Rust 中创建数据结构的九条规则。创建 Rust 数据结构比我预期的时间要长。这一方面是因为我创建了一个完整的`SortedDisjoint`子库，另一方面则是因为 Rust 的要求，比如需要我定义 13 个公共迭代器结构体。然而，从 Rust 的速度和安全性来看，这段时间是值得的。按照这九条规则来创建你自己的强大 Rust 数据结构吧。

> *顺便提一下，如果你对未来的文章感兴趣，请* [*在 Medium 上关注我*](https://medium.com/@carlmkadie)*。我会写关于 Rust 和 Python 的科学编程、机器学习和统计学的文章。我通常每月写一篇文章。*
