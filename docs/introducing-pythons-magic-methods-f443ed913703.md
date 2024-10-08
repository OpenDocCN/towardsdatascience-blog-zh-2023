# 介绍 Python 的魔法方法

> 原文：[`towardsdatascience.com/introducing-pythons-magic-methods-f443ed913703`](https://towardsdatascience.com/introducing-pythons-magic-methods-f443ed913703)

## PYTHON | PROGRAMMING

## 一份关于利用 dunder 函数的力量来提升编程水平的实用指南

[](https://david-farrugia.medium.com/?source=post_page-----f443ed913703--------------------------------)![David Farrugia](https://david-farrugia.medium.com/?source=post_page-----f443ed913703--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f443ed913703--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f443ed913703--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----f443ed913703--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f443ed913703--------------------------------) ·阅读时长 7 分钟·2023 年 5 月 24 日

--

![](img/e1d07678085006aa654f8b8a1494a25c.png)

照片由 [Matt Palmer](https://unsplash.com/@mattpalmer?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 是一种很棒的编程语言，[GitHub](https://octoverse.github.com/2022/top-programming-languages)发现，它在 2022 年也是第二大热门语言。Python 最吸引人的优点是其强大的社区。似乎 Python 为你可能遇到的任何使用场景都有一个包。

Python 还有很多酷炫的特性，这些特性并不是常见的知识。如果你有兴趣了解更多，请随时查看我之前的相关文章。

[](/5-awesome-python-hidden-features-a0172e0bd98e?source=post_page-----f443ed913703--------------------------------) ## 5 个精彩的 Python 隐藏特性 — 第一部分

### 通过这些酷炫的隐藏 Python 特性，将你的编码技能提升到新水平

towardsdatascience.com

在广阔而动态的 Python 编程世界中，存在一组独特的函数，这些函数通常被初学者忽视，但它们在语言的生态系统中具有重要意义。这些就是魔法方法（也称为 dunder 函数）。

魔法方法是一组在 Python 中预定义的方法，提供了特殊的语法特性。它们通过其前后有双下划线来轻松识别，例如 `__init__`、`__call__`、`__len__` 等等。

魔法方法在定义类的对象如何处理各种 Python 操作方面发挥了关键作用。实质上，它们允许自定义对象表现得类似于内置的 Python 类型。直接的好处是增强了 Python 的一致性和直观性。

在这篇文章中，我们将专注于并讨论强大的 Dunder 函数背后的魔法。我们将探讨它们的目的并讨论它们的使用。

无论你是 Python 新手还是经验丰富的程序员，本指南旨在提供对 Dunder 函数的全面理解，使你的 Python 编程体验更加高效和愉快。

记住，Python 的魔力不仅在于它的简单性和多样性，还在于像 Dunder 函数这样的强大功能。所以，让我们一起揭开魔法的面纱吧！

# `__init__`

也许所有 Dunder 函数中最基本的是`__init__`。这是 Python 在我们创建（或如名称所示，初始化）一个新对象时自动调用的魔法方法。

```py
class Pizza:
    def __init__(self, size, toppings):
        self.size = size
        self.toppings = toppings

# Now let's create a pizza
my_pizza = Pizza('large', ['pepperoni', 'mushrooms'])

print(my_pizza.size)  # This will print: large
print(my_pizza.toppings)  # This will print: ['pepperoni', 'mushrooms']
```

我们创建了一个名为`Pizza`的类。我们将`__init__`函数设置为接受初始化时指定的大小和配料参数，并将它们设置为自定义对象的属性。

在这里，`self`用于表示类的实例。所以当我们说`self.size = size`时，我们是在说：“嘿，这个 pizza 对象有一个属性 size，我想它是我在创建对象时提供的大小。”

# `__str__`和`__repr__`

## `__str__`

这是 Python 的魔法方法，允许我们为自定义对象定义描述。它本质上是在回答以下问题：

> “你会如何向朋友描述这个对象？”

当你使用`str()`打印对象或将其转换为字符串时，Python 会检查你是否为该对象的类定义了`__str__`方法。

如果你有，它会使用那个方法将对象转换为字符串。

我们可以扩展我们的 Pizza 示例，加入一个`__str__`函数，如下所示：

```py
class Pizza:
    def __init__(self, size, toppings):
        self.size = size
        self.toppings = toppings

    def __str__(self):
        return f"A {self.size} pizza with {', '.join(self.toppings)}"

my_pizza = Pizza('large', ['pepperoni', 'mushrooms'])
print(my_pizza)  # This will print: A large pizza with pepperoni, mushrooms
```

## `__repr__`

`__str__`函数更像是描述对象属性的非正式方式。另一方面，`__repr__`用于提供一个更正式、详细和明确的自定义对象描述。

如果你在对象上调用`repr()`，或者在控制台中输入对象的名称，Python 将查找`__repr__`方法。

如果`__str__`没有定义，Python 会使用`__repr__`作为备份来打印对象或将其转换为字符串。因此，至少定义`__repr__`通常是一个好主意，即使你不定义`__str__`。

下面是我们可以为我们的 pizza 示例定义`__repr__`的方法：

```py
class Pizza:
    def __init__(self, size, toppings):
        self.size = size
        self.toppings = toppings

    def __repr__(self):
        return f"Pizza('{self.size}', {self.toppings})"

my_pizza = Pizza('large', ['pepperoni', 'mushrooms'])
print(repr(my_pizza))  # This will print: Pizza('large', ['pepperoni', 'mushrooms'])
```

你看，`__repr__`会给你一个可以作为 Python 命令运行的字符串，以重建 pizza 对象，而`__str__`则提供了一个更易于理解的描述。希望这有助于你更好地理解这些魔法方法！

# `__add__`

在 Python 中，我们都知道你可以使用`+`运算符将数字相加，例如`3 + 5`。

但是如果我们想添加一些自定义对象的实例呢？`__add__` Dunder 函数允许我们做到这一点。它使我们能够定义`+`运算符在自定义对象上的行为。

为了保持一致性，假设我们想要在我们的披萨示例中定义 `+` 行为。假设每当我们将两张或更多的披萨相加时，它将自动合并所有的配料。以下是它可能的实现方式：

```py
class Pizza:
    def __init__(self, size, toppings):
        self.size = size
        self.toppings = toppings

    def __add__(self, other):
        if not isinstance(other, Pizza):
            raise TypeError("You can only add another Pizza!")
        new_toppings = self.toppings + other.toppings
        return Pizza(self.size, new_toppings)

# Let's create two pizzas
pizza1 = Pizza('large', ['pepperoni', 'mushrooms'])
pizza2 = Pizza('large', ['olives', 'pineapple'])

# And now let's "add" them
combined_pizza = pizza1 + pizza2

print(combined_pizza.toppings)  # This will print: ['pepperoni', 'mushrooms', 'olives', 'pineapple']
```

类似于 `__add__` 双下划线方法，我们还可以定义其他算术函数，比如 `__sub__`（用于使用 `—` 操作符进行减法）和 `__mul__`（用于使用 `*` 操作符进行乘法）。

# __len__

这个双下划线方法允许我们定义 `len()` 函数对于我们自定义对象应返回的内容。

Python 使用 `len()` 来获取数据结构的长度或大小，比如列表或字符串。

在我们的披萨类的上下文中，我们可以说披萨的“长度”是它拥有的配料数量。以下是我们可以实现这一点的方法：

```py
class Pizza:
    def __init__(self, size, toppings):
        self.size = size
        self.toppings = toppings

    def __len__(self):
        return len(self.toppings)

# Let's create a pizza
my_pizza = Pizza('large', ['pepperoni', 'mushrooms', 'olives'])

print(len(my_pizza))  # This will print: 3
```

在 `__len__` 方法中，我们仅返回 `toppings` 列表的长度。现在，`len(my_pizza)` 将告诉我们 `my_pizza` 上有多少个配料。

> **注意：**
> 
> `__len__` 应该始终返回一个整数，并且预期是一个非负值。负配料只是怪异，不是吗？

# __iter__

这个双下划线方法允许你的对象是可迭代的——也就是说，可以在 `for` 循环中使用。

为此，我们还需要定义 `__next__` 函数。这个函数用于定义返回迭代中的下一个值的行为。此外，它还应该在序列中没有更多项目时向可迭代对象发出信号。我们通常通过引发 `StopIteration` 异常来实现这一点。

对于我们的披萨示例，假设我们想要迭代配料。我们可以通过定义 `__iter__` 方法使我们的披萨类可迭代，如下所示：

```py
class Pizza:
    def __init__(self, size, toppings):
        self.size = size
        self.toppings = toppings

    def __iter__(self):
        self.n = 0
        return self

    def __next__(self):
        if self.n < len(self.toppings):
            result = self.toppings[self.n]
            self.n += 1
            return result
        else:
            raise StopIteration

# Let's create a pizza
my_pizza = Pizza('large', ['pepperoni', 'mushrooms', 'olives'])

# And now let's iterate over it
for topping in my_pizza:
    print(topping)
```

在这种情况下，`for` 循环调用 `__iter__`，它初始化一个计数器（`self.n`）并返回披萨对象本身（`self`）。

然后，`for` 循环调用 `__next__` 来依次获取每个配料。

当 `__next__` 返回了所有配料后，它会引发 `StopIteration` 异常，然后 `for` 循环现在知道没有更多的配料了，因此将停止迭代过程。

# 结论

总之，很明显，魔法方法确实是 Python 语言的核心。

它们构成了许多 Python 内置操作的基础，为我们开发者提供了必要的自由度，以便以与语言其余部分的用法一致的方式个性化我们对象的行为。

在本文中，我们讨论了如何利用双下划线函数，如 `__init__`、`__iter__` 和 `__len__`，来满足我们的需求。你可以在这里找到 Python 允许的所有双下划线方法的完整列表：

[](https://mathspp.com/blog/pydonts/dunder-methods?source=post_page-----f443ed913703--------------------------------) [## mathspp - 将你的 Python 🐍 提升到下一个水平 🚀

### 这是对 Python 中双下划线方法的介绍，帮助你理解它们是什么以及它们的作用。（如果…

[mathspp.com](https://mathspp.com/blog/pydonts/dunder-methods?source=post_page-----f443ed913703--------------------------------)

**你喜欢这篇文章吗？每月$5，你可以成为会员，解锁无限访问 Medium 的权限。你将直接支持我和你在 Medium 上喜爱的所有其他作家。非常感谢！**

[## 通过我的推荐链接加入 Medium - David Farrugia](https://david-farrugia.medium.com/membership?source=post_page-----f443ed913703--------------------------------)

### 获取对我所有⚡高级⚡内容和 Medium 上无限制的访问权限。通过买一杯咖啡支持我的工作…

[david-farrugia.medium.com](https://david-farrugia.medium.com/membership?source=post_page-----f443ed913703--------------------------------)

# 想要联系我吗？

我非常想听听你对这个话题或关于 AI 和数据的任何想法。

如果你希望联系我，请发邮件至 ***davidfarrugia53@gmail.com***。

[Linkedin](https://www.linkedin.com/in/david-farrugia/)
