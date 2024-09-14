# Python 中的类型提示

> 原文：[`towardsdatascience.com/type-hints-in-python-1c096f44f375`](https://towardsdatascience.com/type-hints-in-python-1c096f44f375)

## 你的代码将不再是一个谜

[](https://polmarin.medium.com/?source=post_page-----1c096f44f375--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----1c096f44f375--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c096f44f375--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c096f44f375--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----1c096f44f375--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c096f44f375--------------------------------) ·阅读时间 7 分钟·2023 年 7 月 11 日

--

![](img/645e61c388600f7ac62f741fec4cf3ab.png)

图片来源：[Agence Olloweb](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

前几天我试图解读我过去编写的一个脚本如何工作。我知道它做了什么，它的解释和文档都很充分，但理解其工作原理更为麻烦。

代码繁琐且复杂，虽然有些注释，但缺乏适当的样式。这时我决定学习 PEP 8[1]并将其融入我的代码中。

如果你不知道 PEP 8 是什么，它基本上是一个提供指导方针、编码约定和最佳实践的文档，用于编写 Python 代码。

我们难以理解的代码的解决方案就在那儿。然而，我们大多数人从未花时间去阅读它并将这些指导方针融入日常实践中。

这需要时间和很多错误，但相信我，这是值得的。我学到了很多东西，代码现在开始变得更好了。

我最喜欢的发现之一是**类型提示**（或**类型注释**）——这将是今天帖子的主题。实际上，类型提示已经出现在 PEP 3107[2]中，回到 2006 年，并在 484[3]版本（2014 年）中重新审视并全面记录。从那时起，它在新的 PEP 版本中得到了多次改进，几乎成为经典。

所以，对于许多人来说，这是一个老话题但又非常新鲜。

## 什么是类型提示？

类型提示指示函数（同样适用于类方法）的输入和输出的数据类型。

许多 Python 用户抱怨的一个问题是我们可以自由地更改变量类型。在 C 语言和许多其他语言中，你需要声明一个变量并指定其类型：字符、整数等。

每个人都会有自己的看法——有些人可能喜欢 Python 的自由（以及对内存管理的影响），而另一些人则更喜欢传统语言的限制，因为这使他们的代码更具可读性。

无论如何。

类型提示旨在使你的 Python 代码更具可读性，我相信我们大多数人都很欣赏这种方法。然而，它们旨在澄清，它们并不强制变量的数据类型。

**如果变量的类型不是我们预期的，程序不会引发错误。**

## 为什么数据科学家应该考虑使用它们？

说实话，任何 Python 程序员都能从类型注解中受益。但这对数据科学家和其他与数据相关的专业人士来说可能更有意义。

那是因为我们处理各种数据。不仅仅是简单的字符串、列表或元组。我们使用的数据可能涉及超级复杂的结构，而类型提示有潜力节省我们很多时间，帮助我们了解函数预期的数据类型。

例如，假设我们有一个基于字典的结构。它的键是元组，值是具有字符串键和集合值的嵌套字典。

祝好运，希望几个月后你重访代码时能记住这一点！

好的一点是，类型提示非常容易理解和使用。我们没有理由不使用它们，而且不使用它们也没有任何好处。

那么，让我们继续并开始查看一些代码。

## 1\. 首先概述

我将使用 Python 3.11，但大多数示例适用于之前的 Python 3 版本。

让我们使用一个示例和虚拟函数：

```py
def meet_someone(name, age):
    return f"Hey {name}, I heard you are {age}. I'm 21 and from Mars!"
```

这很愚蠢。但它包含了我们需要的一切，我们现在将添加一些变体。

我们这里没有任何类型注解。只有一个接受两个参数并返回字符串的函数。我相信你知道 `name` 参数应该是字符串，而 `age` 参数则预期是整数（甚至是浮点数）。

但你知道，因为这是一个非常简单的函数。很少会如此简单。这就是为什么添加一些提示可能是明智的。

这是更新：

```py
def meet_someone(name: str, age: int) -> str:
    return f"Hey {name}, I heard you are {age}. I'm 21 and from Mars!"
```

在这种情况下，我指定 `age` 应该是一个整数。让我们尝试运行这个函数：

```py
>>> meet_someone('Marc', 22)
Hey Marc, I heard you are 22\. I'm 21 and from Mars!
```

为了说明我在上一节末尾说的内容：

```py
>>> meet_someone('Marc', 22.4)
Hey Marc, I heard you are 22.4\. I'm 21 and from Mars!
```

即使 22.4 是浮点数（而不是预期的整数），它也工作得很好。如前所述，这些只是类型提示，仅此而已。

好的，基础知识已经涵盖。让我们开始制作一些变体。

## 2\. 多种数据类型

假设我们希望允许整数和浮点数作为年龄参数的数据类型。我们可以使用**Union**，来自 typing 模块[4]：

```py
from typing import Union
def meet_someone(name: str, age: Union[int, float]) -> str:
    return f"Hey {name}, I heard you are {age}. I'm 21 and from Mars!"
```

很简单：`Union[int, float]` 表示我们期望值是整数或浮点数。

然而，如果你使用的是 Python 3.10 或更高版本，还有另一种方法可以在不使用 Union 的情况下实现相同功能：

```py
def meet_someone(name: str, age: int | float) -> str:
    return f"Hey {name}, I heard you are {age}. I'm 21 and from Mars!"
```

这只是一个简单的 OR 运算符。在我看来，更容易理解。

## 3\. 高级数据类型

现在假设我们要处理更复杂的参数，比如字典或列表。接下来我们使用下一个函数，它使用了`meet_someone`函数：

```py
def meet_them_all(people) -> str:
    msg = ""
    for person in people:
        msg += meet_someone(person, people[person])
    return msg
```

这仍然是一个非常简单的函数，但现在参数可能不像我们之前看到的那样清晰。如果你实际检查代码，你会看到我们期望的是一个字典。

但如果我们不需要猜测会不会更好？这就是类型提示的力量。

到此为止，如果我让你自己添加类型提示，你可能会做这样的事情：

```py
def meet_them_all(people: dict) -> str:
    msg = ""
    for person in people:
        msg += meet_someone(person, people[person])
    return msg
```

这很好。但是我们还没有充分利用它的全部潜力。我们在这里指定了想要一个`dict`，但没有指定它的键和值的类型。这是一个改进版：

```py
def meet_them_all(people: dict[str, int]) -> str:
    msg = ""
    for person in people:
        msg += meet_someone(person, people[person])
    return msg
```

在这里，我们说我们期望`people`是一个字典，键是字符串，值是整数。类似于`{'Pol': 23, 'Marc': 21}`。

但记住我们想要接受年龄为整数或浮点数…

```py
from typing import Union
def meet_them_all(people: dict[str, Union[int, float]]) -> str:
    msg = ""
    for person in people:
        msg += meet_someone(person, people[person])
    return msg
```

我们可以直接使用我们在第二部分学到的内容！很酷吧？

哦，它不仅适用于内置数据类型。你可以使用任何你想要的数据类型。例如，假设我们想要一个 Pandas 数据框的列表，用于一个不返回任何东西的函数：

```py
import pandas as pd

Vector = list[pd.DataFrame]

def print_vector_length(dfs: Vector) -> None:
    print(f'We received {len(dfs)} dfs')
```

我在这里做的是声明数据类型，它只是一个数据框的列表，并将其用作类型提示。

此外，还有我们今天之前没有见过的，这个函数不会返回任何东西。这就是为什么输出数据类型是 None。

## 4. Optional 运算符

我们经常创建一些参数不是必需的——它们是可选的。

既然我们已经了解了这些，下面是如何编写一个带有可选参数的函数：

```py
def meet_someone(name: str, 
                 age: int | float, 
                 last_name: None | str = None
                ) -> str:
    msg = f"Hey {name}{' ' + last_name if last_name else ''}, "\
          f"I heard you are {age}. I'm 21 and from Mars!"

    return msg
```

我已经更新了返回的消息，但重要的部分是最后一个参数`last_name`的类型提示。看看我这里怎么说：“`last_name`要么是一个字符串，要么是一个 Null 值。它是可选的，默认情况下是 None。”

这很酷，也很直观，但想象一下一个参数有几种可能的数据类型……这可能会很长。

这就是为什么 Optional 运算符在这里很有用，它基本上允许我们跳过`None`提示：

```py
from typing import Optional

def meet_someone(name: str, 
                 age: int | float, 
                 last_name: Optional[str] = None
                ) -> str:
    msg = f"Hey {name}{' ' + last_name if last_name else ''}, "\
          f"I heard you are {age}. I'm 21 and from Mars!"

    return msg
```

## 结论与下一步

我希望我已经传达了类型提示在提高代码可读性和理解方面的有用性。不仅仅是为了我们的同事程序员，也是为了我们未来的自己！

我已经介绍了基础知识，但我建议你继续查看 typing 模块提供的内容。那里有几类可以让你的代码看起来更好。

```py
**Thanks for reading the post!** 

I really hope you enjoyed it and found it insightful.

Follow me and subscribe to my mailing list for more 
content like this one, it helps a lot!

**@polmarin**
```

如果你想进一步支持我，请考虑通过下面的链接订阅 Medium 会员：这不会花费你额外的钱，但会帮助我完成这个过程。

[](https://medium.com/@polmarin/membership?source=post_page-----1c096f44f375--------------------------------) [## 通过我的推荐链接加入 Medium - Pol Marin

### 阅读 Pol Marin 的每一篇故事（以及 Medium 上成千上万其他作家的文章）。你的会员费用直接支持 Pol…

[medium.com](https://medium.com/@polmarin/membership?source=post_page-----1c096f44f375--------------------------------)

## 资源

[1] [PEP 8 — Python 代码风格指南](https://peps.python.org/pep-0008/) | peps.python.org

[2] [3107 — 函数注解](https://peps.python.org/pep-3107/) | peps.python.org

[3] [484 — 类型提示 | peps.python.org](https://peps.python.org/pep-0484/)

[4] [typing — 对类型提示的支持](https://docs.python.org/3/library/typing.html)
