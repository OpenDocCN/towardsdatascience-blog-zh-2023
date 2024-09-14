# Python getattr() 函数解释

> 原文：[`towardsdatascience.com/python-getattr-function-explained-pyshark-cc7f49c59b2e`](https://towardsdatascience.com/python-getattr-function-explained-pyshark-cc7f49c59b2e)

## 在本文中，我们将探讨如何使用 Python getattr() 函数。

[](https://pyshark.medium.com/?source=post_page-----cc7f49c59b2e--------------------------------)![Misha Sv](https://pyshark.medium.com/?source=post_page-----cc7f49c59b2e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cc7f49c59b2e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cc7f49c59b2e--------------------------------) [Misha Sv](https://pyshark.medium.com/?source=post_page-----cc7f49c59b2e--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cc7f49c59b2e--------------------------------) ·4 分钟阅读·2023 年 3 月 20 日

--

![](img/a5e64fb6704e20c5a2c1070c9a117bc2.png)

[Shane Aldendorff](https://unsplash.com/@pluyar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄于 [Unsplash](https://unsplash.com/photos/3AzL-IR3v7Y?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## **目录**

+   介绍

+   使用 getattr() 动态访问对象的属性

+   使用 getattr() 构建动态 API

+   使用 getattr() 动态加载模块

+   结论

# 介绍

Python **getattr()** 函数是一个内置函数，允许动态访问对象的属性。具体来说，它用于检索 Python 对象的名称属性。

Python **getattr()** 函数的语法是：

```py
getattr(object, name[, default])
```

其中：

+   *object* — 我们希望从中检索属性的 Python 对象

+   *name* — Python 对象中命名属性的名称

+   *default* — 可选参数，用于指定如果未找到指定属性时的返回值。如果未指定，代码将返回**AttributeError**。

**getattr()** 函数在调用时，会搜索指定的 Python 对象中的名称属性并返回其值。

在接下来的章节中，我们将探讨一些 **getattr()** 函数的常见使用案例。

# 使用 getattr() 动态访问对象的属性

Python **getattr()** 函数最流行的使用案例之一是动态访问对象的属性。

让我们开始创建一个新的 Python 对象 **Car**，它有三个属性（*make*、*model*、*price*）：

```py
class Car:

    def __init__(self, make, model, price):

        self.make = make
        self.model = model
        self.price = price
```

接下来，我们将创建一个带有一些示例值的该类实例：

```py
car = Car('Audi', 'Q7', 100000)
```

现在我们可以使用 **getattr()** 函数动态访问这个类的属性。

例如，假设我们想要检索刚刚创建的 **car** 对象的 **price** 属性：

```py
attr_name = 'price'

attr_value = getattr(car, attr_name)

print(attr_value)
```

你应该得到：

```py
100000
```

如果你尝试检索对象没有的属性，你会看到 **AttributeError**。

例如，这个对象没有属性**colour**，所以让我们看看当我们尝试检索它时会发生什么：

```py
attr_name = 'colour'

attr_value = getattr(car, attr_name)

print(attr_value)
```

你应该得到：

```py
AttributeError: 'Car' object has no attribute 'colour'
```

如果你正在处理多个类，而不知道它们是否具有你正在寻找的属性，这种方法非常有用，它可以节省大量时间和代码量，快速运行这些测试以检索属性值。

# 使用 getattr() 构建动态 API

Python **getattr()** 函数的另一个用例是构建 Python 中的动态 API。

让我们开始创建一个简单的 **Calculator** 类，包含几个执行数学计算的方法：

```py
class Calculator:

    def add(self, x, y):
        return x + y

    def subtract(self, x, y):
        return x - y
```

现在我们可以围绕这个 **Calculator** 类构建一个 API，它将允许动态调用任何方法（使用 Python **getattr()** 函数）：

```py
class CalculatorAPI:

    def __init__(self, calculator):

        self.calculator = calculator

    def call_method(self, method_name, *args):

        method = getattr(self.calculator, method_name, None)

        if method:
            return method(*args)
        else:
            return f"Method '{method_name}' not found"
```

一旦 API 构建完成，我们可以用不同的计算，如加法和减法来测试它，并检查结果：

```py
calculator = Calculator()

api = CalculatorAPI(calculator)

print(api.call_method("add", 7, 8))
print(api.call_method("subtract", 9, 1))
```

你应该得到：

```py
15
8
```

在这个例子中，我们使用 Python **getattr()** 函数动态访问 Python 类的所需方法。

# 使用 getattr() 动态加载模块

Python **getattr()** 函数的另一个用例是在运行时动态加载模块。

在这个例子中，我们将使用一个内置的 Python 模块，这实际上是 **import** 语句的实现。具体来说，我们将使用 **import_module()** 函数进行编程导入。

我们将使用 **getattr()** 函数来访问加载模块中的特定函数。

假设我们想构建一个小程序，询问用户要导入哪个模块、要访问该模块的哪个函数以及要执行什么操作。

例如，我们想导入数学模块，访问 **sqrt()** 函数并找到 25 的平方根。

我们将以编程方式加载模块和函数，并执行计算：

```py
#Import the required dependency
import importlib

#Define module name
module_name = 'math'

#Programmatically load module
module = importlib.import_module(module_name)

#Define function name
function_name = 'sqrt'

#Programmatically load function
function = getattr(module, function_name)

#Define input for the function
num = 25

#Calculate the result
result = function(num)

#Print the result
print(f"Result: {result}")
```

你应该得到：

```py
5.0
```

虽然这是一个非常简单的例子，看起来不像是 **sqrt()** 函数的有用应用，但它说明了动态加载模块和函数的一般思路。

# 结论

在这篇文章中，我们探讨了 Python **getattr()** 函数。

现在你已经了解了基本功能，你可以在项目中练习使用它，以向代码中添加更多功能。

如果你有任何问题或建议，请随时在下面留言，查看更多我的 [Python Functions](https://pyshark.com/category/python-functions/) 教程。

*最初发布于* [*https://pyshark.com*](https://pyshark.com/python-getattr-function/) *2023 年 3 月 20 日。*
