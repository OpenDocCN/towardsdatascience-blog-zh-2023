# Rust代码SIMD加速的九条规则（第一部分）

> 原文：[https://towardsdatascience.com/nine-rules-for-simd-acceleration-of-your-rust-code-part-1-c16fe639ce21?source=collection_archive---------2-----------------------#2023-12-12](https://towardsdatascience.com/nine-rules-for-simd-acceleration-of-your-rust-code-part-1-c16fe639ce21?source=collection_archive---------2-----------------------#2023-12-12)

## 通过将数据摄入在`range-set-blaze`库中提升7倍的一般经验教训。

[](https://medium.com/@carlmkadie?source=post_page-----c16fe639ce21--------------------------------)[![Carl M. Kadie](../Images/9dbe27c76e9567136e5a7dc587f1fb15.png)](https://medium.com/@carlmkadie?source=post_page-----c16fe639ce21--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c16fe639ce21--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c16fe639ce21--------------------------------) [Carl M. Kadie](https://medium.com/@carlmkadie?source=post_page-----c16fe639ce21--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5e87027005f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-simd-acceleration-of-your-rust-code-part-1-c16fe639ce21&user=Carl+M.+Kadie&userId=a5e87027005f&source=post_page-a5e87027005f----c16fe639ce21---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c16fe639ce21--------------------------------) ·17 min read·Dec 12, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc16fe639ce21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-simd-acceleration-of-your-rust-code-part-1-c16fe639ce21&user=Carl+M.+Kadie&userId=a5e87027005f&source=-----c16fe639ce21---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc16fe639ce21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-simd-acceleration-of-your-rust-code-part-1-c16fe639ce21&source=-----c16fe639ce21---------------------bookmark_footer-----------)![](../Images/03b7cab4f40a89f2f1582704fd509bdd.png)

蟹通过小蟹委派进行计算 — 来源：[https://openai.com/dall-e-2/](https://openai.com/dall-e-2/)。所有其他数据来自作者。

> 感谢Ben Lichtman（B3NNY）在西雅图Rust Meetup中为我指明了SIMD的正确方向。

[SIMD](https://en.wikipedia.org/wiki/Single_instruction,_multiple_data)（单指令、多数据）操作自 2000 年代初以来一直是 Intel/AMD 和 ARM CPU 的一个特性。这些操作使你可以，例如，只用一个 CPU 操作 **在单核** 上将八个 `i32` 的数组加到另一个八个 `i32` 的数组上。使用 SIMD 操作大大加快了某些任务的速度。如果你没有使用 SIMD，你可能没有充分利用你 CPU 的能力。

这篇文章是“另一个 Rust 和 SIMD”文章吗？是的，也不是。是的，我确实将 SIMD 应用于一个编程问题，然后觉得有必要写一篇文章。不是，我希望这篇文章也能深入到足以指导你完成*你的*项目。它解释了 Rust nightly 中新提供的 SIMD 功能和设置。它包括一个 Rust SIMD 速查表。它展示了如何在不离开安全 Rust 的情况下使你的 SIMD 代码通用。它让你开始使用如 Godbolt 和 Criterion 等工具。最后，它介绍了简化过程的新 cargo 命令。

`[range-set-blaze](https://crates.io/crates/range-set-blaze)` crate 使用其 `RangeSetBlaze::from_iter` 方法来处理可能很长的整数序列。当整数是“clumpy”时，它可以比 Rust 的标准 `HashSet::from_iter` [快 30 倍](https://github.com/CarlKCarlK/range-set-blaze/blob/main/docs/bench.md)。如果我们使用 SIMD 操作，能做到更好吗？是的！

> 查看 [此文档](https://docs.rs/range-set-blaze/latest/range_set_blaze/struct.RangeSetBlaze.html#constructor-performance) 了解“clumpy”的定义。此外，当整数不不规则时会发生什么？`RangeSetBlaze` 比 `HashSet` [慢 2 到 3 倍](https://github.com/CarlKCarlK/range-set-blaze/blob/main/docs/bench.md)。

对于不规则整数，`RangeSetBlaze::from_slice` — 基于 SIMD 操作的新方法 — 比 `RangeSetBlaze::from_iter` 快 7 倍。这使它比 `HashSet::from_iter` 快超过 200 倍。（当整数不不规则时，它仍然比 `HashSet` 慢 2 到 3 倍。）

在实现这一加速的过程中，我学到了九条规则，这些规则可以帮助你使用 SIMD 操作加速你的项目。

这些规则是：

1.  使用 nightly Rust 和 `core::simd`，Rust 的实验性标准 SIMD 模块。

1.  CCC: 检查、控制并选择你计算机的 SIMD 能力。

1.  学习 `core::simd`，但要有选择地。

1.  头脑风暴候选算法。

1.  使用 Godbolt 和 AI 来理解你代码的汇编，即使你不懂汇编语言。

1.  使用内联泛型（当这不起作用时）宏，（当宏不起作用时）特性，将其推广到所有类型和 LANES。

查看 [第 2 部分](/nine-rules-for-simd-acceleration-of-your-rust-code-part-2-6a104b3be6f3) 以获取这些规则：

*7\. 使用 Criterion 基准测试来选择算法，并发现 LANES 应该（几乎）始终为 32 或 64。*

*8\. 将您的最佳SIMD算法集成到您的项目中，并使用* `*as_simd*` *特别的代码处理* `*i128*` */* `*u128*` *，并额外进行上下文基准测试。*

*9\. 从项目中提取出您的最佳SIMD算法（目前）并选择一个可选的cargo特性。*

*旁注：为了避免含糊其辞，我称这些为“规则”，但它们当然只是建议。*

# 规则1：使用nightly Rust和 `core::simd`，Rust的实验性标准SIMD模块。

Rust可以通过稳定的 `[core::arch](https://doc.rust-lang.org/core/arch/index.html)` 模块或nightly的 `[core::simd](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html)` 模块访问SIMD操作。让我们比较一下它们：

`**core::arch**`

+   稳定

+   [“[这并非世界上最容易的事情](https://doc.rust-lang.org/core/arch/index.html#ergonomics)”]

+   为您的crate的下游用户提供高性能。例如，因为[regex](https://github.com/BurntSushi/regex)和[`memchr`](https://github.com/BurntSushi/memchr)采用了这种方法，超过100,000个其他crate免费获得了稳定的SIMD加速。[[Reddit讨论](https://www.reddit.com/r/rust/comments/18hj1m6/comment/kdbfktb/?utm_source=share&utm_medium=web2x&context=3)，[一些相关的](https://github.com/BurntSushi/memchr/blob/master/src/arch/x86_64/memchr.rs) [`memchr`](https://github.com/BurntSushi/memchr/blob/master/src/arch/x86_64/memchr.rs) [代码](https://github.com/BurntSushi/memchr/blob/master/src/arch/x86_64/memchr.rs)]

`**core::simd**`

+   Nightly

+   令人愉快的简单和可移植。

+   限制了向下游用户只能使用nightly版。

我决定选择“简单”。如果您决定选择更难的路线，首先从更简单的路径开始可能仍然是值得的。

无论哪种情况，在我们尝试在一个更大的项目中使用SIMD操作之前，让我们确保我们能够完全使用它们。以下是步骤：

首先，创建一个名为 `simd_hello` 的项目：

```py
cargo new simd_hello
cd simd_hello
```

编辑 `src/main.rs` 以包含 ([Rust playground](https://play.rust-lang.org/?version=nightly&mode=debug&edition=2021&gist=e39aa876c0abed9915d389fe73687839)):

```py
// Tell nightly Rust to enable 'portable_simd'
#![feature(portable_simd)]
use core::simd::prelude::*;

// constant Simd structs
const LANES: usize = 32;
const THIRTEENS: Simd<u8, LANES> = Simd::<u8, LANES>::from_array([13; LANES]);
const TWENTYSIXS: Simd<u8, LANES> = Simd::<u8, LANES>::from_array([26; LANES]);
const ZEES: Simd<u8, LANES> = Simd::<u8, LANES>::from_array([b'Z'; LANES]);

fn main() {
    // create a Simd struct from a slice of LANES bytes
    let mut data = Simd::<u8, LANES>::from_slice(b"URYYBJBEYQVQBUBCRVGFNYYTBVATJRYY");

    data += THIRTEENS; // add 13 to each byte

    // compare each byte to 'Z', where the byte is greater than 'Z', subtract 26
    let mask = data.simd_gt(ZEES); // compare each byte to 'Z'
    data = mask.select(data - TWENTYSIXS, data);

    let output = String::from_utf8_lossy(data.as_array());
    assert_eq!(output, "HELLOWORLDIDOHOPEITSALLGOINGWELL");
    println!("{}", output);
}
```

接下来 —— 全面的SIMD功能需要Rust的nightly版本。假设您已安装了Rust，请安装nightly版 (`rustup install nightly`)。确保您有最新的nightly版本 (`rustup update nightly`)。最后，设置此项目使用nightly版 (`rustup override set nightly`)。

您现在可以使用 `cargo run` 运行程序。该程序对32个大写字母的ROT13解密。通过SIMD，程序可以同时解密所有32个字节。

让我们看看程序的每个部分是如何工作的。它从以下开始：

```py
#![feature(portable_simd)]
use core::simd::prelude::*;
```

Rust nightly仅在请求时提供其额外的功能（或“特性”）。 `#![feature(portable_simd)]` 语句请求Rust nightly可用新的实验性 `core::simd` 模块。然后，`use` 语句导入了模块的最重要的类型和特征。

在代码的下一部分中，我们定义了一些有用的常量：

```py
const LANES: usize = 32;
const THIRTEENS: Simd<u8, LANES> = Simd::<u8, LANES>::from_array([13; LANES]);
const TWENTYSIXS: Simd<u8, LANES> = Simd::<u8, LANES>::from_array([26; LANES]);
const ZEES: Simd<u8, LANES> = Simd::<u8, LANES>::from_array([b'Z'; LANES]);
```

`Simd`结构体是一种特殊类型的Rust数组。（例如，它始终是内存对齐的。）常量`LANES`告诉了`Simd`数组的长度。`from_array`构造函数复制一个常规的Rust数组来创建一个`Simd`。在这种情况下，因为我们需要`const` `Simd`，所以我们构造的数组也必须是`const`。

接下来的两行将我们加密的文本复制到`data`，然后对每个字母添加13。

```py
let mut data = Simd::<u8, LANES>::from_slice(b"URYYBJBEYQVQBUBCRVGFNYYTBVATJRYY");
data += THIRTEENS;
```

如果您出错了，您的加密文本长度不正好为`LANES`（32）怎么办？遗憾的是，编译器不会告诉您。相反，在运行程序时，`from_slice`将会崩溃。如果加密文本包含非大写字母怎么办？在本示例程序中，我们将忽略这种可能性。

`+=`操作符在`Simd` `data`和`Simd` `THIRTEENS`之间进行逐元素加法。它将结果放入`data`中。请记住，常规Rust加法的调试构建会检查溢出。但SIMD不会这样做。Rust定义了SIMD算术运算符总是进行包装。类型为`u8`的值在255之后会包装。

巧合的是，Rot13解密也需要包装，但是在‘Z’之后而不是在255之后。这里有一种编码所需Rot13包装的方法。它从任何值中减去26，超出了‘Z’。

```py
let mask = data.simd_gt(ZEES);
data = mask.select(data - TWENTYSIXS, data);
```

这里要求找到逐个元素的超过‘Z’的位置。然后，从所有值中减去26。在感兴趣的位置，使用减去的值。在其他位置，使用原始值。从所有值中减去然后只使用一些看起来是不是浪费了？使用SIMD，这不需要额外的计算机时间并且避免了跳转。因此，这种策略是高效且常见的。

程序以此方式结束：

```py
let output = String::from_utf8_lossy(data.as_array());
assert_eq!(output, "HELLOWORLDIDOHOPEITSALLGOINGWELL");
println!("{}", output);
```

注意`.as_array()`方法。它安全地将`Simd`结构体转换为常规的Rust数组而不复制。

令我惊讶的是，这个程序在没有SIMD扩展的计算机上运行良好。Rust nightly将代码编译成常规（非SIMD）指令。但我们不仅仅想要运行“良好”，我们想要运行*更快*。这需要我们打开计算机的SIMD性能。

# 规则2：CCC：检查，控制和选择您计算机的SIMD能力。

要使SIMD程序在您的计算机上运行得更快，您必须首先发现您的计算机支持哪些SIMD扩展。如果您有Intel/AMD计算机，可以使用我的`[simd-detect](https://github.com/CarlKCarlK/cargo-simd-detect)` cargo命令。

运行：

```py
rustup override set nightly
cargo install cargo-simd-detect --force
cargo simd-detect
```

在我的计算机上，输出如下：

```py
extension       width                   available       enabled
sse2            128-bit/16-bytes        true            true
avx2            256-bit/32-bytes        true            false
avx512f         512-bit/64-bytes        true            false
```

这说明我的计算机支持`sse2`，`avx2`和`avx512f` SIMD扩展。在其中，默认情况下，Rust启用了普遍存在已有二十年历史的`sse2`扩展。

SIMD扩展形成一个层次结构，`avx512f`在`avx2`之上，在`sse2`之上。启用更高级别的扩展也会启用较低级别的扩展。

大多数Intel/AMD计算机也支持十年历史的`avx2`扩展。您可以通过设置环境变量来启用它：

```py
# For Windows Command Prompt
set RUSTFLAGS=-C target-feature=+avx2

# For Unix-like shells (like Bash)
export RUSTFLAGS="-C target-feature=+avx2"
```

“强制安装”并再次运行`simd-detect`，您应该看到启用了`avx2`。

```py
# Force install every time to see changes to 'enabled'
cargo install cargo-simd-detect --force
cargo simd-detect
```

```py
extension         width                   available       enabled
sse2            128-bit/16-bytes        true            true
avx2            256-bit/32-bytes        true            true
avx512f         512-bit/64-bytes        true            false
```

或者，你可以打开你的机器支持的每一个 SIMD 扩展：

```py
# For Windows Command Prompt
set RUSTFLAGS=-C target-cpu=native

# For Unix-like shells (like Bash)
export RUSTFLAGS="-C target-cpu=native"
```

在我的机器上，这启用了 `avx512f`，这是一种新的 SIMD 扩展，由一些英特尔计算机和少数 AMD 计算机支持。

你可以将 SIMD 扩展设置回它们的默认值（在英特尔/AMD 上是 `sse2`）：

```py
# For Windows Command Prompt
set RUSTFLAGS=

# For Unix-like shells (like Bash)
unset RUSTFLAGS
```

你可能会想知道为什么 `target-cpu=native` 不是 Rust 的默认值。问题在于使用 `avx2` 或 `avx512f` 创建的二进制文件不能在缺少这些 SIMD 扩展的计算机上运行。因此，如果只为自己使用编译，请使用 `target-cpu=native`。然而，如果为其他人编译，请慎重选择 SIMD 扩展，并告知人们你所假设的 SIMD 扩展级别。

令人高兴的是，无论你选择哪种 SIMD 扩展级别，Rust 的 SIMD 支持都非常灵活，你可以轻松更改你的决策。接下来让我们详细了解在 Rust 中使用 SIMD 编程的细节。

# 规则 3：学习 `core::simd`，但要有选择性。

要使用 Rust 的新 `[core::simd](https://doc.rust-lang.org/nightly/core/simd/index.html)` 模块，你应该学习选择的构建模块。这里有一个[速查表](https://github.com/CarlKCarlK/range-set-blaze/blob/nov23/examples/simd/rust_simd_cheatsheet.md)，包含我发现最有用的结构体、方法等。每个项目都包含到其[文档](https://doc.rust-lang.org/nightly/core/simd/index.html)的链接。

## 结构体

+   `[Simd](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html)` - 一个特殊的、对齐的、固定长度的数组，由`[SimdElement](https://doc.rust-lang.org/std/simd/trait.SimdElement.html)`组成。我们将数组中的位置及其存储的元素称为“lane”。默认情况下，我们复制 `Simd` 结构体而不是引用它们。

+   `[Mask](https://doc.rust-lang.org/nightly/core/simd/struct.Mask.html)` - 一种特殊的布尔数组，显示每个 lane 的包含/排除情况。

## SimdElements

+   浮点类型：`f32`、`f64`

+   整数类型：`i8`、`u8`、`i16`、`u16`、`i32`、`u32`、`i64`、`u64`、`isize`、`usize`

+   — [*但不包括*](https://github.com/rust-lang/portable-simd/issues/108) `[*i128*](https://github.com/rust-lang/portable-simd/issues/108)`[*,*](https://github.com/rust-lang/portable-simd/issues/108) `[*u128*](https://github.com/rust-lang/portable-simd/issues/108)`

## `**Simd**` **构造函数**

+   `[Simd::from_array](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.from_array)` - 通过复制固定长度数组创建一个 `Simd` 结构体。

+   `[Simd::from_slice](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.from_slice)` - 通过复制切片的前 `LANE` 个元素创建一个 `Simd<T,LANE>` 结构体。

+   `[Simd::splat](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.splat)` - 将单个值复制到 `Simd` 结构的所有 lane 中。

+   `[slice::as_simd](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.to_simd)` - 安全地将常规切片转换为对齐的 `Simd` 切片（加上不对齐的剩余部分），而不进行复制。

## `**Simd**` **转换**

+   `[Simd::as_array](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.as_array)` - 在不复制的情况下，将`Simd`结构体安全地转换为普通数组引用。

## **Simd** **方法和运算符**

+   `[simd[i]](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.index)` - 从`Simd`的一个通道中提取一个值。

+   `[simd + simd](https://doc.rust-lang.org/core/simd/struct.Simd.html#impl-Add%3C%26'rhs+Simd%3CT,+LANES%3E%3E-for-%26'lhs+Simd%3CT,+LANES%3E)` - 执行两个`Simd`结构体的元素级加法。同时支持`-`、`*`、`/`、`%`、余数、按位与、按位或、异或、按位非、位移。

+   `[simd += simd](https://doc.rust-lang.org/core/simd/struct.Simd.html#impl-AddAssign%3CU%3E-for-Simd%3CT,+LANES%3E)` - 将另一个`Simd`结构体加到当前结构体上，进行就地操作。其他运算符也受支持。

+   `[Simd::simd_gt](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.simd_gt)` - 比较两个`Simd`结构体，返回一个`Mask`，指示第一个结构体的哪些元素大于第二个结构体的元素。同时支持`simd_lt`、`simd_le`、`simd_ge`、`simd_lt`、`simd_eq`、`simd_ne`。

+   `[Simd::rotate_elements_left](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.rotate_elements_left)` - 将`Simd`结构体的元素向左旋转指定的数量。同时支持`rotate_elements_right`。

+   `[simd_swizzle!(simd, indexes)](https://doc.rust-lang.org/std/simd/prelude/macro.simd_swizzle.html)` - 根据指定的常量索引重新排列`Simd`结构体的元素。

+   `[simd == simd](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#impl-Eq-for-Simd%3CT,+N%3E)` - 检查两个`Simd`结构体之间的相等性，返回一个普通的`bool`结果。

+   `[Simd::reduce_and](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.reduce_and)` - 执行`Simd`结构体所有通道的按位与归约。同时支持：`reduce_or`、`reduce_xor`、`reduce_max`、`reduce_min`、`reduce_sum`（但不支持`reduce_eq`）。

## **掩码** **方法和运算符**

+   `[Mask::select](https://doc.rust-lang.org/nightly/core/simd/struct.Mask.html#method.select)` - 根据掩码从两个`Simd`结构体中选择元素。

+   `[Mask::all](https://doc.rust-lang.org/nightly/core/simd/struct.Mask.html#method.all)` - 指示掩码是否全为`true`。

+   `[Mask::any](https://doc.rust-lang.org/nightly/core/simd/struct.Mask.html#method.all)` - 指示掩码是否包含任何`true`。

## **关于通道的一切**

+   `[Simd::LANES](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#associatedconstant.LANES)` - 一个常量，表示`Simd`结构体中的元素（通道）数量。

+   `[SupportedLaneCount](https://doc.rust-lang.org/nightly/core/simd/trait.SupportedLaneCount.html)` - 指示允许的`LANES`值。通过泛型使用。

+   `[simd.lanes](https://doc.rust-lang.org/core/simd/struct.Simd.html#method.lanes)` - 常量方法，告诉`Simd`结构体的通道数量。

## **低级对齐、偏移量等**

*尽可能使用* `[*to_simd*](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.to_simd)` *代替。*

+   `[mem::size_of](https://doc.rust-lang.org/std/mem/fn.size_of.html)`，`[mem::align_of](https://doc.rust-lang.org/std/mem/fn.align_of.html)`，`[mem::align_to](https://doc.rust-lang.org/std/mem/fn.align_to.html)`，`[intrinsics::offset](https://doc.rust-lang.org/std/intrinsics/fn.offset.html)`，`[pointer::read_unaligned](https://doc.rust-lang.org/std/primitive.pointer.html#method.read_unaligned)`（不安全），`[pointer::write_unaligned](https://doc.rust-lang.org/std/primitive.pointer.html#method.write_unaligned)`（不安全），`[mem::transmute](https://doc.rust-lang.org/std/mem/fn.transmute.html)`（不安全，const）

## **更多，也许感兴趣的**

+   `[deinterleave](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.deinterleave)`，`[gather_or](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.gather_or)`，`[reverse](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.reverse)`，`[scatter](https://doc.rust-lang.org/nightly/core/simd/struct.Simd.html#method.scatter)`

有了这些构建模块，现在是时候创造一些东西了。

# 规则 4：头脑风暴候选算法。

你想加速什么？你事先不会知道哪种 SIMD 方法（如果有的话）最好。因此，你应该创建许多算法，然后分析（规则 5）和基准测试（规则 7）它们。

我希望加速 `[range-set-blaze](https://crates.io/crates/range-set-blaze)`，一个用于操作“clumpy”整数集的 crate。我希望创建 `is_consecutive`，一个用于检测连续整数块的函数，会很有用。

> **背景：** Crate `*range-set-blaze*` *用于处理“clumpy”整数。这里的“clumpy”意味着用于表示数据的范围数量与输入整数的数量相比较少。例如，这些 1002 个输入整数
> 
> `100, 101,` ..., `489, 499, 501, 502,` ..., `998, 999, 999, 100, 0`
> 
> 最终变成三个 Rust 范围：
> 
> `0..=0, 100..=499, 501..=999`。
> 
> （在内部，`[*RangeSetBlaze*](https://docs.rs/range-set-blaze/latest/range_set_blaze/struct.RangeSetBlaze.html#)` 结构将整数集表示为存储在高效缓存 BTreeMap 中的排序不相交范围列表。）
> 
> 尽管允许输入整数是无序和冗余的，但我们期望它们通常是“好的”。RangeSetBlaze 的 `from_iter` 构造函数已经利用这一期望通过组合相邻整数来分组。例如，`from_iter` 首先将这 1002 个输入整数转换为四个范围
> 
> `*100..=499, 501..=999, 100..=100, 0..=0.*`
> 
> 在最小的恒定内存使用下，独立于输入大小。然后，它对这些减少的范围进行排序和合并。
> 
> 我想知道是否可以通过快速找到（一些）连续整数来加速从类似数组的输入构建的 `from_slice` 方法。例如，是否可以在最小的恒定内存下，将 1002 个输入整数 *转换为五个 Rust 范围：*
> 
> `*100..=499, 501..=999, 999..=999, 100..=100, 0..=0.*`
> 
> *如果是这样，* `*from_iter*` *可以快速完成处理。*

让我们先用常规 Rust 编写 `is_consecutive`：

```py
pub const LANES: usize = 16;
pub fn is_consecutive_regular(chunk: &[u32; LANES]) -> bool {
    for i in 1..LANES {
        if chunk[i - 1].checked_add(1) != Some(chunk[i]) {
            return false;
        }
    }
    true
}
```

算法只是顺序遍历数组，检查每个值是否比前一个值多 1。它还避免了溢出。

遍历这些项似乎很简单，我不确定 SIMD 是否能做得更好。这是我的第一次尝试：

## Splat0

```py
use std::simd::prelude::*;

const COMPARISON_VALUE_SPLAT0: Simd<u32, LANES> =
    Simd::from_array([15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]);

pub fn is_consecutive_splat0(chunk: Simd<u32, LANES>) -> bool {
    if chunk[0].overflowing_add(LANES as u32 - 1) != (chunk[LANES - 1], false) {
        return false;
    }
    let added = chunk + COMPARISON_VALUE_SPLAT0;
    Simd::splat(added[0]) == added
}
```

这里是它的计算概要：

![](../Images/420908b8497341c078410b9436eaa6d1.png)

来源：这张图及所有后续图片均由作者提供。

它首先（不必要地）检查第一个和最后一个项目是否相隔 15。然后，它通过将 15 加到第 0 项，将 14 加到下一个项，以此类推来创建 `added`。最后，为了查看 `added` 中的所有项是否相同，它基于 `added` 的第 0 项创建一个新的 `Simd`，然后进行比较。请记住，`splat` 从一个值创建一个 `Simd` 结构。

## Splat1 & Splat2

当我向 Ben Lichtman 提到 `is_consecutive` 问题时，他独立地提出了这个，即 Splat1：

```py
const COMPARISON_VALUE_SPLAT1: Simd<u32, LANES> =
    Simd::from_array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]);

pub fn is_consecutive_splat1(chunk: Simd<u32, LANES>) -> bool {
    let subtracted = chunk - COMPARISON_VALUE_SPLAT1;
    Simd::splat(chunk[0]) == subtracted
}
```

Splat1 从 `chunk` 中减去比较值，并检查结果是否与 `chunk` 的第一个元素相同，经过 splat。

![](../Images/074c54b0d26c8842f648e64d7246ad79.png)

他还提出了一个变体，称为 Splat2，它 splat `subtracted` 的第一个元素，而不是 `chunk`。这似乎可以避免一次内存访问。

我相信你一定在想这些方法中哪一个最好，但在我们讨论这个问题之前，让我们再看两个候选者。

## Swizzle

Swizzle 类似于 Splat2，但使用 `simd_swizzle!` 而不是 `splat`。宏 `simd_swizzle!` 通过根据索引数组重新排列旧 `Simd` 的通道来创建一个新的 `Simd`。

```py
pub fn is_consecutive_sizzle(chunk: Simd<u32, LANES>) -> bool {
    let subtracted = chunk - COMPARISON_VALUE_SPLAT1;
    simd_swizzle!(subtracted, [0; LANES]) == subtracted
}
```

## Rotate

这个方法不同。我对它寄予厚望。

```py
const COMPARISON_VALUE_ROTATE: Simd<u32, LANES> =
    Simd::from_array([4294967281, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]);

pub fn is_consecutive_rotate(chunk: Simd<u32, LANES>) -> bool {
    let rotated = chunk.rotate_elements_right::<1>();
    chunk - rotated == COMPARISON_VALUE_ROTATE
}
```

这个想法是将所有元素向右旋转一个位置。然后，我们从 `rotated` 中减去原始的 `chunk`。如果输入是连续的，结果应该是“−15”后跟所有 1。 （使用包装减法，-15 是 `4294967281u32`。）

![](../Images/9d8b9ae021da7778af8e11f6ed860b16.png)

现在我们有了候选者，让我们开始评估它们。

# 规则 5：使用 Godbolt 和 AI 来理解你的代码的汇编语言，即使你不知道汇编语言。

我们将通过两种方式评估这些候选者。首先，在这个规则中，我们将查看从代码生成的汇编语言。其次，在规则 7 中，我们将基准测试代码的速度。

> 如果你不知道汇编语言，也不要担心，你仍然可以从中获得一些信息。

查看生成的汇编语言的最简单方法是使用 [Compiler Explorer, AKA Godbolt](https://godbolt.org/z/j5GdGah89)。它在不使用外部 crate 的简短代码片段上效果最佳。它看起来像这样：

![](../Images/1013810e2972f3eb9449156e4404a98f.png)

参考上图中的数字，按照以下步骤使用 Godbolt：

1.  使用你的网页浏览器打开 [godbolt.org](https://godbolt.org/z/odrPv5WcG)。

1.  添加一个新的源编辑器。

1.  选择 Rust 作为你的语言。

1.  粘贴感兴趣的代码。将感兴趣的函数设为公共（`pub fn`）。不包括主函数或不需要的函数。该工具不支持外部包（external crates）。

1.  添加新的编译器。

1.  将编译器版本设置为nightly。

1.  设置选项（暂时）为`-C opt-level=3 -C target-feature=+avx512f.`

1.  如果有错误，请查看输出。

1.  如果您想分享或保存工具的状态，请点击“分享”。

从上面的图像可以看出，Splat2和Sizzle完全相同，因此我们可以将Sizzle从考虑中删除。如果您[打开我的Godbolt会话的副本](https://godbolt.org/z/j5GdGah89)，您还会看到大多数函数编译为大致相同数量的汇编操作。例外是Regular ——它更长——和Splat0——它包括早期检查。

在汇编中，512位寄存器以ZMM开头。256位寄存器以YMM开头。128位寄存器以XMM开头。如果您想更好地理解生成的汇编，请使用AI工具生成注释。例如，我在这里向[Bing Chat](https://www.bing.com/search?q=Bing+AI&showconv=1&FORM=hpcodx)询问关于Splat2的问题：

![](../Images/813f956f0e22302edb254812fa60f303.png)

尝试不同的编译器设置，包括`-C target-feature=+avx2`，然后完全不使用`target-feature`。

较少的汇编操作不一定意味着更快的速度。然而，查看汇编代码确实让我们确认编译器至少尝试使用SIMD操作、内联常量引用等。同样，像Splat1和Swizzle一样，有时它可以让我们知道两个候选项何时相同。

> 您可能需要比Godbolt提供的反汇编功能更多的功能，例如处理使用外部包的代码能力。B3NNY推荐给我cargo工具 `[cargo-show-asm](https://github.com/pacak/cargo-show-asm)`。我试过了，发现使用起来相当容易。

`range-set-blaze`包必须处理超出`u32`的整数类型。此外，我们必须选择一定数量的LANES，但我们没有理由认为16 LANES总是最好的。为了满足这些需求，在下一条规则中我们将概括代码。

# 规则 6：广义应用于所有类型和LANES，包括内联泛型（in-lined generics），（当它不起作用时）宏（macros），以及（当它不起作用时）特性（traits）。

让我们首先用泛型概括Splat1。

```py
#[inline]
pub fn is_consecutive_splat1_gen<T, const N: usize>(
    chunk: Simd<T, N>,
    comparison_value: Simd<T, N>,
) -> bool
where
    T: SimdElement + PartialEq,
    Simd<T, N>: Sub<Simd<T, N>, Output = Simd<T, N>>,
    LaneCount<N>: SupportedLaneCount,
{
    let subtracted = chunk - comparison_value;
    Simd::splat(chunk[0]) == subtracted
}
```

首先注意`#[inline]`属性。对效率很重要，我们将几乎在所有这些小函数上使用它。

上面定义的函数`is_consecutive_splat1_gen`看起来很棒，除了它需要第二个输入，称为`comparison_value`，我们尚未定义。

> 如果您不需要通用常量`comparison_value`，我羡慕您。如果您愿意，您可以跳过下一条规则。同样地，如果您正在未来阅读此内容，并且创建通用常量`comparison_value`就像您个人机器人做家务一样轻松，那我就双倍羡慕您。

我们可以尝试创建一个`comparison_value_splat_gen`，它是通用的和const的。不幸的是，`From<usize>`和替代的`T::One`都不是const，所以这个方法行不通：

```py
// DOESN'T WORK BECAUSE From<usize> is not const
pub const fn comparison_value_splat_gen<T, const N: usize>() -> Simd<T, N>
where
    T: SimdElement + Default + From<usize> + AddAssign,
    LaneCount<N>: SupportedLaneCount,
{
    let mut arr: [T; N] = [T::from(0usize); N];
    let mut i_usize = 0;
    while i_usize < N {
        arr[i_usize] = T::from(i_usize);
        i_usize += 1;
    }
    Simd::from_array(arr)
}
```

**宏是无赖的最后避难所**。因此，让我们使用宏：

```py
#[macro_export]
macro_rules! define_is_consecutive_splat1 {
    ($function:ident, $type:ty) => {
        #[inline]
        pub fn $function<const N: usize>(chunk: Simd<$type, N>) -> bool
        where
            LaneCount<N>: SupportedLaneCount,
        {
            define_comparison_value_splat!(comparison_value_splat, $type);

            let subtracted = chunk - comparison_value_splat();
            Simd::splat(chunk[0]) == subtracted
        }
    };
}
#[macro_export]
macro_rules! define_comparison_value_splat {
    ($function:ident, $type:ty) => {
        pub const fn $function<const N: usize>() -> Simd<$type, N>
        where
            LaneCount<N>: SupportedLaneCount,
        {
            let mut arr: [$type; N] = [0; N];
            let mut i = 0;
            while i < N {
                arr[i] = i as $type;
                i += 1;
            }
            Simd::from_array(arr)
        }
    };
}
```

这使我们能够在任何特定元素类型和所有LANES上运行（[Rust Playground](https://play.rust-lang.org/?version=nightly&mode=debug&edition=2021&gist=f5a6fbac31d64f3ae79440d5613e44ec)）：

```py
define_is_consecutive_splat1!(is_consecutive_splat1_i32, i32);

let a: Simd<i32, 16> = black_box(Simd::from_array(array::from_fn(|i| 100 + i as i32)));
let ninety_nines: Simd<i32, 16> = black_box(Simd::from_array([99; 16]));
assert!(is_consecutive_splat1_i32(a));
assert!(!is_consecutive_splat1_i32(ninety_nines));
```

遗憾的是，对于`range-set-blaze`来说还不够。它需要在*所有*元素类型（而不仅仅是一种）和（理想情况下）所有LANES（而不仅仅是一个LANE）上运行。

幸运的是，有一个解决方法，再次依赖于宏。它还利用了我们只需要支持有限类型列表的事实，即：`i8`、`i16`、`i32`、`i64`、`isize`、`u8`、`u16`、`u32`、`u64`和`usize`。如果您需要同时（或者替代地）支持`f32`和`f64`，那也没问题。

> 另一方面，如果您需要支持`i128`和`u128`，那可能就没有办法了。`core::simd`模块不支持它们。在第8条规则中，我们将看到`range-set-blaze`如何通过牺牲性能来解决这个问题。

这个解决方法定义了一个新的trait，这里称为`IsConsecutive`。然后，我们使用一个宏（调用一个宏，再调用一个宏）来在这10种感兴趣的类型上实现这个trait。

```py
pub trait IsConsecutive {
    fn is_consecutive<const N: usize>(chunk: Simd<Self, N>) -> bool
    where
        Self: SimdElement,
        Simd<Self, N>: Sub<Simd<Self, N>, Output = Simd<Self, N>>,
        LaneCount<N>: SupportedLaneCount;
}

macro_rules! impl_is_consecutive {
    ($type:ty) => {
        impl IsConsecutive for $type {
            #[inline] // very important
            fn is_consecutive<const N: usize>(chunk: Simd<Self, N>) -> bool
            where
                Self: SimdElement,
                Simd<Self, N>: Sub<Simd<Self, N>, Output = Simd<Self, N>>,
                LaneCount<N>: SupportedLaneCount,
            {
                define_is_consecutive_splat1!(is_consecutive_splat1, $type);
                is_consecutive_splat1(chunk)
            }
        }
    };
}

impl_is_consecutive!(i8);
impl_is_consecutive!(i16);
impl_is_consecutive!(i32);
impl_is_consecutive!(i64);
impl_is_consecutive!(isize);
impl_is_consecutive!(u8);
impl_is_consecutive!(u16);
impl_is_consecutive!(u32);
impl_is_consecutive!(u64);
impl_is_consecutive!(usize);
```

现在我们可以调用完全通用的代码（Rust Playground）：

```py
// Works on i32 and 16 lanes
let a: Simd<i32, 16> = black_box(Simd::from_array(array::from_fn(|i| 100 + i as i32)));
let ninety_nines: Simd<i32, 16> = black_box(Simd::from_array([99; 16]));

assert!(IsConsecutive::is_consecutive(a));
assert!(!IsConsecutive::is_consecutive(ninety_nines));

// Works on i8 and 64 lanes
let a: Simd<i8, 64> = black_box(Simd::from_array(array::from_fn(|i| 10 + i as i8)));
let ninety_nines: Simd<i8, 64> = black_box(Simd::from_array([99; 64]));

assert!(IsConsecutive::is_consecutive(a));
assert!(!IsConsecutive::is_consecutive(ninety_nines));
```

使用这种技术，我们可以创建多个完全通用于类型和LANES的候选算法。接下来，是时候进行基准测试，看看哪些算法最快。

这些是向Rust添加SIMD代码的前六条规则。在[第2部分](/nine-rules-for-simd-acceleration-of-your-rust-code-part-2-6a104b3be6f3)中，我们将看到第7到第9条规则。这些规则将涵盖如何选择算法和设置LANES，以及如何将SIMD操作集成到现有代码中（重要的是），如何使其可选。第2部分结束时将讨论何时/如果应该使用SIMD以及改进Rust的SIMD体验的想法。我希望能在[那里](/nine-rules-for-simd-acceleration-of-your-rust-code-part-2-6a104b3be6f3)见到你。

*请* [*关注Carl在Medium上的文章*](https://medium.com/@carlmkadie)*。我写关于Rust和Python中的科学编程，机器学习和统计学的文章。我倾向于每个月写一篇文章。*
