# Rust: 数据科学中的下一个大事

> 原文：[https://towardsdatascience.com/rust-the-next-big-thing-in-data-science-319a03305883?source=collection_archive---------0-----------------------#2023-04-24](https://towardsdatascience.com/rust-the-next-big-thing-in-data-science-319a03305883?source=collection_archive---------0-----------------------#2023-04-24)

## 数据科学家和分析师的上下文指南

[](https://wiseai.medium.com/?source=post_page-----319a03305883--------------------------------)[![Mahmoud Harmouch](../Images/d61617549d25565399975debaad5908f.png)](https://wiseai.medium.com/?source=post_page-----319a03305883--------------------------------)[](https://towardsdatascience.com/?source=post_page-----319a03305883--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----319a03305883--------------------------------) [Mahmoud Harmouch](https://wiseai.medium.com/?source=post_page-----319a03305883--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb15db3da5667&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frust-the-next-big-thing-in-data-science-319a03305883&user=Mahmoud+Harmouch&userId=b15db3da5667&source=post_page-b15db3da5667----319a03305883---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----319a03305883--------------------------------) · 25 分钟阅读 · 2023年4月24日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F319a03305883&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frust-the-next-big-thing-in-data-science-319a03305883&user=Mahmoud+Harmouch&userId=b15db3da5667&source=-----319a03305883---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F319a03305883&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frust-the-next-big-thing-in-data-science-319a03305883&source=-----319a03305883---------------------bookmark_footer-----------)![](../Images/f70c77e0e50bd4fba3d37f016cf86374.png)

图片来源 [Yvette W](https://pixabay.com/users/wallusy-7300500/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=6246450) 来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=6246450)

## **TL**;**DR**

Rust 在数据科学中脱颖而出，因其卓越的性能和持续的安全特性。尽管它可能没有 Python 的所有功能，但在处理大型数据集时，Rust 提供了出色的效率。此外，开发者可以使用专门设计用于数据分析的各种库，以进一步简化工作流程。通过对该语言复杂性的正确掌握，从业人员可以通过将 Rust 纳入工具箱获得显著优势。

本文将**深入探讨** Rust 工具的广泛应用及其在分析[**鸢尾花数据集**](https://www.kaggle.com/datasets/uciml/iris)中的应用。尽管 Rust 在数据科学项目中的受欢迎程度不及 Python 或 R，但其作为数据科学语言的力量显而易见。其潜力和能力是无尽的，使其成为那些希望将数据科学工作提升到常规手段之外的优秀选择。

> ***注意***: 本文假设你对 Rust 及其生态系统有所了解。*
> 
> *你可以在以下仓库中找到为这篇文章开发的笔记本：*

[](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----319a03305883--------------------------------) [## GitHub - wiseaidev/rust-data-analysis: 在 Rust 中对鸢尾花数据集进行数据分析…

### 在 Rust 内核中进行鸢尾花数据集的数据分析。 curl --proto '=https' --tlsv1\. 2 -sSf https…

github.com](https://github.com/wiseaidev/rust-data-analysis?source=post_page-----319a03305883--------------------------------)

## 目录(TOC)

∘ [这篇文章适合谁?](#d61d)

∘ [为什么选择 Rust?](#82d6)

∘ [Rust 的优势](#1886)

∘ [生锈的笔记本](#784b)

∘ [关于数据集](#43a5)

∘ [读取 CSV 文件](#8699)

∘ [将 CSV 文件加载到数据框](#50ae)

∘ [转换为 ndarray](#1e06)

∘ [Numpy 等效](#e6b1)

∘ [共享相似性](#b5b5)

∘ [关键差异](#8fa8)

∘ [为什么选择 ndarray?](#4cab)

∘ [绘图工具](#4b63)

∘ [散点图](#b86d)

∘ [结论](#5524)

∘ [结束语](#157d)

∘ [参考文献](#0aad)

## 这篇文章适合谁?

![](../Images/bd20329b4e0e174eb670fe5a024a33d6.png)

照片由 [Sigmund](https://unsplash.com/fr/@sigmund?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文为那些将 Rust 作为主要编程语言的开发者撰写，并希望开始他们的数据科学之旅。其目的是为他们提供探索性数据分析所需的基本工具，包括加载、转换和可视化数据。无论你是希望了解更多关于 Rust 的初学者，还是希望在项目中使用 Rust 的经验丰富的数据科学家或分析师，这篇文章都将是一个宝贵的资源。

## 为什么选择 Rust?

![](../Images/02a47eda7418650cc4b785b58a3548ff.png)

[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) 拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数十年来，计算机科学家们致力于解决源自C和C++等编程语言的安全问题。他们的努力催生了一类新的系统编程语言，称为“内存安全”语言。这些前沿的编码实践明确旨在防止可能导致恶意网络攻击的内存相关错误。Rust无疑是这些选项中的先进工具；在当代享有广泛使用和认可。

对于那些不了解的人来说，内存安全问题指的是源自编程错误的漏洞，这些错误与内存的不当使用有关。这些问题可能导致安全漏洞、数据退化和系统故障。因此，越来越强调使用专门设计以确保最佳内存安全水平的编程语言。

像谷歌这样的科技巨头已经认识到与内存相关的问题对软件安全的巨大影响，强调了使用这些语言以防范此类漏洞的绝对必要性¹。这样的认可强有力地证明了采取主动措施保护软件免受潜在威胁的重要性。它突显了这些语言在确保软件开发未来更安全方面的作用。

Meta正在采用Rust，因为它在性能和安全性方面的好处，标志着软件工程的新纪元。通过利用Rust的现代特性和功能，Meta确保了强大的产品安全性，同时实现了更高的效率和可扩展性²。

开源社区热烈欢迎Rust，正如Linux内核的采用所证明的那样³。这一发展使得开发者可以利用Rust在基于Linux的系统上打造可靠和安全的软件。

Rust是一种极具适应性的编程语言，提供广泛的应用。不论是编写低级系统代码还是构建操作系统内核，Rust都能创建高性能、安全的软件解决方案。毫不奇怪，IEEE Spectrum最近将Rust排在2022年顶级编程语言的第20位⁴！最近在Stackoverflow中排名第14的最受欢迎语言也不足为奇⁵！

作为一家杰出的计算机技术公司，微软已表达了对一种超越当前安全标准的编程语言的需求⁶。作为一种开源编程语言，它似乎是解决这个问题的最可行解决方案之一。在这些选项中，Rust脱颖而出，因为它在安全性和速度方面表现卓越，是开发中的值得选择。

Mozilla 与三星合作创建了名为 Servo 的网页浏览器，因为 Rust 在构建安全网页浏览器方面表现出色。Servo 的目标是开发一个开创性的 Rust 浏览器引擎，将 Mozilla 在网页浏览器方面的专业技能与三星在硬件方面的专长相结合。该倡议旨在制造一个可以用于桌面电脑和移动设备的创新网页引擎。通过利用两家公司的强项，Servo 有潜力在性能上超越其他现有的网页浏览器。

可悲的是，曾经充满希望的合作突然终止，因为 Mozilla 在响应 2020 年疫情时公布了其重组战略。随着 Servo 团队的解散，许多人对 Rust 的前进势头产生了焦虑，因为该语言已成为开发安全和可靠应用程序的重要组成部分。

尽管如此，尽管经历了这一挫折，Rust 仍然成为当今最受欢迎的编程语言之一，并且在全球开发者中继续获得更多的赞誉。通过优先考虑可靠性、安全性和效率，毫无疑问，Rust 将继续成为未来构建安全网页应用程序的可靠语言。

Pydantic，一个著名的开源项目，已经将其核心实现重写为 Rust，从而显著提高了性能。Pydantic V2 比其前身 Pydantic V1.9.1 快 4 倍到 50 倍，在验证包含常见字段的模型时，性能提高了大约 17 倍。

在最近的一次公告中，微软透露了其计划在成功将 `[**dwrite**](https://learn.microsoft.com/en-us/windows/win32/directwrite/dwritecore-overview)` 字体解析库移植到 Rust 之后，重写 Windows 内核的计划。微软这一大胆举动标志着编程实践向优先考虑安全性和效率的方向转变。

随着 Rust 在各个行业中继续巩固其作为构建强健和安全应用程序的首选语言的地位，我们可以自信地期待未来安全问题的显著减少。

简而言之，使用 Rust 的主要目的是增强安全性、速度和并发性，即同时运行多个计算的能力。

## Rust 的优势。

![](../Images/dece0fdd1ee1d3b9bd150136d1545d64.png)

由[Den Harrson](https://unsplash.com/@harrson?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**1\. 类 C 的速度。**

Rust 被开发以提供类似于 C 编程语言的闪电般的性能。此外，它还提供了内存和线程安全的附加优势。这使得 Rust 成为高性能游戏、数据处理或网络应用程序的理想选择。为进一步说明这一点，请考虑以下代码片段，该代码片段使用 Rust 高效地计算了斐波那契数列：

```py
use std::hint::black_box;

fn fibonacci(n: u64) -> u64 {
    match n {
        1 | 0 => n,
        _ => fibonacci(n - 1) + fibonacci(n - 2),
    }
}

fn main() {
    let mut total: f64 = 0.0;
    for _ in 1..=20 {
        let start = std::time::Instant::now();
        black_box(fibonacci(black_box(40)));
        let elapsed = start.elapsed().as_secs_f64();
        total += elapsed;
    }
    let avg_time = total / 20.0;
    println!("Average time taken: {} s", avg_time);
}

// Average time taken: 0.3688494305 s
```

上面的代码片段使用递归计算了斐波那契数列中的 **第40个数字**。它的执行时间为 **不到一秒**，比许多其他语言中的等效代码要快得多。例如，Python 中计算相同斐波那契数列需要大约 **22.2秒**，这比 Rust 版本慢得多。

```py
>>> import timeit
>>> def fibonacci(n):
… if n < 2:
… return n
… return fibonacci(n-1) + fibonacci(n-2)
… 
>>> timeit.Timer("fibonacci(40)", "from __main__ import fibonacci").timeit(number=1)
22.262923367998155
```

**2\. 类型安全。**

Rust 旨在在编译时捕获许多错误，而不是在运行时，从而减少最终产品中出现错误的可能性。以下是一个展示 Rust 类型安全性的代码示例：

```py
fn add_numbers(a: i32, b: i32) -> i32 {
    a + b
}

fn main() {
    let a = 1;
    let b = "2";
    let sum = add_numbers(a, b); // Compile error:  expected `i32`, found `&str
    println!("{} + {} = {}", a, b, sum);
}
```

上面的代码片段试图将一个整数和一个字符串相加，由于类型安全的原因，Rust 不允许这样做。代码无法编译，并且提供了一个有用的错误信息，指明了问题所在。

**3\. 内存安全。**

Rust 已被精心开发以防止常见的内存错误，包括缓冲区溢出和空指针解引用，从而减少安全漏洞的可能性。以下是一个展示 Rust 内存安全措施的场景：

```py
fn main() {
    let mut v = vec![1, 2, 3];
    let first = v.get(0); // Compile error: immutable borrow occurs here
    v.push(4); // Compile error: mutable borrow occurs here
    println!("{:?}", first); // Compile error: immutable borrow later used here
}
```

上面的代码尝试在持有对其第一个元素的不可变引用的同时向向量中追加一个元素。由于内存安全原因，这在 Rust 中是不允许的，代码无法编译，并且提供了一个有用的错误信息。

**4\. 真实且安全的并行性。**

Rust 的所有权模型提供了一种安全而高效的并行性方式，消除了数据竞争和其他与并发相关的错误。以下是一个展示 Rust 并行性的示例：

```py
use std::thread;

fn main() {
    let mut handles = vec![];
    let mut x = 0;
    for i in 0..10 {
        handles.push(thread::spawn(move || {
            x += 1;
            println!("Hello from thread {} with x = {}", i, x);
        }));
    }
    for handle in handles {
        handle.join().unwrap();
    }
}

// Output

// Hello from thread 0 with x = 1
// Hello from thread 1 with x = 1
// Hello from thread 2 with x = 1
// Hello from thread 4 with x = 1
// Hello from thread 3 with x = 1
// Hello from thread 5 with x = 1
// Hello from thread 6 with x = 1
// Hello from thread 7 with x = 1
// Hello from thread 8 with x = 1
// Hello from thread 9 with x = 1
```

上面的代码创建了十个线程，这些线程向控制台打印消息。Rust 的所有权模型保证每个线程对所需资源具有独占访问权，有效地防止了数据竞争和其他与并发相关的错误。

**5\. 丰富的生态系统。**

Rust 提供了一个蓬勃发展的动态生态系统，拥有各种适用于广泛领域的库和工具。例如，Rust 提供了强大的数据分析工具，如 `[**ndarray**](https://docs.rs/ndarray/latest/ndarray/)` 和 `[**polors**](https://www.pola.rs/)`，而其 `[**serde**](https://serde.rs/)` 库的性能优于任何用 Python 编写的 **JSON** 库。

这些优势以及其他优势使得 Rust 成为像数据科学家这样的开发者的一个有吸引力的选择，他们寻找一种方便的编程语言，该语言提供了丰富的工具列表。

现在，有了这些认识，让我们探索在 Rust 中可以利用的不同数据分析工具，帮助你高效地进行探索性数据分析 (**EDA**)。

## Rusty Notebooks

![](../Images/911d5df96cd7e2fa6e15d36a691890b5.png)

图片由 [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

编程爱好者会同意，Rust 由于其极快的速度、可靠性和无与伦比的灵活性，已成为顶级编程语言。然而，新手 Rust 开发者长期面临一个令人生畏的挑战：缺乏一个易于访问的开发环境。

幸运的是，通过不懈的坚持和决心，Rust 开发者突破了这一障碍，提供了一个突破性的解决方案：通过**Jupyter Notebook**访问 Rust。这是通过一个称为 `[**evcxr_jupyter**](https://github.com/evcxr/evcxr/blob/main/evcxr_jupyter/README.md)` 的卓越开源项目实现的。它使开发者能够在 J**upyter Notebook** 环境中编写和执行 Rust 代码，将他们的编程体验提升到一个新的水平。

要安装 `[**evcxr_jupyter**](https://github.com/evcxr/evcxr/blob/main/evcxr_jupyter/README.md)`，你首先需要安装 [**Jupyter**](https://jupyter.org/install)。完成后，你可以运行以下命令安装**Rust Jupyter Kernel**。但首先，你需要在机器上安装 Rust。

安装了**Jupyter**后，下一步是安装**Rust Jupyter Kernel**。但在安装之前，你必须确保机器上已经安装了 Rust。

## 开始使用。

第一步是设置并安装 Rust。为此，请访问 [rustup 网站](https://rustup.rs/) 并按照说明进行操作，或者运行以下命令：

```py
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --default-toolchain nightly
```

安装 Rust 后，执行以下命令将安装 Rust Jupyter Kernel，之后你将能够在 Jupyter Notebook 上释放 Rust 的全部潜力。

```py
cargo install evcxr_jupyter
evcxr_jupyter --install
```

完成后，运行以下命令以启动 Jupyter Notebook：

```py
jupyter notebook
```

现在，是时候进行探索性数据分析 (**EDA**) 了。

## 所需依赖

如果你熟悉 Python 内核及其使用 `!pip` 安装库的卓越灵活性，那么你会高兴地发现，**Rust Jupyter Kernel** 中也提供了类似的功能。在这里，你可以使用 `:dep` 安装所需的 crates 以支持**EDA**。

安装过程非常简单，如下代码片段所示：

```py
:dep polars = {version = "0.28.0"}
```

这个 crate 提供了一系列功能，包括加载和转换数据等。现在你已经安装了必要的工具，是时候选择一个数据集，以展示 Rust 在 EDA 中的真正力量。为了简单起见，我选择了 Iris 数据集，一个流行且易于访问的数据集，将为展示 Rust 的数据处理能力提供坚实的基础。

## 关于数据集

![](../Images/1b7b67321b66ee3a7561743d810a3d72.png)

照片由 [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Iris 数据集在数据科学中至关重要，因为它在各种应用中被广泛使用，从统计分析到机器学习。拥有六列信息，它是进行探索性数据分析的理想数据集。每一列都提供了对 Iris 花卉特征的独特见解，并帮助我们深入了解这一壮丽植物。

+   `**Id**`：一个唯一的行标识符。虽然它可能很重要，但在我们接下来的分析中不需要。因此，这一列将从数据集中删除，以有效简化我们的研究过程。

+   `**SepalLengthCm**`、`**SepalWidthCm**`、`**PetalLengthCm**` 和 `**PetalWidthCm**`：每个花样本的萼片和花瓣的尺寸由列中的多变量数据描述。这些值可能包含小数部分，因此需要将其存储为浮点数据类型，如 f32，以进行精确计算。

+   `**Species**`：此列包含被收集的 Iris 花卉的具体类型。这些值是类别型的，在分析中需要以不同的方式处理。我们可以将它们转换为数值（整数）值，如 `u32`，或者保留为字符串以便更方便地处理。现在，我们将使用 String 类型以保持简单。

如您所见，Iris 数据集帮助我们揭示了 Iris 花卉的独特特征，它提供有价值见解的潜力是无限的。我们的后续分析将利用 Rust 的能力和 `Polars` crate 进行数据操作，以获得重要的发现。

## 读取 CSV 文件

![](../Images/ffc89244f6017c9c29159c1d199a7113.png)

照片由 [Mika Baumeister](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

首先，我们需要通过利用 Rust 出色的特性，选择性地导入必要的组件来导入必需的模块。以下代码片段轻松完成了这一任务。

```py
use polars::prelude::*;
use polars::frame::DataFrame;
use std::path::Path;
```

现在我们一切就绪，时机已到，掌控数据集并以精准有效的方式处理它。得益于 `polars` 提供的全面工具，处理数据从未如此轻松；所有必要的组件都包含在其 `prelude` 中，可以通过一行代码无缝导入。让我们通过这个强大的工具开始导入和处理数据吧！

## 将 CSV 文件加载到数据框中

![](../Images/fa8f1969ea89dc654520e7fb1810fd61.png)

照片由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

让我们通过以下代码片段深入了解将 CSV 文件加载到 Polars 的 DataFrame 的过程：

```py
fn read_data_frame_from_csv(
    csv_file_path: &Path,
) -> DataFrame {
    CsvReader::from_path(csv_file_path)
        .expect("Cannot open file.")
        .has_header(true)
        .finish()
        .unwrap()
}

let iris_file_path: &Path = Path::new("dataset/Iris.csv");
let iris_df: DataFrame = read_data_frame_from_csv(iris_file_path);
```

代码首先定义了一个函数 `**read_data_frame_from_csv**`，它接受 `**CSV**` 文件路径并返回一个 `**DataFrame**`。代码在该函数中创建了一个 `**CsvReader**` 对象，使用 `**from_path**` 方法。然后，它使用 `**expect**` 和 `**has_header**` 分别检查文件是否存在和是否有标题。最后，它使用 finish 加载 `**CSV**` 文件并返回结果 `**DataFrame**`，该 `**DataFrame**` 从 `**PolarsResult**` 中解包。

这段代码可以轻松地将我们的 `**CSV**` 数据集加载到 `**Polars**` `**DataFrame**` 中，并开始我们的探索性数据分析。

## 数据集维度

![](../Images/1105b0e3d44cf32700bcdaf624b33420.png)

[Lewis Guapo](https://unsplash.com/@lewisguapo?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

一旦我们将其加载到 `**DataFrame**` 中，我们可以利用 `**shape()**` 方法迅速获得关于行和列的信息。这使我们能够确定样本的数量（`**rows**`）和特征（`**columns**`），这是进一步研究和建模的基础。

```py
println!("{}", iris_df.shape());
```

```py
(150, 6)
```

我们可以看到它返回了一个元组，其中第一个元素表示行数，第二个元素表示列数。如果你对数据集有先验知识，这可能是一个很好的指示，说明你的数据集是否正确加载。这些信息在我们初始化新数组时将会很有帮助。

## 头部

+   **声明：**

```py
 iris_df.head(Some(5)) 
```

+   **输出：**

```py
shape: (5, 6)
┌─────┬───────────────┬──────────────┬───────────────┬──────────────┬─────────────┐
│ Id  ┆ SepalLengthCm ┆ SepalWidthCm ┆ PetalLengthCm ┆ PetalWidthCm ┆ Species     │
│ --- ┆ ---           ┆ ---          ┆ ---           ┆ ---          ┆ ---         │
│ i64 ┆ f64           ┆ f64          ┆ f64           ┆ f64          ┆ str         │
╞═════╪═══════════════╪══════════════╪═══════════════╪══════════════╪═════════════╡
│ 1   ┆ 5.1           ┆ 3.5          ┆ 1.4           ┆ 0.2          ┆ Iris-setosa │
│ 2   ┆ 4.9           ┆ 3.0          ┆ 1.4           ┆ 0.2          ┆ Iris-setosa │
│ 3   ┆ 4.7           ┆ 3.2          ┆ 1.3           ┆ 0.2          ┆ Iris-setosa │
│ 4   ┆ 4.6           ┆ 3.1          ┆ 1.5           ┆ 0.2          ┆ Iris-setosa │
│ 5   ┆ 5.0           ┆ 3.6          ┆ 1.4           ┆ 0.2          ┆ Iris-setosa │
└─────┴───────────────┴──────────────┴───────────────┴──────────────┴─────────────┘
```

## 尾部

+   **声明：**

```py
iris_df.tail(Some(5));
```

+   **输出：**

```py
shape: (5, 6)
┌─────┬───────────────┬──────────────┬───────────────┬──────────────┬────────────────┐
│ Id  ┆ SepalLengthCm ┆ SepalWidthCm ┆ PetalLengthCm ┆ PetalWidthCm ┆ Species        │
│ --- ┆ ---           ┆ ---          ┆ ---           ┆ ---          ┆ ---            │
│ i64 ┆ f64           ┆ f64          ┆ f64           ┆ f64          ┆ str            │
╞═════╪═══════════════╪══════════════╪═══════════════╪══════════════╪════════════════╡
│ 146 ┆ 6.7           ┆ 3.0          ┆ 5.2           ┆ 2.3          ┆ Iris-virginica │
│ 147 ┆ 6.3           ┆ 2.5          ┆ 5.0           ┆ 1.9          ┆ Iris-virginica │
│ 148 ┆ 6.5           ┆ 3.0          ┆ 5.2           ┆ 2.0          ┆ Iris-virginica │
│ 149 ┆ 6.2           ┆ 3.4          ┆ 5.4           ┆ 2.3          ┆ Iris-virginica │
│ 150 ┆ 5.9           ┆ 3.0          ┆ 5.1           ┆ 1.8          ┆ Iris-virginica │
└─────┴───────────────┴──────────────┴───────────────┴──────────────┴────────────────┘
```

## 描述

+   **声明：**

```py
iris_df.describe(None)
```

+   **输出：**

```py
Ok(shape: (9, 7)
┌────────────┬───────────┬────────────┬───────────────┬──────────────┬──────────────┬──────────────┐
│ describe   ┆ Id        ┆ SepalLengt ┆ SepalWidthCm  ┆ PetalLengthC ┆ PetalWidthCm ┆ Species      │
│ ---        ┆ ---       ┆ hCm        ┆ ---           ┆ m            ┆ ---          ┆ ---          │
│ str        ┆ f64       ┆ ---        ┆ f64           ┆ ---          ┆ f64          ┆ str          │
│            ┆           ┆ f64        ┆               ┆ f64          ┆              ┆              │
╞════════════╪═══════════╪════════════╪═══════════════╪══════════════╪══════════════╪══════════════╡
│ count      ┆ 150.0     ┆ 150.0      ┆ 150.0         ┆ 150.0        ┆ 150.0        ┆ 150          │
│ null_count ┆ 0.0       ┆ 0.0        ┆ 0.0           ┆ 0.0          ┆ 0.0          ┆ 0            │
│ mean       ┆ 75.5      ┆ 5.843333   ┆ 3.054         ┆ 3.758667     ┆ 1.198667     ┆ null         │
│ std        ┆ 43.445368 ┆ 0.828066   ┆ 0.433594      ┆ 1.76442      ┆ 0.763161     ┆ null         │
│ …          ┆ …         ┆ …          ┆ …             ┆ …            ┆ …            ┆ …            │
│ 25%        ┆ 38.25     ┆ 5.1        ┆ 2.8           ┆ 1.6          ┆ 0.3          ┆ null         │
│ 50%        ┆ 75.5      ┆ 5.8        ┆ 3.0           ┆ 4.35         ┆ 1.3          ┆ null         │
│ 75%        ┆ 112.75    ┆ 6.4        ┆ 3.3           ┆ 5.1          ┆ 1.8          ┆ null         │
│ max        ┆ 150.0     ┆ 7.9        ┆ 4.4           ┆ 6.9          ┆ 2.5          ┆ Iris-virgini │
│            ┆           ┆            ┆               ┆              ┆              ┆ ca           │
└────────────┴───────────┴────────────┴───────────────┴──────────────┴──────────────┴──────────────┘
```

## 列

+   **声明：**

```py
let column_names = iris_df.get_column_names(); 

{
    column_names
}
```

+   **输出：**

```py
["Id", "SepalLengthCm", "SepalWidthCm", "PetalLengthCm", "PetalWidthCm", "Species"]
```

## 删除物种列

+   **声明：**

```py
println!("{}", numeric_iris_df.mean());
```

+   **输出：**

```py
shape: (1, 5)
┌──────┬───────────────┬──────────────┬───────────────┬──────────────┐
│ Id   ┆ SepalLengthCm ┆ SepalWidthCm ┆ PetalLengthCm ┆ PetalWidthCm │
│ ---  ┆ ---           ┆ ---          ┆ ---           ┆ ---          │
│ f64  ┆ f64           ┆ f64          ┆ f64           ┆ f64          │
╞══════╪═══════════════╪══════════════╪═══════════════╪══════════════╡
│ 75.5 ┆ 5.843333      ┆ 3.054        ┆ 3.758667      ┆ 1.198667     │
└──────┴───────────────┴──────────────┴───────────────┴──────────────┘
```

## 最大

+   **声明：**

```py
println!("{}", numeric_iris_df.max());
```

+   **输出：**

```py
shape: (1, 5)
┌─────┬───────────────┬──────────────┬───────────────┬──────────────┐
│ Id  ┆ SepalLengthCm ┆ SepalWidthCm ┆ PetalLengthCm ┆ PetalWidthCm │
│ --- ┆ ---           ┆ ---          ┆ ---           ┆ ---          │
│ i64 ┆ f64           ┆ f64          ┆ f64           ┆ f64          │
╞═════╪═══════════════╪══════════════╪═══════════════╪══════════════╡
│ 150 ┆ 7.9           ┆ 4.4          ┆ 6.9           ┆ 2.5          │
└─────┴───────────────┴──────────────┴───────────────┴──────────────┘
```

## 转换为 ndarray

+   **声明**

```py
let numeric_iris_ndarray: ArrayBase<_, _> = numeric_iris_df.to_ndarray::<Float64Type>().unwrap();
numeric_iris_ndarray
```

+   **输出：**

```py
[[1.0, 5.1, 3.5, 1.4, 0.2],
 [2.0, 4.9, 3.0, 1.4, 0.2],
 [3.0, 4.7, 3.2, 1.3, 0.2],
 [4.0, 4.6, 3.1, 1.5, 0.2],
 [5.0, 5.0, 3.6, 1.4, 0.2],
 ...,
 [146.0, 6.7, 3.0, 5.2, 2.3],
 [147.0, 6.3, 2.5, 5.0, 1.9],
 [148.0, 6.5, 3.0, 5.2, 2.0],
 [149.0, 6.2, 3.4, 5.4, 2.3],
 [150.0, 5.9, 3.0, 5.1, 1.8]], shape=[150, 5], strides=[1, 150], layout=Ff (0xa), const ndim=2
```

在接下来的部分中，我们将深入探讨 `**ndarray**` crate 并在我们的数据集上使用其不同的方法。

## Numpy 等效

![](../Images/ee98cf79a28850ddfe0146668498d39a.png)

[Nick Hillier](https://unsplash.com/@nhillier?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

在 Rust 中，有一个强大的 crate，或你在 Python 中称之为包，相当于 `**Numpy**`，它允许我们轻松存储和操控数据。它叫做 `**ndarray**`，提供了一个多维容器，包含分类或数值元素。

值得注意的是，在 Rust 中，包被称为 crates，这取决于存储包的注册表名称。`**ndarray**` crate 可以在 [**crate.io**](https://crates.io/crates/ndarray) 上找到，类似于 Python 中的 **Pypi**。

使用 `**ndarray**`，我们可以创建 n 维数组，进行切片和视图，进行数学运算等。这些功能在我们将数据集加载到可以操作的容器并进行分析时将是必不可少的。

## 共享相似性

![](../Images/50dc303d754d9eb34361df8abb5d2cfd.png)

[Jonny Clow](https://unsplash.com/ja/@jonnyclow?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上的照片

来自`**ndarray**` crate的`[**ArrayBase**](https://docs.rs/ndarray/latest/ndarray/struct.ArrayBase.html)`类型是Rust中数据操作的一个重要工具，配备了许多强大的功能。它在特定元素类型、无限维度和任意步幅方面与`**NumPy**`的数组类型`**numpy.ndarray**`类似。如果你希望以无与伦比的效率处理大量数据，`**ndarray**`是最佳选择。

不能过分强调`**ndarray**`和`**NumPy**`数组类型之间的根本相似性；即从零开始的索引，而非从一开始。不要低估这个看似微不足道的特性，因为它在处理大型数据集时可能会产生显著影响。

另一个重要的相似点是`**ndarray**`和`**NumPy**`数组类型的默认内存布局，即行优先。换句话说，默认迭代器遵循行的逻辑顺序。这个特性在处理超出内存容量且无法同时完全加载的数组时非常宝贵。

算术运算符在`**ndarray**`和`**NumPy**`的数组类型中分别作用于每个元素。简单来说，执行`**a * b**`会进行逐元素相乘，而不是矩阵乘法。这一功能的优点在于可以轻松地对相对较大的数组进行计算。

`**ndarray**`和`**NumPy**`的数组类型中的拥有数组在内存中是连续的。这意味着它们存储在一个单独的内存块中，这可以提高访问数组元素时的性能。

许多操作，如切片和算术运算，也被`**ndarray**`和`**NumPy**`的数组类型所支持。这使得根据需要在这两种数组类型之间切换变得简单。

高效执行操作是计算数据处理领域中显著影响处理时间和资源使用的关键方面。切片就是一个很好的例子，因为它的成本很低——只返回数组的视图，而不是重复整个数据集。

在撰写本文时，`**ndarray**`中缺少一些`**NumPy**`的重要功能。特别是，当涉及到左侧和右侧数组同时进行广播功能的二进制操作时，这一能力目前只能通过`**numpy**`实现，而不是仅通过`**ndarray**`。

## 主要区别

![](../Images/6072cc3586e2d46dc3f9ea34377afca3.png)

图片由[Eric Prouzet](https://unsplash.com/@eprouzet?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

`**Numpy**`和`**ndarray**`之间有许多关键差异。例如，在`**NumPy**`中，没有对拥有的数组、视图和可变视图的区分。多个数组（`**numpy. ndarray**`的实例）可以可变地引用相同的数据。另一方面，在ndarray中，所有数组都是`**ArrayBase**`的实例，但`**ArrayBase**`是对数据所有权的泛型。Array拥有其数据；`**ArrayView**`是一个视图；`**ArrayViewMut**`是一个可变视图；`**CowArray**`要么拥有其数据，要么是视图（带有视图变体的写时复制变更）；`**ArcArray**`有一个对其数据的引用计数指针（带有写时复制变更）。数组和视图遵循Rust的别名规则。

`**NumPy**`的另一个重要特性是所有数组的维度是灵活的。然而，使用`**ndarray**`，你可以创建像Array2这样的固定维度数组，这可以提供更准确的结果，并消除与形状和步幅相关的多余堆分配。

最后，当在`**NumPy**`中进行切片时，索引从`**start + step, start + 2 * step, …**`开始，一直到结束（不包括结束）。在`**ndarray**`中，首先对轴进行`start..end`切片。如果步长为正，则第一个索引是切片的前端；如果步长为负，则第一个索引是切片的后端。这意味着，行为与`**NumPy**`相同，除非`**step < -1**`。有关更多详细信息，请参阅[**s!宏**](https://docs.rs/ndarray/latest/ndarray/macro.s.html)的文档。

## 为什么选择ndarray？

对于经验丰富的Rust开发者，可以提出这样一个观点，即语言已经拥有了许多数据结构，例如向量，因此不需要第三方库来处理数据。然而，这一论断未能认识到`**ndarray**`的专门性质，它旨在处理具有数学重点的n维数组。

Rust无疑是一种强大的编程语言，可以轻松应对各种编程挑战。然而，对于多维数组的复杂操作，`**ndarray**`是终极解决方案。它的专门设计使得在科学计算和分析环境中能够无缝执行高级数据操作任务，使其成为任何寻求最佳结果的程序员的必备工具。

为了说明这一点，考虑一个例子，其中研究人员需要操作来自科学实验的大量多维数据。Rust的内置数据结构，如[**向量**](https://doc.rust-lang.org/rust-by-example/std/vec.html)，可能不适合这一任务，因为它们缺乏复杂数组操作所需的高级特性。相比之下，`**ndarray**`提供了广泛的功能，包括切片、广播和逐元素操作，可以在分析数据时简化和加速数据操作任务，正如我们将在以下部分中探索的那样。

## **数组创建**

本节提供了许多从头创建数组的技巧，使用户能够生成适合其特定需求的数组。然而，值得注意的是，除了本节之外，还有其他创建数组的方法。例如，通过对现有数组执行算术运算，也可以生成数组。

现在，让我们探索`**ndarray**`提供的不同功能：

+   **2行 × 3列** 浮点数组字面量：

```py
array![[1.,2.,3.], [4.,5.,6.]]
// or
arr2(&[[1.,2.,3.], [4.,5.,6.]])
```

+   **1-D范围** 的值：

```py
Array::range(0., 10., 0.5) //  0.0, 0.5, 1.5 ... 9.5
```

+   `**1-D数组**`，范围内的n个元素：

```py
Array::linspace(0., 10., 11)
```

+   **3×4×5** 的数组：

```py
Array::ones((3, 4, 5))
```

+   **3×4×5** 的零数组：

```py
Array::zeros((3, 4, 5))
```

+   **3×3** 单位矩阵：

```py
Array::eye(3)
```

## 索引和切片

+   最后一个元素：

```py
arr[arr.len() - 1]
```

+   第1行，第4列：

```py
arr[[1, 4]]
```

+   前5行：

```py
arr.slice(s![0..5, ..])
// or
arr.slice(s![..5, ..])
// or
arr.slice_axis(Axis(0), Slice::from(0..5))
```

+   最后5行：

```py
arr.slice(s![-5.., ..])
// or
arr.slice_axis(Axis(0), Slice::from(-5..))
```

## 数学

+   求和：

```py
arr.sum()
```

+   沿轴求和：

```py
// first axis
arr.sum_axis(Axis(0))
// second axis
arr.sum_axis(Axis(1))
```

+   平均值：

```py
arr.mean().unwrap()
```

+   转置：

```py
arr.t()
// or
arr.reversed_axes()
```

+   **2-D矩阵** 乘法：

```py
mat1.dot(&mat2)
```

+   平方根：

```py
data_2D.mapv(f32::sqrt)
```

+   算术：

```py
&a + 1.0
&mat1 + &mat2
&mat1_2D + &mat2_1D
```

在本节中，我们探索了`**ndarray**`提供的各种功能；这是一个强大的工具，适用于多维容器，并提供了一系列用于简化数据处理的功能。我们的探索涵盖了使用`**ndarray**`的关键要素：创建数组、确定数组维度、通过索引技术访问数组以及高效执行基本数学操作。

总结来说，`**ndarray**`是开发人员和数据分析师的宝贵资产。它提供了许多方法，能够高效地处理多维数组，既方便又准确。通过掌握本节讨论的技巧，并利用`**ndarray**`的潜力，用户可以轻松执行复杂的数据处理任务，同时根据其发现生成更快速且准确的见解。

## Plotters

![](../Images/184bc187368b06488376fef7750a8546.png)

由[Lukas Blazek](https://unsplash.com/@goumbik?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在使用`**ndarray**`处理和操作数据之后，下一步逻辑是通过使用`[**Plotters**](https://docs.rs/plotters/latest/plotters/)`库来获得有价值的见解。这个强大的库使我们能够轻松而精准地创建令人惊叹且信息丰富的数据可视化。

为了充分利用`**jupyter-evcxr**`和Plotters库，需要在执行以下命令之前先导入它：

```py
:dep plotters = { version = "^0.3.0", default_features = false, features = ["evcxr", "all_series"] }
```

由于`**evcxr**`仅依赖于SVG图像并支持所有系列类型，因此不需要额外的后端。因此，将其用法融入我们的系统中是非常好的，使用如下：

```py
default_features = false, features = ["evcxr", "all_series"]
```

在导入库之后，我们可以利用其丰富的可视化工具来制作引人注目且富有启发性的视觉效果，如图表、图形和其他形式。通过这些可视化，我们可以轻松地检测模式、趋势或洞察力。这使得基于数据的决策成为可能，从而产生有价值的结果。

首先，我们开始绘制花萼特征的散点图。

## 散点图

我们将散点图代码分成几个部分以便于阅读。以下是一个示例：

```py
let sepal_samples:Vec<(f64,f64)> = {
    let sepal_length_cm: DataFrame = iris_df.select(vec!["SepalLengthCm"]).unwrap();
    let mut sepal_length = sepal_length_cm.to_ndarray::<Float64Type>().unwrap().into_raw_vec().into_iter();
    let sepal_width_cm: DataFrame = iris_df.select(vec!["SepalWidthCm"]).unwrap();
    let mut sepal_width = sepal_width_cm.to_ndarray::<Float64Type>().unwrap().into_raw_vec().into_iter();
    sepal_width.zip(sepal_length).collect()
};
```

这段代码创建了一个名为`**sepal_samples**`的元组向量，其中每个元组表示来自鸢尾花数据集的花萼长度和花萼宽度的样本。现在，让我们逐行分析代码的功能：

+   `**let sepal_samples: Vec<(f64,f64)> = {…}**`：定义了一个名为`**sepal_samples**`的变量，并将一个用大括号`**{…}**`括起来的代码块赋值给它。`**Vec<(f64,f64)>**`数据类型注释表明该向量包含由两个**64位**浮点数组成的元组。这一声明使Rust能够有效地识别和处理数据集中每个元组。

+   `**let sepal_length_cm: DataFrame = iris_df.select(vec![“SepalLengthCm”]).unwrap();**`：为了从`**iris_df**` DataFrame中提取`**SepalLengthCm**`列，我们使用`**select**`函数，并将其存储在一个名为`**sepal_length_cm**`的新`**DataFrame**`对象中。

+   `**let mut sepal_length = sepal_length_cm.to_ndarray::<Float64Type>().unwrap().into_raw_vec().into_iter();**`：通过`**to_ndarray**`方法，我们可以将`**sepal_length_cm**`的`**DataFrame**`对象转换为`**Float64Type**`类型的`**ndarray**`。接着，使用`**into_raw_vec**`方法可以将这个新数组转换为原始向量格式。通过调用`**into_iter**`生成的迭代器，我们可以逐个消费和利用每个元素；这非常有趣！

+   `**let sepal_width_cm: DataFrame = iris_df.select(vec![“SepalWidthCm”]).unwrap();**`：从`**iris_df**` DataFrame中选择`**SepalWidthCm**`列，并将结果存储在一个名为`**sepal_width_cm**`的新`**DataFrame**`对象中。

+   `**let mut sepal_width = sepal_width_cm.to_ndarray::<Float64Type>().unwrap().into_raw_vec().into_iter();**`：通过`**to_ndarray**`方法，将名为`**sepal_width_cm**`的`**DataFrame**`对象转换为数据类型为`**Float64Type**`的`**ndarray**`对象。然后，通过应用`**into_raw_vec**`将结果`**ndarray**`转换为原始向量，最终通过调用`**.into_iter()**`生成一个迭代器，以便逐个消费其元素。

+   `**sepal_width.zip(sepal_length).collect()**`：通过对`**sepal_width**`调用`**zip**`函数，并将`**sepal_length**`作为参数传递，生成一个新的迭代器。该迭代器产生包含一个萼片宽度元素和一个萼片长度元素的元组。这些元组随后使用`collect`方法收集，形成一个新向量——类型为`**Vec<(f64,f64)>**`——并存储在名为`**sepal_samples**`的变量中。

以下代码块看起来如下：

```py
evcxr_figure((640, 480), |root| {
    let mut chart = ChartBuilder::on(&root)
        .caption("Iris Dataset", ("Arial", 30).into_font())
        .x_label_area_size(40)
        .y_label_area_size(40)
        .build_cartesian_2d(1f64..5f64, 3f64..9f64)?;

    chart.configure_mesh()
        .x_desc("Sepal Length (cm)")
        .y_desc("Sepal Width (cm)")
        .draw()?;

    chart.draw_series(sepal_samples.iter().map(|(x, y)| Circle::new((*x,*y), 3, BLUE.filled())));

    Ok(())
}).style("width:60%")
```

+   `**evcxr_figure((640, 480), |root| {**`：使用640像素宽和480像素高的尺寸初始化了一个新的Evcxr图形。此外，还传递了一个接受根参数的闭包，该参数表示声明图形的基本绘图区域。

+   `**let mut chart = ChartBuilder::on(&root)**`：这使用根绘图区域作为基础创建了一个新的图表构建对象。

+   `**.caption(“Iris Dataset”, (“Arial”, 30).into_font())**`：这为图表添加了标题，文本为`**Iris Dataset**`，字体为`**Arial**`，大小为`**30**`。

+   `**.x_label_area_size(40)**`：这将`**X轴**`标签区域的大小设置为`**40**`像素。

+   `**.y_label_area_size(40)**`：这将`**Y轴**`标签区域的大小设置为`**40**`像素。

+   `**.build_cartesian_2d(1f64..5f64, 3f64..9f64)?;**`：这一行代码构建了一个2D笛卡尔图表，`**X轴**`的范围从`**1到5**`，`**Y轴**`的范围从`**3到9**`，并返回一个`Result`类型，该类型用`**?**`运算符进行解包。

+   `**chart.configure_mesh()**`：这配置了图表的网格，即图表的网格线和刻度线。

+   `**.x_desc(“Sepal Length (cm)”)**`：这将`**X轴**`的描述设置为`**Sepal Length (cm)**`。

+   `**.y_desc(“Sepal Width (cm)”)**`：这将`**Y轴**`的描述设置为`**Sepal Width (cm)**`。

+   `**.draw()?;**`：这绘制了网格并返回一个`Result`类型，该类型用`**?**`运算符进行解包。

+   `**chart.draw_series(sepal_samples.iter().map(|(x, y)| Circle::new((*x,*y), 3, BLUE.filled())));**`：使用`**sepal_samples**`向量作为输入，在图表上绘制了一系列数据点。调用`**iter()**`函数以遍历`**sepal_samples**`中的每个元素，并使用`**map()**`方法创建一个迭代器，将每个点转换为一个填充蓝色且半径为3的`**Circle**`对象。最后，将这些`Circle`对象的系列传递给`**chart.draw_series()**`，将它们美丽地呈现在图表画布上。

运行上述代码块将在你的笔记本中绘制以下内容：

![](../Images/e1281a8fc0052b6b1f757cbd2c3b952e.png)

Iris 数据集萼片散点图（图片来源：作者）

## 结论

![](../Images/52ec672772fd60552bccf21434500155.png)

图片由[Aaron Burden](https://unsplash.com/it/@aaronburden?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在本文中，我们深入探讨了Rust中的三个工具，并应用它们来分析鸢尾花数据集的数据。我们的发现表明，Rust 是一种强大的语言，具有巨大的潜力，可以轻松执行数据科学项目。尽管它的普及程度不及 Python 或 R，但其能力使其成为那些希望显著提升数据科学工作的人的绝佳选择。

已确认 Rust 是一种快速高效的语言，其类型系统使调试相对容易。此外，Rust 中有许多专门针对数据科学任务的库和框架，例如 `[**Polars**](https://docs.rs/polars/latest/polars/)` 和 `[**ndarray**](https://docs.rs/ndarray/latest/ndarray/)`，它们能够无缝处理大量数据集。

总体而言，Rust 是一个出色的编程语言，适合数据科学项目，因为它提供了卓越的性能，并且相对容易管理复杂的数据集。数据科学领域的有志开发者应考虑将 Rust 作为他们的选择之一，以便在这一领域开启成功的旅程。

## 结束语

在我们结束本教程时，我想对所有那些投入时间和精力完成本教程的人表示诚挚的感谢。能够与你们一起展示Rust编程语言的卓越能力，我感到非常高兴。

对数据科学充满热情的我承诺，从现在开始，我每周至少会写一篇关于相关主题的综合性文章。如果你对我的工作感兴趣，考虑通过各种社交媒体平台与我联系，或者直接联系我寻求其他帮助。

感谢！

## 参考文献

[1] 队列的硬化增强。 (2019年5月9日)。发表于 Google 安全博客。[https://security.googleblog.com/2019/05/queue-hardening-enhancements.html](https://security.googleblog.com/2019/05/queue-hardening-enhancements.html)

[2] Rust 在 Facebook 的简要历史。 (2021年4月29日)。发表于 Engineering.fb 博客。[https://engineering.fb.com/2021/04/29/developer-tools/rust](https://engineering.fb.com/2021/04/29/developer-tools/rust)

[3] Linux 6.1 正式在内核中添加对 Rust 的支持。 (2022年12月20日)。发表于 infoq.com。[https://www.infoq.com/news/2022/12/linux-6-1-rust](https://www.infoq.com/news/2022/12/linux-6-1-rust)

[4] 2022年顶级编程语言。 (2022年8月23日)。发表于 spectrum.ieee.com。[https://spectrum.ieee.org/top-programming-languages-2022](https://spectrum.ieee.org/top-programming-languages-2022)

[5] 编程、脚本和标记语言。 (2022年5月)。发表于 StackOverflow 开发者调查 2022。[https://survey.stackoverflow.co/2022/#programming-scripting-and-markup-languages](https://survey.stackoverflow.co/2022/#programming-scripting-and-markup-languages)

[6] 我们需要一种更安全的系统编程语言。（2019年7月18日）。在Microsoft安全响应中心博客上。 [https://msrc.microsoft.com/blog/2019/07/we-need-a-safer-systems-programming-language/](https://msrc.microsoft.com/blog/2019/07/we-need-a-safer-systems-programming-language/)

[7] Mozilla和三星合作开发下一代网页浏览器引擎。（2013年4月3日）。在Mozilla博客上。 [https://blog.mozillarr.org/en/mozilla/mozilla-and-samsung-collaborate-on-next-generation-web-browser-engine/](https://blog.mozillarr.org/en/mozilla/mozilla-and-samsung-collaborate-on-next-generation-web-browser-engine/)

[8] 由于疫情，Mozilla裁员250人。（2020年8月11日）。在Engadget上。 [https://www.engadget.com/mozilla-firefox-250-employees-layoffs-151324924.html](https://www.engadget.com/mozilla-firefox-250-employees-layoffs-151324924.html)

[9] Pydantic V2如何利用Rust的超级力量。（2023年2月4日和5日）。在fosdem.org上。 [https://fosdem.org/2023/schedule/event/rust_how_pydantic_v2_leverages_rusts_superpowers/](https://fosdem.org/2023/schedule/event/rust_how_pydantic_v2_leverages_rusts_superpowers/)

[10] pydantic-v2 性能。（2022年12月23日）。在docs.pydantic.dev上。 [https://docs.pydantic.dev/blog/pydantic-v2/#performance](https://docs.pydantic.dev/blog/pydantic-v2/#performance)

[11] BlueHat IL 2023 - David Weston-默认安全。（2023年4月19日）。在youtube.com上。 [https://www.youtube.com/watch?v=8T6ClX-y2AE&t=2703s](https://www.youtube.com/watch?v=8T6ClX-y2AE&t=2703s)
