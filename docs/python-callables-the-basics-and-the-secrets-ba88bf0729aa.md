# Python 可调用对象：基础和秘密

> 原文：[`towardsdatascience.com/python-callables-the-basics-and-the-secrets-ba88bf0729aa`](https://towardsdatascience.com/python-callables-the-basics-and-the-secrets-ba88bf0729aa)

## PYTHON 编程

## 了解 Python 可调用对象的强大功能。

[](https://medium.com/@nyggus?source=post_page-----ba88bf0729aa--------------------------------)![Marcin Kozak](https://medium.com/@nyggus?source=post_page-----ba88bf0729aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ba88bf0729aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba88bf0729aa--------------------------------) [Marcin Kozak](https://medium.com/@nyggus?source=post_page-----ba88bf0729aa--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ba88bf0729aa--------------------------------) ·10 分钟阅读·2023 年 10 月 27 日

--

![](img/16289801bda2c8ec8dfa0b3a3bba76f4.png)

在 Python 中，有许多可调用对象可以选择。照片由 [Pavan Trikutam](https://unsplash.com/@ptrikutam?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在编程语言中，可调用对象通常与函数相关联，这也是有充分理由的。函数可能是可调用对象的最佳示例，但它们并不是唯一的。在 Python 中，还有许多其他可调用类型，它们可以非常有用且强大。你还可以创建自己的可调用对象。本文将讨论这两者。

*可调用对象*是指可以通过一对括号来调用的对象，例如下面的例子，我们使用了内置函数 `sum()`：

```py
>>> sum([1, 1, 1])
3
```

对可调用对象的调用，根据其定义，可能是

+   没有任何参数，如 `no_args_callable()`

+   或一系列位置参数和/或关键字参数，如 `args_callable(arg1, arg2)`、`args_callable(arg1, arg2=value2)` 或 `args_callable(arg1=value1, arg2=value2)`

上述中，我将可调用对象描述为一个名词。然而，*可调用对象*一词也可以用作形容词，意思是 *作为一个可调用对象*。因此，可调用对象与可调用对象是相同的。

Python 有一个内置函数 `callable()`，用于检查一个对象是否是可调用的，或者换句话说，是否是一个可调用对象。请考虑以下实际的可调用对象示例：

```py
>>> callable(lambda x: x + 1)
True
>>> callable(print)
True
>>> def foo(): ...
>>> callable(foo)
True
```

下面的对象不是可调用的：

```py
>>> callable(None)
False
>>> callable(10)
False
>>> callable("hello")
False
```

上述正面示例是关于函数的，这也是大多数人对可调用对象的主要关联。然而，实际上，每个 Python 类都是可调用的。如果你了解 Python 中面向对象编程的基础知识，你会知道要创建一个类的实例，你需要执行以下操作¹

```py
>>> class Empty: ...
```

这看起来完全像是一次调用，实际上确实如此——这就是为什么 Python 类是可调用的原因。

这段代码显示了`Empty`类是可调用的，但事实上，每一个 Python 类都是可调用的。然而，在 Python 术语中，“可调用类”通常用于表示不同的东西：其实例是可调用的类。

我们的`Empty`类是可调用的，但其实例不是：

```py
>>> empty_instance = Empty()
>>> empty_instance()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'Empty' object is not callable
```

这段代码抛出了`TypeError`，因为`Empty`类的实例不可调用：

```py
>>> callable(empty_instance)
False
```

要使类的实例可调用，你需要实现`.__call__()`方法。我们在下面做到了这一点，尽管该方法是空的——它什么也不做：

```py
>>> class EmptyCallable:
...     def __call__(self): ...
>>> empty_call_instance = Empty()
>>> callable(empty_call_instance)
True
>>> empty_call_instance()
```

如你所见，什么都没发生——这基本上意味着`Empty()`返回了`None`。然而，这次没有抛出错误，因为`EmptyCallable`的实例确实是可调用的。

注意：每个 Python 类都是可调用的，这意味着调用它会创建一个类的新实例。然而，并非所有 Python 类的实例都是可调用的。要使实例可调用，你必须在类体中实现`__call__()`方法。我们通常只将类称为可调用类，当其实例是可调用的，即使从理论上讲这并不完全正确。

# 可调用对象的示例

有时候，展示某样东西的最佳方式是……去做它。因此，我将提供一系列可调用对象的示例，然后我们将讨论使对象可调用的情况。

让我们从最明显的示例开始，然后逐渐深入到不太常见的示例。

*常规和 lambda 函数*

```py
>>> def foo(): ...
>>> callable(foo)
True
>>> callable(lambda: ...)
True
```

*类和类实例*

```py
>>> class Empty:
...     def __call__(self): ...
...     def method(self): ...
...     @staticmethod
...     def static_method(): ...
...     @classmethod
...     def class_method(cls): ...
>>> callable(Empty)
True
>>> callable(Empty.class_method)
True
>>> instance = Empty()
>>> callable(instance)
True
>>> callable(instance.method)
True
>>> callable(instance.static_method)
True
```

*来自 operator 模块的函数*

```py
>>> from operator import mul, itemgetter, or_
>>> callable(mul), callable(itemgetter), callable(or_)
(True, True, True)
```

*部分对象*

```py
>>> from functools import partial
>>> def foo(x, y, z): return f"{x = }, {y = }, {z = }"
>>> foo(1, 2, 3)
'x = 1, y = 2, z = 3'
>>> fooxy5 = partial(foo, z=5)
>>> fooxy5(1, 2)
'x = 1, y = 2, z = 5'
>>> callable(fooxy5)
True
```

*装饰器*

```py
>>> def decorator(func):
...     def inner():
...         print("I'm a callable and I return one!")
...         return func
...     return inner
>>> callable(decorator)
True
>>> callable(decorator(foo))
True
```

*闭包*

```py
>>> callable(lambda x: lambda y:x*y)
True
```

让我们在这里稍作停顿。这一行代码需要一些解释。

我绝不会推荐编写这段代码。唯一的好处是它展示了你对 Python 复杂性的理解。然而，它绝不应该在生产代码中编写。

让我们看看它是如何工作的：

```py
>>> mult = lambda x: lambda y:x*y
>>> mult_by_5 = mult(5)
>>> mult_by_5(2)
10
```

你看到发生了什么吗？`mult`函数是闭包的一个示例：一个返回函数的函数，内层函数可以访问外层函数作用域中的自由变量，即使外层函数`mult()`已经返回。

如果你对`operator`模块比较熟悉，你会知道它的一些函数是闭包。示例包括`mul`、`add`、`itemgetter`和`methodcaller`。了解闭包及其工作原理是很有益的，因为理解它们可以将你的 Python 技能提升到更深层次。

闭包在 Python 中非常有用，并以多种不同方式使用，但这不是本文的主题。我们将另择时间讨论它们。现在对我们来说重要的是*闭包是可调用的，并返回可调用的*，这与上面的`decorator`装饰器观察到的情况相同。

让我们重写闭包，使其更具可读性；这样更容易看到发生了什么，以及为什么闭包既是又返回可调用对象：

```py
>>> def defmult(x):
...     def inner(y):
...         return x * y
...     return inner
>>> defmult_by_5 = mult(5)
>>> defmult_by_5(2)
10
```

绝对更干净。如果你注意到装饰器只是闭包的特定示例，那你是对的。无论如何：

```py
>>> callable(defmult)
True
>>> callable(defmult_by_5)
True
```

所以，*闭包是*并且*闭包返回*可调用对象。

短暂地，我想回到`functools.partial`。在 Python 中讨论可调用对象时，不能忽视它们——因为部分对象构成了一个极其有用的工具。

这是另一个值得专门讨论的主题，所以我只会展示一些简单的这些强大可调用对象的例子。[官方 Python 文档](https://docs.python.org/3/library/functools.html#functools.partial) 解释了部分对象如下：

> `partial()`用于部分函数应用，它“冻结”函数的某些参数和/或关键字，结果是一个具有简化签名的新对象。

因此，你可以实现类似于闭包的功能。还记得吗？

```py
>>> mult = lambda x: lambda y: x*y 
>>> mult_by_5 = mult(5)
>>> mult_by_5(2)
10
```

我们可以通过以下方式实现相同的功能：

```py
>>> from functools import partial
>>> def mult(x, y): return x*y
>>> partialmult_by_5 = partial(mult, y=5)
>>> partialmult_by_5(2)
10
```

你可以创建一个新的可调用对象，使某些（甚至全部，如果这是你需要的）参数被赋予特定的值。你也可以用它来改变函数参数的默认值：

```py
>>> def multiply_str(s: str, n: int = 2) -> str:
...     return s*n
>>> multiply_str("abc")
'abcabc'
>>> multiply_str_5 = partial(multiply_str, n=5)
>>> multiply_str_5("abc")
'abcabcabcabcabc'
```

从技术上讲，部分对象不是函数：

```py
>>> type(multiply_str)
<class 'function'>
>>> type(multiply_str_5)
<class 'functools.partial'>
>>> callable(multiply_str_5)
True
```

它们是可调用的，而且它们是[*部分对象*](https://docs.python.org/3/library/functools.html#functools.partial)。还值得一提的是`functools.partialmethod()`，它创建的部分对象可用作类方法，与`functools.partial()`不同，后者的对象用作函数。如果你感兴趣，我希望在不久的将来发布专门的文章；目前，你可以阅读[官方文档](https://docs.python.org/3/library/functools.html#functools.partialmethod)。

# 什么时候需要使对象可调用？

如上所示，不仅 Python 充满了可调用对象，我们还可以轻松创建它们。以下文章展示了这一点：

[](/a-callable-float-fun-and-creativity-in-python-7a311ccd742d?source=post_page-----ba88bf0729aa--------------------------------) ## 可调用的浮点数？ Python 中的乐趣与创造力

### 为了学习如何进行创造性编程，我们将在 Python 中实现可调用的浮点数。

[towardsdatascience.com

我在这里展示了如何实现一个可调用类`Float`，它继承自`float`。这个类的实例是可调用的，所以我们在讨论可调用的浮点数。

为什么要这样做？你能通过这种方式实现什么？考虑这个从上面文章中提取的例子，如果你对`Float`类的实现感兴趣，你可以在那里找到它：

```py
>>> i = Float(12.105)
>>> 2*i
24.21
>>> i(round)
12
>>> i(lambda x: 200)
200
>>> i(lambda x: x + 1)
13.105
>>> def square_root_of(x):
...     return x**.5
>>> i(square_root_of)
3.479224051422961
>>> i(lambda x: round(square_root_of(x), 5))
3.47922
>>> i = Float(12345.12345)
>>> i(lambda x: Float(str(i)[::-1]))
54321.54321
```

所以，你可以调用一个`Float`数字，并将一个函数作为参数提供，函数将应用于实例所保持的数字。相关的文章并没有指出这样一个可调用类是有用的；我也不打算在这里提出这个观点。不试图证明这样的类有意义，文章讨论了 Python 中的创造力，并展示了 Python 编码的乐趣。

那我为什么提到这一点？因为这篇文章确实展示了其他内容。它表明你可以轻松创建可调用对象。关键是要知道不仅仅如何做，而且——如果不是主要的话——何时做。

当你可能需要创建一个可调用对象时，下面的例子提供了一些。我将省略最明显的，比如需要创建一个函数或类。

+   类函数对象。例如，你可能想要创建一个函数类对象，它接受参数并返回一个值，但也有附加的状态或行为。闭包、装饰器和上下文管理器是很好的例子。

+   使用可调用对象的设计模式。[策略模式](https://en.wikipedia.org/wiki/Strategy_pattern)就是一个完美的例子；它允许你定义一系列算法，封装每一个算法，并使它们可互换。

+   动态函数和动态可调用对象。这意味着在运行时创建一个函数或可调用对象。可调用对象允许你轻松做到这一点。

这相当技术性和理论性，所以我们来分析一个实际的例子。假设你有一个`ClassifyTextTo`类，旨在将文本分类到多个类别中。我们忽略实现细节，专注于类的设计。我们可以写出这个类的以下原型：

```py
class ClassifyTextTo:
    def __init__(self, config, path):
        self.config = config
        self.path = path
    def read_text(self):
        ...
    def preprocess_text(self):
        ...
    def classify(self):
        ...
    def diagnose(self):
        ...
    def report(self):
        ...
    def pipeline(self):
        self.read_text()
        self.preprocess_text()
        self.classify()
        self.report()
```

`.pipeline()`方法解释了整个过程：

+   文本从`self.path`中读取

+   文本被预处理，以使其准备好用于分类模型

+   分类模型被运行；它在`self.config`中配置

+   模型被诊断

+   创建报告并记录

该类适用于特定的文本——或者更具体地说——适用于位于`path`中的特定文件。因此，对于每个文件，你创建一个类的实例并运行管道，就像这里：

```py
>>> classify1 = ClassifyTextTo("texts/text1.txt", configuration)
>>> classify1.pipeline()
>>> classify2 = ClassifyTextTo("texts/text2.txt", configuration)
>>> classify2.pipeline()
```

如果你有更多类似的文本，可以循环处理：

```py
>>> for text in texts:
...     classify = ClassifyTextTo(text, configuration)
...     classify.pipeline()
```

或者，更简单地说：

```py
>>> for text in texts:
...     ClassifyTextTo(text, configuration).pipeline()
```

注意，我们正在为每个文本创建`ClassifyTextTo`的实例。我们需要这样做吗？

当你在一个类中运行管道时，通常创建一个可调用类是一件自然的事情。考虑这个替代原型：

```py
class CallClassifyTextTo:
    def __init__(self, config):
        self.config = config
    def read_text(self, path):
        ...
    def preprocess_text(self):
        ...
    def classify(self):
        ...
    def diagnose(self):
        ...
    def report(self):
        ...
    def __call__(self, path):
        self.read_text(path)
        self.preprocess_text()
        self.classify()
        self.report()
```

尽管实现看起来没什么不同，但差异是显著的，体现在设计上。虽然`ClassifyTextTo`需要为每个路径创建一个实例，`CallClassifyTextTo`则不需要。如果所有文本的配置都相同，我们只需使用一个实例。事实上，即使配置需要从路径到路径变化，我们也可以使用一个实例，但那样我们将失去设计的重大优势——为每个路径使用相同的实例：

```py
>>> classify = CallClassifyTextTo(configuration)
>>> classify("texts/text1.txt")
>>> classify("texts/text2.txt") 
```

或者：

```py
>>> classify = ClassifyTextTo(configuration)
>>> for text in texts:
...     classify(text)
```

这是一件自然的事情，因为现在我们有了`classify`对象，它是可调用的，得益于`ClassifyTextTo`类中的`.__call__()`方法。这比第一种方法便宜一点，因为它只创建一个类实例。

对我来说，最重要的是对每条路径调用`classify()`是一件自然的事，因为这意味着对每条路径运行整个管道。我喜欢这个设计的简单性：

+   `CallClassifyTextTo`类的实例代表一个特定的模型，而不是路径。

+   运行一个模型是一种操作，而调用实例确实代表了这种操作，就像调用一个函数一样。

我并不是说这是唯一正确的方法。在编程中，通常有几种方法是正确的。在这种情况下，我使用几个标准来决定使用哪一种：

+   代码的可读性和简洁性

+   代码设计如何反映实际对象和操作

+   性能

+   团队和我个人的偏好

# 结论

我们讨论了 Python 编程中可调用对象的基础和复杂性。我会说，如果你想成为一名高级 Python 开发人员，你需要了解这两方面。幸运的是，它们不像最初看起来那么困难。

另一方面，我只是讨论了如何理解 Python 中的可调用对象。这与理解它们可以使用的每个场景不同。例如，闭包是一个相当复杂的话题，理解它们与理解如何创建具有可调用实例的类完全不同。通常，你使用的唯一闭包是装饰器，但有一天你可能需要在其他场景中使用它们。一个例子是[the](https://github.com/nyggus/rounder) `[rounder](https://github.com/nyggus/rounder)` [package](https://github.com/nyggus/rounder)和`_do()`函数：

[](https://github.com/nyggus/rounder/blob/main/rounder/rounder.py?source=post_page-----ba88bf0729aa--------------------------------) [## rounder/rounder/rounder.py at main · nyggus/rounder

### 用于在复杂 Python 对象中舍入浮点数和复杂数字的 Python 包。 - rounder/rounder/rounder.py at main…

github.com](https://github.com/nyggus/rounder/blob/main/rounder/rounder.py?source=post_page-----ba88bf0729aa--------------------------------)

尽管如此，我希望这篇文章能帮助你理解 Python 可调用对象的基础知识，并了解它们的一些细节。从现在起，在你的 Python 工作中，记住 Python 可调用对象可能非常强大，并考虑是否使用它们可以改善项目的代码设计。

# 脚注

¹ `Empty`类什么都不做。然而，这并不意味着它没有任何实际用途。例如，它可以作为一个哨兵，即一个用于指示特定状态或条件的对象。`None`是最著名的哨兵例子。

感谢阅读。如果你喜欢这篇文章，你可能也会喜欢我写的其他文章，你可以在[这里](https://medium.com/@nyggus)查看。如果你想加入 Medium，请使用下面的推荐链接：

[](https://medium.com/@nyggus/membership?source=post_page-----ba88bf0729aa--------------------------------) [## 使用我的推荐链接加入 Medium - Marcin Kozak

### 作为 Medium 会员，您的一部分会员费用会分配给您阅读的作者，并且您可以完全访问每一个故事……

[medium.com](https://medium.com/@nyggus/membership?source=post_page-----ba88bf0729aa--------------------------------)
