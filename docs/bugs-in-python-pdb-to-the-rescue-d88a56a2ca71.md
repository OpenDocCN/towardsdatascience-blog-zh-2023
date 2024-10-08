# Python 中的错误？`pdb`来拯救！

> 原文：[`towardsdatascience.com/bugs-in-python-pdb-to-the-rescue-d88a56a2ca71`](https://towardsdatascience.com/bugs-in-python-pdb-to-the-rescue-d88a56a2ca71)

## PYTHON 编程

## `pdb`调试器值得学习和使用吗？

[](https://medium.com/@nyggus?source=post_page-----d88a56a2ca71--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----d88a56a2ca71--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d88a56a2ca71--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d88a56a2ca71--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----d88a56a2ca71--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d88a56a2ca71--------------------------------) ·13 分钟阅读·2023 年 9 月 21 日

--

![](img/dd08cfd6f7862fe8ce97e843516e22a3.png)

调试有助于你从失败中学习。照片由[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)拍摄，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)提供

各种工具可以用于调试 Python 代码，从最简单的`print()`函数，通过静态但更高级的`[icecream](https://github.com/gruns/icecream)`及其兄弟`[ycecream](https://github.com/salabim/ycecream)`，到各种 IDE 提供的交互式调试器。然而，我一直选择的是内置的`pdb`调试器，以及内置的`breakpoint()`函数。

# 调试

调试是编程的核心。你在开始学习编程时就开始调试，当你承诺你刚刚写完最后一行代码时你才会停止调试——如果你能遵守这个承诺的话。

你可能会认为，减少调试时间的一种方法是编写优质代码。让我们面对现实吧：往往，编写优质代码意味着……在开发过程中调试很多。诚然，一个好的程序员会编写更好的代码并犯更少的错误——但这并不意味着他或她不需要调试。

不过，有一种方法可以减少调试的次数：减少调试的方法就是编写良好的单元测试。

> 为了减少调试的次数，编写良好的单元测试。

无论你是否使用[测试驱动开发](https://en.wikipedia.org/wiki/Test-driven_development)，都要编写良好的测试。编写*良好的测试*意味着编写足够数量的精心编写的测试。我在这里不打算讨论测试，所以我给你留个想法；我在这里写了更多关于测试的内容：

[](https://medium.com/geekculture/make-yourself-enjoy-writing-unit-tests-e639711c10bd?source=post_page-----d88a56a2ca71--------------------------------) [## 让自己享受编写单元测试

### 大多数开发者不喜欢编写测试。如果你也是其中之一，尽力改变这一点。

[medium.com](https://medium.com/geekculture/make-yourself-enjoy-writing-unit-tests-e639711c10bd?source=post_page-----d88a56a2ca71--------------------------------)

我们可以假设所有程序员都需要调试他们的代码。有些人可能会说他们不需要，但这不是真的。他们确实需要；只不过他们不使用专门的调试工具，即调试器。相反，他们会运行代码来处理特定的输入，然后检查结果，然后发现有问题后修改代码并重复这个过程。因此，尽管他们不使用调试器，他们依然在调试代码；只是需要花费更多时间。调试器是有其存在意义的！

有时，单独调用 `print()` 函数就能解决问题。但不要自欺欺人：这*不是*一种非常有效的调试方法。我不是说你不应该使用它——但这是一个过于简单的方法，只能在最简单的情况下起作用。

许多使用 IDE 进行代码开发的人喜欢使用内置的调试器。Visual Studio Code 有自己的调试器，Pycharm 有一个，甚至 [Thonny](https://thonny.org/) 也有一个。

你还可以使用作为 Python 包提供的各种调试器，这些包可以从 PyPi 安装。打开 [PyPi](https://pypi.org/) 并搜索“debugger”一词；你会找到很多结果，但你可能需要花费一些耐心来找到能帮助你调试代码的工具。

你可以在下面的 *Towards Data Science* 文章中阅读关于 Python 调试器的内容：

[## 5 Python 调试工具，优于“Print”](https://docs.python.org/3/library/pdb.html?source=post_page-----d88a56a2ca71--------------------------------)

### 更快、更高效地调试你的代码。

towardsdatascience.com

文章讨论了——尽管没有展示如何使用——`pdb`、PyCharm 和 Visual Studio（以及 VS Code）的调试器、Komodo 和 Jupyter Visual Debugger。

## 静态调试器与交互式调试器

调试器可以是*静态*的也可以是*交互式*的。前者只是*展示*对象；后者则允许你*操作*它们。

两者都可能有帮助，但交互式调试器提供了最大的调试能力，因为它们能够暂停程序并查看当前状态。你可以查看和使用本地和全局作用域中的所有对象；你可以检查特定命令或命令集是否有效。这就是为什么我常常偏好交互式调试而不是静态调试。

`print()` 函数是静态调试的一个完美示例。IDE 调试器通常是交互式的。

然而，有一个调试器同时提供了简单性和强大功能。它就是 `pdb`，一个内置的交互式 Python 调试器：

[## pdb - Python 调试器](https://docs.python.org/3/library/pdb.html?source=post_page-----d88a56a2ca71--------------------------------)

### 该模块定义了一个用于 Python 程序的交互式源代码调试器。它支持设置（条件）…

[docs.python.org](https://docs.python.org/3/library/pdb.html?source=post_page-----d88a56a2ca71--------------------------------)

是的，`pdb`是*内置的*，所以你不需要安装它。它随 Python 安装包一起提供，你可以在任何环境中使用它。而且，`pdb`是*交互式的*。这实际上是我对调试器的主要期望！

> 是的，`pdb`是*内置的*，所以你不需要安装它。它随 Python 安装包一起提供，你可以在任何环境中使用它。而且，`pdb`是*交互式的*。

在本文中，我们将讨论`pdb`的基础知识。我们将介绍这个强大工具的基础知识，但要注意，它提供的功能远不止这些。好在这些基础知识足以让你开始使用`pdb`。说实话，我很少使用`pdb`的高级选项。因此，阅读这篇文章将为你提供调试 Python 代码的强大工具。

# 关于 pdb 的几点说明

`pdb`的一个优点是你可以在任何地方使用它，而无需安装任何额外的东西。即使是远程环境——`pdb`也能正常工作。只需运行它，瞧，你就有了一个可以远程使用的交互式调试器。或者在本地使用也没问题。

首先，让我解释如何使用`pdb`，然后你可以决定它是否适合你。

基本上，你可以在两种模式下使用`pdb`。首先，你可以在`pdb`模式下运行你的 Python 程序。这意味着程序会逐行执行，直到完成执行或发生错误。然后程序会在死后模式下重新运行，这意味着它会在错误发生之前停下来，你将能够查看局部和全局作用域中的情况。

其次，你可以在代码中添加一个所谓的断点，调试器将会在断点处停止程序。你还可以添加更多的断点。当然，调试器只有在断点之前没有抛出错误的情况下才能停止程序。下面，我们将讨论这两种情况。

## `pdb`模式

要在`pdb`模式下运行你的程序，只需按照以下方式运行它：

```py
$ python -m pdb myapp.py
```

这意味着`pdb`控制台将打开，`myapp.py`脚本将逐行运行。你可以更改这种行为，将其运行到第一个错误或程序结束。最好通过一些示例来展示这如何工作。

我们将使用以下脚本，保存为`myapp.py`：

```py
def foo(s):
    if not isinstance(s, str):
        raise TypeError
    return s.upper()

if __name__ == "__main__":
    for s in ("string1", "string2"):
        _ = foo(s)
```

（这是一个示例脚本，没有什么值得自豪的。我们确实需要简单的案例来进行分析。）

我们还将使用其错误版本，其中 Python 将抛出一个错误；该脚本保存在`myapp_error.py`文件中：

```py
def foo(s):
    if not isinstance(s, str):
        raise TypeError
    return s.upper()

if __name__ == "__main__":
    for s in ("string1", 10):
        _ = foo(s)
```

如你所见，正确的程序将运行一个 `for` 循环，在每次循环中，它将对不同的 `s` 参数值运行 `foo()` 函数：首先是 `"string1"` 然后是 `"string2"`，这两个值都是正确的。在错误的版本中，`foo("string2")` 应该被 `foo()` 替换为不正确的值 `10`，这将导致 `TypeError` 被抛出。

目前，你需要知道的唯一 `pdb` 命令是

+   `c`，或 `continue`；命令的另一个版本是 `cont`；

+   `n`，或 `next`；和

+   `q`，或 `quit`。

有时你需要使用`quit`两到三次，甚至更多次，才能退出调试器。

`continue` 命令会执行程序直到以下两种情况之一发生：程序结束或抛出错误。为了查看这如何工作，我们来运行我们脚本的正确版本 `myapp.py`：

```py
$ python -m pdb myapp.py
> /{path}/myapp.py(1)<module>()
-> def foo(s: str):
(Pdb) c
The program finished and will be restarted
> /{path}/myapp.py(1)<module>()
-> def foo(s: str):
(Pdb)
```

（在代码块中，`{path}` 代表从我的计算机上的长路径。）

如你所见，在运行了 shell 命令 `python -m pdb myapp.py` 之后，我们进入了一个新的 `pdb` 会话，调试器正在等待我们的第一个命令。如上所示，`c` 命令将继续程序运行直到第一个错误或程序结束。由于我们运行的是正确的脚本，调试器没有遇到任何问题，并且打印了`程序已完成，将重新启动`。这将我们移回到程序的第一行，调试器再次等待我们的命令。现在我们可以逐行调试（如下所示）。

让我们看看如果我们对错误的脚本使用 `c` 命令会发生什么：

```py
$ python -m pdb myapp_error.py
> /{path}/myapp_error.py(1)<module>()
-> def foo(s: str):
(Pdb) c
Traceback (most recent call last):
  File "/usr/lib/python3.9/pdb.py", line 1726, in main
    pdb._runscript(mainpyfile)
  File "/usr/lib/python3.9/pdb.py", line 1586, in _runscript
    self.run(statement)
  File "/usr/lib/python3.9/bdb.py", line 580, in run
    exec(cmd, globals, locals)
  File "<string>", line 1, in <module>
  File "/{path}/myapp_error.py", line 1, in <module>
    def foo(s: str):
  File "/{path}/myapp_error.py", line 3, in foo
    raise TypeError
TypeError
Uncaught exception. Entering post mortem debugging
Running 'cont' or 'step' will restart the program
> /{path}/myapp_error.py(3)foo()
-> raise TypeError
(Pdb) 
```

如你所见，这次程序引发了一个错误（`TypeError`，没有消息）。当抛出未捕获的错误时，程序会停止执行，调试器进入所谓的*事后调试*阶段。这时你可以了解你的程序发生了什么以及为何失败。

按 `n`，`pdb` 将执行代码的下一行。不是下一条命令，而是下一行，因此如果下一条命令被拆分成两行或更多行，你需要调用每一行，最终执行命令。请注意这个 `pdb` 会话：

```py
$ python -m pdb myapp.py
> /{path}/myapp.py(1)<module>()
-> def foo(s: str):
(Pdb) n
> /{path}/myapp.py(1)<module>()
-> if __name__ == "__main__":
(Pdb) 
> /{path}/myapp.py(1)<module>()
-> for s in ("string1", 10, "string2"):
(Pdb) 
> /{path}/myapp.py(1)<module>()
-> _ = foo(s)
(Pdb) 
> /{path}/myapp.py(1)<module>()
-> for s in ("string1", 10, "string2"):
(Pdb) 
> /{path}/myapp.py(1)<module>()
-> _ = foo(s)
(Pdb) 
TypeError
> /{path}/myapp.py(1)<module>()
-> _ = foo(s)
(Pdb) 
```

首先，请注意当你使用一个命令（在这里是 `n`）时，你不需要重复它来运行。`pdb` 会记住你最后的命令，按回车键将再次执行它。在按了几次之后，它把我们带到了停止程序的错误。

请注意，在 `pdb` 模式下，Tab 自动补全的行为并不完全正常。这并不意味着它完全无效；你只需要在输入其他内容之前使用 `p` 命令。例如，在这种情况下按下 Tab 键：

```py
(Pdb) al
```

将不会有任何结果。但在这里按：

```py
(Pdb) p al
```

将会完成 `alpha` 名称：

```py
(Pdb) p alpha
```

有很多 `pdb` 命令可供你使用。你可以在这里找到它们：

[](https://docs.python.org/3/library/pdb.html?source=post_page-----d88a56a2ca71--------------------------------) [## pdb - The Python Debugger

### 源代码：Lib/pdb.py 模块 pdb 定义了一个用于 Python 程序的交互式源代码调试器。它支持…

[docs.python.org](https://docs.python.org/3/library/pdb.html?source=post_page-----d88a56a2ca71--------------------------------)

在继续之前，我想与您分享一个简单的命令；也许它不是*最*重要的，但在我过去的经验中我非常欣赏它。它是`pp`，用于漂亮打印：

```py
(Pdb) {f"{x_i = }, {alpha = }, and {beta = }": (x_i + alpha)/(1 + beta) for x_i in x}
{'x_i = 1, alpha = 4, and beta = 0': 5.0, 'x_i = 2, alpha = 4, and beta = 0': 6.0, 'x_i = 3, alpha = 4, and beta = 0': 7.0}
(Pdb) pp {f"{x_i = }, {alpha = }, and {beta = }": (x_i + alpha)/(1 + beta) for x_i in x}
{'x_i = 1, alpha = 4, and beta = 0': 5.0,
 'x_i = 2, alpha = 4, and beta = 0': 6.0,
 'x_i = 3, alpha = 4, and beta = 0': 7.0}
```

正如您所见，调用一个表达式和使用 `pp` 命令调用它之间差别很大。因此，记住它是好的。

还有一件事。即使上面的字典推导很长，我也没有将其拆分成两行或更多行。这是因为 `pdb` 不允许这样做，至少在其调试模式下是如此——但您可以使用其交互模式，您可以通过 `interact` 命令运行它：

```py
(Pdb) interact
>>> {f"{x_i = }, {alpha = }, and {beta = }":
...      (x_i + alpha)/(1 + beta) for x_i in x}
{'x_i = 1, alpha = 4, and beta = 0': 5.0, 'x_i = 2, alpha = 4, and beta = 0': 6.0, 'x_i = 3, alpha = 4, and beta = 0': 7.0}
```

记住，在交互模式下，`pdb` 命令不起作用。要离开此模式并返回 `pdb` 模式，请按 `<Ctrl + D>`。

## 使用 breakpoint() 函数进行调试

上面我们讨论了在 `pdb` 模式下调试。然而，通常情况下，设置一个所谓的*断点*会更容易。断点是代码中的一个位置，在这个位置您希望程序暂停并进行分析；您可以在代码中创建多个断点，代码会在每个断点处停止——除非抛出错误。

要创建一个，请在您希望调试器停止并让您进入的代码位置添加对 `breakpoint()` 函数的调用：

```py
def y(x, alpha, beta):
    breakpoint()
    return [(xi + alpha)/(1 + beta) for xi in x]

x = [1, 2, 3]
y(x)
```

运行此脚本将引导您进入此调试会话：

```py
-> return [(xi + alpha)/(1 + beta) for xi in x]
(Pdb) l
  1     def y(x, alpha, beta):
  2         breakpoint()
  3  ->     return [(xi + alpha)/(1 + beta) for xi in x]
  4  
  5  
  6     x = [1, 2, 3]
  7     y(x, 4, 0)
[EOF]
(Pdb) 
```

`l`（`list`）命令显示您当前所在位置周围的十一行。您还可以使用 `ll`（`longlist`），它将打印当前函数或帧的整个源代码。

其余部分与之前相同，因为您已经进入了我们上面讨论的 `pdb` 模式。使用 `breakpoint()` 函数的明显优势是可以精确地在您希望的地方停止程序。坦白说，我在几乎所有的调试会话中都使用 `breakpoint()`。

![](img/f7bf66da240f342b5437058c6e603c69.png)

代码中的断点让您暂停片刻，检查您希望检查的代码位置的内部情况。照片由 [Malte Helmhold](https://unsplash.com/@maltehelmhold?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上拍摄

# 对象丢失了？

您可能会遇到一种奇怪的情况——虽然它只对那些不知道如何处理的人来说很奇怪。有时，您可能会发现 `pdb` 的行为非常特殊：虽然它可以看到局部变量，但它……看不到这些局部变量。

听起来像完全的废话？让我解释一下。考虑这个非常简单的函数：

```py
def y(x, alpha, beta):
    return [(xi + alpha)/(1 + beta) for xi in x]
```

它计算一个简单模型的值，针对一个值列表 `x`，给定两个模型参数 `alpha` 和 `beta`。例如：

```py
>>> def y(x, alpha, beta):
...     return [(xi + alpha)/(1 + beta) for xi in x]
... 
>>> x = [1, 2, 3]
>>> y(x, .25, 0)
[1.25, 2.25, 3.25]
```

现在想象一下，您希望进入函数并检查多个 `x` 列表的函数。您可以通过 `pdb` 的帮助来做到这一点：

```py
>>> def y(x, alpha, beta):
...     breakpoint()
...     return [(xi + alpha)/(1 + beta) for xi in x]
... 
>>> y(x, .25, 0)
> <stdin>(3)y()
(Pdb) alpha, beta
(0.25, 0)
(Pdb) [(xi + alpha)/(1 + beta) for xi in x]
*** NameError: name 'alpha' is not defined
```

什么？刚刚发生了什么？为什么`pdb`看不到`alpha`——它不是刚刚看到的吗？确实，在这一行：

```py
(Pdb) alpha, beta
(0.25, 0)
```

所以，它能看到`alpha`和`beta`——但它看不到它们？

也许我们应该再次给这些变量赋值？让我们检查一下：

```py
(Pdb) alpha = .25; beta = 0
(Pdb) alpha
0.25
(Pdb) [(xi + alpha)/(1 + beta) for xi in x]
*** NameError: name 'alpha' is not defined
```

不，这根本没有帮助。

问题是，列表推导式——以及其他推导式——有自己的作用域，局部变量在那里是不可见的。幸运的是，你有很多解决方案，如下所示。

*交互模式*

交互模式实际上在各种情况下都非常有用。你可以使用`pdb` shell 中的`interact`命令来启动它：

```py
(Pdb) interact
*interactive*
>>> [(xi + alpha)/(1 + beta) for xi in x]
[1.25, 2.25, 3.25]
```

如你所见，在交互模式下，代码的运行方式是正常的。

*将缺失的对象添加到 globals*

缺少一个特定的对象，所以只需将其添加到`globals()`中：

```py
(Pdb) globals()['alpha'] = alpha
(Pdb) [(xi + alpha)/(1 + beta) for xi in x]
*** NameError: name 'beta' is not defined
```

如你所见，`pdb`可以看到`alpha`但看不到`beta`。一种解决方案是将其添加到`globals()`中，就像我们添加`alpha`一样，但逐个提供所有全局变量并不好玩；下一个解决方案只需一条命令即可完成。

*将所有局部变量添加到 globals*

`locals()`和`globals()`都是字典，因此我们可以简单地将前者添加到后者中。你可以按照以下方式进行：

```py
(Pdb) globals().update(locals())
(Pdb) [(xi + alpha)/(1 + beta) for xi in x]
[1.25, 2.25, 3.25]
```

希望你喜欢这篇文章。虽然文章没有涵盖`pdb`的所有知识，但它提供了足够的知识来在大多数情况下使用这个调试工具。

在我超过 5 年的 Python 实践中，我注意到很少有人使用`pdb`来调试代码。我不知道为什么。IDE 调试工具确实能提供更多，但`pdb`的强大之处在于它在 Python 标准库中的可用性。

我不确定这是否值得自豪，但我会对你诚实：`pdb`是我选择的调试工具。我几乎不使用其他调试工具。我从未遇到过任何问题；相反，它在我所有的 Python 项目中都提供了帮助。

当我在尝试其他调试工具时，确实遇到了各种问题。也许是我自己的问题；也许是我没有足够长时间地使用它们以体验它们的强大。这可能是真的——但我可以说我已经足够长时间地使用`pdb`，尽管它很简单，但它可以是一个很棒的调试工具。

感谢阅读。如果你喜欢这篇文章，你可能也会喜欢我写的其他文章；你可以在这里看到它们。如果你想加入 Medium，请使用下面的推荐链接：

[链接](https://medium.com/@nyggus/membership?source=post_page-----d88a56a2ca71--------------------------------) [## 使用我的推荐链接加入 Medium - Marcin Kozak

### 作为 Medium 的会员，你的部分会员费会分给你阅读的作者，同时你可以完全访问每一个故事……

[链接](https://medium.com/@nyggus/membership?source=post_page-----d88a56a2ca71--------------------------------)
