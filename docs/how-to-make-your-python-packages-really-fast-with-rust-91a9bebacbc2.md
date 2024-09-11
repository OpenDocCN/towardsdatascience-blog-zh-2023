# 如何用 Rust 让你的 Python 包真正加速

> 原文：[https://towardsdatascience.com/how-to-make-your-python-packages-really-fast-with-rust-91a9bebacbc2?source=collection_archive---------2-----------------------#2023-05-31](https://towardsdatascience.com/how-to-make-your-python-packages-really-fast-with-rust-91a9bebacbc2?source=collection_archive---------2-----------------------#2023-05-31)

## 告别，慢速代码

[](https://medium.isaacharrisholt.com/?source=post_page-----91a9bebacbc2--------------------------------)[![Isaac Harris-Holt](../Images/723dfa962a9eb9b9d4fcc99502a7a294.png)](https://medium.isaacharrisholt.com/?source=post_page-----91a9bebacbc2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----91a9bebacbc2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----91a9bebacbc2--------------------------------) [Isaac Harris-Holt](https://medium.isaacharrisholt.com/?source=post_page-----91a9bebacbc2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae5d6a2a28aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-make-your-python-packages-really-fast-with-rust-91a9bebacbc2&user=Isaac+Harris-Holt&userId=ae5d6a2a28aa&source=post_page-ae5d6a2a28aa----91a9bebacbc2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----91a9bebacbc2--------------------------------) · 6 分钟阅读 · 2023年5月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F91a9bebacbc2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-make-your-python-packages-really-fast-with-rust-91a9bebacbc2&user=Isaac+Harris-Holt&userId=ae5d6a2a28aa&source=-----91a9bebacbc2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F91a9bebacbc2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-make-your-python-packages-really-fast-with-rust-91a9bebacbc2&source=-----91a9bebacbc2---------------------bookmark_footer-----------)![](../Images/aa4989d4b59e24e5610fb197df1de377.png)

照片由[Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

Python 是……慢。这并不是什么新发现。很多动态语言都是如此。实际上，Python 的速度如此之慢，以至于许多对性能有严格要求的 Python 包作者转向了另一种语言——C。但 C 并不好玩，而且 C 有足够多的陷阱可以让一个蜈蚣陷入困境。

介绍 Rust。

Rust 是一种内存高效的语言，没有运行时或垃圾回收器。它速度极快，可靠性极高，并且有一个非常优秀的社区。此外，由于像 PyO3 和 maturin 这样的优秀工具，它也*非常容易*嵌入到你的 Python 代码中。

听起来很兴奋？太好了！因为我接下来将一步一步向你展示如何在 Rust 中创建一个 Python 包。如果你对 Rust 一无所知，也不用担心——我们不会做任何过于复杂的事情，所以你仍然可以跟得上。准备好了吗？让我们来氧化这条蛇吧。

## 前提条件

在开始之前，你需要在你的机器上安装 Rust。你可以通过访问 [rustup.rs](https://rustup.rs/) 并按照那里的说明进行安装。我还建议创建一个虚拟环境，以便用于测试你的 Rust 包。

## 脚本概述

这是一个脚本，给定一个数字 n，它将计算第 n 个斐波那契数 100 次，并记录所需时间。

这是一个非常初级、完全未优化的函数，使用纯 Python 也有很多方法可以使其更快，但我今天不会讨论那些方法。相反，我们将利用这段代码来创建一个 Rust 中的 Python 包。

## Maturin 设置

第一步是安装 [maturin](https://www.maturin.rs/)，这是一个用于构建和发布 Rust crate 作为 Python 包的构建系统。你可以通过 `pip install maturin` 来完成安装。

接下来，为你的包创建一个目录。我将其命名为 `fibbers`。最后一步是从你的新目录中运行 `maturin init`。此时，你将被提示选择要使用的 Rust 绑定。选择 `pyo3`。

![](../Images/b909e60c1bf09cc8fdeed5803878c8b8.png)

图片来源：作者。

现在，如果你查看你的 `fibbers` 目录，你会看到一些文件。Maturin 为我们创建了一些配置文件，即 `Cargo.toml` 和 `pyproject.toml`。`Cargo.toml` 文件是 Rust 构建工具 `cargo` 的配置文件，包含有关包的一些默认元数据、构建选项和 `pyo3` 的依赖项。`pyproject.toml` 文件是相当标准的，但它被设置为使用 `maturin` 作为构建后端。

Maturin 还会为发布你的包创建一个 GitHub Actions 工作流。这虽然是个小事，但在维护开源项目时会让生活变得*轻松多了*。然而，我们最关心的文件是 `src` 目录中的 `lib.rs` 文件。

这是生成的文件结构的概述。

```py
fibbers/
├── .github/
│   └── workflows/
│       └── CI.yml
├── .gitignore
├── Cargo.toml
├── pyproject.toml
└── src/
    └── lib.rs
```

## 编写 Rust 代码

Maturin 已经使用我们之前提到的 PyO3 绑定为我们创建了一个 Python 模块的框架。

这段代码的主要部分是 `sum_as_string` 函数，它被标记为 `pyfunction` 属性，以及 `fibbers` 函数，它代表我们的 Python 模块。`fibbers` 函数实际上只是将 `sum_as_string` 函数注册到我们的 `fibbers` 模块中。

如果我们现在安装这个，我们将能够从Python调用`fibbers.sum_as_string()`，一切都会按预期工作。

不过，我首先要做的是将`sum_as_string`函数替换为我们的`fib`函数。

这与我们之前编写的Python实现完全相同——它接受一个正整数n并返回第n个斐波那契数。我还将我们的新函数注册到`fibbers`模块，所以我们可以开始使用了！

## 基准测试我们的函数

要安装我们的`fibbers`包，我们只需在终端中运行`maturin develop`即可。这将下载并编译我们的Rust包，并将其安装到我们的虚拟环境中。

![](../Images/1403ae627ee0b4a01ea8ad0968f0b11a.png)

图片由作者提供。

现在，在我们的`fib.py`文件中，我们可以导入`fibbers`，打印`fibbers.fib()`的结果，然后为我们的Rust实现添加一个`timeit`案例。

如果我们现在运行第10个斐波那契数，你会发现我们的Rust函数比Python快约5倍，尽管我们使用的是相同的实现！

![](../Images/42faca074401862fe3478468a5e55196.png)

图片由作者提供。

如果我们计算第20和第30个斐波那契数，我们可以看到Rust比Python快约15倍。

![](../Images/dd9ba721121d38effa68225d2e8f6e41.png)

第20个斐波那契数结果。图片由作者提供。

![](../Images/56c728d6b3607441c5f57b78a9dd71dd.png)

第30个斐波那契数结果。图片由作者提供。

但如果我告诉你我们还没有达到最大速度呢？

你看，默认情况下，`maturin develop`会构建Rust crate的开发版本，这会放弃许多优化以减少编译时间，这意味着程序运行的速度不会达到最佳。如果我们返回到`fibbers`目录，再次运行`maturin develop`，但这次加上`--release`标志，我们将得到优化后的生产就绪版本的二进制文件。

![](../Images/7f723ac21a572c815c7dbd8c320a9f7a.png)

如果我们现在基准测试第30个斐波那契数，我们会看到Rust现在比Python快了令人惊叹的*40倍*！

![](../Images/f75f3a616edc770b5df0868e54190488.png)

第30个斐波那契数，优化版。图片由作者提供。

## Rust的限制

然而，我们的Rust实现确实有问题。如果我们尝试使用`fibbers.fib()`获取第50个斐波那契数，你会看到我们实际上遇到了溢出错误，并且得到的答案与Python不同。

![](../Images/8a5ced8efee69a67b0329f1a8c5e7c69.png)

Rust经历了整数溢出。图片由作者提供。

这是因为，与Python不同，Rust有固定大小的整数，32位整数不足以容纳我们的第50个斐波那契数。

我们可以通过将Rust函数中的类型从`u32`更改为`u64`来解决这个问题，但这会使用更多的内存，并且可能不是每台机器都支持。我们还可以使用像[num_bigint](https://docs.rs/num-bigint/latest/num_bigint/)这样的crate来解决这个问题，但这超出了本文的范围。

另一个小限制是使用 PyO3 绑定会有一些开销。你可以在这里看到我仅仅获取第一个斐波那契数，实际上 Python 比 Rust 更快，这要归功于这种开销。

![](../Images/21a63bceb9f6e6ef68242a6c1d67e854.png)

Python 在 n=1 时更快。图片来自作者。

## 需要记住的事项

本文中的数据不是在完美的机器上记录的。基准测试在我的个人机器上进行，可能无法反映真实世界的问题。请对像这样的微基准测试保持谨慎，因为它们通常不完美，并模拟了许多实际程序的方面。

希望你觉得这篇文章有帮助，并且它鼓励你尝试用 Rust 重写 Python 代码中的性能关键部分。要注意，虽然 Rust 可以加速很多东西，但如果你的 CPU 循环大部分时间都花在系统调用或网络 IO 上，它不会提供太多优势，因为这些减速超出了程序的范围。

如果你想查看更多用 Rust 编写的 Python 包的完整示例，请查看以下示例：

+   [isaacharrisholt/uuidt](https://github.com/isaacharrisholt/uuidt)

+   [pydantic/pydantic-core](https://github.com/pydantic/pydantic-core)

+   [pola-rs/polars](https://github.com/pola-rs/polars/)

本文基于我的这段视频脚本，所以如果你更倾向于视觉学习，或者希望有快速参考资料，欢迎查看：

或者查看 GitHub 上的代码：

[](https://github.com/isaacharrisholt/youtube/tree/main/010-creating-python-packages-with-rust?source=post_page-----91a9bebacbc2--------------------------------) [## youtube/010-creating-python-packages-with-rust at main · isaacharrisholt/youtube

### 每个人都知道程序速度不是 Python 的强项。这就是为什么有如此多的数据科学…

github.com](https://github.com/isaacharrisholt/youtube/tree/main/010-creating-python-packages-with-rust?source=post_page-----91a9bebacbc2--------------------------------)

保持快速！

Isaac
