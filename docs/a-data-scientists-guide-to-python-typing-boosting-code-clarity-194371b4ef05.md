# 数据科学家的 Python 类型指南：提升代码清晰度

> 原文：[`towardsdatascience.com/a-data-scientists-guide-to-python-typing-boosting-code-clarity-194371b4ef05`](https://towardsdatascience.com/a-data-scientists-guide-to-python-typing-boosting-code-clarity-194371b4ef05)

## 类型的重要性以及如何在 Python 中实现

[](https://medium.com/@egorhowell?source=post_page-----194371b4ef05--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----194371b4ef05--------------------------------)[](https://towardsdatascience.com/?source=post_page-----194371b4ef05--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----194371b4ef05--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----194371b4ef05--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----194371b4ef05--------------------------------) ·阅读时长 4 分钟·2023 年 7 月 31 日

--

![](img/76d1ae8f42e8d81de522593a525b0fd6.png)

照片由[Pankaj Patel](https://unsplash.com/@pankajpatel?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 什么是‘类型’？

这里的类型不是指物理上触碰键盘，而是我们 Python 代码中变量（和函数）所采用的数据类型！

Python 本质上是[**动态语言**](https://en.wikipedia.org/wiki/Dynamic_programming_language)，这意味着没有正式的要求来声明变量的数据类型。例如，一个变量可能在开始时是整数，但在代码的其他地方变成字符串。这种灵活性常常会导致在运行时出现难以调试的错误。

其他语言是[**静态类型**](https://www.techopedia.com/definition/22321/statically-typed#:~:text=Statically%20typed%20is%20a%20programming,with%20variables%2C%20not%20with%20values.)的，这意味着它们的变量类型需要明确声明，并且在运行时不能更改。如果一个变量被声明为整数，它在程序运行期间必须始终是整数。静态类型语言的例子有[**Fortran**](https://medium.com/towards-data-science/why-you-should-consider-using-fortran-as-a-data-scientist-5511e05ef89)和 C++。

然而，近年来 Python 已经开发了对类型的支持，现在它已成为行业标准。对于需要将稳健的机器学习模型投入生产的数据科学家尤为重要。

在这篇文章中，我想带你了解 Python 中的基本语法和类型过程，以及如何使用`mypy`包，这使我们能够无缝地检查代码的类型。

> 如[**PEP 484**](https://peps.python.org/pep-0484/)**所示，实际上推荐使用类型注解。**

# 基本示例

让我们通过一个简单的示例来解释在 Python 中进行类型检查的必要性。下面我们有一个将两个数字相加的函数，巧妙地命名为 `adding_two_numbers`：

作者的 GitHub Gist。

两个 `print` 语句的输出是什么？首先是：

```py
print(adding_two_numbers(5, 5))

>>> 10
```

这是预期中的情况。然而，第二个 `print` 语句的输出是：

```py
print(adding_two_numbers("5", "5"))

>>> 55
```

尽管这个结果在‘技术上’是正确的，但显然不是我们在这个特定函数中试图实现的目标。

为了帮助解决这个问题，我们可以为函数添加 *类型注解* 以明确我们需要传递的参数类型和预期的 *返回类型*：

作者的 GitHub Gist。

在上面的示例中，我们明确了 `num1` 和 `num2` *应该* 都是整数，预期的输出 *应该* 也是整数。

重要的是要提到，这些真的只是‘提示’，如果你传入一个字符串，在运行程序时仍然不会出现运行时错误，因为 Python 本质上是动态类型的。

所以，声明类型的通用语法是：

```py
function (variable: variable_type) -> return_type
```

此外，如果你不确定对象或变量的数据类型，你可以通过调用 `type()` 函数来检查：

```py
print(type(1))

>>> <class 'int'>
```

# 类型模块

如果你想让特定函数返回一个 `list`，但 `list` 中的每个元素都必须是整数怎么办？不幸的是，Python 的固有类型不能很容易做到这一点。这就是我们使用 `typing` 包的地方，可以通过运行 `pip install typing` 来安装。

我们可以使用 `typing` 包来更精细地声明我们的数据类型。以下是一些示例：

作者的 GitHub Gist。

> `typing` 包中还有许多其他类型可以满足你遇到的‘任何’变量（无意的双关语！）。[查看这个备忘单](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html)如果你有兴趣深入了解。

# 创建类型

你还可以通过简单地构造一个类来创建自己的类型。下面是我制作的 `dog` 类的示例：

作者的 GitHub Gist。

# MyPy 教程

`mypy` 是现在检查 Python 代码类型的行业标准包。它几乎在所有生产部署的代码中使用，特别是机器学习算法，因此作为数据科学家掌握它是非常值得的。

要开始使用 `mypy`，只需将其安装为 `pip install mypy`。然后要使用它，你只需运行 `mypy <file_name.py>`。这就是全部！

> 如果你想学习 mypy 的一些更高级的功能，请查看[这里](https://mypy.readthedocs.io/en/stable/)。

让我们通过一个示例来使这个概念更具体。如果我们回到之前的函数 `adding_two_numbers`，它看起来是这样的：

作者的 GitHub Gist。

然后，我们运行 `mypy adding_two_numbers.py`，输出结果如下：

```py
adding_two_numbers.py:6: error: Argument 1 to "adding_two_numbers" has incompatible type "str"; expected "int"  [arg-type]
adding_two_numbers.py:6: error: Argument 2 to "adding_two_numbers" has incompatible type "str"; expected "int"  [arg-type]
Found 2 errors in 1 file (checked 1 source file)
```

注意，错误仅出现在第 6 行，因为我们传入了字符串类型，但函数期望的是整数类型。错误信息中甚至指出了这一点。

对于第 5 行的 `print` 语句没有引发错误，因为我们传入了正确的类型，函数也返回了预期的整数类型。

# 总结：优缺点

让我们总结一下 Python 类型注解的一些主要优缺点：

## 优点

+   *有助于* [***linting***](https://en.wikipedia.org/wiki/Lint_(software)) *并减少代码中出现错误的可能性。*

+   *提高代码的可读性和文档化。*

## 缺点

+   *实施和编写类型的时间。*

+   *某些类型的向后兼容性并非所有 Python 版本都支持。*

## 总体而言

类型注解是大多数 Python 代码中的行业标准程序，包括数据科学工作。因此，这是一项重要且相对容易学习和实现的技能。这不仅会使你的代码更直观，而且有助于防止你的机器学习模型在生产环境中出现故障！

# 参考资料与进一步阅读

+   [*mypy 类型速查表*](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html)

+   [*Python 类型详细信息*](https://realpython.com/python-type-checking/)

# 另一个事项！

我有一个免费的新闻通讯，[**数据揭秘**](https://dishingthedata.substack.com/)，每周分享成为更好的数据科学家的技巧。没有“废话”或“诱饵”，只有来自实践数据科学家的纯粹可操作见解。

[](https://newsletter.egorhowell.com/?source=post_page-----194371b4ef05--------------------------------) [## 数据揭秘 | Egor Howell | Substack

### 如何成为更好的数据科学家。点击阅读《数据揭秘》，由 Egor Howell 编写的 Substack 出版物，内容包括…

[newsletter.egorhowell.com](https://newsletter.egorhowell.com/?source=post_page-----194371b4ef05--------------------------------)

# 与我联系！

+   [**YouTube**](https://www.youtube.com/@egorhowell?sub_confirmation=1)

+   [**领英**](https://www.linkedin.com/in/egor-howell-092a721b3/)

+   [**Twitter**](https://twitter.com/EgorHowell)

+   [**GitHub**](https://github.com/egorhowell)
