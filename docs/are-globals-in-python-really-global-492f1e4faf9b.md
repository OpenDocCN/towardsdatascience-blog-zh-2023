# Python 中的全局变量真的全局吗？

> 原文：[`towardsdatascience.com/are-globals-in-python-really-global-492f1e4faf9b`](https://towardsdatascience.com/are-globals-in-python-really-global-492f1e4faf9b)

## PYTHON 编程

## 学习一个技巧，使 Python 对象真正成为全局的。

[](https://medium.com/@nyggus?source=post_page-----492f1e4faf9b--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----492f1e4faf9b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----492f1e4faf9b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----492f1e4faf9b--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----492f1e4faf9b--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----492f1e4faf9b--------------------------------) ·阅读时间 8 分钟·2023 年 11 月 17 日

--

![](img/4f883bbad7e256509273e34303fb4715.png)

真正的全局变量意味着可以从任何地方访问。照片由[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 是否提供全局变量？

直接的回答是，Python 确实提供了全局变量。确实，只需查看[Python 官方文档](https://docs.python.org/3/faq/programming.html#what-are-the-rules-for-local-and-global-variables-in-python)即可了解到…

> 在 Python 中，仅在函数内部引用的变量是隐式全局的。如果一个变量在函数体内的任何地方被赋值，默认假设它是局部变量，除非明确声明为全局变量。

所以，Python 确实提供了全局变量。更重要的是，全局变量构成了一个相当有争议的话题，因为使用它们可能会给开发者和用户带来严重的困难。

你可能会想，为什么我们要使用这么有争议的编程工具。这是一个合理的问题——但答案很简单。全局变量是编程工具之一，只要正确使用，它们可以非常有用。然而，如果使用不当，它们可能会带来更多的坏处而非好处。

> 全局变量是编程工具之一，只要正确使用，它们可以非常有用。然而，如果使用不当，它们可能会带来更多的坏处而非好处。

全局变量可以从程序中的任何地方访问。因此，如果你需要一个特定对象在任何地方都能访问——这就是你创建全局对象的原因。

你可以创建在程序执行期间不改变的全局常量；以及可以改变的全局变量。因此，全局常量是字面值。例如，可以是公司的名称、圆周率的值或任何数学常量。全局变量可以是价格、需求或颜色；这些值可以改变，这就是它们是变量的原因。

[Python 的命名约定](https://peps.python.org/pep-0008/)如下：全局常量使用大写字母（`PI`），全局变量使用小写字母（`price`）。

全局变量是编程中的一个典型话题，但这并不意味着它们在每种编程语言中都表现相同。Python 中的全局变量有其自身的特殊性，任何 Python 开发者都必须了解它们的工作方式。

在本文中，我们将探讨什么构成全局变量的本质：全局作用域。我会向你展示在 Python 上下文中*全局*的含义，以及我们通常认为的 Python 中的全局变量并不一定是*真正的全局*。

我们还将分析这个问题的影响——我会展示如何在编码实践中利用这些知识。这需要知识、技能和——最重要的是——谨慎，因为这些是相当微妙的事项，容易导致重大错误。

# 什么是全局变量？

本文旨在探讨全局变量的本质：全局作用域。**全局变量**具有全局作用域，其值可以改变，而**全局常量**则具有全局作用域，其值不可改变。

我们将讨论是否在 Python 程序中使用全局变量的事宜留到另一天。首先，这是一个如此重要的话题，值得我们全神贯注，因此值得一篇专门的文章。其次，讨论这些内容时，我们需要了解更多关于 Python 中的全局作用域的知识，这是本文所介绍的。

## 全局变量一般

正如 Tony Gaddis 在他的[书籍](https://media.pearsoncmg.com/bc/abp/cs-resources/products/product.html#product,isbn=0134801156)中解释的，**全局变量**可以在程序中的所有模块中使用。换句话说，全局作用域是整个程序，因此全局变量或常量可以在程序的每个模块中使用。

这是一个非常简洁的解释，但这正是我们所需的，因为全局变量的定义并不复杂。

## Python 中的全局变量

你会发现 Python 中的全局定义更复杂。有趣的是，官方 Python 文档对全局变量的描述并不多。你可以阅读我上面链接的小节，也可以阅读[the](https://docs.python.org/3/reference/simple_stmts.html#the-global-statement) `[global](https://docs.python.org/3/reference/simple_stmts.html#the-global-statement)` [statement itself](https://docs.python.org/3/reference/simple_stmts.html#the-global-statement)。你还应该注意[PEP8](https://peps.python.org/pep-0008/#global-variable-names)中的这一点，其中样式指南解释了全局变量的命名约定：

> 希望这些变量仅用于一个模块内部。

换句话说，*希望全局变量是用于模块级别，而不是完全的全局级别*。

问题是，你知道如何使用 Python 中真正全局的对象吗？在接下来的部分，我将向你展示它们是什么以及如何使对象在整个程序中全局。这样的对象在特定的 Python 会话中可以从任何模块访问，而无需导入，就像`sum`、`list`和许多其他对象一样，它们在不实际导入的情况下就可以使用。

## 模块全局作用域

模块全局作用域就是字面上的意思：特定模块的全局作用域。最好通过一个例子来展示这一点：

```py
# module_scopes.py

# define globals
COMPANY_NAME = "The Global Bindu Company"
year = 2023

# use globals in a function
def represent():
    return f"{COMPANY_NAME} in {year}"
```

好吧，那么我们有什么呢？首先，我们有两个全局对象：

+   `COMPANY_NAME` — 一个全局常量

+   `year` — 一个全局变量

然后我们有一个函数`represent()`，它使用了这两个全局变量。让我们看看它的实际效果：

```py
>>> import module_scopes
>>> module_scopes.represent()
The Global Bindu Company in 2023
>>> module_scopes.year = 2024
>>> represent()
The Global Bindu Company in 2024
```

你刚刚看到的是 Python 中通常理解的全局作用域：模块全局作用域。现在是时候继续讨论程序全局作用域了。

## 程序全局作用域

在 Python 中，唯一可以从所有模块访问的全局变量是那些在`builtins`模块中的变量。

它包含了例如上述提到的`sum`和`list`对象以及许多其他对象，如异常（例如，`ValueError`或`Exception`）或各种函数（例如，`any`或`all`）。所以，基本上，任何通过`builtins`模块提供的东西都可以在 Python 会话中的所有模块中使用。

现在，进行我们的简单小技巧。如果你对 Python 不陌生，你可能已经自己想到这个方法了。如果你想让一个对象成为全局的，只需将其添加到`builtins`模块中。这会立即使该对象在会话中的所有模块中可用，而无需导入任何内容。当然，我们说的是*特定的会话*，即对象已被添加到`builtins`作用域的会话。

> 如果你想让一个对象成为全局的，只需将其添加到`builtins`模块中即可。

让我们看看这个技巧是如何工作的。首先，创建一个`main.py`模块：

```py
# main.py

import builtins

builtins.SCREAM = "SCREAM!!!"
```

现在创建另一个模块，`use.py`，位于同一目录下：

```py
# use.py

def scream(n: int) -> str:
    return SCREAM * n
```

正如你所见，`scream()` 函数使用了 `SCREAM` 对象，即使它在 `use` 模块中既没有定义也没有导入。我们确切知道的是，它在 `main` 模块中的 `builtins` 模块中被定义并添加。这样会有效吗？

首先，请注意 [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance) 不会喜欢在 `use` 模块中使用 `SCREAM` 对象：

![](img/b5879ece268772f2eed871a2a77f0136.png)

[Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance) 并不知道 SCREAM 已经被添加到内建函数中。截图来自 Visual Studio Code，作者提供

`[mypy](https://mypy.readthedocs.io/en/stable/)` 也不会喜欢：

![](img/e2b4e19ce451a9364511bcd80ac730ba.png)

`[Mypy](https://mypy.readthedocs.io/en/stable/)` 并不知道 SCREAM 已经被添加到内建函数中。截图来自 Visual Studio Code，作者提供

那么，这是否被认为是静态错误呢？

即使我们能让它动态运行，这也是一个静态错误，因为是否能正常工作取决于导入的顺序。如果你只是导入`use`并运行`scream()`函数，你将会遇到一个错误：

![](img/b3c8d92db6a6d0bbf3fbbd6b3bfdd588.png)

use.scream(2)失败：SCREAM 未定义。截图来自 Visual Studio Code，作者提供

但是，如果你先导入`main`然后再导入`use`，你可能会惊讶地发现`scream()`函数可以正常工作：

![](img/a759c4deb36a4f9836c44a4fb07d1a21.png)

use.scream(2) 成功：SCREAM 已经被添加到内建函数中。截图来自 Visual Studio Code，作者提供

这次它起作用了，因为当我们导入`main`时，这些代码行被执行了：

![](img/36855bf89b0521963bab657e66d1b8cb.png)

内建函数破解：SCREAM 现在是一个真正的全局对象。截图来自 Visual Studio Code，作者提供

这样，`builtins` 模块创建了一个新的对象，`SCREAM`。从现在起，你可以使用 `SCREAM`：

![](img/939afa11120e8c2a863e0df581f389ab.png)

内建函数破解：SCREAM 现在是一个真正的全局对象。截图来自 Visual Studio Code，作者提供

# 结论

在文章中，我向你展示了一个小技巧，使 Python 对象真正成为全局对象。你可能没见过这种方法，但这并不意味着它完全没有用。

一个例子是 `[tracemem](https://github.com/nyggus/tracemem/)` Python 包。在 [它的文档](https://github.com/nyggus/tracemem/#why-the-builtins-global-scope) 中，你将看到以下关于 `tracemem` 如何利用 `builtins` 全局作用域的解释：

> 由于 `*tracemem*` 这个功能用于调试来自不同模块的内存使用，因此在所有这些模块中导入所需的对象会很不方便。这就是为什么所需的对象被保留在全局作用域中的原因……

无论你是否觉得这个解释有说服力，我相信了解这个技巧是有价值的。它是那些可以显著扩展你 Python 知识的东西，通过帮助你掌握语言的复杂性。

然而，是否在你的编码实践中使用`builtins`全局变量是另一回事。我确信一点：在没有对 Python 中全局变量的操作有深入理解的情况下，你绝不应该决定使用它。

我称之为技巧，但永远不要忘记，所有在会话中可用的 Python 对象都是通过`builtins`全局变量提供的。然而，这并不意味着你应该对任何全局变量使用这种方法：任何全局变量并不等同于 Python 内置对象。

因此，你绝对不应该过度使用这个技巧。它不仅可能引入静态和动态错误，还可能使代码难以阅读和理解。然而，在少数情况下，你可能会觉得尽管有这些缺点，这正是你所需要的。

如果是这种情况，请暂停并重新考虑你的决定。然后，再考虑一次，甚至再考虑一次。与项目成员讨论。在经过这彻底的评估后，如果没有人提出异议，才考虑使用这种方法。

无论你是否在自己的项目中使用这个概念，理解`builtins`作用域和全局变量的工作原理都是至关重要的。这是因为`builtins`作用域建立了 Python 的基本作用域，没有理解这一点，你永远无法真正了解 Python 的工作机制。

感谢阅读。如果你喜欢这篇文章，你也可能会喜欢我写的其他文章；你可以在[这里](https://medium.com/@nyggus)看到它们。如果你想加入 Medium，请使用我下面的推荐链接：

[## 通过我的推荐链接加入 Medium - Marcin Kozak](https://medium.com/@nyggus/membership?source=post_page-----492f1e4faf9b--------------------------------)

### 作为 Medium 的会员，你的部分会员费用将用于你阅读的作者，你可以完全访问每一篇故事…

[medium.com](https://medium.com/@nyggus/membership?source=post_page-----492f1e4faf9b--------------------------------)
