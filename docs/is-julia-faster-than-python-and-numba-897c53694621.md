# Julia 比 Python 和 Numba 更快吗？

> 原文：[`towardsdatascience.com/is-julia-faster-than-python-and-numba-897c53694621`](https://towardsdatascience.com/is-julia-faster-than-python-and-numba-897c53694621)

## 优化

## Numba 非常快速，但它够快吗？

[](https://medium.com/@maclayton?source=post_page-----897c53694621--------------------------------)![Mike Clayton](https://medium.com/@maclayton?source=post_page-----897c53694621--------------------------------)[](https://towardsdatascience.com/?source=post_page-----897c53694621--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----897c53694621--------------------------------) [Mike Clayton](https://medium.com/@maclayton?source=post_page-----897c53694621--------------------------------)

·发布于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----897c53694621--------------------------------) ·16 分钟阅读·2023 年 9 月 19 日

--

![](img/69fa7749831310e9eb6a02054d9ee87d.png)

图片来源：[Stanos](https://unsplash.com/@stan_remnev?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)于[Unsplash](https://unsplash.com/photos/2Yj6MBvJ0sg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**Numba 是一个广泛使用的 Python 优化库，它将函数执行时间提升到接近 C 语言的水平，而 C 语言毫无疑问是快速的。**

**这种优化水平足以与像 Julia 这样较新的、专门构建的目标语言竞争吗？如果是的话，在 Python 中实现这种执行速度是否有任何陷阱？**

# 引言

![](img/4aa570daa0aa0c472205cffbdcfa200e.png)

图片来源：[Ann H](https://www.pexels.com/photo/yellow-letter-tiles-1888003/)于[Pexels](https://www.pexels.com/#)

我之前写过一篇对比 NumPy 和 Julia 的文章。结果基本上是 Julia 确实比 NumPy 快，总体而言。然而，事情比这更复杂一点，所以我建议你查看那篇文章，了解详细信息：

[](/is-julia-really-faster-than-python-and-numpy-242e0a5fe34f?source=post_page-----897c53694621--------------------------------) ## Julia 真的比 Python 和 Numpy 更快吗？

### C 语言的速度与 Python 的简单性

towardsdatascience.com

对那篇文章最常见的回应之一是类似于：

> 好吧，你也应该使用 Numba。它实现简单，而且更快！
> 
> -相当多人

…因此这篇文章将直接尝试解决这个建议。

使用 Numba 是否能与 Julia 的速度相匹配，甚至超越？它是否像人们声称的那样易于使用和实现？而且，有什么缺点吗？

让我们来看看……

# 关于 Julia 的简要介绍

![](img/0d756d1fe48b6f81c08fe8e1cfc26c38.png)

[Ann H](https://www.pexels.com/photo/yellow-letter-tiles-1888003/) 在 [Pexels](https://www.pexels.com/#) 上的照片

由于你们中的一些人可能还没有阅读关于 NumPy 的前一篇文章，我将重复那篇文章中包含的“什么是 Julia？”部分，但如果你已经阅读了前一篇文章，可以随意跳过。

## 什么是 Julia？

如果你对 Julia 一无所知，这里有一个快速入门指南。

[Julia](https://julialang.org/) 是一种开源语言，具有动态类型、直观且易于使用的特点，类似于 Python，但执行速度如同 C 语言一般。

它已经存在大约 11 年（诞生于 2012 年），所以它是一种相对较新的语言。然而，它已经发展到一个成熟的阶段，你不会把它称作是一时的潮流。

语言的原始创造者活跃于相关领域的工作中：

> 对于我们所做的工作——科学计算、机器学习、数据挖掘、大规模线性代数、分布式和并行计算——…
> 
> - [julialang.org](https://julialang.org/blog/2012/02/why-we-created-julia/) — Jeff Bezanson, Stefan Karpinski, Viral B. Shah, Alan Edelman

总的来说，它是一种**专门为数据科学领域设计的现代语言**。创造者的目标本身就能告诉你很多东西：

> 我们希望拥有 C 的速度和 Ruby 的动态特性。我们希望有一种语言，它是同构的，具有类似 Lisp 的真正宏，但也具备像 Matlab 那样明显的、熟悉的数学符号。我们希望它像 Python 一样适用于通用编程，像 R 一样适合统计，像 Perl 一样自然地处理字符串，像 Matlab 一样强大地进行线性代数，像 shell 一样擅长将程序粘合在一起。我们希望它易于学习，同时能够让最严肃的黑客感到满意。我们希望它既具交互性又具编译性。
> 
> （我们提到过它应该和 C 一样快吗？）
> 
> - [julialang.org](https://julialang.org/blog/2012/02/why-we-created-julia/) — Jeff Bezanson, Stefan Karpinski, Viral B. Shah, Alan Edelman

听起来很令人兴奋，对吧？

*顺便说一下，如果你想了解 Python 和 Julia 在语法和一般使用方面的对比，那么你可以查看我的另一篇文章，该文章深入探讨了使用 Julia（Flux）和 Python（TensorFlow）解决深度学习图像分类问题的情况：*

[](/julias-flux-vs-python-s-tensorflow-how-do-they-compare-574a3fb5e059?source=post_page-----897c53694621--------------------------------) ## Julia 的 Flux 与 Python 的 TensorFlow：它们如何比较？

### 一个端到端深度学习项目的比较

[towardsdatascience.com

# Numba 是什么，它为什么如此快速（以及受欢迎）？

![](img/2a1aaf170dbbf7c9485d0cd5023662a6.png)

图片由 [Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，在 [Unsplash](https://unsplash.com/photos/oZuBNC-6E2s?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Numba 的理念极其简单。

将 Python 代码预编译为机器代码，并执行编译后的代码，而不是 Python 代码。或者更详细地说：

> Numba 使用行业标准的 [LLVM](https://llvm.org/) 编译器库，将 Python 函数翻译为优化后的机器代码。用 Numba 编译的 Python 数值算法可以接近 C 或 FORTRAN 的速度。
> 
> -[numba.pydata.org](https://numba.pydata.org/)

要接近 C（或 FORTRAN）的速度，意味着 Numba 确实会非常快。

## 实现（通常）很简单

Numba 实现方式的一个主要优点是，在大多数情况下，它非常易于使用。以下是一个普通 Python 函数的例子，以及相应的 Numba 函数：

```py
#normal function
def loop_function(a, b):
    c = np.zeros(a.shape, dtype=np.float32)
    for i in np.arange(c.shape[0]):
        if a[i] < b[i]:
            c[i] = 1.0
        else:
            c[i] = 2.0
    return c, a + b

#the same function implemented with Numba implemented
@njit
def loop_function_numba(a, b):
    c = np.zeros(a.shape, dtype=np.float32)
    for i in np.arange(c.shape[0]):
        if a[i] < b[i]:
            c[i] = 1.0
        else:
            c[i] = 2.0
    return c, a + b
```

实际上只需一个装饰器！似乎不在每个函数上实现 Numba 就有些不妥。（稍后会更多地讲到现实情况……）

## 那我应该用 Numba 替换 NumPy 吗？

Numba 不是 NumPy 的替代品或替代选择。它的设计目的是**补充**NumPy。

> Numba 旨在与 NumPy 数组和函数一起使用。Numba 为不同的数组数据类型和布局生成专门的代码，以优化性能。特殊的装饰器可以创建 [通用函数](https://docs.scipy.org/doc/numpy/reference/ufuncs.html)，这些函数可以像 NumPy 函数一样对 NumPy 数组进行广播。
> 
> [-numba.pydata.org](https://numba.pydata.org/)

这非常出色，因为 NumPy 已经非常强大，而 Numba 进一步提升了它的性能。此外，如果你决定使用 Numba，也不需要重新编写所有的 NumPy 代码。

## Numba 的其他技巧

编译步骤只是开始。Numba 具有一整套附加功能，可以进一步提高执行速度。一些例子：

1.  [***并行处理***](https://numba.readthedocs.io/en/stable/user/parallel.html)— 如果你的 CPU 有多个核心，你可以利用它们进行并行处理，以加快计算速度

1.  [***快速数学***](https://numba.readthedocs.io/en/stable/user/performance-tips.html#fastmath) — 减少数值精度以提高执行速度

1.  [***缓存***](https://numba.readthedocs.io/en/stable/user/jit.html#cache) — 将编译后的代码保存在缓存中，以减少重复使用时的编译开销

1.  [***CUDA***](https://numba.readthedocs.io/en/stable/cuda/index.html)— 利用你的 GPU 进行计算

当然，除了上述四个选项之外，还有更多的选择，如果需要的话，还可以进行大量的自定义。本文将主要关注上面列表中的并行处理和 Fastmath。

使用 GPU (CUDA) 需要另外一篇文章来详细说明，因此在这篇文章中不会涉及。

## 缺点

Numba 主要有两个负面点。

第一个也是最明显的，就是**需要编译**，而且编译需要时间。

如果函数只执行一次，那么编译时间可能是一个显著的缺点。然而，如果代码需要重复使用编译的函数（例如在循环中），那么这个缺点可能会变得微不足道。这完全取决于情况。

> …需要编译，而且编译需要时间。

第二个，根据直接来自 Numba 网站的引用，是 Numba 只能在其设计用于的函数上实现。

> Numba 是一个开源 JIT 编译器，将**Python 和 NumPy 代码的一个子集**翻译成快速的机器代码。
> 
> [-numba.pydata.org](https://numba.pydata.org/)

我会指出，虽然提供的函数非常丰富，因此你可能不会觉得这是个问题，但这仍然是一个负面点。此外，还有相当多的内置灵活性，允许编写自定义代码，比如 [NumPy ufuncs](https://numba.readthedocs.io/en/stable/user/vectorize.html#)。因此，如果你有非常独特的需求，也有一些变通办法。

# 速度测试的基础

![](img/5c49768600b5c778213e8806c5df4352.png)

图片来源 [StockSnap](https://pixabay.com/users/stocksnap-894430/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2606859) [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2606859)

正如本文开头所提到的，这篇文章实际上是两篇之前文章的汇总。

第一个提供了如何利用 NumPy 矢量化来加速你的 Python 代码的指南：

[](/how-to-speedup-data-processing-with-numpy-vectorization-12acac71cfca?source=post_page-----897c53694621--------------------------------) ## 如何利用 NumPy 矢量化加速数据处理

### 比普通函数快多达 8000 倍

towardsdatascience.com

…一个自然的进展是查看 NumPy 及其矢量化实现与 Julia 的比较：

[](/is-julia-really-faster-than-python-and-numpy-242e0a5fe34f?source=post_page-----897c53694621--------------------------------) ## Julia 真的是比 Python 和 NumPy 更快吗？

### C 的速度与 Python 的简单性

towardsdatascience.com

…现在，主要是由于对之前 Julia 文章的评论，我认为观察 Julia 与 Numba 增加的功能相比如何将会很有趣。

那么，让我们开始吧！

# 测试将如何进行？

![](img/912092a1bd5b04350e215a97c5f87798.png)

图片由 [Nguyen Dang Hoang Nhu](https://unsplash.com/@nguyendhn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/cbEvoHbJnIE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

将测试三种不同的函数。每个函数的复杂度逐渐增加。

## 函数 1 — 简单的求和

以下函数的输入（**a** 和 **b**）定义为具有一百万个元素的一维数组/向量。每个元素是从正态分布中取出的随机数，类型为 float32。

对于 Python 函数，数组将是 NumPy 数组。

```py
# Example of an input array
series1 = np.random.randn(1000000).astype(np.float32)

# Python + NumPy + Numba
@njit
def sum_nums_numba(a, b):
    return a + b
```

```py
# Example of an input array
series1 = randn(MersenneTwister(12), Float32, 1000000);

# Julia
function sum_nums(a::Vector{Float32}, b::Vector{Float32})
    return a + b
end
```

## 函数 2 — 循环函数

循环是无处不在的，因此值得关注。

输入数组将与函数 1 相同。

```py
# Python + NumPy + Numba
@njit
def loop_function_numba(a, b):
    c = np.zeros(a.shape, dtype=np.float32)
    for i in np.arange(c.shape[0]):
        if a[i] < b[i]:
            c[i] = 1.0
        else:
            c[i] = 2.0
    return c, a + b
```

```py
# Julia
function loop_function(a::Vector{Float32}, b::Vector{Float32})
    c::Vector{Float32} = zeros(Float32, size(a))
    for i = 1:size(c)[1]
        a[i] < b[i] ? c[i] = 1.0 : c[i] = 2.0
 end
 return c, a + b
end
```

## 函数 3 — 矩阵操作

矩阵操作是数据科学（尤其是深度学习）领域许多算法和任务的关键组成部分，因此是一个重要的考虑因素。

输入到函数中的数据将是一个从正态分布中取出的 100 x 100 随机数矩阵。

```py
# Example of an input matrix
matrix1 = np.random.randn(100,100).astype(np.float32)

# Python + NumPy + Numba
@njit
def matrix_func(mat_a, mat_b):
    a = mat_a.T
    c = np.dot(mat_b,a)
    d = c.reshape(50,50,4)
    e = mat_b.reshape(50,4,50)
    f = d.reshape(200,50)
    g = e.reshape(50,200)
    h = np.dot(f,g)
    i = h.reshape(40000)
    result = 0.0
    for j in np.arange(i.shape[0]):
        result = result + (i[j] - np.sum(i)) / np.sqrt(abs(np.average(i)))
    return result
```

```py
# Example of an input matrix
matrix1 = randn(MersenneTwister(12), Float32, 10000);
matrix1 = reshape(matrix1,(100,100));

# Julia
function matrix_func(mat_a, mat_b)
    a = mat_a'
    c = mat_b * a
    d = reshape(c,(50,50,4))
    e = reshape(mat_b,(50,4,50))
    f = reshape(d,(200,50))
    g = reshape(e,(50,200))
    h = f * g
    i = reshape(h,40000)
    result = 0.0
    for j = 1:size(i)[1]
        result = result + (i[j] - sum(i)) / sqrt(abs(mean(i)))
    end
    return result
end
```

## 额外的调查

为了让信息更具参考性，还将研究以下项目：

1.  所有 Numba 函数都将分别在包含和不包含编译阶段的情况下进行计时。这将帮助判断编译对整体执行时间的影响。

1.  将进行单次迭代和多次迭代测试。再次是为了研究编译对执行时间的影响。

1.  除了“正常”运行函数外，还将比较 Numba 和 Julia 中并行处理的有效性。

1.  将研究 Numba 中 Fastmath 参数的额外好处

## 测量

函数的计时将使用 Python 中的 [timeit 模块](https://docs.python.org/3/library/timeit.html) 和 Julia 中的 [BenchmarkTools 模块](https://juliaci.github.io/BenchmarkTools.jl/stable/) 进行。

```py
iterations = 10000
timeit.timeit(stmt=numba_func, setup=setup, number=iterations)
```

```py
@benchmark sum_nums(rand_array1, rand_array2) samples=10000
```

## 一些常规信息（环境、版本等）

所有的测试都在完全相同的硬件上进行，该硬件使用的是 4 核心/8 线程 CPU（i7–4790K）（具体细节打印在 Jupyter notebooks 中）。

软件版本如下：

**Julia：** 1.9.2

**Python：** 3.11.4

**NumPy：** 1.23.5

**Numba：** 0.57.1

# 本文的 notebooks

![](img/7a23361e1a8b46a4e777f807d816a9df.png)

图片由 [Jessica Lewis](https://www.pexels.com/photo/ball-point-pen-on-opened-notebook-606541/) 提供，来源于 [Pexels](https://www.pexels.com/)

本文中生成结果所用的所有代码都完整地保存在两个 Jupyter notebooks 中，链接如下：

[](https://github.com/thetestspecimen/notebooks/tree/main/julia-numba-comparison?source=post_page-----897c53694621--------------------------------) [## notebooks/julia-numba-comparison at main · thetestspecimen/notebooks

### Jupyter 笔记本。通过在 GitHub 上创建帐户来为 thetestspecimen/notebooks 的开发做出贡献。

github.com](https://github.com/thetestspecimen/notebooks/tree/main/julia-numba-comparison?source=post_page-----897c53694621--------------------------------)

# 结果

![](img/cbe240e6d9f026ec916742b4a45f619a.png)

图片由 [Anna Nekrashevich](https://www.pexels.com/photo/magnifying-glass-on-top-of-document-6801648/) 提供，来源于 [Pexels](https://www.pexels.com/)

从……

## 一个简单的求和

首先在单次迭代中。

![](img/58ef2049cc84e1743e349b89f3e93e06.png)

对两个数组进行一次迭代的简单求和 — 图表由作者提供

这个第一次单次迭代的运行仅仅是为了说明，相较于整体执行时间，使用 Numba 时函数编译**可能**会带来显著的开销。

值得指出的是，要避免陷入仅仅因为 Numba 而使用它的陷阱，而不考虑它是否合适。如果你只需执行一个相对简单的函数一次，那么通常直接使用 NumPy 会更好，或者如果情况允许，至少利用[缓存](https://numba.readthedocs.io/en/stable/user/jit.html#cache)。

现在让我们增加迭代次数，以减少初始编译的影响。

![](img/89251f787087fcb1410f7b63d0fd1aa8.png)

对两个数组进行 10000 次迭代的简单求和 — 图表由作者提供

好了，就是这样。Julia 垫底了！

实际上，差异很小（10000 次迭代 — Julia[7.2s] — NumPy[5.1s]），但这仍然是一个差异。

更令人惊讶的是，即使忽略掉 Numba 的编译时间，Numba 的执行速度实际上比 NumPy 稍慢。

这很好地说明了*适当*矢量化的 NumPy 计算非常优化，这是在考虑 Numba 是否适合你的特定应用时需要考虑的另一个因素。

***注意：*** *有关 NumPy 矢量化的详细信息，以及它是如何工作的，请查看我之前的文章，我在其中详细讲解了：*

[](/how-to-speedup-data-processing-with-numpy-vectorization-12acac71cfca?source=post_page-----897c53694621--------------------------------) ## 如何通过 Numpy 矢量化加速数据处理

### 比普通函数快最多 8000 倍

towardsdatascience.com

## 循环函数

进入一个更现实且稍微复杂的场景，让我们看看一个循环函数。

本质上是逐个元素地运行一个包含 100 万个元素的数组，并根据 if-else 语句替换每个元素。

```py
# Python + NumPy + Numba
@njit
def loop_function_numba(a, b):
    c = np.zeros(a.shape, dtype=np.float32)
    for i in np.arange(c.shape[0]):
        if a[i] < b[i]:
            c[i] = 1.0
        else:
            c[i] = 2.0
    return c, a + b
```

![](img/575ccf1c2aa455dd85b03e8c61e739ff.png)

100 次迭代的循环函数（**Comp** — 包括函数编译时间，**Para** — 并行处理）— 作者图表

再次，Numba 占据了上风。

有趣的是，即使在 Numba 运行中包含编译时间，Numba 的速度仍然比 Julia 快。显然，如果迭代次数较少，这种领先优势将会减小，甚至**最终**会逆转。然而，执行阶段确实更快。

你还会注意到，在 Julia 和 Numba 的情况下，并行处理可以显著帮助。函数需要做一些轻微调整，但没有过于极端的改动。

对于 Julia，只需在 for 循环前添加 `Threads.@threads`：

```py
# parallelised Julia function
function loop_function_para(a::Vector{Float32}, b::Vector{Float32})
    c::Vector{Float32} = zeros(Float32, size(a))
    Threads.@threads for i = 1:size(c)[1]
        a[i] < b[i] ? c[i] = 1.0 : c[i] = 2.0
 end
 return c, a + b
end
```

对于 Numba，只需在装饰器中添加 `parallel=True`，并将 `np.arange` 替换为 `prange`（即 [并行范围](https://numba.pydata.org/numba-doc/latest/user/parallel.html?highlight=prange)）。

```py
@njit(parallel=True)
def loop_function_numba_par(a, b):
    c = np.zeros(a.shape, dtype=np.float32)
    for i in prange(c.shape[0]):
        if a[i] < b[i]:
            c[i] = 1.0
        else:
            c[i] = 2.0
    return c, a + b
```

通过使用 [Fastmath](https://numba.pydata.org/numba-doc/latest/user/performance-tips.html?highlight=fastmath#fastmath) 获得了额外的微秒，这本质上是通过降低精度来提高执行速度。这在机器/深度学习领域非常有用，因为在训练模型时不一定需要高数字精度。

只需对装饰器做一个简单的补充：

```py
@njit(parallel=True, fastmath=True)
def loop_function_numba_par_fast(a, b):
    c = np.zeros(a.shape, dtype=np.float32)
    for i in prange(c.shape[0]):
        if a[i] < b[i]:
            c[i] = 1.0
        else:
            c[i] = 2.0
    return c, a + b
```

## 矩阵操作

矩阵操作是数据科学工作流和深度学习/神经网络中不可或缺的一部分。

鉴于此，我觉得可能有趣的是看看 Julia 和 Numba 如何处理不同的矩阵操作链，包括：

+   普遍存在的点积

+   转置

+   重新形状

+   求和

+   平方根

+   绝对值

+   平均值

```py
# the Python code for reference
@njit
def matrix_func(mat_a, mat_b):
    a = mat_a.T
    c = np.dot(mat_b,a)
    d = c.reshape(50,50,4)
    e = mat_b.reshape(50,4,50)
    f = d.reshape(200,50)
    g = e.reshape(50,200)
    h = np.dot(f,g)
    i = h.reshape(40000)
    result = 0.0
    for j in np.arange(i.shape[0]):
        result = result + (i[j] - np.sum(i)) / np.sqrt(abs(np.average(i)))
    return result
```

![](img/11f13a8a0fd7e4593271f29348367508.png)

20 次迭代的矩阵操作（**Comp** — 包括函数编译时间，**Para** — 并行处理）— 作者图表

在这种情况下，Julia 表现最佳，差距相当大（大约快 10 倍）。为什么会这样尚难以解释，需要进一步调查。然而，我怀疑这与 Numba 的一些限制有关，我将在下一节进一步讨论。

另一个需要注意的地方是，由于这个函数的执行时间较长，相比于函数 1 和 2，编译时间在 20 次迭代时已经微不足道。

**最终**，Numba 如人们所说的那样快速，并且在**大多数情况下**使用起来非常简单。它轻松地将 Python 及其生态系统推向 Julia 的领域，整体上更新鲜。

但有一些注意事项…

# 局限性

![](img/1b13fc39d5096096b5e1dd1140a9f81e.png)

图片由 [RDNE Stock project](https://www.pexels.com/photo/woman-hand-holding-paper-bag-7564158/) 提供，来自 [Pexels](https://www.pexels.com/)

可以公平地说，Numba 确实能够跟上，有时甚至超越 Julia 的执行速度。

然而，Julia 和 Numba 之间有一个主要区别。Numba 是一个语言的外部库，而 Julia 中使用的方法是集成到核心语言中的原生方法。

> Numba 是一个语言的外部库，而 Julia 中使用的方法是集成到核心语言中的原生方法。

这基本上意味着，使用 Julia 你非常不容易遇到不兼容问题或方法应用限制。而对于 Numba，则不能这样说。Numba 的应用限制不仅在官方文档中被明确规定：

[支持的 Python 特性](https://numba.readthedocs.io/en/stable/reference/pysupported.html)

[支持的 NumPy 特性](https://numba.readthedocs.io/en/stable/reference/numpysupported.html)

……但由于不兼容性，你更可能会发现 bugs。

## 我发现的一些 bug

本文中的代码非常有限。比较简单的函数。然而，我仍然遇到了 Numba 的问题，因此不得不调整我的分析。

在最近对 2D 数组/矩阵（函数 3）进行的基准测试中，为了使函数在使用 Numba 时能够运行，必须进行各种调整。

**需要注意的是，使用纯 Python/NumPy 而不使用 Numba 时，以下问题都不存在。**

1.  你不能使用带有第二个参数的 `np.reshape`（即你不能指定重新排序类型‘F’，‘C’等）。这是一个问题，因为 Julia 和 Python 使用不同的默认矩阵索引顺序，尝试尽可能保持比较/公平需要查看这个第二个参数。

1.  `np.matmul` 不受支持，因此你必须使用 `np.dot`。这对像本文中使用的 2D 数组没有问题，但这些方法在高维数组中并不等效，因此如果你依赖于 `np.matmul` 处理高维数组，可能会遇到问题。

1.  [打开 bug](https://github.com/numba/numba/issues/5433) — 在转置后使用 `np.reshape` 需要进行复制，否则会失败

1.  [打开 bug](https://github.com/numba/numba/issues/6714) — `np.dot` 不支持整数数组。由于第 2 点，被迫从 `np.matmul` 切换到 `np.dot` 后，在测试中我遇到了另一个问题，然后才转向浮点数。

我在上述四个点上浪费了不少时间，因为不总是清楚你是否在做错事（即尝试实现不支持的内容），或者是在处理一个 bug。如果我处理的不是用于文章的“实验”代码，而是项目代码，那可能会变得相当令人沮丧。

# 结论

![](img/6480554e0174618f60b14cd1368bdc38.png)

由 [Ann H](https://www.pexels.com/photo/yellow-letter-tiles-1888003/) 提供的照片，来源于 [Pexels](https://www.pexels.com/#)

Numba 非常优秀，最重要的是它完全符合人们对它的所有评价：

**快速且易于实现**

…是的，总的来说，它基本上和 Julia 一样快。只要在适当的情况下使用。

然而，我不能忽视的是，与 Julia 直接比较时，Numba 有一些严重的局限性。当然，这些局限性的重要性会根据你的需求和特定约束有所不同。

在现实世界中，这基本上意味着，如果你的当前项目或基础设施依赖于 Python，而转到新语言太困难（缺乏经验丰富的开发者、太多的遗留代码、预算不足等）。那么由于像 NumPy 和 Numba 背后开发者的辛勤工作和坚持不懈，Python 在数据科学领域的速度和功能仍然是最新的。Numba 和 NumPy 非常有效地填补了这一空白。

然而，当你在 Julia 中编写代码时，你很清楚，基础语言在没有任何外部库的情况下已经进行了高度优化。它也是从根本上以数据科学为设计目标，因为这是创造者自己所需的：

> 对于我们所做的工作——科学计算、机器学习、数据挖掘、大规模线性代数、分布式和并行计算——…
> 
> - [julialang.org](https://julialang.org/blog/2012/02/why-we-created-julia/) — Jeff Bezanson, Stefan Karpinski, Viral B. Shah, Alan Edelman

你不需要了解或熟悉外部库和工具来确保你的项目在 Julia 中运行得很快。它默认就是很快的。这允许你更多地考虑你想要实现的目标，而不是不断考虑如何加快速度或优化。

这就是为什么**如果条件允许**，转向 Julia 是非常有意义的（在我看来！）。

***注意：*** *如果你想了解更多关于 Julia 与 Python（TensorFlow）在深度学习方面的对比，务必查看这篇文章：*

[](/julias-flux-vs-python-s-tensorflow-how-do-they-compare-574a3fb5e059?source=post_page-----897c53694621--------------------------------) ## Julia 的 Flux 与 Python 的 TensorFlow：它们的比较如何？

### 一个端到端深度学习项目的比较

towardsdatascience.com

如果你觉得这篇文章有趣或有用，记得关注我，或 [注册我的新闻通讯](https://medium.com/@maclayton/subscribe) 获取更多类似内容。

如果你还没有，可以考虑 [订阅 Medium](https://medium.com/@maclayton/membership)。你的会员费不仅直接支持我，还支持你阅读的其他作者。你还将获得 Medium 上每篇文章的完全无限制访问权限。

使用我的推荐链接注册将使我获得少量回扣，而不会影响你的会员资格，因此，如果你选择这样做，我非常感谢。

[](https://medium.com/@maclayton/membership?source=post_page-----897c53694621--------------------------------) [## 使用我的推荐链接加入 Medium - Mike Clayton

### 作为 Medium 会员，你的会员费的一部分将支付给你阅读的作者，而且你可以完全访问每一个故事…

medium.com](https://medium.com/@maclayton/membership?source=post_page-----897c53694621--------------------------------)
