# 高级Python：函数

> 原文：[https://towardsdatascience.com/advanced-python-functions-3be6810f92d1?source=collection_archive---------1-----------------------#2023-08-01](https://towardsdatascience.com/advanced-python-functions-3be6810f92d1?source=collection_archive---------1-----------------------#2023-08-01)

[](https://medium.com/@ilija.lazarevic?source=post_page-----3be6810f92d1--------------------------------)[![Ilija Lazarevic](../Images/4a0d84af6d8fa97705ee35444d319b07.png)](https://medium.com/@ilija.lazarevic?source=post_page-----3be6810f92d1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3be6810f92d1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3be6810f92d1--------------------------------) [Ilija Lazarevic](https://medium.com/@ilija.lazarevic?source=post_page-----3be6810f92d1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe73ea2eae8e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-python-functions-3be6810f92d1&user=Ilija+Lazarevic&userId=e73ea2eae8e6&source=post_page-e73ea2eae8e6----3be6810f92d1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3be6810f92d1--------------------------------) ·11分钟阅读·2023年8月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3be6810f92d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-python-functions-3be6810f92d1&user=Ilija+Lazarevic&userId=e73ea2eae8e6&source=-----3be6810f92d1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3be6810f92d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvanced-python-functions-3be6810f92d1&source=-----3be6810f92d1---------------------bookmark_footer-----------)![](../Images/c4534173429b47e995ec7ff47f4c6c95.png)

如何让自己与Python纠缠在一起。照片由 [iam_os](https://unsplash.com/@iam_os?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/jst9zmbCD58?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

阅读标题后，你可能会问自己，“Python中的函数是高级概念吗？怎么会？所有课程都将函数作为语言的基础构件介绍。”你既对又错。

大多数Python课程将函数作为基本概念和构建块进行介绍，因为没有它们，你根本无法编写功能代码。这与函数式编程范式完全不同，后者是一个独立的概念，但我也会触及这一点。

在我们深入研究Python函数的复杂细节之前，让我们简要回顾一些基本概念和你可能已经知道的内容。

# 基础概念

所以你开始编写程序，在某个时刻你发现自己重复编写相同的代码序列。你开始重复自己和代码块。这证明这是引入函数的好时机和地方。至少，在Python中定义函数的方法是：

```py
def shout(name):
    print(f'Hey! My name is {name}.')
```

在软件工程领域，我们对函数定义的各个部分进行区分：

+   `def` - 用于定义函数的Python关键字。

+   `shout` - 函数名。

+   `shout(name)` - 函数声明。

+   `name` - 函数参数。

+   `print(...)` 是函数体的一部分，或者我们称之为函数定义。

函数可以返回一个值或根本没有返回值，就像我们之前定义的那样。当函数返回值时，它可以返回一个或多个值：

```py
def break_sentence(sentence):
    return sentence.split(' ')
```

你得到的结果是一个元组，你可以解包或者选择其中的任何一个元组元素继续操作。

对于那些不知情的人，Python中的函数是**一等公民**。这意味着什么？这意味着你可以像操作其他变量一样操作函数。你可以将它们作为参数传递给其他函数，从函数中返回它们，甚至将它们存储在变量中。这是一个例子：

```py
def shout(name):
    return f'Hey! My name is {name}.'

# we will use break_sentence defined above

# assign function to another variable
another_breaker = break_sentence 

another_breaker(shout('John'))
# ['Hey!', 'My', 'name', 'is', 'John.']

# Woah! Yes, this is a valid way to define function
name_decorator = lambda x: '-'.join(list(name))

name_decorator('John')
# 'J-o-h-n'
```

等等，这个`lambda`是什么？这是一种你可以在Python中定义函数的方式。它被称为无名函数或匿名函数。在这个例子中，我们将其赋值给一个名为`name_decorator`的变量，但你也可以将`lambda`表达式作为另一个函数的参数传递而无需给它命名。我会稍后详细讲解。

剩下的就是给出如何将函数作为参数传递或作为另一个函数的返回值的例子。这是我们迈向高级概念的部分，所以请耐心等待。

```py
def dash_decorator(name):
    return '-'.join(list(name))

def no_decorator(name):
    return name

def shout(name, decorator=no_decorator):
    decorated_name = decorator(name)
    return f'Hey! My name is {decorated_name}'

shout('John')
# 'Hey! My name is John'

shout('John', decorator=dash_decorator)
# 'Hey! My name is J-o-h-n'
```

这就是将函数作为参数传递给另一个函数的样子。那么`lambda`函数呢？请看下一个例子：

```py
def shout(name, decorator=lambda x: x):
    decorated_name = decorator(name)
    return f'Hey! My name is {decorated_name}'

print(shout('John'))
# Hey! My name is John

print(shout('John', decorator=dash_decorator))
# Hey! My name is J-o-h-n
```

现在默认的装饰函数是`lambda`，它返回参数的值（幂等）。在这里，它是匿名的，因为没有附加名称。

注意`print`也是一个函数，我们将函数`shout`作为参数传递给它。从本质上讲，我们是在链接函数。这可以引导我们进入函数式编程范式，这是一种你可以在Python中选择的路径。我会尝试写另一篇专门讨论这个主题的博客文章，因为它对我非常有趣。现在，我们将继续使用过程式编程范式，也就是我们将继续我们目前的做法。

如前所述，函数可以赋值给变量，可以作为另一个函数的参数，也可以从该函数返回。我给你展示了一些前两种情况的简单示例，但函数从函数中返回呢？起初我想保持简单，但毕竟这是高级Python！

# 中级或高级部分

这绝不会是**终极**的Python函数和函数相关高级概念指南。有很多很棒的材料，我会在这篇文章的最后留给你。然而，我想讨论几个我发现非常有趣的方面。

Python中的函数是**对象**。我们怎么知道这一点呢？每个Python中的对象都是一个类的实例，最终继承自一个特定的类叫做`type`。这其中的细节很复杂，但为了能够理解这与函数的关系，这里有一个例子：

```py
type(shout)
# function

type(type(shout))
# type
```

当你在Python中定义一个类时，它会自动继承`object`类。而`object`类继承自哪个类呢？

```py
type(object)
# type
```

我应该告诉你Python中的类也是对象吗？确实，这对初学者来说令人震惊。但正如Andrew Ng所说，这并不是那么重要，不用太担心。

好的，所以函数是对象。函数应该有一些魔法方法，对吧？

```py
shout.__class__
# function

shout.__name__
# shout

shout.__call__
# <method-wrapper '__call__' of function object at 0x10d8b69e0>
# Oh snap!
```

魔法方法`__call__`定义在可调用的对象上。所以我们的`shout`对象（函数）是可调用的。我们可以*调用*它，带有或不带有参数。但这很有趣。我们之前所做的是定义一个`shout`函数，并得到一个可调用的对象，其`__call__`魔法方法是一个函数。你看过《盗梦空间》电影吗？

所以，我们的*函数*实际上不是一个函数，而是一个对象。对象是类的实例，包含方法和属性，对吧？这是你从面向对象编程（OOP）中应该知道的。我们如何找出对象的属性是什么？有一个叫做`vars`的Python函数，它返回一个包含对象属性及其值的字典。让我们看看下一个例子会发生什么：

```py
vars(shout)
# {}

shout.name = 'Jimmy'

vars(shout)
# {'name': 'Jimmy'}
```

这很有趣。你可能一时无法直接找到这种用例。即便你找到了，我也强烈不建议你使用这种黑魔法。尽管这很有趣，但不易理解。我向你展示这一点是因为我们想证明函数确实是对象。记住，Python中的一切都是对象。这就是Python的方式。

现在，期待已久的函数正在返回。这个概念也很有趣，因为它提供了很多实用性。稍微加点语法糖，你会变得非常表达丰富。让我们深入了解。

首先，函数的定义可以包含另一个函数的定义，甚至不止一个。这是一个完全有效的例子：

```py
def shout(name):
    def _upper_case(s):
        return s.upper()

    return _upper_case(name)
```

如果你认为这只是`name.upper()`的复杂版，你是对的。但等等，我们快到了。

因此，根据前面的例子，这是完全可用的Python代码，你可以尝试在函数内部定义多个函数。这种巧妙的技巧有什么价值？嗯，你可能会遇到你的函数非常庞大且有重复的代码块。这样，定义一个*子函数*可以增加可读性。在实际应用中，巨大的函数是代码异味的迹象，强烈建议将其拆分成几个较小的函数。因此，遵循这个建议，你很少需要在函数内部定义多个函数。需要注意的一点是，`_upper_case`函数是隐藏的，无法在`shout`函数定义和调用的范围内访问。这种方法的另一个问题是，你无法轻易测试它。

然而，有一种特定情况，在另一个函数内部定义函数是一个可行的方法。这就是当你实现函数的装饰器时。这与我们之前例子中用于装饰`name`字符串的函数无关。

# Python中的装饰器函数

什么是*装饰器函数*？可以把它看作是一个包装你函数的函数。这样做的目标是给一个已经存在的函数添加额外的功能。例如，假设你想记录每次调用你的函数时：

```py
def my_function():
    return sum(range(10))

def my_logger(fun):
    print(f'{fun.__name__} is being called!')
    return fun

my_function()
# 45

my_logger(my_function)
# my_function is being called!
# <function my_function at 0x105afbeb0>

my_logger(my_function)()
# my_function is being called!
# 45
```

注意我们如何装饰我们的函数；我们将它作为参数传递给装饰函数。但这还不够！记住，装饰器返回函数，这个函数需要被调用（执行）。这就是最后一次调用的作用。

现在，在实际操作中，你真正想要的是装饰保持在原函数的名称下。在我们的例子中，我们希望在解释器解析我们的代码后，`my_function`是装饰后的函数的名称。这样，我们保持简单易懂，同时确保代码的任何部分都无法调用未装饰版本的函数。示例：

```py
def my_function():
    return sum(range(10))

def my_logger(fun):
    print(f'{fun.__name__} is being called!')
    return fun

my_function = my_logger(my_function)

my_function(10)
# my_function is being called!
# 45
```

你会承认，将函数名称重新分配为装饰后的名称这一部分确实很麻烦。你必须记住这一点。如果有很多函数调用需要记录，那么会有大量重复的代码。这就是语法糖派上用场的时候。装饰器函数定义之后，你可以通过在函数定义前加上`@`和装饰器函数的名称来装饰另一个函数。示例：

```py
def my_logger(fun):
    print(f'{fun.__name__} is being called!')
    return fun

@my_logger
def my_function():
    return sum(range(10))

my_function()
# my_function is being called!
# 45
```

这是Python的禅宗。看看代码的表达力和简洁性。

这里有一点重要的事情需要注意！尽管输出是合理的，但这不是你所期望的！在加载你的Python代码时，解释器将调用`my_logger`函数并有效地运行它！你会得到日志输出，但这将不是我们最初想要的结果。现在看看代码：

```py
def my_logger(fun):
    print(f'{fun.__name__} is being called!')
    return fun

@my_logger
def my_function():
    return sum(range(10))

my_function()
# my_function is being called!
# 45
my_function()
# 45
```

为了在调用原始函数时能够运行装饰器代码，我们必须将其包裹在另一个函数中。这是事情可能变得混乱的地方。这是一个示例：

```py
def my_logger(fun):
    def _inner_decorator(*args, **kwargs):
        print(f'{fun.__name__} is being called!')
        return fun(*args, **kwargs)

    return _inner_decorator

@my_logger
def my_function(n):
    return sum(range(n))

print(my_function(5))
# my_function is being called!
# 10
```

在这个示例中，还有一些更新，我们来逐一查看：

1.  我们希望能够将参数传递给`my_function`。

1.  我们希望能够装饰任何函数，而不仅仅是`my_function`。因为我们不知道未来函数的确切参数数量，我们必须尽可能保持通用，这就是为什么我们使用`*args`和`**kwargs`。

1.  最重要的是，我们定义了`_inner_decorator`，它将在我们在代码中调用`my_function`时被调用。它接受位置参数和关键字参数，并将它们作为参数传递给装饰的函数。

始终记住，装饰器函数必须返回一个接受相同参数（数量及其类型）并返回相同输出（数量及其类型）的函数。也就是说，如果你想让函数用户不感到困惑，代码阅读者也不需要搞清楚发生了什么。

比如说，你有两个结果不同但也需要参数的函数：

```py
@my_logger
def my_function(n):
    return sum(range(n))

@my_logger
def my_unordinary_function(n, m):
    return sum(range(n)) + m

print(my_function(5))
# my_function is being called!
# 10

print(my_unordinary_function(5, 1))
# my_unordinary_function is being called!
# 11
```

在我们的示例中，装饰器函数只接受它装饰的函数。但是，如果你想传递额外的参数并动态改变装饰器行为呢？比如说，你想调整日志记录器装饰器的详细程度。到目前为止，我们的装饰器函数接受了一个参数：它装饰的函数。然而，当装饰器函数有自己的参数时，这些参数会首先传递给它。然后，装饰器函数必须返回一个接受被装饰函数的函数。基本上，事情变得更加复杂了。还记得电影《盗梦空间》的引用吗？

这是一个示例：

```py
 from enum import IntEnum, auto
from datetime import datetime
from functools import wraps

class LogVerbosity(IntEnum):
    ZERO = auto()
    LOW = auto()
    MEDIUM = auto()
    HIGH = auto()

def my_logger(verbosity: LogVerbosity):

    def _inner_logger(fun):

        def _inner_decorator(*args, **kwargs):
            if verbosity >= LogVerbosity.LOW:
                print(f'LOG: Verbosity level: {verbosity}')
                print(f'LOG: {fun.__name__} is being called!')
            if verbosity >= LogVerbosity.MEDIUM:
                print(f'LOG: Date and time of call is {datetime.utcnow()}.')
            if verbosity == LogVerbosity.HIGH:
                print(f'LOG: Scope of the caller is {__name__}.')
                print(f'LOG: Arguments are {args}, {kwargs}')

            return fun(*args, **kwargs)

        return _inner_decorator

    return _inner_logger

@my_logger(verbosity=LogVerbosity.LOW)
def my_function(n):
    return sum(range(n))

@my_logger(verbosity=LogVerbosity.HIGH)
def my_unordinary_function(n, m):
    return sum(range(n)) + m

print(my_function(10))
# LOG: Verbosity level: LOW
# LOG: my_function is being called!
# 45

print(my_unordinary_function(5, 1))
# LOG: Verbosity level: HIGH
# LOG: my_unordinary_function is being called!
# LOG: Date and time of call is 2023-07-25 19:09:15.954603.
# LOG: Scope of the caller is __main__.
# LOG: Arguments are (5, 1), {}
# 11 
```

我不会详细描述与装饰器无关的代码，但我鼓励你查阅相关资料进行学习。在这里，我们有一个装饰器，它以不同的详细程度记录函数调用。如前所述，`my_logger` 装饰器现在接受动态改变其行为的参数。参数传递后，它返回的结果函数应该接受一个需要装饰的函数。这就是`_inner_logger`函数。到现在为止，你应该了解装饰器代码的其他部分在做什么。

# 结论

我最初的想法是写一些像Python中的装饰器这样的高级主题。然而，正如你现在可能已经知道的，我还提到和使用了许多其他高级主题。在未来的帖子中，我会在一定程度上讨论其中一些。然而，我的建议是去其他来源学习这里提到的内容。如果你在任何编程语言中开发，理解这些函数是必须的，但掌握你所选择的编程语言的所有方面可以极大地提高你编写代码的能力。

希望我为你介绍了一些新内容，并且你现在对作为高级 Python 程序员编写函数感到自信。

# 参考资料

+   [Python 装饰器入门](https://realpython.com/primer-on-python-decorators/)

+   [Python 内部函数：它们有什么用？](https://realpython.com/inner-functions-what-are-they-good-for/)

+   [枚举 HOWTO](https://docs.python.org/3/howto/enum.html)
