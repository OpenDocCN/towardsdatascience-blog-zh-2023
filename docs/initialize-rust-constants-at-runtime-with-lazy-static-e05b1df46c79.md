# 使用 lazy_static 在运行时初始化 Rust 常量

> 原文：[`towardsdatascience.com/initialize-rust-constants-at-runtime-with-lazy-static-e05b1df46c79`](https://towardsdatascience.com/initialize-rust-constants-at-runtime-with-lazy-static-e05b1df46c79)

## 使用懒惰初始化定义非常量静态变量

[](https://medium.com/@nic-obert?source=post_page-----e05b1df46c79--------------------------------)![Nicholas Obert](https://medium.com/@nic-obert?source=post_page-----e05b1df46c79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e05b1df46c79--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e05b1df46c79--------------------------------) [Nicholas Obert](https://medium.com/@nic-obert?source=post_page-----e05b1df46c79--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e05b1df46c79--------------------------------) ·4 分钟阅读·2023 年 8 月 5 日

--

![](img/d3a48ff6643b48687887a2af35a5c63e.png)

由 [Christian Lue](https://unsplash.com/@christianlue?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

在编程中，在编译时初始化常量的好处是显而易见的。这样不仅减少了初始化开销，还使编译器能够更聪明地优化代码，因为它提前知道了常量的值。

然而，有时在编译时初始化每个常量是不可能的，因为这需要执行非常量操作或获取仅在运行时可用的数据。例如，假设我们在程序中反复使用数字 `√7`。与其每次计算它，不如像下面这样定义一个常量：

```py
const ROOT_OF_SEVEN: f64 = 7_f64.sqrt();
```

然而，这段代码是无效的。Rust 编译器返回以下错误：

```py
cannot call non-const fn `f64::<impl f64>::sqrt` in constants
calls in constants are limited to constant functions, tuple structs and tuple variants
```

如果我们尝试用环境变量初始化一个常量，也会发生相同的情况：

```py
const LANG: String = env::var("LANG").unwrap(); 
```

来自 Rust 编译器：

```py
cannot call non-const fn `std::env::var::<&str>` in constants
calls in constants are limited to constant functions, tuple structs and tuple variants
```

正如你所见，某些在编译时初始化的常量需要非常量操作。这时，Rust 的 `lazy_static` crate 就派上用场了。`lazy_static` 允许你定义全局静态变量，这些变量会被懒惰地初始化，即它们的值仅在第一次实际使用时才被设置。懒惰的静态变量只需在第一次使用时初始化一次，由于这是一次性操作，因此它们的运行时初始化开销可以忽略不计。

在本文中，我们将探讨如何使用 Rust 的 `lazy_static` crate 来懒惰地初始化全局常量及其一些用例。

## 使用 lazy_static

要使用 `lazy_static` crate，你只需将其添加到项目依赖中

```py
cargo add lazy_static
```

一旦你添加了这个 crate，你可以在源文件中像这样导入 `lazy_static!` 宏：

```py
use lazy_static::lazy_static;
```

要定义延迟静态，需将所有声明包含在 `lazy_static!` 宏中：

```py
lazy_static! {
  static ref LANG: String = env::var("LANG").unwrap();
  static ref ROOT_OF_SEVEN: f64 = 7_f64.sqrt();
}
```

通过这样做，你声明了一个全局静态引用，用于延迟初始化的值。要使用这些“延迟常量”，你只需对它们进行解引用：

```py
fn main() {

  println!("System language: LANG is {}", *LANG);
  println!("Square root of 7 is {}", *ROOT_OF_SEVEN);

}
```

现在你可以有效地使用 `lazy_static` crate 在运行时初始化全局常量。继续阅读以了解它是如何工作的。

## 什么是延迟初始化？

延迟静态何时初始化？在计算中，“延迟”一词通常意味着只有在需要时才进行工作。否则，延迟操作不会执行。在延迟静态的情况下，它们会在首次使用之前初始化。

为了更清楚，请查看以下代码示例：

```py
lazy_static! {

    static ref BYTES_WRITTEN: usize = {
        let bytes_written = std::io::stdout().write(b"Initializing lazy_static!\n").unwrap();
        std::io::stdout().flush().unwrap();
        bytes_written
    };

    static ref ANOTHER_LAZY_STATIC: String = {
        println!("Initializing ANOTHER_LAZY_STATIC!");
        "Hello, World!".to_string()
    };

    static ref UNUSED_LAZY_STATIC: String = {
        println!("Initializing UNUSED_LAZY_STATIC!");
        "Hello, World!".to_string()
    };

}
```

所有这些延迟静态初始化都有副作用：它们会将内容打印到控制台。利用这些副作用，我们可以检测到它们的值何时被实际初始化。

`main()` 函数：

```py
fn main() {

    println!("Doing stuff before using lazy statics...");

    println!("Bytes written during initialization: BYTES_WRITTEN is {}", *BYTES_WRITTEN);

    println!("Reusing an already initialized lazy static: BYTES_WRITTEN is {}", *BYTES_WRITTEN);

    println!("ANOTHER_LAZY_STATIC is {}", *ANOTHER_LAZY_STATIC);

}
```

输出顺序显示了每个操作在运行时的执行时间：

```py
Doing stuff before using lazy statics...

Initializing lazy_static!

Bytes written during initialization: BYTES_WRITTEN is 26

Reusing an already initialized lazy static: BYTES_WRITTEN is 26

Initializing ANOTHER_LAZY_STATIC!

ANOTHER_LAZY_STATIC is Hello, World!
```

正如从第一个输出行中看到的，所有延迟静态都在实际需要之前没有初始化。第二和第三输出行显示初始化恰好发生在值被使用之前。

第四行突出了这样一个事实：对于已使用的延迟静态，不需要初始化，因为它们的值已被存储。

另外，你是否注意到 `UNUSED_LAZY_STATIC` 从未初始化？那是因为只有在实际需要时才会执行延迟操作。

## 深入阅读

本文旨在成为 `lazy_static` crate 及其最流行功能的基础教程。欲了解更多信息，请阅读 [官方文档](https://docs.rs/lazy_static/latest/lazy_static/)。

希望你喜欢这篇文章。如果你有任何补充，请在评论中分享你的想法。感谢阅读！

如果你想了解更多关于低级编程的内容，我强烈推荐你查看下面的故事：

[## 这里是 CPU 如何处理 if 语句和分支](https://betterprogramming.pub/heres-how-the-cpu-handles-if-statements-and-branching-95cfd42af9c?source=post_page-----e05b1df46c79--------------------------------)

### 了解为什么 if 语句不受欢迎以及它们实际是如何工作的。剧透：它们并没有那么糟糕

[betterprogramming.pub](https://betterprogramming.pub/heres-how-the-cpu-handles-if-statements-and-branching-95cfd42af9c?source=post_page-----e05b1df46c79--------------------------------)
