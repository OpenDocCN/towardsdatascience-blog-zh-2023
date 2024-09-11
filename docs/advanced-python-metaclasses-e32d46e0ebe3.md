# 高级 Python： metaclasses

> 原文：[https://towardsdatascience.com/advanced-python-metaclasses-e32d46e0ebe3?source=collection_archive---------1-----------------------#2023-10-06](https://towardsdatascience.com/advanced-python-metaclasses-e32d46e0ebe3?source=collection_archive---------1-----------------------#2023-10-06)

## 对 Python 类对象及其创建方式的简要介绍

[](https://medium.com/@ilija.lazarevic?source=post_page-----e32d46e0ebe3--------------------------------)[![Ilija Lazarevic](../Images/4a0d84af6d8fa97705ee35444d319b07.png)](https://medium.com/@ilija.lazarevic?source=post_page-----e32d46e0ebe3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e32d46e0ebe3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e32d46e0ebe3--------------------------------) [Ilija Lazarevic](https://medium.com/@ilija.lazarevic?source=post_page-----e32d46e0ebe3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe73ea2eae8e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-python-metaclasses-e32d46e0ebe3&user=Ilija+Lazarevic&userId=e73ea2eae8e6&source=post_page-e73ea2eae8e6----e32d46e0ebe3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e32d46e0ebe3--------------------------------) ·8分钟阅读·2023年10月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe32d46e0ebe3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-python-metaclasses-e32d46e0ebe3&user=Ilija+Lazarevic&userId=e73ea2eae8e6&source=-----e32d46e0ebe3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe32d46e0ebe3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-python-metaclasses-e32d46e0ebe3&source=-----e32d46e0ebe3---------------------bookmark_footer-----------)![](../Images/e409921158233f95efc4e3cb4cd1ba6f.png)

正如阿特拉斯之于天空，metaclasses 之于类。照片由 [Alexander Nikitenko](https://unsplash.com/@quintonik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/H6obC_biCSk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

本文继续了《高级 Python》系列（前一篇关于 Python 函数的 [文章](https://medium.com/towards-data-science/advanced-python-functions-3be6810f92d1)）。这次，我介绍了元类。这个主题相当高级，因为工程师通常不需要实现自定义元类。然而，这是每个了解 Python 的开发者都应该知道的最重要的构造和机制之一，主要是因为它支持面向对象编程范式。

在理解了元类的概念以及类对象是如何创建之后，你将能够继续学习面向对象编程的封装、抽象、继承和多态原则。然后，你将能够通过许多设计模式以及软件工程中的一些原则（例如，*SOLID*）来理解如何应用这些原则。

现在，让我们从这个看似微不足道的例子开始：

```py
class Person:
    pass

class Child(Person):
    pass

child = Child()
```

当你学习面向对象编程时，你很可能遇到过一个描述类和对象是什么的一般性概念，它是这样的：

> “类就像一个饼干模具。对象则是由它模制出来的饼干。”

这是一个非常直观的解释，并且相当清晰地传达了这个概念。话虽如此，我们的例子定义了两个几乎没有功能的模板，但它们仍然有效。你可以尝试定义 `__init__` 方法，设置一些对象属性，使其更具可用性。

然而，**在 Python 中**有趣的是，**尽管类是一个“模板”，用于从中创建对象，但它本身也是一个对象。** 学习 Python 面向对象编程的人很快就会略过这个陈述，并没有深入思考。Python 中的一切都是对象，那又怎样呢？但是一旦你开始思考这个问题，会出现很多问题，Python 的复杂性也会揭示出来。

在我开始问你这些问题之前，让我们记住**在 Python 中，一切都是对象**。我说的“一切”就是所有东西。这可能是你已经了解的，即使你是新手。下面的例子展示了这一点：

```py
class Person:
    pass

id(Person) 
# some memory location

class Child(Person):
    pass

id(Child)
# some memory location

# Class objects are created, you can instantiate objects

child = Child()

id(child)
# some memory location
```

基于这些例子，这里有一些你应该问自己的问题：

+   如果类是一个对象，那么它是在什么时候创建的？

+   谁创建了类对象？

+   如果类是一个对象，那我怎么能在实例化对象时调用它呢？

# 类对象创建

Python 被广泛认为是一种解释型语言。这意味着有一个解释器（程序或进程）逐行读取并尝试将其转换为机器码。这与像 C 这样的编译型编程语言相对，在编译型语言中，编程代码会在运行之前被转换为机器码。这是一个非常简化的观点。更准确地说，Python 既是编译型的又是解释型的，但这是另一个话题。对我们示例重要的是，解释器会遍历类定义，并在类代码块完成后创建类对象。从那时起，你可以从中实例化对象。当然，你必须显式地做到这一点，即使类对象是隐式实例化的。

当解释器完成读取类代码块时，会触发什么“过程”？我们可以直接进入细节，但一张图胜过千言万语：

![](../Images/8b234379df93408228ea4868c29df990.png)

对象、类和元类之间的关系。图像由 Ilija Lazarevic 提供。

如果你还不知道，Python 有 `type` 函数可以用于我们的目的。通过将对象作为参数调用 `type`，你将得到对象的类型。真是巧妙！看一下：

```py
class Person:
    pass

class Child(Person):
    pass

child = Child()

type(child)
# Child

type(Child)
# type
```

示例中的`type`调用是有意义的。`child` 的类型是 `Child`。我们使用了一个类对象来创建它。因此，从某种意义上说，你可以认为`type(child)`给出了其“创建者”的名称。在某种程度上，`Child` 类是它的创建者，因为你调用它来创建一个新实例。但当你尝试获取类对象的“创建者”时，即`type(Child)`，你得到的是 `type`。总结一下，**对象是类的实例，类是类型的实例**。到现在为止，你可能会想知道类是如何成为函数的实例的，答案是`type`既是函数又是类。这故意保留了这种状态，以确保与早期版本的向后兼容。

让你头脑发热的是我们用来创建类对象的类的名称。它叫做**元类**。在这里，重要的是要区分从面向对象范式的角度来看继承和语言机制，它们使你能够实践这个范式。元类提供了这种机制。更令人困惑的是，元类能够像普通类一样继承父类。但这很快就会变成“自我反转”的编程，所以我们不要深入探讨。

我们是否需要每天处理这些元类？嗯，不需要。在少数情况下，你可能需要定义和使用它们，但大多数情况下，默认行为就足够了。

让我们继续我们的旅程，这次用一个新的示例：

```py
class Parent:
    def __init__(self, name, age):
        self.name = name
        self.age = age

parent = Parent('John', 35)
```

这应该是你在 Python 中的第一个面向对象编程步骤。你被教导 `__init__` 是一个构造函数，你在其中设置对象属性的值，然后就可以继续。然而，这个 `__init__` dunder 方法确实如其所言：初始化步骤。奇怪的是，你调用它来初始化一个对象，但却得到一个对象实例的返回值？那里没有 `return`，对吗？那怎么可能呢？是谁返回了类的实例？

很少有人在 Python 学习之初就知道还有另一个方法被隐式调用，名为 `__new__`。这个方法实际上在 `__init__` 被调用来初始化之前创建一个实例。这里是一个例子：

```py
class Parent:
    def __new__(cls, name, age):
        print('new is called')
        return super().__new__(cls)

    def __init__(self, name, age):
        print('init is called')
        self.name = name
        self.age = age

parent = Parent('John', 35)
# new is called
# init is called
```

你会立即看到 `__new__` 返回 `super().__new__(cls)`。这是一个新的实例。`super()` 获取 `Parent` 的父类，隐式为 `object` 类。这个类被 Python 中的所有类继承。它本身也是一个对象。Python 创造者的另一个创新举措！

```py
isinstance(object, object)
# True
```

但是，`__new__` 和 `__init__` 是如何绑定在一起的呢？当我们调用 `Parent('John' ,35)` 时，对象实例化的过程一定还有其他内容。再看一遍，你是在像调用函数一样调用一个类对象。

# Python 可调用

Python 作为一种结构化类型语言，使你能够在类中定义特定的方法，这些方法描述了一个 *协议*（使用其对象的方式），基于此，所有类的实例将按预期方式行为。如果你来自其他编程语言，不要感到畏惧。*协议* 就像其他语言中的 *接口*。但是，在这里我们并不明确声明我们正在实现一个特定的接口，因此也没有具体行为。我们只是实现由 *协议* 描述的方法，所有对象将具有协议的行为。其中一个 *协议* 是 *可调用的*。通过实现 dunder 方法 `__call__`，你使你的对象像函数一样被调用。看一下这个例子：

```py
class Parent:
    def __new__(cls, name, age):
        print('new is called')
        return super().__new__(cls)

    def __init__(self, name, age):
        print('init is called')
        self.name = name
        self.age = age

    def __call__(self):
        print('Parent here!')

parent = Parent('John', 35)
parent()
# Parent here!
```

通过在类定义中实现 `__call__`，你的类实例变得 *可调用*。但 `Parent('John', 35)` 怎么办呢？如何在你的类对象中实现相同的功能？如果对象的类型定义（类）指定该对象是 *可调用的*，那么类对象类型（即元类）也应该指定类对象是可调用的，对吗？`__new__` 和 `__init__` 的 dunder 方法在这里发生。

此时，是时候开始玩转元类了。

# Python 元类

有至少两种方法可以改变类对象的创建过程。一种是通过使用类装饰器；另一种是通过显式指定元类。我将描述元类的方法。请记住，元类看起来像一个普通的类，唯一的例外是它必须继承`type`类。为什么？因为`type`类包含了我们代码正常工作的所有实现。例如：

```py
class MyMeta(type):
    def __call__(self, *args, **kwargs):
        print(f'{self.__name__} is called'
              f' with args={args}, kwargs={kwarg}')

class Parent(metaclass=MyMeta):
    def __new__(cls, name, age):
        print('new is called')
        return super().__new__(cls)

    def __init__(self, name, age):
        print('init is called')
        self.name = name
        self.age = age

parent = Parent('John', 35)
# Parent is called with args=('John', 35), kwargs={}
type(parent)
# NoneType
```

在这里，`MyMeta`是新类对象实例化的驱动因素，同时指定了新类实例是如何创建的。仔细查看示例的最后两行。`parent`什么也没有！但为什么？因为，如你所见，`MyMeta.__call__`只是打印信息而不返回任何内容。明确地说，就是这样。隐含地说，它返回`None`，即`NoneType`。

我们应该如何解决这个问题？

```py
class MyMeta(type):

    def __call__(cls, *args, **kwargs):
        print(f'{cls.__name__} is called'
              f'with args={args}, kwargs={kwargs}')
        print('metaclass calls __new__')
        obj = cls.__new__(cls, *args, **kwargs)

        if isinstance(obj, cls):
            print('metaclass calls __init__')
            cls.__init__(obj, *args, **kwargs)

        return obj

class Parent(metaclass=MyMeta):

    def __new__(cls, name, age):
        print('new is called')
        return super().__new__(cls)

    def __init__(self, name, age):
        print('init is called')
        self.name = name
        self.age = age

parent = Parent('John', 35)
# Parent is called with args=('John', 35), kwargs={}
# metaclass calls __new__
# new is called
# metaclass calls __init__
# init is called

type(parent)
# Parent

str(parent)
# '<__main__.Parent object at 0x103d540a0>'
```

从输出中，你可以看到在`MyMeta.__call__`调用时发生了什么。提供的实现只是展示整个过程如何工作的一个例子。如果你打算自己重写元类的某些部分，你需要更加小心。有一些边缘情况需要你覆盖。例如，其中一个边缘情况是`Parent.__new__`可能返回一个不是`Parent`类实例的对象。在这种情况下，该对象不会被`Parent.__init__`方法初始化。这是你需要注意的预期行为，初始化一个不是同一类实例的对象实际上是没有意义的。

# 结论

这将总结定义类并创建实例时发生的简要概述。当然，你还可以进一步了解类块解释期间发生的事情。所有这些都发生在元类中。对大多数人来说，我们可能不需要创建和使用特定的元类，这真是幸运。然而，理解一切如何运作是有用的。我想引用一个类似的说法，适用于使用 NoSQL 数据库，内容大致如下：如果你不确定是否需要使用 Python 元类，你可能不需要。

# 参考资料

+   [Python 元类](https://jfreeman.dev/blog/2020/12/07/python-metaclasses/)

+   [理解 Python 中的对象实例化和元类](https://www.honeybadger.io/blog/python-instantiation-metaclass/)
