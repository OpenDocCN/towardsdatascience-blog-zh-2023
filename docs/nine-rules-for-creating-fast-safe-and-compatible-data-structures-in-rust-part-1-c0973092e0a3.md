# 创建快速、安全且兼容的数据结构的九条规则（第1部分）

> 原文：[https://towardsdatascience.com/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-1-c0973092e0a3?source=collection_archive---------6-----------------------#2023-04-05](https://towardsdatascience.com/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-1-c0973092e0a3?source=collection_archive---------6-----------------------#2023-04-05)

## 来自 RangeSetBlaze 的经验教训

[](https://medium.com/@carlmkadie?source=post_page-----c0973092e0a3--------------------------------)[![Carl M. Kadie](../Images/9dbe27c76e9567136e5a7dc587f1fb15.png)](https://medium.com/@carlmkadie?source=post_page-----c0973092e0a3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c0973092e0a3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c0973092e0a3--------------------------------) [Carl M. Kadie](https://medium.com/@carlmkadie?source=post_page-----c0973092e0a3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5e87027005f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-1-c0973092e0a3&user=Carl+M.+Kadie&userId=a5e87027005f&source=post_page-a5e87027005f----c0973092e0a3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c0973092e0a3--------------------------------) · 13 分钟阅读 · 2023年4月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc0973092e0a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-1-c0973092e0a3&user=Carl+M.+Kadie&userId=a5e87027005f&source=-----c0973092e0a3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc0973092e0a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-1-c0973092e0a3&source=-----c0973092e0a3---------------------bookmark_footer-----------)![](../Images/77adb3f2a102655781a270954799608d.png)

将数字存储在树中 — 来源：Stable Diffusion

今年，我开发了一个新的 Rust crate，名为 `[range-set-blaze](https://crates.io/crates/range-set-blaze)`，它实现了范围集合数据结构。范围集合是一种有用（虽然较少见）的数据结构，它将整数集合存储为已排序且不相交的范围。例如，它存储以下三个范围：

```py
100..=2_393, 20_303..=30_239_000, 501_000_013..=501_000_016
```

而不是30220996个单独的整数。除了潜在的内存节省，`range-set-blaze`还提供了高效的集合操作，如并集、交集、补集、差集和对称差集。

在创建`range-set-blaze`时，我学到了九条规则，这些规则可以帮助你在Rust中创建数据结构。除了数据结构，这些规则中的许多还可以帮助你提高任何Rust代码的性能和兼容性。

规则如下：

1.  抄袭你的API、文档，甚至代码——从标准库中抄袭。

1.  设计构造函数以便于使用、兼容性和速度。

1.  创建比预期更多的Rust迭代器。

1.  使用traits使非法值不可表示。

1.  定义具有保证属性和有用方法的通用迭代器。

*在* [*第2部分*](/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-2-da5e6961a0b7)*中讨论：*

*6\. 定义运算符和快速操作。*

*7\. 遵循“良好API设计的九条规则”，特别是“编写良好的文档”。*

*8\. 使用代表性数据、Criterion Benchmarking和性能分析来优化性能。*

*9\. 测试覆盖率、文档、traits、编译器错误和正确性。*

在查看前五条规则之前，让我们先看看`range-set-blaze`可能的使用场景，它的集合操作是如何工作的，以及它与其他范围集crate的比较。

**有用性：** 想象一下在一个不可靠的集群上运行100亿个统计实验。集群上的每个任务运行几个实验。每个实验产生一行带有实验编号的输出。所以，一个任务可能会把这些放入一个文件中：

![](../Images/39ad3c9b2cde703e0f3e8e37d1c985c9.png)

你会使用什么数据结构来查找哪些实验缺失并需要重新提交？一个选项是：将输出的实验编号存储在一个`[BTreeSet](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html)`中，然后进行线性扫描以查找间隙。

更快且内存效率更高的选项：使用范围集。八年前，我创建了`[IntRangeSet](https://fastlmm.github.io/PySnpTools/#util-intrangeset)`，一个用Python编写的范围集来解决这个问题。现在，我会在Rust中使用`range-set-blaze`（[示例代码](https://github.com/CarlKCarlK/range-set-blaze/blob/main/examples/missing.rs)）。

**集合操作**：这是一个简单的并集运算符（`|`）示例：

![](../Images/c39d2fc449a075e26bc091e3cdf11e5e.png)

```py
use range_set_blaze::RangeSetBlaze;
// a is the set of integers from 100 to 499 (inclusive) and 501 to 1000 (inclusive)
let a = RangeSetBlaze::from_iter([100..=499, 501..=999]);
 // b is the set of integers -20 and the range 400 to 599 (inclusive)
let b = RangeSetBlaze::from_iter([-20..=-20, 400..=599]);
// c is the union of a and b, namely -20 and 100 to 999 (inclusive)
let c = a | b;
assert_eq!(c, RangeSetBlaze::from_iter([-20..=-20, 100..=999]));
```

> 附注：请参阅项目的`[README.md](https://github.com/CarlKCarlK/range-set-blaze)`以获取来自生物学的另一个集合运算符示例。该示例使用`RangeSetBlaze`结构体从转录区域和外显子区域中查找基因的内含子区域。

**与其他范围相关的crate的比较**

**好处：** 尽管 [Rust 的 crates.io 已经包含了几个范围集合的 crate](https://github.com/CarlKCarlK/range-set-blaze/blob/main/docs/bench.md)，我希望我的版本能提供完整的集合操作，同时保持性能。通过各种优化措施，我相信它达到了这些目标（请参见 [基准报告](https://github.com/CarlKCarlK/range-set-blaze/blob/main/docs/bench.md)）。例如，它可以比最流行的范围集合 crate 快 75 倍来处理单个整数（因为其他 crate 没有对单个处理做特殊优化——但它可以轻松添加这种优化）。在另一个基准测试中，`range-set-blaze`——使用混合算法——在合并两个集合时比其他 crate 快 30% 到 600%。

**不足：** 与其他范围相关的 crate 相比，`range-set-blaze` 有两个重要不足。首先，它仅适用于 Rust 整数类型。大多数其他 crate 处理任何可以排序的元素（日期、浮点数、IP 地址、字符串等）。其次，它仅提供集合功能。许多其他 crate 还处理映射。随着兴趣（以及可能的帮助），这些不足可能会在未来得到解决。

创建数据结构需要做出许多决策。根据我在 `range-set-blaze` 上的经验，以下是我推荐的决策。为了避免优柔寡断，我将这些建议表述为规则。当然，每个数据结构都不同，因此并非每条规则都适用于每个数据结构。

本文涵盖规则 1 到 5。[第 2 部分](/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-2-da5e6961a0b7) 涵盖规则 6 到 9。

# 规则 1：抄袭 API、文档甚至代码——来自标准库

查找标准库中的类似数据结构，并逐行研究其文档。我选择了 [`BTreeSet`](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html) 作为我的模型。它可以在缓存高效的平衡树中存储整数集合。

> 附带说明：稍后，在基准测试（规则 8）中，我们将看到 `range_set_blaze::*RangeSetBlaze*` 在某些“块状”整数集合上的速度可能比 `*BTreeSet*` 快 1000 倍。

`BTreeSet` 提供了 28 个方法，例如，[`clear`](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html#method.clear) 和 [`is_subset`](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html#method.is_subset)。它还实现了 18 个特性，例如，[`FromIterator<T>`](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html#impl-FromIterator%3CT%3E-for-BTreeSet%3CT%2C%20Global%3E)。这是 `BTreeSet` 的 `clear` 文档和 `RangeSetBlaze` 的 `clear` 文档：

![](../Images/7852ec97e3809279c5dfd01079f1fe04.png)

你可以看到我主要是直接复制的。我将“元素”改为“整数元素”，以提醒用户 `RangeSetBlaze` 支持什么。我删除了 `where A: Clone`，因为所有整数必然是可克隆的。注意，Rust 文档包括一个“源”链接，这使得复制变得容易。

复制提供了这些优点：

+   它告诉你需要提供哪些方法。换句话说，它为你的 API（应用程序编程接口）提供了一个起点。这节省了设计时间。此外，用户会理解并期望这些方法。你甚至可以使你的数据结构成为标准数据结构的直接替代品。

+   几乎可以免费获得文档文本和文档测试。

+   你甚至可以复制代码。例如，这里是 `BTreeSet` 和 `RangeSetBlaze` 的 `is_superset` 代码：

```py
#[must_use]
#[stable(feature = "rust1", since = "1.0.0")]
pub fn is_superset(&self, other: &BTreeSet<T, A>) -> bool
where
    T: Ord,
{
    other.is_subset(self)
}
```

```py
#[must_use]
pub fn is_superset(&self, other: &RangeSetBlaze<T>) -> bool {
    other.is_subset(self)
}
```

`BTreeSet` 代码让我想起了超集可以通过子集来定义，以及 `#[must_use]` 是一个存在且在这里适用的特性。

你可能 *决定* 不支持标准数据结构中的所有功能。例如，我跳过了 `new_in`，这是一个实验性特性。同样，标准库支持映射（不仅仅是集合）、任何可排序的元素（不仅仅是整数）和 Serde 序列化。对我而言，这些是可能的未来特性。

你也可以 *决定* 以不同的方式支持某些内容。例如，`BTreeSet::first` 返回 `Option<&T>` 但 `RangeSetBlaze::first` 返回 `Option<T>`。我知道 `T` 是一个便于克隆的整数，所以不需要是一个引用。

> 顺便提一下：Rust 没有一个通用的 `*Set*` 特性来告诉所有集合应该实现哪些方法，甚至提供一些默认实现（例如，`*is_superset*` 以 `*is_subset*` 作为基础）吗？没有，但这个问题正在被 [讨论](https://www.reddit.com/r/rust/comments/ypuf8f/generic_mapset_traits/)。

你也可能 *决定* 支持比标准数据结构更多的方法。例如，`RangeSetBlaze::len` 和 `BTreeSet::len` 一样，返回集合中的元素数量。然而，`RangeSetBlaze` 还提供 `ranges_len`，它返回集合中排序的、不相交的范围的数量。

# 规则 2：设计构造函数以提高易用性、兼容性和速度

如果有一个空版本的数据结构是有意义的，你会想定义一个 `new` 方法和一个 `[Default::default](https://doc.rust-lang.org/std/default/trait.Default.html)` 方法。

类似地，如果从迭代器填充数据结构是有意义的，你会想定义 `[FromIterator::from_iter](https://doc.rust-lang.org/std/iter/trait.FromIterator.html)` 方法。这些方法也会自动定义 `collect` 方法。像 `BTreeSet` 一样，`RangeSetBlaze` 接受整数的迭代器和对整数的引用。（接受引用很重要，因为许多 Rust 迭代器提供引用。）以下是 `from_iter` 和 `collect` 使用的示例：

```py
let a0 = RangeSetBlaze::from_iter([3, 2, 1, 100, 1]);
let a1: RangeSetBlaze<i32> = [3, 2, 1, 100, 1].into_iter().collect();
assert!(a0 == a1 && a0.to_string() == "1..=3, 100..=100");
```

`RangeSetBlaze` 也接受包含范围的迭代器以及对这些范围的引用。它对输入范围没有限制。这些范围可以是无序的、重叠的、空的或重复的。

```py
#[allow(clippy::reversed_empty_ranges)]
let a0 = RangeSetBlaze::from_iter([1..=2, 2..=2, -10..=-5, 1..=0]);
#[allow(clippy::reversed_empty_ranges)]
let a1: RangeSetBlaze<i32> = [1..=2, 2..=2, -10..=-5, 1..=0].into_iter().collect();
assert!(a0 == a1 && a0.to_string() == "-10..=-5, 1..=2");
```

最后，考虑定义额外的 `From::from` 方法。这些方法会自动定义 `into` 方法。例如，为了兼容 `BTreeSet`，`RangeSetBlaze` 在数组上定义了一个 `From::from` 方法。

```py
let a0 = RangeSetBlaze::from([3, 2, 1, 100, 1]);
let a1: RangeSetBlaze<i32> = [3, 2, 1, 100, 1].into();
assert!(a0 == a1 && a0.to_string() == "1..=3, 100..=100")
```

`RangeSetBlaze`还定义了`from_sorted_disjoint/into_range_set_blaze`，用于保证已排序且不相交的区间的迭代器。（我们将在规则5中看到，如何通过特殊特性和Rust编译器来强制执行这一保证。）

```py
let a0 = RangeSetBlaze::from_sorted_disjoint(CheckSortedDisjoint::from([-10..=-5, 1..=2]));
let a1: RangeSetBlaze<i32> = CheckSortedDisjoint::from([-10..=-5, 1..=2]).into_range_set_blaze();
assert!(a0 == a1 && a0.to_string() == "-10..=-5, 1..=2");
```

> 附言：为什么使用`*from_sorted_disjoint*`/`*into_range_set_blaze*`而不是`*from_iter /collect*`或`*from/into*`？请参见[这个讨论](https://stackoverflow.com/questions/75522966/construct-a-struct-that-holds-a-generic-iterator-of-i32-from-from-iter)和[这个讨论](https://stackoverflow.com/questions/37347311/how-is-there-a-conflicting-implementation-of-from-when-using-a-generic-type)。

对于你的每一个构造函数，考虑可能的加速和优化。`RangeSetBlaze`在`from_iter`中实现了这种优化：

+   将相邻（可能无序）整数/区间合并成不相交的区间，O(*n₁*)

+   按起始位置对不相交的区间进行排序，O(*n₂* log *n₂*)

+   合并相邻的区间，O(*n₂*)

+   从现在排序且不相交的区间创建一个`BTreeMap`，O(*n₃* log *n₃*)

其中 *n*₁ 是输入整数/区间的数量，n₂ 是不相交且无序的区间数量，*n*₃ 是最终排序且不相交的区间数量。

“块状”整数的影响是什么？如果 *n₂* ≈ sqrt(*n₁*)，则构建时间为O(*n₁*)。（实际上，只要 *n₂* ≤ *n₁*/ln(*n₁*)，构建时间为O(*n₁*)。）在基准测试中，这在块状整数迭代器上变成了比`HashSet`和`BTreeSet`快700倍。

# 规则3：创建比你预期的更多的Rust迭代器

你猜测标准`BTreeSet`定义了多少种不同的迭代器类型？

答案是八种：`Iter`，`IntoIter`，`DrainFilter`，`Range`，`Difference`，`SymmetricDifference`，`Intersection`，和`Union`。许多非Rust编程语言可以将任何方法变成迭代器/生成器，只需几个“yield”语句。然而，Rust并不提供这种功能（但[正在讨论中](https://www.reddit.com/r/rust/comments/nsaovn/generatorsyield/)）。因此，几乎每个与迭代相关的方法都需要你定义一个新的迭代器结构类型。这些结构至少会实现一个`next`方法，该方法返回`Some(`值`)`或`None`。

`RangeSetBlaze`及其相关类型定义了13个迭代器结构。让我们看两个。

首先，用户可以调用`ranges`并将整数作为一系列排序的不相交区间进行迭代。（请记住，`RangeSetBlaze`接受无序、重叠的区间，但存储排序的不相交区间。）

```py
use range_set_blaze::RangeSetBlaze;
let set = RangeSetBlaze::from_iter([30..=40, 15..=25, 10..=20]);
let mut ranges = set.ranges();
assert_eq!(ranges.next(), Some(10..=25));
assert_eq!(ranges.next(), Some(30..=40));
assert_eq!(ranges.next(), None);
```

在内部，`RangeSetBlaze`使用标准`BTreeMap`来存储区间信息。因此，`RangeSetBlaze::ranges`方法构造一个包含`BTreeMap::Iter`的`RangesIter`结构。然后我们让`RangesIter::next`方法调用`BTreeMap::Iter`的`next`方法，并将结果转换成所需类型。这里是代码：

```py
impl<T: Integer> RangeSetBlaze<T> {
    pub fn ranges(&self) -> RangesIter<'_, T> {
        RangesIter {
            iter: self.btree_map.iter(),
        }
    }
}

#[derive(Clone, Debug)]
#[must_use = "iterators are lazy and do nothing unless consumed"]
pub struct RangesIter<'a, T: Integer> {
    pub(crate) iter: btree_map::Iter<'a, T, T>,
}

impl<'a, T: Integer> Iterator for RangesIter<'a, T> {
    type Item = RangeInclusive<T>;
    fn next(&mut self) -> Option<Self::Item> {
        self.iter.next().map(|(start, end)| *start..=*end)
    }
    fn size_hint(&self) -> (usize, Option<usize>) {
        self.iter.size_hint()
    }
}
```

其次，用户可能希望调用`iter`并逐个以排序顺序遍历整数。在这种情况下，我们将返回一个名为`Iter`的结构体，它包含一个`RangeIter`，然后逐个遍历范围内的整数。以下是`Iter::next`的原始代码，之后是关注点的讨论。

```py
impl<T: Integer, I> Iterator for Iter<T, I>
where
    I: Iterator<Item = RangeInclusive<T>> + SortedDisjoint,
{
    type Item = T;
    fn next(&mut self) -> Option<T> {
        loop {
            if let Some(range) = self.option_range.clone() {
                let (start, end) = range.into_inner();
                debug_assert!(start <= end && end <= T::safe_max_value());
                if start < end {
                    self.option_range = Some(start + T::one()..=end);
                } else {
                    self.option_range = None;
                }
                return Some(start);
            } else if let Some(range) = self.iter.next() {
                self.option_range = Some(range);
                continue;
            } else {
                return None;
            }
        }
    }
```

`SortedDisjoint`特征涉及到保证内部迭代器提供排序的、不相交的范围。我们将在规则5中讨论它。

`option_range` 字段保存我们当前返回整数的范围（如果有的话）。我们使用`loop`和`continue`来填充空的`option_range`。这个循环最多只循环两次，因此我本可以使用递归。然而，其他一些迭代器的递归次数足以导致栈溢出。因此，…

[尾递归优化在Rust中没有保证](https://stackoverflow.com/questions/59257543/when-is-tail-recursion-guaranteed-in-rust)。我的政策是：在`next`函数中从不使用递归。

> 附注：感谢Michael Roth，当前版本的`Iter::next`现在更简短了。他的拉取请求在[这里](https://github.com/CarlKCarlK/range-set-blaze/commit/b582f30689502993e1079520ce806c3a905e523e)。

`BTreeSet`和`RangeSetBlaze`除了`iter`方法外，还定义了一个`into_iter`迭代器方法。同样，`RangeSetBlaze`除了其`ranges`方法外，还定义了一个`into_ranges`迭代器方法。这些`into`*_whatever*方法获取`RangeSetBlaze`的所有权，这在某些情况下很有用。

# 规则 4：通过特征使非法值不可表示

我说过`RangeSetBlaze`只适用于整数，但有什么阻止你将它应用于字符呢？

```py
use range_set_blaze::RangeSetBlaze;

fn _some_fn() {
    let _char_set = RangeSetBlaze::from_iter(['a', 'b', 'c', 'd']);
}
```

答案？编译器会阻止你。它返回这个错误消息：

```py
let _char_set = RangeSetBlaze::from_iter(['a', 'b', 'c', 'd']);
  |                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |                     the trait `Integer` is not implemented for `char`
  |
  = help: the following other types implement trait `Integer`:
            i128
            i16
            i32
            i64
            i8
            isize
            u128
            u16
          and $N others
```

为了实现这一点，`RangeSetBlaze`定义了一个它称之为`Integer`的特征。以下是该定义（以及我找到的所有超级特征）：

```py
pub trait Integer:
    num_integer::Integer
    + FromStr
    + fmt::Display
    + fmt::Debug
    + std::iter::Sum
    + num_traits::NumAssignOps
    + FromStr
    + Copy
    + num_traits::Bounded
    + num_traits::NumCast
    + Send
    + Sync
    + OverflowingSub
    + SampleUniform
{
    // associated type SafeLen definition not shown ...
    fn safe_len(range: &RangeInclusive<Self>) -> <Self as Integer>::SafeLen;
    fn safe_max_value() -> Self {        Self::max_value()    }
    fn f64_to_safe_len(f: f64) -> Self::SafeLen;
    fn safe_len_to_f64(len: Self::SafeLen) -> f64;
    fn add_len_less_one(a: Self, b: Self::SafeLen) -> Self;
    fn sub_len_less_one(a: Self, b: Self::SafeLen) -> Self;
}
```

接下来，我在所有感兴趣的整数类型（`u8`到`u128`包括`usize`，`i8`到`i128`包括`isize`）上实现了`Integer`特征。例如，

```py
impl Integer for i32 {
    #[cfg(target_pointer_width = "64")]
    type SafeLen = usize;
    fn safe_len(r: &RangeInclusive<Self>) -> <Self as Integer>::SafeLen {
        r.end().overflowing_sub(*r.start()).0 as u32 as <Self as Integer>::SafeLen + 1
    }
    fn safe_len_to_f64(len: Self::SafeLen) -> f64 {len as f64}
    fn f64_to_safe_len(f: f64) -> Self::SafeLen {f as Self::SafeLen}
    fn add_len_less_one(a: Self, b: Self::SafeLen) -> Self {a + (b - 1) as Self}
    fn sub_len_less_one(a: Self, b: Self::SafeLen) -> Self {a - (b - 1) as Self}
}
```

有了这个，我可以使代码泛型化为`<T: Integer>`，如规则3中的代码示例所示。

> 附注：为什么Rust没有提供一个标准的“整数”特征来做所有事情？[这里是讨论](https://www.reddit.com/r/rust/comments/qlyn12/how_to_write_a_generic_function_for_only_numeric/)。

# 规则 5：定义具有保证属性和有用方法的泛型迭代器

`RangeSetBlaze`的`from_sorted_disjoint`构造函数假设输入是排序好的不相交范围。这让`RangeSetBlaze`避免了工作。但是如果这个假设错误了呢？例如，如果我们给它未排序的范围，会发生什么？

```py
use range_set_blaze::RangeSetBlaze;

fn _some_fn() {
    let not_guaranteed = [5..=6, 1..=3, 3..=4].into_iter();
    let _range_set_int = RangeSetBlaze::from_sorted_disjoint(not_guaranteed);
}
```

与规则4一样，编译器会捕捉错误并返回有用的消息：

```py
7 |     let _range_set_int = RangeSetBlaze::from_sorted_disjoint(not_guaranteed);
  |                          ----------------------------------- ^^^^^^^^^^^^^^
                              the trait `SortedDisjoint<_>` is not implemented for `std::array::IntoIter<RangeInclusive<{integer}>, 3>`
  |                          |
  |                          required by a bound introduced by this call
  |
  = help: the following other types implement trait `SortedDisjoint<T>`:
            CheckSortedDisjoint<T, I> ...
```

为了实现这一点，`RangeSetBlaze`定义了特征`SortedDisjoint`。以下是相关定义：

```py
pub trait SortedStarts<T: Integer>: Iterator<Item = RangeInclusive<T>> {}
pub trait SortedDisjoint<T: Integer>: SortedStarts<T> {
// methods not shown, yet
}
```

这说明 `SortedDisjoint` 是对整数的泛型，并且每个 `SortedDisjoint` 必须是 `SortedStarts`。此外，所有 `SortedStarts` 都是整数范围的迭代器。

> 附注：我的项目需要两个新的特征，因为我需要保证两个不同的属性。需要保证一个属性的项目只需要一个新的特征。

那么，重点是什么呢？为什么要引入新的特征，而不是直接使用 `Iterator<Item = RangeInclusive<T>`？正如我从 Rüdiger Klaehn 的精彩 [sorted-iter](https://docs.rs/sorted-iter/latest/sorted_iter/) crate 中学到的，我们可以使用这些新特征来强制执行保证。例如，这个构造函数使用 `where` 子句只接受保证的（排序且不重叠的）整数迭代器：

```py
impl<T: Integer> RangeSetBlaze<T> {
    pub fn from_sorted_disjoint<I>(iter: I) -> Self
    where
        I: SortedDisjoint<T>,
    {
        // ... code omitted ...
    }
}
```

那么，保证的迭代器如何获得所需的 `SortedDisjoint` 特征？它们实现了这个特征！例如，我们知道 `RangeSetBlaze::ranges` 方法返回一个 `RangesIter` 迭代器，它由排序且不重叠的范围组成，因此我们让 `RangesIter` 实现 `SortedDisjoint` 特征，如下所示：

```py
impl<T: Integer> SortedStarts for RangesIter<'_, T> {}
impl<T: Integer> SortedDisjoint for RangesIter<'_, T> {}
```

就这样。我们已经将 `RangesIter` 标记为 `SortedDisjoint`。编译器会完成剩下的工作。

**不保证到保证**：我还标记了一个名为 `CheckSortedDisjoint` 的迭代器为 `SortedDisjoint`。有趣的是，它遍历一个 *不保证* 的内部迭代器。这怎么可能呢？实际上，当它迭代时，它也会检查——如果发现任何未排序或重叠的范围则会引发恐慌。结果是一个保证的迭代器。

**有时保证外部迭代器**：那么有时是`SortedDisjoint`而有时不是的迭代器呢？例如，流行的 `[Itertools::tee](https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.tee)` 方法将任何迭代器转换为两个具有相同内容的迭代器。如果其输入迭代器是`SortedDisjoint`，那么其输出迭代器也将是：

```py
impl<T: Integer, I: SortedDisjoint<T>> SortedDisjoint<T> for Tee<I> {}
```

**定义方法**：几乎可以说是额外的好处，我们可以在泛型 `SortedDisjoint` 迭代器上定义方法。例如，在这里我们定义了 `complement` 方法，该方法生成当前迭代器中 *不* 包含的每个排序且不重叠的整数范围。

```py
pub trait SortedDisjoint<T: Integer>: SortedStarts<T> {
    fn complement(self) -> NotIter<T, Self>
    where
        Self: Sized,
    {
        NotIter::new(self)
    }
}
```

这是来自 `complement` 文档的一个使用示例：

```py
use range_set_blaze::prelude::*;

let a = CheckSortedDisjoint::from([-10i16..=0, 1000..=2000]);
let complement = a.complement();
assert_eq!(complement.to_string(), "-32768..=-11, 1..=999, 2001..=32767");
```

`complement` 方法使用 `NotIter`，另一个迭代器（见规则 3）。`NotIter` 也实现了 `SortedDisjoint`。这个示例还使用了 `to_string`，这是另一个 `SortedDisjoint` 方法。

要使用 `complement` 和 `to_string`，用户必须

`use range_set_blaze::SortedDisjoint;` 或 `use range_set_blaze::prelude::*;`。前导模块起作用是因为项目的 `lib.rs` 指定了

`pub mod prelude;` 和 `prelude.rs` 文件包含这个 `pub use` 语句，它包括 `SortedDisjoint`：

```py
pub use crate::{
    intersection_dyn, union_dyn, CheckSortedDisjoint, DynSortedDisjoint, MultiwayRangeSetBlaze,
    MultiwayRangeSetBlazeRef, MultiwaySortedDisjoint, RangeSetBlaze, SortedDisjoint,
};
```

这些是创建 Rust 数据结构的前五条规则。请参见 [第 2 部分](/nine-rules-for-creating-fast-safe-and-compatible-data-structures-in-rust-part-2-da5e6961a0b7) 了解规则 6 到 9。

> 附言：如果你对未来的文章感兴趣，请[关注我的 Medium](https://medium.com/@carlmkadie)。我写关于 Rust 和 Python 的科学编程、机器学习和统计学的文章。我通常每个月写一篇文章。
