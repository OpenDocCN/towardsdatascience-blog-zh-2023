# Python 元组，真相和唯一的真相：深入探讨

> 原文：[`towardsdatascience.com/python-tuple-the-whole-truth-and-only-truth-lets-dig-deep-24d2bf02971b`](https://towardsdatascience.com/python-tuple-the-whole-truth-and-only-truth-lets-dig-deep-24d2bf02971b)

## PYTHON PROGRAMMING

## 学习元组的复杂性。

[](https://medium.com/@nyggus?source=post_page-----24d2bf02971b--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----24d2bf02971b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----24d2bf02971b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----24d2bf02971b--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----24d2bf02971b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----24d2bf02971b--------------------------------) ·阅读时间 24 分钟 ·2023 年 1 月 27 日

--

![](img/e10e34210758fdd1c9b2276ce2c67e81.png)

元组的不可变性可能令人困惑且令人头痛。照片由 [Aarón Blanco Tejedor](https://unsplash.com/@innernature?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在上一篇文章中，我们讨论了元组的基础知识：

[](/python-tuple-the-whole-truth-and-only-the-truth-hello-tuple-12a7ab9dbd0d?source=post_page-----24d2bf02971b--------------------------------) ## Python Tuple, the Whole Truth, and Only the Truth: Hello, Tuple!

### 学习元组的基础知识及其使用方法

towardsdatascience.com

我向你展示了元组是什么，它提供了哪些方法，以及最重要的是，我们讨论了元组的不可变性。但元组远不止这些，这篇文章将对上一篇文章进行扩展。你将在这里学习元组类型的以下方面：

+   元组的复杂性：不可变性对复制元组的影响以及元组类型提示。

+   从元组继承。

+   元组性能：执行时间和内存。

+   元组相较于列表的优势（？）：清晰度、性能以及元组作为字典键的使用。

+   元组推导（？）

+   命名元组

# 元组的复杂性

元组最重要的复杂性可能就是它的不可变性。但由于这定义了这种类型的本质，即使是初学者也应该了解这种不可变性是如何工作的，以及它在理论和实践中的意义。因此，我们在上述提到的上一篇文章中讨论了这一点。在这里，我们将讨论元组的其他重要复杂性。

## 不可变性对复制元组的影响

这将会很有趣！

一位理论家可能会对我大喊，称只有一种元组的不可变性，那就是我们在上一篇文章中讨论的那个。好吧，这是事实，但……但 Python 本身区分了两种不同的不可变性！而 Python *必须* 做出这种区分。这是因为只有真正不可变的对象才是可哈希的。在下面的代码中，你会看到第一个元组是可哈希的，而第二个元组则不是：

```py
>>> hash((1,2))
-3550055125485641917
>>> hash((1,[2]))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

一个对象是否可哈希会影响到各种事情——这也是为什么 Python 区分可哈希和不可哈希的元组；前者是我称之为真正不可变的元组。我将展示 Python 如何处理这两种元组，包括元组复制的工作原理和将元组用作字典键的情况。

首先，让我们看看在元组复制中它是如何工作的。为此，我们创建一个完全不可变的元组，并使用所有可用的方法进行复制：

```py
>>> import copy
>>> a = (1, 2, 3)
>>> b = a
>>> c = tuple(a)
>>> d = a[:]
>>> e = copy.copy(a)     # a shallow copy
>>> f = copy.deepcopy(a) # a deep copy
```

由于 `a` 是一个完全不可变的元组，原始元组 (`a`) 及其所有副本应该指向同一个对象：

```py
>>> a is b is c is d is e is f
True
```

正如预期的那样——也应该是完全不可变类型的情况——所有这些名称都指向同一个对象；它们的 `id` 是相同的。这就是我所称的真正或完全不可变性。

现在我们用第二种类型的元组做同样的事情；也就是说，一个包含一个或多个可变元素的元组：

```py
>>> import copy
>>> a = ([1], 2, 3)
>>> b = a
>>> c = tuple(a)
>>> d = a[:]
>>> e = copy.copy(a)     # a shallow copy
>>> f = copy.deepcopy(a) # a deep copy
```

从 `b` 到 `e` 的副本是浅复制，因此它们将引用与原始名称相同的对象：

```py
>>> a is b is c is d is e
True
```

这就是我们需要深度复制的原因。深度复制应该覆盖所有对象，包括嵌套在内部的对象。由于 `a` 元组内部有一个可变对象，因此与之前不同的是，这次深度复制 `f` 将*不会*指向相同的对象：

```py
>>> a is f
False
```

元组的第一个元素（索引 `0`）是 `[1]`，所以它是可变的。当我们创建 `a` 的浅复制时，元组 `a` 到 `e` 的第一个元素指向相同的列表：

```py
>>> a[0] is b[0] is c[0] is d[0] is e[0]
True
```

但创建深度复制意味着创建一个新的列表：

```py
>>> a[0] is f[0]
False
```

现在让我们看看这两种不可变性在将元组用作字典键时的工作差异：

```py
>>> d = {}
>>> d[(1, 2)] = 3
>>> d[(1, [2])] = 4
Traceback (most recent call last):
    ...
TypeError: unhashable type: 'list'
```

所以，如果你想将一个元组用作字典键，它必须是可哈希的——也就是说，它必须真正不可变。

所以，如果有人告诉你 Python 元组只有一种不可变性，你会知道这并不完全正确——因为在不可变性方面有两种类型的元组：

+   完全不可变的元组，仅包含不可变元素；这在引用和值两个方面都表现为不可变性；

+   从引用角度看不可变但从值角度看可变的元组，即包含可变元素的元组。

如果不区分这两者，你将无法理解元组复制的工作原理。

## 元组类型提示

类型提示在 Python 中变得越来越重要。有些人说现代 Python 代码中没有类型提示是不可能的。正如我在另一篇文章中所写的那样，我不会在这里重复。如果你感兴趣，请阅读它：

[](https://betterprogramming.pub/pythons-type-hinting-friend-foe-or-just-a-headache-73c7849039c7?source=post_page-----24d2bf02971b--------------------------------) [## Python 的类型提示：朋友、敌人，还是只是个头疼的问题？

### 类型提示在 Python 社区中的受欢迎程度不断上升。这会将我们引向何处？我们能做些什么来利用它……

[betterprogramming.pub](https://betterprogramming.pub/pythons-type-hinting-friend-foe-or-just-a-headache-73c7849039c7?source=post_page-----24d2bf02971b--------------------------------)

在这里，我们简要讨论如何处理元组的类型提示。我将展示现代版本的元组类型提示，即 Python 3.11。由于类型提示在不断变化，请注意，并非所有旧版本的 Python 都能以相同的方式工作。

从 Python 3.9 开始，事情变得更简单，因为可以使用内置的 `tuple` 类型，并用方括号 `[]` 指示字段。以下是你可以做的几个示例。

`tuple[int, ...]`、`tuple[str, ...]` 等等

这意味着对象是 `int` / `str` / 等等元素的元组，长度不限。省略号 `...` 表明元组可以有任意长度；无法固定长度。

`tuple[int | float, ...]` 如上所述，但元组可以包含 `int` 和 `float` 类型的元素。

`tuple[int, int]` 与上述不同，这个元组是两个整数的记录。

`tuple[str, int|float]` 再次是一个两项记录，第一项是字符串，第二项是整数或浮点数。

`tuple[str, str, tuple[int, float]]` 一个包含三项的记录，前两项是字符串，第三项是包含一个整数和一个浮点数的二元素元组。

`tuple[Population, Area, Coordinates]`

这是一个特定的记录，包含三种特定类型的元素。这些类型，`Population`、`Area`、`Coordinates`，是命名元组或先前定义的数据类型，或类型别名。正如我在上述文章中所解释的，使用这些类型别名比使用内置类型如 `int`、`float` 等更具可读性。

这些只是几个示例，但我希望它们能帮助你了解你可以用元组的类型提示做些什么。我只提到了 *命名元组*，因为我将在下面的另一个部分讨论这种类型。不过，请记住，在类型提示的背景下，命名元组也非常有用，因为借助命名元组，你可以获得一个自定义的类型别名，它也是一个数据容器——这是一个强大的组合。

# 从 `tuple` 继承

你可以从 `list` 继承，尽管有时从 `collections.UserList` 继承更好。那么，我们是否可以对元组做同样的事情？我们可以从 `tuple` 类继承吗？

基本上，忘掉创建类似元组的通用类型的想法。`tuple`没有自己的`.__init__()`方法，因此你不能像继承自列表那样调用`super().__init__()`。没有这一点，你几乎没有任何功能，因为`tuple`类继承的是`object.__init__()`。

然而，这并不意味着你不能从`tuple`继承。你可以，但不是为了创建通用类型，而是特定类型。你还记得`City`类吗？我们可以做类似的事情，但要注意，这可能并不有趣。

```py
>>> class City(tuple):
...    def __new__(self, lat, long, population, area):
...        return tuple.__new__(City, (lat, long, population, area))
```

我们有一个类似元组的`City`类：

```py
>>> Warsaw = City(52.2297, 21.0122, 1_765_000, 517.2)
>>> Warsaw
(52.2297, 21.0122, 1765000, 517.2)
>>> Warsaw[0]
52.2297
```

这个类*确切*地接受四个参数，既不多也不少：

```py
>>> Warsaw = City(52.2297, 21.0122, 1_765_000)
Traceback (most recent call last):
    ...
TypeError: __new__() missing 1 required positional argument: 'area'
>>> Warsaw = City(52.2297, 21.0122, 1_765_000, 517.2, 50)
Traceback (most recent call last):
    ...
TypeError: __new__() takes 5 positional arguments but 6 were given
```

请注意，在当前版本中，我们可以使用参数名称，但不必这样做，因为它们是位置参数。

```py
>>> Warsaw_names = City(
...     lat=52.2297,
...     long=21.0122,
...     population=1_765_000,
...     area=517.2
... )
>>> Warsaw == Warsaw_names
True
```

但是我们不能通过名称访问值：

```py
>>> Warsaw.area
Traceback (most recent call last):
    ...
AttributeError: 'City' object has no attribute 'area'
```

我们可以通过两种方式来改变这一点。一种是使用`collections`或`typing`模块中的命名元组；我们稍后会讨论它们。但我们也可以使用我们的`City`类来实现相同的效果，感谢`operator`模块：

```py
>>> import operator
>>> City.lat = property(operator.itemgetter(0))
>>> City.long = property(operator.itemgetter(1))
```

现在我们可以按名称访问`lat`和`long`属性：

```py
>>> Warsaw.lat
52.2297
>>> Warsaw.long
21.0122
```

然而，由于我们只对`lat`和`long`进行了上述操作，我们将无法按名称访问`population`和`area`：

```py
>>> Warsaw.area
Traceback (most recent call last):
    ...
AttributeError: 'City' object has no attribute 'area'
```

我们当然可以改变这一点：

```py
>>> City.population = property(operator.itemgetter(2))
>>> City.area = property(operator.itemgetter(3))
>>> Warsaw.population
1765000
>>> Warsaw.area
517.2
```

不过，我从未做过这样的事情。如果你想要这样的功能，你应该使用命名元组。

# 元组性能

## 执行时间

为了基准测试使用元组的各种操作，以及作为比较的列表，我使用了附录中接近文章末尾的脚本。你还会在那里找到运行代码的结果。我提供代码不仅仅是为了记录，也为了让你可以扩展实验。

总体而言，无论其大小和执行的操作是什么，列表*总是*更快。我常听说创建元组的原因之一是它们较小的内存消耗。我们的这个小实验远未确认这一观点。虽然有时元组确实使用了稍少的内存，但通常它们使用的内存稍多。因此，我对 5 百万和 1000 万整数项的非常长的列表和元组进行了实验。结果是，列表通常消耗的内存更少……

那么，这些小内存消耗的元组在哪里呢？也许这与元组和相应列表所占的磁盘空间有关？让我们检查一下：

```py
>>> from pympler.asizeof import asizeof
>>> for n in (3, 10, 100, 1000, 1_000_000, 5_000_000, 10_000_000):
...     print(
...         f"tuple, n of {n: 9}: {asizeof(tuple(range(n))):10d}"
...         "\n"
...         f" list, n of {n: 9}: {asizeof(list(range(n))):10d}"
...         "\n"
...         f"{'-'*33}"
...         )
tuple, n of         3:        152
 list, n of         3:        168
---------------------------------
tuple, n of        10:        432
 list, n of        10:        448
---------------------------------
tuple, n of       100:       4032
 list, n of       100:       4048
---------------------------------
tuple, n of      1000:      40032
 list, n of      1000:      40048
---------------------------------
tuple, n of   1000000:   40000032
 list, n of   1000000:   40000048
---------------------------------
tuple, n of   5000000:  200000032
 list, n of   5000000:  200000048
---------------------------------
tuple, n of  10000000:  400000032
 list, n of  10000000:  400000048
---------------------------------
```

仅在小元组及其相应列表的情况下，内存使用差异才是明显的——例如，`152`与`168`。但我认为你会同意，`400_000_032`与`400_000_048`实际上并没有小那么多，对吧？

我在过去的实验中观察到的另一件事（代码未展示）。Python 编译器以特殊方式处理元组字面量，因为它将它们保存在静态内存中——所以它们是在编译时创建的。其他方式创建的列表和元组都不能保存在静态内存中——它们总是使用动态内存，这意味着它们是在运行时创建的。这个话题复杂到足以值得单独的文章，因此我们就此停下。

我将这些基准留给你。如果你想扩展它们，请随意。如果你学到了新且意外的东西，请在评论中分享。

我学到的是，几乎不要仅仅因为性能而使用元组。但的确，如果我们需要一个简单的类型来存储非常小的记录，比如由两个或三个元素组成，元组可能是一个有趣的选择。如果字段名称有帮助，而且字段更多，我宁愿使用其他东西，命名元组是一个选择，`dataclasses.dataclass`是另一个选择。

![](img/af48b3b5aa071084f4966cb0336dd90b.png)

一个列表和一个元组。作者提供的图像。

# 元组相对于列表的优势（？）

在*流畅的 Python*中，L. Ramalho 提到元组相对于列表的两个优势：清晰度和性能。老实说，我找不到其他优势，但这两个优势可能已经足够。因此，让我们逐一讨论它们，并决定它们是否确实在某些方面使元组优于列表。

## 清晰度

正如 L. Ramalho 所写，当你使用元组时，你知道它的长度永远不会改变——这增加了代码的清晰度。我们已经讨论过元组长度可能发生的情况。的确，由于不可变性带来的清晰度是很棒的，我们确实知道任何元组的长度*永远*不会改变，但……

正如 L. Ramalho 自己警告的那样，包含可变项的元组可能是难以发现的错误来源。你还记得我之前提到的与原地操作相关的内容吗？一方面，我们可以确定一个元组，比如`x`，它的长度永远不会改变。我同意这是一个在清晰度方面很有价值的信息。然而，当我们对`x`进行原地操作时，这个元组将*不再*是同一个元组，即便它仍然是一个名为`x`的元组——但，请让我重复，是一个不同的名为`x`的元组。

因此，我们应该按如下方式修订上述清晰度优势：

+   *我们可以确定一个特定的* `id` *的元组长度永远不会改变*。

或者：

+   *我们可以确定，如果我们定义一个特定长度的元组，它的长度不会改变，但我们应该记住，如果我们使用任何原地操作，那么这个元组就不是我们之前所指的那个元组*。

听起来有点疯狂？我完全同意：这确实很疯狂。对我来说，这不是清晰；这是清晰的对立面。有人这样想吗？想象一下你在一个函数中定义了一个元组 `x`。然后你执行原地连接操作，例如 `x += y`，这看起来就像 `y` 保持不变但 `x` 发生了变化。我们知道这不是真的——因为这个原始的 `x` 已经不存在，我们有一个全新的 `x`——但这就是它*看起来*的样子，尤其是因为我们仍然有一个元组 `x`，其第一个元素与原始 `x` 元组中的元素完全相同。

当然，我知道从 Python 的角度来看这一切都是有意义的。但当我编码时，我不希望我的思维被这种方式占据。为了使代码清晰，我更倾向于让它在不需要做出这样的假设的情况下保持清晰。这正是为什么对我来说，元组并不意味着清晰；它们意味着比我在列表中看到的清晰度要低。

这还不是元组清晰度的全部。在代码方面，我特别喜欢列表中的一个特性，但不喜欢元组中的这个特性。用于创建列表的方括号 `[]` 使得它们在代码中显得突出，因为没有其他容器使用方括号。看看字典：它们使用大括号 `{}`，集合也可以使用这些大括号。元组使用圆括号 `()`，而圆括号不仅在生成器表达式中使用，而且在代码中的许多不同地方使用，因为 Python 代码使用圆括号的目的非常多。因此，我喜欢列表在代码中显得突出——而不喜欢元组的*不突出*。

## 性能

L. Ramalho 写道，元组使用的内存比对应的列表少，Python 可以对这两者进行相同的优化。我们已经分析了内存性能，因为我们知道这并不总是如此——实际上，元组所用的磁盘内存确实比对应的列表要小，但这种差异可能微不足道。

这种知识，加上列表在执行时间上的更好性能，使我认为性能*不*使元组成为更好的选择。在执行时间方面，列表更好。在内存使用方面，元组确实可以更好——但现在，随着现代计算机的出现，这些差异真的很小。此外，当我需要一个真正节省内存的容器来收集大量数据时，我不会选择列表或元组——而是选择生成器。

## 另一件事：元组作为字典键

除了这两个方面，还有一个值得考虑的第三个方面，我们已经提到过——你不能将列表用作字典中的键，但可以使用元组。或者说，你可以使用真正不可变（即，可哈希）的元组。原因在于前者的可变性和后者的不可变性。

与前两个优势不同，这个优势在特定情况下可能非常显著，即使这种情况比较少见。

# 元组推导（?）

如果你希望从这一节中了解到 Python 中存在元组推导，或者希望学到一些能让你的 Python 爱好者同伴惊叹的惊人技巧——我很抱歉！我并不想制造虚假的希望。今天没有元组推导；没有让人震撼的语法。

你可能还记得，在我关于 Python 推导的文章中，我并没有提到元组推导：

[](/a-guide-to-python-comprehensions-4d16af68c97e?source=post_page-----24d2bf02971b--------------------------------) ## Python 推导指南

### 学习列表推导（listcomps）、集合推导（setcomps）、字典推导的复杂性…

towardsdatascience.com

这是因为没有元组推导。但我不想让你空手而归，我为你准备了一个安慰奖。我会向你展示一些元组推导的替代方案。

首先，记住生成器表达式*不是*元组推导。我认为许多 Python 初学者会混淆这两者。我特别记得在学习列表推导后第一次看到我的生成器表达式。我的第一反应是，“嗯，这就是了。一个元组推导。”我很快意识到，虽然前者确实是列表推导，但后者*不是*元组推导：

```py
>>> listcomp = [i**2 for i in range(7)] # a list comprehension
>>> genexp = (i**2 for i in range(7))   # NOT a tuple comprehension
```

我花了一些时间——如果不是*浪费*的话——才了解到有列表推导、集合推导、字典推导和生成器表达式——但没有元组推导。不要重蹈我的覆辙。不要花几个小时去寻找元组推导。它们在 Python 中不存在。

这就是我的安慰奖——两个元组推导的替代方案。

*替代方案 1*: `tuple()` + `genexp`

```py
>>> tuple(i**2 for i in range(7))
(0, 1, 4, 9, 16, 25, 36)
```

你有没有注意到你不需要先创建一个列表推导然后是元组？确实，在这里我们创建了一个生成器表达式，并用`tuple()`类来转换它。这自然会给我们一个元组。

*替代方案 2*: `genexp` + 生成器解包

```py
>>> *(i**2 for i in range(7)),
(0, 1, 4, 9, 16, 25, 36)
```

一个不错的小技巧，不是吗？它使用了[扩展的可迭代解包](https://peps.python.org/pep-3132/)，它返回一个元组。你可以用它来处理任何可迭代对象，既然生成器是其中之一，它就有效！让我们检查它是否也对列表有效：

```py
>>> x = [i**2 for i in range(7)]
>>> *x,
(0, 1, 4, 9, 16, 25, 36)
```

你可以不赋值给`x`而做同样的事情：

```py
>>> *[i**2 for i in range(7)],
(0, 1, 4, 9, 16, 25, 36)
```

它适用于任何可迭代对象——但别忘了行末的逗号；没有它，这个技巧将无法奏效：

```py
>>> *[i**2 for i in range(7)]
  File "<stdin>", line 1
SyntaxError: can't use starred expression here
```

让我们检查集合：

```py
>>> x = {i**2 for i in range(7)}
>>> *x,
(0, 1, 4, 9, 16, 25, 36)
```

它有效！并且要注意，通常，解包提供一个元组。这就是为什么扩展的可迭代解包看起来有点像元组推导。虽然它确实像一个不错的小技巧，但其实不是：这是 Python 提供的工具之一，尽管它确实是一个边缘情况。

但我*不会*使用*替代方案 2*。我会选择*替代方案 1*，它使用`tuple()`。我们大多数人喜欢像第二个替代方案这样的技巧，但它们很少清晰——而且*替代方案 2*，与*替代方案 1*相比，远不如前者清晰。不过，任何 Python 爱好者都会看到*替代方案 1*中的内容，即使他们没有看到其中隐藏的生成器表达式。

# 命名元组

元组是未命名的——但这并不意味着 Python 中没有命名元组。恰恰相反，确实存在——而且，毫无意外，它们被称为……命名元组。

你有两种方法来使用命名元组：`collections.namedtuple`和`typing.NamedTuple`。命名元组顾名思义：它们的元素（称为字段）具有名称。你可以在附录中的基准测试脚本中看到前者的实际应用。

就个人而言，我认为它们在许多不同情况下都非常有帮助。它们不会提高性能；甚至可能会降低性能。但在清晰性方面，它们可以更清楚，无论是对开发者还是对代码的用户。

因此，尽管我经常使用常规元组，有时我会选择命名元组——这正是因为它的清晰性。

命名元组提供了丰富的可能性，值得专门为它们写一篇文章。因此，我在这里仅仅讲述这些——但我计划写一篇专门讨论这种强大类型的文章。

![](img/3d0766b50bbdc25cfa5ecbb9dde984d7.png)

“元组”在各种语言中的表示。图像由作者提供。

# 结论

本文以及上一篇文章旨在提供关于元组、它们的用例、优缺点及其复杂性的深入信息。尽管元组的使用非常普遍，但在开发者中，尤其是那些经验较少的 Python 开发者中，它们并不那么知名。这就是为什么我想将关于这个有趣类型的丰富信息集中在一个地方——希望你从阅读中学到了一些东西，甚至像我从写作中学到的一样多。

说实话，在开始写关于元组的内容时，我以为会发现更多的优势。我从开始使用 Python 的第一天起就一直在使用元组。尽管我使用列表的频率要高得多，但我还是喜欢元组，尽管对它们了解不多——所以我在这篇文章中包含的一些信息对我来说是新的。

然而，在写完这篇文章后，我对元组的喜爱已经不那么强烈了。我仍然认为它们是处理小记录的有价值类型，但它们的扩展——命名元组——或数据类似乎是更好的方法。而且，元组似乎也不是特别有效。它们比列表要慢，而且只节省了少量内存。那么，我为什么还要使用它们呢？

也许是因为它们的不可变性？也许。如果你喜欢基于不可变性概念的函数式编程，你肯定会更喜欢元组而不是列表。我曾多次使用这个论点来说服自己在这种或那种情况下应该更喜欢元组而不是列表。

但元组所提供的不可变性，如我们讨论的那样，并不是那么*明确*。假设`x`是一个不可变类型的项的元组。我们知道这个元组确实是不可变的，对吗？如果是这样，我不喜欢以下代码，这在 Python 中是完全正确的：

```py
>>> x = (1, 2, 'Zulu Minster', )
>>> y = (4, 4, )
>>> x += y
>>> x
(1, 2, 'Zulu Minster', 4, 4)
>>> x *= 2
>>> x
(1, 2, 'Zulu Minster', 4, 4, 1, 2, 'Zulu Minster', 4, 4)
```

我知道这在 Python 中是正确的，我知道这甚至是 Pythonic 的代码——但我不喜欢它。我不喜欢我可以用 Python 元组做这样的事情。它根本没有元组不可变性的感觉。依我看，如果你有一个不可变类型，你应该能够复制它，你应该能够连接两个实例等等——但你*不应该*能够通过就地操作将一个新元组赋给旧名称。你想让这个名称保持不变？你的选择。所以，我对以下情况没问题：

```py
>>> x = x + y
```

因为这意味着将`x + y`赋值给`x`，这基本上意味着覆盖这个名称。如果你选择覆盖`x`的先前值，这是你的选择。但就我而言，就地操作至少没有不可变性的感觉。我更愿意在 Python 中*不能*做到这一点。

如果没有不可变性，那么也许其他的因素应该说服我更常使用元组？但是什么呢？性能？元组的性能较差，因此这并不能说服我。在执行时间方面，毫无争议；它们确实比相应的列表慢。你可以说在内存方面。确实，它们占用的磁盘空间更少，但差异微妙，对于长容器来说——完全可以忽略。RAM 内存使用？这个论点也没有太成功，因为通常列表的效率和元组一样——有时甚至更高。如果我们有一个巨大的集合，生成器在内存方面会表现更好。

尽管如此，元组在 Python 中确实有其存在的意义。它们非常频繁地被用来从函数或方法中返回两个或三个项——所以，作为小型未命名记录。它们被用作可迭代解包的输出。它们构成了命名元组的基础——`collections.namedtuple`和`typing.NamedTuple`——这些是元组的强大兄弟，可以用作具有命名字段的记录。

总的来说，我不再像写这篇文章之前那样喜欢元组了。我曾把它们视为一个重要的 Python 类型；现在在我眼中它们不再那么重要——但我接受它们在 Python 中的各种使用场景，甚至喜欢其中一些。

我对元组是否不公平？也许。如果你这么认为，请在评论中告诉我。我总是很享受与读者的有益讨论。

感谢阅读。如果你喜欢这篇文章，你可能也会喜欢我写的其他文章；你可以在[这里](https://medium.com/@nyggus)查看。如果你想加入 Medium，请使用下面的推荐链接：

[](https://medium.com/@nyggus/membership?source=post_page-----24d2bf02971b--------------------------------) [## 使用我的推荐链接加入 Medium — Marcin Kozak

### 阅读 Marcin Kozak 的每一个故事（以及 Medium 上的其他成千上万的作者）。你的会员费直接支持…

[medium.com](https://medium.com/@nyggus/membership?source=post_page-----24d2bf02971b--------------------------------)

# 资源

[](/a-guide-to-python-comprehensions-4d16af68c97e?source=post_page-----24d2bf02971b--------------------------------) ## Python 理解的指南

### 了解列表推导（listcomps）、集合推导（setcomps）、字典推导的细节…

towardsdatascience.com  [## PEP 3132 — 扩展的可迭代解包

### 这个 PEP 提议对可迭代解包语法进行更改，允许指定一个“全能”名称来接收…

[peps.python.org](https://peps.python.org/pep-3132/?source=post_page-----24d2bf02971b--------------------------------) [](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/?source=post_page-----24d2bf02971b--------------------------------) [## Fluent Python, 第 2 版

### Python 的简洁性让你能够迅速提高生产力，但这通常意味着你没有充分利用它所具备的所有功能…

[www.oreilly.com](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/?source=post_page-----24d2bf02971b--------------------------------)

# 附录

在这个附录中，你将找到我用来基准测试元组与列表的脚本。我使用了`perftester`包，你可以在这篇文章中阅读相关信息：

[](/benchmarking-python-functions-the-easy-way-perftester-77f75596bc81?source=post_page-----24d2bf02971b--------------------------------) ## 轻松进行 Python 函数基准测试：perftester

### 你可以使用 perftester 轻松对 Python 函数进行基准测试

towardsdatascience.com

这是代码：

```py
import perftester

from collections import namedtuple
from typing import Callable, Optional
Length = int

TimeBenchmarks = namedtuple("TimeBenchmarks", "tuple list better")
MemoryBenchmarks = namedtuple("MemoryBenchmarks", "tuple list better")
Benchmarks = namedtuple("Benchmarks", "time memory")

def benchmark(func_tuple, func_list: Callable,
              number: Optional[int] = None) -> Benchmarks:
    # time
    t_tuple = perftester.time_benchmark(func_tuple, Number=number)
    t_list = perftester.time_benchmark(func_list, Number=number)
    better = "tuple" if t_tuple["min"] < t_list["min"] else "list"
    time = TimeBenchmarks(t_tuple["min"], t_list["min"], better)

    # memory
    m_tuple = perftester.memory_usage_benchmark(func_tuple)
    m_list = perftester.memory_usage_benchmark(func_list)
    better = "tuple" if m_tuple["max"] < m_list["max"] else "list"
    memory = MemoryBenchmarks(m_tuple["max"], m_list["max"], better)

    return Benchmarks(time, memory)

def comprehension(n: Length) -> Benchmarks:
    """List comprehension vs tuple comprehension.

    Here, we're benchmarking two operations:
      * creating a container
      * looping over it, using a for loop; nothing is done in the loop.
    """
    def with_tuple(n: Length):
        x = tuple(i**2 for i in range(n))
        for _ in x:
            pass

    def with_list(n: Length):
        x = [i**2 for i in range(n)]
        for _ in x:
            pass
    number = int(10_000_000 / n) + 10
    return benchmark(lambda: with_tuple(n), lambda: with_list(n), number)

def empty_container() -> Benchmarks:
    """List vs tuple benchmark: creating an empty container."""
    return benchmark(lambda: tuple(), lambda: [], number=100_000)

def short_literal() -> Benchmarks:
    """List vs tuple benchmark: tuple literal."""
    return benchmark(lambda: (1, 2, 3), lambda: [1, 2, 3], number=100_000)

def long_literal() -> Benchmarks:
    """List vs tuple benchmark: tuple literal."""
    return benchmark(
        lambda: (1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,),
        lambda: [1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,
1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3,],
        number=100_000)

def func_with_range(n: Length) -> Benchmarks:
    """List vs tuple benchmark: func(range(n))."""
    def with_tuple(n: Length):
        return tuple(range(n)) 

    def with_list(n: Length):
        return list(range(n))
    number = int(10_000_000 / n) + 10
    return benchmark(lambda: with_tuple(n), lambda: with_list(n), number)

def concatenation(n: Length) -> Benchmarks:
    """List vs tuple benchmark: func(range(n))."""
    def with_tuple(x: tuple):
        x += x
        return x

    def with_list(y: list):
        y += y
        return y
    number = int(10_000_000 / n) + 10
    return benchmark(lambda: with_tuple(tuple(range(n))),
                     lambda: with_list(list(range(n))),
                     number)

def repeated_concatenation(n: Length) -> Benchmarks:
    """List vs tuple benchmark: func(range(n))."""
    def with_tuple(x: tuple):
        x *= 5
        return x

    def with_list(y: list):
        y *= 5
        return y
    number = int(10_000_000 / n) + 10
    return benchmark(lambda: with_tuple(tuple(range(n))),
                     lambda: with_list(list(range(n))), number)

if __name__ == "__main__":
    n_set = (3, 10, 20, 50, 100, 10_000, 1_000_000)
    functions = (
        comprehension,
        empty_container,
        short_literal,
        long_literal,
        func_with_range,
        concatenation,
        repeated_concatenation,
        )
    functions_with_n = (
        comprehension,
        func_with_range,
        concatenation,
        repeated_concatenation,
    )

    results = {}
    for func in functions:
        name = func.__name__
        print(name)
        if func in functions_with_n:
            results[name] = {}
            for n in n_set:
                results[name][n] = func(n)
        else:
            results[name] = func()
    perftester.pp(results)
```

以下是结果：

```py
{'comprehension': {3: Benchmarks(time=TimeBenchmarks(tuple=9.549e-07, list=8.086e-07, better='list'), memory=MemoryBenchmarks(tuple=15.62, list=15.63, better='tuple')),
                   10: Benchmarks(time=TimeBenchmarks(tuple=2.09e-06, list=1.94e-06, better='list'), memory=MemoryBenchmarks(tuple=15.64, list=15.64, better='list')),
                   20: Benchmarks(time=TimeBenchmarks(tuple=4.428e-06, list=4.085e-06, better='list'), memory=MemoryBenchmarks(tuple=15.64, list=15.65, better='tuple')),
                   50: Benchmarks(time=TimeBenchmarks(tuple=1.056e-05, list=9.694e-06, better='list'), memory=MemoryBenchmarks(tuple=15.69, list=15.69, better='list')),
                   100: Benchmarks(time=TimeBenchmarks(tuple=2.032e-05, list=1.968e-05, better='list'), memory=MemoryBenchmarks(tuple=15.7, list=15.7, better='list')),
                   10000: Benchmarks(time=TimeBenchmarks(tuple=0.002413, list=0.002266, better='list'), memory=MemoryBenchmarks(tuple=15.96, list=16.04, better='tuple')),
                   1000000: Benchmarks(time=TimeBenchmarks(tuple=0.2522, list=0.2011, better='list'), memory=MemoryBenchmarks(tuple=54.89, list=54.78, better='list'))},
 'concatenation': {3: Benchmarks(time=TimeBenchmarks(tuple=3.38e-07, list=3.527e-07, better='tuple'), memory=MemoryBenchmarks(tuple=31.45, list=31.45, better='list')),
                   10: Benchmarks(time=TimeBenchmarks(tuple=4.89e-07, list=4.113e-07, better='list'), memory=MemoryBenchmarks(tuple=31.45, list=31.45, better='list')),
                   20: Benchmarks(time=TimeBenchmarks(tuple=5.04e-07, list=4.368e-07, better='list'), memory=MemoryBenchmarks(tuple=31.45, list=31.45, better='list')),
                   50: Benchmarks(time=TimeBenchmarks(tuple=7.542e-07, list=6.22e-07, better='list'), memory=MemoryBenchmarks(tuple=31.45, list=31.45, better='list')),
                   100: Benchmarks(time=TimeBenchmarks(tuple=1.133e-06, list=9.005e-07, better='list'), memory=MemoryBenchmarks(tuple=31.45, list=31.45, better='list')),
                   10000: Benchmarks(time=TimeBenchmarks(tuple=0.0001473, list=0.000126, better='list'), memory=MemoryBenchmarks(tuple=31.7, list=31.7, better='list')),
                   1000000: Benchmarks(time=TimeBenchmarks(tuple=0.04862, list=0.04247, better='list'), memory=MemoryBenchmarks(tuple=123.5, list=125.4, better='tuple'))},
 'empty_container': Benchmarks(time=TimeBenchmarks(tuple=1.285e-07, list=1.107e-07, better='list'), memory=MemoryBenchmarks(tuple=23.92, list=23.92, better='list')),
 'func_with_range': {3: Benchmarks(time=TimeBenchmarks(tuple=3.002e-07, list=3.128e-07, better='tuple'), memory=MemoryBenchmarks(tuple=23.92, list=23.92, better='list')),
                     10: Benchmarks(time=TimeBenchmarks(tuple=4.112e-07, list=3.861e-07, better='list'), memory=MemoryBenchmarks(tuple=23.92, list=23.92, better='list')),
                     20: Benchmarks(time=TimeBenchmarks(tuple=4.228e-07, list=4.104e-07, better='list'), memory=MemoryBenchmarks(tuple=23.93, list=23.93, better='list')),
                     50: Benchmarks(time=TimeBenchmarks(tuple=5.761e-07, list=5.068e-07, better='list'), memory=MemoryBenchmarks(tuple=23.93, list=23.94, better='tuple')),
                     100: Benchmarks(time=TimeBenchmarks(tuple=7.794e-07, list=6.825e-07, better='list'), memory=MemoryBenchmarks(tuple=23.94, list=23.94, better='list')),
                     10000: Benchmarks(time=TimeBenchmarks(tuple=0.0001536, list=0.000159, better='tuple'), memory=MemoryBenchmarks(tuple=24.67, list=24.67, better='list')),
                     1000000: Benchmarks(time=TimeBenchmarks(tuple=0.03574, list=0.03539, better='list'), memory=MemoryBenchmarks(tuple=91.7, list=88.45, better='list'))},
 'long_literal': Benchmarks(time=TimeBenchmarks(tuple=1.081e-07, list=8.712e-07, better='tuple'), memory=MemoryBenchmarks(tuple=23.92, list=23.92, better='list')),
 'repeated_concatenation': {3: Benchmarks(time=TimeBenchmarks(tuple=3.734e-07, list=3.836e-07, better='tuple'), memory=MemoryBenchmarks(tuple=31.83, list=31.83, better='list')),
                            10: Benchmarks(time=TimeBenchmarks(tuple=4.594e-07, list=4.388e-07, better='list'), memory=MemoryBenchmarks(tuple=31.83, list=31.83, better='list')),
                            20: Benchmarks(time=TimeBenchmarks(tuple=5.975e-07, list=5.578e-07, better='list'), memory=MemoryBenchmarks(tuple=31.83, list=31.83, better='list')),
                            50: Benchmarks(time=TimeBenchmarks(tuple=9.951e-07, list=8.459e-07, better='list'), memory=MemoryBenchmarks(tuple=31.83, list=31.83, better='list')),
                            100: Benchmarks(time=TimeBenchmarks(tuple=1.654e-06, list=1.297e-06, better='list'), memory=MemoryBenchmarks(tuple=31.83, list=31.83, better='list')),
                            10000: Benchmarks(time=TimeBenchmarks(tuple=0.0002266, list=0.0001945, better='list'), memory=MemoryBenchmarks(tuple=31.83, list=31.83, better='list')),
                            1000000: Benchmarks(time=TimeBenchmarks(tuple=0.09504, list=0.08721, better='list'), memory=MemoryBenchmarks(tuple=169.4, list=169.4, better='tuple'))},
 'short_literal': Benchmarks(time=TimeBenchmarks(tuple=1.048e-07, list=1.403e-07, better='tuple'), memory=MemoryBenchmarks(tuple=23.92, list=23.92, better='list'))}
```

我决定对更大的`n`进行内存使用基准测试，即 500 万和 1000 万。我不会在这里展示代码，如果你有时间，可以基于上面的脚本写一个代码，这将是一个不错的练习。

如果你只想查看代码，你可以在[这里](https://gist.github.com/nyggus/3dc967d5e88593b624e2acb3e675a62d)找到。请注意，我可以改进代码，例如将两个实验的代码合并。我决定不这样做，以保持两个脚本的简单性。

这是结果：

```py
{'comprehension': {5000000: MemoryBenchmarks(tuple=208.8, list=208.8, better='list'),
                   10000000: MemoryBenchmarks(tuple=402.2, list=402.2, better='tuple')},
 'concatenation': {5000000: MemoryBenchmarks(tuple=285.4, list=247.2, better='list'),
                   10000000: MemoryBenchmarks(tuple=554.8, list=478.5, better='list')},
 'func_with_range': {5000000: MemoryBenchmarks(tuple=400.4, list=396.4, better='list'),
                     10000000: MemoryBenchmarks(tuple=402.2, list=402.2, better='list')},
 'repeated_concatenation': {5000000: MemoryBenchmarks(tuple=399.8, list=361.7, better='list'),
                            10000000: MemoryBenchmarks(tuple=783.7, list=707.4, better='list')}}
```

如你所见，对于我们研究的操作，元组要么占用相同的内存，要么占用更多的内存——有时甚至显著更多（例如，比较`554.8`与`478.5`或`783.7`与`707.4`）。
