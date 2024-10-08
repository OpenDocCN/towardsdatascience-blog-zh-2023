# 使用 Rust 扩展提升你的 Python 代码

> 原文：[`towardsdatascience.com/boost-your-python-code-with-rust-extensions-108afdbd4e13`](https://towardsdatascience.com/boost-your-python-code-with-rust-extensions-108afdbd4e13)

## 提高速度几个数量级并增强代码安全性

[](https://medium.com/@nic-obert?source=post_page-----108afdbd4e13--------------------------------)![Nicholas Obert](https://medium.com/@nic-obert?source=post_page-----108afdbd4e13--------------------------------)[](https://towardsdatascience.com/?source=post_page-----108afdbd4e13--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----108afdbd4e13--------------------------------) [Nicholas Obert](https://medium.com/@nic-obert?source=post_page-----108afdbd4e13--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----108afdbd4e13--------------------------------) ·6 分钟阅读·2023 年 7 月 14 日

--

![](img/ec001b47bea181b0429a89366a8512e8.png)

图片由作者提供，使用 DALLE 生成

正如你们大多数人已经知道的，Python 是一种通用编程语言，优化了简单性和易用性。虽然它是处理轻量级任务的优秀工具，但代码执行速度很快就会成为你程序的主要瓶颈。

在这篇文章中，我们将讨论为什么 Python 相较于其他编程语言如此缓慢。然后，我们将看到如何为 Python 编写一个基本的 Rust 扩展，并将其性能与原生 Python 实现进行比较。

## 为什么 Python 很慢

在我们开始之前，我想指出编程语言本身并不 inherently 快或慢：它们的实现才是决定因素。如果你想了解语言和其实现之间的区别，请查看这篇文章：

[](https://betterprogramming.pub/the-fastest-programming-language-myth-65eaaf7e5d1a?source=post_page-----108afdbd4e13--------------------------------) [## 最快编程语言的神话

### 一个必须消除的常见编程误解

[betterprogramming.pub](https://betterprogramming.pub/the-fastest-programming-language-myth-65eaaf7e5d1a?source=post_page-----108afdbd4e13--------------------------------)

首先，Python 是动态类型的，这意味着变量的类型仅在运行时知道，而不是在编译时知道。虽然这种设计选择允许代码更加灵活，但 Python 解释器无法对你的变量及其大小做出假设。因此，它不能像静态编译器那样进行优化。

另一个使 Python 比其他替代方案更慢的设计选择是臭名昭著的 GIL。全局解释器锁是一个互斥锁，只允许一个线程在任何时候执行。GIL 最初是为了保证线程安全，但遭到了多线程应用程序开发者的强烈反对。

此外，Python 代码是通过虚拟机执行的，而不是直接在 CPU 上运行。这层额外的抽象增加了显著的执行开销，与静态编译语言相比。

此外，Python 对象在内部被视为字典（或哈希表），它们的属性（通过点操作符访问的属性和方法）通常不是通过内存偏移量访问的，而是通过字典查找，这显著较慢。如果你有兴趣了解更多，我写了一篇文章深入探讨了 Python 中属性查找的工作原理：

[## 通过 __slots__ 将您的 Python 代码库优化到接近 3 倍](https://betterprogramming.pub/optimize-your-python-programs-for-free-with-slots-4ff4e1611d9d?source=post_page-----108afdbd4e13--------------------------------)

### 通过添加一行代码来提高执行速度和内存使用

[betterprogramming.pub](https://betterprogramming.pub/optimize-your-python-programs-for-free-with-slots-4ff4e1611d9d?source=post_page-----108afdbd4e13--------------------------------)

## Python 扩展

你可能会问，为什么尽管 Python 有明显的性能缺陷，它仍然被广泛使用。数据科学和机器学习难道不需要高计算能力吗？

为了绕过 Python 的速度问题，开发者在代码中调用外部优化函数来完成繁重的计算。这些外部函数通常是使用静态类型语言如 C 编写，并预先编译成库，然后可以导入到你的程序中。许多 Python 最受欢迎的库都是用 C 编写的，包括 numpy、TensorFlow 和 pandas。它们在 Python 接口的易用性和 C 的性能之间达到了完美的平衡。

然而，C 代码本身容易受到内存相关的错误影响，并不像现代编程语言那样对开发者友好。这就是为什么我建议使用 Rust 来编写你的 Python 扩展。

## 环境设置

为了构建我们的 Rust 扩展，我们将使用`[maturin](https://github.com/PyO3/maturin)`，这是一个零配置的 Rust 基础 Python 包构建和发布工具。我们还将使用`[PyO3](https://github.com/PyO3/pyo3)`来创建本地 Python 模块，以便在你的源代码中导入。

在本教程中，我们将编写一个简单的 Rust 库来计算给定数字的质因数。首先，让我们为我们的项目创建一个新目录：

```py
mkdir prime_fact
cd prime_fact
```

现在，让我们创建一个新的 Python 虚拟环境来安装`maturin`：

```py
python -m venv .venv
source .venv/bin/activate
pip install maturin
```

并初始化`maturin`项目：

```py
maturin init --bindings pyo3
```

该命令将生成一个 Rust 项目结构。我们感兴趣的文件是 `Cargo.toml` 和 `src/lib.rs`。前者包含有关 Rust 库的信息，而后者包含实际的 Rust 源代码。

## 编写 Rust 扩展

现在我们已经准备好了，是时候编写一些 Rust 代码了。我假设你已经有一些基本的 Rust 知识，但即使没有，你也应该能轻松跟上。打开你的 `src/lib.rs` 文件，并添加以下函数：

```py
// Include Python-related symbols
use pyo3::prelude::*;

// The 'pymodule' macro is used to implement Python modules
#[pymodule]
fn prime_fact(_py: Python, m: &PyModule) -> PyResult<()> {
    Ok(())
}
```

函数名称必须与 `Cargo.toml` 文件中的 `lib.name` 设置匹配。在这种情况下，`prime_fact` 是 Rust 包的名称，如 `Cargo.toml` 中所指定。

现在是时候为我们的 Rust 库添加一些功能了。让我们创建一个计算给定数字的质因数的函数：

```py
/// Calculates the prime factors of the given number.
// Use the macro 'pyfunction' to expose the function to the Python interface
#[pyfunction]
// The function takes in an unsigned 128-bit integer (u128)
// The return value is a Python object constructed from a vector of u128
fn factorize(mut n: u128) -> PyResult<Vec<u128>> {
    // Initialize an empty vector (equivalent of a list) for the factors
    let mut factors = Vec::new();

    // 2 is the only even prime, so try it first
    while n % 2 == 0 {
        factors.push(2);
        n /= 2;
    }

    let mut i = 3;
    while i <= n.sqrt() + 1 {
        if n % i == 0 {
            factors.push(i);
            n /= i;
        } else {
            i += 2;
        }
    }

    if n > 2 {
        factors.push(n);
    }

    // Return successfully the vector of factors
    Ok(factors)
}
```

要将新创建的函数添加到模块中，请按如下方式更新`prime_fact`模块函数：

```py
#[pymodule]
fn prime_fact(_py: Python, m: &PyModule) -> PyResult<()> {
    // Add the 'factorize' function to the module
    m.add_function(wrap_pyfunction!(factorize, m)?)?;
    Ok(())
}
```

我们已经完成了 Rust 库的编写。剩下的就是构建它：

```py
maturin develop
```

该命令将构建 Rust 包并在你当前的虚拟环境中将其安装为 Python 库。

如果你希望全局安装该库，请改为运行以下命令：

```py
# From within the project root directory 
maturin build
pip install .
```

如果你希望全局安装该包，请记得退出你可能处于的任何虚拟环境。

## 从 Python 导入和基准测试

要使用新创建的库，你只需要像普通的 Python 模块一样导入它：

```py
# Import the module
import prime_fact

n = 17376382193
# Call the function
factors = prime_fact.factorize(n)
print(f'{n} = {" * ".join(map(str, factors))}')
```

预期的输出是：

```py
17376382193 = 17 * 191 * 5351519
```

现在让我们将 Rust 实现的性能与其原生 Python 等效实现进行比较：

```py
use pyo3::prelude::*;
use num_integer::Roots;

/// Calculates the prime factors of the given number.
#[pyfunction]
fn factorize(mut n: u128) -> PyResult<Vec<u128>> {
    let mut factors = Vec::new();

    while n % 2 == 0 {
        factors.push(2);
        n /= 2;
    }

    let mut i = 3;
    while i <= n.sqrt() + 1 {
        if n % i == 0 {
            factors.push(i);
            n /= i;
        } else {
            i += 2;
        }
    }

    if n > 2 {
        factors.push(n);
    }

    Ok(factors)
}

/// A Python module implemented in Rust.
#[pymodule]
fn prime_fact(_py: Python, m: &PyModule) -> PyResult<()> {
    m.add_function(wrap_pyfunction!(factorize, m)?)?;
    Ok(())
}
```

```py
from prime_fact import factorize as rust_factorize
import math
import timeit

def py_factorize(n):
    factors = []

    while n % 2 == 0:
        factors.append(2)
        n = n / 2

    for i in range(3, round(math.sqrt(n)) + 1, 2):
        while n % i == 0:
            factors.append(i)
            n = n / i

    if n > 2:
        factors.append(n)

    return factors

# Benchmark

COUNT_NUMBER = 10
REPEAT_NUMBER = 5
NUMBER = 123456789061514

py_time = timeit.repeat(lambda: py_factorize(NUMBER), repeat=REPEAT_NUMBER, number=COUNT_NUMBER, globals=globals())
py_time = sum(py_time) / len(py_time)

rust_time = timeit.repeat(lambda: rust_factorize(NUMBER), repeat=REPEAT_NUMBER, number=COUNT_NUMBER, globals=globals())
rust_time = sum(rust_time) / len(rust_time)

print(f"Python: {py_time} seconds")
print(f"Rust:   {rust_time} seconds")
```

截断的输出是：

```py
Python: 2.531 seconds
Rust:   0.014 seconds
```

显然，Rust 实现比其 Python 等效实现要快几个数量级。

有关更多文档和示例，请查看 GitHub 上的 `[PyO3](https://github.com/PyO3/pyo3)`。

## 结论

总结一下，Rust 扩展是提升 Python 代码库执行速度和类型安全的绝佳工具。与 C 扩展相比，它们更安全、更易于实现，并且对开发者更友好。

希望你喜欢这篇文章。如果你有任何补充，请在评论中分享你的想法。感谢阅读！

如果你对了解如何加速 Python 以及如何访问对象属性感兴趣，我建议你查看下面的故事：

[## 通过 __slots__ 将您的 Python 代码库优化近 3 倍](https://betterprogramming.pub/optimize-your-python-programs-for-free-with-slots-4ff4e1611d9d?source=post_page-----108afdbd4e13--------------------------------)

### 通过添加一行代码来提高执行速度和内存使用效率

[优化您的 Python 程序，免费使用 __slots__](https://betterprogramming.pub/optimize-your-python-programs-for-free-with-slots-4ff4e1611d9d?source=post_page-----108afdbd4e13--------------------------------)
