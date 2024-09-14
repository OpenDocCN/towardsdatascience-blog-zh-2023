# 你已经成为高级 Pythonista 的 5 个迹象，你可能都没意识到

> 原文：[`towardsdatascience.com/5-signs-youve-become-an-advanced-pythonista-without-even-realizing-it-2b1dd7ef57f3`](https://towardsdatascience.com/5-signs-youve-become-an-advanced-pythonista-without-even-realizing-it-2b1dd7ef57f3)

## 时候到了，应该获得认可

[](https://ibexorigin.medium.com/?source=post_page-----2b1dd7ef57f3--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----2b1dd7ef57f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2b1dd7ef57f3--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----2b1dd7ef57f3--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----2b1dd7ef57f3--------------------------------)

·发表于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----2b1dd7ef57f3--------------------------------) ·阅读时间 8 分钟·2023 年 2 月 13 日

--

![](img/a2033ec4c4744971f2de38e2fa95ec63.png)

图片由 [Charles Thonney](https://pixabay.com/users/summerglow-20203311/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7120431) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7120431)

## 介绍

你已经编写 Python 代码一段时间了，编写脚本并解决各种问题。你认为自己已经很不错了，是不是？好吧，大家抓紧了，因为你可能已经成为一名高级 Pythonista，却未曾意识到！

从闭包到上下文管理器，我列出了一些高级 Python 特性，这些特性会让你感叹：“我一直在用这些！”

即使这些概念对你来说是新的，你也将拥有一个出色的检查清单，将你的技能提升到新的水平。

## 1. 作用域

高级 Python 编程的一个关键方面是对作用域概念的深入了解。

作用域定义了 Python 解释器在程序中查找名称的顺序。Python 的作用域遵循 **LEGB** 规则（本地、封闭、全局和内置作用域）。根据这一规则，当你访问一个名称（它可以是变量、函数或类）时，解释器会按顺序在 *本地*、*封闭*、*全局* 和 *内置* 作用域中查找。

让我们通过示例更好地理解每个级别。

**示例 1 — 本地作用域**

```py
def func():
    x = 10
    print(x)

func() # 10
print(x) # Raises NameError, x is only defined within the scope of func()
```

在这里，`x` 仅在 *本地* 于 `func` 的作用域中定义。因此，它在脚本的其他地方无法访问。

**示例 2 — 封闭作用域**

```py
def outer_func():
    x = 20
    def inner_func():
        print(x)
    inner_func()

outer_func() # 20
```

包含作用域是局部作用域和全局作用域之间的中介作用域。在上面的示例中，`x`在`outer_func`的局部作用域中。另一方面，`x`相对于嵌套的`inner_func`函数在*包含作用域*中。局部作用域总是具有对包含作用域的只读访问权限。

**示例 3 — 全局作用域**

```py
x = 30

def func():
    print(x)

func() # 30
```

在这里，`x`和`func`定义在全局作用域中，这意味着它们可以从当前脚本中的任何地方读取。

要在更小的作用域（局部和包含）中修改它们，应使用`global`关键字访问它们：

```py
def func2():
    global x
    x = 40
    print(x)

func2() # 40
print(x) # 40
```

**示例 4 — 内建作用域**

内建作用域包括所有已经定义的库、类、函数和变量，这些都**不需要**显式的导入语句。Python 中的一些内建函数和变量的例子包括`print`、`len`、`range`、`str`、`int`、`float`等。

## 2\. 函数闭包

对作用域的扎实掌握开启了另一个重要概念的大门——函数闭包。

默认情况下，函数执行完毕后，会返回到一个空白状态。这意味着其内存会被清除所有过去的参数。

```py
def func(x):
    return x ** 2

func(3)
```

```py
9
```

```py
print(x) # NameError
```

上面，我们将 3 的值分配给了`x`，但函数在执行后忘记了它。如果我们不想让它忘记`x`的值怎么办？

这就是*函数闭包*发挥作用的地方。通过在某个内函数的包含作用域中定义变量，你可以将其存储在内函数的内存中，即使函数返回后也能保持。

这是一个简单的示例函数，用于计算其执行次数：

```py
def counter():
    count = 0
    def inner():
        nonlocal count
        count += 1
        return count
    return inner

# Return the inner function
counter = counter()
print(counter()) # 1
print(counter()) # 2
print(counter()) # 3
```

```py
1
2
3
```

按照 Python 的所有规则，我们在第一次执行后应该已经丢失了`counter`变量。但是，由于它在内函数的闭包中，它会一直存在直到你关闭会话：

```py
counter.__closure__[0].cell_contents
```

```py
3
```

## 3\. 装饰器

函数闭包有比简单计数器更重要的应用。其中之一是创建装饰器。装饰器是一个嵌套函数，你可以将其添加到其他函数中以增强或甚至修改它们的行为。

例如，下面我们正在创建一个*caching*装饰器，它记住了函数的每一个位置参数和关键字参数的状态。

```py
def stateful_function(func):
    cache = {}
    def inner(*args, **kwargs):
        key = str(args) + str(kwargs)
        if key not in cache:
            cache[key] = func(*args, **kwargs)
        return cache[key]
    return inner
```

`stateful_function` 装饰器现在可以添加到可能会在相同参数上重复使用的计算密集型函数中。示例是以下递归斐波那契函数，它返回序列中的*n*th 数字：

```py
%%time

@stateful_function
def fibonacci(n):
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n-1) + fibonacci(n-2)

fibonacci(1000)
```

```py
CPU times: user 1.53 ms, sys: 88 µs, total: 1.62 ms
Wall time: 1.62 ms

[OUT]:

43466557686937456435688527675040625802564660517371780402481729089536555417949051890403879840079255169295922593080322634775209689623239873322471161642996440906533187938298969649928516003704476137795166849228875
```

我们在不到一秒的时间里找到了斐波那契序列中的第 1000 个巨大的数字。下面是没有缓存装饰器的情况下，执行相同过程所需的时间：

```py
%%time

def fibonacci(n):
    if n <= 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n-1) + fibonacci(n-2)

fibonacci(40)
```

```py
CPU times: user 21 s, sys: 0 ns, total: 21 s
Wall time: 21 s

[OUT]:

102334155
```

计算第 40 个数字花费了 21 秒。如果没有缓存，计算第 1000 个数字将需要几天时间。

你可以在我另一篇文章中了解如何创建自己的装饰器（包括作用域和闭包）的详细信息：

[](/an-in-depth-tutorial-to-python-decorators-that-you-can-actually-use-1e34d3d2d305?source=post_page-----2b1dd7ef57f3--------------------------------) ## 实用的 Python 装饰器深入教程

### 编辑描述

towardsdatascience.com

## 4\. 生成器

生成器是 Python 中强大的构造，可以高效地处理大量数据。

假设你在某个软件崩溃后有一个 10GB 的日志文件。为了找出出了什么问题，你必须在 Python 中高效地筛选它。

做这件事的最糟糕方法是像下面这样读取整个文件：

```py
with open("logs.txt", "r") as f:
    contents = f.read()

    print(contents)
```

由于你逐行处理日志，你不需要读取所有的 10GB，只需一次处理其中的块即可。这就是你可以使用生成器的地方：

```py
def read_large_file(filename):
    with open(filename) as f:
        while True:
            chunk = f.read(1024)
            if not chunk:
                break
            yield chunk # Generators are defined with `yield` instead of `return`

for chunk in read_large_file("logs.txt"):
    process(chunk)    # Process the chunk
```

上面，我们定义了一个生成器，它一次仅迭代日志文件的 1024 行。因此，末尾的`for`循环非常高效。在每次循环迭代中，内存中只有 1024 行文件内容。之前的块会被丢弃，而其余内容仅在需要时加载。

生成器的另一个特点是能够逐个生成元素，即使在循环之外，也可以使用`next`函数。下面，我们定义了一个极速的函数来生成斐波那契数列。

要创建生成器，你需要调用一次函数，然后在结果对象上调用`next`：

```py
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()

type(fib)
```

```py
generator
```

```py
print(next(fib)) # 0
print(next(fib)) # 1
print(next(fib)) # 1
print(next(fib)) # 2
print(next(fib)) # 3
```

你可以阅读下面的文章来了解更多关于生成器的信息。

[](https://realpython.com/introduction-to-python-generators/?source=post_page-----2b1dd7ef57f3--------------------------------) [## 如何在 Python 中使用生成器和 yield - Real Python

### 生成器函数是 PEP 255 引入的一种特殊函数，返回一个懒惰的迭代器。这些是……

realpython.com](https://realpython.com/introduction-to-python-generators/?source=post_page-----2b1dd7ef57f3--------------------------------)

## 5\. 上下文管理器

你一定已经使用上下文管理器很长时间了。它们允许开发者高效管理资源，如文件、数据库和网络连接。它们自动打开和关闭资源，从而使代码干净且无错误。

但是，使用上下文管理器和自己编写的上下文管理器之间有很大的区别。做得对的话，它们可以让你在原有功能的基础上抽象出很多样板代码。

一个流行的自定义上下文管理器的例子是计时器：

```py
import time

class TimerContextManager:
    """
    Measure the time it takes to run
    a block of code.
    """
    def __enter__(self):
        self.start = time.time()

    def __exit__(self, type, value, traceback):
        end = time.time()
        print(f"The code took {end - self.start:.2f} seconds to execute.")
```

上面，我们定义了一个`TimerContextManager`类，它将作为我们未来的上下文管理器。它的`__enter__`方法定义了当我们使用`with`关键字进入上下文时发生的事情。在这种情况下，我们启动了计时器。

在`__exit__`中，我们退出上下文，停止计时器，并报告经过的时间。

```py
with TimerContextManager():
    # This code is timed
    time.sleep(1)
```

```py
The code took 1.00 seconds to execute.
```

这是一个更复杂的示例，它使资源锁定，以便每次只能由一个进程使用。

```py
import threading

lock = threading.Lock()

class LockContextManager:
    def __enter__(self):
        lock.acquire()

    def __exit__(self, type, value, traceback):
        lock.release()

with LockContextManager():
    # This code is executed with the lock acquired
    # Only one process can be inside this block at a time

# The lock is automatically released when the with block ends, even if an error occurs
```

如果你想更温和地了解上下文管理器，请查看我关于这个话题的文章。

[](/how-to-build-custom-context-managers-in-python-31727ffe96e1?source=post_page-----2b1dd7ef57f3--------------------------------) ## 如何在 Python 中构建自定义上下文管理器

### 编辑描述

[towardsdatascience.com

如果你想深入了解并学习所有相关内容，这里还有一篇出色的 RealPython 文章。

[## 上下文管理器与 Python 的 with 语句 - Real Python](https://realpython.com/python-with-statement/?source=post_page-----2b1dd7ef57f3--------------------------------)

### 在本教程中，你将学习：你在编程中常遇到的一个问题是如何正确管理外部…

[realpython.com](https://realpython.com/python-with-statement/?source=post_page-----2b1dd7ef57f3--------------------------------)

## 结论

就这样，各位！你有多少次说过，“我知道这一点！”？即使不是很多次，你现在也知道了要学习的东西，以变得更高级。

不要害怕学习新事物带来的不适。只要记住，伟大的力量伴随着（我不说了！）更多挑战性的 bug 需要修复。但嘿，你现在是个高手了，小小的调试对你来说算什么？

感谢阅读！

喜欢这篇文章吗？让我们面对现实，它的奇特写作风格？想象一下能访问更多类似的文章，全部由一位才华横溢、迷人、机智的作者（顺便说一下，就是我 :））。

仅需 4.99 美元的会员费，你不仅可以访问我的故事，还能获取来自 Medium 上最优秀、最聪明头脑的知识宝藏。如果你使用 [我的推荐链接](https://ibexorigin.medium.com/membership)，你将获得我超级 nova 的感激和虚拟击掌以支持我的工作。

[## 通过我的推荐链接加入 Medium — Bex T.](https://ibexorigin.medium.com/membership?source=post_page-----2b1dd7ef57f3--------------------------------)

### 获取我所有⚡高级⚡内容的独家访问权，并在 Medium 上畅享无限支持我的工作，通过购买我…

[ibexorigin.medium.com](https://ibexorigin.medium.com/membership?source=post_page-----2b1dd7ef57f3--------------------------------) ![](img/c9f77aa48a95aa1c7ccc67e064f39990.png)
