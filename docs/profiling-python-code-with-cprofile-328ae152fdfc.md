# 使用 **cProfile** 对 Python 代码进行性能分析

> 原文：[`towardsdatascience.com/profiling-python-code-with-cprofile-328ae152fdfc`](https://towardsdatascience.com/profiling-python-code-with-cprofile-328ae152fdfc)

## 在这篇文章中，我们将探索如何使用 **cProfile** 模块对 Python 代码进行性能分析

[](https://pyshark.medium.com/?source=post_page-----328ae152fdfc--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----328ae152fdfc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----328ae152fdfc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----328ae152fdfc--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----328ae152fdfc--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----328ae152fdfc--------------------------------) ·阅读时长 6 分钟·2023 年 2 月 10 日

--

![](img/89cacd18fab67e3c61470469f4cd9bca.png)

图片由 [Arnold Francisca](https://unsplash.com/@clark_fransa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/f77Bh3inUpE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**目录**

+   介绍

+   什么是代码性能分析？

+   **cProfile** 基本使用

+   从终端使用 **cProfile** 对 Python 代码进行性能分析

+   使用 **cProfile** 对 Python 代码进行性能分析

+   使用 **cProfile** 在 Python 中对函数进行性能分析

+   导出 **cProfile** 数据

+   结论

# 介绍

现在程序员可以在几天内编写成千上万行代码。新程序和应用程序的复杂性不断演变，代码库包含多个函数，其中一些可能会减慢整个程序的性能。

专注于代码性能分析，特别是对你的 Python 代码进行性能分析以识别性能瓶颈，可以显著提高软件的性能，并改善用户体验。

在本教程中，我们将使用 Python 内置的 [cProfile](https://docs.python.org/3/library/profile.html) 模块，该模块提供 Python 程序的确定性性能分析。

# 什么是代码性能分析？

代码性能分析是分析程序性能的过程，特别是分析代码性能以识别潜在的瓶颈。

确定代码中运行缓慢的部分并优化这些代码可以显著提高软件的性能，减少内存使用和资源消耗。

在 Python 中，**cProfile** 性能分析工具可以跟踪 Python 脚本的执行时间和内存使用情况，帮助识别运行缓慢和高资源消耗的代码部分。

# **cProfile** 基本使用

这是一个使用 **cProfile** 进行性能分析和解释输出的快速示例：

```py
import cProfile
import re

cProfile.run("re.compile('Python')")
```

你应该得到：

```py
Python
         4 function calls in 0.000 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.000    0.000 :1()
        1    0.000    0.000    0.000    0.000 {built-in method builtins.exec}
        1    0.000    0.000    0.000    0.000 {built-in method builtins.print}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}Now let’s interpret the output:
```

现在让我们来解释输出结果：

+   *ncalls* — 调用次数

+   *tottime* — 在给定函数中花费的总时间

+   *percall* — *tottime* 和 *ncalls* 的比率

+   *cumtime* — 当前函数及其子函数的累计时间

+   *percall* — *cumtime* 和原始调用的比率

+   *filename* — 每个函数的数据

# 从终端使用 cProfile 对 Python 代码进行性能分析

在开始 Python 代码性能分析之前，我们需要一些示例 Python 代码来进行测试。

让我们创建一个简单的脚本，它将“Python Programming”打印 5 次，并将其命名为**main.py**：

```py
i=0
for i in range(5):
    print('Python Programming')
    i+=1
```

一旦你运行它，你应该得到：

```py
Python Programming
Python Programming
Python Programming
Python Programming
Python Programming
```

使用 cProfile 性能分析 Python 代码最简单的方法之一是从终端运行 cProfile。

打开终端，导航到你的 Python 脚本 (**main.py**) 所在的文件夹，并运行：

```py
python -m cProfile main.py
```

你应该得到：

```py
Python Programming
Python Programming
Python Programming
Python Programming
Python Programming
         8 function calls in 0.001 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.001    0.001 main.py:1()
        1    0.000    0.000    0.001    0.001 {built-in method builtins.exec}
        5    0.001    0.000    0.001    0.000 {built-in method builtins.print}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

# 使用 Python 的 cProfile 对 Python 代码进行性能分析

使用 cProfile 直接在 Python 脚本中分析 Python 代码是另一种方法。

你需要将 cProfile 模块导入到 Python 环境中，并显式调用性能分析函数，同时将 Python 代码作为字符串传递给该函数作为参数：

```py
import cProfile

cProfile.run("print('Python Programming')")
```

你应该得到：

```py
Python Programming
         4 function calls in 0.000 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.000    0.000 :1()
        1    0.000    0.000    0.000    0.000 {built-in method builtins.exec}
        1    0.000    0.000    0.000    0.000 {built-in method builtins.print}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

在这个示例中，我们将代码简化为仅打印“一行‘Python Programming’”，因为如你所见，将代码作为字符串传递给**cProfile.run()**并不是最方便的选项。

应该有更好的方法，对吧？当然！在下一节中，我们将探讨如何使用函数和**cProfile**对 Python 代码进行性能分析。

# 使用 Python 的 cProfile 对 Python 函数进行性能分析

让我们重用前面部分的 Python 代码，现在将其放入函数**my_func()**中：

```py
def my_func():
    i=0
    for i in range(5):
        print('Python Programming')
        i+=1
```

现在我们可以通过将该函数作为参数传递给**cProfile.run()**来轻松地对其进行性能分析：

```py
import cProfile

def my_func():
    i=0
    for i in range(5):
        print('Python Programming')
        i+=1

cProfile.run('my_func()')
```

你应该得到：

```py
Python Programming
Python Programming
Python Programming
Python Programming
Python Programming
         9 function calls in 0.001 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.001    0.001 :1()
        1    0.000    0.000    0.001    0.001 main.py:3(my_func)
        1    0.000    0.000    0.001    0.001 {built-in method builtins.exec}
        5    0.001    0.000    0.001    0.000 {built-in method builtins.print}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

# 导出 cProfile 数据

在前面的部分中，我们对 Python 代码进行了性能分析，结果在终端中打印出来。

那我们可以提取并保存分析数据吗？

是的！使用内置的**pstats**模块和**cProfile**，我们可以将分析结果提取并保存到一个简单的.txt 文件中。

请注意，**profile.run()** 功能定义如下：

```py
profile.run(command, filename=None, sort=- 1)
```

当 *filename* 设置为 **None** 时，它会自动打印出性能分析报告。

现在，让我们将 *filename* 设置为一些示例文件名，如‘results’，并运行上一节的代码：

```py
import cProfile

def my_func():
    i=0
    for i in range(5):
        print('Python Programming')
        i+=1

cProfile.run('my_func()', 'results')
```

你会注意到终端中的输出仅仅是函数应该生成的内容，但没有性能分析报告：

```py
Python Programming
Python Programming
Python Programming
Python Programming
Python Programming
```

现在，你会在项目目录中创建一个名为‘results’的新文件：

![](img/f3e0452cb4ac9b383083201cae14a716.png)

图片由作者提供

我们有了包含分析报告的文件，但我们还不能打开它。现在我们需要使用**pstats**模块将其转换为.txt 文件，然后才能访问报告。

这段额外的代码将把‘results’文件转换为‘results.txt’：

```py
import pstats

with open('results.txt', 'w') as file:
    profile = pstats.Stats('results', stream=file)
    profile.print_stats()
    file.close()
```

你应该在目录中看到一个新文件：

![](img/a227b4c0de84c899318c356e98872f76.png)

作者提供的图片

现在我们已经成功创建了一个包含分析报告的.txt 文件。它应包含以下数据：

![](img/91a6ba3791355fd7e9174beedf3e975e.png)

作者提供的图片

# 提取 cProfile 数据的完整代码

```py
import cProfile
import pstats

def my_func():
    i=0
    for i in range(5):
        print('Python Programming')
        i+=1

cProfile.run('my_func()', 'results')

with open('results.txt', 'w') as file:
    profile = pstats.Stats('results', stream=file)
    profile.print_stats()
    file.close()
```

# 结论

在这篇文章中，我们探讨了如何使用[**cProfile**](https://docs.python.org/3/library/profile.html#module-cProfile)模块来分析 Python 代码。

代码分析帮助识别代码中的瓶颈，并帮助理解哪些部分的代码应该优化以提高整体性能。

如果您有任何问题或对某些编辑有建议，请随时在下面留言，并查看我的更多[Python 编程](https://pyshark.com/category/python-functions/)教程。

*最初发表于* [*https://pyshark.com*](https://pyshark.com/profiling-python-code-with-cprofile/) *2023 年 2 月 10 日。*
