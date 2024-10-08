# mypy 介绍

> 原文：[`towardsdatascience.com/introduction-to-mypy-3d32fc96db75`](https://towardsdatascience.com/introduction-to-mypy-3d32fc96db75)

## Python 的静态类型检查

[](https://medium.com/@hrmnmichaels?source=post_page-----3d32fc96db75--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----3d32fc96db75--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3d32fc96db75--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d32fc96db75--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----3d32fc96db75--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d32fc96db75--------------------------------) ·阅读时间 8 分钟·2023 年 4 月 6 日

--

![](img/1256da4cc8ec98ef8ebafde56c0e759c.png)

图片由[Agence Olloweb](https://unsplash.com/@olloweb?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，在[Unsplash](https://unsplash.com/photos/d9ILr-dbEdg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上

我们在之前关于[Python 最佳实践](https://medium.com/towards-data-science/best-practices-for-python-development-bf74c2880f87)的文章中提到了 mypy 是必备的——在这里，我们希望更详细地介绍它。

[mypy](https://mypy.readthedocs.io/en/stable/)，正如文档所解释的，是一个“Python 的静态类型检查器”。这意味着，它为 Python 添加类型注解和检查，而 Python 是设计上动态类型的（类型在运行时推断，与例如 C++ 相对）。这样做可以在编译时发现代码中的错误，这是一种极大的帮助——并且是任何半专业 Python 项目的必备条件，正如我在之前的文章中所解释的那样。

在这篇文章中，我们将通过几个示例来介绍 mypy。免责声明：这篇文章不会介绍 mypy 的所有功能（甚至连一部分都不会）。相反，我会尝试在提供足够细节以让你编写几乎所有想要的代码和从零开始到扎实理解 mypy 之间找到一个良好的平衡。有关更多细节，我建议查阅官方文档或其他优秀教程。

# 安装

要安装 mypy，只需运行：`pip install mypy`

但是，我建议使用某种形式的依赖管理系统，例如[poetry](https://medium.com/towards-data-science/dependency-management-with-poetry-f1d598591161)。如何将它和 mypy 包含在更大的软件项目中，已在[这里](https://medium.com/towards-data-science/best-practices-for-python-development-bf74c2880f87)进行了说明。

# 初步步骤

让我们通过第一个示例来激发对 mypy 使用的兴趣。请考虑以下代码：

```py
def multiply(num_1, num_2):
    return num_1 * num_2

print(multiply(3, 10))
print(multiply("a", "b"))
```

`multiply`期望两个数字并返回它们的乘积。因此，`multiply(3, 10)`工作正常并返回预期结果。但是第二条语句失败并崩溃，因为我们不能将字符串相乘。由于 Python 是动态类型的，没有任何东西阻止我们编写/执行该语句，我们只在运行时发现了这个问题——这很有问题。

在这里，mypy 及时提供了帮助。我们现在可以注解函数的参数和返回类型，如下所示：

```py
def multiply(num_1: int, num_2: int) -> int:
    return num_1 * num_2

print(multiply(3, 10))
print(multiply("a", "b"))
```

这种注解不会以任何方式改变执行，特别是，你仍然可以运行这个有问题的程序。然而，在这样做和发布我们的程序之前，我们现在可以运行 mypy 并通过以下命令检查任何可能的错误：`mypy .`

运行此命令将失败，并正确指出我们不能将字符串传递给`multiply`。上述命令旨在从应用程序的主文件夹中执行，`。`将检查当前文件夹及子目录中的每个文件。但你也可以通过`mypy file_to_check.py`来检查特定文件。

这希望能激发对 mypy 的需求和使用的理解，现在让我们更深入探讨。

# 配置 mypy

mypy 可以通过多种方式进行配置——无需详细说明，它只需要找到一个包含“mypy”部分的配置文件（如 mypy.ini，[pyproject.toml](https://medium.com/towards-data-science/dependency-management-with-poetry-f1d598591161) 等）。在这里，我们将创建默认的`mypy.ini`文件，它应该位于项目的主文件夹中。

现在，让我们来看看可能的配置选项。为此，我们回到最初的示例：

```py
def multiply(num_1, num_2):
    return num_1 * num_2

print(multiply(3, 10))
print(multiply("a", "b"))
```

实际上，简单地运行 mypy 并不会产生错误！这是因为类型提示默认是可选的——而 mypy 只检查那些有注解的类型。我们可以通过标志`— disallow-untyped-defs`来禁用这一点。此外，还有许多其他标志可以使用（参见 [here](https://mypy.readthedocs.io/en/stable/command_line.html#miscellaneous-strictness-flags)）。然而，根据本帖的一般格式，我们不会详细讨论所有这些标志——而是仅呈现严格模式。此模式基本上开启所有可选检查。在我的经验中，使用 mypy 的最佳方法是要求尽可能严格的检查——然后修复（或选择性地忽略）所有出现的问题。

要做到这一点，我们可以像这样填写`mypy.ini`文件：

```py
[mypy]
strict = true
```

`[mypy]`部分头对于任何 mypy 相关的配置都是必要的，下一行是相当自解释的。

当我们像往常一样运行 mypy 时，我们会遇到关于缺失类型注解的错误——这些错误只有在所有内容都被注解并且我们移除错误的字符串调用后才会消失。

现在让我们更详细地看看如何使用 mypy 进行注解。

# 使用 mypy 注解

在本节中，我们将描述最常见的类型注解和 mypy 关键字。

# 基本类型

我们可以通过简单地使用其 Python 类型来注解基本类型，即`bool`、`int`、`float`、`str`等：

```py
def negate(value: bool) -> bool:
    return not value

def multiply(multiplicand: int, multiplier: int) -> int:
    return multiplicand * multiplier

def divide(dividend: float, divisor: float) -> float:
    return dividend / divisor

def concat(str_a: str, str_b: str) -> str:
    return str_a + " " + str_b

print(negate(True))
print(multiply(3, 10))
print(divide(10, 3))
print(concat("Hello", "world"))
```

# 集合类型

从 Python 3.9 开始，内置的集合类型也可以用作类型注解。即 `list`、`set`、`dict` 等：

```py
def add_numbers(numbers: list[int]) -> int:
    return sum(numbers)

def cardinality(numbers: set[int]) -> int:
    return len(numbers)

def concat_values(value_dict: dict[str, float]) -> list[float]:
    return [val for _, val in value_dict.items()]

print(add_numbers([1, 2, 3, 4]))
print(cardinality({1, 2, 3}))
print(concat_values({"a": 1.5, "b": 10}))
```

正如我们所见，我们必须指定容器的内容（例如`int`）。对于混合类型，请参见下文。

## 早期 Python 版本

对于早期的 Python 版本，必须使用 `typing` 模块中的旧版类型：

```py
from typing import Dict, List, Set

def add_numbers(numbers: List[int]) -> int:
    return sum(numbers)

def cardinality(numbers: Set[int]) -> int:
    return len(numbers)

def concat_values(value_dict: Dict[str, float]) -> list[float]:
    return [val for _, val in value_dict.items()]

print(add_numbers([1, 2, 3, 4]))
print(cardinality({1, 2, 3}))
print(concat_values({"a": 1.5, "b": 10}))
```

## 混合内容

如上所述，我们可能想要创建包含不同数据类型的容器。为此，我们可以使用`Union`关键字——它允许我们将类型注解为类型的联合：

```py
from typing import Union

def scan_list(elements: list[Union[str | int]]) -> None:
    for el in elements:
        if isinstance(el, str):
            print(f"I got a string! ({el})")
        elif isinstance(el, int):
            print(f"I got an int! ({el})")
        else:
            # NOTE: we don't reach here because of mypy!
            raise ValueError(f"Unexpected element type {el}")

scan_list([1, "hello", "world", 100])
```

类似于 Python 3.9 中的简化，Python 3.10（具体是[PEP 604](https://peps.python.org/pep-0604/)）引入了使用逻辑或操作符（`|`）的`Union`类型的缩写表示法：

```py
def scan_list(elements: list[str | int]) -> None:
    for el in elements:
        if isinstance(el, str):
            print(f"I got a string! ({el})")
        elif isinstance(el, int):
            print(f"I got an int! ({el})")
        else:
            # NOTE: we don't reach here because of mypy!
            raise ValueError(f"Unexpected element type {el}")

scan_list([1, "hello", "world", 100])
```

# 其他 mypy 关键字

在本节中，我们将介绍更多基本类型和关键字。

## 无

`None`，就像在“正常”Python 中一样，表示一个`None`值——最常用于注解没有返回类型的函数：

```py
def print_foo() -> None:
    print("Foo")

print_foo()
```

## 可选

我们经常会遇到这样一种情况：我们想根据是否传递了参数值来实现分支代码——通常，我们使用`None`来表示其缺失。为此，我们可以使用`typing.Optional[X]`——它正是表示这一点：它注解类型`X`，但也允许`None`：

```py
from typing import Optional

def square_number(x: Optional[int]) -> Optional[int]:
    return x**2 if x is not None else None

print(square_number(14))
```

继 Python 3.10 及以上版本引入的 PEP 604，`Optional` 可以再次缩短为 `X | None`：

```py
def square_number(x: int | None) -> int | None:
    return x**2 if x is not None else None

print(square_number(14))
```

请注意，这与[必需或可选参数](https://levelup.gitconnected.com/untangling-required-optional-positional-and-keyword-arguments-f7792320a40d)不对应，这一点经常被混淆！可选参数是指在调用函数时不必指定的参数——而 mypy 的 `Optional` 表示一个可以是某种类型，但也可以是 `None` 的参数。可能的混淆来源是可选参数的一个常见默认值是 `None`。

## 任意

`Any`，正如其名所示（我觉得我总是在重复这句话……）简单地允许任何类型——从而有效地关闭任何类型检查。因此，尽量避免在可能的情况下使用它。

```py
from typing import Any

def print_everything(val: Any) -> None:
    print(val)

print_everything(0)
print_everything(True)
print_everything("hello")
```

## 注解变量

到目前为止，我们仅使用 mypy 注解了函数参数和返回类型。自然地，这也可以扩展到任何类型的变量：

```py
int_var: int = 0
float_var: float = 1.5
str_var: str = "hello"
```

然而，这种用法相对较少（并且严格版的 mypy 也不强制执行），因为变量的类型通常可以从上下文中推断出来。通常情况下，只有在代码相对模糊且难以阅读时，你才会这么做。

# 注解复杂类型

在本节中，我们将讨论类的注解，以及使用你自己的和其他复杂类进行注解。

## 注解类

注解类可以很快处理：只需像注解其他函数一样注解类函数，但不要注解构造函数中的 self 参数：

```py
class SampleClass:
    def __init__(self, x: int) -> None:
        self.x = x

    def get_x(self) -> int:
        return self.x

sample_class = SampleClass(5)
print(sample_class.get_x())
```

## 使用自定义/复杂类进行注解

定义好类之后，我们现在可以像其他类型注解一样使用其名称：

```py
sample_class: SampleClass = SampleClass(5)
```

事实上，mypy 可以直接与大多数类和类型一起使用，例如：

```py
import pathlib

def read_file(path: pathlib.Path) -> str:
    with open(path, "r") as file:
        return file.read()

print(read_file(pathlib.Path("mypy.ini")))
```

# 修复 mypy 问题

在本节中，我们将看到如何处理不支持类型标注的外部库，并有选择地禁用对某些引发问题的代码行的类型检查——基于一个稍微复杂一点的示例，其中涉及到[numpy](https://numpy.org/)和[matplotlib](https://matplotlib.org/stable/index.html)。

让我们从代码的第一个版本开始：

```py
import matplotlib.pyplot as plt
import numpy as np
import numpy.typing as npt

def calc_np_sin(x: npt.NDArray[np.float32]) -> npt.NDArray[np.float32]:
    return np.sin(x)

x = np.linspace(0, 10, 100)
y = calc_np_sin(x)

plt.plot(x, y)
plt.savefig("plot.png")
```

我们定义了一个简单的函数来计算 numpy 数组的正弦，并将其应用于输入值`x`，这些值覆盖了区间[0, 10]。然后，我们使用`matplotlib`绘制正弦曲线。

在这段代码中，我们还看到使用`numpy.typing`进行 numpy 数组的正确类型标注。

然而，如果我们在此运行 mypy，会出现两个错误。第一个是：

> error: Returning Any from function declared to return “ndarray[Any, dtype[floating[_32Bit]]]”

这是 mypy 中一种相对常见的模式。我们实际上没有做错什么，但 mypy 希望它更为明确——在这里——以及在其他情况中——我们必须“强制”mypy 接受我们的代码。例如，我们可以通过引入一个正确类型的代理变量来做到这一点：

```py
def calc_np_sin(x: npt.NDArray[np.float32]) -> npt.NDArray[np.float32]:
    y: npt.NDArray[np.float32] = np.sin(x)
    return y
```

下一个错误是：

> error: Skipping analyzing “matplotlib”: found module but no type hints or library stubs

这是因为`matplotlib`尚未进行类型标注。因此，我们需要让 mypy 知道排除它的检查。我们通过在 mypy.ini 文件中添加以下内容来实现：

```py
[mypy-matplotlib.*]
ignore_missing_imports = True
ignore_errors = True
```

最后，请注意，你还可以通过在代码行末添加`# type: ignore`来有选择地忽略任何代码行。如果确实存在无法解决的 mypy 问题，或者你想要忽略一些已知但不相关的警告/错误，可以这样做。我们也可以通过这样做来隐藏上面的第一个错误：

```py
def calc_np_sin(x: npt.NDArray[np.float32]) -> npt.NDArray[np.float32]:
    return np.sin(x)  # type: ignore
```

# 结论

在这篇文章中，我们介绍了 mypy，它是一个用于 Python 的静态类型检查工具。使用 mypy，我们可以（并且应该）注释变量、参数和返回值的类型——为我们提供了一种在编译时对程序进行合理性检查的方法。mypy 被广泛使用，推荐用于任何中型以上的软件项目。

我们从安装和配置 mypy 开始。然后，我们介绍了如何注释基本和复杂类型，例如列表、字典或集合。接下来，我们讨论了其他重要的注释器，如`Union`、`Optional`、`None`或`Any`。最终，我们展示了 mypy 支持广泛的复杂类型，如自定义类。我们通过展示如何调试和修复 mypy 错误来结束教程。

这就是关于 mypy 的内容——希望你喜欢这篇文章，感谢阅读！
