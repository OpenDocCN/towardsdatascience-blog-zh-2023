# 发掘 Python 的全部潜力

> 原文：[`towardsdatascience.com/unlock-the-full-potential-of-python-bc78a980168b`](https://towardsdatascience.com/unlock-the-full-potential-of-python-bc78a980168b)

## 可读且高效的代码提示与技巧

[](https://tinztwinspro.medium.com/?source=post_page-----bc78a980168b--------------------------------)![Janik 和 Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----bc78a980168b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bc78a980168b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bc78a980168b--------------------------------) [Janik 和 Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----bc78a980168b--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bc78a980168b--------------------------------) ·阅读时间 9 分钟·2023 年 5 月 10 日

--

![](img/f2a8e5fcd50cccc27e31966429fead88.png)

照片由 [Maxwell Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral) 提供，刊登于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**Python** 是开发人员和数据科学家中最受欢迎的编程语言之一。开发人员使用该语言进行后台和前端开发。数据科学家和分析师使用 Python 进行数据分析。主要的机器学习库都可以在 Python 中使用。因此，Python 在数据科学家中非常受欢迎。

在这篇文章中，我们展示了一些 Python 的提示和技巧。这些提示和技巧将帮助你让 Python 代码更高效、更易读。你可以在下一个 Python 项目中直接使用这些技巧。我们将提示结构化成可以独立阅读的形式。阅读标题并决定哪个提示或技巧对你感兴趣！**现在就开始吧！**

# 使用 `enumerate()`

在 Python 中，你通常会对一个可迭代对象编写 `for()` 循环。因此，你不需要一个计数变量来访问元素。然而，有时确实需要一个计数器变量。有几种方法可以实现这样的 `for()` 循环。今天我们将展示两种变体。下面的示例代码展示了变体 A。

```py
# variant A

company_list = ["Tesla", "Apple", "Block", "Palantir"]

i = 0
for company in company_list:
    print(i, company)
    i+=1

# Output:
# 0 Tesla
# 1 Apple
# 2 Block
# 3 Palantir
```

在这个示例中，我们创建了一个计数变量 `i`。我们在每次迭代时递增这个计数变量。这种实现容易出错。你必须记住在每次迭代时更新 `i`。

你可以通过在循环中使用 `enumerate()` 来避免这个错误。不要将可迭代对象直接插入 `for()` 循环中，而是将可迭代对象放入 `enumerate()` 的括号中。下面的示例展示了它是如何工作的：

```py
# variant B

company_list = ["Tesla", "Apple", "Block", "Palantir"]

for count, company in enumerate(company_list, start=0):
    print(count, company)

# Output
# 0 Tesla
# 1 Apple
# 2 Block
# 3 Palantir
```

`enumerate()` 函数还允许你设置计数器的起始值。该函数从可维护性、可读性和效率方面改进了你的代码。

# 使用 `zip()`

Python 的`zip()`函数创建一个迭代器，将两个或更多可迭代对象的元素结合起来。使用这个迭代器，你可以解决常见的编程问题。我们展示了如何用实际例子使用 Python 函数`zip()`。

你可以将`zip()`函数视作一个物理的拉链。这种类比将帮助你理解`zip()`函数。`zip()`函数的参数是可迭代对象，并返回一个迭代器。这个迭代器创建一系列的元组。元组包含了每个可迭代对象中的元素。`zip()`函数接受任何类型的可迭代对象（例如文件、列表、元组、字典等）。以下代码块展示了它如何处理两个列表作为参数。

```py
list_a = [0,1,1]
list_b = [2,3,5]

zipped = zip(list_a, list_b)
list(zipped)

# Output
# [(0, 2), (1, 3), (1, 5)]
```

注意，`zip()`函数返回一个迭代器。我们需要使用`list()`来调用列表对象。在上面的示例中，迭代对象的长度是相同的。然而，也可能出现迭代对象长度不同的情况。我们现在将更详细地查看这种情况。

```py
list(zip(range(7), range(42)))

# Output
# [(0, 0), (1, 1), (2, 2), (3, 3), (4, 4), (5, 5), (6, 6)]
```

我们在上面的代码示例中看到使用了最短可迭代对象的长度。`zip()`函数忽略了较长可迭代对象中的剩余元素。从 Python 3.10 开始，`zip()`有了一个新的可选参数叫做 strict。这个参数的主要目的是提供一种安全的方式来处理不等长的可迭代对象。看看下面的代码示例。

```py
list(zip(range(7), range(42), strict=True))

# Output
# ---------------------------------------------------------------------------
# ValueError                                Traceback (most recent call last)
# Cell In[2], line 1
# ----> 1 list(zip(range(7), range(42), strict=True))
#
# ValueError: zip() argument 2 is longer than argument 1
```

我们得到一个 ValueError，因为长度不匹配。当你需要确保函数仅接受相同长度的可迭代对象时，`strict`参数非常有用。

对多个可迭代对象进行循环是 Python `zip()`函数最常见的使用案例之一。`zip()`函数对迭代序列（例如列表、字典）非常有用。我们展示了如何使用`zip()`同时运行多个可迭代对象。我们还将经典方法与`zip()`方法进行比较。以下代码示例展示了遍历两个列表的经典方式。

```py
from time import perf_counter

list_a = range(0, 10_000, 1)
list_b = range(10_000, 20_000, 1)

start = perf_counter()
for i in range(len(list_a)):
    a = list_a[i]
    b = list_b[i]
print(perf_counter()-start)

# Output
# 0.00775 s -> 7.75 ms
```

在这个例子中，我们迭代了两个列表`list_a`和`list_b`。在`for()`循环中，我们必须手动访问每个列表元素。这一过程增加了代码的运行时间。现在让我们来看看如何使用`zip()`函数改进代码。

```py
from time import perf_counter

list_a = range(0, 10_000, 1)
list_b = range(10_000, 20_000, 1)

start = perf_counter()
for a,b in zip(list_a, list_b):
    # do something
    pass
print(perf_counter()-start)
# Output
# 0.00093 s -> 0.93 ms
```

在这个例子中，我们再次遍历列表`list_a`和`list_b`。现在我们可以直接在`for()`循环中访问列表的各个元素。其优点是代码更简短且运行时间更短。

不要对数字中的`_`感到惊讶。解决方案在最后一个提示中。

# 解包和打包函数参数

我们来看一个接收两个参数的 add 函数。在下面，你将看到经典的实现方式，和你在其他编程语言中看到的一样。

```py
def add_function(a, b):
    print(a + b)

foo = 2
bar = 5
add_function(foo, bar)

# Output
# 7
```

在这个例子中，我们将 5 和 2 相加。结果是 7。在 Python 中，我们还可以将参数列表传递给函数。这真的很酷！

```py
# Unpacking
def add_function(a, b):
    print(a + b)

foo1 = [2, 5]
foo2 = {'a':2, 'b':5}
add_function(*foo1)
add_function(**foo2)

# Output
# 7
# 7
```

在这个例子中，我们使用*来解包列表，以便将其所有元素传递出去。此外，我们使用**来解包字典。

我们还可以传递任意数量的参数。这被称为打包。

```py
# Packing
def add_function(*args):
    print(sum(args))

add_function(1, 1, 2, 3, 5)

# Output
# 12
```

上面的`add_function()`将所有参数打包到一个变量中。通过打包的变量，我们可以像使用普通元组一样处理它。`args[0]`和`args[1]`给你第一个和第二个参数。

解包和打包使你的代码更少出错且更清晰。它如此简单，你可以直接在下一个项目中使用它。

# 就地交换值

许多开发者使用临时变量来缓存。实际上有一种**更简单更快速的方式**，无需创建额外的变量。请看下面的代码。

```py
# with additional tmp variable
a = 2
b = 4
tmp = a
a = b
b = tmp
print(a, b)

# Output
# 4 2

# without additional tmp variable
a = 2
b = 4
a, b = b, a
print(a, b)

# Output
# 4 2
```

你会发现这非常简单。然而，这种方法不仅限于变量。你也可以将其应用于列表。下面的代码展示了使用前几个斐波那契数的示例。

```py
example_list = [1, 1, 2, 3, 5]
example_list[0], example_list[3] = example_list[3], example_list[0]

print(example_list)

# Output
# [3, 1, 2, 1, 5]
```

在这个例子中，我们重新排列了列表元素的顺序。往往是简单的东西让你的代码更干净。记住这个提示用于你的下一个 Python 项目。

# 避免使用字典的`Get()`方法引发异常

作为一名 Python 开发者，你已经多次见过`KeyError`异常。目标是防止意外的`KeyError`异常发生。以下代码展示了一个小例子中的异常。

```py
dict={1: 2, 3: 4, 3: 5}
print(dict[8]) 

# Output
# ---------------------------------------------------------------------------
# KeyError                                  Traceback (most recent call last)
# Cell In[30], line 2
#      1 dict={1: 2, 3: 4, 3: 5}
# ----> 2 print(dict[8]) 
#
# KeyError: 8
```

停止此错误的一个解决方案是使用`.get()`函数。我们来使用`.get()`。

```py
dict={1: 2, 3: 4, 3: 5}
print(dict.get(8)) 

# Output
# None
```

现在，我们不会得到`KeyError`异常，因为我们使用了更安全的`.get()`方法。如果未找到键，返回值为 None（默认）。你还可以通过传递第二个参数来指定默认值。

```py
dict={1: 2, 3: 4, 3: 5}
print(dict.get(8), 0) 

# Output
# 0
```

在这个例子中，我们将默认返回值改为 0。`.get()`函数让你的代码更安全。

# 使用上下文管理器

Python 的上下文管理器是语言中一个强大的特性，用于管理资源、网络和数据库连接。诸如文件、网络或数据库连接等资源会在需要或不需要时自动设置和清理。上下文管理器在资源稀缺的项目或处理多个连接时非常有用。此外，上下文管理器还可以帮助处理错误。在 Python 中，`with`语句实现了上下文管理器。我们来看一个例子。

```py
import json
with open('config/config_timescaleDB.json') as timescaleDB_file:
    config_timescaleDB_dict = json.load(timescaleDB_file)
```

首先，我们来看语法。在`with`语句之后是上下文管理器表达式。这个表达式负责上下文管理器对象的设置。上下文管理器表达式的示例包括数据库连接或打开文件（如我们的例子）。`as`之后是一个可选变量，它接收上下文管理器的`__enter__()`方法的结果。在我们的例子中，这是 JSON 文件。接着是代码块。在代码块中，我们在上下文中执行代码。在我们的例子中，我们加载 JSON 文件并将内容保存到字典中。

在代码主体执行后，`with` 语句调用上下文管理器的 `__exit__()` 方法。`__exit__()` 方法执行清理操作（例如：关闭文件或释放资源）。实际上，`with` 语句至关重要，特别是在处理数据库连接时。使用上下文管理器可以确保数据库连接自动关闭。**高效的资源管理变得可能。**

使用上下文管理器时，许多手动工作被自动化处理。这导致代码中不是所有步骤都可见。因此，当你第一次查看代码时，很难理解。

上下文管理器应用于内存优化和清理资源管理，因为它们可以减少你的工作量。

# 处理大数字

你曾经在 Python 中处理过大数字吗？哪个数字更易读，1000000 还是 1,000,000？是第二个，对吧？不幸的是，第二种表示法在 Python 中不起作用。然而，在 Python 中，你可以使用 `_` 代替 `,`。我们来看一个例子。

```py
# bad way
million = 1000000     # 1 million
thousand = 1000        # 1 thousand
total = million + thousand
print(f'{total:,}')  

# Output
# 1,001,000

# smart way
million = 1_000_000     # 1 million
thousand = 1_000        # 1 thousand
total = million + thousand
print(f'{total:,}')

# Output
# 1,001,000
```

两种方法给出相同的结果。唯一的区别是大数字的表示方式。第二种方法更易读，避免了错误。

# 结论

在这篇文章中，我们学习了如何编写更清洁和更安全的代码。关键发现如下：

+   **在循环中使用 enumerate() 函数：你可以获得当前迭代的计数和该迭代项的值。**

+   **在循环中使用 zip() 以迭代多个可迭代对象。**

+   **使用拆包和打包来减少错误并使代码更清晰。**

+   **避免使用临时变量进行就地值交换。**

+   **使用字典的 .get() 方法避免 KeyError 异常。**

+   **使用上下文管理器来管理资源。**

+   **使用 _ 来处理大数字，使你的代码更具可读性。**

👉🏽 [**加入我们的免费每周魔法 AI 时事通讯，获取最新的 AI 更新！**](https://magicai.tinztwins.de)

👉🏽 [**你可以在我们的数字产品页面上找到所有免费资源！**](https://shop.tinztwins.de/)

[**免费订阅**](https://tinztwinspro.medium.com/subscribe) **以便在我们发布新故事时收到通知：**

[](https://tinztwinspro.medium.com/subscribe?source=post_page-----bc78a980168b--------------------------------) [## 订阅邮件以获取 Janik 和 Patrick Tinz 的最新发布。

### 每当 Janik 和 Patrick Tinz 发布时，你将收到一封电子邮件。通过注册，你将创建一个 Medium 账户，如果你还没有的话…

tinztwinspro.medium.com](https://tinztwinspro.medium.com/subscribe?source=post_page-----bc78a980168b--------------------------------)

了解更多关于我们的信息，请访问我们的 [关于页面](https://medium.com/@tinztwinspro/about)。不要忘记关注我们在 [X](https://twitter.com/tinztwins)。非常感谢你的阅读。如果你喜欢这篇文章，请随意分享。**祝你有个美好的一天！**

通过使用[我们的链接](https://tinztwinspro.medium.com/membership)注册 Medium 会员，即可阅读无限量的 Medium 故事。
