# Python 元组，真相大白，只有真相：你好，元组！

> 原文：[`towardsdatascience.com/python-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d`](https://towardsdatascience.com/python-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d)

## PYTHON 编程

## 学习元组的基础知识及其使用方法

[](https://medium.com/@nyggus?source=post_page-----12a7ab9dbd0d--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----12a7ab9dbd0d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----12a7ab9dbd0d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----12a7ab9dbd0d--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----12a7ab9dbd0d--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----12a7ab9dbd0d--------------------------------) ·阅读时长 16 分钟·2023 年 1 月 21 日

--

![](img/74860d4642efa6cd08de3d7411205690.png)

元组通常被视为记录。照片由[Samuel Regan-Asante](https://unsplash.com/@fkaregan?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

元组是 Python 中的一种不可变集合类型。它是 Python 中三种最流行的集合类型之一，另外两种是列表和字典。虽然我认为许多初学者和中级开发者对这两种类型了解颇多，但他们可能在真正理解元组是什么以及如何工作上存在问题。即使是高级 Python 开发者也不必了解所有关于元组的知识——鉴于这种类型的特殊性，我对此并不感到惊讶。

作为一个初学者甚至中级 Python 开发者，我对元组了解不多。让我给你一个例子；想象一下我写了一段类似于以下的代码：

```py
from pathlib import Path

ROOT = Path(__file__).resolve().parent

basic_names = [
    "file1",
    "file2",
    "file_miss_x56",
    "xyz_settings",
]
files = [
    Path(ROOT) / f"{name}.csv"
    for name in basic_names
]
```

如你所见，我使用了列表字面量来定义`basic_names`列表——但为什么不使用*元组字面量*呢？它看起来会是下面这样：

```py
basic_names = (
    "file1",
    "file2",
    "file_miss_x56",
    "xyz_settings",
)
```

关于元组，我们知道的主要事情是它是不可变的——代码本身表明`basic_names`容器将*不会*改变。因此，元组在这里似乎比列表更自然，对吧？那么，两种方法之间是否存在*实际*差异？比如性能、安全性或其他方面？

知识上的这些空白使我们成为更差的程序员。本文旨在通过帮助你了解 Python 中一个非常重要但许多人不了解的数据类型：元组，从而帮助你成为更好的程序员。我的目标是使这篇文章从实际角度尽可能详尽。因此，例如，我们不会讨论元组的 C 语言实现细节，但会讨论在 Python 中使用元组的细节。

元组是一个丰富的话题。因此，我将把关于它的知识分为两部分——和两篇文章。以下是我将在第一部分中覆盖的主题——也就是这里：

+   元组的基础。

+   使用元组：元组解包和元组方法。

因此，我们将在这里专注于基础知识。在第二部分，我将覆盖元组的更多高级主题，例如继承自元组、元组性能和元组推导。你可以在这里找到它：

[](/python-tuple-the-whole-truth-and-only-truth-lets-dig-deep-24d2bf02971b?source=post_page-----12a7ab9dbd0d--------------------------------) [## Python 元组，完全的真相和唯一的真相：让我们深入探讨]

### 了解元组的复杂性

towardsdatascience.com

# 元组的基础知识

元组是一个值的容器，类似于列表。在他伟大的著作《*流畅的 Python*》中，L. Ramalho 解释说，元组是为了成为*不可变的列表*而创建的，这个术语很好地描述了元组的本质。但他也提到，元组不仅仅是不可变的列表；它们远不止于此。

特别是，元组可以用作没有字段名称的记录。这意味着我们可以有一个包含几个未命名字段的记录。当然，这种基于元组的记录只有在每个字段的含义明确时才有意义。

当你想在 Python 中使用元组字面量创建元组时，你需要使用圆括号 `()` 而不是方括号 `[]`，就像创建列表时一样¹：

```py
>>> x_tuple_1 = (1, 2, 3)
>>> x_tuple_1
(1, 2, 3)
>>> x_tuple_2 = ([1, 2], 3)
>>> x_tuple_2
([1, 2], 3)
```

这里，`x_tuple_1 = (1, 2, 3)` 创建了一个包含数字 `1`、`2` 和 `3` 的三元素元组；`x_tuple_2 = ([1, 2], 3)` 创建了一个包含两个值的两元素元组：一个列表 `[1, 2]` 和数字 `3`。如你所见，你可以在元组中使用任何类型的对象。你甚至可以创建一个空元组的元组：

```py
>>> tuple((tuple(), tuple()))
((), ())
```

尽管，说实话，我不知道你为什么会想这样做。

好的，我们上面使用了元组字面量。创建元组的第二种方法是使用内置的 `tuple()` 类。只需提供一个可迭代对象作为参数，这将把可迭代对象转换为元组：

```py
>>> tuple([1, 2, 5])
(1, 2, 5)
>>> tuple(i for i in range(5))
(0, 1, 2, 3, 4)
```

要访问元组中的值，你可以使用典型的索引：`x_tuple_1[0]` 将返回 `1`，而 `x_tuple_2[0]` 将返回一个列表 `[1, 2]`。注意，因为 `x_tuple_2[0]` 是一个列表，所以你可以使用它的索引来访问它的元素——因此，你将使用多个（在这里是双重）索引；例如，`x_tuple_2[0][0]` 将返回 `1`，而 `x_tuple_2[0][1]` 将返回 `2`。

列表和元组之间最大的区别在于列表是可变的，所以你可以改变它们，而元组是不可变的，所以你不能改变它们：

```py
>>> x_list = [1, 2, 3]
>>> x_tuple = (1, 2, 3)
>>> x_list[0] = 10
>>> x_list
[10, 2, 3]
>>> x_tuple[0] = 10
Traceback (most recent call last):
    ...
TypeError: 'tuple' object does not support item assignment
```

如你所见，你不能对元组进行项赋值。这一特性使得元组比列表更不容易出错，因为你可以确定（实际上，几乎可以确定，我们将下文讨论）元组不会改变。然而，你可以确定的是，它们的长度不会改变。

有一个关于元组的常见面试问题：*由于元组是不可变的，你不能改变它们的值，对吗？* 对这个问题的回答是：*嗯*…

这是因为你可以改变元组中可变元素的值：

```py
>>> x_tuple = ([1, 2], 3)
>>> x_tuple[0][0] = 10
>>> x_tuple
([10, 2], 3)
>>> x_tuple[1] = 10
Traceback (most recent call last):
    ...
TypeError: 'tuple' object does not support item assignment
```

所以，尽管元组是不可变的，但如果它们的元素不是，你可以改变这些元素，因此，至少间接地，你可以改变元组。这使得改变一个不可变的东西成为可能…

如果你感到困惑，至少要意识到你并不孤单。你只是其中之一。然而，这种不可变性至少在理论上是有意义的，所以让我解释一下这里发生了什么。

整个真相在于以下几点。像其他集合一样，元组不包含对象，而是包含对它们的引用；不可变意味着在这些引用方面是不可变的。因此，一旦创建，元组将始终包含相同的引用集合。

+   *理论上*，当一个元组引用的对象发生变化时，元组保持不变：它仍然是完全相同的元组，具有完全相同的引用。

+   *实际上*（也就是说，从我们典型/自然的角度来看），当一个元组引用的对象发生变化时，元组*似乎*已经改变：尽管引用完全相同，一个对象发生了变化，因此，从实际情况来看，元组*看起来*与变化前不同。但在理论上，元组（一个引用的集合）没有发生任何变化。

> 像其他集合一样，元组不包含对象，而是包含对它们的引用；不可变意味着在这些引用方面是不可变的。

好了，现在我们知道了元组的不可变性是如何工作的，我们应该记住也要以这种方式来看待元组。但知道某件事并不意味着习惯它会很容易。以这种方式思考不可变性并不容易。记住，从现在开始，你应该记住元组是对对象的不可变引用集合，而不是对象的不可变集合。元组包含的对象的值实际上可以改变——但对象必须保持不变……已经觉得头疼了吗？这只是开始…

让我们考虑一下典型的元组长度。然而，为了增加一些背景，我们应该考虑它在列表中的表现。我认为可以安全地说，短列表和长列表都经常使用。你可以通过多种方法创建列表，比如字面量、`for`循环、`list()`方法和列表推导。

元组是不可变的，它们并不像那样工作。你不能在`for`循环中更新它们（除非你在更新它们的可变元素）或在推导式中更新它们。你可以用两种方式创建一个元组，使用元组字面量，比如这里：

```py
>>> x = (1, 56, "string")
```

或调用`tuple()`类（`tuple()`是一个可调用类）对一个可迭代对象：

```py
>>> x = tuple(x**.5 for x in range(100))
```

我猜前一种用法要频繁得多。也许元组最常见的用法是从函数中返回值，特别是当返回两个或三个值时（你很少（如果有的话）会为十个值这么做）。

当元组字面量很短时，通常会省略括号：

```py
>>> x = 1, 56, "string"
```

这种方法通常与 `return` 语句一起使用，但不仅限于此。带括号和不带括号的两种方式中哪一种更好？一般来说，没有哪一种更好；但这要视情况而定。有时，括号会使代码更清晰，有时则不需要括号。

请记住非括号元组，因为它们可能成为难以发现的错误来源；见这里：

[找到 Python 代码中的 bug：小细节作用大](https://betterprogramming.pub/find-a-bug-in-python-code-a-little-thing-does-so-much-89d0abd0300c?source=post_page-----12a7ab9dbd0d--------------------------------)

### 即使是最小的字符也可能引发大问题

[更好编程](https://betterprogramming.pub/find-a-bug-in-python-code-a-little-thing-does-so-much-89d0abd0300c?source=post_page-----12a7ab9dbd0d--------------------------------)

简而言之，当你忘记在行末添加逗号时，你可能会将一个对象作为元组而不是单独的对象来使用：

```py
>>> x = {10, 20, 50},
```

你可能认为 `x` 是一个包含三个元素的集合，但实际上它是一个包含一个元素的元组：

```py
>>> x
({10, 20, 50},)
```

正如你所见，这一个单独的逗号放在右大括号后面，而不是前面，使得 `x` 成为了一个一元素的元组。

# 元组的实际应用

元组提供的方法比列表少，但仍然有不少。有些方法比其他方法更为人所知；有些方法甚至非常少为人知晓且使用得不频繁。在本节中，我们将探讨使用元组的两个重要方面：元组方法和元组解包。

## 解包

元组的一个极好的特性是 *元组解包*。你可以用它将一个元组的值一次性赋给多个变量。例如：

```py
>>> my_tuple = (1, 2, 3,)
>>> a, b, c = my_tuple
```

在这里，`a` 将变为 `1`，`b` 将变为 `2`，而 `c` 将变为 `3`。

考虑以下示例：

```py
>>> x_tuple = ([1, 2], 3)
>>> x, y = x_tuple
>>> x
[1, 2]
>>> y
3
```

你还可以使用带有星号 `*` 的特殊解包语法：

```py
>>> x_tuple = (1, 2, 3, 4, 5)
>>> a, b* = x_tuple
>>> a
1
>>> b
[2, 3, 4, 5]

>>> *a, b = x_tuple
>>> a
[1, 2, 3, 4]
>>> b
5

>>> a, *b, c = x_tuple
>>> a
1
>>> b
[2, 3, 4]
>>> c
5
```

正如你所见，当你将星号 `*` 附加到一个变量名时，就像是在说：“将这个项及所有接下来的项解包到这个名字中。”所以：

+   `a, b*` 意味着将第一个元素解包到 `a`，所有剩余的元素解包到 `b`。

+   `*a, b` 意味着将最后一个元素解包到 `b`，所有之前的元素解包到 `a`。

+   `a, *b, c` 意味着将第一个元素解包到 `a`，最后一个元素解包到 `c`，所有中间的元素解包到 `b`。

当元组中的元素更多时，你可以考虑更多场景。想象一下你有一个包含七个元素的元组，而你对前两个和最后一个感兴趣。你可以用解包的方式将它们获取并赋值给变量，如下所示：

```py
>>> t = 1, 2, "a", "ty", 5, 5.1, 60
>>> a, b, *_, c = t
>>> a, b, c
(1, 2, 60)
```

这里还要注意一点。我使用了 `*_`，因为我只需要提取这三个值，其他值可以忽略。这里，下划线字符 `_` 正是表示这一点：我不关心这些元组中的其他值，因此让我们忽略它们。如果你使用名称，代码的读者会认为该名称在代码中被使用——但你的 IDE 也会对分配给一个在作用域中未被使用的名称而发出警告²。

元组解包可以用于各种场景，但当你赋值时，特别是从返回元组的函数或方法中获得值时，它特别有用。下面的例子展示了从函数/方法返回值中解包的有用性。

首先，让我们创建一个 `Rectangle` 类：

```py
>>> @dataclass
... class Rectangle:
...     x: float
...     y: float
...     def area(self):
...         return self.x * self.y
...     def perimeter(self):
...         return 2*self.x + 2*self.y
...     def summarize(self):
...         return self.area(), self.perimeter()
>>> rect = Rectangle(20, 10)
>>> rect
Rectangle(x=20, y=10)
>>> rect.summarize()
(200, 60)
```

如你所见，`Rectangle.summarize()` 方法返回两个组织在元组中的值：矩形的面积和周长。如果我们想将这些值分配给名称，我们可以这样做：

```py
>>> results = rect.summarize()
>>> area = result[0]       # poor!
>>> perimeter = result[1]  # poor!
```

然而，上述方法并不是一个好的选择，尤其是出于清晰性考虑，我们可以使用元组解包更有效地完成这个任务：

```py
>>> area, perimeter = rect.summarize()
>>> area
200
>>> perimeter
60
```

如你所见，它更加清晰简洁：只需一行而不是三行。此外，它不使用索引来从元组中获取值。索引降低了可读性，使用名称而非位置会更好。我们将在下面的部分讨论，从 `tuple` 类继承和命名元组。但请记住，当一个函数/方法返回一个元组——这是一种相当常见的情况——你应该解包这些值，而不是直接使用元组索引分配它们。

另一个例子，也使用 `dataclass`³：

```py
>>> from dataclasses import dataclass
>>> KmSquare = float
>>> @dataclass
... class City:
...     lat: float
...     long: float
...     population: int
...     area: KmSquare
...     def get_coordinates(self):
...         return self.lat, self.long
>>> Warsaw = City(52.2297, 21.0122, 1_765_000, 517.2)
>>> lat, long = Warsaw.get_coordinates()
>>> lat
52.2297
>>> long
21.0122 
```

上述示例展示了元组解包的最常见用例。然而，有时我们可能需要从基于元组的嵌套数据结构中解包值。考虑以下例子。假设我们有一个如上所示的城市列表，每个城市由一个字典中的列表表示，而不是 `dataclass`：

```py
>>> cities = {
...     "Warsaw": [(52.2297, 21.0122), 1_765_000, 517.2],
...     "Prague": [(50.0755, 14.4378), 1_309_000, 496],
...     "Bratislava": [(48.1486, 17.1077), 424_428_000, 367.6],
... }
```

如你所见，我们将城市的坐标组织成了列表中的元组。我们可以使用嵌套解包来获取这些坐标：

```py
>>> (lat, long), *rest = cities["Warsaw"]
>>> lat
52.2297
>>> long
21.0122
```

或者我们可能还需要面积：

```py
>>> (lat, long), _, area = cities["Warsaw"]
>>> lat, long, area
(52.2297, 21.0122, 517.2)
```

再次，我使用了下划线字符 `_` 来分配我们不需要的值。

请注意，我们对 `*args` 所做的正是解包。通过将 `*args` 放在函数的参数中，你让用户知道他们可以使用任何参数：

```py
>>> def foo(*args):
...     return args
>>> foo(50, 100)
(50, 100)
>>> foo(50, "Zulu Gula", 100)
(50, 'Zulu Gula', 100)
```

在这里，`*args` 将所有位置参数（而非关键字参数）收集到 `args` 元组中。这个 `return` 语句使我们能够查看 `args` 元组中的这些参数。

还有一点：解包不仅限于元组，你也可以将它用于其他可迭代对象：

```py
>>> a, *_, b = [i**2 for i in range(100)]
>>> a, b
(0, 9801)
>>> x = (i for i in range(10))
>>> a, b, *c = x
>>> c
[2, 3, 4, 5, 6, 7, 8, 9]
```

## 元组方法

Python 初学者很快就会了解元组。随着时间的推移，他们会多了解一些，主要是它们的不变性及其后果。但许多开发者不知道`tuple`类提供的所有方法。说实话，在写这篇文章之前，当我认为自己是一个相当高级的开发者时，我也不知道这些方法。不过了解这些方法是好的——这一小节旨在帮助你学习这些方法。

这并不意味着你需要使用所有这些操作。但例如，记住可以在元组上使用就地操作及其结果是好的。这些知识足以让你回忆起，元组只有两种就地操作：就地拼接和就地重复拼接。

为了学习这些方法，我们再看看*《流畅的 Python》*。我们将找到一个比较列表和元组方法的漂亮表格，从中我们可以提取出元组的方法。因此，下面你将找到`tuple`类的完整方法列表，每个方法附有一个或多个简单示例。

*获取长度*：`len(x)`

```py
>>> len(y)
7
```

*拼接*：`x + y`

```py
>>> x = (1, 2, 3)
>>> y = ("a", "b", "c")
>>> z = x + y
>>> z
(1, 2, 3, 'a', 'b', 'c')
```

*重复拼接*：`x * n`

```py
>>> x = (1, 2, 3)
>>> x * 3
(1, 2, 3, 1, 2, 3, 1, 2, 3)
```

*反向重复拼接*：`n * x`

```py
>>> x = (1, 2, 3)
>>> 3 * x
(1, 2, 3, 1, 2, 3, 1, 2, 3)
```

*就地拼接*：`x += y`

```py
>>> x = (1, 2, 3)
>>> y = ("a", "b", "c")
>>> x += y
>>> x
(1, 2, 3, 'a', 'b', 'c')
```

就地拼接的语法可能会暗示我们在处理相同的对象：我们从等于`(1, 2, 3)`的元组`x`开始；在拼接`y`之后，`x`仍然是一个元组，但包含了六个值：`(1, 2, 3, "a", "b", "c")`。由于我们讨论了元组的不变性，我们知道`x`之前和`x`之后是两个不同的对象。

我们可以通过以下简单测试轻松检查这一点。它使用两个对象的`id`：如果它们有相同的`id`，那么它们是同一个对象；但如果`id`不同，那么在就地拼接之前和之后的`x`是两个不同的对象。我们来做一下测试：

```py
>>> x = (1, 2, 3)
>>> first_id = id(x)
>>> y = ("a", "b", "c")
>>> x += y
>>> second_id = id(x)
>>> first_id == second_id
False
```

两个`id`不同，这意味着在就地操作之后的`x`与之前的`x`是不同的对象。

*就地重复拼接*：`x *= n`

```py
>>> x = (1, 2, 3)
>>> x *= 3
>>> x
(1, 2, 3, 1, 2, 3, 1, 2, 3)
```

我上面写的同样适用在这里：尽管我们在这里看到的只有一个名字，`x`，但实际上有两个对象：`x`之前的和`x`之后的。

*包含*：`in`

```py
>>> x = (1, 2, 3)
>>> 1 in x
True
>>> 100 in x
False
```

*计算元素出现的次数*：`x.count(element)`

```py
>>> y = ("a", "b", "c", "a", "a", "b", "C")
>>> y.count("a")
3
>>> y.count("b")
2
```

*获取指定位置的项*：`x[i]`（`x.__getitem__(i)`）

```py
>>> y[0]
'a'
>>> y[4], y[5]
('a', 'b')
```

*查找第一次出现的* `element` *的位置*：`x.index(element)`

```py
>>> y = ("a", "b", "c", "a", "a", "b", "C")
>>> y.index("a")
0
>>> y.index("b")
1
```

*获取迭代器*：`iter(x)`（`x.__iter__()`）

```py
>>> y_iter = iter(y)
>>> y_iter # doctest: +ELLIPSIS
<tuple_iterator object at 0x7...>
>>> next(y_iter)
'a'
>>> next(y_iter)
'b'
>>> for y_i in iter(y):
...     print(y_i, end=" | ")
a | b | c | a | a | b | C |
```

*支持使用* `pickle` *优化序列化*：`x.__getnewargs__()`

这个方法不像上面那些方法那样直接使用。相反，它在 pickle 序列化过程中用于优化元组的序列化，如下面的玩具示例所示：

```py
>>> import pickle
>>> with open("x.pkl", "wb") as f:
...     pickle.dump(x, f)
>>> with open("x.pkl", "rb") as f: 
...     x_unpickled = pickle.load(f)
>>> x_unpickled
(1, 2, 3)
```

在他那本精彩的书*《流畅的 Python》（第 2 版）*中，Luciano Ramalho 列出了 15 个列表有而元组没有的方法——但这个优化序列化的方法是元组独有的，是列表没有的*唯一*方法。

![](img/3d0766b50bbdc25cfa5ecbb9dde984d7.png)

“元组”一词在不同语言中的表达。图片由作者提供。

# 结论

在这篇文章中，我们讨论了 Python 中最常见的集合类型之一——元组的基础知识。希望你喜欢这篇文章——如果喜欢，请注意，我们讨论的不仅仅是基础知识，还可以说是非争议性的。

然而，元组还有更多内容，其中一些内容并不像我们从这篇文章中学到的那样清晰。我们将在这篇文章的后续部分讨论这些内容。你会看到，元组并不像你读完这篇文章后可能想象的那样简单。在我看来，元组比任何其他内置类型都更具争议性。也许元组甚至被过度使用了——但在读完下一篇文章后，我让你自己决定。老实说，我对元组有些不满。实际上，我会对元组有点苛刻……甚至可能有些过头？

我希望我已经足够引起你的兴趣，让你阅读这篇文章的续集。你可以在这里找到它：

## Python 元组，全面的真相与唯一真相：让我们深入探讨

### 了解元组的复杂性

towardsdatascience.com

感谢阅读。如果你喜欢这篇文章，你可能也会喜欢我写的其他文章；你可以在[这里](https://medium.com/@nyggus)查看。如果你想加入 Medium，请使用下面的推荐链接：

[## 通过我的推荐链接加入 Medium — Marcin Kozak](https://medium.com/@nyggus/membership?source=post_page-----12a7ab9dbd0d--------------------------------)

### 阅读 Marcin Kozak 的每一个故事（以及 Medium 上的其他数千位作者的故事）。你的会员费直接支持……

[medium.com](https://medium.com/@nyggus/membership?source=post_page-----12a7ab9dbd0d--------------------------------)

# 脚注

¹ 请注意，在许多代码块中，如上面所示，我使用了`doctest`测试，以确保示例正确运行。你可以在模块的[文档](https://docs.python.org/3/library/doctest.html)和[这篇在*Towards Data Science*上发布的介绍文章](https://medium.com/towards-data-science/python-documentation-testing-with-doctest-the-easy-way-c024556313ca)中了解更多关于`doctest`的信息。

² 请注意，我写的是“在范围内”，而不是“在代码中”。这是因为虽然我们在这里不需要这些值，但我们可能在代码的其他地方，在某个其他范围内需要它们（例如，在另一个函数中）。在特定范围内使用特定解包只会影响这个范围；因此，我们可以在另一个范围内再次解包相同的可迭代对象，这种解包可能会有所不同。

在代码块中，你会发现 `KmSquare` 类型别名。我使用它来提高定义城市时浮点数的可读性。你可以在[这里](https://medium.com/better-programming/pythons-type-hinting-friend-foe-or-just-a-headache-73c7849039c7)阅读更多关于类型提示和类型别名的内容。

# 资源

[## Python 文档测试，使用 doctest：简单的方法](https://towardsdatascience.com/python-documentation-testing-with-doctest-the-easy-way-c024556313ca?source=post_page-----12a7ab9dbd0d--------------------------------)

### doctest 允许进行文档、单元和集成测试，以及测试驱动开发。

[## Python 列表推导指南](https://towardsdatascience.com/a-guide-to-python-comprehensions-4d16af68c97e?source=post_page-----12a7ab9dbd0d--------------------------------)

### 了解列表推导（listcomps）、集合推导（setcomps）、字典推导等的复杂性…

[## 找到 Python 代码中的 bug：小细节产生大问题](https://betterprogramming.pub/find-a-bug-in-python-code-a-little-thing-does-so-much-89d0abd0300c?source=post_page-----12a7ab9dbd0d--------------------------------)

### 即使是最小的字符也可能引入大问题

[## Fluent Python，第 2 版](https://betterprogramming.pub/find-a-bug-in-python-code-a-little-thing-does-so-much-89d0abd0300c?source=post_page-----12a7ab9dbd0d--------------------------------)

### Python 的简洁性使你可以迅速变得高效，但这通常意味着你并没有充分利用它的所有功能…

[## Python 文档测试，使用 doctest：简单的方法](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/?source=post_page-----12a7ab9dbd0d--------------------------------)
