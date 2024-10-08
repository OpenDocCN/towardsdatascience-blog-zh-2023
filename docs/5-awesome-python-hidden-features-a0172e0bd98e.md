# 5 个令人惊叹的 Python 隐藏功能 — 第一部分

> 原文：[`towardsdatascience.com/5-awesome-python-hidden-features-a0172e0bd98e`](https://towardsdatascience.com/5-awesome-python-hidden-features-a0172e0bd98e)

## PYTHON | 编程 | 特性

## 使用这些酷炫的隐藏 Python 功能，将你的编程技能提升到一个新水平

[](https://david-farrugia.medium.com/?source=post_page-----a0172e0bd98e--------------------------------)![David Farrugia](https://david-farrugia.medium.com/?source=post_page-----a0172e0bd98e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0172e0bd98e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0172e0bd98e--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----a0172e0bd98e--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0172e0bd98e--------------------------------) ·6 分钟阅读·2023 年 3 月 16 日

--

![](img/6c1f21e18c4752f82bb8317b21a6b31d.png)

图片由 [Emile Perron](https://unsplash.com/@emilep?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python 是一种奇妙的编程语言。

Stack Overflow 的 2022 年开发者调查将 Python 排在 2022 年最受欢迎编程语言的第一位。

Python 非常适合初学者。它的语法简单易懂，大大减少了学习曲线的难度。

Python 是多才多艺的。得益于庞大而活跃的 Python 社区，Python 拥有强大的包和框架，能够解决几乎所有的开发需求。

> **想要编写 API 吗？** 你可以使用 Python。
> 
> **想要制作游戏吗？** Python 能够满足你的需求。
> 
> **想要处理数据并训练机器学习模型吗？** 当然可以。Python 拥有适合你的工具！

Python 还隐藏着许多绝妙的技巧。我总是对所有能优雅解决复杂任务的 Python 一行代码感到惊讶！

在这篇文章中，我们将介绍 5 个酷炫的 Python 技巧，让你可以在同事面前炫耀 😜

# 隐藏功能 1：在 FOR 和 WHILE 循环中使用 ELSE

我们在开始编程时学习的第一件事之一就是条件语句（即`if-else`块）。这些条件语句允许我们根据某些变量的值来改变代码的执行流程。在`if`块中，我们检查某些逻辑。如果这个逻辑条件没有满足，我们就执行`else`块中定义的代码。这些都是常识。

不过，我们也可以在任何`for`或`while`循环中使用`else`关键字。在这种情况下，`else`的功能是**仅在循环成功完成且没有遇到任何`break`语句**时执行代码。

你可能会问，这有什么用处？

假设我们有一个数字列表。我们想要编写逻辑来确定元组中的任何单一数字是否为偶数。

通常，我们可能会写类似这样的代码：

```py
# we define our numbers list
numbers: list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
# we also define a flag variable to indicate if an even number was found
found_even: bool = False

for num in numbers:
    # if number modulus 2 is 0, then it is even
    if num % 2 == 0:
        print(f"{num} is even")
        # we set our flag to True because we found an even number
        found_even = True
        # we can stop execution because we found an even number
        break

# if the flag is False, no even numbers where found
if not found_even:
    print("No even numbers found")
```

这个逻辑相对简单。我们使用一个标志（在这个例子中是`found_even`变量）来表示是否找到了偶数。如果在迭代过程中找到了偶数，我们使用`break`关键字来停止循环执行。

上述代码也可以写成如下形式：

```py
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
for num in numbers:
    if num % 2 == 0:
        print(f"{num} is even")
        break
else:
    print("No even numbers found")
```

我们现在不再需要标志变量`found_even`。我们可以使用`else`关键字来仅在循环迭代过程中从未达到`break`关键字时打印“*未找到偶数*”。

# 隐藏功能 2：海象运算符

海象运算符（`:=`）是在 Python 3.8 中引入的。我们使用海象运算符将变量与值分配为一个表达式。

参考以下示例。我们想要实现生成一系列随机数的逻辑，直到生成一个特定的数字。假设我们要得到我最喜欢的数字：10。我们通常会写类似这样的代码：

```py
import random

rand = None
while True:
    # generate a random number between 1 and 100
    rand = random.randint(1, 100)
    # if the random number is 10, break the execution
    if rand != 10:
        print(rand)
    else:
        break

# this will only be executed if we get a 10 and break the loop
print("We got a 10!")
```

在我们的循环中，我们生成一个随机数并将其存储在变量`rand`中。迭代的次数是基于`rand`变量的值。`rand`变为 10 的时间越早，我们就越早打破循环。

现在，使用海象运算符，我们可以通过以下代码获得相同的功能：

```py
import random

while (rand := random.randint(1, 100)) != 10:
    print(rand)

print("We got a 10!")
```

在这里，我们告诉 Python 我们希望 `while` 循环在 `rand` 的值不等于 10 时运行。我们还告诉它，每次新迭代时，`rand` 将从 `random.randint(1, 100)` 获取其值。

# 隐藏功能 3：省略号

省略号（即`...`）是一个有趣的关键字，在早期开发阶段非常有用。当处理复杂逻辑时，最佳策略是分而治之——将复杂的逻辑拆分成较小且更易于实现的部分。通常，这需要我们首先实现这些较小的函数，然后将所有内容结合在一起。

然而，我们有时（出于各种原因）想要定义函数但稍后再编写其代码。省略号允许我们做到这一点！

让我们看看如何实现。

```py
def some_function(x, y, z):
    # do something with x, y, z
    ...

# we can use it anywhere we like
some_list = [1, 2, 3, ...]
```

即使函数`some_function`没有定义代码，上述代码也不会失败。我们甚至可以调用这个函数，它仍然不会失败（当然，它也不会返回任何东西）。

# 隐藏功能 4：函数属性

在 Python 中，任何函数都被存储为一个对象。任何对象都可以拥有属性。因此，在 Python 中，函数也可以有属性。

我们可以使用函数属性来定义有关函数的附加信息和其他元数据。例如，假设我们想跟踪某个特定函数被调用的次数。我们可以设置一个计数器属性，在每次调用后递增它。

```py
def my_function(x):
    return x * 2

my_function.counter = 0
my_function.counter += 1
print(my_function.counter)
```

函数属性的另一个有趣用例是设置 `is_verbose` 属性，以便在打印额外信息时进行切换。这通常通过向函数传递一个额外的参数来实现。使用函数属性，我们将不再需要额外的参数。

另一个好的例子是展示函数的文档字符串。

```py
def my_function(x):
    """This is a docstring for my_function."""
    return x * 2

print(my_function.__name__)
print(my_function.__doc__)
```

通过调用属性 `__name__`，我们指示 Python 打印函数的名称。`__doc__` 则打印函数的文档字符串。

函数属性有许多用途。你可以在这里阅读更多关于它们的内容：

[## PEP 232 - 函数属性

### 这个 PEP 描述了对 Python 的扩展，为函数和方法添加属性字典。这个 PEP 跟踪了……

[peps.python.org](https://peps.python.org/pep-0232/?source=post_page-----a0172e0bd98e--------------------------------)

# 隐藏特性 5：三元运算符

Python 中的三元运算符是一种将 `if-else` 语句定义为单行代码的方法。

请考虑下面的示例：

```py
x = 5
y = 10

if x > y:
    result: str = "x is greater than y"
else:
    result: str = "y is greater than or equal to x"

print(result)
```

我们可以使用三元运算符语法来获得相同的功能，如下所示：

```py
x = 5
y = 10

result = "x is greater than y" if x > y else "y is greater than or equal to x"

print(result)
```

**你可以在这里找到本系列的第二部分：**

## 5 个更多令人惊叹的 Python 隐藏特性 — 第二部分

### 探索一些强大的功能以释放 Python 的全部潜力

towardsdatascience.com](/5-more-awesome-python-hidden-features-part-2-160a533c212b?source=post_page-----a0172e0bd98e--------------------------------)

# 结论

在这篇文章中，我们讨论了 5 个不被认为是常识的 Python 特性。这些隐藏特性的目的不仅仅是让我们炫耀 Python 技能。它们确实可以节省宝贵的开发时间，提高代码的可读性，并帮助我们编写更高效、更美观的代码。

**你喜欢这篇文章吗？每月 5 美元，你可以成为会员，解锁对 Medium 的无限访问权限。你将直接支持我和 Medium 上所有你喜欢的作家。非常感谢！**

[## 使用我的推荐链接加入 Medium - David Farrugia](https://david-farrugia.medium.com/membership?source=post_page-----a0172e0bd98e--------------------------------)

### 阅读 David Farrugia 的每一篇故事（以及 Medium 上成千上万的其他作家）。你的会员费用直接支持……

[david-farrugia.medium.com](https://david-farrugia.medium.com/membership?source=post_page-----a0172e0bd98e--------------------------------)

**也许你还可以考虑订阅我的邮件列表，以便在我发布新内容时收到通知。这个服务是免费的 :)**

[## 获取 David Farrugia 发布内容的邮件通知](https://david-farrugia.medium.com/subscribe?source=post_page-----a0172e0bd98e--------------------------------)

### 每当 David Farrugia 发布新内容时，你会收到一封邮件。通过注册，你将创建一个 Medium 帐户（如果你还没有的话）……

[david-farrugia.medium.com](https://david-farrugia.medium.com/subscribe?source=post_page-----a0172e0bd98e--------------------------------)

# 想要联系我吗？

我很想听听你对这个话题的看法，或者关于人工智能和数据的任何意见。

如果你希望联系我，可以发邮件到***davidfarrugia53@gmail.com***。

[Linkedin](https://www.linkedin.com/in/david-farrugia/)
