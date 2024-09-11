# 使用 Criterion 基准测试 Rust 编译器设置

> 原文：[https://towardsdatascience.com/benchmarking-rust-compiler-settings-with-criterion-62db50cd62fb?source=collection_archive---------12-----------------------#2023-12-15](https://towardsdatascience.com/benchmarking-rust-compiler-settings-with-criterion-62db50cd62fb?source=collection_archive---------12-----------------------#2023-12-15)

## 使用脚本和环境变量控制 Criterion

[](https://medium.com/@carlmkadie?source=post_page-----62db50cd62fb--------------------------------)[![Carl M. Kadie](../Images/9dbe27c76e9567136e5a7dc587f1fb15.png)](https://medium.com/@carlmkadie?source=post_page-----62db50cd62fb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----62db50cd62fb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----62db50cd62fb--------------------------------) [Carl M. Kadie](https://medium.com/@carlmkadie?source=post_page-----62db50cd62fb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa5e87027005f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbenchmarking-rust-compiler-settings-with-criterion-62db50cd62fb&user=Carl+M.+Kadie&userId=a5e87027005f&source=post_page-a5e87027005f----62db50cd62fb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----62db50cd62fb--------------------------------) ·6分钟阅读·2023年12月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F62db50cd62fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbenchmarking-rust-compiler-settings-with-criterion-62db50cd62fb&user=Carl+M.+Kadie&userId=a5e87027005f&source=-----62db50cd62fb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F62db50cd62fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbenchmarking-rust-compiler-settings-with-criterion-62db50cd62fb&source=-----62db50cd62fb---------------------bookmark_footer-----------)![](../Images/d23babfcffe8bcef29dff4ca4666ab4c.png)

蟹赛时间 — 来源：[https://openai.com/dall-e-2/](https://openai.com/dall-e-2/)。所有其他图像来自作者。

本文首先解释了如何使用流行的 [criterion](https://docs.rs/criterion/latest/criterion/) crate 进行基准测试。然后，提供了额外的信息，展示了如何在不同编译器设置下进行基准测试。虽然每种编译器设置的组合都需要重新编译和单独运行，但我们仍然可以汇总和分析结果。本文是 *Towards Data Science* 中的文章 [Nine Rules for SIMD Acceleration of Your Rust Code](https://medium.com/towards-data-science/nine-rules-for-simd-acceleration-of-your-rust-code-part-1-c16fe639ce21) 的配套文章。

我们将把这项技术应用到 `[range-set-blaze](https://github.com/CarlKCarlK/range-set-blaze)` crate。我们的目标是测量不同 SIMD（单指令、多数据）设置的性能效果。我们还希望比较不同 CPU 之间的性能。这个方法也有助于理解不同优化级别的好处。

在 `range-set-blaze` 的背景下，我们评估：

+   3 种 SIMD 扩展级别 — `sse2`（128 位），`avx2`（256 位），`avx512f`（512 位）

+   10 种元素类型 — `i8`，`u8`，`i16`，`u16`，`i32`，`u32`，`i64`，`u64`，`isize`，`usize`

+   5 个 lane 数量 — 4，8，16，32，64

+   2 种 CPU — AMD 7950X（带 `avx512f`），Intel i5–8250U（带 `avx2`）

+   5 种算法 — Regular，Splat0，Splat1，Splat2，Rotate

+   4 个输入长度 — 1024；10,240；102,400；1,024,000

在这些变量中，我们外部调整前四个变量（SIMD 扩展级别、元素类型、lane 数量、CPU）。我们通过在常规 Rust 基准测试代码内部的循环控制最后两个变量（算法和输入长度）。

# 使用 Criterion 入门

要将基准测试添加到你的项目中，添加这个开发依赖并创建一个子文件夹：

```py
cargo add criterion --dev --features html_reports
mkdir benches
```

在 `Cargo.toml` 中添加：

```py
[[bench]]
name = "bench"
harness = false
```

创建一个 `benches/bench.rs`。这里是一个示例：

```py
#![feature(portable_simd)]
#![feature(array_chunks)]
use criterion::{black_box, criterion_group, criterion_main, Criterion};
use is_consecutive1::*;

// create a string from the SIMD extension used
const SIMD_SUFFIX: &str = if cfg!(target_feature = "avx512f") {
    "avx512f,512"
} else if cfg!(target_feature = "avx2") {
    "avx2,256"
} else if cfg!(target_feature = "sse2") {
    "sse2,128"
} else {
    "error"
};
type Integer = i32;
const LANES: usize = 64;
// compare against this
#[inline]
pub fn is_consecutive_regular(chunk: &[Integer; LANES]) -> bool {
    for i in 1..LANES {
        if chunk[i - 1].checked_add(1) != Some(chunk[i]) {
            return false;
        }
    }
    true
}
// define a benchmark called "simple"
fn simple(c: &mut Criterion) {
    let mut group = c.benchmark_group("simple");
    group.sample_size(1000);
    // generate about 1 million aligned elements
    let parameter: Integer = 1_024_000;
    let v = (100..parameter + 100).collect::<Vec<_>>();
    let (prefix, simd_chunks, reminder) = v.as_simd::<LANES>(); // keep aligned part
    let v = &v[prefix.len()..v.len() - reminder.len()]; // keep aligned part
    group.bench_function(format!("regular,{}", SIMD_SUFFIX), |b| {
        b.iter(|| {
            let _: usize = black_box(
                v.array_chunks::<LANES>()
                    .map(|chunk| is_consecutive_regular(chunk) as usize)
                    .sum(),
            );
        });
    });
    group.bench_function(format!("splat1,{}", SIMD_SUFFIX), |b| {
        b.iter(|| {
            let _: usize = black_box(
                simd_chunks
                    .iter()
                    .map(|chunk| IsConsecutive::is_consecutive(*chunk) as usize)
                    .sum(),
            );
        });
    });
    group.finish();
}
criterion_group!(benches, simple);
criterion_main!(benches);
```

如果你想运行这个示例，[代码在 GitHub 上](https://github.com/CarlKCarlK/range-set-blaze/tree/nov23/examples/simd/is_consecutive1)。

使用命令 `cargo bench` 运行基准测试。报告将出现在 `target/criterion/simple/report/index.html` 中，并包括像这样的图表，显示 Splat1 的运行速度比 Regular 快很多。

![](../Images/0baad5a4a40ae23d4c2e6973b4acc72f.png)

# 跳出 Criterion 的思维框架

我们面临一个问题。我们希望基准测试 `sse2` 与 `avx2` 与 `avx512f` 的性能，这需要（[一般来说](https://doc.rust-lang.org/reference/attributes/codegen.html#the-target_feature-attribute)）多次编译和 `criterion` 运行。

这是我们的方法：

+   使用 Bash 脚本设置环境变量并调用基准测试。

    例如，`bench.sh`：

```py
#!/bin/bash
SIMD_INTEGER_VALUES=("i64" "i32" "i16" "i8" "isize" "u64" "u32" "u16" "u8" "usize")
SIMD_LANES_VALUES=(64 32 16 8 4)
RUSTFLAGS_VALUES=("-C target-feature=+avx512f" "-C target-feature=+avx2" "")

for simdLanes in "${SIMD_LANES_VALUES[@]}"; do
    for simdInteger in "${SIMD_INTEGER_VALUES[@]}"; do
        for rustFlags in "${RUSTFLAGS_VALUES[@]}"; do
            echo "Running with SIMD_INTEGER=$simdInteger, SIMD_LANES=$simdLanes, RUSTFLAGS=$rustFlags"
            SIMD_LANES=$simdLanes SIMD_INTEGER=$simdInteger RUSTFLAGS="$rustFlags" cargo bench
        done
    done
done
```

> 附注：如果你有 Git 和/或 VS Code，你可以 [轻松地在 Windows 上使用 Bash](https://code.visualstudio.com/docs/sourcecontrol/intro-to-git#_git-bash-on-windows)。

+   使用 `[build.rs](https://doc.rust-lang.org/cargo/reference/build-scripts.html)` 将这些环境变量转化为 Rust 配置：

```py
use std::env;

fn main() {
    if let Ok(simd_lanes) = env::var("SIMD_LANES") {
        println!("cargo:rustc-cfg=simd_lanes=\"{}\"", simd_lanes);
        println!("cargo:rerun-if-env-changed=SIMD_LANES");
    }
    if let Ok(simd_integer) = env::var("SIMD_INTEGER") {
        println!("cargo:rustc-cfg=simd_integer=\"{}\"", simd_integer);
        println!("cargo:rerun-if-env-changed=SIMD_INTEGER");
    }
}
```

+   在 `[benches/build.rs](https://github.com/CarlKCarlK/range-set-blaze/blob/nov23/examples/simd/is_consecutive2/benches/bench.rs)` 中将这些配置转换为 Rust 常量和类型：

```py
const SIMD_SUFFIX: &str = if cfg!(target_feature = "avx512f") {
    "avx512f,512"
} else if cfg!(target_feature = "avx2") {
    "avx2,256"
} else if cfg!(target_feature = "sse2") {
    "sse2,128"
} else {
    "error"
};

#[cfg(simd_integer = "i8")]
type Integer = i8;
#[cfg(simd_integer = "i16")]
type Integer = i16;
#[cfg(simd_integer = "i32")]
type Integer = i32;
#[cfg(simd_integer = "i64")]
type Integer = i64;
#[cfg(simd_integer = "isize")]
type Integer = isize;
#[cfg(simd_integer = "u8")]
type Integer = u8;
#[cfg(simd_integer = "u16")]
type Integer = u16;
#[cfg(simd_integer = "u32")]
type Integer = u32;
#[cfg(simd_integer = "u64")]
type Integer = u64;
#[cfg(simd_integer = "usize")]
type Integer = usize;
#[cfg(not(any(
    simd_integer = "i8",
    simd_integer = "i16",
    simd_integer = "i32",
    simd_integer = "i64",
    simd_integer = "isize",
    simd_integer = "u8",
    simd_integer = "u16",
    simd_integer = "u32",
    simd_integer = "u64",
    simd_integer = "usize"
)))]
type Integer = i32;
const LANES: usize = if cfg!(simd_lanes = "2") {
    2
} else if cfg!(simd_lanes = "4") {
    4
} else if cfg!(simd_lanes = "8") {
    8
} else if cfg!(simd_lanes = "16") {
    16
} else if cfg!(simd_lanes = "32") {
    32
} else {
    64
};
```

+   在 `benches.rs` 中，创建一个基准 id，用来记录你正在测试的变量组合，变量之间用逗号分隔。这可以是一个字符串，也可以是一个 criterion `BenchmarkId`。我使用了以下调用创建了一个 `BenchmarkId`：`create_benchmark_id::<Integer>("regular", LANES, *parameter)`。

```py
fn create_benchmark_id<T>(name: &str, lanes: usize, parameter: usize) -> BenchmarkId
where
    T: SimdElement,
{
    BenchmarkId::new(
        format!(
            "{},{},{},{},{}",
            name,
            SIMD_SUFFIX,
            type_name::<T>(),
            mem::size_of::<T>() * 8,
            lanes,
        ),
        parameter,
    )
}
```

+   对于制表和分析，我喜欢将基准测试结果以逗号分隔值（CSVs）的形式保存。[Criterion 已经转向](https://bheisler.github.io/criterion.rs/book/user_guide/csv_output.html) `[*.csv](https://bheisler.github.io/criterion.rs/book/user_guide/csv_output.html)` [文件](https://bheisler.github.io/criterion.rs/book/user_guide/csv_output.html)，而转向 `*.json` 文件。为了从 `*.json` 中提取 `*.csv`，我创建了一个新的 cargo 命令供你使用：`[criterion-means](https://github.com/CarlKCarlK/cargo-criterion-means)`。

安装：

```py
cargo install cargo-criterion-means
```

运行：

```py
cargo criterion-means > results.csv
```

输出示例：

```py
Group,Id,Parameter,Mean(ns),StdErr(ns)
vector,regular,avx2,256,i16,16,16,1024,291.47,0.080141
vector,regular,avx2,256,i16,16,16,10240,2821.6,3.3949
vector,regular,avx2,256,i16,16,16,102400,28224,7.8341
vector,regular,avx2,256,i16,16,16,1024000,287220,67.067
# ...
```

# 分析

CSV 文件适合通过 [电子表格数据透视表](https://support.microsoft.com/en-us/office/create-a-pivotchart-c1b1e057-6990-4c38-b52b-8255538e7b1c) 或数据框工具，如 [Polars](https://pola-rs.github.io/polars/user-guide/transformations/pivot/) 进行分析。

例如，这是我 5000 行长的 Excel 数据文件的顶部：

![](../Images/95f1e30fa085dc0341b141331e6b2999.png)

A 到 J 列来自基准测试。K 到 N 列由 Excel 计算得出。

这是基于数据的透视表（及图表）。它显示了 SIMD lanes 数量变化对吞吐量的影响。图表平均了元素类型和输入长度。图表表明，对于最佳算法，32 或 64 个 lanes 是最好的。

![](../Images/d01656abaf5c1374a97aa21d3cfee7c0.png)

有了这次分析，我们现在可以选择我们的算法并决定如何设置 LANES 参数。

# 结论

感谢你与我一起踏上 Criterion 基准测试的旅程。

如果你之前没有使用过 Criterion，希望这能鼓励你尝试一下。如果你已经使用过 Criterion 但未能测量你关心的所有内容，希望这能为你提供一个前进的方向。以这种扩展的方式使用 Criterion 可以解锁对你 Rust 项目性能特征的更深刻洞察。

*请* [*关注 Carl 的 Medium 博客*](https://medium.com/@carlmkadie)*。我在 Rust 和 Python 的科学编程、机器学习和统计学方面写作。我每个月写一篇文章。*
