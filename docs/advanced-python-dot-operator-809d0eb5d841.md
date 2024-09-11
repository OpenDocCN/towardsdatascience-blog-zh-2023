# 高级 Python：点运算符

> 原文：[`towardsdatascience.com/advanced-python-dot-operator-809d0eb5d841?source=collection_archive---------0-----------------------#2023-10-20`](https://towardsdatascience.com/advanced-python-dot-operator-809d0eb5d841?source=collection_archive---------0-----------------------#2023-10-20)

## 这个运算符使 Python 中的面向对象范式成为可能

[](https://medium.com/@ilija.lazarevic?source=post_page-----809d0eb5d841--------------------------------)![Ilija Lazarevic](https://medium.com/@ilija.lazarevic?source=post_page-----809d0eb5d841--------------------------------)[](https://towardsdatascience.com/?source=post_page-----809d0eb5d841--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----809d0eb5d841--------------------------------) [Ilija Lazarevic](https://medium.com/@ilija.lazarevic?source=post_page-----809d0eb5d841--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe73ea2eae8e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-python-dot-operator-809d0eb5d841&user=Ilija+Lazarevic&userId=e73ea2eae8e6&source=post_page-e73ea2eae8e6----809d0eb5d841---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----809d0eb5d841--------------------------------) ·13 分钟阅读·2023 年 10 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F809d0eb5d841&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-python-dot-operator-809d0eb5d841&user=Ilija+Lazarevic&userId=e73ea2eae8e6&source=-----809d0eb5d841---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F809d0eb5d841&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-python-dot-operator-809d0eb5d841&source=-----809d0eb5d841---------------------bookmark_footer-----------)![](img/841c96a738ad1ab6ce24a4c82866859e.png)

点运算符是 Python 中面向对象范式的支柱之一。照片来源：[Madeline Pere](https://unsplash.com/@mpere?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 在 [Unsplash](https://unsplash.com/photos/r37QcATSbD4?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

这次，我将讨论一个看似微不足道的东西，那就是“点操作符”。你们中的大多数人已经多次使用了这个操作符，却不知道或未曾质疑其背后的工作原理。与我上次讨论的[元类](https://medium.com/towards-data-science/advanced-python-metaclasses-e32d46e0ebe3)概念相比，这个操作符在日常任务中稍显实用。开个玩笑，你在使用 Python 做更多事情时几乎每次都会用到它。这正是我认为你们可能想深入了解的原因，我愿意做你的向导。让我们开始这段旅程吧！

我将从一个简单的问题开始：“什么是‘点操作符’？”

这里有一个例子：

```py
hello = 'Hello world!'

print(hello.upper())
# HELLO WORLD!
```

好吧，这确实是一个“Hello World”示例，但我很难想象有人会像这样开始教你 Python。无论如何，“点操作符”就是`hello.upper()`中的“.”部分。让我们试着给出一个更详细的例子：

```py
class Person:

    num_of_persons = 0

    def __init__(self, name):
        self.name = name

    def shout(self):
        print(f"Hey! I'm {self.name}")

p = Person('John')
p.shout()
# Hey I'm John.

p.num_of_persons
# 0

p.name
# 'John'
```

你在几个地方使用“点操作符”。为了更清楚地了解整体情况，让我们总结一下你在两个情况下如何使用它：

+   用它来访问对象或类的属性，

+   用它来访问在类定义中定义的函数。

显然，我们的例子中都包含了这些内容，看起来直观且符合预期。但事情远不止于此！仔细看看这个例子：

```py
p.shout
# <bound method Person.shout of <__main__.Person object at 0x1037d3a60>>

id(p.shout)
# 4363645248

Person.shout
# <function __main__.Person.shout(self)>

id(Person.shout)
# 4364388816
```

不知为何，`p.shout` 并未引用与 `Person.shout` 相同的函数，尽管它应该如此。至少你会期待它这样做，对吧？而且 `p.shout` 甚至不是一个函数！在我们开始讨论发生了什么之前，让我们先看下一个例子：

```py
class Person:

    num_of_persons = 0

    def __init__(self, name):
        self.name = name

    def shout(self):
        print(f"Hey! I'm {self.name}.")

p = Person('John')

vars(p)
# {'name': 'John'}

def shout_v2(self):
    print("Hey, what's up?")

p.shout_v2 = shout_v2

vars(p)
# {'name': 'John', 'shout_v2': <function __main__.shout_v2(self)>}

p.shout()
# Hey, I'm John.

p.shout_v2()
# TypeError: shout_v2() missing 1 required positional argument: 'self'
```

对于不了解 `vars` 函数的人，它返回一个字典，该字典包含实例的属性。如果你运行 `vars(Person)`，你会得到一个稍有不同的响应，但你会明白整体情况。字典中会包含属性及其值，还有持有类函数定义的变量。显然，类的实例对象与类对象本身存在差异，因此这两个对象的 `vars` 函数响应也会有所不同。

现在，在对象创建之后额外定义一个函数是完全有效的。这就是 `p.shout_v2 = shout_v2` 这一行。这确实在实例字典中引入了另一个键值对。看起来一切正常，我们能够顺利运行，就像 `shout_v2` 是在类定义中指定的那样。但可惜的是！确实出现了问题。我们无法像调用 `shout` 方法那样调用它。

聪明的读者现在应该已经注意到我如何谨慎地使用*函数*和*方法*这两个术语。毕竟，Python 打印这些术语的方式也是不同的。看看之前的例子。`shout` 是一个方法，而 `shout_v2` 是一个函数。至少从对象 `p` 的角度来看是这样的。如果从 `Person` 类的角度来看，`shout` 是一个函数，而 `shout_v2` 并不存在。它仅在对象的字典（命名空间）中定义。因此，如果你真的要依赖面向对象的范式和机制，比如封装、继承、抽象和多态，你不会在对象上定义函数，就像 `p` 在我们的例子中那样。你会确保在类定义（主体）中定义函数。

那么这两者为什么不同，我们为什么会得到错误？最快的答案是因为“点操作符”的工作方式。较长的答案是背后有一个机制为你进行（属性）名称解析。这个机制由 `__getattribute__` 和 `__getattr__` 双下划线方法组成。

# 获取属性

起初，这可能听起来不直观而且复杂，但请耐心点。从本质上讲，当你尝试访问 Python 对象的属性时，有两种情况会发生：要么存在属性，要么不存在。简单来说。在这两种情况下，`__getattribute__` 会被调用，或者为了让你更容易理解，它**总是会被调用**。这个方法：

+   返回计算出的属性值，

+   显式调用 `__getattr__`，或者

+   在这种情况下会引发 `AttributeError`，此时会默认调用 `__getattr__`。

如果你想拦截解析属性名称的机制，这就是可以劫持的地方。你只需要小心，因为很容易陷入无限循环或搞砸整个名称解析机制，特别是在面向对象的继承场景中。这并不像看起来那样简单。

如果你想处理对象字典中没有属性的情况，你可以直接实现 `__getattr__` 方法。当 `__getattribute__` 无法访问属性名称时，这个方法会被调用。如果这个方法也找不到属性或处理不了缺失的属性，它也会引发 `AttributeError` 异常。你可以这样玩弄这些方法：

```py
class Person:

    num_of_persons = 0

    def __init__(self, name):
        self.name = name

    def shout(self):
        print(f"Hey! I'm {self.name}.")

    def __getattribute__(self, name):
        print(f'getting the attribute name: {name}')
        return super().__getattribute__(name)

    def __getattr__(self, name):
        print(f'this attribute doesn\'t exist: {name}')
        raise AttributeError()

p = Person('John')

p.name
# getting the attribute name: name
# 'John'

p.name1
# getting the attribute name: name1
# this attribute doesn't exist: name1
#
# ... exception stack trace
# AttributeError:
```

在你实现 `__getattribute__` 时调用 `super().__getattribute__(...)` 是非常重要的，原因正如我之前所写的，因为 Python 的默认实现中发生了很多事情。这正是“点操作符”魔力的来源之一。好吧，至少有一半的魔力在这里。另一部分在于类定义被解释后类对象的创建方式。

# 类函数

我使用的术语是有目的的。类确实仅包含*函数*，我们在最初的例子中已经看到这一点。

```py
p.shout
# <bound method Person.shout of <__main__.Person object at 0x1037d3a60>>

Person.shout
# <function __main__.Person.shout(self)>
```

从对象的角度来看，这些被称为方法。将类的函数转换为对象的方法的过程叫做绑定，结果就是你在前面的示例中看到的一个*绑定方法*。是什么让它变成*绑定*，以及绑定到什么？好吧，一旦你有了一个类的实例并开始调用它的方法，实际上你是在将对象引用传递给每个方法。记住`self`参数吗？那么，这个过程是怎么发生的，谁来做呢？

好吧，第一部分发生在类体被解释的时候。这个过程中发生了很多事情，比如定义类的命名空间、向其中添加属性值、定义（类）函数，并将它们绑定到它们的名称上。现在，当这些函数被定义时，它们会以某种方式被包装。以一种名为***描述符***的对象进行包装。这个*描述符*使得我们之前看到的类函数的标识和行为发生变化。我会确保单独写一篇关于*描述符*的博客文章，但现在，请知道这个对象是一个实现了一组预定义的双下划线方法的类的实例。这也被称为*协议*。一旦这些实现了，就可以说这个类的对象*遵循*特定的协议，从而表现出预期的行为。**数据**描述符和**非数据**描述符之间有区别。前者实现了`__get__`、`__set__`和/或`__delete__`双下划线方法。后者只实现了`__get__`方法。无论如何，类中的每个函数最终都会被包装在所谓的**非数据**描述符中。

一旦你使用“点操作符”启动属性查找，`__getattribute__`方法就会被调用，整个名称解析过程开始。这一过程在解析成功时停止，流程大致如下：

1.  返回具有所需名称的数据描述符（类级别），或者

1.  返回具有所需名称的实例属性（实例级别），或者

1.  返回具有所需名称的非数据描述符（类级别），或者

1.  返回具有所需名称的类属性（类级别），或者

1.  引发`AttributeError`，实际上调用了`__getattr__`方法。

我最初的想法是给你提供一个关于这个机制如何实现的官方文档的参考，至少是一个 Python 模拟版，供学习使用，但我决定也帮助你完成这部分内容。然而，我强烈建议你去阅读官方文档的整页内容。

所以，在下一个代码片段中，我会在注释中放一些描述，以便更容易阅读和理解代码。这里是：

```py
def object_getattribute(obj, name):
    "Emulate PyObject_GenericGetAttr() in Objects/object.c"
    # Create vanilla object for later use.
    null = object()

    """
    obj is an object instantiated from our custom class. Here we try 
    to find the name of the class it was instantiated from.
    """
    objtype = type(obj) 

    """
    name represents the name of the class function, instance attribute, 
    or any class attribute. Here, we try to find it and keep a 
    reference to it. MRO is short for Method Resolution Order, and it 
    has to do with class inheritance. Not really that important at 
    this point. Let's say that this mechanism optimally finds name
    through all parent classes.
    """
    cls_var = find_name_in_mro(objtype, name, null)

    """
    Here we check if this class attribute is an object that has the 
    __get__ method implemented. If it does,  it is a non-data 
    descriptor. This is important for further steps.
    """
    descr_get = getattr(type(cls_var), '__get__', null)

    """
    So now it's either our class attribute references a descriptor, in
    which case we test to see if it is a data descriptor and we 
    return reference to the descriptor's __get__ method, or we go to 
    the next if code block.
    """
    if descr_get is not null:
        if (hasattr(type(cls_var), '__set__')
            or hasattr(type(cls_var), '__delete__')):
            return descr_get(cls_var, obj, objtype)  # data descriptor

    """
    In cases where the name doesn't reference a data descriptor, we 
    check to see if it references the variable in the object's 
    dictionary, and if so, we return its value.
    """
    if hasattr(obj, '__dict__') and name in vars(obj):
        return vars(obj)[name]  # instance variable

    """
    In cases where the name does not reference the variable in the 
    object's dictionary, we try to see if it references a non-data 
    descriptor and return a reference to it.    
    """
    if descr_get is not null:
        return descr_get(cls_var, obj, objtype)  # non-data descriptor

    """
    In case name did not reference anything from above, we try to see 
    if it references a class attribute and return its value.
    """
    if cls_var is not null:
        return cls_var                                  # class variable

    """
    If name resolution was unsuccessful, we throw an AttriuteError 
    exception, and __getattr__ is being invoked.
    """
    raise AttributeError(name)
```

请记住，这个实现是用 Python 编写的，目的是为了记录和描述在 `__getattribute__` 方法中实现的逻辑。实际上，它是用 C 实现的。仅通过查看代码，你可以想象最好不要尝试重新实现整个过程。最好的方法是自己尝试解决部分问题，然后参考 CPython 实现中的 `return super().__getattribute__(name)`，如上例所示。

这里重要的是，每个类函数（它是一个对象）都被包装在一个非数据描述符（即 `function` 类对象）中，这意味着这个包装对象定义了 `__get__` 双下划线方法。这个双下划线方法的作用是返回一个新的可调用对象（可以把它看作是一个新函数），其中第一个参数是我们正在执行“点操作符”的对象的引用。我说可以把它看作是一个新函数，因为它是 *可调用* 的。本质上，它是另一个名为 `MethodType` 的对象。看看这个：

```py
type(p.shout)
# getting the attribute name: shout
# method

type(Person.shout)
# function
```

一个有趣的事情确实是这个 `function` 类。这个类正是定义 `__get__` 方法的包装对象。然而，一旦我们尝试通过“点操作符”访问它作为方法 `shout`，`__getattribute__` 会遍历列表并停在第三种情况（返回非数据描述符）。这个 `__get__` 方法包含了额外的逻辑，它获取对象的引用并创建一个 `MethodType`，其中包含对 `function` 和对象的引用。

这是官方文档的模型：

```py
class Function:
    ...

    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return MethodType(self, obj)
```

忽略类名的不同。我一直使用 `function` 而不是 `Function` 以便于理解，但从现在开始我会使用 `Function` 名称，以便它与官方文档的解释一致。

无论如何，仅通过查看这个模型，可能已经足以理解 `function` 类如何融入整体，但让我再添加几行缺失的代码，这将可能使事情更加清晰。我将在这个示例中再添加两个类函数，即：

```py
class Function:
    ...

    def __init__(self, fun, *args, **kwargs):
        ...
        self.fun = fun

    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return MethodType(self, obj)

    def __call__(self, *args, **kwargs):
        ...
        return self.fun(*args, **kwargs)
```

我为什么添加这些函数？实际上，你现在可以很容易地想象 `Function` 对象在这个方法绑定的整个场景中所扮演的角色。这个新的 `Function` 对象将原始函数作为一个属性存储。这个对象也是 *可调用* 的，这意味着我们可以像调用函数一样调用它。在这种情况下，它的工作方式和它包装的函数一样。记住，Python 中的一切都是对象，包括函数。而 `MethodType` ‘包装’ 了 `Function` 对象以及对我们调用方法的对象的引用（在我们的例子中是 `shout`）。

`MethodType` 是如何做到这一点的？实际上，它保持这些引用并实现了一个可调用协议。这里是 `MethodType` 类的官方文档模型：

```py
class MethodType:

    def __init__(self, func, obj):
        self.__func__ = func
        self.__self__ = obj

    def __call__(self, *args, **kwargs):
        func = self.__func__
        obj = self.__self__
        return func(obj, *args, **kwargs)
```

为了简洁起见，`func` 最终引用了我们最初的类函数（`shout`），`obj` 引用了实例（`p`），然后我们还有传递的参数和关键字参数。`self` 在 `shout` 声明中最终引用了这个`obj`，在我们的示例中本质上是 `p`。

最终，应该清楚为什么我们区分函数和方法，以及函数如何在通过对象使用“点操作符”访问时被绑定。如果你考虑一下，我们完全可以通过以下方式调用类函数：

```py
class Person:

    num_of_persons = 0

    def __init__(self, name):
        self.name = name

    def shout(self):
        print(f"Hey! I'm {self.name}.")

p = Person('John')

Person.shout(p)
# Hey! I'm John.
```

然而，这真的不是推荐的方法，只是纯粹的丑陋。通常，你在代码中不需要这样做。

所以，在总结之前，我想回顾几个属性解析的例子，以便更容易理解。让我们使用之前的例子，搞清楚点操作符是如何工作的。

```py
p.name
"""
1\. __getattribute__ is invoked with p and "name" arguments.

2\. objtype is Person.

3\. descr_get is null because the Person class doesn't have 
   "name" in its dictionary (namespace).

4\. Since there is no descr_get at all, we skip the first if block.

5\. "name" does exist in the object's dictionary so we get the value.
"""

p.shout('Hey')
"""
Before we go into name resolution steps, keep in mind that 
Person.shout is an instance of a function class. Essentially, it gets 
wrapped in it. And this object is callable, so you can invoke it with 
Person.shout(...). From a developer perspective, everything works just
as if it were defined in the class body. But in the background, it 
most certainly is not.

1\. __getattribute__ is invoked with p and "shout" arguments.

2\. objtype is Person.

3\. Person.shout is actually wrapped and is a non-data descriptor.
So this wrapper does have the __get__ method implemented, and it
gets referenced by descr_get.

4\. The wrapper object is a non-data descriptor, so the first if block 
   is skipped.

5\. "shout" doesn't exist in the object's dictionary because it is part 
   of class definition. Second if block is skipped.

6\. "shout" is a non-data descriptor, and its __get__ method is returned
   from the third if code block.

Now, here we tried accessing p.shout('Hey'), but what we did get is
p.shout.__get__ method. This one returns a MethodType object. Because
of this p.shout(...) works, but what ends up being called is an 
instance of the MethodType class. This object is essentially a wrapper
around the `Function` wrapper, and it holds reference to the `Function`
wrapper and our object p. In the end, when you invoke p.shout('Hey'), 
what ends up being invoked is `Function` wrapper with p object, and 
'Hey' as one of the positional arguments.
"""

Person.shout(p)
"""
Before we go into name resolution steps, keep in mind that 
Person.shout is an instance of a function class. Essentially, it gets 
wrapped in it. And this object is callable, so you can invoke it with 
Person.shout(...). From a developer perspective, everything works just
as if it were defined in the class body. But in the background, it 
most certainly is not.

This part is the same. The following steps are different. Check
it out.

1\. __getattribute__ is invoked with Person and "shout" arguments.

2\. objtype is a type. This mechanism is described in my post on
metaclasses.

3\. Person.shout is actually wrapped and is a non-data descriptor,
so this wrapper does have the __get__ method implemented, and it
gets referenced by descr_get.

4\. The wrapper object is a non-data descriptor, so first if block is 
   skipped.

5\. "shout" does exist in an object's dictionary because Person is
   object after all. So the "shout" function is returned.

When Person.shout is invoked, what actually gets invoked is an instance
of the `Function` class, which is also callable and wrapper around the
original function defined in the class body. This way, the original 
function gets called with all positional and keyword arguments.
"""
```

# 结论

如果一次性阅读这篇文章不是一件容易的事情，请不要担心！“点操作符”背后的整个机制并不容易理解。至少有两个原因，一个是 `__getattribute__` 如何进行名称解析，另一个是类函数如何在类体解释时被封装。所以，确保你多读几遍这篇文章，并尝试例子。实验正是促使我开始一个叫做高级 Python 的系列的原因。

还有一件事！如果你喜欢我讲解的方式，并且有 Python 世界中的一些高级内容你想了解，喊一声！

高级 Python 系列的上一篇文章：

[高级 Python 函数](https://towardsdatascience.com/advanced-python-functions-3be6810f92d1?source=post_page-----809d0eb5d841--------------------------------) [## 高级 Python：函数

### 阅读完标题后，你可能会问自己类似“Python 中的函数是高级的……”这样的问题。

[高级 Python 函数](https://towardsdatascience.com/advanced-python-functions-3be6810f92d1?source=post_page-----809d0eb5d841--------------------------------) [高级 Python 元类](https://towardsdatascience.com/advanced-python-metaclasses-e32d46e0ebe3?source=post_page-----809d0eb5d841--------------------------------) [## 高级 Python：元类

### 对 Python 类对象及其创建方法的简要介绍

[高级 Python 元类](https://towardsdatascience.com/advanced-python-metaclasses-e32d46e0ebe3?source=post_page-----809d0eb5d841--------------------------------)

# 参考文献

+   [从实例中调用](https://docs.python.org/3/howto/descriptor.html#invocation-from-an-instance)

+   [函数和方法](https://docs.python.org/3/howto/descriptor.html#id25)
