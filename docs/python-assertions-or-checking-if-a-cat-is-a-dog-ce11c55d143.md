# Python 断言，或检查猫是否是狗

> 原文：[`towardsdatascience.com/python-assertions-or-checking-if-a-cat-is-a-dog-ce11c55d143`](https://towardsdatascience.com/python-assertions-or-checking-if-a-cat-is-a-dog-ce11c55d143)

## PYTHON 编程

## 了解在 Python 中使用断言的规则——以及不使用它们的规则

[](https://medium.com/@nyggus?source=post_page-----ce11c55d143--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----ce11c55d143--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ce11c55d143--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce11c55d143--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----ce11c55d143--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ce11c55d143--------------------------------) ·13 分钟阅读·2023 年 3 月 2 日

--

![](img/84e60e7f48e20b9ca7e0acd560d557f3.png)

错误的断言应该让你停止：有什么问题！图片由 [Jose Aragones](https://unsplash.com/@jodaarba?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

断言是你用来测试对程序的假设的语句。这一简短的定义，一方面很清晰。另一方面，它远未解释你何时应该使用断言。

`assert` 语句，作为 Python 中主要的断言工具，与内置的 `__debug__` 对象密切相关。在我学习 Python 的某个阶段，我对这个对象一无所知，因此我猜许多数据科学家和 Python 开发者也不清楚。在阅读本文后，你将了解如何使用 `__debug__` 和断言——以及如何 *不* 使用它们。

你会在测试中找到断言的主要位置。无论你使用哪个测试框架，它都会使用断言。虽然 `unittest` 使用特定类型的断言方法（如 `.AssertTrue()`, `.AssertFalse()`, `.AssertEqual()`），`pytest` 更喜欢裸露的 `assert` 语句。就个人而言，我喜欢后者的简洁。如果你想断言 `x` 是 10，可以用这种简单的方法：

```py
assert x == 10
```

当你想要断言 `x` 是整数时，可以这样做：

```py
assert isinstance(x, int)
```

对我来说，这很简单明确，而简单和明确是 Python 代码的重要美德。测试也不例外。

当条件不成立时，`assert` 语句会引发 `AssertionError`：

```py
>>> x = 20
>>> assert x == 10
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError
```

你还可以使用可选消息：

```py
>>> x = 20
>>> assert x == 10, "x is not 10"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError: x is not 10
```

现在，为什么不能在 `if` 块中做这个，而不使用 `assert` 呢？过程如下：

```py
>>> x = 20
>>> if x != 10:
...     raise AssertionError("x is not 10")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError: x is not 10
```

如果你希望你的程序在 `x` 不为 10 时抛出异常，你可以这样做。但这不应该是 `AssertionError`，因为 `AssertionError` 是在特定情况下使用的特定错误类型。然而，还有更多的内容。

正如 Mark Lutz 在他出色的书籍 *Learning Python*（第 5 版）中所写的那样，`assert` 语句只是一个快捷方式。在我们上面的例子中，我们可以用两种等效的方式编写断言（让我们使用可选消息）；第一种是你已经知道的：

```py
assert x == 10, "x is not 10"
```

这是一个更长代码段的快捷方式：

```py
if __debug__:
    if x != 10:
        raise AssertionError("x is not 10")
```

与之前的 `if` 块相比，它们确实不同——显而易见的问题是，`__debug__` 是什么？对这个问题的答案将帮助我们理解断言是什么以及何时使用它。

# 什么是 __debug__？

`__debug__` 对象是一个布尔变量，可以直接在你的 Python 会话中使用：

![](img/8f53b43591f27210ac6a9c13db88fac0.png)

一张来自 Python 3.11 调试模式会话的截图。图片来源：作者

注意，你不能在 Python 会话中更改它：

![](img/3e4eacc94ffb3d17fc64fd8c5f2f640b.png)

一张来自 Python 3.11 调试模式会话的截图。图片来源：作者

我将稍后展示如何将 `__debug__` 更改为 `False`。但首先，让我们看看 `__debug__` 与断言的关系：

![](img/19f730177dc6ce6d2e8b68ee9796fb0f.png)

一张来自 Python 3.11 调试模式会话的截图。图片来源：作者

正如你所见，当你在调试模式下运行 Python 时——这是默认模式——`__debug__` 为 `True`，断言按常规方式工作。

让我们在生产模式下打开 Python REPL，以查看断言在那里的工作方式。为此，我们需要提供一个 `-O` 标志：

![](img/3575c790377567d1f18923e1d567302c.png)

一张来自 Python 3.11 生产模式会话的截图。图片来源：作者

正如你所见，`__debug__` 现在为 `False`，这意味着我们在生产模式下工作。这意味着代码将被优化：

+   当 `__debug__` 为 `True` 时，所有断言以及跟随 `if __debug__:` 检查的其他内容（在这里我将称之为调试模式检查）将被执行。

+   当 `__debug__` 为 `False` 时，代码会被优化，以至于调试模式检查中的代码不会被执行。正如我们上面所看到的，这包括所有的断言，它们不会被运行。我们可以在上面的截图中看到这一点。

特别注意，当 `__debug__` 为 `False` 时，无论是 `assert True` 还是 `assert False` 都不会做任何事情。所以，特别地，`assert False` 没有引发 `AssertionError`，而在调试模式下它会。这完全是因为 `__debug__` 为 `False`，这意味着断言被关闭了。

## 如何使用 __debug__ 来优化代码执行

如上所述，生产模式下执行的代码是经过优化的。这意味着只有一件事：在调试模式下的检查代码将不会被执行。因此，你可以使用`__debug__`来添加仅在调试模式下执行的代码；在生产模式下，这段代码将被忽略。这样，你的生产代码将会更快——当然，前提是它包含调试模式检查，包括断言。

为了实现这一点，你可以手动添加代码到调试模式检查中：

```py
if __debug__:
    if x < 7:
        debug_logger.warning(f"x is below seven: {x = };"
                              " hence it's set to 7")
        x = 7
    elif x > 13:
        debug_logger.warning(f"x is over thirteen: {x = };"
                              " hence it's set to 13")
        x = 13
    else:
        debug_logger.info(f"x is fine: {x = }")
```

如果你在生产模式下运行代码，这个`if`块的内容将不会被执行，`debug_logger`也不会记录任何东西。假设你有很多这样的检查（例如，在一个长循环中）；忽略它们可以使代码更快。

我可以想象，这可能有点难以思考。我的建议是，下次你编写代码时，考虑是否有一些代码只希望在调试模式下执行，而在生产模式下不执行。有时，你可能找不到任何这样的东西；其他时候，你可能会发现这样的情况。

然而，你应该能够找到在某些地方使用断言会很有效。我们将在下面讨论这个问题。

总结一下，记住两件事：

+   当你使用很多断言和调试检查时，代码可能会显著变慢。

+   如果你想运行一些代码，不管模式如何，为什么还需要检查`__debug__`是否为真呢？当然，当使用`assert`语句时，实际上是会在底层进行检查的——但我们已经知道它是如何进行的。然而，请记住，如果你希望代码在调试模式和生产模式下都能运行，就不要在调试检查中添加代码。

# 何时使用断言

终于！现在我们知道了`__debug__`和调试模式，我们可以开始讨论断言。

如上所述，就代码而言，断言是在调试模式下执行的检查：当条件为真时，什么也不会发生，而当条件不满足时，会引发`AssertionError`。

这并没有完全解释你何时应该使用断言。简单地说，使用断言

+   在测试中（测试总是在调试模式下进行），以检查特定测试是否通过；

+   在开发模式下，以检查*绝不应该发生*的条件。

*至于测试*，一切都很清楚。正如我上面所写，`pytest`使用断言作为检查条件的主要工具。你通常在调试模式下运行单元测试。然而，尝试在生产模式下运行`pytest`，例如使用`python -O -m pytest`，你会看到以下警告：

```py
PytestConfigWarning: assertions not in test modules or plugins
will be ignored because assert statements are not executed by 
the underlying Python interpreter (are you using python -O?)

    self._warn_about_missing_assertion(mode)

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
```

正如你所见，当你想在生产模式下运行`pytest`时，它实际上是在我们可以称之为混合模式的模式下运行。测试函数中的所有`assert`语句都会被执行，但实际代码中的任何断言都不会被执行。这就是`PytestConfigWarning`告诉我们的。

*至于开发模式*，情况有所不同。正如你在上面所读到的，`assert` 语句帮助你检查一个*绝不可能发生*的条件是否为真。这乍一看可能有些奇怪。我为什么要检查一个不应该发生的条件？这不是像检查猫是否是狗一样吗？

正确！你明白了！在代码中使用断言就像检查猫是否是狗。当然，我们知道它*不是*——而断言 `assert cat is not dog` 并*不*真正旨在检查 `cat is not dog`，我们确实知道这是真的，而是检查代码是否正确。

换句话说，断言帮助你检查代码是否按预期工作。如果不是，它可能导致一些不可能的情况，例如猫是狗、整数是字符串、自然数是负数、样本大小大于总体大小等。因此，请记住断言是什么：你检查一些显而易见的东西——当它不是时，断言失败，你就知道代码或代码实现的逻辑有问题。

> 断言 `assert cat is not dog` 并*不*真正旨在检查 `cat is not dog`，我们确实知道这是真的，而是检查代码是否正确。

如果根据你的代码 `cat is dog`，这当然是不正确的，断言将失败并引发 `AssertionError`。这意味着代码不正确。

现在我们知道了何时使用断言。首先，你可以在测试中使用它们。其次，你可以通过添加必须为真的断言来确保代码的正确性。如果这样的断言失败了，代码就是不正确的——因为猫不能是狗。

> 如果这样的断言失败了，代码就是不正确的——因为猫不能是狗。

还有一点重要的事情要补充。*不要*过度使用断言。不要仅仅因为可以就把它们放到任何地方。只在重要的地方使用它们，那些地方有重要意义。用它们来捕捉重要的缺陷。

> *不要*过度使用断言。不要仅仅因为可以就把它们放到任何地方。

# 什么时候不使用断言

既然我们知道了什么时候使用断言，那么何时不使用它们也应该很明确。

首先，你不应该使用断言来处理常规异常。这些异常可能是错误的参数值、数据、错误的密码等。这类错误应该以常规方式处理。

> 你不应该使用断言来处理常规异常。

让我们来看一下。考虑以下函数：

```py
def preprocess_text(text: str) -> str:
    assert isinstance(text, str)
    return text.lower().strip()
```

这个函数旨在以特定方式预处理一个字符串。在我们的例子中，预处理非常简单，`text.lower().strip()`，但这只是函数可能执行的一种示例。该函数还检查提供的参数 `text` 的值是否具有正确的类型，即 `str`；如果不是，它会引发异常，对吧？

错误！为了检查类型，函数使用了`assert`语句，而我们已经知道这不正确。首先，请注意，如果你提供了不同类型的对象，将引发`AssertionError`，而且它不会说明应该是什么——即类型不正确。Python 对此有`TypeError`。

其次，请注意在生产模式下，这个检查不会被执行。这真的是你希望这个函数的行为吗？我更倾向于说，如果你需要检查`text`的类型，那么你应该在两种模式下都进行检查。在这里，你可能会在调试模式和生产模式中得到不同的行为。我猜很多在这种情况下使用`assert`的开发者可能不知道这一点。

我们知道哪里出了问题。这个函数不应该使用`assert`。相反，它可以使用`if`语句结合`raise`语句，或者使用专门的工具，如`easycheck`包：

[## GitHub - nyggus/easycheck：一个提供简单且可读断言的 Python 函数的模块](https://github.com/nyggus/easycheck?source=post_page-----ce11c55d143--------------------------------)

### 一个提供用于代码内部和测试中的简单、可读断言的 Python 函数的模块。…

[github.com](https://github.com/nyggus/easycheck?source=post_page-----ce11c55d143--------------------------------)

我计划写一篇关于`easycheck`的更长文章，但你已经可以在这里阅读它的特定用例：

[## 使用 easycheck 比较浮点数](https://towardsdatascience.com/comparing-floating-point-numbers-with-easycheck-dcbae480f75f?source=post_page-----ce11c55d143--------------------------------)

### easycheck 可以帮助你在类似断言的情况下比较浮点数

towardsdatascience.com

在我们上面的函数中，我们可以添加一个`easycheck`检查，如下所示：

```py
import easycheck

def preprocess_text(text: str) -> str:
    easycheck.check_type(
        text, 
        expected_type=str,
        handle_with=TypeError,
        message="Argument text must be string, "
                f"not {type(text).__name__}"
    )
    return text.lower().strip()
```

上述检查可以用以下方式理解——在我看来是自然的：检查`text`的类型；它应该是`str`；如果不是`str`，则引发`TypeError`，并附上以下消息：`f"Argument text must be string, not {type(text).__name__}"`。

所以，当你提供一个整数时，你会看到以下内容：

```py
>>> preprocess_text(108)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: Argument text must be string, not int
```

让我们总结一下这个例子：

**问题：** 我是否应该在`preprocess_text()`中使用断言来检查`text`是否是字符串？

**答案：** 不

**问题：** 为什么不呢？

**答案：** 你不应该在这里使用断言，因为不同类型的`text`不是一种不可能发生的情况。当用户提供了错误类型的`text`参数时，这种情况确实*可能*发生。

**问题：** 那么我应该使用什么呢？

**答案：** 你可以使用`if`检查`text`的类型，并在类型不正确时引发`TypeError`。或者你可以使用`easycheck`，这是一个专门针对这种情况的工具。

# 生产模式、测试和断言

现在，一个重要的问题是你是否应该使用通过`-O`标志调用的生产模式。

在他的[*Python 中的清晰代码：开发可维护和高效的代码*](https://www.oreilly.com/library/view/clean-code-in/9781788835831/)一书中，Mariano Anaya 说你不应该这么做。你不应该，因为断言有助于捕捉错误，那么为什么仅仅因为你在生产中运行代码就放弃这个机会呢？当断言失败时，代码中出现了严重的问题 —— 无论如何代码都会崩溃，但可能会晚一些。最好尽早引发异常。

我完全同意上述方法，但……

有时使用生产模式更好。这是在执行时间至关重要且代码中的断言显著减慢了速度的情况下。假设代码经过了充分测试，你可能会选择关闭所有断言，以使应用程序运行得更快 —— 尤其是当你使用了很多断言时。

在一些项目中，决定是否使用生产模式很简单。当执行时间不重要时，在调试模式下运行你的生产代码。这就像在生产中测试生产代码 —— 没有比这更好的测试了。在其他项目中，决定也很简单 —— 当执行时间重要时，我的意思是*它确实很重要*，使用生产模式。在这种情况下，代码应该通过单元测试和集成测试进行良好的覆盖，并且*所有*测试应在每个新版本部署之前运行。¹ 尽管如此，如果你想使用生产模式，我认为你应该在调试模式和生产模式下运行测试。后者 —— 实际上我上面称之为混合模式 —— 可以帮助你捕捉在调试模式下无法捕捉到的错误。由于我从未找到关于这个主题的任何字样，我计划写一篇专门的文章，详细解释和举例；当它准备好并发布时，我会在这里链接。

在一些项目中，决策将不会那么简单。你必须根据项目的假设、代码和测试的质量以及测试覆盖率来决定是否运行断言或关闭它们。

# 结论

我写这篇文章是因为我注意到许多 Python 开发者不了解断言是什么。我对此感到同情。我也经历过。在我的 Python 学习旅程的某个时刻，我也不理解断言。

我希望这篇文章对断言和`__debug__`进行了充分的解释。让我们总结一下我们讨论的内容：

+   断言仅在调试模式下执行，这也是默认的 Python 模式。通过使用`-O`标志来在生产模式下运行 Python，即`python -O`。

+   使用断言来检查*必须为真的*条件。如果它们失败，说明代码中有问题。

+   不要使用断言来检查其他事情，比如与参数值相关的条件。这些检查应该使用 `if` 检查结合 `raise`，或使用像 `easycheck` 这样的专用工具进行。

+   使用 `__debug__` 添加在调试模式下执行的代码，而在生产模式下不执行。

+   当你在生产模式下运行你的生产代码时，你也应该在生产模式下运行你的单元测试和集成测试。

总的来说，总是根据具体情况决定是否使用生产模式。

如果你想了解更多关于 `AssertionError` 的信息，下面的文章展示了一个小技巧，即如何用不同类型的异常覆盖它。例如，你可能希望在单元测试中使用自定义的项目异常而不是内置的 `AssertionError`。我认为这不是你在生产代码中实际使用的东西，但这可以帮助你理解异常处理和断言——以及 Python 本身。

[](https://betterprogramming.pub/how-to-overwrite-asserterror-in-python-and-use-custom-exceptions-c0b252989977?source=post_page-----ce11c55d143--------------------------------) [## 如何在 Python 中覆盖 AssertionError 并使用自定义异常

### Python 的 assert 语句使用 AssertionError。了解如何使用其他异常代替

betterprogramming.pub](https://betterprogramming.pub/how-to-overwrite-asserterror-in-python-and-use-custom-exceptions-c0b252989977?source=post_page-----ce11c55d143--------------------------------)

如果你对如何格式化长断言感兴趣，你可能会发现以下文章很有趣：

[](https://medium.com/pythoniq/dont-surround-python-assertions-with-parentheses-f9b28729609a?source=post_page-----ce11c55d143--------------------------------) [## 不要用括号围绕 Python 断言

### 了解为什么在使用消息时不应将 assert 语句括起来。

medium.com](https://medium.com/pythoniq/dont-surround-python-assertions-with-parentheses-f9b28729609a?source=post_page-----ce11c55d143--------------------------------)

你会在文中看到，当断言太长无法放在一行时该怎么办——以及为什么*永远*不要用括号围绕它的条件和消息。

# 脚注

¹ 嗯，你*应该总是*在部署新版本之前运行测试。然而，有时你可能决定只运行重新部署的模块的测试，但这取决于应用程序的架构。无论如何，在部署之前运行所有测试总是更安全。我们编写测试是为了运行它们，不是吗？

感谢阅读。如果你喜欢这篇文章，你可能还会喜欢我写的其他文章，你可以在[这里](https://medium.com/@nyggus)查看。如果你想加入 Medium，请使用下面我的推荐链接：

[](https://medium.com/@nyggus/membership?source=post_page-----ce11c55d143--------------------------------) [## 通过我的推荐链接加入 Medium - Marcin Kozak

### 阅读 Marcin Kozak 的每一个故事（以及 Medium 上成千上万的其他作家的故事）。您的会员费用将直接支持…

medium.com](https://medium.com/@nyggus/membership?source=post_page-----ce11c55d143--------------------------------)
