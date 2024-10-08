# 加速你的 Python 技能

> 原文：[`towardsdatascience.com/speed-up-your-python-skills-in-2023-e680f4c56f37`](https://towardsdatascience.com/speed-up-your-python-skills-in-2023-e680f4c56f37)

## 七个技巧助你更上一层楼

[](https://tinztwinspro.medium.com/?source=post_page-----e680f4c56f37--------------------------------)![Janik 和 Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----e680f4c56f37--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e680f4c56f37--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e680f4c56f37--------------------------------) [Janik 和 Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----e680f4c56f37--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e680f4c56f37--------------------------------) ·阅读时间 9 分钟·2023 年 1 月 16 日

--

![](img/509f2e1b0475dabe336a49f134d00b80.png)

图片来源 [Emile Perron](https://unsplash.com/@emilep?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**Python** 是数据科学领域使用最广泛的编程语言，其受欢迎程度不断增长。近年来，整个数据科学领域也得到了巨大的发展。

在这篇文章中，我们将向你展示七个提高 Python 技能的技巧。通常是那些小细节能产生大的差异。这些提示将丰富你作为数据科学家的生活。因此，我们提供了七个可以立即实践的技巧。**保持好奇！**

作为数据科学家，你经常需要处理大量的数据。因此，你必须在运行时间和内存方面高效编码。你的 Python 代码也应该结构良好且易于阅读。这些提示将帮助你编写高效且易读的 Python 代码。

# 提示 1: 加速 NumPy

[NumPy](https://numpy.org) 是一个用于高效处理数组的 Python 库。它还提供了快速和优化的矢量化操作。但！它不支持并行处理。作为 NumPy 的替代品，你可以使用 [NumExpr](https://numexpr.readthedocs.io/en/latest/index.html)。

NumExpr 的性能显著优于 NumPy，因为它支持多线程。此外，它避免了为中间结果分配内存。

首先，你需要安装 NumPy 和 NumExpr 包。例如：

```py
$ pip install numpy numexpr
```

看看这个例子并尝试一下。

```py
import numpy as np
import numexpr as ne
import timeit

var1 = np.random.random(2**27)
var2 = np.random.random(2**27)

%timeit np.sin(var1) / np.cos(var2)
# 2.73 s

%timeit ne.evaluate("sin(var1) / cos(var2)")
# 566 ms
```

哇！该语句在 NumExpr 下执行速度大约提高了 5 倍。因此，如果你想加快 NumPy 语句的速度，这为你提供了一种实现方法。

NumExpr 在处理大数组时效果最佳。如果你有一个强大的多核计算机，它也能发挥其最大性能。因此，当这两个条件都满足时，我们推荐使用 NumExpr。对于小规模数组操作，你也可以使用 NumPy，因为性能差异非常小。原因是 NumExpr 将数组操作数拆分成小块，这些小块很容易适应 CPU 缓存。这些小块在 CPU 的可用核心之间分配，从而实现并行执行。

如果你想了解更多关于 NumExpr 的内容，可以查看[NumExpr 的 GitHub](https://github.com/pydata/numexpr)库。

# 提示 2：pandas apply()的快速替代方案

pandas 的 apply()函数可以在数据框的一个轴上执行函数。许多程序员将 apply()函数与 lambda 函数结合使用。但你如何提高 apply()函数的性能呢？

你可以使用 swifter 包。这个包将函数非常快速地应用于数据框或序列。pandas 的 apply()函数在一个核心上运行，而 swifter 则提供了多个核心支持。

首先，你需要安装 swifter 包。

```py
$ pip install swifter
```

安装后，你可以直接尝试它。

```py
import pandas as pd
import numpy as np
import swifter
import timeit

df = pd.DataFrame({'a': np.random.randint(7, 10, 10**7)})

# pandas apply()
%timeit  df.apply(lambda x: x**7)
# 54 ms

# swifter.apply()
%timeit  df.swifter.apply(lambda x: x**7)
# 37.5 ms
```

这个简单的例子显示了 swifter.apply()函数的运行时间更快。尤其是在多核的强大计算机上，这一差异尤其明显。如果你在下一个项目中需要性能提升，考虑使用 swifter 包。

# 提示 3：使用内置的 Python 函数

你经常实现一个函数却不知道它在 Python 中已经存在。尤其是如果你来自其他编程语言，如 C 或 C++。首先，你应该总是检查 Python 内置函数是否已经存在。Python 内置函数比自定义实现要快得多，因此你应该总是使用它们。以下示例演示了这一点。

```py
import numpy as np
from time import perf_counter

result_list = []
company_list = ["Tesla", "Block", "Palantir", "Apple"]
company_list_sample = np.repeat(company_list, 10**7)

start = perf_counter()
for company in company_list_sample:
  result_list.append(company.lower())
print(perf_counter()-start)
# 17.13 s

start = perf_counter()
result_list = map(str.lower, company_list_sample)
print(perf_counter()-start)
# 0.97 s
```

在上面的代码中，我们将一个包含四个条目的列表复制了 1 千万次，因此得到一个包含 4 千万个条目的列表。然后我们将列表中的字符串转换为小写。你可以看到，内置函数的速度约快了 17 倍。特别是在大量数据的情况下，这个提示带来了巨大的性能提升。所以要使用内置函数！

还有许多内置函数，如 min()、max()、all()等。如果你需要特定的 Python 函数，可以自己查找。**这很值得！**

# 提示 4：使用列表推导代替循环

程序员经常将列表与循环结合使用来存储计算结果。然而，这种方法在运行时间上效率不高。因此，最好使用列表推导，它具有更好的性能。以下示例显示了性能的差异。

```py
import numpy as np
from time import perf_counter

result_list_loop = []
result_list_com = []

number_round = 10000000

start = perf_counter()
for i in range(number_round):
  result_list_loop.append(i*i)
print(perf_counter()-start)
# 1.47 s

start = perf_counter()
result_list_com = [i*i for i in range(number_round)]
print(perf_counter()-start)
# 0.69 s

print(result_list_com[10])
# 100
```

从这个示例中我们学到了什么？尽可能使用列表推导式。列表推导式在编程中有些争议。一些程序员觉得语法难以阅读，因为一行代码表达了所有语句。在我们看来，语法清晰简洁。这是一个口味问题，但列表推导式的性能更好。

列表推导式以开括号[开始。然后是 for 循环中的计算。接着是循环头，包含三个元素（关键字 for，运行变量，循环长度）。列表推导式以闭括号]结束。一旦你理解了语法，你可以更紧凑地编写 for 循环。

那么在内存使用方面呢？我们如何减少内存空间？如果我们想对大列表进行进一步操作，这一点尤其建议。在我们的示例中，我们在列表中存储了 10000000 个值。但我们是否必须直接保存所有条目，还是仅在需要时才保存它们？

在这些情况下，我们可以使用生成器。生成器在需要时创建一个列表项。因此，生成器需要更少的内存并且运行时间更好。看看下面的示例。

```py
import sys 
from time import perf_counter

print(sys.getsizeof(result_list_com), 'bytes')
# 89095160 bytes

start = perf_counter()
result_gen = (i*i for i in range(number_round))
print(perf_counter()-start)
# 0.22 ms

print(sys.getsizeof(result_gen), 'bytes')
# 112 bytes

print(list(result_gen)[10])
# 100
```

我们可以执行与之前示例相同的所有操作。唯一的区别是我们现在使用的是()而不是[]。我们存储的是生成器而不是列表。这种方法在内存上更为高效。检查一下你是否可以在项目中使用列表推导式或生成器。它们可以提高性能并减少内存使用。

# 提示 5：使用双星号语法**合并字典

如何合并字典？你可以用一行代码来实现。我们使用了星号语法**。在下面的示例中，你可以看到它是如何工作的。

```py
dict_1 = {'company': 'Tesla', 'founding': 2002}
dict_2 = {'company': 'Tesla', 'founding': 2003, 'CEO': 'Elon Musk'}

dict_merged = {**dict_1, **dict_2}
print(dict_merged)
# {'company': 'Tesla', 'Founding': 2003, 'CEO': 'Elon Musk'}
```

首先，我们定义两个字典，它们有相同和不同的键值对。特斯拉成立于 2003 年，所以 dict_2 更加最新。如果两个字典包含相同的键但值不同，则使用最后一个字典的值。合并后，新字典包含所有三个键值对。语法简洁紧凑，因此合并非常简单。而且最棒的是，你可以合并三个或更多字典。这个技巧可以节省大量时间。

另一种方法是 update 方法。此方法更新第一个字典，并不创建副本。看看下面的示例。

```py
dict_1 = {'company': 'Tesla', 'founding': 2002}
dict_2 = {'company': 'Tesla', 'founding': 2003, 'CEO': 'Elon Musk'}

dict_1.update(dict_2)
print(dict_1)
# {'company': 'Tesla', 'Founding': 2003, 'CEO': 'Elon Musk'}
```

update 方法的缺点是你只能使用一个字典进行更新。如果你未来想要合并字典，请记住这个提示。

# 提示 6：不要导入不必要的模块

你可能听到过这个提示很多次，但它可以显著提高代码的性能。没有必要导入整个库。你通常只需要其中的某些函数。此外，由于必须先导入整个库，所以你的代码启动时间较长。这不应该是这样的。此外，你还必须通过点表示法访问单独的函数。这非常低效，你应该避免使用点表示法。以下示例演示了这一点。

```py
import math
from time import perf_counter

start = perf_counter()
variable = math.exp(7)
print(perf_counter()-start)
# 8.47-05 s
```

在这个示例中，我们使用了 `math.exp()` 函数和点表示法。这导致了代码性能不佳。此外，我们导入了整个 math 库，尽管我们只需要 `exp()` 函数。

```py
from math import exp
from time import perf_counter

start = perf_counter()
variable = exp(7)
print(perf_counter()-start)
# 4.51-05 s
```

在这个示例中，我们在没有点表示法的情况下导入了 `exp()` 函数。通过使用这个技巧，我们可以将代码的运行时间减半。哇，太棒了！

# 提示 7：使用即时编译器

[Numba](https://numba.readthedocs.io/en/stable/index.html) 是一个即时 (jit) 编译器，能够很好地与 NumPy 循环、数组和函数配合使用。装饰器用于指示 Numba 用 Numba 编译特定函数。Numba 将装饰过的函数即时编译为机器代码，以便所有或部分代码以本地机器代码的速度运行。

首先，我们需要通过 pip 安装 Numba。

```py
pip install numba
```

安装成功后，你可以使用 Numba。请查看以下示例：

```py
import numpy as np
from numba import jit
import timeit

var = np.random.random(10**7)
num_loop = 10000

def foo(var):
    result = 0
    for i in range(num_loop):
        result += 1
    result = np.sin(var) + np.cos(var)
    return result               
%timeit foo(var)
# 154 ms

@jit(nopython=True)
def foo_numba(var):
    result = 0
    for i in range(num_loop):
        result += 1
    result = np.sin(var) + np.cos(var)
    return result    

%timeit foo_numba(var)
# 76.3 ms
```

你可以看到，上述 foo 函数的装饰器加快了代码的执行速度。装饰器 *nopython=True* 表明编译将不涉及 Python 解释器。Numba 加速了循环和 NumPy 三角函数的执行。然而，它不能与所有 Python 函数一起使用。以下是 [Numba](https://numba.readthedocs.io/en/stable/user/5minguide.html) 的优缺点：

***缺点：***

+   Numba 不支持 pandas。

+   不支持的代码通过解释器执行，并且有额外的 Numba 开销。

+   仅对 M1/Arm64 提供非官方支持。

***优点：***

+   对 NumPy 数组和函数以及循环的支持非常好。

+   支持 Nvidia CUDA。它可以很好地用于基于 NumPy 的神经网络开发。

缺点和优点表明 Numba 应主要用于 NumPy 操作。此外，你应该总是在开始时检查 Numba 是否适合相应的实现。

# 结论

在这篇文章中，我们学习了如何提高代码的运行时间和内存效率。学到的教训：

+   NumPy 不支持并行处理。你可以使用 NumExpr 来处理这个问题。

+   Pandas 的 `apply()` 函数可以通过 swifter 加速。

+   检查是否有内置函数。

+   使用列表推导而不是循环。检查生成器是否适合你的项目。

+   使用双星号语法合并字典**。

+   不要导入不必要的模块。

+   如果你遇到运行时间问题，你可以使用即时编译器。即时编译器可以加速你的代码。

👉🏽 [**您可以在我们的数字产品页面上找到所有免费的资源！**](https://shop.tinztwins.de/)

👉🏽 [**加入我们的每周免费 Magic AI 通讯，获取最新的 AI 更新！**](https://magicai.tinztwins.de)

[**免费订阅**](https://tinztwinspro.medium.com/subscribe) **以便在我们发布新故事时收到通知：**

[](https://tinztwinspro.medium.com/subscribe?source=post_page-----e680f4c56f37--------------------------------) [## 每当 Janik 和 Patrick Tinz 发布新内容时，您将收到电子邮件。

### 每当 Janik 和 Patrick Tinz 发布新内容时，您将收到电子邮件。通过注册，您将创建一个 Medium 账户，如果您还没有的话…

[tinztwinspro.medium.com](https://tinztwinspro.medium.com/subscribe?source=post_page-----e680f4c56f37--------------------------------)

在我们的[关于页面](https://medium.com/@tinztwinspro/about)了解更多关于我们的信息。不要忘记在[X](https://twitter.com/tinztwins)上关注我们。非常感谢您的阅读。如果您喜欢这篇文章，请随意分享。**祝您一天愉快！**

使用[我们的链接](https://tinztwinspro.medium.com/membership)注册 Medium 会员，阅读无限制的 Medium 故事。
