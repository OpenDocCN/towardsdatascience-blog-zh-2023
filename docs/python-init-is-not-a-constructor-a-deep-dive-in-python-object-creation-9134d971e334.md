# Python：__init__ 不是构造函数：深入探讨 Python 对象创建

> 原文：[`towardsdatascience.com/python-init-is-not-a-constructor-a-deep-dive-in-python-object-creation-9134d971e334`](https://towardsdatascience.com/python-init-is-not-a-constructor-a-deep-dive-in-python-object-creation-9134d971e334)

## 使用 Python 的构造函数创建快速且内存高效的类

[](https://mikehuls.medium.com/?source=post_page-----9134d971e334--------------------------------)![Mike Huls](https://mikehuls.medium.com/?source=post_page-----9134d971e334--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9134d971e334--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9134d971e334--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----9134d971e334--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9134d971e334--------------------------------) ·阅读时间 9 分钟·2023 年 11 月 27 日

--

![](img/50dee26173534a7a06708762a2ea94ac.png)

Python 如何构建对象（图像由 ChatGPT 提供）

你知道`__init__`方法**不是构造函数**吗？但如果`__init__`不*创建*对象，那究竟是什么呢？对象在 Python 中是如何创建的？Python 甚至有构造函数吗？

本文的目标是**更好地理解 Python 如何创建对象**并**操控这一过程以构建更好的应用程序**。

首先，我们将深入了解 Python 如何创建对象。接下来，我们将应用这些知识，讨论一些有趣的用例，并提供一些实际示例。让我们开始编码吧！

# 1\. 理论：在 Python 中创建对象

在这一部分，我们将弄清楚在你创建对象时 Python 背后发生了什么。在下一部分，我们将运用这些新知识进行第二部分的实践。

## 如何在 Python 中创建对象？

这应该很简单；你只需创建一个类的实例。或者，你可以创建一个新的内置类型，比如`str`或`int`。在下面的代码中，创建了一个基本类的实例。它只包含一个`__init__`函数和一个`say_hello`方法：

```py
class SimpleObject:
  greet_name:str

  def __init__(self, name:str):
    self.greet_name = name

  def say_hello(self) -> None:
    print(f"Hello {self.greet_name}!")

my_instance = SimpleObject(name="bob")
my_instance.say_hello()
```

注意`__init__`方法。它接收一个`name`参数，并将其值存储在`SimpleObject`实例的`greet_name`属性上。这允许我们的实例保持*状态*。

现在问题出现了：为了**保存状态**，我们需要**有东西来保存状态**。`__init__` 从哪里得到对象？

## 那么，__init__ 是构造函数吗？

答案是：从技术上讲，没有。构造函数实际上*创建*新对象；`__init__`方法仅负责设置对象的*状态*。它只是通过参数接收值，并将这些值分配给像`greet_name`这样的类属性。

在 Python 中，对象的实际*创建*发生在初始化之前。对于**对象创建，Python 使用一个名为**`**__new__**`**的方法，该方法存在于每个对象上。

[](/creating-and-publishing-your-own-python-package-for-absolute-beginners-7656c893f387?source=post_page-----9134d971e334--------------------------------) ## 为绝对初学者创建和发布自己的 Python 包

### 在 5 分钟内创建、构建和发布一个 Python 包

towardsdatascience.com

## `__new__` 做了什么？

`__new__` 是一个**类方法**，意味着它是直接在类上调用的，而不是在类的实例上。它**存在于每个对象上**，并**负责实际创建和返回**对象。`__new__` 的最重要的方面是它必须返回一个类的实例。我们将在本文后面进一步研究这个方法。

## `__new__` 方法来自哪里？

简短的回答是：**Python 中的一切都是对象**，**object 类有一个** `**__new__**` **方法**。你可以把这看作是*“每个类都继承自* `*object*` *类”*。

请注意，即使我们的 `SimpleObject` 类没有继承任何东西，我们仍然可以证明它是 `object` 的一个实例：

```py
# SimpleObject is of type 'object'
my_instance = SimpleObject(name="bob")
print(isinstance(my_instance, object))    # <-- True
# but all other types as well:
print(isinstance(42, object))             # <-- True
print(isinstance('hello world', object))  # <-- True
print(isinstance({"my": "dict"}, object)) # <-- True
```

总结来说，一切都是对象，`object` 定义了 `__new__` 方法，因此 Python 中的一切都有一个 `__new__` 方法。

## `__new__` 和 `__init__` 有何不同？

`__new__` 方法用于实际**创建对象**：分配内存并返回新对象。一旦对象创建完成，我们可以用 `__init__` 来**初始化**它；设置初始的*状态*。

[](/python-args-kwargs-and-all-other-ways-to-pass-arguments-to-your-function-bd2acdce72b5?source=post_page-----9134d971e334--------------------------------) ## Python 的 args、kwargs 和传递参数的所有其他方式

### 精巧地设计你的函数参数的 6 个示例

towardsdatascience.com

## Python 对象创建的过程是什么样的？

内部，下面的函数在你创建新对象时会被执行：

+   `__new__`：分配内存并返回新对象

+   `__init__`：初始化新创建的对象；设置状态

在下面的代码中，我们通过**重写**`**__new__**`**来展示这一点**。在下一部分我们将利用这一原则做一些有趣的事情：

```py
class SimpleObject:
  greet_name:str

  def __new__(cls, *args, **kwargs):      # <-- newly added function
    print("__new__ method")               
    return super().__new__(cls)            

  def __init__(self, name:str):
    print("__init__ method")
    self.greet_name = name

  def say_hello(self) -> None:
    print(f"Hello {self.greet_name}!")

my_instance = SimpleObject(name="bob")
my_instance.say_hello()
```

（*我们将在接下来的部分解释为什么和如何工作*。）这将打印以下内容：

```py
__new__ method
__init__ method
Hello bob!
```

这意味着我们可以访问初始化我们类的实例的函数！我们还看到 `__new__` 先执行。在下一部分我们将了解 `__new__` 的行为：`super().__new__(cls)` 是什么意思？

## `__new__` 是如何工作的？

`__new__`的默认行为如下所示。在这一部分，我们将尝试理解发生了什么，以便在下一部分的实际示例中对其进行调整。

```py
class SimpleObject:
  def __new__(cls, *args, **kwargs):
    return super().__new__(cls)
```

请注意，`__new__`是在`super()`方法上调用的，它返回一个“引用”*(实际上是一个代理对象)*到`SimpleObject`的父类。请记住，`SimpleObject`继承自`object`，其中定义了`__new__`方法。

**分解：**

1.  我们获得了我们所在类的基类的“引用”。以`SimpleObject`为例，我们获得了`object`的“引用”

1.  我们在“引用”上调用`__new__`，因此`object.__new__`

1.  我们将`cls`作为参数传递。

    *这就是像* `*__new__*` *这样的类方法的工作方式；它是对类本身的引用*

**综合起来**：我们请求`SimpleObject`的父类创建一个`SimpleObject`的新实例。

这与`my = object.__new__(SimpleObject)`是一样的

## 那么我可以使用`__new__`创建一个新实例吗？

是的，请记住，默认的`__new__`实现实际上直接调用它：`return super().**__new__**(cls)`。因此，下面代码中的方法做了同样的事情：

```py
# 1\. __new__ and __init__ are called internally
my_instance = SimpleObject(name='bob')

# 2\. __new__ and __init__ are called directly:
my_instance = SimpleObject.__new__(SimpleObject)
my_instance.__init__(name='bob')
my_instance.say_hello()
```

在*直接*方法中发生的事情：

1.  我们在`SimpleObject`上调用`__new__`函数，传递`SimpleObject`类型。

1.  `SimpleObject.__new__` 在其父类（`object`）上调用`__new__`

1.  `object.__new__`创建并返回一个`SimpleObject`的实例

1.  `SimpleObject.__new__`返回新实例

1.  我们调用`__init__`来初始化它。

这些事情在非直接方法中也会发生，但它们是在幕后处理的，所以我们没有注意到。

[](/simple-trick-to-work-with-relative-paths-in-python-c072cdc9acb9?source=post_page-----9134d971e334--------------------------------) ## 在 Python 中处理相对路径的简单技巧

### 轻松在运行时计算文件路径

towardsdatascience.com

# 实际应用 1：子类化不可变类型

现在我们知道`__new__`是如何工作的，我们可以利用它做一些有趣的事情。我们将理论付诸实践，子类化一个不可变类型。这样，我们可以拥有自己的特殊类型，其方法定义在一个非常快速的内置类型上。

## 目标

我们有一个处理许多坐标的应用程序。因此，我们希望将坐标存储在元组中，因为它们很小且内存高效。

我们将创建自己的`Point`类，继承自`tuple`。这样，`Point`是一个`tuple`，因此它非常快速且小巧，并且我们可以添加如下功能：

+   对对象创建的控制（例如，只在所有坐标都是正数时创建新对象）

+   额外的方法，例如计算两个坐标之间的距离。

cython-for-absolute-beginners-30x-faster-code-in-two-simple-steps-bbb6c10d06ad?source=post_page-----9134d971e334-------------------------------- ## Cython 的绝对初学者指南：两步实现代码 30 倍加速

### 为闪电般快速的应用程序提供简单的 Python 代码编译

[towardsdatascience.com

## 带有 __new__ 重写的 Point 类

在第一次尝试中，我们仅创建一个继承自元组的`Point`类，并尝试使用`x, y`坐标初始化元组。**这不会成功：**

```py
class Point(tuple):

  x: float
  y: float

  def __init__(self, x:float, y:float):
    self.x = x
    self.y = y

p = Point(1,2)    # <-- tuple expects 1 argument, got 2
```

失败的原因是因为我们的类是`tuple`的子类，而`tuple`是**不可变的**。记住，`tuple`是通过`__new__`创建的，然后`__init__`运行。在初始化时，元组已经被创建，不能再被修改，因为它们是不可变的。

我们**可以通过重写`__new__`来解决这个问题：**

```py
class Point(tuple):

  x: float
  y: float

  def __new__(cls, x:float, y:float):    # <-- newly added method
    return super().__new__(cls, (x, y))

  def __init__(self, x:float, y:float):
    self.x = x
    self.y = y
```

这之所以有效，是因为在`__new__`中，我们使用`super()`来获取`Point`的父类引用，即`tuple`。接下来，我们使用`tuple.__new__`并传递一个可迭代对象（`(x, y)`）来创建一个新元组。这与`tuple((1, 2))`是一样的。

## 控制实例创建和附加方法

结果是一个`Point`类，底层是一个`tuple`，但我们可以添加各种额外功能：

```py
class Point(tuple):
    x: int
    y: int

    def __new__(cls, x:float, y:float):
      if x < 0 or y < 0:                                  # <-- filter inputs
          raise ValueError("x and y must be positive")
      return super().__new__(cls, (x, y))

    def __init__(self, x:float, y:float):
      self.x = x
      self.y = y

    def distance_from(self, other_point: Point):          # <-- new method
      return math.sqrt(
        (other_point.x - self.x) ** 2 + (other_point.y - self.y) ** 2
      )

p = Point(1, 2)
p2 = Point(3, 1)
print(p.distance_from(other_point=p2))  # <-- 2.23606797749979
```

注意我们添加了一个计算`Point`之间距离的方法，以及一些输入验证。我们现在在`__new__`中检查提供的`X`和`y`值是否为正，并在不符合条件时完全阻止对象创建。

[](/a-complete-guide-to-using-environment-variables-and-files-with-docker-and-compose-4549c21dc6af?source=post_page-----9134d971e334--------------------------------) ## 使用 Docker 和 Compose 的环境变量和文件的完整指南

### 通过这个简单的教程保持你的容器安全和灵活

[towardsdatascience.com

# 实际应用 2：添加元数据

在这个示例中，我们从不可变的`float`创建了一个子类，并添加了一些元数据。下面的类将生成一个真正的`float`，但我们添加了一些关于符号的额外信息。

```py
class Currency(float):

    def __new__(cls, value: float, symbol: str):
        obj = super(Currency, cls).__new__(cls, value)
        obj.symbol = symbol
        return obj

    def __str__(self) -> str:
        return f"{self.symbol} {self:.2f}"  # <-- returns symbol & float formatted to 2 decimals

price = Currency(12.768544, symbol='€')
print(price)                            # <-- prints: "€ 12.74"
```

正如你所见，我们继承自`float`，这使得`Currency`的实例实际上是一个`float`。如你所见，我们还可以访问诸如用于美观打印的符号等元数据。

还要注意这是一个实际的浮点数；我们可以毫无问题地执行`float`操作：

```py
print(isinstance(price, float))        # True
print(f"{price.symbol} {price * 2}")   # prints: "€ 25.48"
```

[](/args-vs-kwargs-which-is-the-fastest-way-to-call-a-function-in-python-afb2e817120?source=post_page-----9134d971e334--------------------------------) ## 参数与关键字参数：哪种方式在 Python 中调用函数最快？

### `timeit`模块的清晰演示

towardsdatascience.com

# 实际应用 3：单例模式

有些情况下你不想每次实例化类时都返回一个*新的*对象。例如，一个数据库连接。单例模式**将类的实例化限制为唯一实例**。该模式用于确保一个类只有一个实例，并提供一个**全局访问点**来访问该实例：

```py
class Singleton:
  _instance = None

  def __new__(cls):
    if cls._instance is None:
      cls._instance = super(Singleton, cls).__new__(cls)
    return cls._instance

singleton1 = Singleton()
singleton2 = Singleton()

print(id(singleton1))
print(id(singleton2))
print(singleton1 is singleton2)  # True
```

这段代码创建一个`Singleton`类的实例（如果它尚不存在），并将其作为属性保存在`cls`上。当`Singleton`再次被调用时，它返回之前存储的实例。

[](/run-code-after-your-program-exits-with-pythons-atexit-82a0069b486a?source=post_page-----9134d971e334--------------------------------) ## 使用 Python 的 AtExit 在程序退出后运行代码

### 注册在脚本结束或出错后运行的清理函数

towardsdatascience.com

# 其他实际应用

其他一些应用包括：

+   **控制实例创建**

    我们在`Point`示例中已经看到过：在创建实例之前添加额外的逻辑。这可以包括输入验证、修改或日志记录。

+   **工厂方法**

    根据输入在`__new__`中确定将返回哪个类。

+   **缓存**

    对于资源密集型对象创建。像单例模式一样，我们可以在类本身上存储之前创建的对象。我们可以在`__new__`中检查是否已经存在等效的对象，并返回它，而不是创建一个新的。

[](/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893?source=post_page-----9134d971e334--------------------------------) ## 从你的 Git 仓库创建可以用 PIP 安装的自定义私有 Python 包

### 使用你的 git 仓库分享你自己构建的 Python 包。

towardsdatascience.com

# 结论

在这篇文章中，我们深入探讨了 Python 对象创建，了解了它是如何工作的以及为什么这样工作。然后我们看了一些实际示例，演示了我们可以用新获得的知识做很多有趣的事情。控制对象创建可以使你创建高效的类，并显著提高你的代码的专业性。

为了进一步改进你的代码，我认为最重要的是真正理解你的代码，了解 Python 的工作原理并应用合适的数据结构。为此，请查看我的[**其他文章**](https://mikehuls.com/articles?tags=fast)或这[**个演示**](https://www.youtube.com/watch?v=N7cgUnW-tZQ)。

我希望这篇文章能像我期望的那样清晰，但如果不清楚，请告诉我可以进一步澄清的内容。同时，请查看我在 [其他文章](https://mikehuls.com/articles) 上关于各种编程相关主题的文章：

+   [绝对初学者的 Git：通过视频游戏理解 Git](https://mikehuls.medium.com/git-for-absolute-beginners-understanding-git-with-the-help-of-a-video-game-88826054459a)

+   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)

+   [使用 FastAPI 在 5 行代码中创建快速的自动文档、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)

祝编码愉快！

— Mike

*附言：喜欢我做的事吗？* [*关注我*](https://mikehuls.medium.com)*！*

[](https://mikehuls.medium.com/?source=post_page-----9134d971e334--------------------------------) [## Mike Huls - Medium

### 阅读 Mike Huls 在 Medium 上的文章。我是一名全栈开发者，对编程、技术充满热情，…

mikehuls.medium.com](https://mikehuls.medium.com/?source=post_page-----9134d971e334--------------------------------)
