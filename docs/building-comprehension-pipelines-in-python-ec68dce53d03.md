# 在 Python 中构建理解管道

> 原文：[`towardsdatascience.com/building-comprehension-pipelines-in-python-ec68dce53d03`](https://towardsdatascience.com/building-comprehension-pipelines-in-python-ec68dce53d03)

## PYTHON 编程

## 理解管道是构建管道的一种 Python 特有的理念

[](https://medium.com/@nyggus?source=post_page-----ec68dce53d03--------------------------------)![马尔钦·科扎克](https://medium.com/@nyggus?source=post_page-----ec68dce53d03--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec68dce53d03--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec68dce53d03--------------------------------) [马尔钦·科扎克](https://medium.com/@nyggus?source=post_page-----ec68dce53d03--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec68dce53d03--------------------------------) ·阅读时间 12 分钟·2023 年 2 月 17 日

--

![](img/686b7a1a7f83a53912af9f9378a8391c.png)

理解管道能直接带你到达目标。图片来源于[安妮卡·胡辛加](https://unsplash.com/@iam_anih?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

生成器管道提供了一种 Pythonic 的方式来创建软件管道，即操作链，其中每个操作（除了第一个）都将前一个操作的输出作为其输入：

[](/building-generator-pipelines-in-python-8931535792ff?source=post_page-----ec68dce53d03--------------------------------) ## 在 Python 中构建生成器管道

### 这篇文章提出了一种优雅的方式来构建生成器管道

towardsdatascience.com

它们使你能够应用转换编程，如托马斯和亨特在他们伟大的书籍*程序员的修炼*中所描述的那样

[](https://en.wikipedia.org/wiki/The_Pragmatic_Programmer?source=post_page-----ec68dce53d03--------------------------------) [## 程序员的修炼 - 维基百科

### 来自维基百科，百科全书《程序员的修炼：从学徒到大师》是一本关于计算机的书…

en.wikipedia.org](https://en.wikipedia.org/wiki/The_Pragmatic_Programmer?source=post_page-----ec68dce53d03--------------------------------)

Python 中的典型生成器管道在管道的每一步都使用生成器。换句话说，管道的每一步都构建为一个生成器。Thomas 和 Hunt 讨论了通过管道操作符实现的管道，这在许多编程语言中都可以使用。虽然 Python 没有内置的管道操作符，但由于某些操作符可以在类定义中重载，因此可以很容易地创建它。我们可以在[这个](https://github.com/JulienPalard/Pipe) `[Pipe](https://github.com/JulienPalard/Pipe)` [Python 包](https://github.com/JulienPalard/Pipe)中看到这一点，该包使用了`|`操作符。

在上述文章中，我展示了使用生成器来创建管道的每一步可能引入的视觉混乱，从而降低代码的可读性。此外，当管道的每次运行都很快时，这种方法的性能较差。因此，我提出了一种替代的、比经典生成器管道更具可读性的高效构建生成器管道的方法。该方法将函数组合与生成器结合使用。未来，我将向您展示如何使用管道操作符创建管道。

尽管如此，正如我在本文中所写的那样，生成器表达式是推导的一个特例。

[](/a-guide-to-python-comprehensions-4d16af68c97e?source=post_page-----ec68dce53d03--------------------------------) ## Python 推导指南

### 学习列表推导（listcomps）、集合推导（setcomps）、字典推导等的复杂性。

[towardsdatascience.com

那么，为什么我们要把自己局限于生成器管道呢？为什么不考虑列表推导管道、字典推导管道或集合推导管道呢？

这个问题困扰了我一段时间，但我把它当作烦人的苍蝇在头顶嗡嗡作响，无法让我安宁，不停地嗡嗡嗡……最终，我放弃了拒绝这个想法，并决定至少提出讨论。大约两个月前，在一次愉快的冬季散步中，我带着我的两只狗走在森林里。一个美丽的冬天，雪、霜、强风——我和狗狗们在树林里，走在我最喜欢的小路上，思考（好吧，是我，不是狗）如何使用其他推导来构建管道，而不仅仅是生成器表达式。

本文是这次探讨的结果。我提出了一种将生成器管道推广到我称之为*推导管道*的概念，其中生成器管道只是一个特例。

# 生成器管道示例

为了保持一致性和清晰性，我将使用在上一篇文章中用到的相同生成器管道示例。请注意，我稍微修改了一些类型注解。生成器管道如下¹：

```py
import math

from typing import Generator

# Type aliases
Number = int | float
PowerType = int | float
PipelineItems = Iterable[Number]

def double(x: Number) -> Number:
    return x**2

def power(x: Number, n: PowerType) -> Number:
    return x**n

def add(x: Number, y: Number) -> Number:
    return x + y

def calculate(x: Number) -> Number:
    x = power(x, 0.5)
    x = double(x)
    x = add(x, 12)
    x = power(x, 2)
    x = add(x, math.pi**0.5)
    x = round(x, 2)
    x = add(x, 75)
    return x

def get_generator_pipeline(
    items: PipelineItems,
) -> Generator[Number, None, None]:
    """Create generator pipeline applying calculate() to each item."""
    return (calculate(x_i) for x_i in items)
```

`get_generator_pipeline()`函数——之前称为`get_pipeline()`——返回一个生成器管道；也就是说，一个生成器，它懒惰地（按需）计算`items`可迭代对象的后续元素的管道。我们可以用任何我们想要的方式评估生成器，例如，使用`list()`：

```py
>>> items = [1.12, 2.05, 1.122, -0.220002, 7.0036]
>>> pipeline = get_generator_pipeline(items)
>>> list(pipeline)
```

如果我们想得到一个列表，那么使用生成器表达式就没有意义——对应的列表推导式会更好！那么，为什么一开始就不使用列表推导管道而使用生成器管道呢？

# 什么时候使用生成器管道？什么时候不使用？

生成器管道有一个重要的优势，这与生成器的主要优势相同：延迟评估。当`items`可迭代对象很大，或者管道的步骤生成大型对象时，生成器管道将帮助我们避免内存不足的问题。

但是，如果`items`可迭代对象很短且输出可迭代对象占用的内存不多，那我们为何要担心内存问题呢？更何况，我们知道生成器表达式可能比对应的列表推导式要慢——唯一的例外是，当项的数量（或大小）大到无法在内存中全部保留和处理时。

考虑以下这个与上面示例相当不同的例子：

+   我们有`paths`，一个包含文件路径的列表。

+   在管道的每一步中，从一个路径读取一个文本文件，并对文本进行处理。

+   因此，返回的是处理后的文本的可迭代对象。

在这种情况下，输出可迭代对象将远大于输入可迭代对象。因此，当路径数量庞大时，生成器管道在这里效果最好——因为返回一个长文本的长列表将会非常低效，甚至可能无法实现。我们需要管道返回一个生成器，因此需要生成器管道。

现在，想象另一个管道。我们有相同的路径可迭代对象，但我们对文本的处理不同了。之前，我们返回长文本。现在，我们只需要每个文本中是否包含“Python”这个词的信息；因此，对于每个文本，我们只需要一个布尔值。对于路径列表，我们将得到一个长度相同的布尔值列表。生成器管道在这里的优势是什么？没有。

更重要的是，仅返回布尔值几乎没有意义：很难将特定的值与相应的文本联系起来。因此，最好返回一个以路径为键，布尔值为值的字典。这将使管道基于字典。或者，我们可以返回一个包含文本中“Python”单词的路径列表；然而，这种输出会忽略其他路径，从而丢失部分信息——有时我们可能需要这些信息。

# 建立理解管道

我们终于来到了本文的主题：理解管道以及如何构建它们。上述示例展示了我们如何决定是否需要生成器管道或其他类型的管道。除了生成器管道，我们还可以构建

+   列表理解管道，*即* listcomp 管道

+   集合理解管道，*即* setcomp 管道

+   字典理解管道，*即* dictcomp 管道

接下来，我将使用这三种其他类型的理解管道重写使用`calculate()`函数的生成器管道。为了完整性，我将重复生成器管道的代码。我还会改变`typing`的导入，因为这次我们需要比之前更多的类型，之前我们只创建了一个生成器管道。

```py
from typing import Dict, Generator, Iterable, List, Set

def get_generator_pipeline(
    items: PipelineItems,
) -> Generator[Number, None, None]:
    """Create generator pipeline applying calculate() to each item."""
    return (calculate(x_i) for x_i in items)

def get_listcomp_pipeline(items: PipelineItems) -> List[Number]:
    """Create listcomp pipeline applying calculate() to each item."""
    return [calculate(x_i) for x_i in items]

def get_setcomp_pipeline(items: PipelineItems) -> Set[Number]:
    """Create setcomp pipeline applying calculate() to each item."""
    return {calculate(x_i) for x_i in items}

def get_dictcomp_pipeline(items: PipelineItems) -> Dict[Number, Number]:
    """Create dictcomp pipeline using calculate() for items.

    Items are dict keys with calculate(item) being
    the corresponding value.
    """
    return {x_i: calculate(x_i) for x_i in items}

def get_dictcomp_pipeline_str(items: PipelineItems) -> Dict[str, Number]:
    """Create dictcomp pipeline using calculate() for items.

    str(item) are dict keys with calculate(item) being
    the corresponding value.
    """
    return {str(x_i): calculate(x_i) for x_i in items}
```

我使用了特定版本的字典管道，但请注意，我们也可以构建其他版本，具体取决于我们希望将什么用作字典的键。稍后我们将看到一个示例。

接下来，我们将总结这些基本类型的管道，包括生成器管道。请记住，它们在*输出*上有所不同，因为它们都可以接受相同类型的输入——任何可迭代对象都可以。

## 生成器管道

+   以任何可迭代对象（`items`）作为输入。

+   返回一个生成器作为管道。

+   可以使用任何形式和复杂度的生成器表达式（例如，它可以包含多个级别的`if`过滤器和`for`循环）。

+   可以按需求值。

## 列表理解管道，也称为 listcomp 管道

+   以任何可迭代对象（`items`）作为输入。

+   运行列表理解作为管道，因此返回一个列表。

+   可以使用任何复杂度的列表理解。

+   是贪婪求值的。

## 集合理解管道，也称为 setcomp 管道

+   以任何可迭代对象（`items`）作为输入。

+   运行集合理解作为管道，因此返回一个集合。

+   可以使用任何复杂度的集合理解。

+   由于最终输出是一个集合，它将包含唯一的结果。因此，如果可迭代对象中的两个或更多项返回相同的输出，它们的重复实例将被跳过。

+   是贪婪求值的。

## 字典理解管道，也称为 dictcomp 管道

+   以任何可迭代对象（`items`）作为输入。

+   运行字典理解作为管道，因此返回一个字典。

+   可以使用任何复杂度的字典理解。

+   对于特定的`item`，返回一个键值对；键不必是`item`——它可以是从`item`或其任何处理结果中得到的任何东西。

+   由于管道返回一个字典，你应该使用唯一的键；否则，相同键的结果将被覆盖，最后一个键值对将被保留。

+   是贪婪求值的。

# 示例

在这里，让我们看看上述管道的行为。我们将对以下可迭代对象进行操作：

```py
>>> items = (1, 1.0, 10, 50.03, 100)
```

我以 doctests 的形式展示这些示例。你可以在以下*Towards Data Science*文章中阅读更多关于这个用于文档测试的出色 Python 内置包的内容：

[](/python-documentation-testing-with-doctest-the-easy-way-c024556313ca?source=post_page-----ec68dce53d03--------------------------------) ## Python 文档测试使用 doctest：简单的方法

### doctest 允许进行文档、单元和集成测试，以及测试驱动开发。

[towardsdatascience.com

在文章末尾的附录中，你会找到这个练习的完整脚本。

## 生成器管道

```py
>>> gen_pipeline = get_generator_pipeline(items)
>>> gen_pipeline # doctest: +ELLIPSIS
<generator object get_generator_pipeline.<locals>.<genexpr> at 0x7...>
```

正如预期的那样，生成器管道返回一个生成器。因此，目前我们无法看到输出，要做到这一点，我们需要评估其值。如何做取决于管道和解决的问题。在这里，我们将使用一个简单的 `for` 循环：

```py
>>> for i in gen_pipeline:
...     print(i)
245.77
245.77
560.77
3924.49
12620.77
```

请记住，即使 `gen_pipeline` 是使用生成器管道创建的，它仍然是一个普通的生成器。作为生成器，在评估之后（我们在上面的 `for` 循环中做了），它是空的。它仍然存在，但你不能再用它来查看输出：

```py
>>> gen_pipeline # doctest: +ELLIPSIS
<generator object get_generator_pipeline.<locals>.<genexpr> at 0x7...>
>>> next(gen_pipeline)
Traceback (most recent call last):
  ...
StopIteration
```

## Listcomp 管道

```py
>>> list_pipeline = get_listcomp_pipeline(items)
>>> list_pipeline
[245.77, 245.77, 560.77, 3924.49, 12620.77]
```

Listcomp 管道会贪婪地评估管道，因此在创建时就可以看到结果。你可以根据需要多次这样做，这与上面的生成器管道不同。

像之前一样，我们可以看到前两个值完全相同。这是可以预期的，因为 `items` 的前两个元素是相同的……或者不是？第一个是整数 `1`，而第二个是浮点数 `1.0`。理论上，这些 *不是* 相同的对象，因为它们的类型不同。然而，Python 将它们视为相等：

```py
>>> 1 == 1.0
True
```

那么，`setcomp` 和 `dictcomp` 管道会有什么表现呢？我们将在下文中看到。

## Setcomp 管道

```py
>>> set_pipeline = get_setcomp_pipeline(items)
>>> set_pipeline
{560.77, 3924.49, 245.77, 12620.77}
```

哈！注意虽然 `items` 包含五个元素，但上述输出仅包含四个。这并不意外——正如我们之前看到的，Python 将 `1` 和 `1.0` 视为相等，因此对这两个值评估 `calculate(x)` 的结果是相同的。由于它们是相同的，结果集合中只包含一个输出值，即 `245.77`。

使用集合和 setcomp 管道时请记住这一点。因此，当你希望实现这种行为时——换句话说，当你希望只保留唯一结果时——请使用 setcomp 管道。

## Dictcomp 管道

```py
>>> dict_pipeline = get_dictcomp_pipeline(items)
{1: 245.77, 10: 560.77, 50.03: 3924.49, 100: 12620.77}
```

与集合一样，我们得到了结果字典的四个元素。如你所见，当你希望在字典中使用 `1` 和 `1.0` 作为键时，它们会合并成一个键，在我们的例子中是 `1`。如果这正是你想要的结果，那么你完成了。

如果你需要它们两个怎么办？你可以创建字符串键。例如，Python 是否将 `str(1)` 和 `str(1.0)` 视为不同的？让我们来看看：

```py
>>> str(1) != str(1.0)
True
```

是的，确实如此！我们需要重新定义管道函数，然后：

```py
def get_dictcomp_pipeline_str(items: PipelineItems) -> Dict[str, Number]:
    """Create dictcomp pipeline using calculate() for items.

    str(item) are dict keys with calculate(item) being
    the corresponding value.
    """
    return {str(x_i): calculate(x_i) for x_i in items}
```

让我们看看这个新的 dictcomp 管道如何运作：

```py
>>> dict_str_pipeline = get_dictcomp_pipeline_str(items)
>>> dict_str_pipeline
{'1': 245.77, '1.0': 245.77, '10': 560.77, '50.03': 3924.49, '100': 12620.77}
```

结果字典包含五个元素，正如我们所期望的那样。

# 结论

在这篇文章中，我提出了将生成器管道的一般化扩展到理解管道。虽然生成器表达式通常用于创建管道，但生成的生成器管道是理解管道的一个特例。当你创建一个管道时，请考虑哪种类型的管道最能代表你的需求——并使用它。不必因为“生成器管道”这个术语在 Python 社区中很常见而仅仅坚持使用生成器管道。你可以自由使用任何适合你目标的方法。

在这篇文章中，我们使用了简单的示例。我这样做是有目的的：这种简洁性帮助我们集中关注本文的主要话题——理解管道。未来，我计划展示更多代表现实生活场景的高级示例。

请注意，创建最终理解的函数——在我们的示例中，`get_generator_pipeline()`、`get_listcomp_pipeline()`、`get_setcomp_pipeline()`、`get_dictcomp_pipeline()`和`get_dictcomp_pipeline_str()`——仅创建管道的最后一步。然而，实际的管道隐藏在这些函数调用的函数中；在我们的例子中，这是`calculate()`函数。我们暂时回到这个函数：

```py
def calculate(x: Number) -> Number:
    x = power(x, .5)
    x = double(x)
    x = add(x, 12)
    x = power(x, 2)
    x = add(x, math.pi**.5)
    x = round(x, 2)
    x = add(x, 75)
    return x
```

你明白我的意思吗？我们的管道由函数`power()`、`double()`、`add()`、再次`power()`、再次`add()`、`round()`和再次`add()`组成。这里应用了所有的管道步骤，创建输出的函数只是以适合你需求的方式调用这个函数。

记住，如果基本函数（前面句子中列出的前四个函数）不符合你的需求，你可以创建一个新函数，就像我们在定义`get_dictcomp_pipeline_str()`函数时所做的那样。这个例子表明，我们不受限于理解管道的基础版本：只要正确，你可以做任何你想做的事情。

# 脚注

¹ 如果你使用的是低于 3.10 版本的 Python，代码将无法运行。这是因为`typing`的联合运算符`|`可以替代`Union`，它是在 Python 3.10 中添加的。因此，如果你使用的是旧版本 Python，请替换这两行：

```py
Number = int | float
PowerType = int | float
```

使用这三行：

```py
Number = Union[int, float]
PowerType = Union[int, float]
```

这将有效。

# 附录

以下是本文中使用的脚本的完整代码。如上脚注所述，在旧版本的 Python 中，你可能需要将`int | float`替换为`Union[int, float]`，当然在此之前需要从`typing`中导入`Union`。

```py
import math

from typing import Dict, Generator, Iterable, List, Set

Number = int | float
PowerType = int | float
PipelineItems = Iterable[Number]

def double(x: Number) -> Number:
    return x**2

def power(x: Number, n: PowerType) -> Number:
    return x**n

def add(x: Number, y: Number) -> Number:
    return x + y

def calculate(x: Number) -> Number:
    x = power(x, 0.5)
    x = double(x)
    x = add(x, 12)
    x = power(x, 2)
    x = add(x, math.pi**0.5)
    x = round(x, 2)
    x = add(x, 75)
    return x

def get_generator_pipeline(
    items: PipelineItems,
) -> Generator[Number, None, None]:
    """Create generator pipeline applying calculate() to each item."""
    return (calculate(x_i) for x_i in items)

def get_listcomp_pipeline(items: PipelineItems) -> List[Number]:
    """Create listcomp pipeline applying calculate() to each item."""
    return [calculate(x_i) for x_i in items]

def get_setcomp_pipeline(items: PipelineItems) -> Set[Number]:
    """Create setcomp pipeline applying calculate() to each item."""
    return {calculate(x_i) for x_i in items}

def get_dictcomp_pipeline(items: PipelineItems) -> Dict[Number, Number]:
    """Create dictcomp pipeline using calculate() for items.

    Items are dict keys with calculate(item) being the corresponding value.
    """
    return {x_i: calculate(x_i) for x_i in items}

def get_dictcomp_pipeline_str(items: PipelineItems) -> Dict[str, Number]:
    """Create dictcomp pipeline using calculate() for items.

    str(item) are dict keys with calculate(item) being
    the corresponding value.
    """
    return {str(x_i): calculate(x_i) for x_i in items}

if __name__ == "__main__":
    items = (1, 1.0, 10, 50.03, 100)
    gen_pipeline = get_generator_pipeline(items)
    list_pipeline = get_listcomp_pipeline(items)
    set_pipeline = get_setcomp_pipeline(items)
    dict_pipeline = get_dictcomp_pipeline(items)
    dict_str_pipeline = get_dictcomp_pipeline_str(items)

    # Generator pipeline
    # Note that we need to evaluate it to see the output,
    # hence the for loop.
    print(gen_pipeline)
    for i in gen_pipeline:
        print(i)

    # Listcomp pipeline
    print(list_pipeline)

    # Setcomp pipeline
    print(set_pipeline)

    # Dictcomp pipeline
    print(dict_pipeline)

    # Dictcomp pipeline with strings as keys
    print(dict_str_pipeline)
```
