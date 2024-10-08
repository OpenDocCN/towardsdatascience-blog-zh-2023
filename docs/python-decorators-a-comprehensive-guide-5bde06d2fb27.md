# Python 装饰器：全面指南

> 原文：[`towardsdatascience.com/python-decorators-a-comprehensive-guide-5bde06d2fb27`](https://towardsdatascience.com/python-decorators-a-comprehensive-guide-5bde06d2fb27)

## PYTHON 编程

## 文章介绍了 Python 的强大语法糖：装饰器。

[](https://medium.com/@nyggus?source=post_page-----5bde06d2fb27--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----5bde06d2fb27--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5bde06d2fb27--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5bde06d2fb27--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----5bde06d2fb27--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5bde06d2fb27--------------------------------) ·阅读时间 11 分钟·2023 年 10 月 19 日

--

![](img/ca223f2a3dbe822cd12c47eb407ef22a.png)

做一个优秀的装饰器设计师——以及 Python 代码设计师。照片由 [Spacejoy](https://unsplash.com/@spacejoy?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 装饰器是那些看似简单但如果不了解就非常困难的概念之一。许多 Python 初学者将它们视为一种神奇的工具，必须学习并在自己的代码中使用，以便实现真正的魔法。但使用内置的装饰器或来自第三方库的装饰器是不够的；这就像用儿童商店买来的魔法盒子做魔法一样。真正的魔法来自于编写自己的装饰器。

我记得当时我迫切想学习如何在真实项目中编写和使用自己的装饰器，而不仅仅是为了好玩。当那个时刻终于到来时，我感受到的愉悦是巨大的。这段经历让我渴望寻找更多机会来实现自己的装饰器。

我希望在阅读本文后，你对 Python 装饰器不会再有任何困惑。因此，本文旨在向那些尚未理解 Python 装饰器概念的人介绍这个话题。我希望以一种易于理解的方式揭示其背后的魔力。

装饰器的内容远不止这些。我们将讨论基础知识，但好消息是，这些基础知识应该足以让你实现即使是复杂而实用的装饰器。在未来的文章中，我们将深入探讨装饰器的复杂细节及其各种应用场景。

各个水平的 Python 程序员都能从这篇文章中受益。*初学者*将学习装饰器的基础知识，而*中级程序员*将获得更深入的理解；*高级程序员*可以利用这篇文章来刷新记忆。此外，有时从不同的角度看待某个特定概念是好的，不仅仅是我们多年来使用的角度——在这里，我提供了对装饰器及其有用性的观点，希望能对各个水平的读者有所帮助。

# 装饰器简介

Python 装饰器是一个强大而多用途的工具，但应谨慎使用以避免过度使用和滥用。装饰器是一个函数，允许你修改另一个函数的行为。装饰器也可以作为类来编写，但这种情况较少见，本文将不予讨论。

当你有一个装饰器，比如`my_decorator()`时，你可以用它来装饰另一个函数，比如`foo()`，如下所示：

```py
@my_decorator
def foo(x, y):
    # do something; in result,
    # you obtain changed_x and changed_y
    return changed_x, changed_y
```

在装饰了`foo()`函数后，我们不能再知道它的行为，而不了解`my_decorator`装饰器的作用。装饰器可能会添加新的行为，比如日志记录，或者完全改变函数的行为和返回值。例如，装饰过的`foo()`函数可能返回一个字典而不是一个元组，或者可能返回`None`。要了解装饰后的`foo()`函数的行为，我们必须检查`my_decorator`装饰器的定义。

我不知道“装饰器”一词在 Python 上下文中的词源。在 Python 中，这个词源自装饰器模式，但这并没有解释其原始词源。如果你知道，请在评论中与我们分享。

我个人认为 Python 装饰器是一种美妙的语法糖。它们被称为装饰器也不奇怪，因为它们装饰了被装饰的函数。我欣赏装饰器的外观和功能。

不过，我了解装饰器可能带来的困难。如上所述，我们不能知道一个装饰过的函数的行为，而不了解其装饰器的定义。此外，多个装饰器可以作用于一个函数，这使得事情变得更加复杂。

让我将装饰器背后的思想整理成三步：

1.  *需求*。你有一个函数，但你需要改变它的行为。这可能由于各种原因。例如，你可能需要为应用程序中的所有函数添加日志记录，或改变外部模块中函数的行为。

1.  *定义*。你编写一个装饰器函数，负责这种更新的行为。它可以接受一个或多个参数，除了原始函数之外。装饰器函数通常会调用原始函数，但这并不是必须的。

1.  *使用*。你用新函数覆盖原函数。这可以通过装饰或赋值的方式完成，但装饰更为常见。使用原始名称调用装饰函数意味着调用新函数，因为原函数不再存在，除非它被复制。

这三步中的每一步都同样重要，所以我们逐步讨论这三步。

## 第 1 步：需求

好的，所以你*需要*改变函数的行为。

为什么使用装饰器来改变函数的行为而不是简单地重写它？有几个原因：

+   你可能无法重写函数。例如，它可能是来自外部模块的函数。

+   你可能不想重写函数。例如，它可能是一个大型或复杂的函数，或者是一个在许多不同地方使用的函数。

+   重写函数可能很麻烦。例如，它可能被许多不同的函数调用。

+   你可能只需要在开发中改变函数的行为，但希望在生产中使用原函数。

+   你可能需要改变许多函数的行为。在这种情况下，编写装饰器比逐个重写每个函数要高效得多。

最常见的场景是你需要改变许多函数的行为。在这种情况下，你可以编写一个装饰器，并将其应用到所有需要改变的函数上。这可以节省大量时间和代码。

以下是一些装饰器可以使用的例子：

+   向项目中所有现有函数添加日志记录。

+   测量并记录应用中每个函数的执行时间。

+   向应用中调用的函数添加身份验证和授权。

+   缓存函数返回的输出。

+   将函数从写入本地文件的数据更改为写入远程数据库的数据。

+   在测试中使某个特定函数静默。例如，你可以使用装饰器防止函数在测试期间写入远程数据库。

装饰器在 Python 中也被广泛用于模拟。模拟允许你创建假对象来模拟真实对象的行为。这对于测试依赖于外部资源的代码（如数据库和网络服务）非常有用。

## 第 2 步：定义

你是否注意到，在用`@`语法装饰一个函数后，装饰过的函数是用原函数的名称来调用的？这就是使函数成为装饰器的原因。如果原函数，比如`foo()`，仍然可以作为`foo()`使用，并且有一个新的函数，比如`foo_changed()`，其行为是`foo()`的改变后的行为，*这里并没有涉及装饰*。因此，装饰一个函数涉及到覆盖原函数。你可以用一个新名称保留原函数的副本，但原函数本身已被装饰函数替代。

> 装饰涉及到覆盖原函数。

是时候离开抽象的世界，转向实际操作了。让我们创建一个简单的装饰器，称为 `scream()`，使函数尖叫：

```py
from typing import Callable

(1) def scream(func: Callable) -> Callable:
(2)     def inner(*args, **kwargs):
(3)         print("SCREAM!!!")
(4)         return func(*args, **kwargs)
(5)     return inner
```

这个装饰器接受一个可调用对象（`func`）作为输入，并返回一个可调用对象（`inner`）。`inner` 函数在调用原始函数（`func`）之前，会向控制台打印“SCREAM!!!”。为了简化代码，我放弃了文档字符串，但在实际工作中，你应该为装饰器添加文档字符串。

> 在实际工作中，你应该为装饰器添加文档字符串。

这里是 `scream()` 装饰器逐行的解析：

1.  `def scream(func: Callable) -> Callable:` → 这是函数签名。`scream()` 装饰器可以用于装饰任何可调用对象（`func`），并且它也返回一个可调用对象。¹

1.  `def inner(*args, **kwargs):` → 这是内层函数。它打印“SCREAM!!!”；你可以使用任何你想要的名字，但我通常使用 `inner`，像这里一样。装饰函数时，你可以使用被装饰函数接受的任何参数。例如，`scream()` 装饰器可以用于任何函数，接受任何数量和类型的参数（因此使用了 `*args, **kwargs`）。

1.  `print("SCREAM!!!")` → 这是添加到被装饰函数原始行为中的新行为。不管函数做什么，它将*首先*尖叫（通过打印`"SCREAM!!!"`），然后执行它最初应该做的事情。注意，在这个装饰器中，新行为是*之前*添加的，但它也可以*之后*添加，*之前和之后*都添加，甚至*代替*原始行为。

1.  `return func(*args, **kwargs)` → 这是函数的原始行为。

1.  `return inner` → 任何装饰器的标准行：返回 `inner` 函数。这意味着当 `scream()` 装饰器被使用时，它会用内层函数替换原始函数。

在附录 1 中，你会找到 `scream()` 装饰器的两个其他版本：

+   尖叫两次：在原始行为之前和之后

+   嚎叫代替了原始函数应该做的事情。

## 第三步：使用

要装饰一个函数，你可以用两种方式使用装饰器。本小节描述了这两种方式。

*方法 1：将装饰器用作* `*@decorator*`

首先，我们需要一个要装饰的函数。让我们使用两个函数来说明你可以对任意多的函数使用相同的装饰器，并且这些函数可以非常不同。

我们想要装饰的第一个函数是 `foo()`：

```py
def foo():
    return "foo tells you that life is great"
```

当我们调用这个函数时，我们将看到以下输出：

```py
>>> foo()
'foo tells you that life is great'
```

这是几乎最简单的 Python 函数：它不接受任何参数并返回一个字符串。这是我们想要装饰的第二个函数，`bar()`：

```py
from typing import List

def bar(
    x: int,
    string: str,
    func: Callable = lambda a, b: a * b,
    **kwargs
) -> List[str]:
    """Applies a callable to each character in the given string.

    It does so, passing in the given integer and any additional
    keyword arguments. Returns a list of the results.
    """
    return [func(x, s_i, **kwargs) for s_i in string]
```

这个函数比`foo()`复杂。它有三个参数：一个整数`x`、一个字符串`string`和一个可调用的`func`。它还接受任何额外的关键字参数。该函数将`func()`应用于`string`中的每个字符和`x`，如果有额外的关键字参数，也会传递进去。最后，它返回一个结果列表。

一个简单的调用可能是这样的：

```py
>>> bar(3, "abc")
['aaa', 'bbb', 'ccc']
```

让我们使用不同的可调用对象作为`func`：

```py
>>> def concatenate(i: int, s: str, sep: Optional[str] = "-") -> str:
...     return f"{str(i)}{sep}{s}"
>>> bar(5, "abc", func=concatenate)
['3-a', '3-b', '3-c']
>>> bar(3, "abc", func=concatenate, sep=":")
['3:a', '3:b', '3:c']
```

对于我们的目的来说，`foo()`和`bar()`的功能并不重要。重要的是`foo()`是一个非常简单的函数，而`bar()`虽然简洁但更复杂。

我们可以使用装饰器语法装饰这两个函数，这在以`@`字符开头的两行中显示。为了完整性和清晰性，我将展示完整代码，因此我会重复函数的代码：

```py
from typing import Callable, List

@scream
def foo():
    return "foo tells you that life is great"

@scream
def bar(
    x: int,
    string: str,
    func: Callable = lambda a, b: a * b,
    **kwargs
) -> List[str]:
    return [func(x, s_i, **kwargs) for s_i in string]
```

让我们运行这两个函数：

```py
>>> foo()
SCREAM!!!
'foo tells you that life is great'
>>> bar(5, "abc", func=concatenate)
SCREAM!!!
['3-a', '3-b', '3-c']
>>> bar(3, "abc", func=concatenate, sep=":")
SCREAM!!!
['3:a', '3:b', '3:c']
```

*方法 2：将装饰器用作* `*function()*`

使用装饰器函数最常见的方式是作为装饰器。然而，还有一种不太常见的方式：

```py
def foo():
    return "foo tells you that life is great"

foo = scream(foo)
```

这样，你只需将要装饰的函数作为参数调用装饰器函数。

无论你使用哪种方法来应用`scream()`装饰器，当你运行`foo()`时，装饰后的函数将会尖叫，然后做它最初要做的事情。

# 结论

这篇文章解释了 Python 装饰器的基础知识。我尽力做到全面，但仍有许多装饰器的细节我们没有讨论。我们将在未来的文章中深入探讨这些问题。

学习 Python 装饰器的重要性有几个原因，不仅仅是如何使用它们，还包括如何编写新的装饰器。首先，装饰器是一个强大的工具，可以帮助你编写简洁且易读的代码。如果你知道如何编写自定义装饰器，你会发现它们往往能为你节省大量时间和精力。

其次，装饰器可以用来快速更新遗留代码。例如，如果你需要更改一个或多个函数的行为，但修订这些函数本身不可行，你可以使用装饰器。虽然总是可以重写这些函数，但如果所有函数的行为都是相同的，一个装饰器可能就足够了。

第三，装饰器是 Python 中最重要的语法糖之一。如果你不理解它们，你可能会被认为是 Python 初学者。我无法想象一个不知道如何使用装饰器的 Python 开发者，更不用说理解它们了。

最后，装饰器在 Python 代码库中非常常见。如果你不熟悉装饰器，你将无法理解许多现有的代码。

因此，所有中级和高级 Python 开发者应该了解装饰器的概念，理解如何使用它们，并能够编写它们。

尽管装饰器乍看起来可能很复杂，但我相信如果你读到这篇文章的这个部分，你会同意一旦理解了基本原理，它们其实并不是那么复杂。事实上，它们可以是一个非常简单且实用的编码工具。

# 脚注

¹ 为了简便起见，我将使用“装饰器函数”这一术语，而不是“装饰器可调用对象”。但请注意，这只是对以函数和类两种方式定义的装饰器的简写。我只是想避免过度使用“可调用对象”这个词，即使它在 Python 文本中经常出现。

# 附录

## 附录 1：`scream()`装饰器的两个其他版本

*版本 2：在运行函数之前和之后尖叫*

```py
from typing import Callable

def scream(func: Callable) -> Callable:
    def inner(*args, **kwargs):
        print("SCREAM!!!")
        output = func(*args, **kwargs)
        print("SCREAM AGAIN!!!")
        return output
    return inner
```

你将看到用上述装饰器装饰后的`foo()`的输出：

```py
>>> foo()
SCREAM!!!
'foo tells you that life is great'
SCREAM AGAIN!!!
```

*版本 3：尖叫代替运行函数*

```py
from typing import Callable

def scream(func: Callable) -> Callable:
    def inner(*args, **kwargs):
        print("SCREAM, JUST SCREAM!!!")
    return inner
```

以及：

```py
>>> foo()
SCREAM, JUST SCREAM!!!
```

这个版本的`scream()`装饰器完全覆盖了被装饰函数的原始行为。被装饰的函数现在只会尖叫，原始行为完全被移除。这种结构在许多不同情况下都非常有用。例如，你可以用它来完全静音一个函数：

```py
from typing import Callable

def silence(func: Callable) -> Callable:
    def inner(*args, **kwargs):
        pass
    return inner
```

你可以在`[easycheck](https://pypi.org/project/easycheck/)` Python 包的代码中看到这种静音器的例子：

[](https://github.com/nyggus/easycheck/blob/master/easycheck/easycheck.py?source=post_page-----5bde06d2fb27--------------------------------) [## easycheck/easycheck/easycheck.py 在主分支 · nyggus/easycheck

### 一个模块提供了 Python 函数用于简单且可读的断言式检查，可在代码内部以及其他地方使用…

github.com](https://github.com/nyggus/easycheck/blob/master/easycheck/easycheck.py?source=post_page-----5bde06d2fb27--------------------------------)

在这段代码中查找`switch`函数。

你还会看到，你可以堆叠装饰器；以下是来自上面`easycheck`库的一个例子：

```py
@switch
@make_it_true_assertion
def assert_paths(*args: Any, handle_with: type = AssertionError, **kwargs: Any) -> None:
    return check_if_paths_exist(*args, handle_with=handle_with, **kwargs)
```

我们将在未来的文章中讨论装饰器的这些细节。

感谢阅读。如果你喜欢这篇文章，你可能也会喜欢我写的其他文章；你可以在[这里](https://medium.com/@nyggus)查看它们。如果你想加入 Medium，请使用我下面的推荐链接：

[](https://medium.com/@nyggus/membership?source=post_page-----5bde06d2fb27--------------------------------) [## 使用我的推荐链接加入 Medium - Marcin Kozak

### 阅读 Marcin Kozak 的每一个故事（以及 Medium 上的其他成千上万的作者）。你的会员费用直接支持…

medium.com](https://medium.com/@nyggus/membership?source=post_page-----5bde06d2fb27--------------------------------)
