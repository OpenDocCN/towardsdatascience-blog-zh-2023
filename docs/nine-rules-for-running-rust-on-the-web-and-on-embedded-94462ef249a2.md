# 在网络和嵌入式系统上运行 Rust 的九条规则

> 原文：[https://towardsdatascience.com/nine-rules-for-running-rust-on-the-web-and-on-embedded-94462ef249a2?source=collection_archive---------3-----------------------#2023-07-05](https://towardsdatascience.com/nine-rules-for-running-rust-on-the-web-and-on-embedded-94462ef249a2?source=collection_archive---------3-----------------------#2023-07-05)

## 从将 `range-set-blaze` 移植到 no_std 和 WASM 的实际经验

[](https://medium.com/@carlmkadie?source=post_page-----94462ef249a2--------------------------------)[![Carl M. Kadie](../Images/9dbe27c76e9567136e5a7dc587f1fb15.png)](https://medium.com/@carlmkadie?source=post_page-----94462ef249a2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94462ef249a2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----94462ef249a2--------------------------------) [Carl M. Kadie](https://medium.com/@carlmkadie?source=post_page-----94462ef249a2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5e87027005f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-running-rust-on-the-web-and-on-embedded-94462ef249a2&user=Carl+M.+Kadie&userId=a5e87027005f&source=post_page-a5e87027005f----94462ef249a2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----94462ef249a2--------------------------------) ·17 分钟阅读·2023年7月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F94462ef249a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-running-rust-on-the-web-and-on-embedded-94462ef249a2&user=Carl+M.+Kadie&userId=a5e87027005f&source=-----94462ef249a2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F94462ef249a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnine-rules-for-running-rust-on-the-web-and-on-embedded-94462ef249a2&source=-----94462ef249a2---------------------bookmark_footer-----------)![](../Images/59da1d54d5821d92e578ab92da9a5abb.png)

微控制器上的螃蟹 — 来源：[https://openai.com/dall-e-2/](https://openai.com/dall-e-2/)

我推荐使用 Rust，当你需要 C++ 的速度和 Python 的内存安全时。此外，使用 Rust 你可以构建在超过 100,000 个 [软件库](https://crates.io/) 上。除此之外，Rust 还提供了将你的代码运行在不仅是传统计算机上，还有网页甚至机器人上的潜力。

然而，“几乎所有地方”运行会带来复杂性。本文是为那些希望减轻这些复杂性的Rust程序员准备的。（它也可能对那些想了解Rust的“几乎所有地方”运行故事的人感兴趣。）

第一个复杂性：网页和机器人的嵌入式处理器不支持通用文件IO。如果你的项目主要涉及读写文件，它不适合在机器人、其他嵌入式处理器或网页上运行。

第二个复杂性：将代码移植到几乎所有地方需要多个步骤和选择。导航这些选择可能会耗时。遗漏一步可能导致失败。本文旨在通过提供这九条规则来减少第二个复杂性，稍后我们将详细探讨这些规则：

1.  将你的lib.rs或main.rs标记为no_std。

1.  如果可能的话，使用内置的“crate alloc”。

1.  切换到“no std”依赖项。

1.  创建std和alloc功能，并将你的std-only函数设为可选。

1.  为WASM构建你的项目。使用cargo tree来使其正常工作。

1.  创建WASM测试和WASM演示。

1.  [可选] 为嵌入式设备构建你的项目。

1.  [可选] 创建一个嵌入式测试和嵌入式演示。

1.  完成CI测试、Cargo.toml元数据和更新的README.md。

遵循这些规则将帮助你创建运行在从PC到智能手机网页（[demo](https://carlkcarlk.github.io/range-set-blaze/wasm-demo/)）到机器人等所有地方的非常快速且内存安全的代码。代码可以非常小，并且可以利用庞大的Rust crate库。

为了说明这些规则，我们将把`[range-set-blaze](https://github.com/CarlKCarlK/range-set-blaze)` crate移植到网页——WASM——和微控制器——嵌入式。 （这个crate操作“块状”整数的集合。这个crate的用户请求了这个移植。）

移植到WASM和嵌入式要求你避免使用Rust的标准库“std”。转换到“no std”比我预期的既容易又困难。容易是因为你仍然可以使用`Vec`和`String`。困难主要是因为测试。基于我在`range-set-blaze`上的经验，以下是我推荐的决策，逐一描述。为了避免优柔寡断，我将这些建议表达为规则。

# 规则1：将你的lib.rs或main.rs标记为no_std。

> 附注1：首先，使用Git为你的项目创建一个新分支。这样，如果出现问题，你可以轻松地撤销所有更改。
> 
> 附注2：*实际上*，[WASM部分支持std](https://github.com/paritytech/substrate/issues/4043)。例如，它支持`vec`、`String`和`HashSet`。它不支持文件IO。如果你只需要WASM支持，你可能可以跳过本文中的所有“no_std”工作。
> 
> 附注 3：来自[Reddit](https://www.reddit.com/r/rust/comments/14rc70l/)的一个提示，我还没有测试过：“你真的不需要为Wasm设置任何`no_std`。如果你使用诸如`wasm-opt`或`wasm-gc`，甚至只是一个完整的集成管道与`trunk`，你不会看到二进制文件大小的差异，因为任何你没有使用的东西都会被剥离。无需设置`no_std`并寻找`no_std`依赖项。”

在`lib.rs`的顶部标记为：

`#![cfg_attr(not(test), no_std)]`

这告诉Rust编译器除非在测试时，否则不要包含标准库。

> 附注 1：我的项目是一个具有`lib.rs`的库项目。我认为具有`main.rs`的二进制项目的步骤大致相同，但我还没有测试过。
> 
> 附注 2：我们将在后面的规则中详细讨论代码测试。

在`range-set-blaze`的`lib.rs`中添加“no_std”行，会导致40个编译器问题，大多数问题形式如下：

![](../Images/cd34403675a277899925aa81efd0278e.png)

通过将主代码中的“std::”更改为“core::”来修复其中的一些问题（不包括测试代码）。对于`range-set-blaze`，这将问题数量从40个减少到12个。这个修复很有效，因为许多项，如`std::cmp::max`，在`core::cmp::max`中也可以找到。

可悲的是，像`Vec`和`Box`这样的项不能在`core`中，因为它们需要分配内存。幸运的是，如果你愿意支持内存分配，你仍然可以使用它们。

# 规则 2：如果可以的话，使用内置的“crate alloc”。

你是否应该允许你的crate分配内存？对于WASM，你应该允许。对于许多嵌入式应用程序，你也应该允许。然而，对于一些嵌入式应用程序，你不应该允许。如果你决定允许内存分配，那么在`lib.rs`的顶部添加：

`extern crate alloc;`

你现在可以添加这样的行，以获取许多内存分配的项：

```py
extern crate alloc;

use alloc::boxed::Box;
use alloc::collections::btree_map;
use alloc::collections::BTreeMap;
use alloc::vec::Vec;
use alloc::{format, string::String};
use alloc::vec;
```

使用`range-set-blaze`，这将问题数量从12个减少到两个。我们将在规则 3 中解决这些问题。

> 附注：如果你在编写一个不能使用内存分配的嵌入式环境，并且遇到了例如`Vec`的问题，你可以尝试重写。例如，你可以尝试用数组替代向量。如果这样不行，可以查看其他规则。如果都不行，你可能无法将你的crate移植到`no_std`。

# 规则 3：切换到“no std”依赖项。

如果你的项目使用了将“std”函数引入你代码中的crate，Rust编译器会发出警告。有时，你可以搜索[crates.io](https://crates.io/search?q=thiserror+no_std)并找到替代的“no_std” crate。例如，流行的`thiserror` crate会将“std”注入你的代码。然而，社区已经创建了[不含](https://crates.io/search?q=thiserror+no_std)“std”的替代品。

对于 `range-set-blaze`，剩下的两个问题与 crate `[gen_ops](https://crates.io/crates/gen_ops)` 相关——这是一个方便地定义操作符如 “+” 和 “&” 的绝妙 crate。`gen_ops` 的版本 0.3.0 未完全支持 “no std”。然而，版本 0.4.0 支持。我更新了 `Cargo.toml` 中的依赖项，并改进了我的“no std”兼容性。

我现在可以运行这些命令：

```py
cargo check # check that compiles as no_std
cargo test # check that tests, using std, still pass
```

命令 `cargo check` 确认了我的 crate 并没有直接使用标准库。命令 `cargo test` 确认了我的测试（仍使用标准库）继续通过。如果你的 crate 仍然不能编译，请查看下一个规则。

# 规则 4：创建 std 和 alloc 特性，并使你的 std-only 函数可选。

嵌入式处理器通常不支持读取和写入文件。同样，WASM 也尚未完全支持文件。虽然你可以找到一些与文件相关的“no std” crates，但似乎没有一个是全面的。因此，如果文件 IO 是你的 crate 的核心，移植到 WASM 和嵌入式可能不切实际。

然而，如果文件 IO —— 或任何其他 std-only 函数 —— 只是你的 crate 的附带功能，你可以通过“std”特性使该函数可选。方法如下：

将以下部分添加到你的 `Cargo.toml`：

```py
[package]
#...
resolver = "2" # the default for Rust 2021+

[features]
default = ["std"]
std = []
alloc = []
```

这表示你的 crate 现在有两个特性，“std”和“alloc”。默认情况下，编译器应使用“std”。

在你的 `lib.rs` 顶部，替换：

```py
#![cfg_attr(not(test), no_std)]
```

替换为：

```py
#![cfg_attr(not(feature = "std"), no_std)]
```

这表示如果你不应用“std”特性，编译器应在没有标准库的情况下进行编译。

在任何 std-only 代码之前的行中，添加 `#[cfg(feature = "std")]`。例如，这里我们定义了一个基于文件内容创建 `RangeSetBlaze` 结构体的函数：

```py
#[cfg(feature = "std")]
use std::{
    fs::File,
    io::{self, BufRead, BufReader},
    path::Path,
};

#[cfg(feature = "std")]
#[allow(missing_docs)]
pub fn demo_read_ranges_from_file<P, T>(path: P) -> io::Result<RangeSetBlaze<T>>
where
    P: AsRef<Path>,
    T: FromStr + Integer,
{
 //...code not shown
}
```

要检查 “std” 和 “alloc” 特性，请执行以下操作：

```py
cargo check # std
cargo check --features alloc --no-default-features
```

我们可以用以下方式测试“std”：

```py
cargo test
```

> 附注：令人惊讶的是，`cargo test --features alloc --no-default-features` 不会测试 "alloc"。这是因为测试 [需要线程、分配和其他可能在 `no_std` 中不可用的东西](https://github.com/rust-lang/wg-cargo-std-aware/issues/72)，因此 cargo 总是将常规测试作为“std”运行。

在这个阶段，我们检查了“std”和“alloc”，所以我们可以假设我们的库将与 WASM 和嵌入式兼容吗？不！一般来说，**没有经过测试的东西都无法正常工作**。具体来说，我们可能依赖于内部使用“std”代码的 crates。为了发现这些问题，我们必须在 WASM 和嵌入式环境中进行测试。

# 规则 5：为 WASM 构建你的项目。使用 `cargo tree` 来使其正常工作。

安装 WASM 交叉编译器，并用以下命令检查你的项目：

```py
rustup target add wasm32-unknown-unknown # only need to do this once
# may find issues
cargo check --target wasm32-unknown-unknown --features alloc --no-default-features
```

当我在 `range-set-blaze` 上执行此操作时，它抱怨 `getrandom` crate 与 WASM 不兼容。一方面，我不惊讶 WASM 不完全支持随机数。另一方面，我感到惊讶，因为我的项目并不*直接*依赖于 `getrandom`。为了找出*间接*依赖，我使用 `cargo tree`。我发现我的项目依赖于 crate `rand`，而 `rand` 依赖于 `getrandom`。以下是使用的 `cargo tree` 命令：

```py
cargo tree --edges no-dev --format "{p} {f}" --features alloc --no-default-features
```

该命令输出所有的 crate 及其使用的特性：

```py
range-set-blaze v0.1.6 (O:\Projects\Science\wasmetc\wasm3) alloc
├── gen_ops v0.4.0
├── itertools v0.10.5 default,use_alloc,use_std
│   └── either v1.8.1 use_std
├── num-integer v0.1.45 default,std
│   └── num-traits v0.2.15 default,std
│       [build-dependencies]
│       └── autocfg v1.1.0
│   [build-dependencies]
│   └── autocfg v1.1.0
├── num-traits v0.2.15 default,std (*)
├── rand v0.8.5 alloc,default,getrandom,libc,rand_chacha,std,std_rng
│   ├── rand_chacha v0.3.1 std
│   │   ├── ppv-lite86 v0.2.17 simd,std
│   │   └── rand_core v0.6.4 alloc,getrandom,std
│   │       └── getrandom v0.2.9 std
│   │           └── cfg-if v1.0.0
...
```

输出显示`range-set-blaze`依赖于`rand`。此外，它还显示`rand`依赖于带有“std”特性的`getrandom`。

我阅读了 `getrandom` [documentation](https://docs.rs/getrandom/latest/getrandom/#webassembly-support) 并了解到其“js”特性支持 WASM。那么，我们如何让 `rand` 使用 `getrandom/js`，但仅在我们编译时启用我们的“alloc”特性？我们这样更新我们的 `Cargo.toml`：

```py
[features]
default = ["std"]
std = ["getrandom/std"]
alloc = ["getrandom/js"]

[dependencies]
# ...
getrandom = "0.2.10"
```

这表示我们的“std”特性依赖于 `getrandom` 的“std”特性。然而，我们的“alloc”特性应使用 `getrandom` 的 `js` 特性。

现在可以正常工作：

```py
cargo check --target wasm32-unknown-unknown --features alloc --no-default-features
```

所以，我们已经完成了 WASM 的编译，但测试 WASM 呢？

# 规则 6：创建 WASM 测试和 WASM 演示。

让我们先用测试然后用演示网页来运行 WASM 版本。

## 在 `tests/wasm.rs` 中创建 WASM 测试

你可以几乎像测试本地代码一样测试 WASM。我们通过让原始测试仅在本地运行，而几乎重复的测试集在 WASM 上运行来做到这一点。以下是基于 [The](https://rustwasm.github.io/wasm-bindgen/wasm-bindgen-test/index.html) `[wasm-bindgen](https://rustwasm.github.io/wasm-bindgen/wasm-bindgen-test/index.html)` [Guide](https://rustwasm.github.io/wasm-bindgen/wasm-bindgen-test/index.html) 的步骤：

1.  执行 `cargo install wasm-bindgen-cli`

1.  将当前的集成测试从，例如 `tests/integration_tests.rs` 复制到 `tests/wasm.rs`。 （回忆一下，在 Rust 中，集成测试是位于 `src` 目录外的测试，并且只能看到项目的公共方法。）

1.  在 `tests/wasm.rs` 顶部，删除 `#![cfg(test)]` 并添加

    `#![cfg(target_arch = "wasm32")]`

    `use wasm_bindgen_test::*; wasm_bindgen_test_configure!(run_in_browser);`

1.  在 `wasm.rs` 中，将所有的`#[test]`替换为`#[wasm_bindgen_test]`。

1.  在你所有的 `#![cfg(test)]` 处（通常在`tests/integration_tests.rs`和`src/tests.rs`），添加额外的行：`#![cfg(not(target_arch = "wasm32"))]`

1.  在你的 `Cargo.toml` 中，将 `[dev-dependencies]`（如果有的话）更改为 `[target.'cfg(not(target_arch = "wasm32"))'.dev-dependencies]`

1.  在你的 `Cargo.toml` 中，添加一个部分：

```py
[target.'cfg(target_arch = "wasm32")'.dev-dependencies]
wasm-bindgen-test = "0.3.37"
```

设置完成后，本地测试，即`cargo test`，应该仍然有效。如果没有安装 Chrome 浏览器，请安装它。现在尝试使用以下命令运行 WASM 测试：

```py
wasm-pack test --chrome --headless --features alloc --no-default-features
```

可能会失败，因为你的 WASM 测试使用了尚未或无法放入 `Cargo.toml` 的依赖。逐个解决每个问题，或者：

1.  将所需的依赖项添加到 `Cargo.toml` 的 `[target.'cfg(target_arch = "wasm32")'.dev-dependencies]` 部分，或者

1.  从 `tests/wasm.rs` 中移除测试。

对于 `range-set-blaze`，我删除了所有与测试包的基准测试框架相关的 WASM 测试。这些测试仍将在本地运行。在 `tests\wasm.rs` 中的一些有用的测试需要 crate `syntactic-for`，所以我将其添加到 `Cargo.toml` 中的 `[target.'cfg(target_arch = "wasm32")'.dev-dependencies]` 下。修复后，所有 59 个 WASM 测试均已运行并通过。

> 旁注：如果你的项目包括一个 [examples folde](http://xion.io/post/code/rust-examples.html)r，你可能需要在你的示例中创建一个 `native` 模块和一个 `wasm` 模块。查看这个 `range-set-blaze` 文件中的[“示例”示例](https://github.com/CarlKCarlK/range-set-blaze/commit/444b9b65f0a36281d5f02067cb690108e30f77fe?diff=split#diff-37c9e138e5ec0636c73b0b74bfe4c82663f180de02c51a29055d52603f30d4b7)来了解如何操作。

## 在 `tests/wasm-demo` 中创建一个 WASM 演示。

支持 WASM 的乐趣之一是你可以在网页中演示你的 Rust 代码。这是一个网页 [演示](http://carlkcarlk.github.io/range-set-blaze/wasm-demo/) `[range-set-blaze](http://carlkcarlk.github.io/range-set-blaze/wasm-demo/)`。

按照以下步骤创建你自己的网页演示：

在项目的主 `Cargo.toml` 文件中，定义一个工作区并将 `tests/wasm-demo` 添加到其中：

```py
[workspace]
members = [".", "tests/wasm-demo"]
```

在你的测试文件夹中，创建一个 `test/wasm-demo` 子文件夹。

它应该包含一个新的 `Cargo.toml` 文件，类似于这样（将 `range-set-blaze` 更改为你项目的名称）：

```py
[package]
name = "wasm-demo"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib"]

[dependencies]
wasm-bindgen = "0.2"
range-set-blaze = { path = "../..", features = ["alloc"], default-features = false}
```

另外，创建一个文件 `tests/wasm-demo/src/lib.rs`。这是我的示例：

```py
#![no_std]
extern crate alloc;
use alloc::{string::ToString, vec::Vec};
use range_set_blaze::RangeSetBlaze;
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub fn disjoint_intervals(input: Vec<i32>) -> JsValue {
    let set: RangeSetBlaze<_> = input.into_iter().collect();
    let s = set.to_string();
    JsValue::from_str(&s)
}
```

这个文件定义了一个名为 `disjoint_intervals` 的函数，它接受一个整数向量作为输入，例如，`100,103,101,102,-3,-4`。使用 `range-set-blaze` 包，函数返回一个字符串，表示这些整数按排序后、互不重叠的区间，例如，`-4..=-3, 100..=103`。

作为最后一步，创建文件 `tests/wasm-demo/index.html`。我的使用了一些 JavaScript 代码来接收一个整数列表，然后调用 Rust WASM 函数 `disjoint_intervals`。

```py
<!DOCTYPE html>
<html>
<body>
    <h2>Rust WASM RangeSetBlaze Demo</h2>
    <p>Enter a list of comma-separated integers:</p>
    <input id="inputData" type="text" value="100,103,101,102,-3,-4" oninput="callWasmFunction()">
    <br><br>
    <p id="output"></p>
    <script type="module">
        import init, { disjoint_intervals } from './pkg/wasm_demo.js';

        function callWasmFunction() {
            let inputData = document.getElementById("inputData").value;
            let data = inputData.split(',').map(x => x.trim() === "" ? NaN : Number(x)).filter(n => !isNaN(n));
            const typedArray = Int32Array.from(data);
            let result = disjoint_intervals(typedArray);
            document.getElementById("output").innerHTML = result;
        }
        window.callWasmFunction = callWasmFunction;
        init().then(callWasmFunction);
    </script>
</body>
</html>
```

要在本地运行演示，首先将终端移动到 `tests/wasm-demo`。然后执行：

```py
# from tests/wasm-demo
wasm-pack build --target web
```

接下来，启动本地网络服务器并查看页面。我使用了 [Live Preview](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server) 扩展到 VS Code。许多人使用 `python -m http.server`。`range-set-blaze` 演示看起来像这样（也可以[在 GitHub 上实时查看](https://carlkcarlk.github.io/range-set-blaze/wasm-demo/)）：

![](../Images/0413748bafff879c803c0c63b28d809b.png)

我发现看到我的 Rust 项目在网页中运行非常令人满意。如果 WASM 兼容性是你所寻找的一切，你可以跳到规则 9。

# 规则 7：为嵌入式构建你的项目。

如果你想将你的项目推进到 WASM 之外，请遵循这个规则和接下来的规则。

确保将终端移动回项目的主目录。然后，安装 `thumbv7m-none-eabi`，这是一个流行的嵌入式处理器，并使用以下命令检查你的项目：

```py
# from project's home directory
rustup target add thumbv7m-none-eabi # only need to do this once
# will likely find issues
cargo check --target thumbv7m-none-eabi --features alloc --no-default-features
```

当我在`range-set-blaze`上执行此操作时，我得到与四组依赖项相关的错误：

+   `thiserror` — 我的项目依赖于这个crate，但实际上没有使用它。我删除了这个依赖。

+   `rand`和`getrandom` — 我的项目只需要在本地测试中使用随机数，所以我将依赖项移动到了`[target.'cfg(not(target_arch = "wasm32"))'.dev-dependencies]`。我还更新了我的主代码和测试代码。

+   `itertools`、`num-traits`和`num-integer` — 这些crate为“std”和“alloc”提供了功能。我将`Cargo.toml`更新为如下：

```py
...
[features]
default = ["std"]
std = ["itertools/use_std", "num-traits/std", "num-integer/std"]
alloc = ["itertools/use_alloc", "num-traits", "num-integer"]

[dependencies]
itertools = { version = "0.10.1", optional = true, default-features = false }
num-integer = { version = "0.1.44", optional = true, default-features = false }
num-traits = { version = "0.2.15", optional = true, default-features = false }
gen_ops = "0.4.0"

[target.'cfg(not(target_arch = "wasm32"))'.dev-dependencies]
#...
rand = "0.8.4"
#...
```

我怎么知道使用哪个依赖项的哪个功能？理解像`itertools`这样的crate的功能需要阅读[其文档](https://docs.rs/itertools/latest/itertools/#crate-features)并（通常）访问[其GitHub仓库](https://github.com/rust-itertools/itertools/blob/master/Cargo.toml)并阅读其`Cargo.toml`。你还应该使用`cargo tree`来检查你是否从每个依赖项中获得了所需的功能。例如，这种使用`cargo tree`的方法显示了对于默认编译，我得到了`range-set-blaze`、`num-integer`和`num-traits`的“std”功能，以及`itertools`和`either`的“use-std”功能：

```py
cargo tree --edges no-dev --format "{p} {f}"
```

```py
range-set-blaze v0.1.6 (O:\Projects\Science\wasmetc\wasm4) default,itertools,num-integer,num-traits,std
├── gen_ops v0.4.0
├── itertools v0.10.5 use_alloc,use_std
│   └── either v1.8.1 use_std
├── num-integer v0.1.45 std
│   └── num-traits v0.2.15 std
│       [build-dependencies]
│       └── autocfg v1.1.0
│   [build-dependencies]
│   └── autocfg v1.1.0
└── num-traits v0.2.15 std (*)
```

这表明，对于`--features alloc --no-default-feature`编译，我获得了`itertools`的“use_alloc”功能以及其他依赖项的“无默认”版本：

```py
cargo tree --edges no-dev --format "{p} {f}" --features alloc --no-default-features
```

```py
range-set-blaze v0.1.6 (O:\Projects\Science\wasmetc\wasm4) alloc,itertools,num-integer,num-traits
├── gen_ops v0.4.0
├── itertools v0.10.5 use_alloc
│   └── either v1.8.1
├── num-integer v0.1.45
│   └── num-traits v0.2.15
│       [build-dependencies]
│       └── autocfg v1.1.0
│   [build-dependencies]
│   └── autocfg v1.1.0
└── num-traits v0.2.15  (*)
```

当你认为一切正常时，使用这些命令来检查/测试本地、WASM和嵌入式：

```py
# test native
cargo test
cargo test --features alloc --no-default-features
# check and test WASM
cargo check --target wasm32-unknown-unknown --features alloc --no-default-features
wasm-pack test --chrome --headless --features alloc --no-default-features
# check embedded
cargo check --target thumbv7m-none-eabi --features alloc --no-default-features
```

这些*检查*嵌入，但*测试*嵌入呢？

# 规则 8：创建一个单一的嵌入式测试和一个嵌入式演示。

让我们通过创建一个综合测试和演示来发挥我们的嵌入式功能。我们将它运行在一个叫做QEMU的模拟器上。

测试本地Rust很简单。测试WASM Rust还可以。测试嵌入式Rust很困难。我们将只进行一次简单的测试。

> 附注 1：有关运行和模拟嵌入式Rust的更多信息，请参见：[The Embedded Rust Book](https://docs.rust-embedded.org/book/start/index.html)。
> 
> 附注 2：有关更完整的嵌入式Rust测试框架的想法，请参见[defmt-test](https://github.com/knurling-rs/defmt/tree/main/firmware/defmt-test)。遗憾的是，我无法搞清楚如何在模拟下运行它。[cortex-m/testsuite](https://github.com/rust-embedded/cortex-m/tree/master/testsuite)项目使用了defmt-test的一个分支，并且可以在模拟下运行，但没有提供独立的测试crate，并且需要三个额外的（子）项目。
> 
> 附注 3：一个嵌入式测试要比没有测试好得多。我们将在本地和WASM级别进行其余的测试。

我们将在当前的`tests`文件夹内创建嵌入式测试和演示。文件将是：

```py
tests/embedded
├── .cargo
│   └── config.toml
├── Cargo.toml
├── build.rs
├── memory.x
└── src
    └── main.rs
```

这里是创建文件和设置的步骤。

1.  安装[QEMU模拟器](https://www.qemu.org/)。在Windows上，这涉及运行安装程序，然后手动将`"C:\Program Files\qemu\"`添加到你的路径中。

2\. 创建一个依赖于本地项目的 `tests/embedded/Cargo.toml`，并包含“无默认功能”和“alloc”。这是我的：

```py
[package]
edition = "2021"
name = "embedded"
version = "0.1.0"

[dependencies]
alloc-cortex-m = "0.4.4"
cortex-m = "0.6.0"
cortex-m-rt = "0.6.10"
cortex-m-semihosting = "0.3.3"
panic-halt = "0.2.0"# reference your local project here
range-set-blaze = { path = "../..", features = ["alloc"], default-features = false }

[[bin]]
name = "embedded"
test = false
bench = false
```

3\. 创建一个文件 `tests/embedded/src/main.rs`。将你的测试代码放在“test goes here”注释之后。这是我的文件：

```py
// based on https://github.com/rust-embedded/cortex-m-quickstart/blob/master/examples/allocator.rs
// and https://github.com/rust-lang/rust/issues/51540
#![feature(alloc_error_handler)]
#![no_main]
#![no_std]

extern crate alloc;
use alloc::string::ToString;
use alloc_cortex_m::CortexMHeap;
use core::{alloc::Layout, iter::FromIterator};
use cortex_m::asm;
use cortex_m_rt::entry;
use cortex_m_semihosting::{debug, hprintln};
use panic_halt as _;
use range_set_blaze::RangeSetBlaze;

#[global_allocator]
static ALLOCATOR: CortexMHeap = CortexMHeap::empty();
const HEAP_SIZE: usize = 1024; // in bytes

#[entry]
fn main() -> ! {
    unsafe { ALLOCATOR.init(cortex_m_rt::heap_start() as usize, HEAP_SIZE) }

    // test goes here
    let range_set_blaze = RangeSetBlaze::from_iter([100, 103, 101, 102, -3, -4]);
    assert!(range_set_blaze.to_string() == "-4..=-3, 100..=103");
    hprintln!("{:?}", range_set_blaze.to_string()).unwrap();

    // exit QEMU/ NOTE do not run this on hardware; it can corrupt OpenOCD state
    debug::exit(debug::EXIT_SUCCESS);
    loop {}
}

#[alloc_error_handler]
fn alloc_error(_layout: Layout) -> ! {
    asm::bkpt();
    loop {}
}
```

4\. 从 `[cortex-m-quickstart](https://github.com/rust-embedded/cortex-m-quickstart/tree/master)` 的 GitHub 仓库复制 `build.rs` 和 `memory.x` 到 `tests/embedded/`。

5\. 创建一个包含以下内容的 `tests/embedded/.cargo/config.toml`：

```py
[target.thumbv7m-none-eabi]
runner = "qemu-system-arm -cpu cortex-m3 -machine lm3s6965evb -nographic -semihosting-config enable=on,target=native -kernel"

[build]
target = "thumbv7m-none-eabi"
```

6\. 通过将 `tests/embedded` 添加到你的工作区来更新项目的主 `Cargo.toml`：

```py
[workspace]
members = [".", "tests/wasm-demo", "tests/embedded"]
```

使用这个设置，你几乎准备好运行模拟的嵌入式了。接下来，准备好你的终端，并将编译器设置为 nightly：

```py
# Be sure qemu is on path, e.g., set PATH="C:\Program Files\qemu\";%PATH%
cd tests/embedded
rustup override set nightly # to support #![feature(alloc_error_handler)]
```

现在你可以在演示应用上使用 `cargo check`、`cargo build` 和 `cargo run`。例如：

```py
cargo run
```

```py
 Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `qemu-system-arm -cpu cortex-m3 -machine lm3s6965evb -nographic -semihosting-config enable=on,target=native -kernel O:\Projects\Science\wasmetc\wasm4\target\thumbv7m-none-eabi\debug\embedded`
Timer with period zero, disabling
"-4..=-3, 100..=103"
```

当你完成这项工作时，你将成功在一个（模拟的）微控制器上运行你的项目！如果你在设置过程中遇到问题，请仔细检查这些说明。如果还是不行，可以查看 [The Embedded Rust Book](https://docs.rust-embedded.org/book/start/qemu.html)。

完成后，请务必将编译器设置回稳定版本：

```py
rustup override set stable
```

# 规则 9：完成 CI 测试、Cargo.toml 元数据和更新后的 README.md。

## CI 测试

我们快完成了，但我们必须确保今天有效的内容明天也能有效。这就是 CI（持续集成）测试的工作。

我将我的 CI 测试设置为每次检查和每个月运行一次。如果在 GitHub 上，创建一个文件 `.github/workflows/tests.yml`：

```py
name: test

on:
  push:
  schedule: # run every month
    - cron: '0 0 1 * *'
  pull_request:
  workflow_dispatch:

jobs:
  test_rust:
    name: Test Rust
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup Rust
        uses: dtolnay/rust-toolchain@master
        with:
          toolchain: stable
      - name: Setup WASM
        uses: jetli/wasm-pack-action@v0.4.0
      - name: Test Native & WASM
        run: |
          cargo clippy --verbose --all-targets --all-features -- -D warnings
          cargo test --verbose
          cargo test --features alloc --no-default-features --verbose
          wasm-pack test --chrome --headless --features alloc --no-default-features --verbose
      - name: Setup and check Embedded
        run: |
          rustup target add thumbv7m-none-eabi
          cargo check --target thumbv7m-none-eabi --features alloc --no-default-features
          rustup override set nightly
          rustup target add thumbv7m-none-eabi
          cargo check --target thumbv7m-none-eabi --features alloc --no-default-features
          sudo apt-get update && sudo apt-get install qemu qemu-system-arm
      - name: Test Embedded (in nightly)
        timeout-minutes: 3
        run: |
          cd tests/embedded
          cargo run
```

如果你仅仅使用 WASM，你可以省略与嵌入式相关的最后两个步骤。

> 附注：为什么最后一个测试会显示 `timeout-minutes: 3`？因为一个失败的嵌入式测试不会返回失败。相反，它会进入一个无限循环。我们通过超时来捕捉这一点。

## 元数据

Rust 允许你标记你的代码适用于特定的架构和环境。惯例是使用关键字和类别元数据。具体来说，根据需要将这些关键字和 [类别](https://crates.io/category_slugs) 添加到你的 `Cargo.toml` 中：

```py
[package]
#...
keywords = [
#...
    "wasm",
    "no_std",
]
categories = [
#...
    "wasm",
    "no-std",
] 
```

**README.md**

你还应该更新你的 `README.md`，告诉大家你支持 WASM 和嵌入式。这是我添加的内容：

```py
The crate supports no_std, WASM, and embedded projects:

```toml

[dependencies]

range-set-blaze = { features = ["alloc"], default-features = false, version=VERSION }

```py

 *Relace VERSION with the current version.
```

所以，你现在有了：针对 Rust 中 WASM 和 no_std 端口的九条规则。Rust 是一个出色的语言，适用于本地、WASM 和嵌入式编程。它提供了速度、安全性，并访问到成千上万的有用 crate。遵循这九条规则，可以在几乎所有地方运行你自己的 Rust 代码。

> 附注：如果你对未来的文章感兴趣，请 [关注我在 Medium](https://medium.com/@carlmkadie)。我写关于 Rust 和 Python 的科学编程、机器学习和统计。我通常每月写一篇文章。
