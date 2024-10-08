# 现代数据科学家必备的 Julia：5 个你不能忽视的卓越特性

> 原文：[`towardsdatascience.com/julia-for-the-modern-data-scientist-5-excellent-features-you-cant-ignore-c5b5b389547f`](https://towardsdatascience.com/julia-for-the-modern-data-scientist-5-excellent-features-you-cant-ignore-c5b5b389547f)

## 以趣味和机智进行解释

[](https://ibexorigin.medium.com/?source=post_page-----c5b5b389547f--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----c5b5b389547f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c5b5b389547f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c5b5b389547f--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----c5b5b389547f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c5b5b389547f--------------------------------) ·9 分钟阅读·2023 年 5 月 24 日

--

![](img/f36f5b7ab356b04818fef14c6844a5a1.png)

图片由我使用 Midjourney 制作。

是的，Python 的使用更为广泛。是的，它有更多的库。是的，我通过 Python 赚取生计，但这些并不能证明核心本地语言比 Julia 更好。

这就像 iOS 与 Android 的争论一样。仅仅因为更多的设备运行在 Android 上（Python 的许多使用案例），并且它有更多的第三方集成（Python 库），并不意味着 Android（Python）实际上比 iOS（基础的 Julia）更好。

事实上，有很多 iOS 功能是 Android 多年来一直羡慕的，尽管 Android 拥有庞大的用户基础。在这篇文章中，我们将看看 Julia 的一些特性，我相信 Python 开发者会喜欢这些特性。

## 1\. 速度

Julia 用户对其语言的速度感到异常自豪——就像对自己孩子的骄傲一样。他们这样做是对的。Julia 是历史上最快的语言之一，并且与 C、C++ 和 Fortran 一起，属于[PetaFlop 组](https://discourse.julialang.org/t/what-makes-a-language-reach-the-petaflop-mark/79963)。

> Petaflops 是一个计算速度单位，相当于每秒一千亿亿（¹⁰¹⁵）次浮点运算。

目前，大多数人没有能够达到 PetaFlops 的机器，因此我们只能尝试一些基本操作。我们先来比较一下 Flux.jl 和 TensorFlow，它们是这两种语言各自生态系统中的基石深度学习库。

我们将使用 TensorFlow 的 `GradientTape` 来对一个多项式进行微分并测量性能：

```py
import time
```

```py
import tensorflow as tfstart = time.time()
x = tf.Variable(5.0)with tf.GradientTape() as tape:
   y = 3 * x ** 3 + 4 * x ** 2 + 6 * x + 5
   # dy_dx = 9x² + 8x + 6
   dy_dx = tape.gradient(y, x)print(time.time() - start)
```

```py
[OUT]: 0.003016233444213867
```

第一次运行花费了超过一秒钟，所以我重新运行了代码块，让 TensorFlow 有一个开局优势。现在，让我们对 Julia 做同样的事情：

![](img/08d1a650e680dcd3bf9eda93f120c51f.png)

图片由我制作

第一次运行大约需要 ~0.002，已经比 TensorFlow 的起始速度快。Flux.jl 的第二次运行速度快了大约 450 倍！随着我们运行代码更多次，它对其他输入也变得更快。

如果你觉得比较深度学习库范围太窄，我们可以尝试原生 Python 和 Julia 代码。我们将基准测试一个返回 *第 n 个* Fibonacci 数字的简单函数，这样就足够了。

```py
import time
```

```py
def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)
```

我愚蠢地进行这次比较时不知道 Python 计算 Fibonacci 数列第 100 个数字需要多长时间。结果发现要花很长时间。

所以，我尝试了第 40 个数字，大约花了 21 秒：

```py
start = time.time()
```

```py
print(fib(40))
print("Execution time in Python: ", time.time() - start)
```

```py
Execution time in Python:  20.88260841369629
```

```py
[OUT]: 102334155
```

> 请记住，我故意选择了最慢的 Fibonacci 数字计算方法，以便查看在最坏情况下这些语言的比较。

现在，让我们尝试 Julia。

```py
using TimerOutputs
```

```py
function fib(n)
    if n == 0
        return 0
    elseif n == 1
        return 1
    else
        return fib(n-1) + fib(n-2)
    end
end@time fib(40)
```

```py
[OUT]:
102334155
```

```py
0.527796 seconds
```

Julia 的版本快了大约 40 倍。这就像现实生活中扭曲的乌龟与兔子的故事，最后兔子一直赢得比赛。

公平地说，NumPy 实际上在矩阵操作方面比 Julia 更快，但我们必须记住 NumPy 大部分是用 C 编写的。

## 2\. 互操作性

Julia 的灵感来源于多种语言，如 [Julia 的贪婪](https://julialang.org/blog/2012/02/why-we-created-julia/) 中所述。这使得 Julia 与其他语言友好，你可以轻松调用 C、Python、Fortran 或 R 代码。这允许这些语言之间的代码和库的平滑集成。

既然我们不关心其他内容（包括 R），我们将查看 `PyCall` 包，以运行像 Pandas 这样的 Python 库：

```py
using PyCall
```

```py
# Create a random array
a = rand(1000)# Import Pandas
pd = pyimport("pandas")data = Dict("name" => ["Alice", "Bob", "Charlie"],
            "age" => [25, 30, 35],
            "city" => ["New York", "Los Angeles", "Chicago"])# Create a dataframe
pd.DataFrame(data).head()
```

![](img/59f707f9a2cda85d5694dbdaa9404abd.png)

图片由作者提供

`pyimport` 函数将 Python 库加载到变量中。然后，我们可以在 Pandas 函数和类中使用 Julia 语法和对象，就像我们用 Julia 字典创建 DataFrame 一样。

`pd` 对象可以几乎与 Python 相同使用（`.plot()` 尚不支持）：

```py
penguins = pd.read_csv("data/penguins.csv")
```

```py
penguins.info()
```

```py
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 344 entries, 0 to 343
Data columns (total 7 columns):
 #   Column             Non-Null Count  Dtype  
---  ------             --------------  -----  
 0   species            344 non-null    object 
 1   island             344 non-null    object 
 2   bill_length_mm     342 non-null    float64
 3   bill_depth_mm      342 non-null    float64
 4   flipper_length_mm  342 non-null    float64
 5   body_mass_g        342 non-null    float64
 6   sex                333 non-null    object 
dtypes: float64(4), object(3)
memory usage: 18.9+ KB
```

```py
penguins.groupby("island").count()
```

![](img/157998acdb13c81e0883155220df20ad.png)

图片由我提供

## 3\. 数学支持

Julia 的主要卖点之一是它专注于科学计算，这意味着它对数学和统计有很好的支持。

首先，让我们看看 MATLAB 风格的多项式书写语法：

```py
x = 73
```

```py
4x² - 8x + x
```

```py
[OUT]: 20805
```

```py
(x + 1)x + 5x
```

```py
[OUT]:5767
```

写系数和变量时可以省略乘法符号。这使得代码更易读，更容易输入。

Julia 的安装还附带了一整套常用的数学函数。以下是幂、对数和根的列表：

![](img/32a9678ef214615d3a9c612eda8f59d9.png)

Julia 文档的截图。

以及大约 30 个三角函数：

![](img/136d517d95bc0e239ef4cf6f31585d24.png)

Julia 文档的截图。

所有这些函数直接从全局范围内提供 —— 不需要导入。在 Python 中，你需要 `math` 模块来进行基本数学运算，或者安装额外的依赖项，如 `NumPy` 或 `SciPy`。

我认为，对于一个被公认为科学计算之王的语言，不支持基本的微积分和线性代数，以及缺乏向量化功能是很尴尬的。

在 Julia 中，任何数组或矩阵使用任何函数（无论是原生函数还是用户定义函数）都可以实现向量化：

```py
array = [1, 2, 3]
```

```py
array .^ 5
```

```py
[OUT]:
```

```py
3-element Vector{Int64}:
   1
  32
 243
```

```py
array .+ x
```

```py
[OUT]:
```

```py
3-element Vector{Int64}:
 74
 75
 76
```

只需在任何函数前添加一个 `.`（点），即可实现向量化（对所有数组元素同时应用操作，而不是在循环中），这会带来显著的性能提升。

向量化对于命名函数也是可能的；只需在打开括号前加点：

```py
exp.(array)
```

```py
3-element Vector{Float64}:
  2.718281828459045
  7.38905609893065
 20.085536923187668
```

```py
log.(array)
```

```py
3-element Vector{Float64}:
 0.0
 0.6931471805599453
 1.0986122886681098
```

## 4. 多重分发

Julia 是实现多重分发的最佳语言之一。

多重分发是一种技术，允许一个函数具有多种方法实现，每种实现都专门针对特定的参数类型。这种技术被称为“通用函数”或“多方法”。

像 Common Lisp、Dylan 和 Scala 这样的语言也支持此功能。Julia 的实现独特在于它是内置于语言中的，并且在标准库中广泛使用，这使得该功能的使用更加一致和高效。Julia 的多重分发还设计为与其 JIT 编译器无缝配合，从而实现更快的代码执行。

这是一个简单的 `add` 函数的多重分发示例：

```py
function add(x::Number, y::Number)
    return x + y
end
```

```py
function add(x::String, y::String)
    return x * " " * y
end# Define methods for specific argument types
add(x::Number, y::Unitful.Quantity) = add(x, y.value)
add(x::Unitful.Quantity, y::Number) = add(x.value, y)
add(x::Unitful.Quantity, y::Unitful.Quantity) = add(x.value, y.value)
```

```py
# Usage of the function
julia> add(2, 3)
5
```

```py
julia> add("hello", "world")
"hello world"
```

```py
julia> add(2u"m", 3u"cm")
2.03m
```

即使你不懂 Julia，你也可以看到我们在定义五种不同数据类型对之间的加法操作：

1.  数字

1.  字符串

1.  数字和带单位的量，如米或厘米

1.  带单位的量和数字

1.  带单位的量

你可以为任意多的数据类型对定义 `add`，它每次都会有不同的表现，而不会有任何性能损失。

Python 在函数和类中使用鸭子类型来确定对象的类型和行为。这种鸭子类型虽然允许代码的灵活性，但比多重分发的预测性差且容易出错。

## 5. 命令行

我认为自 Guido Van Rossum 发布 Python 初始版本以来，Python 解释器就没有更新过。它实在是过于基础，几乎没有用。

至于 Julia 的 REPL，它类似于 Jupyter 但在终端中运行。让我们看看它的一些伟大功能，从帮助模式开始。

输入 `julia` 进入 REPL 后，你可以通过按下‘？’并输入名称来获取任何变量、函数或类的帮助：

![](img/6acd12b4c8515adc337732a85ba4dd72.png)

GIF 由作者提供

一旦按下‘？’，REPL 进入帮助模式。输出格式整齐，带有一些代码高亮，这已经比 Python 解释器的 `help` 函数好得多。

REPL 还有一个特殊的 `ans` 变量，用于存储最后一个表达式的结果。这类似于 Jupyter 中的 `_`（下划线）操作符，功能类似。

![](img/1184f027df7de02eb7586dd163edd7be.png)

GIF 由作者提供

在 REPL 模式下，您可能需要运行一两个终端命令，但又不希望丢失会话变量。在 Python 解释器中，这是不可能的。

在 Julia REPL 中，您可以通过在新行中按‘;’（分号）进入 Shell 模式。在那里，您可以运行任意数量的终端命令，然后通过按`Backspace`返回到会话中。

![](img/cb780a84ebda677c47c8b62291f523e4.png)

作者提供的 GIF

> 哦，我提到过 REPL 还有制表符补全功能吗？

REPL 还附带一些魔法命令，如`@time`：

```py
julia> @time println("Bam!")
Bam!
  0.000114 seconds (7 allocations: 144 bytes)
```

## 结论

在这篇文章中，我们看到了一些 Julia 相对于 Python 的最佳功能。虽然 Julia 要追赶 Python 还需要相当长的时间——绝对是几年——但它确实有了一个很好的开端。

分析[TIOBE 指数](https://www.tiobe.com/tiobe-index/)，我们可以看到，Julia 是相对于其初始发布而言增长最快的语言之一。这使得 Julia 成为任何从事数据科学、科学计算或需要高性能和表达性代码的领域的工作者值得关注的语言。

喜欢这篇文章和它那奇怪的写作风格吗？想象一下，您可以访问更多类似的文章，全部由一位才华横溢、迷人、风趣的作者（就是我 :）。

仅需 4.99 美元的会员费用，您不仅能获得我的故事，还有来自 Medium 上最聪明才智的知识宝库。如果您使用[我的推荐链接](https://ibexorigin.medium.com/membership)，您将获得我的**超级感激**和一个虚拟的击掌，以支持我的工作。

[](https://ibexorigin.medium.com/membership?source=post_page-----c5b5b389547f--------------------------------) [## 使用我的推荐链接加入 Medium — Bex T.

### 获取独家访问权限，享受所有⚡高级⚡内容，无限制地浏览 Medium。通过买一杯咖啡来支持我的工作…

ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----c5b5b389547f--------------------------------) ![](img/a01b5e4fb641db5f35b8172a4388e821.png)

图片由我提供。通过 Midjourney。
