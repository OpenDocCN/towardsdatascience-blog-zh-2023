# 《… Python 中的捕手：用一个工具捕获异常和警告》

> 原文：[`towardsdatascience.com/the-catcher-in-the-python-catch-exceptions-and-warnings-with-one-tool-81714117da76`](https://towardsdatascience.com/the-catcher-in-the-python-catch-exceptions-and-warnings-with-one-tool-81714117da76)

## PYTHON PROGRAMMING

## 为什么不在 Python 中实现一个同时捕获异常和警告的工具呢？

[](https://medium.com/@nyggus?source=post_page-----81714117da76--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----81714117da76--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81714117da76--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----81714117da76--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----81714117da76--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----81714117da76--------------------------------) ·13 min read·2023 年 5 月 23 日

--

![](img/6f42c45eba05eb69d8a1a9adcc012803.png)

这篇文章讨论了在 Python 中捕获异常和警告的问题。照片由 [Keith Johnston](https://unsplash.com/de/@acfb5071?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 有一些很好的异常处理方法。你可以捕获它们、引发它们、重新引发它们、同时引发两个异常（或者说，一个异常引发另一个异常）、记录它们，还可以创建自定义错误及其层次结构，只列举了最重要的一些。如果你想处理警告，你也可以做到——但你需要两个不同的工具：一个用于异常，一个用于警告。

阅读这篇文章，了解如何编写一个同时捕获异常和警告的工具，以及如何根据需求调整它。

我非常喜欢 Python 的异常处理。我喜欢它的简洁性，比如这里展示的：

```py
class MultiplicationError(Exception): ...

def multiply_str(x: str, n: int) -> str:
    try:
        xn = x * n
    except TypeError as e:
        raise MultiplicationError(
            "Can't multiply objects of "
            f"{type(x).__name__} "
            f"and {type(n).__name__} types"
            ) from e
    if not isinstance(xn, str):
        raise MultiplicationError(
            f"{type(x).__name__} multiplied by"
            f" {type(n).__name__} "
            "does not give str object by "
            f"{type(xn).__name__}"
            ) from TypeError
    return xn
```

在分析函数之前，让我们看看它的实际应用：

![](img/a88c74fc599d02c8e7b5c31090fae843.png)

multiply_str() 函数的实际应用；来自 Python REPL 的截图。图片由作者提供

前两个调用是正确的。第二个调用不正确，因为我们试图乘以两个字符串。第三个调用也不正确，但这次是因为函数接收了两个整数值。它们可以被相乘，但我们的函数通过检测结果对象不是字符串来捕获这个场景——这基本上意味着出了问题，因此产生了异常。

我们在这里看到的被称为鸭子类型。我们不是检查输入值是否具有正确的类型；而是执行函数的目标——在这里是乘以两个值。如果一切顺利，那么……就是好的。鸭子类型是 Python 中一种典型的动态方法。

然而，在这个函数中，情况要复杂一些：该函数也将与两个数值一起工作，这是我们不感兴趣的。我们*不*希望函数具有多态性：我们希望它与 `x` 为字符串和 `n` 为整数一起工作。

因此，我们可以检查它们的类型，并引发与上述相同的异常。在上述版本中，我们检查返回值的类型，这是一种更快的方法。它更快，因为我们只检查一个对象的类型（`xn`），但仅在我们知道 `x` 和 `n` 可以相乘时才进行检查。因此，这并不是真正的鸭子类型。然而，通常你会选择纯粹的鸭子类型，这是最自然的 Python 方法。

然而，在这篇文章中，我们讨论的是别的内容：捕获错误，所以让我们继续。`multiply_str()` 函数将一个字符串 `x` 乘以一个整数 `n`，但以受控的方式进行——这意味着如果出现问题，相应的异常会被捕获并使用自定义异常引发。实际上，它做的还不止这些：

+   已添加自定义消息。

+   `MultiplicationError` 从 `TypeError` 中引发。

后者在我们讨论的背景下很有趣。实际上，它帮助你使用 `except MultiplicationError` 和 `except TypeError` 捕获 `multiply_str` 的错误。两者都会捕获 `MultiplicationError` —— 即使 `TypeError` 与前者没有（程序上）关系。所以，这：

```py
try:
    return multiply_str("test", "it")
except TypeError as e:
    print(e)
    return multiply_str("test", 1)
```

将捕获与此相同的错误：

```py
try:
    return multiply_str("test", "it")
except MultiplicationError as e:
    print(e)
    return multiply_str("test", 1)
```

这在各种情况下都很有用，尤其是当一个人在其自己定义 `multiply_str()` 的包之外使用 `multiply_str()` 时。否则，用户将不得不导入 `MultiplicationError`。尝试捕获 `TypeError` 更简单，这也允许你捕获 `MultiplicationError`。

当然，Python 中使用异常的内容远不止这些。你可以从这篇文章中了解更多关于自定义异常的内容：

[](/should-we-use-custom-exceptions-in-python-b4b4bca474ac?source=post_page-----81714117da76--------------------------------) ## 我们是否应该在 Python 中使用自定义异常？

### Python 有这么多内置异常，以至于我们很少需要创建和使用自定义异常。或者说，我们需要吗？

towardsdatascience.com

你可能还会发现这篇文章很有趣：

[](https://betterprogramming.pub/how-to-overwrite-asserterror-in-python-and-use-custom-exceptions-c0b252989977?source=post_page-----81714117da76--------------------------------) [## 如何在 Python 中重写 AssertionError 并使用自定义异常

### Python 的 assert 语句使用 AssertionError。了解如何使用其他异常代替。

betterprogramming.pub](https://betterprogramming.pub/how-to-overwrite-asserterror-in-python-and-use-custom-exceptions-c0b252989977?source=post_page-----81714117da76--------------------------------)

但现在，让我们继续讨论警告。

# 处理警告

警告的引发和捕获方式类似。

```py
import warnings

class MultiplicationError(Exception): ...

class MultiplicationWarning(Warning): ...

def multiply_str(x: str, n: int) -> str:
    try:
        xn = x * n
    except TypeError as e:
        raise MultiplicationError(
            "Can't multiply objects of "
            f"{type(x).__name__} "
            f"and {type(n).__name__} types"
            ) from e
    if not isinstance(xn, str):
        warnings.warn(
            f"{type(x).__name__} multiplied by"
            f" {type(n).__name__} "
            "does not give str object by "
            f"{type(xn).__name__}",
            MultiplicationWarning
            )
    return xn
```

当乘法过程中抛出异常时，我们仍然会抛出异常，但当返回的对象不是字符串时，会发出警告。看看这是如何工作的：

![](img/3718005531b811c86fac54dfade5550f.png)

`multiply_str()` 函数的实际操作，这次有异常和警告；来自 Python REPL 的截图。图片由作者提供

如你所见，得到警告时，你会收到警告，但也会得到输出，这与异常的情况不同。你需要记住，Python 的默认行为是只警告一次：

![](img/c4001c19e48aac36c42204d0c8797023.png)

默认行为是只发出一次特定的警告；来自 Python REPL 的截图。图片由作者提供

这假设你会记住你已经被警告过，所以你得自己解决。你可以改变这种行为，但我们会在其他时间讨论这个问题。改变这种行为很简单。请看下面的截图*来自一个新会话*，代码与之前导入的一样：

![](img/2d8b5cca9d1fc0d4b621f9f02f6095e7.png)

一个“始终”警告过滤器会改变默认行为；来自 Python REPL 的截图。图片由作者提供

从现在开始，我们将使用这个警告过滤器。

警告远不止这些。让我们在这里停下，今天我们主要的目标是实现一个可以同时捕捉异常和警告的工具。

# 定义捕捉器

好的，我们要实现一个工具，使我们能够捕捉到异常和警告。首先，让我们看看如何捕捉我们自定义的`MultiplicationError`：

```py
my_string = 10
factor = 2

try:
    mult_string = multiply_str(my_string, factor)
except MultiplicationError as e:
    mult_string = my_string
    print(
        "Error during multiplication of "
        f"{my_string} by {factor}: {e}"
    )
```

和警告：

```py
with warnings.catch_warnings(record=True) as w:
    mult_string = multiply_str(my_string, factor)
```

当然，这些只是一些基本示例。我们可以根据希望返回给用户的内容以各种方式实现它。暂时，我们返回一个按捕捉时间排序的异常和警告列表。

这是最简单的例子之一：

```py
from typing import Callable

def catch(func: Callable) -> list[str]:
    """Catch issues and warnings using one function."""
    to_return = None
    issues = []
    with warnings.catch_warnings(record=True) as warning:
        try:
            to_return = func()
        except Exception as e:
            issues.append(f"Exception: {e}")
        finally:
            for w in warning:
                issues.append(f"Warning: {w.category.__name__}:"
                              f" {w.message}")
    return to_return, issues
```

让我们看看它是如何工作的：

```py
>>> class DivisionWarning(Warning): ...
>>> def divdiv(x, y):
...     warnings.warn("Watch out!", DivisionWarning)
...     return x / y / x
>>> results = catch(lambda: divdiv(1, 0))[1]
>>> results
(None, ['Exception: division by zero', 'Warning: DivisionWarning: Watch out!'])
```

`divdiv()` 函数确实不自然，因为它总是发出警告，但我想展示一个非常简单的例子。运行不正确的命令 `divdiv(1, 0)` 后，捕捉器包含了一个异常和一个警告。

我们可以稍微改进一下`catch()`函数。虽然许多 Python 爱好者喜欢从函数返回两元素或三元素的元组，但你也可以返回一个命名元组：

```py
from collections import namedtuple

def catch(func: Callable) -> list[str]:
    """Catch issues and warnings using one function."""
    to_return = None
    issues = []
    with warnings.catch_warnings(record=True) as warning:
        try:
            to_return = func()
        except Exception as e:
            issues.append(f"Exception: {e}")
        finally:
            for w in warning:
                issues.append(f"Warning: {w.category.__name__}:"
                              f" {w.message}")
    CatchReturn = namedtuple("CatchReturn", "to_return issues")
    return CatchReturn(to_return, issues)
```

我们可以在函数外定义`CatchReturn`；也可以在函数内部动态定义，使用以下返回行替代上面函数的最后两行：

```py
return namedtuple("CatchReturn", "to_return issues")(to_return, issues)
```

无论如何，使用这个命名元组，函数肯定会返回一个命名元组：

```py
>>> class DivisionWarning(Warning): ...
>>> def divdiv(x, y):
...     warnings.warn("Watch out!", DivisionWarning)
...     return x / y / x
>>> results = catch(lambda: divdiv(1, 0))
>>> results
CatchReturn(to_return=None, issues=['Exception: division by zero', 'Warning: DivisionWarning: Watch out!'])
>>> results.to_return # None
```

好的一点是，你可以忽略它是一个命名元组，将它像普通元组一样对待：

```py
>>> to_return, issues = catch(lambda: divdiv(1, 0))
>>> to_return # None
>>> issues
['Exception: division by zero', 'Warning: DivisionWarning: Watch out!']
```

好的，这就是一个基本版本的联合捕获器。但你可以用这样的捕获器做更多的事情！下面，我展示了两个更多的示例，只有基本的注释。你可以享受这些示例的时间以及理解它们实现的努力。如果你有任何问题，请随时在评论中提问。如果你想到其他你想分享的示例，请务必告诉我！

## 示例：基于闭包的多个捕获器

之前的捕获器与单个函数调用一起工作。在这个示例中，我们将使用闭包来构建一个可以用于许多后续函数调用的捕获器。如果某个调用顺利进行，捕获器将简单地返回结果。如果发出了警告，它将返回输出并记录警告。如果引发了异常，它将捕获异常——但完全不会返回。

注意实现：我使用了闭包。Python 中闭包最常见的例子是装饰器，但闭包也可以用于其他目的。在这里，我们将使用它们来保持状态；我们可以使用类（实际上，我们将在下一个示例中这样做），但正如你将看到的，闭包提供了一种更简单的方法。

```py
def catch_all() -> list[str]:
    """Catch multiple issues and warnings using one function."""
    issues = []
    def catch_one(func: Optional[Callable] = None) -> Callable:
        to_return = None
        if not func:
            return issues
        with warnings.catch_warnings(record=True) as warning:
            try:
                to_return = func()
            except Exception as e:
                issues.append(f"{type(e).__name__}: {e}")
            finally:
                for w in warning:
                    issues.append(f"{w.category.__name__}:"
                                  f" {w.message}")
        return to_return
    return catch_one
```

现在，让我们看看这个函数的实际操作。首先，我们需要创建一个捕获器对象：

```py
>>> all_catcher = catch_all()
```

我们现在将使用`all_catcher`对象来调用多个函数。我将使用与上面相同的`DivisionWarning`和`divdiv()`函数。

```py
>>> x1 = all_catcher(lambda: divdiv(1, 0))
>>> x1 # this is None
```

这个捕获器与定义为`lambda`的函数一起使用，尽管我们可以使用不同的结构；我们将在下一个示例中这样做。上面，`x1`是`None`，因为除了`DivisionWarning`之外，捕获器还捕获了一个异常（`ZeroDivisionError`）。要查看`issues`列表，只需调用`all_catcher()`（感谢`catch_one()`函数中的`if`块）：

```py
>>> all_catcher()
['ZeroDivisionError: division by zero', 'DivisionWarning: Watch out!']
```

如果我们调用一个正常工作的函数，我们将得到其返回值，捕获器将不会记录任何内容（所以其`issues`列表将保持不变）：

```py
>>> x2 = all_catcher(lambda: divdiv(1, 1))
>>> x1
1.0
>>> all_catcher()
['ZeroDivisionError: division by zero', 'DivisionWarning: Watch out!']
```

让我们再添加一些调用：

```py
>>> all_catcher(lambda: divdiv(1, 0)) # None again
>>> all_catcher(lambda: 1 + 1)
2
```

捕获器的网应该包含比之前更多的内容¹：

```py
>>> all_catcher() # doctest: NORMALIZE_WHITESPACE
['ZeroDivisionError: division by zero',
 'DivisionWarning: Watch out!',
 'ZeroDivisionError: division by zero',
 'ZeroDivisionError: division by zero',
 'DivisionWarning: Watch out!']
```

它确实如此。错误和警告在这里按捕获顺序记录，但这只是各种可能性中的一种。

哦，还有一件事。你可以在一个会话/脚本中使用更多这样的捕获器：

```py
>>> catcher1 = catch_all()
>>> catcher2 = catch_all()
>>> catcher1(lambda: 1 / 0)
>>> catcher2(lambda: 1 * 2)
2
```

与`catcher1`不同，`catcher2`应该是空的。让我们检查一下：

```py
>>> catcher1()
['ZeroDivisionError: division by zero']
>>> catcher2()
[]
```

作为另一个练习，你可以在`catch_all()`中使用不同的数据结构，而不是列表。或者将异常和警告记录到不同的列表中。或者使用`logging`模块记录。或者为每个元素添加时间戳。

## 示例：基于类的捕获器

我们可以实现一个基于类的捕获器，它的工作方式与我们上面使用闭包实现的捕获器相同，但我将把这留给你作为练习。相反，我将向你展示如何做一些更有趣的事情，即实现一个与`|`管道运算符一起工作的捕获器。

在 Python 中，管道运算符不太流行，但它们确实存在。你可以在`Pipe`包中看到它们：

[## GitHub - JulienPalard/Pipe: 用于在 Python 中使用中缀表示法的 Python 库](https://github.com/JulienPalard/Pipe?source=post_page-----81714117da76--------------------------------)

### 模块启用类似 sh 的中缀语法（使用管道）。作为示例，这里是第 2 个欧拉项目的解决方案……

[github.com](https://github.com/JulienPalard/Pipe?source=post_page-----81714117da76--------------------------------)

虽然 Python 不是一种函数式编程语言，但它提供了各种工具，你可以用来至少模拟函数式编程范式。`Pipe`包是一个很好的示例，展示了如何使用管道来实现这一点。

然而，我们将实现我们自己的管道操作符，通过重载`|`操作符：

```py
class Catcher:
    def __init__(self):
        self.issues = []

    def __or__(self, other):
        to_return = None        
        with warnings.catch_warnings(record=True) as w:
            try:
                to_return = other()
            except Exception as e:
                self.issues.append(f"Exception: {e}")
            finally:
                for ww in w:
                    self.issues.append(f"Warning: {ww.category.__name__}:"
                                       f" {ww.message}")
        return to_return

    def __call__(self):
        return self.issues

    def __repr__(self):
        n = len(self.issues)
        msg = "A Catcher instance. Until now, it caught"
        if not n:
            msg = f"{msg} no exceptions and warnings."
            return msg
        msg = f"{msg} {n} element{'s' if n > 1 else ''}."
        return msg

catcher = Catcher()
```

让我们使用这个`catcher`：

```py
>>> catcher
A Catcher instance. Until now, it caught no exceptions and warnings.
>>> catcher | (lambda: divdiv(1, 1))
1.0
>>> catcher
A Catcher instance. Until now, it caught 1 element.
>>> catcher()
['Warning: DivisionWarning: Watch out!']
>>> to_return = catcher | (lambda: divdiv(1, 1))
>>> to_return
1.0
>>> catcher
A Catcher instance. Until now, it caught 2 elements.
```

这是基本用法。让我们定义几个助手，以便给我们一些灵活性：

```py
import functools

class Printer:
    def __ror__(self, other, **kwargs):
        print(other, **kwargs)

class Ignorer:
    def __ror__(self, other):
        pass

Run = functools.partial
Print = Printer()
Ignore = Ignorer()
```

`Run`函数可以用来替代`lambda`语法（下面，你可以比较这两种调用函数的方式）：

```py
>>> to_return = catcher | Run(divdiv, 1, 1)
>>> to_return
1.0
>>> to_return = catcher | (lambda: divdiv(1, 0))
>>> to_return # None
>>> catcher() # doctest: NORMALIZE_WHITESPACE
['Warning: DivisionWarning: Watch out!',
 'Warning: DivisionWarning: Watch out!',
 'Warning: DivisionWarning: Watch out!']
```

但是如果你使用`Print`来显然地打印，你会看到`None`：

```py
>>> catcher | Run(divdiv, 0, 0) | Print
None
>>> catcher
A Catcher instance. Until now, it caught 7 elements.
>>> catcher() # doctest: NORMALIZE_WHITESPACE
['Warning: DivisionWarning: Watch out!',
 'Warning: DivisionWarning: Watch out!',
 'Warning: DivisionWarning: Watch out!',
 'Exception: division by zero',
 'Warning: DivisionWarning: Watch out!',
 'Exception: division by zero',
 'Warning: DivisionWarning: Watch out!']
```

但是使用`Print`时，捕获器*仅仅*打印而不返回任何东西。你可以修改它，使其同时完成这两项功能，在这种情况下，它可以作为一个简单的调试工具。

我们剩下的是`Ignore`。它并不会真正忽略与之运行的函数；它忽略该函数的任何输出，捕获异常和警告：

```py
>>> catcher | Run(divdiv, 0, 0) | Ignore
>>> catcher
A Catcher instance. Until now, it caught 9 elements.
```

`Run`、`Print`和`Ignore`只是三个示例，如果你有空闲时间，可以考虑其他可以与这个捕获器一起使用的示例。请注意，我使用了一种不典型的——甚至非 Pythonic 的——方法，其中我用大写字母命名与函数类似的类实例（好吧，更准确地说是类似于函数），以区分`Print`和内置的`print`。虽然其他对象在标准 Python 库中没有对应物，我还是希望对所有这些与捕获器一起工作的对象使用相同的命名约定。

我并不是说这是一种好的方法——但我认为展示一下在某些情况下你可以尝试使用不典型语法是个好主意。然而，如果你决定这样做，即走非主流路线，请始终三思你所做的是否对 Python 来说过于不典型。并准备好捍卫你的立场。

# 结论

这篇文章旨在展示为什么实现一个联合捕获警告和异常的工具是值得的，并演示如何实现一些示例。我的目标不是为你实现一个可以在项目中使用的工具，而是展示如何实现这样的工具。

这并不意味着我们讨论的捕获器还没有准备好直接使用；它们已经准备好了。不过，我希望现在你已经掌握了这些知识后，能够实现适应你项目的自定义捕获器。

如果你有任何评论或建议——无论是对我还是对其他读者——请随时在评论中分享。

# 脚注

¹ 你可以在代码中看到一个`doctest`标志，因为我使用它来测试本文中的代码。`doctest`是一个用于文档测试的标准库模块。如果你有兴趣了解更多关于这个有趣模块的内容，可以参考这篇文章：

[](/python-documentation-testing-with-doctest-the-easy-way-c024556313ca?source=post_page-----81714117da76--------------------------------) ## 使用 doctest 进行 Python 文档测试：简单方法

### doctest 允许进行文档、单元和集成测试，以及测试驱动开发。

towardsdatascience.com

感谢阅读。如果你喜欢这篇文章，你也可能会喜欢我写的其他文章；你可以在[这里](https://medium.com/@nyggus)查看。如果你想加入 Medium，请使用下面的推荐链接：

[](https://medium.com/@nyggus/membership?source=post_page-----81714117da76--------------------------------) [## 使用我的推荐链接加入 Medium - Marcin Kozak

### 阅读 Marcin Kozak 和其他成千上万的 Medium 作者的每一篇故事。你的会员费直接支持…

medium.com](https://medium.com/@nyggus/membership?source=post_page-----81714117da76--------------------------------)
