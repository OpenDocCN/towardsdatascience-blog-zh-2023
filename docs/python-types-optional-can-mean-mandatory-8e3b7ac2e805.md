# Python 类型：可选的可以是强制的

> 原文：[`towardsdatascience.com/python-types-optional-can-mean-mandatory-8e3b7ac2e805`](https://towardsdatascience.com/python-types-optional-can-mean-mandatory-8e3b7ac2e805)

## PYTHON 编程

## 了解如何避免对 `typing.Optional` 的常见误用和误解。

[](https://medium.com/@nyggus?source=post_page-----8e3b7ac2e805--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----8e3b7ac2e805--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e3b7ac2e805--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e3b7ac2e805--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----8e3b7ac2e805--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e3b7ac2e805--------------------------------) ·8 分钟阅读·2023 年 11 月 21 日

--

![](img/7d449c5d156e16fcbe145e74f261329c.png)

照片由[Caroline Hall](https://unsplash.com/@notcarolyn?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

根据[Python 文档](https://docs.python.org/3/library/typing.html#typing.Optional)，`typing.Optional` 是一种方便的方式来表示一个对象可以是 `None`。这是一种简洁而优雅的方式来表达这个概念，但它是否也非常清晰？

让我换一种说法：当你在 Python 环境中看到“optional”这个词时，你认为它意味着什么？假设你看到一个名为 `x` 的参数，它的类型是 `Optional[int]`。`int` 部分相当明确，因为它很可能表示一个整数，但 `Optional` 代表什么呢？你的第一反应是什么？

我们来考虑以下两个选项：

1.  我不需要提供 `x` 的值，因为它是可选的。

1.  `x` 的值可以是 `int` 或 `None`。

如果你对 Python 类型提示足够了解，你会知道选项 2 是正确的。但当你不了解时……也许我错了，但我无法想象任何一个不懂 Python 的人会选择选项 2。选项 1 似乎更有意义。当我看到信息说某物是可选的，我会认为……嗯，就是说它是可选的……

这个问题导致了 `typing.Optional` 类型的常见误用。本文旨在揭示这种误用，并引导你正确理解这个类型。

# `typing.Optional` 的含义

这三种类型提示是等效的：

```py
from typing import Optional, Union

x: Union[str, None]
x: Optional[str]
x: str | None
```

它们都传达了相同的信息：`x` 可以是字符串或 `None`。虽然完全有效，但第一个 (`Union[str, None]`) 代表了 Python 类型提示的早期阶段：这是最初的方法，但现在不一定是首选方法。随后，`Optional` 被添加到 `typing` 模块中，提供了一种更简洁和直接的方式来表达这一概念。根据 [the](https://mypy.readthedocs.io/en/stable/kinds_of_types.html?highlight=union#optional-types-and-the-none-type) `[mypy](https://mypy.readthedocs.io/en/stable/kinds_of_types.html?highlight=union#optional-types-and-the-none-type)` [documentation](https://mypy.readthedocs.io/en/stable/kinds_of_types.html?highlight=union#optional-types-and-the-none-type)：

> 你可以使用 `[Optional](https://docs.python.org/3/library/typing.html#typing.Optional)` 类型修饰符来定义允许 `None` 的类型变体，例如 `Optional[int]`（`Optional[X]` 是 `Union[X, None]` 的首选简写）。

最终，在 Python 3.10 中，引入了 `|` 运算符用于类型提示。正如 [mypy 文档](https://mypy.readthedocs.io/en/stable/kinds_of_types.html?highlight=union#x-y-syntax-for-unions) 所述，

> [**PEP 604**](https://peps.python.org/pep-0604/) 引入了一种拼写联合类型的替代方式。在 Python 3.10 及更高版本中，你可以将 `Union[int, str]` 写作 `int | str`。

如你所见，这是一种用于联合类型的一般运算符，并非专门设计用于表示变量可以为 `None`。

尽管这三种版本都是有效的，但选择应取决于几个因素。首先，如果你使用的 Python 版本低于 3.10，则 `|` 运算符不可用。即使使用 `__future__` 导入：

```py
from __future__ import annotations
```

在某些情况下，它仍然可能会失败。你可以在 [the](https://mypy.readthedocs.io/en/stable/runtime_troubles.html#using-x-y-syntax-for-unions) `[mypy](https://mypy.readthedocs.io/en/stable/runtime_troubles.html#using-x-y-syntax-for-unions)` [documentation](https://mypy.readthedocs.io/en/stable/runtime_troubles.html#using-x-y-syntax-for-unions) 中阅读相关内容。

尽管如此，我建议不要使用 `Union` 类型来表示可能为 `None`，因为这不必要地冗长，而且正如上面引用的文档所述，这已经不是首选选项了。`Mypy` 推荐使用 `typing.Optional` ([quote](https://mypy.readthedocs.io/en/stable/kinds_of_types.html?highlight=union#optional-types-and-the-none-type)：“`Optional[X]` 是 `Union[X, None]` 的首选简写”)。我同样支持这一点，原因很简单：`Optional` 类型正是为这种用例创建的，并且它还适用于旧版本的 Python，区别于 `|` 运算符。

以下是几个正确的类型提示示例，这些示例使用了 `Optional`：

![](img/94c7b45c6320ca5a82b43df116d1e4cc.png)

在 Python 3.12 中使用 typing.Optional 的类型提示示例。来自 Visual Studio Code 的截图，作者提供

如你所见，`Optional`可以用于简单和复杂的类型提示。我们只分析其中间的一个。`dict[str, Optional[int]]`类型表示一个变量应该是一个字典，键是字符串，值可以是整数或`None`。

但是，这篇文章并不是关于做出这种选择的。我想讨论`typing.Optional`类型的一个常见误用——并展示如何避免它。在此过程中，我将解释我认为这种误用的来源、如何纠正它，以及如何理解`typing.Optional`类型。

# 对`Optional`的误解

请考虑以下函数签名：

```py
from typing import Optional

def foo(s: str, n: Optional[int] = 1) -> list[str]:
    ...
```

让我们分析一下这个函数签名中的类型提示。但是，不要过于依赖这个分析！因为这些类型提示可能（虽然不一定）是错误的。这里是：

+   `s`是一个字符串（`str`）参数；它可以是位置参数或关键字参数，并且是必需的；

+   `n`是一个可选的整数（`int`）参数，可以是位置参数或关键字参数，默认值是`1`；

+   `foo()`函数返回一个字符串列表（`list[str]`）。

现在有一个问题：*上述分析有什么问题？*

第二个要点是错误的。它说`n`是一个可选的整数。从某种程度上说，这是一句完全有效的英语句子。`n`参数确实是可选的，因为你不必提供它的值；当你省略它时，将使用默认值`1`。

另一方面，这*不一定*是一个有效的`typing`句子。我的意思是，这种说法对`typing.Optional`的理解是不正确的。上面，我们用`Optional[int]`来表示以下含义：你不必提供`n`的值。这**意义是不正确的**。`typing`语法中`Optional[int]`的正确含义是：`n`可以是`int`或`None`。

下图总结了这两种理解：

![](img/e4893d88c8d4ac21daf1a0191be28121.png)

正确和不正确理解`typing.Optional`。图像由作者提供

让我们改进函数签名。我们有四个选项可以选择，每个选项代表不同的情况。选择适合你特定场景的选项。

如你所见，将有选项 0，其中签名保持不变。是的，这种类型提示可以是正确的——但它的含义很少是你需要的。

*选项 0：保持原样*

```py
from typing import Optional

def foo(s: str, n: Optional[int] = 1) -> list[str]:
    ...
```

类型提示`n: Optional[int] = 1`是完全正确的。重点是，它的含义与许多人认为的不同，因为它表示

+   `n`可以是`int`或`None`，并且

+   `n`的默认值是 1。

所以，默认值是`1`，但用户仍然可以提供`None`。

虽然技术上是正确的，但我只会在非常特定且罕见的情况下使用这种类型提示，因为这些情况非常少见，我从未遇到过需要这种类型提示的情况。这对我来说听起来不自然。

我对这个选项非常苛刻，因为在我看来，正是这种类型提示使得`typing.Optional`被频繁误用：它暗示`n`是可选的，因为它有一个默认值，因此你根本不必为这个变量或参数提供值。

我包括这个选项是因为它在技术上是正确的——但实际上你几乎不应该考虑它。至少，记住许多经验较少的 Python 用户很可能会误解这种类型提示。

*选项 1：使用* `Optional` *并且* `None` *作为默认值*

```py
from typing import Optional

def foo(s: str, n: Optional[int] = None) -> list[str]:
    if n is None:
        ...
    ...
```

当你需要`None`作为整数或其他任何类型的默认值时，这是一种最常见的情况。这里使用了默认值（`None`），作为触发特定操作的某种情感标志。因此，如果用户提供了一个整数，则会进行一些基于整数的处理。但当`n`是`None`时，这种处理可以完全关闭。当然，这只是一个示例场景，但它非常常见。

注意`if`块，它旨在进行显式的`None`检查。也许在所有这种情况下都不需要，但根据[the](https://mypy.readthedocs.io/en/stable/kinds_of_types.html?highlight=union#optional-types-and-the-none-type) `[mypy](https://mypy.readthedocs.io/en/stable/kinds_of_types.html?highlight=union#optional-types-and-the-none-type)` [documentation](https://mypy.readthedocs.io/en/stable/kinds_of_types.html?highlight=union#optional-types-and-the-none-type)：

> 对于未加保护的`None`或`[Optional](https://docs.python.org/3/library/typing.html#typing.Optional)`值，大多数操作是不允许的[…] 相反，需要进行显式的`None`检查。Mypy 具有强大的类型推断功能，可以使用常规 Python 习惯来防范`None`值。

*选项 2：不要使用* `Optional`

```py
def foo(s: str, n: int = 1) -> list[str]:
    ...
```

在这个选项中，你真正需要的是`n`的默认值，但`n`不能是`None`。在这里，你完全不需要提供`n`的值，因为有了默认值，你在调用`foo()`时不必提供它的值。因此，这就是英语中*optional*的正确含义（参数是可选的，因为你不必提供它的值），但在`typing`语法中是不正确的（参数*不是*`Optional`，因为`n`不能是`None`）。

*选项 3\. 使用* `Optional` *但要求其值*

这个用例展示了为什么使用`typing.Optional`并不会使参数*optional*。如上所述，`typing.Optional`类型意味着一个变量可以是`None`，但这并不意味着当它用于参数时，你不必提供它的值。因此，这段代码是完全有效的：

```py
def foo(s: str, n: Optional[int]) -> list[str]:
    if n is None:
        ...
    ...
```

尽管`n`是`Optional`，但它*不是*可选的——你必须提供它的值。因此，你必须提供`n`，但它仍然可以是`None`。在这里，可选意味着`n`是可选的`int`，因为它也可以是`None`。

如同选项 1 一样，你通常应该对`n`使用显式的`None`检查，因为我所写的关于使用`None`的内容在这里也适用。

![](img/dee47dd5961b50e442c3a1f2c0f8e114.png)

对于使用 typing.Optional 处理必需参数的肯定。图片作者

# 结论

我们讨论了一个与`typing.Optional`类型相关的常见错误。这个错误源于这样一个事实：尽管名称暗示`typing.Optional`处理可选参数，但它实际上指的是一个变量是否可以是`None`——与是否必须在函数调用中提供参数值无关。

在我看来，“optional”一词并不能准确传达`typing.Optional`的含义。然而，这是一个已经存在一段时间的公认 Python 术语，因此我不预期会有任何变化——无论如何。因此，意识到这种潜在的误解很重要。希望随着时间的推移，Python 代码库会减少对`typing.Optional`的误用和误解。

感谢阅读。如果你喜欢这篇文章，你可能也会喜欢我写的其他文章；你可以在[这里](https://medium.com/@nyggus)看到它们。如果你想加入 Medium，请使用下面的推荐链接：

[## 通过我的推荐链接加入 Medium - Marcin Kozak](https://medium.com/@nyggus/membership?source=post_page-----8e3b7ac2e805--------------------------------)

### 作为 Medium 会员，你的会员费用的一部分将会给你阅读的作者，而你可以全面访问每一个故事……

[medium.com](https://medium.com/@nyggus/membership?source=post_page-----8e3b7ac2e805--------------------------------)
