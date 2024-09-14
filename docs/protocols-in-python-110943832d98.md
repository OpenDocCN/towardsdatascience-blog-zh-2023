# Python 中的协议

> 原文：[`towardsdatascience.com/protocols-in-python-110943832d98`](https://towardsdatascience.com/protocols-in-python-110943832d98)

## 如何使用结构性子类型

[](https://medium.com/@hrmnmichaels?source=post_page-----110943832d98--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----110943832d98--------------------------------)[](https://towardsdatascience.com/?source=post_page-----110943832d98--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----110943832d98--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----110943832d98--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----110943832d98--------------------------------) ·5 min 阅读·2023 年 7 月 27 日

--

Python 3.8 引入了一个新特性：[协议](https://peps.python.org/pep-0544/)。协议是[抽象基类 (ABC)](https://docs.python.org/3/library/abc.html)的替代方案，并允许结构性子类型检查——仅基于可用属性和函数检查两个类是否兼容。在这篇文章中，我们将深入探讨这个细节，并展示如何通过实际示例使用协议。

![](img/90dd020181af078f64485fe5957a0b41.png)

由[Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，[Unsplash](https://unsplash.com/photos/9cd8qOgeNIY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上的照片

# 在 Python 中的输入

让我们首先讨论 Python 的类型。它是动态类型语言，这意味着类型在运行时推断，以下代码可以正常运行：

```py
def add(x, y):
    return x + y

print(add(2, 3))
print(add("str1", "str2"))
```

第一次调用结果是整数相加返回 5，第二次是字符串连接返回“str1str2”。这与静态类型的 C++ 不同——在 C++ 中我们必须提供类型声明：

```py
int add(int x, int y) {
    return x + y;
}

std::string add(std::string x, std::string y) {
    return x + y;
}

int main()
{
    std::cout<<add(2, 3);
    std::cout << add("str1", "str2");
    return 0;
}
```

静态类型的优点在于可以在编译时捕捉到潜在的错误，而在动态类型语言中，我们只能在运行时遇到这些错误。另一方面，动态类型允许更快速的原型设计和实验，这也是 Python 变得如此流行的原因之一。

动态类型也被称为[鸭子类型](https://en.wikipedia.org/wiki/Duck_typing)，这是基于这样一句话：“如果它走起来像一只鸭子，叫声像一只鸭子，那么它一定是一只鸭子”。因此：如果对象提供相同的属性/函数，它们应该被类似对待，例如可以传递给要求另一种类型的函数。

尽管如此，尤其是在较大、更专业的软件产品中，这种不可靠性带来的弊端多于利端——因此趋势是向静态类型检查方向发展，例如通过[使用 mypy 提供类型提示](https://medium.com/towards-data-science/introduction-to-mypy-3d32fc96db75)。

## **子类型化**

一个有趣的问题——在关于鸭子类型的简短段落中有所提及——是子类型化。如果我们有一个签名为 `foo(x: X)` 的函数，除了 `X` 之外，mypy 还允许哪些其他类被传递给该函数？（注意我们现在只讨论类型和类型提示——由于 Python 是动态类型的，如前所述，我们可以将任何对象传递给 `foo`——如果有期望的属性/方法，它就能工作，否则会崩溃。）为此，人们区分结构性和名义性子类型化。结构性子类型化基于类层次/继承：如果类 `B` 继承自 `A`，它是 `A` 的子类型——因此也可以在期望 `A` 的地方使用。另一方面，名义性子类型化基于给定类的可用操作定义——如果 `B` 提供了 `A` 所有的属性/函数，它可以在所有期望 `A` 的地方使用。可以说，后一种方法更“Pythonic”，因为它更像鸭子类型的思想。

# 实践中的子类型化

在 Python 3.8 之前，人们只能通过继承来进行子类型化，例如使用（抽象）基类（ABCs）：在这里，我们定义一个（抽象）基类——一个子类的“蓝图”——然后定义几个从这个基类继承的子类：

```py
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def feed(self) -> None:
        pass

class Duck(Animal):
    def feed(self) -> None:
        print("Duck eats")

def feed(animal: Animal) -> None:
    animal.feed()

duck = Duck()
feed(duck)
```

在这段代码中，我们首先定义了 ABC `Animal`，提供抽象方法 `feed`。然后，我们子类化 `Duck` 并实现这个方法。最后，我们定义一个通用的 `feed` 函数，接受一个 `Animal` 作为参数，然后喂养这个动物。

这似乎是合理的——问题在哪里？确实有一些原因，为什么可能不想使用这种模式：基类往往暴露不充分且难以包含——即，如果你想从另一个模块中的基类继承，也许是一个公共库，你将不得不找到这个基类。其次，你不能更改现有代码，例如公共/第三方模块：如果你希望从这些模块中导入的类型成为其他类型的子类型，可能与其他由你引入的类型组合使用，这会是个问题。最后，这在某种程度上违背了 Python 和鸭子类型的思想。

## **协议**

因此，Python 3.8 引入了协议，缓解了上述问题。协议，如其名称所示，通过定义期望的属性/方法的“接口”隐式地工作，并在必要时检查相关类是否提供了这些属性/方法：

```py
from typing import Protocol

class Animal(Protocol):
    def feed(self) -> None:
        pass

class Duck:
    def feed(self) -> None:
        print("Duck eats")

def feed(animal: Animal) -> None:
    animal.feed()

duck = Duck()
feed(duck)
```

如我们所见，`Animal` 现在是一个 `Protocol`，而 `Duck` 并未从任何基类继承——但 mypy 依然很高兴。

**子类化协议**

自然，我们也可以对子协议进行子类化——即定义一个继承自父协议的子协议，并对其进行扩展。在这样做时，我们只需记住同时继承父协议和 `typing.Protocol`：

```py
from typing import Protocol

class Animal(Protocol):
    def feed(self) -> None:
        pass

class Bird(Animal, Protocol):
    def fly(self) -> None:
        pass

class Duck:
    def feed(self) -> None:
        print("Duck eats")

    def fly(self) -> None:
        print("Duck flies")

def feed(animal: Animal) -> None:
    animal.feed()

def feed_bird(bird: Bird) -> None:
    bird.feed()
    bird.fly()

duck = Duck()
feed_bird(duck)
```

在上述代码中，我们将 `Bird` 指定为特定的 `Animal` 类型，然后定义一个喂鸟的函数，期望它随后飞走。

# 协议简史

上述所有代码都是有效的 Python，甚至在 Python 3.8 之前（记住，Python 是动态类型的）——我们只需要 ABCs 来满足 mypy，否则 mypy 会因为没有 ABCs 的例子而发出警告。然而，协议在 Python 中存在的时间更久，只是不像现在这样明显或显式：大多数 Python 开发者将“协议”一词用作遵守某些接口的约定，就像现在所明确的那样。一个著名的例子是 [迭代器协议](https://www.pythonmorsels.com/iterator-protocol/#:~:text=The%20iterator%20protocol%20is%20how,it%20evaluates%20a%20for%20loop.)——一个描述自定义迭代器需要实现哪些方法的接口。为了使这与 mypy 一起工作而无需显式协议，存在几种“技巧”，例如自定义类型：

```py
from typing import Iterable

class SquareIterator:
    def __init__(self, n: int) -> None:
        self.i = 0
        self.n = n

    def __iter__(self) -> "SquareIterator":
        return self

    def __next__(self) -> int:
        if self.i < self.n:
            i = self.i
            self.i += 1
            return i**2
        else:
            raise StopIteration()

def iterate(items: Iterable[int]) -> None:
    for x in items:
        print(x)

iterator = SquareIterator(5)
iterate(iterator)
```

# 抽象基类与协议

我们已经讨论了 ABCs 的潜在缺点（与外部模块和接口的兼容性、“非 Pythonic” 代码）。然而，协议不应取代 ABCs，而是两者都应使用。例如，ABCs 是重用代码的好方法：所有通用功能应在基类中实现，而只有特定部分在子类中实现。这在协议中是不可能的。

# 结论

在这篇文章中，我们讨论了静态与动态类型（鸭子类型），以及 mypy 如何处理子类型。在 Python 3.8 之前，必须使用抽象基类（ABCs）。随着协议的引入，Python 获得了一种优雅的定义“接口”的方式，并允许 mypy 检查这些接口的遵守情况：以更 Pythonic 的方式，协议允许指定类需要实现哪些属性/函数，然后允许所有这些类作为协议的子类型使用。
